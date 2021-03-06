### PictoCam - A 2D Camera For All

The PictoCam and a web browser, when combined, provide a way for Second Life® users to render a 2D representation of a static environment.

This method can be invaluable to those who need to know who is in an area, and exactly where, at any given time.

**[Marketplace Listing](https://marketplace.secondlife.com/p/PictoCam/12001805)**

[Go Back](https://trevorghseay.github.io/goto-Toggle/Projects)

## Host Script:

    string url;

    key owner;

    string pixels;
    string spr_pixels;


    default
    {
        state_entry()
        {

            owner = llGetOwner();

            llRequestURL();

        }

        touch_start(integer touches)
        {
            if(llDetectedKey(0) == owner)
                llOwnerSay("Your PictoCam's current address is:\n" + url);
        }

        on_rez(integer param)
        {

            owner = llGetOwner();
            pixels="";
            llReleaseURL(url);
            llRequestURL();

        }

        changed(integer change)
        {

            if(change & CHANGED_REGION_START)
            {
                llReleaseURL(url);
                llRequestURL();
            }

        }

        link_message(integer link, integer value, string lmsg, key id)
        {

            if(id == "PIXEL_DATA")
            {

                spr_pixels += lmsg;

                if(value == 0)
                {
                    pixels = spr_pixels;
                    spr_pixels = "";
                }

            }

        }

        http_request(key id, string method, string body)
        {
        
            if(method != URL_REQUEST_GRANTED)
            {
            
                llSetContentType(id, CONTENT_TYPE_XHTML);
                llHTTPResponse(id, 200,  
                                        "<!DOCTYPE html PUBLIC \"-//W3C//DTD XHTML 1.0 Transitional//EN\"" + 
                                        " \"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd\">"+
                                            "<html xmlns=\"http://www.w3.org/1999/xhtml\">" +

                                                "<head>" +
                                                    "<title>Your PictoCam</title>" + 
                                                    "<script src='http://ajax.googleapis.com/ajax/libs/jquery/1.4/jquery.min.js'></script>" + 
                                                "</head>" +

                                                "<body>" + 

                                                    "<canvas id=\"pd\" width=\"256\" height=\"256\"></canvas> " +

                                                    "<script>" +

                                                        //on page load
                                                        "<![CDATA[" +
                                                        "$(document).ready(function(){" + 

                                                            "var c=document.getElementById(\"pd\");" +
                                                            "var ctx=c.getContext(\"2d\");" +
                                                            "var imgData=ctx.createImageData(25,25);" +

                                                            "var shades = [" + pixels + "];" +

                                                            //for every pixel colour in list/array
                                                            "for (var i=0;i<imgData.data.length;i+=4)" +
                                                            "{" + 

                                                                "var shade = [shades[i/4],shades[i/4],shades[i/4]];" +

                                                                "if(shade[0] == -1)" +
                                                                    "shade = [255,205,148];" +

                                                                "else if(shade[0] == -2)" +
                                                                    "shade = [135,206,250];" +

                                                                "else if(shade[0] == -3)" +
                                                                    "shade = [0,92,9];" +

                                                                //write shade
                                                                "imgData.data[i]=shade[0];" +
                                                                "imgData.data[i+1]=shade[1];" +
                                                                "imgData.data[i+2]=shade[2];" +
                                                                "imgData.data[i+3]=255;" +

                                                            "}" +

                                                            "ctx.putImageData(imgData,0,0);" + 

                                                            "ctx.drawImage(c,0,0,c.width*10,c.height*10);" +

                                                        "});" +
                                                        "//]]>" +

                                                    "</script>" + 

                                                "</body>" + 

                                            "</html>");
                id = "";
            }
            else
                url = body;
        }
    }

## Camera Script:

    //constants

    integer DEBUGGING = TRUE;

    integer CHANNEL = -7525436;

    integer CANVAS_WIDTH = 25;
    integer CANVAS_HEIGHT = 25;
    integer CANVAS_AREA;

    float VIEW_DISTANCE = 40.0;

    vector PIXEL_SIZE;


    //variables

    key owner;

    string pixels;


    //functions

    GatherPixelData()
    {

        vector pos = llGetPos();
        rotation rot = llGetLocalRot();

        vector TL = pos + <0.25, -0.25, 0.25> * rot;
        vector TR = pos + <0.25, 0.25, 0.25> * rot;
        vector BL = pos + <0.25, -0.25, -0.25> * rot;

        vector right = llVecNorm(TR - TL) * PIXEL_SIZE.x;
        vector up =  llVecNorm(TL - BL) * PIXEL_SIZE.y;

        vector rear = pos - (<-1.0,0.0,0.0> * rot * 0.5);

        pixels = "";

        integer placeholder = 0;
        while(placeholder != CANVAS_AREA)
        {

            integer pixels_right = placeholder % CANVAS_WIDTH;
            integer pixels_down = CANVAS_HEIGHT - (placeholder / CANVAS_WIDTH);

            vector pixelPos = BL + <PIXEL_SIZE.x * pixels_right, PIXEL_SIZE.y * pixels_down, 0.0> * rot;

            vector direction = llVecNorm(pixelPos - rear) * VIEW_DISTANCE;

            list ray = llCastRay(pixelPos, pixelPos + direction, [RC_REJECT_PHYSICAL, TRUE]);

            key hitID = llList2Key(ray, 0);

            if(llGetAgentSize(hitID) == ZERO_VECTOR)
            {

                vector hitPos = llList2Vector(ray, 1);

                if(ray != [0])
                {

                    if(hitID != NULL_KEY)
                    {
                        float dist = llVecDist(pixelPos, hitPos);
                        dist = 255 - (dist / VIEW_DISTANCE * 255);

                        pixels += (string)( (integer)dist ) + ",";
                    }
                    else
                    {
                        pixels += "-3,";                    
                    }
                }
                else
                {
                    pixels += "-2,";
                }

            }
            else
                pixels += "-1,";


            placeholder += 1;

        }

        pixels += "0";

    }

    SendPixels()
    {

        integer placeholder = 0;
        integer placeholder_l = llCeil((float)llStringLength(pixels)/(float)1024);

        while(placeholder < placeholder_l)
        {

            integer ptttf = placeholder*1024;

            if(placeholder < placeholder_l - 1)
                llMessageLinked(LINK_THIS,1,llGetSubString(pixels,ptttf,ptttf+1023),"PIXEL_DATA");

            else
                llMessageLinked(LINK_THIS,0,llGetSubString(pixels,ptttf,-1),"PIXEL_DATA");

            placeholder += 1;

        }


    }


    default
    {
        state_entry()
        {

            CANVAS_AREA = CANVAS_WIDTH * CANVAS_HEIGHT;
            PIXEL_SIZE = <0.5 / CANVAS_WIDTH, 0.5 / CANVAS_HEIGHT, 0.0>;
            
            llSetTimerEvent(0.01);

        }

        on_rez(integer param)
        {

            if(llGetAttached() == 0)
            {
                owner = llGetOwner();
                llSetTimerEvent(0.01);
            }

            else
            {
                llSetTimerEvent(0.0);
                llOwnerSay("You cannot attach the PictoCam. Please rez it facing an area you wish to keep an eye on.");
            }

        }

        timer()
        {

            llSetTimerEvent(0.0);

            GatherPixelData();
            SendPixels();

            llSetTimerEvent(0.01);

        }
    }
