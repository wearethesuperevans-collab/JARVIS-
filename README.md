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

/* =========================
   JARVIS MEMORY
========================= */

const memory = {
    name:"",
    lastQuestion:"",
    lastTopic:"",
    lastAnswer:""
};

/* =========================
   ELEMENTS
========================= */

const chat = document.getElementById("chat");
const input = document.getElementById("input");
const send = document.getElementById("send");

if(!chat || !input || !send) return;

/* =========================
   TEXT NORMALIZER
========================= */

function norm(text){
    return String(text || "")
        .toLowerCase()
        .replace(/[’']/g,"")
        .replace(/\s+/g," ")
        .trim();
}

/* =========================
   CHAT
========================= */

function addMessage(text,type){
    const div=document.createElement("div");

    div.className="message " + type;
    div.textContent=text;

    chat.appendChild(div);
    chat.scrollTop=chat.scrollHeight;

    return div;
}

/* =========================
   JARVIS VOICE
========================= */

function speak(text){

    if(!("speechSynthesis" in window)) return;

    try{
        speechSynthesis.cancel();

        const clean=text
            .replace(/[🧠🌎🕳️😂⚡🤖]/g,"")
            .replace(/\s+/g," ")
            .trim();

        const utterance=new SpeechSynthesisUtterance(clean);

        utterance.rate=0.88;
        utterance.pitch=0.72;
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

        if(selected) utterance.voice=selected;

        speechSynthesis.speak(utterance);

    }catch(e){}
}

/* =========================
   MATH ENGINE
   IMPORTANT:
   MATH NEVER GOES TO WIKIPEDIA
========================= */

function solveMath(text){

    let s=norm(text);

    const original=s;

    /*
       Only treat it as math when it actually
       contains mathematical patterns.
    */

    const looksLikeMath =
        /\d/.test(s) &&
        (
            /[+\-*/%^×÷=]/.test(s) ||
            /\b(plus|minus|times|multiplied|divided|over|squared|cubed|percent|percentage|sqrt|square root)\b/.test(s) ||
            /\bwhat is\b/.test(s) ||
            /\bcalculate\b/.test(s) ||
            /\bsolve\b/.test(s)
        );

    if(!looksLikeMath){
        return null;
    }

    /*
       Remove normal question wording.
    */

    s=s
        .replace(/^(what is|whats|what's|calculate|compute|find|solve)\s+/,"")
        .replace(/\?+$/,"")
        .trim();

    /*
       Powers.
    */

    s=s.replace(
        /(\d+(?:\.\d+)?)\s+squared\b/g,
        "($1**2)"
    );

    s=s.replace(
        /(\d+(?:\.\d+)?)\s+cubed\b/g,
        "($1**3)"
    );

    /*
       Math words.
    */

    s=s
        .replace(/multiplied by/g,"*")
        .replace(/times/g,"*")
        .replace(/divided by/g,"/")
        .replace(/divided into/g,"/")
        .replace(/plus/g,"+")
        .replace(/minus/g,"-")
        .replace(/negative/g,"-")
        .replace(/×/g,"*")
        .replace(/÷/g,"/")
        .replace(/\^/g,"**")
        .replace(/percent of/g,"%")
        .replace(/percentage of/g,"%");

    /*
       "square root of 144"
    */

    s=s.replace(
        /square root of\s+(\d+(?:\.\d+)?)/g,
        "Math.sqrt($1)"
    );

    /*
       Simple fraction wording.
    */

    s=s.replace(
        /(\d+)\s+over\s+(\d+)/g,
        "($1/$2)"
    );

    /*
       Allow only safe mathematical characters.
    */

    if(!/^[0-9+\-*/().%\sA-Za-z_]+$/.test(s)){
        return null;
    }

    /*
       Prevent arbitrary JavaScript.
    */

    const allowedWords=[
        "Math",
        "sqrt",
        "PI"
    ];

    const words=s.match(/[A-Za-z_]+/g) || [];

    for(const word of words){

        if(!allowedWords.includes(word)){
            return null;
        }
    }

    if(!/\d/.test(s)){
        return null;
    }

    try{

        const result=Function(
            '"use strict"; return (' + s + ')'
        )();

        if(
            typeof result !== "number" ||
            !Number.isFinite(result)
        ){
            return null;
        }

        let answer;

        if(Number.isInteger(result)){
            answer=String(result);
        }else{
            answer=String(
                Number(result.toFixed(8))
            );
        }

        return "The answer is " + answer + ".";

    }catch(e){

        return null;
    }
}

/* =========================
   BUILT-IN KNOWLEDGE
========================= */

const facts=[

{
keys:/black holes?|event horizon|singularity/,
answer:
"A black hole is a region of space where gravity is so strong that nothing that crosses its event horizon can escape, including light. Most black holes form when very massive stars collapse. Supermassive black holes can contain millions or billions of times the mass of the Sun."
},

{
keys:/gravity|gravitational force/,
answer:
"Gravity is the attraction between objects with mass. Earths gravity pulls objects toward its center, while the Sun's gravity keeps the planets in orbit."
},

{
keys:/atom|atoms/,
answer:
"An atom is the basic unit of ordinary matter. It contains a nucleus made of protons and neutrons, surrounded by electrons."
},

{
keys:/dna|genetic code/,
answer:
"DNA stores biological instructions used by living organisms. Its sequence helps cells make proteins and pass genetic information from one generation to the next."
},

{
keys:/photosynthesis/,
answer:
"Photosynthesis is how plants, algae, and some microorganisms use light energy to convert carbon dioxide and water into chemical energy, producing oxygen as a byproduct."
},

{
keys:/evolution|natural selection/,
answer:
"Evolution is the change in inherited characteristics of populations over generations. Natural selection is one major mechanism that can cause those changes."
},

{
keys:/solar system|planets/,
answer:
"Our solar system contains the Sun and everything gravitationally bound to it, including eight planets, dwarf planets, moons, asteroids, comets, and other objects."
},

{
keys:/speed of light|how fast is light/,
answer:
"Light travels through empty space at about 299,792,458 meters per second."
},

{
keys:/relativity|einstein/,
answer:
"Einstein's theories of relativity describe how space, time, motion, gravity, and energy are related. Special relativity deals with high-speed motion, while general relativity describes gravity as the curvature of spacetime."
},

{
keys:/moon/,
answer:
"The Moon is Earth's natural satellite. Its gravity contributes strongly to ocean tides, and its changing appearance from Earth is caused by the geometry between the Sun, Earth, and Moon."
},

{
keys:/earth|our planet/,
answer:
"Earth is the third planet from the Sun. It has a nitrogen-rich atmosphere, large amounts of surface water, an active geological system, and is currently the only world known to support life."
},

{
keys:/mars/,
answer:
"Mars is the fourth planet from the Sun. It is a rocky world with a thin atmosphere, polar ice caps, enormous volcanoes, and evidence that liquid water existed on its surface in the ancient past."
},

{
keys:/jupiter/,
answer:
"Jupiter is the largest planet in the solar system. It is a gas giant with a powerful magnetic field and a famous storm called the Great Red Spot."
},

{
keys:/saturn/,
answer:
"Saturn is a gas giant best known for its extensive ring system. The rings are made mostly of ice and rocky material."
},

{
keys:/venus/,
answer:
"Venus is the second planet from the Sun. It has a very thick carbon-dioxide atmosphere and an extreme greenhouse effect, making its surface hotter than Mercury's despite being farther from the Sun."
},

{
keys:/mercury/,
answer:
"Mercury is the smallest planet and the closest planet to the Sun. It has a heavily cratered surface and almost no substantial atmosphere."
},

{
keys:/neutron star|neutron stars/,
answer:
"A neutron star is the extremely dense leftover core of a massive star after a supernova. A huge amount of mass can be compressed into an object only around the size of a city."
},

{
keys:/chemistry/,
answer:
"Chemistry is the study of matter, its properties, its structure, and how substances interact and transform."
},

{
keys:/cell|cells/,
answer:
"Cells are the basic structural and functional units of living organisms. Some organisms consist of a single cell, while others contain trillions of cells."
},

{
keys:/ocean|oceans/,
answer:
"Earth's oceans cover most of the planets surface and contain an enormous variety of ecosystems. They also play a major role in regulating Earth's climate."
},

{
keys:/volcano|volcanoes/,
answer:
"A volcano is an opening or structure through which molten rock, gases, and other material can reach Earth's surface. Volcanoes are closely connected to Earth's internal heat and tectonic activity."
},

{
keys:/plate tectonic|tectonic plates/,
answer:
"Plate tectonics describes the movement of large pieces of Earth's lithosphere. Their interactions help produce mountains, earthquakes, volcanoes, and ocean basins."
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

/* =========================
   JOKES
========================= */

const jokes=[

"Why did the computer go to the doctor? It had a virus.",
"I would tell you a UDP joke, but you might not get it.",
"Why was the math book sad? It had too many problems.",
"Why do programmers prefer dark mode? Because light attracts bugs.",
"What does a computer eat? Microchips.",
"Why did the robot cross the road? Because somebody programmed it to.",
"I tried to catch some fog earlier. I mist.",
"Why did the scientist install a doorbell? He wanted to win the Nobel prize.",
"Why don't scientists trust atoms? Because they make up everything.",
"Why was the computer cold? It left its Windows open.",
"Why did the photon refuse to check a bag? It was traveling light.",
"Parallel lines have so much in common. It is a shame they will never meet.",
"Why was the equal sign so humble? Because it knew it wasn't less than or greater than anyone.",
"Why did the astronaut break up with the moon? He needed space.",
"Why was six afraid of seven? Because seven eight nine.",
"I told my computer I needed a break. Now it won't stop sending me vacation advertisements.",
"Why did the robot get promoted? Outstanding performance.",
"Why did the AI cross the road? To optimize the other side.",
"Why don't computers ever get lost? They always have a cache.",
"Why was the CPU tired? Too many processes."
];

function joke(){

    return jokes[
        Math.floor(Math.random()*jokes.length)
    ];
}

/* =========================
   LOCAL JARVIS RESPONSES
========================= */

function local(q){

    const text=norm(q);

    /* greetings */

    if(
        /^(hi|hey|hello|yo|sup|whats up|good morning|good afternoon|good evening)$/.test(text)
    ){
        return memory.name
            ? "Good to see you again, " + memory.name + ". How can I assist?"
            : "Good to see you. How can I assist?";
    }

    /* identity */

    if(
        /\b(who are you|what are you|are you jarvis|your name)\b/.test(text)
    ){
        return "I am J.A.R.V.I.S., your personal AI assistant.";
    }

    /* name */

    const nameMatch=text.match(
        /(?:my name is|call me|you can call me)\s+([a-z0-9 _-]{1,30})/
    );

    if(nameMatch){

        memory.name=nameMatch[1]
            .trim()
            .replace(/\b\w/g,c=>c.toUpperCase());

        return "Understood. I'll call you " + memory.name + ".";
    }

    if(/\b(what is my name|do you know my name)\b/.test(text)){

        if(memory.name){
            return "Your name is " + memory.name + ".";
        }

        return "You haven't told me your name yet.";
    }

    /* time */

    if(/\b(what time is it|current time|time right now)\b/.test(text)){

        return "The current time is " +
            new Date().toLocaleTimeString([],{
                hour:"numeric",
                minute:"2-digit"
            }) + ".";
    }

    /* date */

    if(/\b(what date is it|todays date|what day is it)\b/.test(text)){

        return "Today is " +
            new Date().toLocaleDateString([],{
                weekday:"long",
                month:"long",
                day:"numeric",
                year:"numeric"
            }) + ".";
    }

    /* jokes */

    if(
        /\b(tell me a joke|tell me another joke|make me laugh|joke)\b/.test(text)
    ){
        return joke();
    }

    /* thanks */

    if(
        /\b(thanks|thank you|thx)\b/.test(text)
    ){
        return "You're welcome.";
    }

    /* stop voice */

    if(
        /\b(stop talking|stop speaking|be quiet|stop voice)\b/.test(text)
    ){

        try{
            speechSynthesis.cancel();
        }catch(e){}

        return "Voice output disabled.";
    }

    /* status */

    if(
        /\b(system status|status report|how are you|systems status)\b/.test(text)
    ){
        return "All primary JARVIS systems are online and ready.";
    }

    /* reactor */

    if(
        /\b(reactor|arc reactor|power level)\b/.test(text)
    ){
        return "Arc reactor systems are stable. Power output nominal.";
    }

    /* armor */

    if(
        /\b(armor|armour|iron man suit|suit status)\b/.test(text)
    ){
        return "Armor systems are standing by. Diagnostics show no critical faults.";
    }

    /* last question */

    if(
        /\b(what did i ask|what was my question|what did i just ask)\b/.test(text)
    ){

        if(memory.lastQuestion){
            return "You asked: " + memory.lastQuestion;
        }

        return "There is no previous question in my current memory.";
    }

    /* follow-up */

    if(
        /\b(tell me more|more about that|go on|continue|explain more)\b/.test(text)
    ){

        if(memory.lastAnswer){
            return memory.lastAnswer +
                " If you'd like, you can ask me a more specific question about it.";
        }

        return "Certainly. Give me a topic and I'll explain it.";
    }

    /* boredom */

    if(
        /\b(im bored|bored)\b/.test(text)
    ){
        return "Then we have options. Ask me a science question, request a joke, or challenge me with some math.";
    }

    return null;
}

/* =========================
   WIKIPEDIA SEARCH
   ONLY NON-MATH QUESTIONS
========================= */

async function wiki(question){

    let q=norm(question);

    /*
       Remove conversational wording so questions
       like "can you explain black holes" still work.
    */

    q=q
        .replace(/^(can you|could you|please|would you)\s+/,"")
        .replace(/^(tell me about|tell me|explain|what is|whats|what are|who is|who are|how does|how do|why does|why do)\s+/,"")
        .replace(/^(more about)\s+/,"")
        .replace(/\?+$/,"")
        .trim();

    if(!q){
        return "Please give me a topic or question.";
    }

    try{

        const searchURL =
            "https://en.wikipedia.org/w/api.php?action=query" +
            "&list=search" +
            "&srsearch=" +
            encodeURIComponent(q) +
            "&srlimit=5" +
            "&format=json" +
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
            "https://en.wikipedia.org/api/rest_v1/page/summary/" +
            encodeURIComponent(title.replace(/ /g,"_"));

        const summaryResponse=await fetch(summaryURL);

        if(!summaryResponse.ok){
            throw new Error("Summary failed");
        }

        const summary=await summaryResponse.json();

        if(summary.extract){

            let answer=summary.extract;

            if(answer.length>1900){
                answer=answer.slice(0,1900) + "...";
            }

            return answer;
        }

        return "I found information about " + title + ", but I couldn't retrieve the explanation.";

    }catch(e){

        return "I don't have enough information to answer that right now.";
    }
}

/* =========================
   MAIN ANSWER SYSTEM
========================= */

async function answer(question){

    /*
       ⭐ IMPORTANT ⭐

       MATH IS CHECKED FIRST.

       If it is math and the math engine understands it,
       the function RETURNS immediately.

       That means it NEVER reaches Wikipedia.
    */

    const mathAnswer=solveMath(question);

    if(mathAnswer !== null){
        return mathAnswer;
    }

    /*
       Everything that is NOT math continues normally.
    */

    const localAnswer=local(question);

    if(localAnswer){
        return localAnswer;
    }

    const factAnswer=builtInFact(question);

    if(factAnswer){
        return factAnswer;
    }

    /*
       Only non-math questions reach Wikipedia.
    */

    return await wiki(question);
}

/* =========================
   SEND MESSAGE
========================= */

async function sendMessage(){

    const question=input.value.trim();

    if(!question) return;

    input.value="";

    memory.lastQuestion=question;

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

/* =========================
   EVENTS
========================= */

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

/* =========================
   STARTUP
========================= */

addMessage(
    "J.A.R.V.I.S. online. Systems are ready. Ask me anything.",
    "jarvis"
);

})();
</script>

</body>
</html>
