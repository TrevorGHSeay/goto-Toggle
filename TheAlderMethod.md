## The Alder Method

In the spirit of accessibility, The Alder Method uses its own (new) intermediary natural programming language as input API for to produce the effect of a broad range of programmatical assertions and performance. Please note that I created this product entirely myself!

Using The Alder Method, one can simply write basic sentences, in what's called 'Plain English', to have it perform functionality as though you programmed it at the language level. This baby boasts 3D vector coordination at a whim, spherical and linear interpolation at a glance and the ability to change necessary properties with a sentence and no more.

This not only saves users time but money, hassle and the sense of dependence one can feel from relying on a fellow professional for something they themselves consider too easy to be bothered with.

**[Marketplace Listing](https://marketplace.secondlife.com/p/The-Alder-Method/17320766)**

[Go Back](https://trevorghseay.github.io/goto-Toggle/Projects)




    string code="";

    integer codelines=0;

    list varnms=[];         //user variable names
    list vars=[];           //user variable values (cast to strings)

    list actions=[];        //actions to be performed, parsed as sublists to recall multiple actions per step
    list tasks=[];          //tasks to be performed, parsed as sublists to recall multiple tasks inside actions
    list aevents=[];        //the event that the conditional will take place in to perform actions
    list acondit=[];        //conditions to be reached to perform actions, parsed as sublists to recall multiple conditionals per step

    Whipe()
    {
        code="";
        actions=[]; aevents=[]; acondit=[]; tasks=[];
        varnms=[]; vars=[];
    }

    integer lines=0;

    Interpret()
    {

        string spr_code=code; Whipe();
        integer lines=llGetListLength(llParseString2List(spr_code,["."],[""]));
        integer interint=0; actions=[];
        integer startingeventtouse=FALSE;

        do
        {
            //llOwnerSay("Beginning Line");
            integer endline=llSubStringIndex(spr_code," ."); integer condishd=FALSE; //to action if action has conditional
            integer setvar=FALSE; //whether saving random value to var or not
            integer intp=-1; //0=event,1=action,2=conditional
            string subie="";            //current event list
            string subact="";           //current action list
            string subtask="";          //current task list, for action list
            string subcond="";          //current conditional list
            integer prevcond=-1;        //index of conditional being worked on
            string foract="";           //current action name
            string foroper="";          //how much of current operation is loaded
            string formath="";          //how much of current equation is loaded
            string fortask="";          //how much of current task is loaded
            string curline=llGetSubString(spr_code,0,endline+1);

            integer expecting=-1;       //expected next type: 0=operator,1=math symbol,2=variable1,3=variable2,4=variable3,5=action,
                                                            //6=action var1,6=action var2,7=action var3,8=action var4,9=action var5
                                                            //10=condition signal
            do
            {
                integer spc=llSubStringIndex(curline," ");
                string cwordOG=llGetSubString(curline,0,spc-1);
                string cword=llToUpper(cwordOG);

                integer is_quote=-1;
                if(cword=="\"")
                {
                    string sprcurline=llDeleteSubString(curline,0,1);
                    integer qtatnm=llSubStringIndex(sprcurline,"\"");
                    cwordOG=llGetSubString(sprcurline,0,qtatnm-2);
                    is_quote=llStringLength(cwordOG);
                    cword=llToUpper(cwordOG);
                }
                else if(spc==-1) cword=curline;

                integer ievnt=llListFindList(eventtypes,[cword]);               //is event word
                integer iact=llListFindList(actiontypes,[cword]);               //is action word
                integer icond=llListFindList(conditiontypes,[cword]);           //is conditional
                integer ioper=llListFindList(operatortypes,[cword]);            //is condition operator
                integer iopinter=llListFindList(operationtypes,[cword]);        //is math operator
                integer ifuncinter=llListFindList(functionnms,[cword]);         //is function

                if(llListFindList(syntax,[cword])==-1)                          //is not syntax
                {
                    if(ievnt>-1) //is event word
                    {
                        if(llListFindList(aevents,[cword])==-1)
                            subie+=(string)ievnt;                               //is new event type

                        subcond="";
                        subact="";
                    }
                    else if(iact>-1)                                            //is action word
                    {
                        subact+=(string)iact;                                   //declare new sub-act type event
                        foract=cword;
                        condishd=FALSE;
                        foroper="";
                        formath="";
                        expecting=6;                                            //expecting action var1 next
                    }
                    else if(ifuncinter>-1)                                      //is write function
                    {
                        if(llGetSubString(subtask,-1,-1)!="|"&&subtask!="")subtask+="<>";
                        if(formath!="")subtask+=formath;
                        else if(is_quote==-1)subtask+=cword;
                        else subtask+=cwordOG;
                        if(cword!="ME")expecting=6;
                        else expecting=-1;
                    }
                    else if(icond>-1)                                           //is conditional
                    {
                        formath="";                                             //is not equation
                        prevcond=llListFindList(conditiontypes,[cword]);
                        if(cword=="ELSE")
                        {
                            if(subcond!=""&&llGetSubString(subcond,-1,-1)!="|")subcond+="|";
                            subcond+=(string)prevcond;
                            subtask+=cword+"<>--><>";
                        }
                        condishd=TRUE;                                          //is conditional
                        expecting=2;                                            //expecting variable next
                    }       
                    else if(ioper>-1)                                           //is condition operator
                    {
                        if(is_quote==-1)foroper+="<>"+cwordOG+"<>";
                        else foroper+="<>"+cword+"<>";                          //nest conditions
                        expecting=2;                                            //expecting variable next
                    }
                    else if(iopinter>-1)                                        //is math operator
                    {
                        if(expecting!=1)subact+=(string)llListFindList(actiontypes,["DECLARE"]);
                        if(cword=="IS"||cword=="ARE")cword="=";
                        string iopinterend=llGetSubString(formath,-1,-1);
                        if(llGetSubString(formath,-1,-1)!="<>"&&iopinterend!=""&&iopinterend!="|")formath+="<>";
                        if(is_quote==-1)formath+=cword+"<>";                    //nest equation
                        else formath+=cwordOG+"<>";
                        condishd=FALSE;                                         //is not conditional
                        if(cword!="=")expecting=4;                              //expecting variable3 next
                        else expecting=3;                                       //expecting variable2 next
                        iopinterend="";
                    }
                    else                                                        // is variable name, constant or value
                    {
                        integer cvarind=llListFindList(varnms,[cword]);
                        if(condishd==TRUE)
                        {
                            if(subcond!=""&&llGetSubString(subcond,-1,-1)!="|")subcond+="|";
                            subcond+=(string)prevcond;
                        }
                        if(foroper=="" && condishd!=TRUE)                       //is not in conditional
                        {
                            if(formath=="")                                     //is not in equation
                            {
                                if(expecting>5 && expecting<10)                 //expecting action variable
                                {
                                    if(subtask!=""&&expecting==6)
                                    {
                                        if(llGetSubString(subtask,-1,-1)!="|")
                                        {
                                            if(foract=="")
                                            {
                                                string emptyforactst=llGetSubString(subtask,-1,-1);
                                                if(emptyforactst!="|"&&emptyforactst!="<>"&&emptyforactst!="")subtask+="<>";
                                                emptyforactst="";
                                            }
                                        }
                                    }
                                    integer another=FALSE;
                                    if(expecting==6)
                                    {
                                        if(foract!="ECHO"&&foract!="RESIZE"&&foract!="PROMPT"&&foract!="SILENCE")
                                        {
                                            if(is_quote>-1)subtask+=foract+"<>"+cwordOG;
                                            else subtask+=foract+"<>"+cword;
                                            another=TRUE;
                                        }
                                        else
                                        {
                                            if(is_quote>-1)subtask+=foract+"<>"+cwordOG;
                                            else subtask+=foract+"<>"+cword;
                                        }
                                    }
                                    else if(expecting==7)
                                    {
                                        if(foract=="MUMBLE")
                                        {
                                            if(is_quote>-1)subtask+="<>"+cwordOG;
                                            else subtask+="<>"+cword;
                                            another=TRUE;
                                        }
                                        else
                                        {
                                            if(is_quote>-1)subtask+="<>"+cwordOG;
                                            else subtask+="<>"+cword;
                                        }
                                    }
                                    else if(expecting==8)
                                    {
                                        if(foract!="MUMBLE")
                                        {
                                            if(is_quote>-1)subtask+="<>"+cwordOG;
                                            else subtask+="<>"+cword;
                                            another=TRUE;
                                        }
                                        else
                                        {
                                            if(is_quote>-1)subtask+="<>"+cwordOG;
                                            else subtask+="<>"+cword;
                                        }
                                    }
                                    if(another==TRUE) expecting+=another;
                                    else
                                    {
                                        expecting=-1;
                                    }
                                    another=0;
                                }
                                else if(cvarind==-1 && foract=="")                          //is undeclared variable
                                {
                                    varnms+=cword;      //declare variable name
                                    vars+="";           //index variable value placeholder
                                    formath=cword;     //declare variable to act on
                                }
                                else
                                {
                                    string subtasklast1=llGetSubString(subtask,-1,-1);
                                    if(llGetSubString(subtask,-2,-1)!="<>"&&subtasklast1!="|"&&subtasklast1!="")subtask+="<>";
                                    if(is_quote>-1)subtask+=cwordOG;
                                    else subtask+=cword;
                                    subtasklast1="";
                                }
                            } 
                            else                                                            //is in equation
                            {
                                if(subtask!=""&&llGetSubString(subtask,-1,-1)!="|") subtask+="<>";
                                if(expecting==3)                                            //is variable2 declaration 
                                {
                                    subtask+=formath;
                                    if(is_quote>-1)subtask+=cwordOG;                        //build equation with quoted string
                                    else subtask+=cword;                                    //build equation without quoted string
                                    formath="";
                                    expecting=1;                                            //expecting math symbol next
                                }
                                else if(expecting==4){ 
                                    subtask+=formath;                                  
                                    if(is_quote>-1)subtask+=cwordOG; //declare sub-task using variable3 declaration with quoted string
                                    else subtask+=cword;             //declare sub-task using variable3 declaration without quoted string
                                    expecting=-1;
                                }
                            }                                                           
                        }
                        else
                        {                                                                   //is in conditional
                            subtask+=foroper+cword;                                         //declare sub-task
                            if(llListFindList(operatortypes,[llGetSubString(foroper,2,-3)])>-1)subtask+="<>--><>";
                            formath="";foroper="";fortask="";foract="";  
                            condishd=FALSE; 
                            expecting=-1;           
                        }
                    }
                }
                else if(cword=="SO"||cword=="AND"||cword==",")
                {
                    if(subact!="") subact+="|";
                    if(subtask!="")subtask+="|";
                    integer subactlength2=llGetListLength(llParseString2List(subact,["|"],[""]));
                    integer subcondlength2=llGetListLength(llParseString2List(subcond,["|"],[""]));
                    if(subactlength2>subcondlength2)
                    {
                        if(subcond!=""&&llGetSubString(subcond,-1,-1)!="|")subcond+="|";
                        subcond+="-1";
                    }
                    formath="";foroper="";fortask="";foract="";
                    condishd=FALSE;
                    expecting=-1;
                }
                else if(cword==".")
                {
                    integer subactlength=llGetListLength(llParseString2List(subact,["|"],[""]));
                    integer subcondlength=llGetListLength(llParseString2List(subcond,["|"],[""]));
                    if(subactlength>subcondlength)
                    {
                        if(subcond!=""&&llGetSubString(subcond,-1,-1)!="|")subcond+="|";
                        subcond+="-1";
                    }
                    formath="";foroper="";fortask="";foract="";
                    condishd=FALSE;
                    expecting=-1;
                    curline="";
                    subactlength=0;subcondlength=0;
                }

                if(is_quote>-1) curline=llDeleteSubString(curline,0,is_quote+4);
                else if(cword!=".")curline=llDeleteSubString(curline,0,spc);

                ievnt=0;iact=0;icond=0;ioper=0;iopinter=0;ifuncinter=0; 
                spc=0;
                cwordOG="";cword="";
                is_quote=0;     
            } while(llStringLength(curline)>0);

            if(subie!="")aevents+=subie;
            if(subact!="")actions+=subact;
            if(subtask!="")tasks+=subtask;
            if(subcond!="")acondit+=subcond;

            spr_code=llDeleteSubString(spr_code,0,endline+1);
            ++interint;
            endline=0; condishd=0;setvar=FALSE;intp=0;
            subie="";subact="";subtask="";subcond=""; 
            foract="";foroper="";formath="";fortask="";
            curline="";

            expecting=0;
        } while(interint<lines);

        if(llGetListLength(tasks)>0)
        {
            string obj_nm=llGetObjectName();
            llSetObjectName("Your 'Plain English'");llOwnerSay("/me has been compiled.");
            llSetObjectName(obj_nm);
            integer isstartingevent=llListFindList(aevents,[(string)llListFindList(eventtypes,["POSSIBLE"])]);
            if(isstartingevent>-1) ExecuteActions(isstartingevent,[]);
        }
    }

    Move(vector mv1,vector mv2,integer mi1)
    { //vector 1, vector 2, link number
        if(mv1!=mv2){
            integer mi2=PRIM_POSITION;
            if(mi1>1) mi2=PRIM_POS_LOCAL;
            llSetLinkPrimitiveParamsFast(mi1,[mi2,mv2]);
        }
    }

    Scale(string Scf1)                                                                          //scale by float percentage of current size
    {
        integer ScPind=llSubStringIndex(Scf1,"%");
        if(ScPind>-1)Scf1=llDeleteSubString(Scf1,ScPind,ScPind);     //remove the % sign
        float Scf2=(float)Scf1*0.01;             //make it a numerator / 100, from a %
        float ScMin=llGetMinScaleFactor();
        float ScMax=llGetMaxScaleFactor();
        if(Scf2<=ScMin)Scf2=ScMin+0.01;   //if too small, make it 1% larger than the minimum
        else if(Scf2>=ScMax)Scf2=ScMax-0.01;//if too big, make it 1% smaller than the maximum
        llScaleByFactor(Scf2);  
    }
    Approach(string APk1,string APs1)
    {
        integer APiskey=llSubStringIndex(APk1,"-");
        vector APv1=(vector)APk1;
        if(APiskey>-1)APv1=llList2Vector(llGetObjectDetails(APk1,[OBJECT_POS]),0);
        vector APv2=llGetPos();
        vector APv3=APv1-APv2;
        integer APi1=LINK_ROOT;
        if(llGetNumberOfPrims()==1)APi1=0;
        if(APs1=="SLOWLY")APs1="0.03";
        else if(APs1=="SMOOTHLY")APs1="0.075";
        else if(APs1=="QUICKLY")APs1="0.15";
        while(llVecDist(APv1,APv2)>1.5)
        {
            if(APiskey>-1)APv1=llList2Vector(llGetObjectDetails(APk1,[OBJECT_POS]),0);
            APv2=llGetPos();
            APv3=APv1-APv2;
            APv2+=APv3*(float)APs1;
            llSetLinkPrimitiveParamsFast(APi1,[PRIM_POSITION,APv2]);
            llSleep(0.1);
        }
    }
    PointAt(string PAs1)
    {
        vector PApos=llGetLocalPos();

        if(llGetLinkNumber()>1)PApos=llGetRootPosition()+PApos;
        vector PAtg=(vector)PAs1; //target pos
        vector PAv1=llVecNorm(<PAtg.x,PAtg.y,PApos.z>-PApos);

        llSetLinkPrimitiveParamsFast(LINK_THIS,[PRIM_ROT_LOCAL,llRotBetween(<1.0,0.0,0.0>,PAv1)]);
    }
    ControlLinkFace(string CLFs1,string CLFs2,string CLFs3)
    {
        if(llGetNumberOfPrims()==1)CLFs1="0";
        integer CLFi1=llGetListLength(llParseString2List(CLFs3,[","],[]));
        integer CLFi2=llListFindList(colours,[CLFs3]);        
        if(CLFi1!=3&&CLFi2==-1)                                            //is not vector or colour name; must be texture
            llSetLinkPrimitiveParamsFast((integer)CLFs1,[PRIM_TEXTURE,(integer)CLFs2,CLFs3,<1.0,1.0,1.0>,<0.0,0.0,0.0>,0.0]);
        else
        {
            vector CLFv1=(vector)CLFs3;                                     //defaults as vector colour
            if(CLFi2>-1)CLFv1=(vector)llList2String(colours,CLFi2+1);       //but if is colour name
            llSetLinkPrimitiveParamsFast((integer)CLFs1,[PRIM_COLOR,(integer)CLFs2,CLFv1,1.0]); 
        }
    }
    Solidify(string SDs1)
    {
        integer SDi1=1;
        if(SDs1=="SOLID")SDi1=0;
        llSetStatus(STATUS_PHANTOM,SDi1);
    }
    list colours=["RED","<0.8001,0.0,0.0>","ORANGE","<0.8001,0.3,0.0>","YELLOW","<0.8001,0.8001,0.15>","GREEN","<0.0,0.8001,0.0>","BLUE","<0.0,0.0,0.8001>","PURPLE","<0.5,0.0,0.8001>"];
    string Colour(string CRs1)
    {
        integer CRi1=llListFindList(colours,[CRs1]);
        string CRs2=CRs1;
        if(CRi1>-1)CRs2=llList2String(colours,CRi1+1);
        return CRs2;
    }
    string Direction(string Dis1)
    {
        vector Div1=<1.0,0.0,0.0>;
        if((vector)Dis1!=ZERO_VECTOR)Div1=(vector)Dis1;
        else if(Dis1=="BACKWARD")Div1=<-1.0,0.0,0.0>;
        else if(Dis1=="LEFT")Div1=<0.0,1.0,0.0>;
        else if(Dis1=="RIGHT")Div1=<0.0,-1.0,0.0>;
        else if(Dis1=="UP")Div1=<0.0,0.0,1.0>;
        else if(Dis1=="DOWN")Div1=<0.0,0.0,-1.0>;
        return (string)Div1;
    }
    string Constants(integer Ci1,list Cl1)
    { 
        string Cs1=llList2String(Cl1,0); string Cs2=llList2String(Cl1,1); string Cs3=llList2String(Cl1,2); string Cs4=llList2String(Cl1,3);
        string CrtrnS="";
        if(Ci1<5)
        {
            if(Ci1==0)CrtrnS=(string)llGetOwner();
            else if(Ci1==1||Ci1==5)CrtrnS=Cs1;  //'them' or 'payer'
            else if(Ci1==2)CrtrnS=(string)llGetPos();
            else if(Ci1==3)CrtrnS=llList2String(llGetObjectDetails((key)Cs1,[OBJECT_POS]),0); //there
            else if(Ci1==4)CrtrnS=Cs2;               //deposit
        }
        else
        {
            if(Ci1==5)CrtrnS=Cs2;          //message to chat channel or 1st msg in mumble
            else if(Ci1==6)CrtrnS=Cs3;          //thought (2nd msg in mumble)
            else if(Ci1==7)CrtrnS=Cs1;          //measure (value from mumble)
        }
        return CrtrnS;
    }

    string Runtime(string callname,list plugs)                                          //returns result of function as string, or "" if no return
    {
        integer callind=llListFindList(functionnms,[callname]);
        string RTrtrnS="";
        string RTs1=llList2String(plugs,0); string RTs2=llList2String(plugs,1); string RTs3=llList2String(plugs,2);
        string RTs4=llList2String(plugs,3); string RTs5=llList2String(plugs,4);
        if(callind>-1)
        {
            if(callind<5)
            {
                if(callind==0)llMessageLinked(LINK_SET,(integer)RTs3,RTs1,RTs2);
                else if(callind==1)llWhisper((integer)RTs2,RTs1);
                else if(callind==2)llSay((integer)RTs2,RTs1);
                else if(callind==3)llShout((integer)RTs2,RTs1);
                else if(callind==4)llRegionSay((integer)RTs2,RTs1);
            }
            else if(callind<10)
            {
                if(callind==5)llInstantMessage((key)RTs1,RTs2);
                else if(callind==6)llOwnerSay(RTs1);
                else if(callind==7)Move((vector)RTs1,(vector)RTs2,(integer)RTs3);
                else if(callind==8)PointAt(RTs2);
                else if(callind==9)llSleep((float)RTs1);
            }
            else if(callind<15)
            {
                if(callind==10)llSetTimerEvent((float)RTs1);
                else if(callind==11)Approach(RTs1,RTs2);
                else if(callind==12)llSetText(RTs1+"\n ",(vector)Colour(RTs2),1.0);
                else if(callind==13)Scale(RTs1);
                else if(callind==14)ControlLinkFace(RTs1,RTs2,RTs3);
            }
            else if(callind<20)
            {
                if(callind==15){llListenRemove(listen_handle);listen_handle=llListen((integer)RTs1,"","","");}
                else if(callind==16)llListenRemove(listen_handle);
                else if(callind==17)Solidify(RTs1);
            }
        }
        return RTrtrnS;
    }  

    //The follow are all action types, including math and not limited to only functions

    list actiontypes=["MUMBLE","WHISPER","SAY","SHOUT","SCREAM","TELL","ECHO","MOVE","POINT","WAIT","DECLARE","PROMPT","APPROACH",
                      "PUBLICIZE","RESIZE","DISGUISE","LISTEN","SILENCE","DENSITY"];

    list functionnms=["MUMBLE","WHISPER","SAY","SHOUT","SCREAM","TELL","ECHO","MOVE",
                      "POINT","WAIT","PROMPT","APPROACH","PUBLICIZE",
                      "RESIZE","DISGUISE","LISTEN","SILENCE","DENSITY"];

    list constantnms=["OWNER","THEM","HERE","THERE","DEPOSIT","MESSAGE","THOUGHT","MEASURE"];

    list syntax=["ON","OF","AS","TO","SO","AT","IN","THE","FOR","FROM","ARE","WELL","THEN","THAT","WHEN","WITH","AND","EVERY","ABOUT","SAYING","WORTH","MEANS","SECONDS","SECOND","METER","METERS","CHANNEL","FACE","LINK","COLOR","COLOUR","MAKE","SET",".",","];

    list eventtypes=["POSSIBLE","TOUCHED","TOUCHING","UNTOUCHED","ATTACHED","BORN","PROMPTED","PAID","MESSAGED","THINKING"];
    list conditiontypes=["IF","ELSE","UNLESS"];
    list operatortypes=["<",">","=="];
    list operationtypes=["+","-","*","/","=","IS"];

    integer listen_handle;


    //pseudo event
    ExecuteActions(integer eai1,list eal1)                          //event index,cur event data
    {
        list eaActions=llParseString2List(llList2String(actions,eai1),["|"],[""]);    //cur actions 
        list eaCondits=llParseString2List(llList2String(acondit,eai1),["|"],[""]);
        list eaTasks=llParseString2List(llList2String(tasks,eai1),["|"],[""]); //cur task grouping to perform, nested
        integer eai2=0;
        integer eai3=llGetListLength(eaActions);
        do
        {                                                                         //loop to perform event actions (using conditions)
            integer ceaAction=(integer)llList2String(eaActions,eai2);
            integer ceaCondit=(integer)llList2String(eaCondits,eai2);
            string ceaTasksS=llList2String(eaTasks,eai2);
            list ceaTasks=llParseString2List(ceaTasksS,["<>"],[""]); //cur tasks to perform, nested
            integer performbool=FALSE;
            string var1=llList2String(ceaTasks,0); string var2=llList2String(ceaTasks,2); string var3="";
            if(ceaCondit>-1)
            {
                integer operatorind=llListFindList(operatortypes,[llList2String(ceaTasks,1)]);  
                list Cond2Do=[];
                integer vars1ind=llListFindList(varnms,[var1]);
                integer vars2ind=llListFindList(varnms,[var2]);
                if(vars1ind>-1) Cond2Do+=llList2String(vars,vars1ind);
                else Cond2Do+=var1;
                if(vars2ind>-1) Cond2Do+=llList2String(vars,vars2ind);
                else Cond2Do+=var2;
                performbool=TestCondition(operatorind,ceaCondit,Cond2Do);          //attempt test
                integer to_del_to=llListFindList(ceaTasks,["-->"]);
                ceaTasks=llDeleteSubList(ceaTasks,0,to_del_to);
            }
            else performbool=TRUE;
            if(performbool==TRUE)
            {
                var1=llList2String(ceaTasks,0); var2=llList2String(ceaTasks,2); var3=llList2String(ceaTasks,4); 
                integer mathind=llSubStringIndex(ceaTasksS,"<>=<>");
                integer functionind=llListFindList(functionnms,[var1]);
                integer run2ind=llListFindList(functionnms,[var2]);
                integer run3ind=llListFindList(functionnms,[var3]);
                integer var1ind=-1; integer var2ind=-1; integer var3ind=-1; 
                integer data2ind=0;
                integer data3ind=0;
                if(mathind>-1)
                {
                    list Math2Do=[];
                    var1ind=llListFindList(varnms,[var1]);
                    var2ind=llListFindList(varnms,[var2]);
                    var3ind=llListFindList(varnms,[var3]);
                    functionind=llListFindList(functionnms,[var1]);
                    run2ind=llListFindList(functionnms,[var2]);
                    run3ind=llListFindList(functionnms,[var3]);
                    data2ind=llListFindList(constantnms,[var2]);
                    data3ind=llListFindList(constantnms,[var3]);
                    string var7=llList2String(ceaTasks,8);
                    if(data2ind>-1)Math2Do+=Constants(data2ind,eal1);
                    else if(run2ind>-1)Math2Do+=Runtime(var2,[llList2String(ceaTasks,3),var3,llList2String(ceaTasks,5),llList2String(ceaTasks,6)]);
                    else if(var2ind>-1)Math2Do+=llList2String(vars,var2ind);
                    else Math2Do+=var2;
                    if(data3ind>-1)Math2Do+=Constants(data3ind,eal1);
                    else if(run3ind>-1)Math2Do+=Runtime(var3,[llList2String(ceaTasks,5),llList2String(ceaTasks,6),llList2String(ceaTasks,7),llList2String(ceaTasks,8)]);
                    else if(var3ind>-1) Math2Do+=llList2String(vars,var3ind);

                    else Math2Do+=var3;

                    string result=DoMath(llListFindList(operationtypes,[llList2String(ceaTasks,3)]),Math2Do);
                    if(var1ind==-1)
                    {
                        varnms+=var1;
                        vars+=result;
                    }
                    else vars=llListReplaceList(vars,[result],var1ind,var1ind);
                }
                else if(functionind>-1)
                {
                    list RunTimeList=[];
                    var3=var2; var2=llList2String(ceaTasks,1);
                    string var4=llList2String(ceaTasks,3); 
                    string var5=llList2String(ceaTasks,4); 
                    var2ind=llListFindList(varnms,[var2]);  
                    data2ind=llListFindList(constantnms,[var2]);
                    if(data2ind>-1)RunTimeList+=Constants(data2ind,eal1);
                    else if(var2ind>-1) RunTimeList+=llList2String(vars,var2ind);
                    else RunTimeList+=var2;
                    var3ind=llListFindList(varnms,[var3]);
                    data3ind=llListFindList(constantnms,[var3]); 
                    if(data3ind>-1)RunTimeList+=Constants(data3ind,eal1); 
                    else if(var3ind>-1) RunTimeList+=llList2String(vars,var3ind);
                    else RunTimeList+=var3;
                    integer var4ind=llListFindList(varnms,[var4]); 
                    integer data4ind=llListFindList(constantnms,[var4]);
                    if(data4ind>-1)RunTimeList+=Constants(data4ind,eal1); 
                    else if(var4ind>-1) RunTimeList+=llList2String(vars,var4ind);
                    else RunTimeList+=var4;
                    integer var5ind=llListFindList(varnms,[var5]);  
                    integer data5ind=llListFindList(constantnms,[var5]);
                    if(data5ind>-1)RunTimeList+=Constants(data5ind,eal1);
                    else if(var5ind>-1) RunTimeList+=llList2String(vars,var5ind);
                    else RunTimeList+=var5; 

                    Runtime(var1,RunTimeList);    
                }
            }
            ++eai2;
        } while(eai2<eai3);
    }
    integer last_condition;
    integer TestCondition(integer tci1,integer tci2,list tcl1)              //integer test type, if/else/unless, list subjects
    {
        string tcs1=llList2String(tcl1,0);
        string tcs2=llList2String(tcl1,1);
        integer tci3=FALSE;

        if(tci2!=1)
        {
            if(tci1==0) // <
            {
                if((float)tcs1<(float)tcs2)tci3=TRUE;
                else tci3=FALSE;
            }
            else if(tci1==1) // >
            {
                if((float)tcs1>(float)tcs2)tci3=TRUE;
                else tci3=FALSE;
            }
            else if(tci1==2) // ==
            {
                if(tcs1==tcs2)tci3=TRUE;
                else tci3=FALSE;
            }
            if(tci2==2)                            //if is 'unless' (not 'if')
            {
                if(tci3==FALSE)tci3=TRUE;
                else tci3=FALSE;
            }
        }
        else                                       //if is 'else'
        {
            if(last_condition==FALSE) tci3=TRUE;
            else tci3=FALSE;
        }
        last_condition=tci3;
        return tci3;
    }

    string DoMath(integer DMi1,list DMl1)
    {
        integer DMtype=0; //defaulted as string
        string DMs1=llList2String(DMl1,0); string DMs2=llList2String(DMl1,1);
        list DMl2=llParseString2List(DMs1,[","],[]);
        integer DMl2lg=llGetListLength(DMl2);
        vector DMv1=(vector)DMs1;
        if((string)((integer)DMs1)==DMs1)DMtype=1;
        else if((string)((float)DMs1)==DMs1)DMtype=2;
        else
        {
            if(llSubStringIndex(DMs1,"<")!=-1&&llSubStringIndex(DMs1,">")!=-1&&DMl2lg==3)
                DMtype=3;
        }
        if(DMs2!="")
        {
            if(DMi1==0)
            {
                if(DMtype==0)return DMs1+DMs2;//           +
                else if(DMtype==1)return(string)((integer)DMs1+(integer)DMs2);
                else if(DMtype==2)return(string)((float)DMs1+(float)DMs2);
                else if(DMtype==3)return(string)((vector)DMs1+(vector)DMs2);
                else return"";
            }
            else if(DMi1==1)
            {
                if(DMtype==0)
                {
                    integer DMss1=llSubStringIndex(DMs1,DMs2);
                    if(DMss1>-1)return llDeleteSubString(DMs1,DMss1,DMss1+llStringLength(DMs2)-1);
                    else return DMs1;
                }
                else if(DMtype==1)return(string)((integer)DMs1-(integer)DMs2);
                else if(DMtype==2)return(string)((float)DMs1-(float)DMs2);//      -
                else return(string)((vector)DMs1-(vector)DMs2);
            }
            else if(DMi1==2)
            { 
                if(DMtype==1)return(string)((integer)DMs1*(integer)DMs2);
                else if(DMtype==2)return(string)((float)DMs1*(float)DMs2);//      *
                else if(DMtype==3)return(string)((vector)DMs1*(float)DMs2);
                else if(DMtype==4)return(string)((rotation)DMs1*(rotation)DMs2);
                else return"";
            }
            else if(DMi1==3)
            {
                if(DMtype==1||DMtype==2)return(string)((integer)DMs1/(float)DMs2);//      /
                else if(DMtype==3)return(string)((vector)DMs1/(rotation)DMs2);
                else return"";
            }
            else return"";
        }
        else return DMs1;
    }

    key nrequest="";
    key lrequest="";

    integer req=0;
    string prev_line="";

    integer saved=FALSE;

    default
    {
        attach(key attacher)
        {
            if(attacher!=NULL_KEY&&llGetAttached()>0)
            {
                integer isattachevent=llListFindList(aevents,[(string)llListFindList(eventtypes,["ATTACHED"])]);
                if(isattachevent>-1)
                    ExecuteActions(isattachevent,[attacher]);
            }
        }
        on_rez(integer startparams)
        {
            integer isrezevent=llListFindList(aevents,[(string)llListFindList(eventtypes,["BORN"])]);
            if(isrezevent>-1)
                ExecuteActions(isrezevent,[]);
        }
        touch_start(integer total_number1)
        {
            integer istouchevent1=llListFindList(aevents,[(string)llListFindList(eventtypes,["TOUCHED"])]);
            if(istouchevent1>-1)
                ExecuteActions(istouchevent1,[llDetectedKey(0)]);
        }
        touch(integer total_number2)
        {
            integer istouchevent2=llListFindList(aevents,[(string)llListFindList(eventtypes,["TOUCHING"])]);
            if(istouchevent2>-1)
                ExecuteActions(istouchevent2,[llDetectedKey(0)]);
        }
        touch_end(integer total_number3)
        {
            integer istouchevent3=llListFindList(aevents,[(string)llListFindList(eventtypes,["UNTOUCHED"])]);
            if(istouchevent3>-1)
                ExecuteActions(istouchevent3,[llDetectedKey(0)]);
        }
        timer()
        {
            llSetTimerEvent(0.0);
            integer istimerevent=llListFindList(aevents,[(string)llListFindList(eventtypes,["PROMPTED"])]);
            if(istimerevent>-1)
                ExecuteActions(istimerevent,[]);
        }
        money(key payer,integer amount)
        {
            integer ismoneyevent=llListFindList(aevents,[(string)llListFindList(eventtypes,["PAID"])]);
            if(ismoneyevent>-1)
                ExecuteActions(ismoneyevent,[payer,(string)amount]);
        }
        listen(integer channel,string talker_name,key talker_id,string said_msg)
        {
            integer islistenevent=llListFindList(aevents,[(string)llListFindList(eventtypes,["MESSAGED"])]);
            if(islistenevent>-1)
                ExecuteActions(islistenevent,[talker_id,said_msg]);
        }
        link_message(integer sender_link,integer value,string link_msg,key pseudo_id)
        {
            integer isthinkevent=llListFindList(aevents,[(string)llListFindList(eventtypes,["THINKING"])]);
            if(isthinkevent>-1)
                ExecuteActions(isthinkevent,[(string)value,link_msg,pseudo_id]);
        }
        dataserver(key reqst,string datas)
        {
            if(reqst==lrequest)
            {
                if(datas=="SAVE")
                {
                    saved=TRUE;
                    string obj_name=llGetObjectName();
                    llSetObjectName("Your script");llOwnerSay("/me has been strapped.\nThank you for using The Alder Method.");
                    llSetObjectName(obj_name);
                    return;
                }
                else if(datas!=EOF)
                {
                    string trimmedLine=llStringTrim(datas,STRING_TRIM);
                    integer nsanl=llSubStringIndex(trimmedLine," .");
                    if(nsanl>-1)
                    {
                        string code2add="";
                        if(prev_line!="")
                        {
                            if(code2add!="")code2add+=" ";
                            code2add+=prev_line+" "+trimmedLine;
                        }
                        else code2add=trimmedLine;
                        prev_line="";
                        code+=code2add;
                    }
                    else
                    {
                        if(prev_line!="")prev_line+=" ";
                        prev_line+=trimmedLine;
                    }
                    ++req;
                    lrequest=llGetNotecardLine("Plain English",req);
                }
                else
                {
                    if(prev_line!="")code+=prev_line;
                    prev_line="";
                    req=0;
                    Interpret();
                }
            }
        }
        changed(integer change)
        {
            if(change&(CHANGED_INVENTORY|CHANGED_ALLOWED_DROP))
            {
                if(llGetInventoryType("Plain English")==INVENTORY_NOTECARD&&saved!=TRUE)
                    lrequest=llGetNotecardLine("Plain English",0);
            }        
        }
    }
