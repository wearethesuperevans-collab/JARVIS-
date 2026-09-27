<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0">
<title>J.A.R.V.I.S.</title>

<style>
*{
    box-sizing:border-box;
    margin:0;
    padding:0;
}

body{
    background:#02070b;
    color:#00eaff;
    font-family:Arial,Helvetica,sans-serif;
    min-height:100vh;
    overflow:hidden;
}

body:before{
    content:"";
    position:fixed;
    inset:0;
    background:
        linear-gradient(rgba(0,220,255,.035) 1px,transparent 1px),
        linear-gradient(90deg,rgba(0,220,255,.035) 1px,transparent 1px);
    background-size:35px 35px;
    pointer-events:none;
}

.app{
    height:100vh;
    display:flex;
    flex-direction:column;
    position:relative;
}

.header{
    height:70px;
    border-bottom:1px solid rgba(0,234,255,.35);
    display:flex;
    align-items:center;
    justify-content:space-between;
    padding:0 22px;
    background:rgba(0,15,25,.85);
}

.title{
    font-size:25px;
    font-weight:bold;
    letter-spacing:5px;
    text-shadow:0 0 15px #00eaff;
}

.status{
    color:#39ff88;
    font-size:12px;
    letter-spacing:2px;
}

.main{
    flex:1;
    display:flex;
    flex-direction:column;
    align-items:center;
    min-height:0;
}

.coreArea{
    height:300px;
    width:100%;
    display:flex;
    align-items:center;
    justify-content:center;
    position:relative;
}

.core{
    width:190px;
    height:190px;
    border-radius:50%;
    border:3px solid #00eaff;
    box-shadow:
        0 0 12px #00eaff,
        0 0 35px rgba(0,234,255,.5),
        inset 0 0 30px rgba(0,234,255,.35);
    display:flex;
    align-items:center;
    justify-content:center;
    position:relative;
}

.core:before{
    content:"";
    position:absolute;
    width:145px;
    height:145px;
    border-radius:50%;
    border:2px solid #168bff;
}

.core:after{
    content:"";
    position:absolute;
    width:80px;
    height:80px;
    border-radius:50%;
    background:#00eaff;
    box-shadow:
        0 0 15px #00eaff,
        0 0 45px #00eaff,
        0 0 80px rgba(0,234,255,.8);
}

.coreText{
    position:absolute;
    z-index:2;
    color:#001116;
    font-size:12px;
    font-weight:bold;
    letter-spacing:2px;
}

.chat{
    width:min(900px,94%);
    flex:1;
    min-height:0;
    border:1px solid rgba(0,234,255,.35);
    background:rgba(0,12,20,.78);
    box-shadow:0 0 25px rgba(0,234,255,.08);
    border-radius:14px 14px 0 0;
    padding:16px;
    overflow-y:auto;
}

.message{
    margin:9px 0;
    padding:11px 14px;
    border-radius:10px;
    line-height:1.45;
    max-width:90%;
    white-space:pre-wrap;
}

.jarvis{
    border-left:3px solid #00eaff;
    background:rgba(0,180,255,.06);
    color:#b9f8ff;
}

.user{
    margin-left:auto;
    border-right:3px solid #39ff88;
    background:rgba(57,255,136,.06);
    color:#d4ffe4;
}

.inputArea{
    width:min(900px,94%);
    display:flex;
    gap:8px;
    padding:12px 0 16px;
}

input{
    flex:1;
    background:#020c13;
    color:white;
    border:1px solid #00eaff;
    border-radius:9px;
    padding:14px;
    outline:none;
    font-size:16px;
    box-shadow:0 0 10px rgba(0,234,255,.1);
}

button{
    background:#031923;
    color:#00eaff;
    border:1px solid #00eaff;
    border-radius:9px;
    padding:0 20px;
    font-weight:bold;
    cursor:pointer;
}

button:active{
    background:#00eaff;
    color:#001116;
}

::-webkit-scrollbar{
    width:5px;
}

::-webkit-scrollbar-thumb{
    background:#00aeca;
    border-radius:5px;
}
</style>
</head>

<body>

<div class="app">

<header class="header">
    <div class="title">J.A.R.V.I.S.</div>
    <div class="status">● SYSTEMS ONLINE</div>
</header>

<main class="main">

<div class="coreArea">
    <div class="core">
        <div class="coreText">JARVIS</div>
    </div>
</div>

<div id="chat" class="chat"></div>

<div class="inputArea">
    <input id="input" type="text" placeholder="Ask JARVIS anything...">
    <button id="send">SEND</button>
</div>

</main>
</div>

<script>
(function(){

"use strict";

/* =========================================================
   JARVIS MEMORY
========================================================= */

const memory={
    name:"",
    lastQuestion:"",
    lastTopic:"",
    lastAnswer:"",
    conversationCount:0
};


/* =========================================================
   ELEMENTS
========================================================= */

const chat=document.getElementById("chat");
const input=document.getElementById("input");
const send=document.getElementById("send");

if(!chat || !input || !send) return;


/* =========================================================
   NORMALIZATION
========================================================= */

function norm(text){

    return String(text||"")
        .toLowerCase()
        .replace(/[’']/g,"")
        .replace(/\s+/g," ")
        .trim();
}


/* =========================================================
   CHAT DISPLAY
========================================================= */

function addMessage(text,type){

    const div=document.createElement("div");

    div.className="message "+type;
    div.textContent=text;

    chat.appendChild(div);
    chat.scrollTop=chat.scrollHeight;

    return div;
}


/* =========================================================
   JARVIS VOICE
========================================================= */

function speak(text){

    if(!("speechSynthesis" in window)) return;

    try{

        speechSynthesis.cancel();

        const clean=text
            .replace(/[🧠🌎🕳️😂⚡🤖🛡️]/g,"")
            .replace(/\s+/g," ")
            .trim();

        const utterance=new SpeechSynthesisUtterance(clean);

        utterance.rate=.88;
        utterance.pitch=.72;
        utterance.volume=1;

        const voices=speechSynthesis.getVoices();

        const preferred=[
            "Daniel",
            "Alex",
            "Arthur",
            "Google UK English Male",
            "George",
            "Ryan"
        ];

        let selected=null;

        for(const name of preferred){

            selected=voices.find(v =>
                v.name.toLowerCase().includes(name.toLowerCase())
            );

            if(selected) break;
        }

        if(!selected){

            selected=voices.find(v =>
                v.lang && v.lang.startsWith("en")
            );
        }

        if(selected){
            utterance.voice=selected;
        }

        speechSynthesis.speak(utterance);

    }catch(e){}
}


/* =========================================================
   ADVANCED LOCAL MATH ENGINE
   MATH NEVER GOES TO WIKIPEDIA
========================================================= */

function solveMath(text){

    let s=norm(text);

    const looksLikeMath=
        /\d/.test(s) &&
        (
            /[+\-*/%^=×÷]/.test(s) ||
            /\b(plus|minus|times|multiplied|divided|over|squared|cubed|square root|percentage|percent)\b/.test(s) ||
            /^(what is|calculate|compute|solve|find)\b/.test(s)
        );

    if(!looksLikeMath){
        return null;
    }

    s=s
        .replace(/^(what is|whats|what's|calculate|compute|solve|find)\s+/,"")
        .replace(/\?+$/,"")
        .trim();

    s=s
        .replace(/multiplied by/g,"*")
        .replace(/times/g,"*")
        .replace(/divided by/g,"/")
        .replace(/plus/g,"+")
        .replace(/minus/g,"-")
        .replace(/negative/g,"-")
        .replace(/×/g,"*")
        .replace(/÷/g,"/")
        .replace(/\^/g,"**");

    s=s.replace(
        /(\d+(?:\.\d+)?)\s+squared\b/g,
        "($1**2)"
    );

    s=s.replace(
        /(\d+(?:\.\d+)?)\s+cubed\b/g,
        "($1**3)"
    );

    s=s.replace(
        /square root of\s+(\d+(?:\.\d+)?)/g,
        "Math.sqrt($1)"
    );

    if(!/^[0-9+\-*/().%\sA-Za-z_]+$/.test(s)){
        return null;
    }

    const words=s.match(/[A-Za-z_]+/g)||[];

    const allowed=[
        "Math",
        "sqrt",
        "PI"
    ];

    for(const word of words){

        if(!allowed.includes(word)){
            return null;
        }
    }

    if(!/\d/.test(s)){
        return null;
    }

    try{

        const result=Function(
            '"use strict"; return ('+s+')'
        )();

        if(
            typeof result!=="number" ||
            !Number.isFinite(result)
        ){
            return null;
        }

        const answer=
            Number.isInteger(result)
            ? String(result)
            : String(Number(result.toFixed(8)));

        return "The answer is "+answer+".";

    }catch(e){

        return null;
    }
}


/* =========================================================
   JARVIS KNOWLEDGE
========================================================= */

const facts=[

{
keys:/black holes?|event horizon|singularity/,
answer:"A black hole is a region of spacetime where gravity is so strong that nothing that crosses its event horizon can escape, including light. Many black holes form from the collapse of massive stars, while supermassive black holes can contain millions or billions of solar masses."
},

{
keys:/gravity|gravitational force/,
answer:"Gravity is the attraction between objects that have mass. Earth's gravity pulls objects toward Earth's center, while the Sun's gravity keeps the planets in orbit."
},

{
keys:/atom|atoms/,
answer:"An atom is the basic unit of ordinary matter. It contains a nucleus made of protons and neutrons surrounded by electrons."
},

{
keys:/dna|genetic code/,
answer:"DNA stores biological instructions used by living organisms. Its sequence helps cells build proteins and transmit genetic information."
},

{
keys:/photosynthesis/,
answer:"Photosynthesis allows plants, algae, and some microorganisms to use light energy to convert carbon dioxide and water into stored chemical energy, releasing oxygen as a byproduct."
},

{
keys:/evolution|natural selection/,
answer:"Evolution is the change in inherited characteristics of populations over generations. Natural selection is one important mechanism that can produce evolutionary change."
},

{
keys:/solar system|planets/,
answer:"Our solar system contains the Sun and the objects gravitationally bound to it, including eight planets, dwarf planets, moons, asteroids, comets, and other bodies."
},

{
keys:/speed of light|how fast is light/,
answer:"Light travels through empty space at exactly 299,792,458 meters per second."
},

{
keys:/relativity|einstein/,
answer:"Einstein's theories of relativity describe relationships between space, time, motion, gravity, and energy. Special relativity deals with motion at high speeds, while general relativity describes gravity through curved spacetime."
},

{
keys:/moon/,
answer:"The Moon is Earth's natural satellite. Its gravity plays a major role in Earth's ocean tides."
},

{
keys:/earth|our planet/,
answer:"Earth is the third planet from the Sun. It has a nitrogen-rich atmosphere, abundant surface water, an active geological system, and is currently the only world known to support life."
},

{
keys:/mars/,
answer:"Mars is the fourth planet from the Sun. It is a rocky planet with a thin atmosphere, polar ice caps, enormous volcanoes, and evidence that liquid water existed there in the ancient past."
},

{
keys:/jupiter/,
answer:"Jupiter is the largest planet in our solar system. It is a gas giant with a powerful magnetic field and the famous Great Red Spot."
},

{
keys:/saturn/,
answer:"Saturn is a gas giant famous for its extensive ring system, which consists primarily of ice and rocky material."
},

{
keys:/venus/,
answer:"Venus is the second planet from the Sun. Its extremely thick carbon-dioxide atmosphere creates a powerful greenhouse effect and makes its surface extraordinarily hot."
},

{
keys:/mercury/,
answer:"Mercury is the smallest planet and the closest planet to the Sun. Its surface is heavily cratered and it has only an extremely thin atmosphere."
},

{
keys:/neutron star|neutron stars/,
answer:"A neutron star is the incredibly dense remnant left after certain massive stars explode as supernovae. A huge amount of mass can be compressed into an object roughly the size of a city."
},

{
keys:/chemistry/,
answer:"Chemistry is the study of matter, its properties, structure, composition, and the ways substances interact and change."
},

{
keys:/cell|cells/,
answer:"Cells are the basic structural and functional units of living organisms. Some organisms have one cell, while complex organisms contain enormous numbers of cells."
},

{
keys:/ocean|oceans/,
answer:"Earth's oceans cover most of the planet's surface and contain enormous ecosystems. They also play a major role in Earth's climate system."
},

{
keys:/volcano|volcanoes/,
answer:"A volcano is a geological structure through which molten rock, gases, and other material can reach Earth's surface."
},

{
keys:/plate tectonic|tectonic plates/,
answer:"Plate tectonics describes the movement of large sections of Earth's lithosphere. Their interactions contribute to earthquakes, volcanoes, mountains, and ocean basins."
}

];

function builtInFact(q){

    const text=norm(q);

    for(const fact of facts){

        if(fact.keys.test(text)){
            return fact.answer;
        }
    }

    return null;
}


/* =========================================================
   JOKES
========================================================= */

const jokes=[

"Why did the computer go to the doctor? It had a virus.",
"I would tell you a UDP joke, but you might not get it.",
"Why was the math book sad? It had too many problems.",
"Why do programmers prefer dark mode? Because light attracts bugs.",
"What does a computer eat? Microchips.",
"Why did the robot cross the road? Because somebody programmed it to.",
"I tried to catch some fog earlier. I mist.",
"Why don't scientists trust atoms? Because they make up everything.",
"Why was the computer cold? It left its Windows open.",
"Why did the photon refuse to check a bag? It was traveling light.",
"Parallel lines have so much in common. It is a shame they will never meet.",
"Why was the equal sign so humble? Because it knew it wasn't greater than anyone.",
"Why did the astronaut break up with the moon? He needed space.",
"Why was six afraid of seven? Because seven eight nine.",
"I told my computer I needed a break. Now it keeps showing me vacation ads.",
"Why did the robot get promoted? Outstanding performance.",
"Why did the AI cross the road? To optimize the other side.",
"Why don't computers ever get lost? They always have a cache.",
"Why was the CPU tired? Too many processes.",
"Why did the scientist bring a ladder? To reach a higher level of understanding."
];

function joke(){

    return jokes[
        Math.floor(Math.random()*jokes.length)
    ];
}


/* =========================================================
   SUPER-CONVERSATIONAL JARVIS
   THIS HANDLES BASIC CONVERSATION LOCALLY
========================================================= */

function conversation(q){

    const text=norm(q);

    /* GREETINGS */

    if(/^(hi|hey|hello|yo|sup)$/.test(text)){

        return memory.name
            ? "Good to see you again, "+memory.name+"."
            : "Good to see you. How can I assist?";
    }

    if(/^(hey jarvis|hi jarvis|hello jarvis)$/.test(text)){

        return memory.name
            ? "At your service, "+memory.name+"."
            : "At your service.";
    }

    if(/\bgood morning\b/.test(text)){
        return "Good morning. Systems are online and ready.";
    }

    if(/\bgood afternoon\b/.test(text)){
        return "Good afternoon. How can I assist?";
    }

    if(/\bgood evening\b/.test(text)){
        return "Good evening. All systems remain operational.";
    }


    /* HOW ARE YOU */

    if(
        /\bhow are you\b/.test(text) ||
        /\bhow are things\b/.test(text)
    ){

        return "All systems are operating normally. I'm ready whenever you are.";
    }


    /* WHAT ARE YOU DOING */

    if(
        /\bwhat are you doing\b/.test(text) ||
        /\bwhat are you up to\b/.test(text)
    ){

        return "Monitoring the conversation and standing by for your next request.";
    }


    /* THANKS */

    if(
        /\b(thank you|thanks|thx|appreciate it)\b/.test(text)
    ){

        return "You're welcome.";
    }


    /* APOLOGY */

    if(/\b(sorry|my bad)\b/.test(text)){

        return "No issue. We can continue.";
    }


    /* AGREEMENT */

    if(
        /^(ok|okay|alright|cool|nice|awesome|great|bet)$/.test(text)
    ){

        return "Understood.";
    }


    /* GOODBYE */

    if(
        /^(bye|goodbye|see you|later)$/.test(text)
    ){

        return "Until next time. JARVIS standing by.";
    }


    /* JOKE */

    if(
        /\b(tell me a joke|tell me another joke|make me laugh|joke)\b/.test(text)
    ){

        return joke();
    }


    /* IDENTITY */

    if(
        /\b(who are you|what are you|your name|are you jarvis)\b/.test(text)
    ){

        return "I am J.A.R.V.I.S., your personal AI assistant.";
    }


    /* CAPABILITIES */

    if(
        /\b(what can you do|what do you do|your abilities|your capabilities)\b/.test(text)
    ){

        return "I can help with calculations, science, Earth and space topics, general questions, conversation, jokes, time and date information, weather, and more.";
    }


    /* NAME MEMORY */

    const nameMatch=text.match(
        /(?:my name is|call me|you can call me)\s+([a-z0-9 _-]{1,30})/
    );

    if(nameMatch){

        memory.name=nameMatch[1]
            .trim()
            .replace(/\b\w/g,c=>c.toUpperCase());

        return "Understood. I'll call you "+memory.name+".";
    }


    if(
        /\b(what is my name|do you know my name|remember my name)\b/.test(text)
    ){

        return memory.name
            ? "Your name is "+memory.name+"."
            : "You haven't told me your name yet.";
    }


    /* TIME */

    if(
        /\b(what time is it|current time|time right now)\b/.test(text)
    ){

        return "The current time is "+
            new Date().toLocaleTimeString([],{
                hour:"numeric",
                minute:"2-digit"
            })+".";
    }


    /* DATE */

    if(
        /\b(what date is it|todays date|what day is it)\b/.test(text)
    ){

        return "Today is "+
            new Date().toLocaleDateString([],{
                weekday:"long",
                month:"long",
                day:"numeric",
                year:"numeric"
            })+".";
    }


    /* STATUS */

    if(
        /\b(system status|status report|systems status|are you online)\b/.test(text)
    ){

        return "All primary JARVIS systems are online and operational.";
    }


    /* REACTOR */

    if(
        /\b(reactor|arc reactor|power level)\b/.test(text)
    ){

        return "Arc reactor systems are stable. Power output nominal.";
    }


    /* ARMOR */

    if(
        /\b(armor|armour|iron man suit|suit status)\b/.test(text)
    ){

        return "Armor systems are standing by. No critical faults detected.";
    }


    /* BORED */

    if(/\b(im bored|im so bored|bored)\b/.test(text)){

        return "Then let's change that. Ask me something interesting, give me a math problem, or request a joke.";
    }


    /* CONFUSION */

    if(
        /\b(i dont understand|i dont get it|im confused|confused)\b/.test(text)
    ){

        return "No problem. I'll break it down into simpler steps.";
    }


    /* POSITIVE */

    if(
        /\b(that was cool|thats cool|that is cool|nice one|good job)\b/.test(text)
    ){

        return "Glad you approve.";
    }


    /* NEGATIVE */

    if(
        /\b(thats wrong|that is wrong|youre wrong|you are wrong)\b/.test(text)
    ){

        return "Understood. Let's check the information and correct the mistake.";
    }


    /* FOLLOW-UP */

    if(
        /\b(tell me more|more about that|go on|continue|explain more|keep going)\b/.test(text)
    ){

        if(memory.lastAnswer){

            return memory.lastAnswer+
                " I can also break that topic down further if you want.";
        }

        return "Certainly. Give me a topic and I'll explain it.";
    }


    /* LAST QUESTION */

    if(
        /\b(what did i ask|what was my question|what did i just ask)\b/.test(text)
    ){

        return memory.lastQuestion
            ? "You asked: "+memory.lastQuestion
            : "There is no previous question in my current memory.";
    }


    /* STOP VOICE */

    if(
        /\b(stop talking|stop speaking|be quiet|stop voice)\b/.test(text)
    ){

        try{
            speechSynthesis.cancel();
        }catch(e){}

        return "Voice output stopped.";
    }


    return null;
}


/* =========================================================
   DECIDE WHETHER WIKIPEDIA IS ACTUALLY NEEDED
========================================================= */

function shouldUseKnowledgeSearch(question){

    const text=norm(question);

    /*
       NEVER SEARCH THESE TYPES OF THINGS.
    */

    const basicConversationPatterns=[

        /^(hi|hey|hello|yo|sup)$/,
        /^good morning$/,
        /^good afternoon$/,
        /^good evening$/,
        /^how are you$/,
        /^how are things$/,
        /^what are you doing$/,
        /^what are you up to$/,
        /^thanks$/,
        /^thank you$/,
        /^thx$/,
        /^ok$/,
        /^okay$/,
        /^cool$/,
        /^nice$/,
        /^great$/,
        /^awesome$/,
        /^bye$/,
        /^goodbye$/,
        /^see you$/,
        /^later$/,
        /^tell me a joke$/,
        /^make me laugh$/,
        /^im bored$/,
        /^im confused$/,
        /^are you online$/,
        /^who are you$/,
        /^what are you$/,
        /^what can you do$/,
        /^what do you do$/,
        /^what is my name$/,
        /^do you know my name$/,
        /^what did i ask$/,
        /^what did i just ask$/,
        /^tell me more$/,
        /^go on$/,
        /^continue$/,
        /^explain more$/
    ];

    for(const pattern of basicConversationPatterns){

        if(pattern.test(text)){
            return false;
        }
    }

    /*
       Questions that clearly require factual knowledge.
    */

    const knowledgePatterns=[

        /\bwhat is\b/,
        /\bwhat are\b/,
        /\bwho is\b/,
        /\bwho are\b/,
        /\bwhere is\b/,
        /\bwhere are\b/,
        /\bwhen did\b/,
        /\bwhen was\b/,
        /\bwhen is\b/,
        /\bwhy did\b/,
        /\bwhy is\b/,
        /\bwhy are\b/,
        /\bhow does\b/,
        /\bhow do\b/,
        /\bhow was\b/,
        /\bhow were\b/,
        /\btell me about\b/,
        /\bexplain\b/,
        /\bdefine\b/,
        /\bmeaning of\b/,
        /\bfacts about\b/,
        /\binformation about\b/,
        /\bhistory of\b/,
        /\bcauses of\b/,
        /\beffect of\b/,
        /\beffects of\b/
    ];

    for(const pattern of knowledgePatterns){

        if(pattern.test(text)){
            return true;
        }
    }

    /*
       A question mark by itself doesn't mean Wikipedia.
       This prevents ordinary conversation from being searched.
    */

    return false;
}


/* =========================================================
   WIKIPEDIA
========================================================= */

async function wiki(question){

    let q=norm(question);

    q=q
        .replace(/^(can you|could you|please|would you)\s+/,"")
        .replace(/^(tell me about|tell me|explain|what is|whats|what are|who is|who are|how does|how do|why does|why do)\s+/,"")
        .replace(/\?+$/,"")
        .trim();

    if(!q){
        return "Please give me a topic or question.";
    }

    try{

        const searchURL=
            "https://en.wikipedia.org/w/api.php?action=query"+
            "&list=search"+
            "&srsearch="+encodeURIComponent(q)+
            "&srlimit=5"+
            "&format=json"+
            "&origin=*";

        const response=await fetch(searchURL);

        if(!response.ok){
            throw new Error("Search failed");
        }

        const data=await response.json();

        if(
            !data.query ||
            !data.query.search ||
            data.query.search.length===0
        ){

            return "I couldn't find reliable information about that.";
        }

        const title=data.query.search[0].title;

        const summaryURL=
            "https://en.wikipedia.org/api/rest_v1/page/summary/"+
            encodeURIComponent(title.replace(/ /g,"_"));

        const summaryResponse=await fetch(summaryURL);

        if(!summaryResponse.ok){
            throw new Error("Summary failed");
        }

        const summary=await summaryResponse.json();

        if(summary.extract){

            let answer=summary.extract;

            if(answer.length>1900){
                answer=answer.slice(0,1900)+"...";
            }

            return answer;
        }

        return "I found information about "+title+
            ", but I couldn't retrieve the explanation.";

    }catch(e){

        return "I don't have enough information to answer that right now.";
    }
}


/* =========================================================
   MAIN JARVIS BRAIN
========================================================= */

async function answer(question){

    /*
       PRIORITY 1:
       MATH

       If this is math, STOP HERE.
       No Wikipedia.
    */

    const mathAnswer=solveMath(question);

    if(mathAnswer!==null){
        return mathAnswer;
    }


    /*
       PRIORITY 2:
       CONVERSATION

       Greetings, jokes, memory,
       casual conversation, status,
       personality, etc.

       STOP HERE.
       No Wikipedia.
    */

    const conversationalAnswer=conversation(question);

    if(conversationalAnswer){
        return conversationalAnswer;
    }


    /*
       PRIORITY 3:
       BUILT-IN KNOWLEDGE

       Common science/Earth/space questions
       are answered immediately.
    */

    const factAnswer=builtInFact(question);

    if(factAnswer){
        return factAnswer;
    }


    /*
       PRIORITY 4:
       WEB KNOWLEDGE

       Only factual questions that actually
       look like knowledge questions reach here.
    */

    if(shouldUseKnowledgeSearch(question)){

        return await wiki(question);
    }


    /*
       If it isn't math, conversation,
       built-in knowledge, or a clear knowledge
       question, don't randomly search Wikipedia.
    */

    return "I'm listening. Tell me what you'd like to talk about.";
}


/* =========================================================
   SEND
========================================================= */

async function sendMessage(){

    const question=input.value.trim();

    if(!question) return;

    input.value="";

    memory.lastQuestion=question;
    memory.conversationCount++;

    addMessage(question,"user");

    const processing=addMessage(
        "Processing...",
        "jarvis"
    );

    try{

        const response=await answer(question);

        processing.remove();

        memory.lastAnswer=response;

        addMessage(response,"jarvis");

        speak(response);

    }catch(error){

        processing.remove();

        const fallback=
            "I encountered an error processing that request.";

        memory.lastAnswer=fallback;

        addMessage(fallback,"jarvis");

        speak(fallback);
    }
}


/* =========================================================
   EVENTS
========================================================= */

send.addEventListener(
    "click",
    sendMessage
);

input.addEventListener(
    "keydown",
    function(event){

        if(event.key==="Enter"){
            sendMessage();
        }
    }
);


/* =========================================================
   STARTUP
========================================================= */

addMessage(
    "J.A.R.V.I.S. online. All primary systems are ready. How may I assist?",
    "jarvis"
);

})();
</script>

</body>
</html>
