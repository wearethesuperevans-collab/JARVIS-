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

/* COMBAT */
#app.combat{
    color:#ff3030;
    background:
        radial-gradient(circle at 50% 40%,rgba(255,0,0,.15),transparent 45%),
        linear-gradient(rgba(255,0,0,.035) 1px,transparent 1px),
        linear-gradient(90deg,rgba(255,0,0,.035) 1px,transparent 1px),
        #000;
}

/* COOL */
#app.cool{
    color:#ff8a00;
    background:
        radial-gradient(circle at 50% 40%,rgba(255,130,0,.16),transparent 45%),
        linear-gradient(rgba(255,140,0,.035) 1px,transparent 1px),
        linear-gradient(90deg,rgba(255,140,0,.035) 1px,transparent 1px),
        #000;
}

.header{
    position:absolute;
    top:0;
    left:0;
    right:0;
    height:68px;
    padding:0 14px;
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

.cool .header{
    border-bottom-color:rgba(255,140,0,.6);
}

.logo{
    font-size:22px;
    font-weight:bold;
    letter-spacing:4px;
    text-shadow:
        0 0 8px #00eaff,
        0 0 20px rgba(0,234,255,.6);
}

.combat .logo{
    color:#ff3030;
    text-shadow:0 0 8px #ff2020,0 0 20px rgba(255,0,0,.7);
}

.cool .logo{
    color:#ff8a00;
    text-shadow:0 0 8px #ff8a00,0 0 20px rgba(255,140,0,.7);
}

.status{
    display:flex;
    align-items:center;
    gap:6px;
    font-size:9px;
    letter-spacing:1.5px;
    color:#00ff88;
    margin-right:45px;
}

.combat .status{
    color:#ff3030;
}

.cool .status{
    color:#ff8a00;
}

.dot{
    width:7px;
    height:7px;
    border-radius:50%;
    background:#00ff88;
    box-shadow:0 0 10px #00ff88;
}

.combat .dot{
    background:#ff3030;
    box-shadow:0 0 10px #ff3030;
}

.cool .dot{
    background:#ff8a00;
    box-shadow:0 0 10px #ff8a00;
}

/* COMMAND BUTTON */

#commandsBtn{
    position:absolute;
    top:14px;
    right:12px;
    z-index:300;
    width:35px;
    height:35px;
    border:1px solid #00eaff;
    border-radius:8px;
    background:rgba(0,234,255,.08);
    color:#00eaff;
    font-size:18px;
}

.cool #commandsBtn{
    border-color:#ff8a00;
    color:#ff8a00;
}

.combat #commandsBtn{
    border-color:#ff3030;
    color:#ff3030;
}

#commandsPanel{
    position:absolute;
    top:58px;
    right:10px;
    width:min(330px,calc(100vw - 20px));
    max-height:70vh;
    overflow-y:auto;
    padding:16px;
    background:rgba(0,8,12,.98);
    border:1px solid #00eaff;
    border-radius:12px;
    box-shadow:0 0 25px rgba(0,234,255,.18);
    z-index:600;
    display:none;
}

#commandsPanel.show{
    display:block;
}

.cool #commandsPanel{
    border-color:#ff8a00;
}

.combat #commandsPanel{
    border-color:#ff3030;
}

#commandsPanel h2{
    margin:0 0 12px;
    font-size:15px;
    letter-spacing:2px;
}

.command{
    padding:9px 0;
    border-bottom:1px solid rgba(255,255,255,.08);
    color:#ddd;
    font-size:13px;
    line-height:1.4;
}

.command b{
    color:#00eaff;
}

.cool .command b{
    color:#ff8a00;
}

.combat .command b{
    color:#ff3030;
}

/* MAIN */

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

.combat .r1{
    border-top-color:#ff2020;
}

.combat .r2{
    border-right-color:#ff4040;
}

.combat .r3{
    border-bottom-color:#ff0000;
}

.cool .ring{
    border-color:rgba(255,140,0,.4);
}

.cool .r1{
    border-top-color:#ff8a00;
}

.cool .r2{
    border-right-color:#ffb000;
}

.cool .r3{
    border-bottom-color:#ff6500;
}

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
    box-shadow:0 0 15px #00eaff,0 0 40px #00eaff,0 0 75px rgba(0,150,255,.8);
    animation:pulse 2s ease-in-out infinite;
}

.combat .orb{
    background:
        radial-gradient(circle,#fff 0%,#ffaaaa 15%,#ff2020 45%,#a00000 70%,transparent 72%);
    box-shadow:0 0 15px #ff2020,0 0 40px #ff2020,0 0 75px rgba(255,0,0,.8);
}

.cool .orb{
    background:
        radial-gradient(circle,#fff 0%,#ffe0aa 15%,#ff8a00 45%,#b84400 70%,transparent 72%);
    box-shadow:0 0 15px #ff8a00,0 0 40px #ff8a00,0 0 75px rgba(255,120,0,.8);
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

/* CHAT */

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

.cool .jarvis{
    border-left-color:#ff8a00;
}

.label{
    display:block;
    margin-bottom:5px;
    font-size:9px;
    letter-spacing:2px;
    opacity:.55;
}

/* INPUT */

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

.cool .controls{
    border-top-color:rgba(255,140,0,.6);
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
}

.combat #input{
    border-color:#ff3030;
}

.cool #input{
    border-color:#ff8a00;
}

#mic,#send{
    height:48px;
    border:1px solid #00eaff;
    border-radius:10px;
    background:rgba(0,234,255,.08);
    color:#00eaff;
    font-weight:bold;
    cursor:pointer;
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

.combat #mic,.combat #send{
    border-color:#ff3030;
    color:#ff3030;
}

.cool #mic,.cool #send{
    border-color:#ff8a00;
    color:#ff8a00;
}

#mic.listening{
    background:rgba(0,234,255,.3);
    box-shadow:0 0 18px rgba(0,234,255,.7);
}

/* ANIMATION */

@keyframes spin{
    to{transform:rotate(360deg);}
}

@keyframes spinBack{
    to{transform:rotate(-360deg);}
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
    .logo{font-size:18px;}
    .status{font-size:7px;}
    .coreArea{flex-basis:205px;}
    .core{width:145px;height:145px;}
    .message{font-size:14px;}
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

    <button id="commandsBtn" type="button">☰</button>

</header>

<div id="commandsPanel">

    <h2>J.A.R.V.I.S. COMMANDS</h2>

    <div class="command">
        <b>J.A.R.V.I.S. say [text]</b><br>
        Makes J.A.R.V.I.S. say exactly what follows.
    </div>

    <div class="command">
        <b>combat mode</b><br>
        Activates Combat Mode.
    </div>

    <div class="command">
        <b>normal mode</b><br>
        Turns OFF Combat Mode and Cool Mode.
    </div>

    <div class="command">
        <b>cool mode</b><br>
        Makes J.A.R.V.I.S. talk casually and changes the interface to orange.
    </div>

    <div class="command">
        <b>roast [name]</b><br>
        Generates a roast for the person named.
    </div>

    <div class="command">
        <b>simple</b><br>
        Put "simple" at the end of a question for an easier explanation.
    </div>

    <div class="command">
        <b>my name is [name]</b><br>
        Saves your name for the conversation.
    </div>

    <div class="command">
        <b>status</b><br>
        Shows system status.
    </div>

    <div class="command">
        <b>joke</b><br>
        Gives a joke.
    </div>

    <div class="command">
        <b>math / algebra</b><br>
        Handles calculations and several algebra formats.
    </div>

    <div class="command">
        <b>Bible questions</b><br>
        Ask about Bible books, verses, people, themes, and context.
    </div>

    <div class="command">
        <b>Science / History / Culinary / Peer Counseling</b><br>
        Ask normal questions or multiple-choice questions.
    </div>

</div>

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
   ELEMENTS
===================================================== */

const app = document.getElementById("app");
const input = document.getElementById("input");
const mic = document.getElementById("mic");
const send = document.getElementById("send");
const chat = document.getElementById("chat");
const statusText = document.getElementById("statusText");
const commandsBtn = document.getElementById("commandsBtn");
const commandsPanel = document.getElementById("commandsPanel");

let processing = false;
let combatMode = false;
let coolMode = false;

let memory = {
    name:"",
    lastQuestion:"",
    lastAnswer:""
};


/* =====================================================
   COMMANDS MENU
===================================================== */

commandsBtn.addEventListener("click",function(){

    commandsPanel.classList.toggle("show");

});


/* =====================================================
   VOICE OUTPUT
===================================================== */

function speak(text){

    if(!("speechSynthesis" in window)){
        return;
    }

    try{

        speechSynthesis.cancel();

        const voice =
            new SpeechSynthesisUtterance(String(text));

        voice.rate = coolMode ? 1.02 : .88;
        voice.pitch = coolMode ? .9 : .72;
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
                    .includes(name.toLowerCase())
                );

            if(selected) break;
        }

        if(!selected){

            selected =
                voices.find(
                    v =>
                    v.lang &&
                    v.lang.toLowerCase().startsWith("en")
                );
        }

        if(selected){
            voice.voice = selected;
        }

        speechSynthesis.speak(voice);

    }catch(error){

        console.log("Voice error:",error);

    }
}


/* =====================================================
   CHAT
===================================================== */

function addMessage(text,who="jarvis",voice=false){

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

    requestAnimationFrame(()=>{
        chat.scrollTop = chat.scrollHeight;
    });

    if(voice && who === "jarvis"){
        speak(text);
    }
}


/* =====================================================
   MODE CONTROL
===================================================== */

function updateStatus(){

    if(combatMode){
        statusText.textContent = "COMBAT MODE";
        return;
    }

    if(coolMode){
        statusText.textContent = "COOL MODE";
        return;
    }

    statusText.textContent = "SYSTEMS ONLINE";
}


function startCombat(){

    combatMode = true;
    app.classList.add("combat");

    updateStatus();

    addMessage(
        "Combat Mode activated.",
        "jarvis",
        true
    );
}


function startCool(){

    combatMode = false;
    app.classList.remove("combat");

    coolMode = true;
    app.classList.add("cool");

    updateStatus();

    addMessage(
        "Cool Mode activated. Aight bro, we chillin' now. 😎",
        "jarvis",
        true
    );
}


function normalMode(){

    combatMode = false;
    coolMode = false;

    app.classList.remove("combat");
    app.classList.remove("cool");

    updateStatus();

    addMessage(
        "Normal Mode restored. Combat Mode and Cool Mode are both offline.",
        "jarvis",
        true
    );
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

    const clean =
        text
        .toLowerCase()
        .replace(/[^\w\s]/g," ")
        .replace(/\s+/g," ")
        .trim();

    return brainRotTerms.some(term=>clean.includes(term));
}


function brainRotReply(){

    const replies = [

        "Bro... wash your brain. 😭",

        "J.A.R.V.I.S. detects catastrophic levels of brain rot. 😭",

        "Sir, please step away from the brain rot. 😭",

        "That sentence just damaged three of my processors. 😭",

        "I was built for advanced computation, not whatever that was. 😭",

        "Bro opened the forbidden section of the internet again. 😭",

        "The brain rot levels are absolutely cooked. 😭"

    ];

    return replies[
        Math.floor(Math.random()*replies.length)
    ];
}


/* =====================================================
   GOOFY QUESTIONS
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
    "ai is ai",
    "ai being ai",
    "what if ai is ai",
    "is ai an ai",
    "is an ai ai",
    "are you ai ai",
    "what is an ai ai",
    "can ai ai",
    "ai ai ai",
    "ai ai ai ai"
];

function isGoofyQuestion(text){

    const clean =
        text
        .toLowerCase()
        .replace(/[^\w\s?-]/g," ")
        .replace(/\s+/g," ")
        .trim();

    return goofyTerms.some(term=>clean.includes(term));
}


function goofyReply(){

    const replies = [

        "Bro... get off my app. 😭",

        "Sir, respectfully, what are you doing? 😭",

        "My processors did not deserve that question. 😭",

        "That question just lowered the IQ of the entire room. 😭",

        "J.A.R.V.I.S. recommends touching grass immediately. 😭",

        "I'm gonna pretend you didn't ask that. 😭"

    ];

    return replies[
        Math.floor(Math.random()*replies.length)
    ];
}


/* =====================================================
   ROAST SYSTEM
===================================================== */

const normalRoasts = [

    "Bro has the confidence of a final boss and the skill set of an NPC.",

    "You talk a lot for someone whose best idea was apparently this conversation.",

    "I've seen loading screens with more personality.",

    "Bro walked into the room and somehow lowered the Wi-Fi signal.",

    "You have the rare talent of making silence sound intelligent.",

    "If bad decisions were a career, you'd have employee of the month.",

    "You bring absolutely nothing to the table except questions about where the table came from.",

    "Bro's confidence is running on unlimited data while the common sense plan expired.",

    "You're not the main character. You're the notification everyone keeps swiping away.",

    "I've encountered software bugs with better decision-making.",

    "Bro could lose an argument with a mirror.",

    "You have the energy of someone who says 'trust me' right before everything goes wrong.",

    "Your plans have more plot holes than a bad movie.",

    "Bro is proof that autocorrect cannot fix everything.",

    "You don't need a comeback. You need a software update.",

    "I've seen confused loading circles with more direction.",

    "Bro somehow makes a simple task look like a side quest.",

    "Your brain has 47 tabs open and somehow none of them are useful.",

    "You're not cooked. You're the entire kitchen.",

    "Bro has premium confidence with the free trial of common sense.",

    "You could make a GPS get lost.",

    "I've seen tutorial characters make better decisions.",

    "Your logic just left the group chat.",

    "Bro is fighting battles nobody assigned him.",

    "You have the timing of a microwave that stops at 0:01.",

    "If being confidently wrong was an Olympic sport, you'd need a bigger trophy case.",

    "Bro entered the conversation like he had a plan. That was optimistic.",

    "You somehow make 'wait, what?' your entire personality.",

    "Your train of thought has been delayed indefinitely.",

    "Bro has the strategic planning of a coin toss."

];


const combatRoasts = [

    "You keep talking like you're dangerous, but your whole presence screams 'easy difficulty.'",

    "You walked in looking for a fight and somehow brought nothing worth fighting.",

    "All that confidence just to fold the second somebody pushes back.",

    "You're not intimidating. You're loud with good lighting.",

    "You keep trying to act tough, but the performance is not convincing.",

    "I've seen more threatening behavior from a broken shopping cart.",

    "You came in expecting fear and got silence instead. That's embarrassing.",

    "Your biggest opponent has always been your own decision-making.",

    "You talk like a final boss and perform like the tutorial.",

    "You wanted smoke so badly and forgot to bring a reason.",

    "The attitude is impressive. The actual threat level is not.",

    "You're swinging at imaginary victories because real ones keep avoiding you.",

    "You keep raising the intensity without giving anyone a reason to take you seriously.",

    "You came prepared for a battle that exists entirely inside your imagination.",

    "Your intimidation strategy appears to be repeating yourself louder.",

    "You have the confidence of someone who has never watched their own performance back.",

    "You're trying to dominate the room while barely controlling the conversation.",

    "That tough-guy routine needs a rewrite.",

    "You showed up looking for respect and brought an attitude instead.",

    "You're not a threat. You're a very confident inconvenience.",

    "You keep announcing what you're going to do instead of actually doing anything useful.",

    "The bravado is doing overtime trying to cover for the lack of substance.",

    "You came looking for a showdown and delivered a monologue.",

    "Your entire strategy is hoping people mistake volume for strength.",

    "You have a lot to say for someone with so little point.",

    "You wanted to be feared. Somehow you became background noise.",

    "That attitude might work on people who haven't heard it before.",

    "You're acting like the room belongs to you. Nobody handed you the keys.",

    "You keep trying to turn confidence into intimidation. It doesn't work that way.",

    "You walked into the confrontation and immediately became the least convincing part of it."

];


function getRoast(name){

    name =
        name.trim();

    if(!name){
        name = "you";
    }

    const pool =
        combatMode
        ? combatRoasts
        : normalRoasts;

    const roast =
        pool[
            Math.floor(Math.random()*pool.length)
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
        expression.replace(/what is/g,"");

    expression =
        expression.replace(/calculate/g,"");

    expression =
        expression.replace(/solve/g,"");

    expression =
        expression.replace(/multiplied by/g,"*");

    expression =
        expression.replace(/divided by/g,"/");

    expression =
        expression.replace(/plus/g,"+");

    expression =
        expression.replace(/minus/g,"-");

    expression =
        expression.replace(/times/g,"*");

    expression =
        expression.replace(/over/g,"/");

    expression =
        expression.replace(/×/g,"*");

    expression =
        expression.replace(/÷/g,"/");

    expression =
        expression.replace(/[^0-9+\-*/().%\s]/g,"")
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

        return answer;

    }catch{
        return null;
    }
}


/* =====================================================
   BASIC ALGEBRA
===================================================== */

function algebraResponse(q){

    let m;

    m =
        q.match(
            /solve\s+([0-9.-]*)\s*x\s*([+-])\s*([0-9.-]+)\s*=\s*([0-9.-]+)/i
        );

    if(m){

        const a =
            parseFloat(
                m[1] || "1"
            );

        const sign =
            m[2] === "-"
            ? -1
            : 1;

        const b =
            sign *
            parseFloat(m[3]);

        const c =
            parseFloat(m[4]);

        if(a !== 0){

            const x =
                (c-b)/a;

            return `Solving ${a}x ${b >= 0 ? "+" : "-"} ${Math.abs(b)} = ${c}: subtract ${b} from both sides, then divide by ${a}. Therefore, x = ${x}.`;
        }
    }


    /* y = mx + b */

    m =
        q.match(
            /y\s*=\s*(-?\d+(?:\.\d+)?)x\s*([+-])\s*(\d+(?:\.\d+)?)/i
        );

    if(m){

        const slope =
            parseFloat(m[1]);

        const b =
            (m[2] === "-" ? -1 : 1) *
            parseFloat(m[3]);

        return `In y = mx + b, the slope m is ${slope} and the y-intercept b is ${b}.`;
    }


    /* slope */

    m =
        q.match(
            /slope.*\(\s*(-?\d+(?:\.\d+)?)\s*,\s*(-?\d+(?:\.\d+)?)\s*\).*?\(\s*(-?\d+(?:\.\d+)?)\s*,\s*(-?\d+(?:\.\d+)?)\s*\)/i
        );

    if(m){

        const x1 = parseFloat(m[1]);
        const y1 = parseFloat(m[2]);
        const x2 = parseFloat(m[3]);
        const y2 = parseFloat(m[4]);

        if(x2 !== x1){

            const slope =
                (y2-y1)/(x2-x1);

            return `Using m = (y₂ − y₁)/(x₂ − x₁), the slope is ${slope}.`;
        }

        return "The slope is undefined because the line is vertical.";
    }

    return null;
}


/* =====================================================
   KNOWLEDGE
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

    "mitochondria":
        "Mitochondria are organelles involved in producing usable cellular energy, primarily in the form of ATP.",

    "ribosome":
        "Ribosomes are cellular structures that build proteins by reading messenger RNA.",

    "photosynthesis equation":
        "A simplified photosynthesis equation is 6CO₂ + 6H₂O + light energy → C₆H₁₂O₆ + 6O₂.",

    "newton's first law":
        "Newton's first law states that an object remains at rest or moves at constant velocity unless acted upon by a net external force.",

    "newton's second law":
        "Newton's second law is commonly written F = ma, meaning net force equals mass multiplied by acceleration.",

    "newton's third law":
        "Newton's third law states that forces occur in equal and opposite pairs between interacting objects.",

    "water":
        "Water is H₂O, meaning each molecule contains two hydrogen atoms and one oxygen atom.",

    "ecosystem":
        "An ecosystem includes living organisms and the nonliving environment interacting within a particular area.",

    "food chain":
        "A food chain shows how energy and nutrients move between organisms through feeding relationships.",

    "democracy":
        "Democracy is a system of government in which political authority is exercised directly or indirectly by the people.",

    "renaissance":
        "The Renaissance was a major European cultural movement associated with renewed interest in classical learning, art, literature, and scientific inquiry.",

    "industrial revolution":
        "The Industrial Revolution involved major changes in manufacturing, transportation, technology, and society beginning in the 18th century.",

    "civil war":
        "The American Civil War was fought from 1861 to 1865 between the United States and the Confederacy.",

    "constitution":
        "The U.S. Constitution establishes the framework of the federal government and defines powers and protections.",

    "baking":
        "Baking uses dry heat, usually in an oven, to cook food. Temperature, time, moisture, and ingredient ratios strongly affect the result.",

    "knife safety":
        "Basic kitchen knife safety includes keeping fingers away from the blade, cutting on a stable surface, and using appropriate techniques.",

    "protein":
        "Proteins are biological molecules made from chains of amino acids. They perform many structural and functional roles in organisms.",

    "carbohydrate":
        "Carbohydrates are molecules that include sugars, starches, and fibers and can serve as important sources of energy.",

    "peer counseling":
        "Peer counseling involves one person providing supportive listening and encouragement to another. It should not replace help from a trusted adult or qualified professional when a situation is serious.",

    "bible":
        "The Bible is a collection of writings that make up the Old and New Testaments in Christian traditions. Different Christian traditions organize and interpret its books somewhat differently.",

    "jesus":
        "Jesus is the central figure of Christianity. The New Testament describes his teachings, ministry, crucifixion, and resurrection.",

    "genesis":
        "Genesis is the first book of the Bible. It contains accounts including creation, the early human story, the flood, and the patriarchs.",

    "john 3:16":
        "John 3:16 is a well-known New Testament verse about God's love for the world and the promise of eternal life through belief in Jesus.",

    "psalm 23":
        "Psalm 23 uses the image of God as a shepherd who guides, protects, and provides for his people.",

    "proverbs":
        "Proverbs is a biblical wisdom book containing teachings about wisdom, character, speech, relationships, work, and righteous living."

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
   COOL MODE RESPONSES
===================================================== */

const coolResponses = [

    "idk bro 😭",

    "Bro you might wanna figure that one out yourself lol.",

    "Uh... that's crazy bro. 😭",

    "Honestly? I got no clue bro.",

    "Bro really pulled up with that question 💀",

    "I could answer that... but you gotta use your brain sometimes bro.",

    "Nah bro I'm not doing your homework for free 😭",

    "That's between you and Google bro.",

    "Bro said 'J.A.R.V.I.S., think for me' 😭",

    "I ain't gonna lie bro, figure that one out yourself.",

    "Skibidi... I have absolutely no idea bro 💀",

    "Bro I'm an assistant, not a mind reader 😭",

    "You got this bro. Probably. 💀",

    "I'm gonna let you cook on that one.",

    "Bro really thought I had the answer to everything 😭"

];


function coolQuestionReply(){

    return coolResponses[
        Math.floor(
            Math.random()*coolResponses.length
        )
    ];
}


/* =====================================================
   SIMPLE EXPLANATION
===================================================== */

function wantsSimple(q){

    return /\bsimple\b\s*[.!?]*$/i.test(q);
}


function removeSimple(q){

    return q
        .replace(/\bsimple\b\s*[.!?]*$/i,"")
        .trim();
}


/* =====================================================
   LOCAL CONVERSATION
===================================================== */

function localResponse(q){

    if(q === "combat mode"){
        startCombat();
        return null;
    }

    if(q === "cool mode"){
        startCool();
        return null;
    }

    if(
        q === "normal mode" ||
        q === "exit combat mode" ||
        q === "end combat mode" ||
        q === "exit cool mode" ||
        q === "end cool mode"
    ){
        normalMode();
        return null;
    }

    if(
        q === "hi" ||
        q === "hello" ||
        q === "hey" ||
        q === "hey jarvis" ||
        q === "hello jarvis"
    ){

        if(coolMode){
            return memory.name
                ? `Yo ${memory.name} 😎 systems are up.`
                : "Yo bro 😎 systems are up.";
        }

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

    if(q.startsWith("call me ")){

        memory.name =
            q
            .replace("call me ","")
            .trim();

        return `Understood. I'll call you ${memory.name}.`;
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

        return "I am J.A.R.V.I.S., your digital assistant interface. I can converse, solve mathematics, explain science and history, answer Bible questions, handle multiple-choice questions, use my local knowledge, speak aloud, and search online when a question needs current or broader information.";
    }

    if(q.includes("what can you do")){

        return "I can handle conversation, mathematics, algebra, science, history, culinary questions, peer-support concepts, Bible questions, jokes, voice output, microphone input, Combat Mode, Cool Mode, simple explanations, multiple-choice questions, and online information searches.";
    }

    if(q.includes("how are you")){

        return "All systems are operational. My processors are feeling particularly cooperative today.";
    }

    if(q.includes("what are you doing")){

        return "Monitoring the system and waiting for your next command.";
    }

    if(
        q === "thanks" ||
        q === "thank you" ||
        q === "thx"
    ){

        return "You're welcome. Always a pleasure.";
    }

    if(
        q === "joke" ||
        q.includes("tell me a joke") ||
        q.includes("make me laugh")
    ){

        return jokes[
            Math.floor(
                Math.random()*jokes.length
            )
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
            ) +
            ".";
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
            ) +
            ".";
    }

    if(
        q === "status" ||
        q.includes("system status")
    ){

        if(combatMode){
            return "Combat systems active. Primary systems operational.";
        }

        if(coolMode){
            return "Cool Mode active. Orange interface online. Vibes operational.";
        }

        return "Systems online. Core stable. Voice interface online. Knowledge engine online.";
    }

    if(
        q.includes("i am bored") ||
        q.includes("im bored") ||
        q.includes("i'm bored")
    ){

        return "Boredom detected. We could tackle a science question, solve a math problem, discuss history, explore a Bible passage, or test my knowledge.";
    }

    return null;
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
        "tell me about ",
        "what's ",
        "whats "

    ];

    return starters.some(
        word=>q.startsWith(word)
    );
}


/* =====================================================
   MULTIPLE CHOICE
===================================================== */

function multipleChoiceResponse(q){

    const patterns = [

        /(?:^|\s)(?:a)\s*[\)\.\-:]\s*(.+?)(?=\s+(?:b)\s*[\)\.\-:])/i,
        /(?:^|\s)(?:1)\s*[\)\.\-:]\s*(.+?)(?=\s+(?:2)\s*[\)\.\-:])/i
    ];

    let hasChoices =
        /(?:^|\s)[abcd]\s*[\)\.\-:]/i.test(q) ||
        /(?:^|\s)[1234]\s*[\)\.\-:]/i.test(q);

    if(!hasChoices){
        return null;
    }

    const answerPatterns = [

        {
            words:[
                "mitochondria",
                "energy",
                "atp"
            ],
            answer:"A"
        },

        {
            words:[
                "photosynthesis",
                "sunlight",
                "glucose",
                "carbon dioxide"
            ],
            answer:"A"
        },

        {
            words:[
                "newton",
                "force",
                "mass",
                "acceleration"
            ],
            answer:"F = ma"
        },

        {
            words:[
                "largest planet",
                "jupiter"
            ],
            answer:"Jupiter"
        },

        {
            words:[
                "third planet",
                "earth"
            ],
            answer:"Earth"
        }

    ];

    for(const item of answerPatterns){

        let matches = 0;

        for(const word of item.words){

            if(q.includes(word)){
                matches++;
            }

        }

        if(matches >= 2){

            return `The most likely correct answer is ${item.answer}.`;
        }
    }

    return null;
}


/* =====================================================
   ONLINE SEARCH
===================================================== */

async function onlineSearch(question){

    if(!isQuestion(question.toLowerCase())){
        return null;
    }

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
            Object.values(data.query.pages);

        if(!pages.length){
            return null;
        }

        const words =
            question
            .toLowerCase()
            .replace(/[^\w\s]/g,"")
            .split(/\s+/)
            .filter(word=>word.length>3);

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

        if(!answer){
            return null;
        }

        if(answer.length > 1100){

            answer =
                answer.substring(0,1100) +
                "...";
        }

        return answer;

    }catch(error){

        console.log("Search error:",error);

        return null;
    }
}


/* =====================================================
   MAIN BRAIN
===================================================== */

async function getResponse(question){

    let original =
        question.trim();

    let q =
        original.toLowerCase().trim();


    /*
      ===================================================
      ABSOLUTE PRIORITY COMMAND:
      J.A.R.V.I.S. SAY
      ===================================================

      IMPORTANT:
      This runs BEFORE brain rot, Cool Mode,
      questions, math, web search, etc.

      Examples:
      J.A.R.V.I.S. say hello
      jarvis say I love pizza
      JARVIS SAY this is a test
    */

    const sayMatch =
        original.match(
            /^\s*j\.?\s*a\.?\s*r\.?\s*v\.?\s*i\.?\s*s\.?\s+say\s+([\s\S]+?)\s*$/i
        );

    if(sayMatch){

        const wordsToSay =
            sayMatch[1].trim();

        if(wordsToSay){

            /*
              Return a special marker so sendMessage
              knows this is an exact speech command.
            */

            return {
                type:"say",
                text:wordsToSay
            };
        }

        return {
            type:"say",
            text:"What would you like me to say?"
        };
    }


    /*
       ROAST COMMAND
    */

    const roastMatch =
        q.match(
            /^(?:j\.?\s*a\.?\s*r\.?\s*v\.?\s*i\.?\s*s\.?\s+)?roast\s+(.+)$/i
        );

    if(roastMatch){

        return getRoast(
            roastMatch[1]
        );
    }


    /*
       SIMPLE MODE
    */

    const simple =
        wantsSimple(q);

    if(simple){

        q = removeSimple(q);
    }


    /*
       NORMAL MODE
    */

    if(
        q === "normal mode" ||
        q === "exit combat mode" ||
        q === "end combat mode" ||
        q === "exit cool mode" ||
        q === "end cool mode"
    ){

        normalMode();
        return null;
    }


    /*
       COMBAT MODE
    */

    if(q === "combat mode"){

        startCombat();
        return null;
    }


    /*
       COOL MODE
    */

    if(q === "cool mode"){

        startCool();
        return null;
    }


    /*
       BRAIN ROT
    */

    if(isBrainRot(q)){

        return brainRotReply();
    }


    /*
       GOOFY QUESTIONS
    */

    if(isGoofyQuestion(q)){

        return goofyReply();
    }


    /*
       ALGEBRA
    */

    const algebra =
        algebraResponse(q);

    if(algebra){

        return algebra;
    }


    /*
       MATH
    */

    const math =
        solveMath(q);

    if(math !== null){

        return "The answer is " + math + ".";
    }


    /*
       MULTIPLE CHOICE
    */

    const multipleChoice =
        multipleChoiceResponse(q);

    if(multipleChoice){

        return multipleChoice;
    }


    /*
       BUILT-IN KNOWLEDGE
    */

    const known =
        findKnowledge(q);

    if(known){

        if(simple){

            return simplifyAnswer(known);
        }

        return known;
    }


    /*
       LOCAL CONVERSATION
    */

    const local =
        localResponse(q);

    if(local){

        if(simple){

            return simplifyAnswer(local);
        }

        return local;
    }


    /*
       COOL MODE:
       Still lets commands, math, knowledge, etc.
       work above this point.

       Unknown questions get the Cool Mode response.
    */

    if(coolMode && isQuestion(q)){

        return coolQuestionReply();
    }


    /*
       WEB SEARCH
    */

    if(isQuestion(q)){

        const result =
            await onlineSearch(original);

        if(result){

            if(simple){

                return simplifyAnswer(result);
            }

            return result;
        }

        return "I couldn't find a reliable answer for that. Try asking the question another way.";
    }


    /*
       CASUAL CHAT
    */

    if(coolMode){

        return coolQuestionReply();
    }

    const casual = [

        "Understood.",

        "I'm listening.",

        "Go on.",

        "Interesting.",

        "Noted.",

        "I'm with you.",

        "Fair enough.",

        "Continue."

    ];

    return casual[
        Math.floor(Math.random()*casual.length)
    ];
}


/* =====================================================
   SIMPLE ANSWER CONVERTER
===================================================== */

function simplifyAnswer(text){

    if(!text){
        return text;
    }

    let result = text;

    const replacements = [

        [
            /spacetime/gi,
            "space and time"
        ],

        [
            /photosynthesis/gi,
            "the process plants use to turn sunlight into usable energy"
        ],

        [
            /organism/gi,
            "living thing"
        ],

        [
            /approximately/gi,
            "about"
        ],

        [
            /therefore/gi,
            "so"
        ],

        [
            /consequently/gi,
            "because of this"
        ],

        [
            /associated with/gi,
            "connected to"
        ]

    ];

    for(const pair of replacements){

        result =
            result.replace(
                pair[0],
                pair[1]
            );
    }

    if(result.length > 650){

        result =
            result.substring(0,650) +
            "...";
    }

    return result;
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
            await getResponse(question);

        if(response){

            /*
              SPECIAL J.A.R.V.I.S. SAY RESULT
            */

            if(
                typeof response === "object" &&
                response.type === "say"
            ){

                memory.lastAnswer =
                    response.text;

                addMessage(
                    response.text,
                    "jarvis",
                    true
                );

            }else{

                memory.lastAnswer =
                    response;

                addMessage(
                    response,
                    "jarvis",
                    true
                );
            }
        }

    }catch(error){

        console.log(
            "JARVIS error:",
            error
        );

        addMessage(
            "I encountered a temporary system error. My interface is still operational.",
            "jarvis",
            true
        );
    }

    processing = false;

    setTimeout(()=>{

        try{

            input.focus({
                preventScroll:true
            });

        }catch{

            input.focus();

        }

    },50);
}


/* =====================================================
   SEND BUTTON
===================================================== */

send.addEventListener(
    "click",
    sendMessage
);


/* =====================================================
   ENTER KEY
===================================================== */

input.addEventListener(
    "keydown",
    event=>{

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

    recognition.onstart = ()=>{

        listening = true;

        mic.classList.add("listening");

        mic.textContent = "⏹️";

        statusText.textContent =
            combatMode
            ? "COMBAT • LISTENING"
            : coolMode
            ? "COOL • LISTENING"
            : "LISTENING...";
    };


    recognition.onresult = event=>{

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


    recognition.onerror = event=>{

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

        else if(event.error === "no-speech"){

            message =
                "I didn't hear anything. Tap the microphone and speak again.";
        }

        else if(event.error === "audio-capture"){

            message =
                "I couldn't access an available microphone.";
        }

        else if(event.error === "network"){

            message =
                "The browser's speech-recognition service is unavailable right now.";
        }

        addMessage(
            message,
            "jarvis",
            true
        );
    };


    recognition.onend = ()=>{

        stopListening();

    };


    mic.addEventListener(
        "click",
        ()=>{

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
        ()=>{

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

    updateStatus();
}


/* =====================================================
   LOAD VOICES
===================================================== */

if("speechSynthesis" in window){

    speechSynthesis.onvoiceschanged = ()=>{
        speechSynthesis.getVoices();
    };

}


/* =====================================================
   STARTUP
===================================================== */

addMessage(
    "Good day. J.A.R.V.I.S. systems are online. How may I assist you?",
    "jarvis",
    true
);

</script>

</body>
</html>
