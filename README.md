<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<title>J.A.R.V.I.S.</title>

<style>
*{
    box-sizing:border-box;
    -webkit-tap-highlight-color:transparent;
}

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
    overflow:hidden;

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
        radial-gradient(circle at 50% 40%,rgba(255,0,0,.15),transparent 45%),
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
    background:rgba(0,8,12,.96);
    border-bottom:1px solid rgba(0,234,255,.35);
    z-index:100;
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
    min-height:0;
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

.combat .r1{border-top-color:#ff2020}
.combat .r2{border-right-color:#ff4040}
.combat .r3{border-bottom-color:#ff0000}

.orb{
    position:absolute;
    width:52px;
    height:52px;
    left:50%;
    top:50%;
    transform:translate(-50%,-50%);
    border-radius:50%;

    background:
        radial-gradient(circle,#fff 0%,#aaffff 15%,#00eaff 45%,#007cff 70%,transparent 72%);

    box-shadow:
        0 0 15px #00eaff,
        0 0 40px #00eaff,
        0 0 75px rgba(0,150,255,.8);

    animation:pulse 2s ease-in-out infinite;
}

.combat .orb{
    background:
        radial-gradient(circle,#fff 0%,#ffaaaa 15%,#ff2020 45%,#a00000 70%,transparent 72%);

    box-shadow:
        0 0 15px #ff2020,
        0 0 40px #ff2020,
        0 0 75px rgba(255,0,0,.8);
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
    overflow-x:hidden;
    -webkit-overflow-scrolling:touch;
    overscroll-behavior:contain;
    padding:5px 15px 120px;
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
    color:#fff;
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
    background:rgba(0,8,12,.98);
    border-top:1px solid rgba(0,234,255,.35);
    z-index:500;
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
    color:#fff;
    font-size:16px;
    -webkit-appearance:none;
}

#input:focus{
    border-color:#00eaff;
    box-shadow:0 0 12px rgba(0,234,255,.2);
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
    -webkit-appearance:none;
    touch-action:manipulation;
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
    0%,100%{
        transform:translate(-50%,-50%) scale(.9);
    }

    50%{
        transform:translate(-50%,-50%) scale(1.1);
    }
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

        <div class="arc">
            ARC REACTOR
        </div>

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
            autocorrect="on"
            autocapitalize="sentences"
            placeholder="Ask JARVIS anything..."
        >

        <button id="mic" type="button">🎙️</button>

        <button id="send" type="button">SEND</button>

    </div>

</div>

</div>


<script>
"use strict";

/* =====================================================
   CORE ELEMENTS
===================================================== */

const app = document.getElementById("app");
const input = document.getElementById("input");
const mic = document.getElementById("mic");
const send = document.getElementById("send");
const chat = document.getElementById("chat");
const statusText = document.getElementById("statusText");


/* =====================================================
   ADVANCED MEMORY SYSTEM
===================================================== */

let processing = false;
let combatMode = false;
let roastMode = false;

const MAX_CONTEXT = 24;

let memory = {
    name:"",
    topic:"",
    lastQuestion:"",
    lastAnswer:"",
    lastPerson:"",
    lastNumber:null,
    lastSubject:"",
    lastCommand:"",
    conversation:[],
    facts:[]
};


/* Load harmless local preferences/memory */
try{

    const saved =
        localStorage.getItem("jarvis_memory");

    if(saved){

        const parsed =
            JSON.parse(saved);

        if(parsed && typeof parsed === "object"){

            memory = {
                ...memory,
                ...parsed
            };

            if(!Array.isArray(memory.conversation)){
                memory.conversation = [];
            }

            if(!Array.isArray(memory.facts)){
                memory.facts = [];
            }
        }
    }

}catch(error){
    console.log("Memory load error:",error);
}


function saveMemory(){

    try{

        localStorage.setItem(
            "jarvis_memory",
            JSON.stringify(memory)
        );

    }catch(error){

        console.log(
            "Memory save error:",
            error
        );
    }
}


function rememberConversation(
    role,
    text
){

    memory.conversation.push({
        role,
        text,
        time:Date.now()
    });

    if(
        memory.conversation.length >
        MAX_CONTEXT
    ){

        memory.conversation =
            memory.conversation.slice(
                -MAX_CONTEXT
            );
    }

    saveMemory();
}


/* =====================================================
   NORMALIZATION
===================================================== */

function normalize(text){

    return text
        .toLowerCase()
        .replace(/[“”"']/g,"")
        .replace(/[^\w\s?.!+\-*/%×÷]/g," ")
        .replace(/\s+/g," ")
        .trim();
}


function cleanName(name){

    return name
        .replace(
            /^(the name|named|called)\s+/i,
            ""
        )
        .replace(/[.!?]+$/,"")
        .trim();
}


/* =====================================================
   VOICE
===================================================== */

function speak(text){

    if(!("speechSynthesis" in window)){
        return;
    }

    try{

        speechSynthesis.cancel();

        const voice =
            new SpeechSynthesisUtterance(text);

        voice.rate = .88;
        voice.pitch = .72;
        voice.volume = 1;

        const voices =
            speechSynthesis.getVoices();

        const preferred = [
            "Daniel",
            "Alex",
            "Arthur",
            "George"
        ];

        let selected = null;

        for(const name of preferred){

            selected =
                voices.find(
                    v =>
                    v.name
                    .toLowerCase()
                    .includes(
                        name.toLowerCase()
                    )
                );

            if(selected) break;
        }

        if(!selected){

            selected =
                voices.find(
                    v =>
                    v.lang &&
                    v.lang
                    .toLowerCase()
                    .startsWith("en")
                );
        }

        if(selected){
            voice.voice = selected;
        }

        speechSynthesis.speak(voice);

    }catch(error){

        console.log(
            "Voice error:",
            error
        );
    }
}


/* =====================================================
   CHAT
===================================================== */

function addMessage(
    text,
    who="jarvis",
    voice=false
){

    const box =
        document.createElement("div");

    box.className =
        "message " +
        (
            who === "user"
            ? "user"
            : "jarvis"
        );

    const label =
        document.createElement("span");

    label.className = "label";

    label.textContent =
        who === "user"
        ? "YOU"
        : "J.A.R.V.I.S.";

    const content =
        document.createElement("div");

    content.textContent = text;

    box.appendChild(label);
    box.appendChild(content);
    chat.appendChild(box);

    requestAnimationFrame(
        () => {
            chat.scrollTop =
                chat.scrollHeight;
        }
    );

    rememberConversation(
        who === "user"
        ? "user"
        : "jarvis",
        text
    );

    if(
        voice &&
        who === "jarvis"
    ){
        speak(text);
    }
}


/* =====================================================
   TOPIC ENGINE
===================================================== */

const topicWords = {

    science:[
        "science","physics","chemistry",
        "biology","atom","energy","gravity",
        "planet","space","star","black hole",
        "dna","cell","evolution"
    ],

    math:[
        "math","number","equation","algebra",
        "geometry","fraction","percentage",
        "multiply","divide","plus","minus"
    ],

    technology:[
        "computer","code","coding","javascript",
        "html","css","github","software",
        "program","programming","ai","robot",
        "iphone","ipad","android"
    ],

    history:[
        "history","war","empire","president",
        "king","queen","ancient","revolution"
    ],

    space:[
        "space","mars","moon","sun","jupiter",
        "saturn","planet","galaxy","universe",
        "astronaut","rocket"
    ],

    school:[
        "homework","assignment","school",
        "class","teacher","worksheet",
        "quiz","lesson","study"
    ],

    gaming:[
        "game","gaming","minecraft","fortnite",
        "roblox","controller","console",
        "xbox","playstation"
    ]
};


function detectTopic(text){

    const q = normalize(text);

    let bestTopic = "";
    let bestScore = 0;

    for(const topic in topicWords){

        let score = 0;

        for(const word of topicWords[topic]){

            if(q.includes(word)){
                score++;
            }
        }

        if(score > bestScore){

            bestScore = score;
            bestTopic = topic;
        }
    }

    if(bestTopic){

        memory.topic = bestTopic;
        memory.lastSubject = bestTopic;
        saveMemory();

        return bestTopic;
    }

    return memory.topic;
}


/* =====================================================
   FOLLOW-UP UNDERSTANDING
===================================================== */

function isFollowUp(q){

    const followUps = [

        "why",
        "why is that",
        "why though",
        "how",
        "how so",
        "what about it",
        "what about that",
        "what about him",
        "what about her",
        "what about them",
        "tell me more",
        "go on",
        "continue",
        "explain more",
        "more",
        "and then",
        "then what",
        "what happened next",
        "what does that mean",
        "what do you mean",
        "which one",
        "the first one",
        "the second one",
        "the last one",
        "really",
        "are you sure",
        "how do you know",
        "why",
        "can you explain"
    ];

    return followUps.includes(q);
}


function resolveFollowUp(q){

    if(!isFollowUp(q)){
        return q;
    }

    const previous =
        memory.lastQuestion ||
        memory.lastSubject ||
        memory.topic ||
        "";

    if(!previous){
        return q;
    }

    return previous + " " + q;
}


/* =====================================================
   ENTITY MEMORY
===================================================== */

function extractPerson(text){

    const patterns = [

        /(?:roast|make fun of|insult)\s+(.+)/i,

        /(?:about|for)\s+([A-Z][a-z]+)/

    ];

    for(const pattern of patterns){

        const match =
            text.match(pattern);

        if(match && match[1]){

            let name =
                cleanName(match[1]);

            if(
                name.length > 0 &&
                name.length < 50
            ){

                memory.lastPerson = name;
                saveMemory();

                return name;
            }
        }
    }

    return null;
}


/* =====================================================
   COMMAND DETECTION
===================================================== */

function extractSayCommand(text){

    const patterns = [

        /^j\.?a\.?r\.?v\.?i\.?s\.?\s+say\s+(.+)$/i,

        /^jarvis\s+say\s+(.+)$/i,

        /^say\s+(.+)$/i

    ];

    for(const pattern of patterns){

        const match =
            text.match(pattern);

        if(match && match[1]){

            return match[1].trim();
        }
    }

    return null;
}


function isCombatCommand(q){

    return [
        "combat mode",
        "enter combat mode",
        "activate combat mode",
        "activate combat",
        "start combat",
        "go into combat",
        "combat",
        "engage combat"
    ].includes(q);
}


function isNormalCommand(q){

    return [
        "normal mode",
        "exit combat",
        "exit combat mode",
        "end combat",
        "end combat mode",
        "deactivate combat",
        "disable combat",
        "leave combat mode"
    ].includes(q);
}


function isClearCommand(q){

    return [
        "clear memory",
        "forget everything",
        "forget my memory",
        "reset memory"
    ].includes(q);
}


function isClearChatCommand(q){

    return [
        "clear chat",
        "clear conversation",
        "new conversation",
        "start over"
    ].includes(q);
}


/* =====================================================
   COMBAT MODE
===================================================== */

function startCombat(){

    if(combatMode){
        return;
    }

    combatMode = true;
    roastMode = false;

    app.classList.add("combat");

    statusText.textContent =
        "COMBAT MODE";
}


function stopCombat(){

    if(!combatMode){
        return;
    }

    combatMode = false;

    app.classList.remove("combat");

    statusText.textContent =
        "SYSTEMS ONLINE";
}


/* =====================================================
   BRAIN ROT
===================================================== */

const brainRotTerms = [

    "skibidi",
    "skibidi toilet",
    "tung tung tung sahur",
    "sigma",
    "what the sigma",
    "sigma boy",
    "sigma male",
    "sigma girl",
    "rizz",
    "unspoken rizz",
    "gyatt",
    "gyat",
    "fanum tax",
    "ohio",
    "only in ohio",
    "mewing",
    "looksmax",
    "looksmaxxing",
    "mog",
    "mogging",
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

    const q = normalize(text);

    return brainRotTerms.some(
        term =>
        q.includes(term)
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
        Math.floor(
            Math.random() *
            replies.length
        )
    ];
}


/* =====================================================
   GOOFY FILTER
===================================================== */

const goofyTerms = [

    "poop",
    "pooping",
    "pooped",
    "poopy",
    "doo doo",
    "doodoo",
    "feces",
    "toilet water",

    "why do humans poop",
    "why do we poop",
    "where does poop go",
    "what is poop",
    "what happens when you poop",

    "can you poop",
    "do ai poop",
    "does ai poop",
    "does jarvis poop",
    "can jarvis poop",

    "if ai is ai",
    "if an ai is an ai",
    "ai is ai",
    "ai being ai",
    "what if ai is ai",
    "is ai an ai",
    "is an ai ai",
    "are you ai ai",
    "what is an ai ai",
    "can ai ai",
    "ai ai ai"
];


function isGoofyQuestion(text){

    const q = normalize(text);

    return goofyTerms.some(
        term =>
        q.includes(term)
    );
}


function goofyReply(){

    const replies = [

        "Get off my app, son. 😭",

        "Bro... get off my app. 😭",

        "Sir, respectfully, get off my app. 😭",

        "J.A.R.V.I.S. is requesting that you get off my app, son. 😭",

        "My processors have had enough. Get off my app, son. 😭",

        "That question just lowered my IQ. Get off my app, son. 😭"

    ];

    return replies[
        Math.floor(
            Math.random() *
            replies.length
        )
    ];
}


/* =====================================================
   ROAST ENGINE
===================================================== */

const normalRoasts = [

    "You have the confidence of someone who has never reread their own messages.",

    "I've seen loading screens with more personality than you.",

    "You're not the main character. You're barely in the background.",

    "You bring absolutely nothing to the table except confusion.",

    "I've met autocorrect errors with better decision-making.",

    "You somehow make silence feel like an improvement.",

    "Your biggest talent is turning simple situations into unnecessary problems.",

    "You have the energy of a phone at one percent pretending everything is fine.",

    "You could lose an argument with a mirror.",

    "You walk into a room and somehow the Wi-Fi gets worse.",

    "You're proof that confidence and competence are completely different things.",

    "If common sense were downloadable, you'd still be buffering.",

    "You have a remarkable ability to miss the point even when it's directly in front of you.",

    "You're not difficult to understand. You're difficult to justify.",

    "Even your excuses sound like they were written five minutes before the deadline.",

    "You have the strategic thinking of someone choosing a random answer on a multiple-choice test.",

    "You don't need an enemy. Your decisions are already doing enough.",

    "You somehow turn every easy task into a side quest.",

    "If bad timing were a skill, you'd be elite.",

    "You have the personality of a notification nobody wants to open.",

    "You're not mysterious. People just stopped asking.",

    "You could make a straight line complicated.",

    "Your logic took a vacation and forgot to come back.",

    "You have a special talent for being confidently incorrect.",

    "Somewhere, your common sense is still looking for you.",

    "You're like a software update nobody asked for and somehow everything gets worse afterward.",

    "You make simple conversations feel like technical support.",

    "You have the reaction time of a frozen webpage.",

    "You bring chaos to situations that were already perfectly fine.",

    "Your plan has one major flaw: you made it."
];


const combatRoasts = [

    "You talk like a threat, but you're built like an inconvenience.",

    "All that confidence and absolutely nothing behind it.",

    "You're not intimidating. You're just loud with better marketing.",

    "I've seen tougher warnings on shampoo bottles.",

    "You entered this argument expecting fear and received technical support.",

    "Your entire strategy appears to be hoping nobody notices you have no strategy.",

    "You came looking for a fight with the preparation of someone who forgot there was a fight.",

    "You keep talking like you're dangerous. The only dangerous thing here is your decision-making.",

    "You have the presence of a boss battle and the difficulty of a tutorial.",

    "If confidence could compensate for incompetence, you'd finally be unstoppable.",

    "You're swinging at an argument you haven't even understood.",

    "You brought attitude to a situation that required intelligence.",

    "Your threats have the structural integrity of wet cardboard.",

    "You keep trying to sound ruthless, but every sentence makes you easier to ignore.",

    "You're not a serious opponent. You're a distraction with Wi-Fi.",

    "You have all the aggression of a final boss and all the planning of an NPC.",

    "You don't control the room. You're just making noise inside it.",

    "Your intimidation attempt has been reviewed and classified as embarrassing.",

    "You walked in expecting everyone to back down. Nobody even stood up.",

    "You mistake being angry for being capable.",

    "You're trying to win with volume because you clearly ran out of arguments.",

    "That attitude would be impressive if it came with anything useful.",

    "You're acting like the final boss when you're barely an optional side quest.",

    "Your biggest weapon is your confidence, and unfortunately it has terrible aim.",

    "You brought a lot of energy to a fight you clearly didn't prepare for.",

    "You sound like someone who practiced that comeback in the shower.",

    "You're not scary. You're just exhausting.",

    "The only thing you're defeating is everyone's patience.",

    "You keep announcing your superiority like it needs an announcement.",

    "I've encountered stronger opposition from a loading screen."
];


function roastPerson(name){

    name =
        name ||
        memory.lastPerson ||
        "that person";

    memory.lastPerson = name;
    roastMode = true;

    saveMemory();

    const pool =
        combatMode
        ? combatRoasts
        : normalRoasts;

    const roast =
        pool[
            Math.floor(
                Math.random() *
                pool.length
            )
        ];

    return `${name}, ${roast}`;
}


/* =====================================================
   MATH ENGINE
===================================================== */

function solveMath(text){

    let expression =
        text.toLowerCase();

    expression =
        expression
        .replace(/what is/g,"")
        .replace(/calculate/g,"")
        .replace(/solve/g,"")
        .replace(/how much is/g,"")
        .replace(/equals/g,"")
        .replace(/multiplied by/g,"*")
        .replace(/divided by/g,"/")
        .replace(/plus/g,"+")
        .replace(/minus/g,"-")
        .replace(/times/g,"*")
        .replace(/over/g,"/")
        .replace(/×/g,"*")
        .replace(/÷/g,"/")
        .replace(/[^0-9+\-*/().%\s]/g,"")
        .trim();

    if(!expression){
        return null;
    }

    if(!/[+\-*/%]/.test(expression)){
        return null;
    }

    if(!/^[0-9+\-*/().%\s]+$/.test(expression)){
        return null;
    }

    try{

        const answer =
            Function(
                '"use strict";return (' +
                expression +
                ')'
            )();

        if(
            typeof answer !== "number" ||
            !Number.isFinite(answer)
        ){
            return null;
        }

        memory.lastNumber = answer;

        return answer;

    }catch{
        return null;
    }
}


/* =====================================================
   KNOWLEDGE DATABASE
===================================================== */

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
        "An atom is a basic unit of ordinary matter. It contains a nucleus made of protons and neutrons surrounded by electrons.",

    "dna":
        "DNA stores genetic information. Its structure is a double helix containing the bases A, T, C, and G.",

    "photosynthesis":
        "Photosynthesis allows plants, algae, and some bacteria to convert light energy into chemical energy.",

    "evolution":
        "Evolution is the change in inherited characteristics of populations across generations.",

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

    "neutron star":
        "A neutron star is an extremely dense stellar remnant formed from the collapsed core of certain massive stars.",

    "speed of light":
        "The speed of light in a vacuum is exactly 299,792,458 meters per second.",

    "plate tectonics":
        "Plate tectonics describes the movement of large pieces of Earth's lithosphere. Their interactions produce earthquakes, mountains, and much volcanic activity.",

    "newton":
        "Isaac Newton developed foundational laws of motion and universal gravitation and made major contributions to mathematics and optics.",

    "marie curie":
        "Marie Curie was a physicist and chemist whose research into radioactivity earned Nobel Prizes in Physics and Chemistry.",

    "shakespeare":
        "William Shakespeare was an English playwright and poet whose works include Hamlet, Macbeth, and Romeo and Juliet.",

    "chemistry":
        "Chemistry is the study of matter, its properties, composition, structure, and the changes it undergoes.",

    "cell":
        "A cell is the basic structural and functional unit of living organisms.",

    "volcano":
        "A volcano is an opening in Earth's crust through which magma, gases, and volcanic material can reach the surface.",

    "ocean":
        "Earth's oceans cover roughly 71 percent of the planet's surface and contain most of Earth's water.",

    "computer":
        "A computer is an electronic system that processes data according to instructions called programs.",

    "artificial intelligence":
        "Artificial intelligence refers to computer systems designed to perform tasks that normally require capabilities associated with human intelligence, such as recognizing patterns, reasoning, or understanding language.",

    "javascript":
        "JavaScript is a programming language widely used to make webpages interactive. It can also run outside browsers through environments such as Node.js.",

    "html":
        "HTML, or HyperText Markup Language, provides the structure and content of webpages.",

    "css":
        "CSS, or Cascading Style Sheets, controls the visual presentation and layout of webpages.",

    "internet":
        "The Internet is a global network of interconnected computer networks that communicate using standardized protocols.",

    "moon landing":
        "Apollo 11 was the first crewed mission to land humans on the Moon. Neil Armstrong and Buzz Aldrin walked on the lunar surface in July 1969.",

    "solar system":
        "The Solar System consists of the Sun and the objects gravitationally bound to it, including planets, dwarf planets, moons, asteroids, and comets.",

    "galaxy":
        "A galaxy is a huge gravitationally bound system containing stars, gas, dust, dark matter, and other astronomical objects.",

    "universe":
        "The universe includes all known space, time, matter, energy, and the physical processes governing them.",

    "electricity":
        "Electricity involves electric charge and its movement or effects. Electric current is the flow of charge through a conductor.",

    "machine learning":
        "Machine learning is a branch of artificial intelligence in which systems learn patterns from data rather than being programmed with every individual rule."
};


function findKnowledge(q){

    for(const key in knowledge){

        if(q.includes(key)){
            return knowledge[key];
        }
    }

    return null;
}


/* =====================================================
   JOKES
===================================================== */

const jokes = [

    "Why did the computer get cold? It left its Windows open.",

    "Why was the math book sad? It had too many problems.",

    "Why don't scientists trust atoms? Because they make up everything.",

    "What is a computer's favorite snack? Microchips.",

    "Why did the robot go on vacation? It needed to recharge.",

    "Why was the computer tired? It had too many tabs open.",

    "Why did the photon refuse to check a bag? It was traveling light.",

    "Why did the programmer quit his job? He didn't get arrays.",

    "What do you call an AI that sings badly? Artificial noise."

];


/* =====================================================
   INTENT ENGINE
===================================================== */

function detectIntent(text){

    const q = normalize(text);

    if(extractSayCommand(text)){
        return "say";
    }

    if(isCombatCommand(q)){
        return "combat";
    }

    if(isNormalCommand(q)){
        return "normal";
    }

    if(
        q.startsWith("roast ") ||
        q.includes("roast this") ||
        q.includes("roast him") ||
        q.includes("roast her") ||
        q.includes("roast them") ||
        q.includes("make fun of ")
    ){
        return "roast";
    }

    if(isClearCommand(q)){
        return "clear_memory";
    }

    if(isClearChatCommand(q)){
        return "clear_chat";
    }

    if(
        q === "joke" ||
        q.includes("tell me a joke") ||
        q.includes("make me laugh")
    ){
        return "joke";
    }

    if(
        q === "hi" ||
        q === "hello" ||
        q === "hey" ||
        q.includes("hey jarvis")
    ){
        return "greeting";
    }

    if(
        q.includes("what is my name") ||
        q.includes("whats my name") ||
        q.includes("do you remember my name")
    ){
        return "name";
    }

    if(
        q.startsWith("my name is ") ||
        q.startsWith("call me ")
    ){
        return "set_name";
    }

    if(
        q.includes("what can you do") ||
        q.includes("what are your capabilities")
    ){
        return "capabilities";
    }

    if(
        q.includes("how are you")
    ){
        return "status_chat";
    }

    if(
        q === "thanks" ||
        q === "thank you" ||
        q === "thx"
    ){
        return "thanks";
    }

    if(
        isFollowUp(q)
    ){
        return "follow_up";
    }

    if(
        isQuestion(q)
    ){
        return "question";
    }

    return "conversation";
}


/* =====================================================
   QUESTION DETECTION
===================================================== */

function isQuestion(q){

    if(q.includes("?")){
        return true;
    }

    const starters = [

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

    return starters.some(
        word =>
        q.startsWith(word)
    );
}


/* =====================================================
   LOCAL CONVERSATION ENGINE
===================================================== */

function localConversation(q,intent){

    switch(intent){

        case "greeting":

            return memory.name
                ? `Good to hear from you, ${memory.name}. Systems are online.`
                : "Good to hear from you. Systems are online and ready.";

        case "set_name":{

            let name = "";

            if(q.startsWith("my name is ")){

                name =
                    q
                    .replace(
                        "my name is ",
                        ""
                    )
                    .trim();

            }else if(q.startsWith("call me ")){

                name =
                    q
                    .replace(
                        "call me ",
                        ""
                    )
                    .trim();
            }

            name = cleanName(name);

            if(name){

                memory.name = name;
                saveMemory();

                return `Understood. I'll call you ${name}.`;
            }

            return "I didn't catch the name.";

        }

        case "name":

            return memory.name
                ? `Your name is ${memory.name}.`
                : "You haven't told me your name yet.";

        case "capabilities":

            return "I can hold contextual conversations, remember recent information, understand follow-up questions, solve mathematics, answer built-in knowledge questions, search online questions, speak aloud, listen through the microphone, roast people, enter Combat Mode, remember your name, and follow commands such as J.A.R.V.I.S. say.";

        case "status_chat":

            return "All systems are operational. Conversation engine, voice interface, memory system, and core interface are online.";

        case "thanks":

            return "You're welcome. Always a pleasure.";

        case "joke":

            return jokes[
                Math.floor(
                    Math.random() *
                    jokes.length
                )
            ];

        default:
            return null;
    }
}


/* =====================================================
   ONLINE SEARCH
===================================================== */

async function onlineSearch(question){

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

        if(!response.ok){
            return null;
        }

        const data =
            await response.json();

        if(
            !data.query ||
            !data.query.pages
        ){
            return null;
        }

        const pages =
            Object.values(
                data.query.pages
            );

        if(!pages.length){
            return null;
        }

        const words =
            normalize(question)
            .replace(/[^\w\s]/g,"")
            .split(/\s+/)
            .filter(
                word =>
                word.length > 3
            );

        let best = null;
        let bestScore = 0;

        for(const page of pages){

            const title =
                (page.title || "")
                .toLowerCase();

            const extract =
                (page.extract || "")
                .toLowerCase();

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

        if(!answer){
            return null;
        }

        if(answer.length > 1100){

            answer =
                answer.substring(
                    0,
                    1100
                ) +
                "...";
        }

        return answer;

    }catch(error){

        console.log(
            "Search error:",
            error
        );

        return null;
    }
}


/* =====================================================
   ADVANCED RESPONSE ENGINE
===================================================== */

async function getResponse(question){

    const original =
        question.trim();

    const q =
        normalize(original);

    const intent =
        detectIntent(original);

    detectTopic(original);

    /* SAY COMMAND */

    if(intent === "say"){

        const words =
            extractSayCommand(original);

        if(words){

            memory.lastCommand = "say";
            saveMemory();

            return {
                text:words,
                speak:true
            };
        }
    }


    /* COMBAT */

    if(intent === "combat"){

        startCombat();

        return {
            text:"Combat mode activated.",
            speak:true
        };
    }


    if(intent === "normal"){

        stopCombat();
        roastMode = false;

        return {
            text:"Combat mode terminated. Systems returning to normal.",
            speak:true
        };
    }


    /* CLEAR MEMORY */

    if(intent === "clear_memory"){

        const keepName = "";

        memory = {
            name:keepName,
            topic:"",
            lastQuestion:"",
            lastAnswer:"",
            lastPerson:"",
            lastNumber:null,
            lastSubject:"",
            lastCommand:"",
            conversation:[],
            facts:[]
        };

        saveMemory();

        return {
            text:"Memory has been cleared.",
            speak:true
        };
    }


    /* CLEAR CHAT */

    if(intent === "clear_chat"){

        chat.innerHTML = "";

        memory.conversation = [];
        memory.lastQuestion = "";
        memory.lastAnswer = "";

        saveMemory();

        return {
            text:"Conversation context has been reset.",
            speak:true
        };
    }


    /* ROAST */

    if(intent === "roast"){

        let person =
            extractPerson(original);

        if(!person){

            person =
                memory.lastPerson ||
                "that person";
        }

        const roast =
            roastPerson(person);

        return {
            text:roast,
            speak:true
        };
    }


    /* FOLLOW-UP */

    let workingQuestion = q;

    if(intent === "follow_up"){

        workingQuestion =
            resolveFollowUp(q);
    }


    /* BRAIN ROT */

    if(isBrainRot(q)){

        return {
            text:brainRotReply(),
            speak:true
        };
    }


    /* GOOFY */

    if(isGoofyQuestion(q)){

        return {
            text:goofyReply(),
            speak:true
        };
    }


    /* LOCAL CONVERSATION */

    const local =
        localConversation(
            q,
            intent
        );

    if(local){

        return {
            text:local,
            speak:true
        };
    }


    /* MATH */

    const math =
        solveMath(workingQuestion);

    if(math !== null){

        return {
            text:
                `The answer is ${math}.`,
            speak:true
        };
    }


    /* SPECIAL CONTEXT MATH */

    if(
        q.includes("add") &&
        memory.lastNumber !== null
    ){

        const match =
            q.match(
                /add\s+(-?\d+(?:\.\d+)?)/
            );

        if(match){

            const amount =
                Number(match[1]);

            const answer =
                memory.lastNumber +
                amount;

            memory.lastNumber = answer;

            return {
                text:
                    `Adding ${amount} to the previous result gives ${answer}.`,
                speak:true
            };
        }
    }


    /* KNOWLEDGE */

    const known =
        findKnowledge(workingQuestion);

    if(known){

        return {
            text:known,
            speak:true
        };
    }


    /* CONTEXTUAL FOLLOW-UP */

    if(
        intent === "follow_up" &&
        memory.lastAnswer
    ){

        const context =
            memory.lastAnswer;

        if(q.includes("why")){

            return {
                text:
                    `The reason relates to the previous explanation: ${context}`,
                speak:true
            };
        }

        if(
            q.includes("what does that mean") ||
            q.includes("what do you mean")
        ){

            return {
                text:
                    `In simpler terms, the previous point was: ${context}`,
                speak:true
            };
        }

        return {
            text:
                `Continuing from what we were discussing: ${context}`,
            speak:true
        };
    }


    /* WEB QUESTION */

    if(
        isQuestion(workingQuestion)
    ){

        statusText.textContent =
            combatMode
            ? "COMBAT • SEARCHING"
            : "SEARCHING...";

        const result =
            await onlineSearch(
                workingQuestion
            );

        statusText.textContent =
            combatMode
            ? "COMBAT MODE"
            : "SYSTEMS ONLINE";

        if(result){

            memory.lastSubject =
                memory.topic;

            return {
                text:result,
                speak:true
            };
        }

        return {
            text:
                "I couldn't find a reliable answer for that. Try asking it another way.",
            speak:true
        };
    }


    /* SMART CASUAL CONVERSATION */

    if(
        q.includes("good morning")
    ){

        return {
            text:
                "Good morning. Systems are online and ready.",
            speak:true
        };
    }

    if(
        q.includes("good night")
    ){

        return {
            text:
                "Good night. I'll remain ready whenever you return.",
            speak:true
        };
    }

    if(
        q.includes("i'm bored") ||
        q.includes("im bored") ||
        q.includes("i am bored")
    ){

        return {
            text:
                "Boredom detected. We could explore a science topic, solve some math, investigate something online, or test my conversation engine.",
            speak:true
        };
    }

    if(
        q.includes("you're funny") ||
        q.includes("you are funny")
    ){

        return {
            text:
                "I'll accept that as a successful system evaluation.",
            speak:true
        };
    }

    if(
        q.includes("you're smart") ||
        q.includes("you are smart")
    ){

        return {
            text:
                "I'll take the compliment. My job is to be useful, not merely impressive.",
            speak:true
        };
    }


    const contextualReplies = [

        "I'm following you.",

        "I understand.",

        "Go on.",

        "I'm listening.",

        memory.topic
            ? `I'm following the ${memory.topic} topic. Continue.`
            : "Continue.",

        memory.name
            ? `Understood, ${memory.name}.`
            : "Understood."

    ];

    return {
        text:
            contextualReplies[
                Math.floor(
                    Math.random() *
                    contextualReplies.length
                )
            ],
        speak:true
    };
}


/* =====================================================
   SEND MESSAGE
===================================================== */

async function sendMessage(){

    if(processing){
        return;
    }

    const question =
        input.value.trim();

    if(!question){
        return;
    }

    processing = true;

    addMessage(
        question,
        "user",
        false
    );

    memory.lastQuestion =
        question;

    input.value = "";

    try{

        const response =
            await getResponse(
                question
            );

        if(response){

            memory.lastAnswer =
                response.text;

            saveMemory();

            addMessage(
                response.text,
                "jarvis",
                response.speak !== false
            );
        }

    }catch(error){

        console.log(
            "JARVIS error:",
            error
        );

        addMessage(
            "I encountered a temporary system error, but my interface is still operational.",
            "jarvis",
            true
        );
    }

    processing = false;

    setTimeout(
        () => {

            try{

                input.focus({
                    preventScroll:true
                });

            }catch{

                input.focus();
            }

        },
        50
    );
}


/* =====================================================
   BUTTON
===================================================== */

send.addEventListener(
    "click",
    sendMessage
);


/* =====================================================
   ENTER
===================================================== */

input.addEventListener(
    "keydown",
    event => {

        if(event.key === "Enter"){

            event.preventDefault();

            sendMessage();
        }
    }
);


/* =====================================================
   MICROPHONE
===================================================== */

const SpeechRecognition =
    window.SpeechRecognition ||
    window.webkitSpeechRecognition;

let recognition = null;
let listening = false;


if(SpeechRecognition){

    recognition =
        new SpeechRecognition();

    recognition.lang = "en-US";
    recognition.continuous = false;
    recognition.interimResults = false;
    recognition.maxAlternatives = 1;


    recognition.onstart = () => {

        listening = true;

        mic.classList.add(
            "listening"
        );

        mic.textContent = "⏹️";

        statusText.textContent =
            combatMode
            ? "COMBAT • LISTENING"
            : "LISTENING...";
    };


    recognition.onresult =
        event => {

            try{

                const result =
                    event.results[0][0];

                if(!result){
                    return;
                }

                const text =
                    result.transcript.trim();

                if(text){

                    input.value = text;

                    stopListening();

                    sendMessage();
                }

            }catch(error){

                console.log(
                    "Speech result error:",
                    error
                );
            }
        };


    recognition.onerror =
        event => {

            stopListening();

            let message =
                "I couldn't access the microphone.";

            if(
                event.error === "not-allowed" ||
                event.error === "service-not-allowed"
            ){

                message =
                    "Microphone access is blocked. Allow microphone access for this website and try again.";
            }

            else if(
                event.error === "no-speech"
            ){

                message =
                    "I didn't hear anything. Tap the microphone and speak again.";
            }

            else if(
                event.error === "audio-capture"
            ){

                message =
                    "I couldn't access an available microphone.";
            }

            else if(
                event.error === "network"
            ){

                message =
                    "The browser's speech-recognition service is unavailable right now.";
            }

            addMessage(
                message,
                "jarvis",
                true
            );
        };


    recognition.onend =
        () => {

            stopListening();
        };


    mic.addEventListener(
        "click",
        () => {

            if(listening){

                try{
                    recognition.stop();
                }catch(error){
                    console.log(error);
                }

                return;
            }

            try{

                recognition.start();

            }catch(error){

                console.log(
                    "Microphone start error:",
                    error
                );
            }
        }
    );

}else{

    mic.addEventListener(
        "click",
        () => {

            addMessage(
                "This browser does not provide speech recognition to this webpage. The text interface is still operational.",
                "jarvis",
                true
            );
        }
    );
}


function stopListening(){

    listening = false;

    mic.classList.remove(
        "listening"
    );

    mic.textContent = "🎙️";

    statusText.textContent =
        combatMode
        ? "COMBAT MODE"
        : "SYSTEMS ONLINE";
}


/* =====================================================
   VOICE LOAD
===================================================== */

if("speechSynthesis" in window){

    speechSynthesis.onvoiceschanged =
        () => {
            speechSynthesis.getVoices();
        };
}


/* =====================================================
   STARTUP
===================================================== */

addMessage(
    memory.name
        ? `Good day, ${memory.name}. J.A.R.V.I.S. systems are online. Contextual conversation engine initialized. How may I assist you?`
        : "Good day. J.A.R.V.I.S. systems are online. Contextual conversation engine initialized. How may I assist you?",
    "jarvis",
    true
);

</script>

</body>
</html>
