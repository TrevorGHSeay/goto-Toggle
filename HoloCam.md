## HoloCam - A 3D Camera for All

The HoloCam and HoloScreen, when combined, provide a way for Second Life® users to render a 3D representation of a static environment. It tracks the movement of up to 10 agents within 20 meters of the HoloCam's surroundings, allowing the owner to check in on valuable locations from afar.

This method can be invaluable to those who need to know who is in an area, and exactly where, at any given time.

**[Marketplace Listing](https://marketplace.secondlife.com/p/HoloCam/11139541)**

[Go Back](https://trevorghseay.github.io/goto-Toggle/Projects)


Screen:

    key owner;

    list map3D;

    integer displayingMap3D = FALSE;

    key tetheredUUID;

    integer isTethered = FALSE;

    integer listenHandle = -1;
    integer tetherChannel = -484391615;

    integer packetCount = 0;


    DisplayMap3D()
    {

        if(displayingMap3D != TRUE || map3D == []) return;

        list SLPPF;

        integer DM_placeholder1 = 0;
        integer DM_placeholder2 = 2;

        integer DM_placeholder1_l = llGetListLength(map3D);

        integer SLPPF_count = 0;

        integer DM_barInd;
        string DM_Lpos;
        string DM_Lrot;
        string DM_isAvatar;

        while(DM_placeholder1 < DM_placeholder1_l)
        {

            list map3D_slice = llParseString2List(llList2String(map3D,DM_placeholder1),["|"],[]);

            integer DM_placeholder3 = 0;
            integer DM_placeholder3_l = llGetListLength(map3D_slice);

            while(DM_placeholder3 < DM_placeholder3_l)
            {

                DM_Lpos = llList2String(map3D_slice, DM_placeholder3);
                DM_Lrot = llList2String(map3D_slice, DM_placeholder3 + 1);
                DM_isAvatar = llList2String(map3D_slice, DM_placeholder3 + 2);

                SLPPF += [34, DM_placeholder2, PRIM_POS_LOCAL, (vector)DM_Lpos,
                          34, DM_placeholder2, PRIM_ROT_LOCAL, llRotBetween(<0.0,0.0,-1.0>,(vector)DM_Lrot)];

                if(DM_isAvatar == "1") //is avatar
                    SLPPF += [34, DM_placeholder2, PRIM_COLOR, ALL_SIDES, <.700,.030,.030>, 1.0];

                else 
                    SLPPF += [34, DM_placeholder2, PRIM_COLOR, ALL_SIDES, <1.0,1.0,1.0>, 1.0];

                SLPPF_count += 14;


                if(SLPPF_count > 140)
                {
                    llSetLinkPrimitiveParamsFast(1,SLPPF);
                    SLPPF = [];
                    SLPPF_count = 0;
                }


                DM_placeholder2 += 1;
                DM_placeholder3 += 3;
            }

            DM_placeholder1 += 1;

        }

        if(SLPPF != []) llSetLinkPrimitiveParamsFast(1,SLPPF);

        map3D = [];
        SLPPF = [];
        SLPPF_count = 0;

        while(DM_placeholder2 < 227) //hide unused points
        {

            if(SLPPF_count > 240)
            {
                llSetLinkPrimitiveParamsFast(1,SLPPF);
                SLPPF = [];
                SLPPF_count = 0;
            }

            SLPPF += [34, DM_placeholder2, PRIM_COLOR, 0, ZERO_VECTOR, 0.0];
            SLPPF_count += 6;

            DM_placeholder2 += 1;

        }

        if(SLPPF != []) llSetLinkPrimitiveParamsFast(1,SLPPF);

        SLPPF = [];

    }

    HideMap3D()
    {

        list SLPPF = [];
        integer SLPPF_count = 0;

        integer HM_placeholder1 = 2;
        while(HM_placeholder1 < 226)
        {

            if(SLPPF_count > 130)
            {
                llSetLinkPrimitiveParamsFast(1,SLPPF);
                SLPPF = [];
                SLPPF_count = 0;
            }

            SLPPF += [34, HM_placeholder1, PRIM_COLOR, ALL_SIDES, ZERO_VECTOR, 0.0,
                      34, HM_placeholder1, PRIM_POS_LOCAL, <0.0,0.0,-0.01>,
                      34, HM_placeholder1, PRIM_ROT_LOCAL, ZERO_ROTATION];

            SLPPF_count += 14;

            HM_placeholder1 += 1;

        }

        if(SLPPF != []) llSetLinkPrimitiveParamsFast(1,SLPPF);

    }

    default
    {

        state_entry()
        { }

        attach(key attacher)
        {
            if(llGetAttached() > 0) llRequestPermissions(llGetOwner(),PERMISSION_TRIGGER_ANIMATION);
        }

        on_rez(integer start_param)
        {

            owner = llGetOwner();
            listenHandle = llListen(tetherChannel,"","","");

            llTargetOmega(<0.0,0.0,1.0>,0.333,1.0);

            displayingMap3D = FALSE;

            HideMap3D();

        }

        touch_start(integer total_number)
        {

            if(llDetectedKey(0) == owner)
            {

                displayingMap3D = !displayingMap3D;

                string toOwner = "HoloScreen toggled ";

                if(displayingMap3D == TRUE) 
                    toOwner += "on.";
                else 
                    toOwner += "off.";

                llOwnerSay(toOwner);

                if(displayingMap3D == TRUE)
                {

                    if(llGetAttached() > 0 && (llGetPermissions() & PERMISSION_TRIGGER_ANIMATION)) 
                        llStartAnimation(checkWatchAnim);

                    DisplayMap3D();

                }
                else
                {

                    HideMap3D();

                    if(llGetAttached() > 0 && (llGetPermissions() & PERMISSION_TRIGGER_ANIMATION)) 
                        llStopAnimation(checkWatchAnim);

                }

            }

        }

        listen(integer channel, string talker_name, key talker_UUID, string msg)
        {
            list l_objectDetails = llGetObjectDetails(talker_UUID,[OBJECT_OWNER,OBJECT_CREATOR]);

            if(llList2Key(l_objectDetails,0) == owner)
            {

                if(msg == "Tether")
                {

                    llRegionSayTo(talker_UUID,tetherChannel,"Tether Found");
                    return;

                }

                map3D += msg;

                ++packetCount;

                if(packetCount == 15) 
                {
                    DisplayMap3D();
                    packetCount = 0;
                    map3D = [];
                }

            }

        }

    }

Camera:

    key owner;
    key tetherUUID;

    string map3D;

    float step_size;

    integer tetherChannel = -484391615;

    default
    {
        state_entry()
        {

            step_size = 22.5; //size of each rotation step for scan

        }

        touch_start(integer total_number)
        {

            if(llDetectedKey(0) == owner)
            {

                llOwnerSay("Attempting to tether..");
                llShout(tetherChannel,"Tether");

            }

        }

        on_rez(integer start_param)
        {

            owner = llGetOwner();
            llListen(tetherChannel,"","","");
            llOwnerSay("Camera setup complete. Ready to be tethered.");

            llSetTimerEvent(10.0);

        }

        timer()
        {
            llResetTime();

            map3D = "";

            vector t_curPos = llGetPos();
            vector t_curRot = <0.0,0.0,0.0>;

            vector t_sprRot = t_curRot;

            integer t_placeholder1 = 15; //15 rows of hit tests

            while(t_placeholder1 > 0)
            {

                t_sprRot.x = step_size * t_placeholder1; 

                integer t_placeholder2 = 15; //15 columns of hit tests

                while(t_placeholder2 > 0)
                {

                    t_sprRot.z = step_size * t_placeholder2;

                    list t_raycast = llCastRay(
                                                    t_curPos, 
                                                    t_curPos + <0.0,20.0,0.0> * llEuler2Rot(t_sprRot),
                                                    [RC_DETECT_PHANTOM,TRUE,RC_DATA_FLAGS,RC_GET_NORMAL]
                                               );

                    vector t_hitLocation = llList2Vector(t_raycast,1);

                    if(llVecDist(t_curPos, t_hitLocation) <= 20.0)
                    {      
                    
                        if(t_placeholder2 != 15 && map3D != "")
                            map3D += "|";

                        t_hitLocation.z += 10.0; //add 10m to detected points to account for platform in screen

                        map3D += (string)( (t_hitLocation - t_curPos) / 80.0) + "|" +  //add local positions, divide them by 80 to make them a maximum distance of 0.25m from hologram center
                                 (string)llList2Vector(t_raycast,2)           + "|";

                        if(llGetDisplayName(llList2String(t_raycast,0)) != "")
                            map3D += "1";

                        else
                            map3D += "0";
                                
                    }

                    --t_placeholder2;
                }

                llRegionSayTo(tetherUUID, tetherChannel, map3D);
                map3D = "";

                --t_placeholder1; //cheaper on memory in mono than --
            }

            llSetTimerEvent(llGetTime() + 2.0);
        }

        listen(integer channel, string talker_name, key talker_UUID, string msg)
        {
            list l_objectDetails = llGetObjectDetails(talker_UUID,[OBJECT_OWNER,OBJECT_CREATOR]);

            if(llList2Key(l_objectDetails,0) == owner)
            {

                tetherUUID = talker_UUID;
                llOwnerSay("Tether Successful.");

            }

        }

    }
