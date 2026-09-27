<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<title>J.A.R.V.I.S.</title>

<style>
*{box-sizing:border-box;-webkit-tap-highlight-color:transparent}

html,body{
    margin:0;
    width:100%;
    height:100%;
    overflow:hidden;
    background:#000;
    color:#00eaff;
    font-family:Arial,Helvetica,sans-serif;
}

body{
    position:fixed;
    inset:0;
}

#app{
    position:fixed;
    inset:0;
    background:
      radial-gradient(circle at 50% 40%,rgba(0,180,255,.12),transparent 45%),
      linear-gradient(rgba(0,234,255,.035) 1px,transparent 1px),
      linear-gradient(90deg,rgba(0,234,255,.035) 1px,transparent 1px),
      #000;
    background-size:auto,28px 28px,28px 28px,auto;
    transition:.4s;
}

#app.combat{
    color:#ff3030;
    background:
      radial-gradient(circle at 50% 40%,rgba(255,0,0,.14),transparent 45%),
      linear-gradient(rgba(255,0,0,.035) 1px,transparent 1px),
      linear-gradient(90deg,rgba(255,0,0,.035) 1px,transparent 1px),
      #000;
}

.header{
    position:absolute;
    top:0;
    left:0;
    right:0;
    height:68px;
    padding:0 20px;
    padding-top:env(safe-area-inset-top);
    display:flex;
    align-items:center;
    justify-content:space-between;
    background:rgba(0,8,12,.95);
    border-bottom:1px solid rgba(0,234,255,.35);
    z-index:20;
}

.combat .header{
    border-bottom-color:rgba(255,40,40,.5);
}

.logo{
    font-size:22px;
    font-weight:bold;
    letter-spacing:4px;
    text-shadow:0 0 8px #00eaff,0 0 20px rgba(0,234,255,.6);
}

.combat .logo{
    color:#ff3030;
    text-shadow:0 0 8px #ff2020,0 0 20px rgba(255,0,0,.7);
}

.status{
    font-size:10px;
    letter-spacing:2px;
    color:#00ff88;
}

.combat .status{
    color:#ff3030;
}

.dot{
    display:inline-block;
    width:7px;
    height:7px;
    border-radius:50%;
    margin-right:5px;
    background:#00ff88;
    box-shadow:0 0 10px #00ff88;
}

.combat .dot{
    background:#ff3030;
    box-shadow:0 0 10px #ff3030;
}

.main{
    position:absolute;
    top:68px;
    left:0;
    right:0;
    bottom:0;
    display:flex;
    flex-direction:column;
}

.coreArea{
    flex:0 0 225px;
    display:flex;
    align-items:center;
    justify-content:center;
}

.core{
    position:relative;
    width:165px;
    height:165px;
}

.ring{
    position:absolute;
    inset:0;
    border:2px solid rgba(0,234,255,.4);
    border-radius:50%;
}

.r1{
    border-top-color:#00eaff;
    animation:spin 8s linear infinite;
}

.r2{
    inset:20px;
    border-right-color:#008cff;
    animation:spinBack 5s linear infinite;
}

.r3{
    inset:42px;
    border-bottom-color:#00ffff;
    animation:spin 3.5s linear infinite;
}

.combat .ring{
    border-color:rgba(255,40,40,.4);
}

.combat .r1{
    border-top-color:#ff2020;
}

.combat .r2{
    border-right-color:#ff4040;
}

.combat .r3{
    border-bottom-color:#ff0000;
}

.orb{
    position:absolute;
    width:52px;
    height:52px;
    left:50%;
    top:50%;
    transform:translate(-50%,-50%);
    border-radius:50%;
    background:radial-gradient(circle,#fff 0%,#aaffff 15%,#00eaff 45%,#007cff 70%,transparent 72%);
    box-shadow:0 0 15px #00eaff,0 0 40px #00eaff,0 0 75px rgba(0,150,255,.8);
    animation:pulse 2s ease-in-out infinite;
}

.combat .orb{
    background:radial-gradient(circle,#fff 0%,#ffaaaa 15%,#ff2020 45%,#a00000 70%,transparent 72%);
    box-shadow:0 0 15px #ff2020,0 0 40px #ff2020,0 0 75px rgba(255,0,0,.8);
}

.arc{
    position:absolute;
    bottom:-25px;
    width:100%;
    text-align:center;
    font-size:9px;
    letter-spacing:3px;
    opacity:.6;
}

.chat{
    flex:1;
    min-height:0;
    overflow-y:auto;
    -webkit-overflow-scrolling:touch;
    padding:5px 15px 110px;
}

.message{
    max-width:900px;
    margin:0 auto 12px;
    padding:12px 15px;
    border-radius:10px;
    line-height:1.45;
    font-size:15px;
    overflow-wrap:anywhere;
}

.jarvis{
    background:rgba(0,160,220,.08);
    border-left:2px solid #00eaff;
}

.user{
    background:rgba(255,255,255,.05);
    border-right:2px solid rgba(255,255,255,.4);
    color:white;
}

.combat .jarvis{
    border-left-color:#ff3030;
}

.label{
    display:block;
    margin-bottom:5px;
    font-size:9px;
    letter-spacing:2px;
    opacity:.55;
}

.controls{
    position:absolute;
    left:0;
    right:0;
    bottom:0;
    min-height:82px;
    padding:9px 10px max(12px,env(safe-area-inset-bottom));
    background:rgba(0,8,12,.97);
    border-top:1px solid rgba(0,234,255,.35);
    z-index:50;
}

.combat .controls{
    border-top-color:rgba(255,40,40,.5);
}

.inputRow{
    display:flex;
    gap:7px;
    width:100%;
    max-width:900px;
    margin:auto;
}

#input{
    flex:1;
    min-width:0;
    height:48px;
    border:1px solid rgba(0,234,255,.55);
    border-radius:10px;
    outline:none;
    padding:0 13px;
    background:#03151b;
    color:white;
    font-size:16px;
}

.combat #input{
    border-color:#ff3030;
}

#mic,#send{
    height:48px;
    border:1px solid #00eaff;
    border-radius:10px;
    background:rgba(0,234,255,.08);
    color:#00eaff;
    font-weight:bold;
    cursor:pointer;
}

#mic{
    width:48px;
    flex:0 0 48px;
    font-size:18px;
}

#send{
    width:65px;
    flex:0 0 65px;
}

.combat #mic,
.combat #send{
    border-color:#ff3030;
    color:#ff3030;
}

#mic.listening{
    background:rgba(0,234,255,.3);
    box-shadow:0 0 18px rgba(0,234,255,.7);
}

.combat #mic.listening{
    background:rgba(255,0,0,.3);
    box-shadow:0 0 18px rgba(255,0,0,.7);
}

@keyframes spin{
    to{transform:rotate(360deg)}
}

@keyframes spinBack{
    to{transform:rotate(-360deg)}
}

@keyframes pulse{
    0%,100%{transform:translate(-50%,-50%) scale(.9)}
    50%{transform:translate(-50%,-50%) scale(1.1)}
}

@media(max-width:600px){
    .logo{font-size:18px}
    .status{font-size:8px}
    .coreArea{flex-basis:205px}
    .core{width:145px;height:145px}
    .message{font-size:14px}
}
</style>
</head>

<body>

<div id="app">

<header class="header">
    <div class="logo">J.A.R.V.I.S.</div>

    <div class="status">
        <span class="dot"></span>
        <span id="statusText">SYSTEMS ONLINE</span>
    </div>
</header>

<main class="main">

<section class="coreArea">
    <div class="core">
        <div class="ring r1"></div>
        <div class="ring r2"></div>
        <div class="ring r3"></div>
        <div class="orb"></div>
        <div class="arc">ARC REACTOR</div>
    </div>
</section>

<section id="chat" class="chat"></section>

</main>

<div class="controls">
    <div class="inputRow">

        <input
            id="input"
            type="text"
            autocomplete="off"
            placeholder="Ask JARVIS anything..."
        >

        <button id="mic" type="button">🎙️</button>

        <button id="send" type="button">SEND</button>

    </div>
</div>

</div>

<script>
"use strict";

/* ==============================
   ELEMENTS
============================== */

const app = document.getElementById("app");
const input = document.getElementById("input");
const mic = document.getElementById("mic");
const send = document.getElementById("send");
const chat = document.getElementById("chat");
const statusText = document.getElementById("statusText");

let processing = false;
let combatMode = false;

const memory = {
    name:"",
    lastQuestion:"",
    lastAnswer:""
};


/* ==============================
   VOICE
============================== */

function speak(text){

    if(!("speechSynthesis" in window)) return;

    speechSynthesis.cancel();

    const u = new SpeechSynthesisUtterance(text);

    u.rate = .88;
    u.pitch = .72;
    u.volume = 1;

    const voices = speechSynthesis.getVoices();

    const preferred = [
        "Daniel",
        "Alex",
        "Arthur",
        "George"
    ];

    let voice = null;

    for(const name of preferred){

        voice = voices.find(v =>
            v.name.toLowerCase().includes(name.toLowerCase())
        );

        if(voice) break;
    }

    if(!voice){

        voice = voices.find(v =>
            v.lang &&
            v.lang.toLowerCase().startsWith("en")
        );
    }

    if(voice) u.voice = voice;

    speechSynthesis.speak(u);
}


/* ==============================
   CHAT
============================== */

function addMessage(text,who="jarvis",voice=false){

    const box = document.createElement("div");

    box.className =
        "message " +
        (who === "user" ? "user" : "jarvis");

    const label = document.createElement("span");

    label.className = "label";

    label.textContent =
        who === "user" ? "YOU" : "J.A.R.V.I.S.";

    const content = document.createElement("div");

    content.textContent = text;

    box.appendChild(label);
    box.appendChild(content);

    chat.appendChild(box);

    requestAnimationFrame(() => {
        chat.scrollTop = chat.scrollHeight;
    });

    if(voice && who === "jarvis"){
        speak(text);
    }
}


/* ==============================
   COMBAT MODE
============================== */

function startCombat(){

    if(combatMode) return;

    combatMode = true;

    app.classList.add("combat");

    statusText.textContent = "COMBAT MODE";

    addMessage(
        "Combat initiated.",
        "jarvis",
        true
    );
}

function stopCombat(){

    if(!combatMode) return;

    combatMode = false;

    app.classList.remove("combat");

    statusText.textContent = "SYSTEMS ONLINE";

    addMessage(
        "Combat mode terminated. Systems returning to normal.",
        "jarvis",
        true
    );
}


/* ==============================
   BRAIN ROT
============================== */

const brainRot = [

    "skibidi",
    "skibidi toilet",
    "tung tung tung sahur",
    "tung tung tung",
    "sigma",
    "what the sigma",
    "rizz",
    "gyatt",
    "gyat",
    "fanum tax",
    "ohio",
    "only in ohio",
    "mewing",
    "looksmaxxing",
    "looksmax",
    "mogging",
    "mog",
    "aura points",
    "negative aura",
    "brainrot",
    "brain rot",
    "tralalero tralala",
    "bombardiro crocodilo",
    "bombombini gusini",
    "brr brr patapim",
    "chimpanzini bananini",
    "lirili larila",
    "cappuccino assassino",
    "ballerina cappuccina",
    "trippi troppi",
    "goofy ahh",
    "among us"
];

function isBrainRot(text){

    const clean =
        text
        .toLowerCase()
        .replace(/[^\w\s]/g," ")
        .replace(/\s+/g," ")
        .trim();

    return brainRot.some(term =>
        clean.includes(term)
    );
}

function brainRotReply(){

    const replies = [
        "Wash your brain, son. 😭",
        "J.A.R.V.I.S. detects dangerous levels of brain rot. Wash your brain, son. 😭",
        "Sir... please step away from the brain rot. 😭",
        "My processors were not designed for this level of brain rot. 😭",
        "Brain-rot levels are exceeding safe operating limits. 😭"
    ];

    return replies[
        Math.floor(Math.random()*replies.length)
    ];
}


/* ==============================
   MATH
============================== */

function solveMath(text){

    let x = text.toLowerCase();

    x = x.replace(/what is/g,"");
    x = x.replace(/calculate/g,"");
    x = x.replace(/solve/g,"");

    x = x.replace(/multiplied by/g,"*");
    x = x.replace(/divided by/g,"/");
    x = x.replace(/plus/g,"+");
    x = x.replace(/minus/g,"-");
    x = x.replace(/times/g,"*");
    x = x.replace(/over/g,"/");
    x = x.replace(/×/g,"*");
    x = x.replace(/÷/g,"/");

    x = x.replace(/[^0-9+\-*/().%\s]/g,"").trim();

    if(!x) return null;

    if(!/[+\-*/%]/.test(x)) return null;

    if(!/^[0-9+\-*/().%\s]+$/.test(x)){
        return null;
    }

    try{

        const answer =
            Function(
                '"use strict";return (' + x + ')'
            )();

        if(
            typeof answer !== "number" ||
            !Number.isFinite(answer)
        ){
            return null;
        }

        return answer;

    }catch(e){

        return null;
    }
}


/* ==============================
   KNOWLEDGE
============================== */

const knowledge = {

    "black hole":
        "A black hole is a region of spacetime where gravity is extremely strong. Beyond its event horizon, nothing can escape to the outside, including light.",

    "earth":
        "Earth is the third planet from the Sun and the only world currently known to support life.",

    "sun":
        "The Sun is the star at the center of our Solar System. It produces energy primarily through nuclear fusion.",

    "moon":
        "The Moon is Earth's natural satellite. Its gravity contributes strongly to ocean tides.",

    "gravity":
        "Gravity is the interaction associated with mass and energy. In general relativity, gravity is described through the curvature of spacetime.",

    "atom":
        "An atom is a basic unit of ordinary matter. It contains a nucleus surrounded by electrons.",

    "dna":
        "DNA stores genetic information. Its structure is a double helix containing the bases A, T, C, and G.",

    "photosynthesis":
        "Photosynthesis allows plants, algae, and some bacteria to convert light energy into chemical energy.",

    "mars":
        "Mars is the fourth planet from the Sun. It is a rocky planet with a thin atmosphere dominated by carbon dioxide.",

    "jupiter":
        "Jupiter is the largest planet in our Solar System. It is a gas giant with a powerful magnetic field.",

    "saturn":
        "Saturn is a gas giant famous for its extensive system of icy rings.",

    "venus":
        "Venus is the second planet from the Sun. Its dense atmosphere produces an extreme greenhouse effect.",

    "mercury":
        "Mercury is the smallest planet in the Solar System and the closest planet to the Sun.",

    "speed of light":
        "The speed of light in a vacuum is exactly 299,792,458 meters per second.",

    "newton":
        "Isaac Newton developed foundational laws of motion and universal gravitation and made major contributions to mathematics and optics.",

    "marie curie":
        "Marie Curie was a physicist and chemist whose research into radioactivity earned Nobel Prizes in Physics and Chemistry.",

    "shakespeare":
        "William Shakespeare was an English playwright and poet whose works include Hamlet, Macbeth, and Romeo and Juliet."
};

function findKnowledge(q){

    for(const key in knowledge){

        if(q.includes(key)){
            return knowledge[key];
        }
    }

    return null;
}


/* ==============================
   JOKES
============================== */

const jokes = [

    "Why did the computer get cold? It left its Windows open.",

    "Why was the math book sad? It had too many problems.",

    "Why don't scientists trust atoms? Because they make up everything.",

    "What is a computer's favorite snack? Microchips.",

    "Why did the robot go on vacation? It needed to recharge.",

    "Why was the computer tired? It had too many tabs open.",

    "Why did the photon refuse to check a bag? It was traveling light."

];


/* ==============================
   LOCAL CONVERSATION
============================== */

function localResponse(q){

    if(q === "combat mode"){
        startCombat();
        return null;
    }

    if(
        q === "normal mode" ||
        q === "exit combat mode" ||
        q === "end combat mode"
    ){
        stopCombat();
        return null;
    }

    if(
        q === "hi" ||
        q === "hello" ||
        q === "hey"
    ){
        return memory.name
            ? `Good to hear from you, ${memory.name}. Systems are online.`
            : "Good to hear from you. Systems are online and ready.";
    }

    if(q.startsWith("my name is ")){

        memory.name =
            q
            .replace("my name is ","")
            .trim();

        return `Understood. I'll remember you as ${memory.name}.`;
    }

    if(
        q.includes("what is my name") ||
        q.includes("what's my name")
    ){

        return memory.name
            ? `Your name is ${memory.name}.`
            : "You haven't told me your name yet.";
    }

    if(
        q.includes("who are you") ||
        q.includes("what are you")
    ){

        return "I am J.A.R.V.I.S., your digital assistant interface. I can converse with you, solve mathematics, answer questions, speak aloud, and search for information when necessary.";
    }

    if(q.includes("how are you")){
        return "All systems are operational. My processors are feeling particularly cooperative today.";
    }

    if(q.includes("what are you doing")){
        return "Monitoring the system and waiting for your next command.";
    }

    if(
        q === "thanks" ||
        q === "thank you"
    ){
        return "You're welcome. Always a pleasure.";
    }

    if(
        q === "joke" ||
        q.includes("tell me a joke")
    ){
        return jokes[
            Math.floor(Math.random()*jokes.length)
        ];
    }

    if(q.includes("what time")){

        return "The current time is " +
            new Date().toLocaleTimeString(
                [],
                {
                    hour:"numeric",
                    minute:"2-digit"
                }
            ) + ".";
    }

    if(
        q.includes("what date") ||
        q.includes("what day")
    ){

        return "Today is " +
            new Date().toLocaleDateString(
                [],
                {
                    weekday:"long",
                    month:"long",
                    day:"numeric",
                    year:"numeric"
                }
            ) + ".";
    }

    if(q === "status"){

        return combatMode
            ? "Combat systems active. All primary systems operational."
            : "Systems online. Core stable. Voice interface online.";
    }

    return null;
}


/* ==============================
   QUESTION DETECTION
============================== */

function isQuestion(q){

    if(q.includes("?")) return true;

    const words = [
        "what ",
        "why ",
        "how ",
        "when ",
        "where ",
        "who ",
        "which ",
        "can ",
        "could ",
        "would ",
        "is ",
        "are ",
        "do ",
        "does ",
        "did ",
        "will ",
        "should ",
        "explain ",
        "define ",
        "tell me about "
    ];

    return words.some(word =>
        q.startsWith(word)
    );
}


/* ==============================
   ONLINE SEARCH
   Only real questions.
============================== */

async function onlineSearch(question){

    if(!isQuestion(question)) return null;

    try{

        const url =
            "https://en.wikipedia.org/w/api.php" +
            "?action=query" +
            "&generator=search" +
            "&gsrsearch=" +
            encodeURIComponent(question) +
            "&gsrnamespace=0" +
            "&gsrlimit=5" +
            "&prop=extracts" +
            "&exintro=1" +
            "&explaintext=1" +
            "&format=json" +
            "&origin=*";

        const response =
            await fetch(url);

        if(!response.ok) return null;

        const data =
            await response.json();

        if(
            !data.query ||
            !data.query.pages
        ){
            return null;
        }

        const pages =
            Object.values(data.query.pages);

        if(!pages.length) return null;

        const words =
            question
            .toLowerCase()
            .replace(/[^\w\s]/g,"")
            .split(/\s+/)
            .filter(x => x.length > 3);

        let best = null;
        let bestScore = 0;

        for(const page of pages){

            const title =
                (page.title || "").toLowerCase();

            const extract =
                (page.extract || "").toLowerCase();

            let score = 0;

            for(const word of words){

                if(title.includes(word)){
                    score += 4;
                }

                if(extract.includes(word)){
                    score += 1;
                }
            }

            if(score > bestScore){

                bestScore = score;
                best = page;
            }
        }

        if(!best || bestScore < 2){
            return null;
        }

        let answer =
            (best.extract || "").trim();

        if(!answer) return null;

        if(answer.length > 850){
            answer =
                answer.substring(0,850) +
                "...";
        }

        return answer;

    }catch(error){

        console.log("Search error:",error);

        return null;
    }
}


/* ==============================
   BRAIN
============================== */

async function getResponse(question){

    const q =
        question
        .toLowerCase()
        .trim();


    /* BRAIN ROT ALWAYS WINS */

    if(isBrainRot(q)){
        return brainRotReply();
    }


    /* COMBAT */

    if(q === "combat mode"){
        startCombat();
        return null;
    }

    if(
        q === "normal mode" ||
        q === "exit combat mode"
    ){
        stopCombat();
        return null;
    }


    /* MATH NEVER SEARCHES */

    const math =
        solveMath(q);

    if(math !== null){
        return "The answer is " + math + ".";
    }


    /* BUILT-IN KNOWLEDGE */

    const known =
        findKnowledge(q);

    if(known){
        return known;
    }


    /* NORMAL CONVERSATION */

    const local =
        localResponse(q);

    if(local){
        return local;
    }


    /* SEARCH ONLY QUESTIONS */

    if(isQuestion(q)){

        const result =
            await onlineSearch(question);

        if(result){
            return result;
        }

        return "I couldn't find a reliable answer for that. Try asking the question another way.";
    }


    /* NO RANDOM SEARCH */

    const casual = [
        "Understood.",
        "I'm listening.",
        "Go on.",
        "Interesting.",
        "Noted.",
        "I'm with you.",
        "Fair enough."
    ];

    return casual[
        Math.floor(Math.random()*casual.length)
    ];
}


/* ==============================
   SEND
============================== */

async function sendMessage(){

    if(processing) return;

    const question =
        input.value.trim();

    if(!question) return;

    processing = true;

    addMessage(
        question,
        "user",
        false
    );

    memory.lastQuestion = question;

    input.value = "";

    try{

        const response =
            await getResponse(question);

        if(response){

            memory.lastAnswer = response;

            addMessage(
                response,
                "jarvis",
                true
            );
        }

    }catch(error){

        console.log(error);

        addMessage(
            "I encountered an internal error, but my core interface is still operational.",
            "jarvis",
            true
        );
    }

    processing = false;

    setTimeout(() => {

        try{
            input.focus({preventScroll:true});
        }catch{
            input.focus();
        }

    },50);
}


/* ==============================
   BUTTONS
============================== */

send.addEventListener(
    "click",
    sendMessage
);

input.addEventListener(
    "keydown",
    function(event){

        if(event.key === "Enter"){

            event.preventDefault();

            sendMessage();
        }
    }
);


/* ==============================
   MICROPHONE
============================== */

const Recognition =
    window.SpeechRecognition ||
    window.webkitSpeechRecognition;

let recognition = null;
let listening = false;

if(Recognition){

    recognition = new Recognition();

    recognition.lang = "en-US";
    recognition.continuous = false;
    recognition.interimResults = false;
    recognition.maxAlternatives = 1;

    recognition.onstart = function(){

        listening = true;

        mic.classList.add("listening");

        mic.textContent = "⏹️";

        statusText.textContent =
            combatMode
                ? "COMBAT • LISTENING"
                : "LISTENING...";
    };

    recognition.onresult = function(event){

        if(
            !event.results ||
            !event.results[0] ||
            !event.results[0][0]
        ){
            return;
        }

        const text =
            event.results[0][0].transcript.trim();

        if(text){

            input.value = text;

            stopListening();

            sendMessage();
        }
    };

    recognition.onerror = function(event){

        stopListening();

        let msg =
            "I couldn't access the microphone.";

        if(
            event.error === "not-allowed" ||
            event.error === "service-not-allowed"
        ){
            msg =
                "Microphone permission is blocked. Allow microphone access for this website and try again.";
        }

        if(event.error === "no-speech"){
            msg =
                "I didn't hear anything. Tap the microphone and speak again.";
        }

        addMessage(
            msg,
            "jarvis",
            true
        );
    };

    recognition.onend = function(){
        stopListening();
    };

}else{

    mic.addEventListener(
        "click",
        function(){

            addMessage(
                "This browser does not provide speech recognition to this webpage. The text interface is still fully operational.",
                "jarvis",
                true
            );
        }
    );
}


function stopListening(){

    listening = false;

    mic.classList.remove("listening");

    mic.textContent = "🎙️";

    statusText.textContent =
        combatMode
            ? "COMBAT MODE"
            : "SYSTEMS ONLINE";
}


if(Recognition){

    mic.addEventListener(
        "click",
        function(){

            if(listening){

                try{
                    recognition.stop();
                }catch(e){}

                return;
            }

            try{

                recognition.start();

            }catch(error){

                console.log(error);
            }
        }
    );
}


/* ==============================
   STARTUP
============================== */

addMessage(
    "Good day. J.A.R.V.I.S. systems are online. How may I assist you?",
    "jarvis",
    true
);

</script>

</body>
</html>
