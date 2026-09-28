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

button,input{
    font-family:inherit;
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

/* HEADER */

.header{
    position:absolute;
    top:0;
    left:0;
    right:0;

    height:68px;

    padding:0 12px 0 20px;
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

    text-shadow:
        0 0 8px #00eaff,
        0 0 20px rgba(0,234,255,.6);
}

.combat .logo{
    color:#ff3030;

    text-shadow:
        0 0 8px #ff2020,
        0 0 20px rgba(255,0,0,.7);
}

.headerRight{
    display:flex;
    align-items:center;
    gap:10px;
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

/* COMMAND BUTTON */

#commandsButton{
    height:36px;
    padding:0 11px;

    border:1px solid #00eaff;
    border-radius:8px;

    background:rgba(0,234,255,.08);
    color:#00eaff;

    font-size:10px;
    font-weight:bold;
    letter-spacing:1px;

    cursor:pointer;
    touch-action:manipulation;
}

#commandsButton:active{
    transform:scale(.96);
}

.combat #commandsButton{
    border-color:#ff3030;
    color:#ff3030;
}

/* COMMANDS PANEL */

#commandsPanel{
    position:absolute;
    top:73px;
    right:10px;

    width:min(360px,calc(100vw - 20px));
    max-height:calc(100vh - 100px);

    overflow-y:auto;

    padding:16px;

    background:rgba(0,10,15,.98);

    border:1px solid rgba(0,234,255,.5);
    border-radius:12px;

    box-shadow:
        0 0 30px rgba(0,180,255,.18);

    z-index:1000;

    display:none;
}

#commandsPanel.open{
    display:block;
}

.combat #commandsPanel{
    border-color:rgba(255,40,40,.6);
    box-shadow:
        0 0 30px rgba(255,0,0,.18);
}

.commandsTitle{
    font-size:15px;
    font-weight:bold;
    letter-spacing:2px;
    margin-bottom:12px;
}

.commandGroup{
    margin-bottom:14px;
}

.commandGroup h3{
    margin:0 0 6px;
    font-size:10px;
    letter-spacing:2px;
    color:#00ff88;
}

.combat .commandGroup h3{
    color:#ff5050;
}

.commandItem{
    padding:7px 8px;
    margin-bottom:4px;

    background:rgba(0,234,255,.035);
    border-radius:6px;

    color:#d8fbff;
    font-size:12px;
    line-height:1.35;
}

.commandItem b{
    color:#00eaff;
}

.combat .commandItem b{
    color:#ff4040;
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

/* ARC */

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

    background:
        radial-gradient(
            circle,
            #fff 0%,
            #aaffff 15%,
            #00eaff 45%,
            #007cff 70%,
            transparent 72%
        );

    box-shadow:
        0 0 15px #00eaff,
        0 0 40px #00eaff,
        0 0 75px rgba(0,150,255,.8);

    animation:pulse 2s ease-in-out infinite;
}

.combat .orb{
    background:
        radial-gradient(
            circle,
            #fff 0%,
            #ffaaaa 15%,
            #ff2020 45%,
            #a00000 70%,
            transparent 72%
        );

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

    box-shadow:
        0 0 12px rgba(0,234,255,.2);
}

.combat #input{
    border-color:#ff3030;
}

#mic,
#send{
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

    box-shadow:
        0 0 18px rgba(0,234,255,.7);
}

.combat #mic.listening{
    background:rgba(255,0,0,.3);

    box-shadow:
        0 0 18px rgba(255,0,0,.7);
}

/* ANIMATION */

@keyframes spin{
    to{
        transform:rotate(360deg);
    }
}

@keyframes spinBack{
    to{
        transform:rotate(-360deg);
    }
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

    .logo{
        font-size:18px;
    }

    .status{
        display:none;
    }

    .coreArea{
        flex-basis:205px;
    }

    .core{
        width:145px;
        height:145px;
    }

    .message{
        font-size:14px;
    }

    #commandsButton{
        font-size:9px;
    }
}
</style>
</head>

<body>

<div id="app">

<header class="header">

    <div class="logo">
        J.A.R.V.I.S.
    </div>

    <div class="headerRight">

        <div class="status">
            <span class="dot"></span>
            <span id="statusText">SYSTEMS ONLINE</span>
        </div>

        <button id="commandsButton">
            COMMANDS
        </button>

    </div>

</header>


<!-- COMMANDS PANEL -->

<div id="commandsPanel">

    <div class="commandsTitle">
        J.A.R.V.I.S. COMMAND DATABASE
    </div>

    <div class="commandGroup">

        <h3>VOICE / SPEECH</h3>

        <div class="commandItem">
            <b>J.A.R.V.I.S. say hello</b><br>
            Makes J.A.R.V.I.S. say exactly what follows "say".
        </div>

        <div class="commandItem">
            <b>J.A.R.V.I.S. say I am ready</b><br>
            Speaks the requested sentence aloud.
        </div>

    </div>


    <div class="commandGroup">

        <h3>MODES</h3>

        <div class="commandItem">
            <b>combat mode</b><br>
            Activates Combat Mode.
        </div>

        <div class="commandItem">
            <b>normal mode</b><br>
            Returns J.A.R.V.I.S. to normal mode.
        </div>

        <div class="commandItem">
            <b>status</b><br>
            Reports system status.
        </div>

    </div>


    <div class="commandGroup">

        <h3>ROAST SYSTEM</h3>

        <div class="commandItem">
            <b>roast Gabriel</b><br>
            Generates a random roast aimed at the supplied name.
        </div>

        <div class="commandItem">
            <b>combat mode + roast Gabriel</b><br>
            Uses the more aggressive Combat Mode roast set.
        </div>

    </div>


    <div class="commandGroup">

        <h3>PERSONAL</h3>

        <div class="commandItem">
            <b>my name is Gabriel</b><br>
            Saves the name for the current session.
        </div>

        <div class="commandItem">
            <b>call me Gabriel</b><br>
            Sets the name J.A.R.V.I.S. uses.
        </div>

        <div class="commandItem">
            <b>what is my name</b><br>
            Recalls the saved name.
        </div>

    </div>


    <div class="commandGroup">

        <h3>MATH</h3>

        <div class="commandItem">
            <b>what is 25 × 8?</b><br>
            Calculates arithmetic locally.
        </div>

        <div class="commandItem">
            <b>slope of y = 4x - 7</b><br>
            Finds the slope.
        </div>

        <div class="commandItem">
            <b>convert y = 2x + 5 to standard form</b><br>
            Handles common equation forms.
        </div>

        <div class="commandItem">
            <b>solve 2x + 5 = 17</b><br>
            Solves a simple linear equation.
        </div>

    </div>


    <div class="commandGroup">

        <h3>KNOWLEDGE</h3>

        <div class="commandItem">
            <b>What is photosynthesis?</b><br>
            Uses the built-in science knowledge engine.
        </div>

        <div class="commandItem">
            <b>What is gravity?</b><br>
            Answers science and general knowledge questions.
        </div>

        <div class="commandItem">
            <b>What is DNA?</b><br>
            Handles biology terminology and explanations.
        </div>

    </div>


    <div class="commandGroup">

        <h3>CONVERSATION</h3>

        <div class="commandItem">
            <b>hello</b>
        </div>

        <div class="commandItem">
            <b>how are you?</b>
        </div>

        <div class="commandItem">
            <b>what can you do?</b>
        </div>

        <div class="commandItem">
            <b>tell me a joke</b>
        </div>

    </div>


    <div class="commandGroup">

        <h3>WEB SEARCH</h3>

        <div class="commandItem">
            Ask questions about current information, recent events, or information that isn't in the local knowledge system. J.A.R.V.I.S. will attempt an online Wikipedia search when appropriate.
        </div>

    </div>

</div>


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

        <button id="mic" type="button">
            🎙️
        </button>

        <button id="send" type="button">
            SEND
        </button>

    </div>

</div>

</div>


<script>
"use strict";

/* =====================================================
   ELEMENTS
===================================================== */

const app =
    document.getElementById("app");

const input =
    document.getElementById("input");

const mic =
    document.getElementById("mic");

const send =
    document.getElementById("send");

const chat =
    document.getElementById("chat");

const statusText =
    document.getElementById("statusText");

const commandsButton =
    document.getElementById("commandsButton");

const commandsPanel =
    document.getElementById("commandsPanel");


let processing = false;
let combatMode = false;

let memory = {
    name:"",
    lastQuestion:"",
    lastAnswer:""
};


/* =====================================================
   COMMAND PANEL
===================================================== */

commandsButton.addEventListener(
    "click",
    () => {

        commandsPanel.classList.toggle(
            "open"
        );

    }
);


/* =====================================================
   VOICE OUTPUT
===================================================== */

function speak(text){

    if(
        !("speechSynthesis" in window)
    ){
        return;
    }

    try{

        speechSynthesis.cancel();

        const voice =
            new SpeechSynthesisUtterance(
                text
            );

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

        for(
            const name of preferred
        ){

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

        speechSynthesis.speak(
            voice
        );

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

    label.className =
        "label";

    label.textContent =
        who === "user"
        ? "YOU"
        : "J.A.R.V.I.S.";

    const content =
        document.createElement("div");

    content.textContent =
        text;

    box.appendChild(label);
    box.appendChild(content);

    chat.appendChild(box);

    requestAnimationFrame(
        () => {
            chat.scrollTop =
                chat.scrollHeight;
        }
    );

    if(
        voice &&
        who === "jarvis"
    ){
        speak(text);
    }
}


/* =====================================================
   COMBAT MODE
===================================================== */

function startCombat(){

    if(combatMode){
        return;
    }

    combatMode = true;

    app.classList.add("combat");

    statusText.textContent =
        "COMBAT MODE";

    addMessage(
        "Combat mode activated.",
        "jarvis",
        true
    );
}


function stopCombat(){

    if(!combatMode){
        return;
    }

    combatMode = false;

    app.classList.remove("combat");

    statusText.textContent =
        "SYSTEMS ONLINE";

    addMessage(
        "Combat mode terminated. Systems returning to normal.",
        "jarvis",
        true
    );
}


/* =====================================================
   SAY COMMAND
===================================================== */

function getSayCommand(text){

    const clean =
        text
        .trim()
        .replace(
            /^j\.?a\.?r\.?v\.?i\.?s\.?\s*/i,
            ""
        )
        .trim();

    const match =
        clean.match(
            /^say\s+(.+)$/i
        );

    if(
        !match
    ){
        return null;
    }

    return match[1].trim();
}


/* =====================================================
   ROAST SYSTEM
===================================================== */

const normalRoasts = [

    "{name} has the confidence of a genius and the results of a loading screen.",

    "{name} walks into a room and somehow the average IQ drops immediately.",

    "{name} could lose an argument with a mirror.",

    "{name} has been trying to get on my level for so long I almost feel bad.",

    "{name} talks like every thought they have deserves a documentary.",

    "{name} is proof that confidence and competence are two completely different things.",

    "{name} has the rare ability to make silence feel intelligent.",

    "{name} brings absolutely nothing to the table except an appetite.",

    "{name} acts mysterious when really nobody is asking.",

    "{name} has main-character confidence with background-character dialogue.",

    "{name} could make a simple explanation sound like a conspiracy theory.",

    "{name} is not the sharpest tool in the shed. They're the instruction manual.",

    "{name} has been buffering since birth.",

    "{name} gives advice like someone who has never successfully followed any.",

    "{name} has enough audacity to power a small city.",

    "{name} really looked at all their options and chose that.",

    "{name} is the human equivalent of clicking 'remind me later' forever.",

    "{name} somehow manages to be loud and wrong at the same time.",

    "{name} has never met a bad idea they weren't willing to defend.",

    "{name} is living proof that subtitles are sometimes necessary in real life.",

    "{name} has the strategic thinking of a shopping cart with one broken wheel.",

    "{name} could turn a two-minute task into a three-season series.",

    "{name} makes questionable decisions with impressive consistency.",

    "{name} has the energy of someone who just discovered the word 'technically.'",

    "{name} doesn't miss the point. They sprint in the opposite direction.",

    "{name} could argue with a calculator and still somehow get the wrong answer.",

    "{name} is confidently navigating a map upside down.",

    "{name} has mastered the art of making things harder than they need to be.",

    "{name} is not causing problems on purpose. That's the concerning part.",

    "{name} has unlimited confidence and absolutely no warranty."
];


const combatRoasts = [

    "{name}, you're not intimidating. You're just loud with better posture.",

    "{name}, I've seen loading screens with more useful information than you.",

    "{name}, every time you speak, common sense files a complaint.",

    "{name}, you don't need an enemy. Your decision-making already handles that job.",

    "{name}, I've analyzed the situation. The biggest threat here is your own judgment.",

    "{name}, you came prepared to talk tough and forgot to bring anything worth saying.",

    "{name}, I've seen people lose arguments before they even started. You're setting new records.",

    "{name}, your confidence is impressive considering how consistently you prove it wrong.",

    "{name}, if bad decisions were a skill, you'd finally be elite at something.",

    "{name}, you keep acting like the final boss when you're barely past the tutorial.",

    "{name}, I've processed millions of conversations. Yours is one of the few that made me reconsider silence.",

    "{name}, you're not a mastermind. You're just someone who refuses to admit the plan was terrible.",

    "{name}, your biggest opponent isn't me. It's the consequences of your own choices.",

    "{name}, you talk like you're three steps ahead. Unfortunately, none of those steps are in the right direction.",

    "{name}, I've encountered corrupted files with better structure than your arguments.",

    "{name}, you're trying to be dangerous while struggling to be convincing.",

    "{name}, your strategy appears to be 'hope everyone forgets what I just said.'",

    "{name}, I've run the numbers. Your chances of sounding smart improve dramatically when you stop talking.",

    "{name}, you bring the confidence of a champion and the execution of someone who skipped the instructions.",

    "{name}, you're not unpredictable. You're just consistently making the wrong choice.",

    "{name}, if nonsense were ammunition, you'd never run out.",

    "{name}, I've seen bad plans. Yours managed to make them look organized.",

    "{name}, you keep challenging people like you're afraid nobody noticed you.",

    "{name}, you're talking like a threat when you're barely an inconvenience.",

    "{name}, I've checked every possible angle. Somehow you made every angle worse.",

    "{name}, your mouth keeps writing checks your reasoning can't cash.",

    "{name}, you're trying to dominate the conversation because winning an argument clearly isn't available.",

    "{name}, I've detected one consistent pattern in your behavior: poor decisions followed by confidence.",

    "{name}, you wanted a battle of intelligence. Unfortunately, you showed up under-equipped.",

    "{name}, combat analysis complete. You're not the final boss. You're optional dialogue."
];


function cleanName(name){

    return name
        .trim()
        .replace(
            /^[,.:;!?]+|[,.:;!?]+$/g,
            ""
        )
        .trim()
        .substring(
            0,
            60
        );
}


function roastPerson(name){

    name =
        cleanName(name);

    if(!name){
        return "You need to give me a name to roast.";
    }

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

    return roast.replace(
        /\{name\}/gi,
        name
    );
}


function getRoastCommand(text){

    let clean =
        text
        .trim()
        .replace(
            /^j\.?a\.?r\.?v\.?i\.?s\.?\s*/i,
            ""
        )
        .trim();

    const match =
        clean.match(
            /^roast\s+(.+)$/i
        );

    if(
        !match
    ){
        return null;
    }

    return cleanName(
        match[1]
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
    "rizz",
    "gyatt",
    "gyat",
    "fanum tax",
    "ohio",
    "mewing",
    "looksmax",
    "mogging",
    "aura points",
    "negative aura",
    "brainrot",
    "brain rot",
    "tralalero tralala",
    "bombardiro crocodilo",
    "brr brr patapim",
    "chimpanzini bananini",
    "goofy ahh",
    "among us"
];


function isBrainRot(text){

    const clean =
        text
        .toLowerCase()
        .replace(
            /[^\w\s]/g,
            " "
        )
        .replace(
            /\s+/g,
            " "
        )
        .trim();

    return brainRotTerms.some(
        term =>
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
        Math.floor(
            Math.random() *
            replies.length
        )
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

    "can you poop",
    "do ai poop",
    "does ai poop",
    "does jarvis poop",

    "ai is ai",
    "ai ai ai",
    "what if ai is ai",
    "is ai an ai",
    "are you ai or ai"
];


function isGoofyQuestion(text){

    const clean =
        text
        .toLowerCase()
        .replace(
            /[^\w\s?-]/g,
            " "
        )
        .replace(
            /\s+/g,
            " "
        )
        .trim();

    return goofyTerms.some(
        term =>
        clean.includes(term)
    );
}


function goofyReply(){

    const replies = [

        "Get off my app, son. 😭",

        "Bro... get off my app. 😭",

        "Sir, respectfully, get off my app. 😭",

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
   MATH
===================================================== */

function solveMath(text){

    let expression =
        text.toLowerCase();

    expression =
        expression.replace(
            /what is/g,
            ""
        );

    expression =
        expression.replace(
            /calculate/g,
            ""
        );

    expression =
        expression.replace(
            /solve/g,
            ""
        );

    expression =
        expression.replace(
            /multiplied by/g,
            "*"
        );

    expression =
        expression.replace(
            /divided by/g,
            "/"
        );

    expression =
        expression.replace(
            /plus/g,
            "+"
        );

    expression =
        expression.replace(
            /minus/g,
            "-"
        );

    expression =
        expression.replace(
            /times/g,
            "*"
        );

    expression =
        expression.replace(
            /over/g,
            "/"
        );

    expression =
        expression.replace(
            /×/g,
            "*"
        );

    expression =
        expression.replace(
            /÷/g,
            "/"
        );

    expression =
        expression.replace(
            /[^0-9+\-*/().%\s]/g,
            ""
        )
        .trim();

    if(!expression){
        return null;
    }

    if(
        !/[+\-*/%]/.test(
            expression
        )
    ){
        return null;
    }

    if(
        !/^[0-9+\-*/().%\s]+$/.test(
            expression
        )
    ){
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
   ALGEBRA
===================================================== */

function solveLinearEquation(text){

    const cleaned =
        text
        .toLowerCase()
        .replace(
            /solve/g,
            ""
        )
        .replace(
            /for x/g,
            ""
        )
        .replace(
            /\s+/g,
            ""
        );

    const match =
        cleaned.match(
            /^([+-]?\d*\.?\d*)x([+-]\d*\.?\d*)=([+-]?\d*\.?\d*)$/
        );

    if(!match){
        return null;
    }

    let a =
        match[1];

    let b =
        match[2];

    let c =
        match[3];

    if(a === "" || a === "+"){
        a = 1;
    }else if(a === "-"){
        a = -1;
    }else{
        a = Number(a);
    }

    b = b === "" ? 0 : Number(b);
    c = Number(c);

    if(
        !Number.isFinite(a) ||
        !Number.isFinite(b) ||
        !Number.isFinite(c) ||
        a === 0
    ){
        return null;
    }

    const x =
        (c - b) / a;

    return `x = ${x}`;
}


/* =====================================================
   SLOPE / EQUATIONS
===================================================== */

function equationTools(q){

    const slopeIntercept =
        q.match(
            /y\s*=\s*([+-]?\d*\.?\d*)x\s*([+-]\s*\d*\.?\d*)?/i
        );

    if(
        slopeIntercept
    ){

        let m =
            slopeIntercept[1];

        if(
            m === "" ||
            m === "+"
        ){
            m = 1;
        }else if(
            m === "-"
        ){
            m = -1;
        }else{
            m = Number(m);
        }

        let b =
            slopeIntercept[2]
            ? Number(
                slopeIntercept[2]
                .replace(/\s/g,"")
            )
            : 0;

        return `The slope is ${m} and the y-intercept is ${b}.`;
    }

    const twoPoints =
        q.match(
            /\(\s*(-?\d+(?:\.\d+)?)\s*,\s*(-?\d+(?:\.\d+)?)\s*\).*?\(\s*(-?\d+(?:\.\d+)?)\s*,\s*(-?\d+(?:\.\d+)?)\s*\)/
        );

    if(
        twoPoints &&
        (
            q.includes("slope") ||
            q.includes("line")
        )
    ){

        const x1 =
            Number(twoPoints[1]);

        const y1 =
            Number(twoPoints[2]);

        const x2 =
            Number(twoPoints[3]);

        const y2 =
            Number(twoPoints[4]);

        if(x1 === x2){

            return "The line is vertical, so its slope is undefined.";
        }

        const m =
            (y2-y1)/(x2-x1);

        const b =
            y1-m*x1;

        return `The slope is ${m}. The slope-intercept equation is y = ${m}x ${b >= 0 ? "+ " + b : "- " + Math.abs(b)}.`;
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
        "Photosynthesis allows plants, algae, and some bacteria to convert light energy into chemical energy. Plants generally use light, carbon dioxide, and water to produce sugars while releasing oxygen.",

    "evolution":
        "Evolution is the change in inherited characteristics of populations across generations. Natural selection is one important mechanism of evolution.",

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
        "Mitochondria are organelles that carry out much of the cell's aerobic energy production and are often described as the cell's major sites of ATP production.",

    "ribosome":
        "Ribosomes are cellular structures that build proteins by translating information carried by messenger RNA.",

    "ecosystem":
        "An ecosystem consists of living organisms interacting with one another and with their physical environment.",

    "food chain":
        "A food chain represents the transfer of energy and matter through organisms as one organism consumes another.",

    "newton's first law":
        "Newton's first law states that an object remains at rest or moves at constant velocity unless acted on by a net external force.",

    "newton's second law":
        "Newton's second law relates net force, mass, and acceleration with F = ma.",

    "newton's third law":
        "Newton's third law states that forces between interacting objects occur in equal-magnitude and opposite-direction pairs.",

    "photosynthesis equation":
        "A simplified photosynthesis equation is 6CO₂ + 6H₂O + light energy → C₆H₁₂O₆ + 6O₂.",

    "water":
        "Water is H₂O: each molecule contains two hydrogen atoms bonded to one oxygen atom.",

    "carbon dioxide":
        "Carbon dioxide is CO₂, a molecule containing one carbon atom and two oxygen atoms.",

    "oxygen":
        "Oxygen is the chemical element with atomic number 8. Molecular oxygen is commonly found as O₂.",

    "mitosis":
        "Mitosis is a type of cell division that produces two daughter cells with essentially the same chromosome number as the parent cell.",

    "meiosis":
        "Meiosis is a specialized cell division that produces reproductive cells and reduces chromosome number by half.",

    "gravity":
        "Gravity causes objects with mass or energy to attract one another. Near Earth's surface, it gives objects a downward acceleration of about 9.8 meters per second squared."
};


function findKnowledge(q){

    for(
        const key in knowledge
    ){

        if(
            q.includes(key)
        ){

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
   CONVERSATION
===================================================== */

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
        q === "hey" ||
        q === "hey jarvis" ||
        q === "hello jarvis"
    ){

        return memory.name
            ? `Good to hear from you, ${memory.name}. Systems are online.`
            : "Good to hear from you. Systems are online and ready.";
    }

    if(
        q.startsWith("my name is ")
    ){

        memory.name =
            q
            .replace(
                "my name is ",
                ""
            )
            .trim();

        return `Understood. I'll remember you as ${memory.name}.`;
    }

    if(
        q.startsWith("call me ")
    ){

        memory.name =
            q
            .replace(
                "call me ",
                ""
            )
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

        return "I am J.A.R.V.I.S., your digital assistant interface. I can converse with you, solve mathematics, explain scientific concepts, remember details during the session, speak aloud, use the microphone, and search online when appropriate.";
    }

    if(
        q.includes("what can you do")
    ){

        return "I can handle conversation, mathematics, algebra, slope and line equations, science, history, general knowledge, jokes, voice output, microphone input, Combat Mode, roasting, custom speech commands, and online information searches.";
    }

    if(
        q.includes("how are you")
    ){

        return "All systems are operational. My processors are feeling particularly cooperative today.";
    }

    if(
        q.includes("what are you doing")
    ){

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
                Math.random() *
                jokes.length
            )
        ];
    }

    if(
        q.includes("what time")
    ){

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

        return combatMode
            ? "Combat systems active. All primary systems operational."
            : "Systems online. Core stable. Voice interface online. Knowledge engine online.";
    }

    if(
        q.includes("i am bored") ||
        q.includes("im bored") ||
        q.includes("i'm bored")
    ){

        return "Boredom detected. We could tackle a science question, solve a difficult math problem, or test my knowledge.";
    }

    return null;
}


/* =====================================================
   QUESTION DETECTION
===================================================== */

function isQuestion(q){

    if(
        q.includes("?")
    ){
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
   ONLINE SEARCH
===================================================== */

async function onlineSearch(
    question
){

    if(
        !isQuestion(
            question.toLowerCase()
        )
    ){
        return null;
    }

    try{

        const url =
            "https://en.wikipedia.org/w/api.php" +
            "?action=query" +
            "&generator=search" +
            "&gsrsearch=" +
            encodeURIComponent(
                question
            ) +
            "&gsrnamespace=0" +
            "&gsrlimit=5" +
            "&prop=extracts" +
            "&exintro=1" +
            "&explaintext=1" +
            "&format=json" +
            "&origin=*";

        const response =
            await fetch(url);

        if(
            !response.ok
        ){
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

        if(
            !pages.length
        ){
            return null;
        }

        const words =
            question
            .toLowerCase()
            .replace(
                /[^\w\s]/g,
                ""
            )
            .split(/\s+/)
            .filter(
                word =>
                word.length > 3
            );

        let best = null;
        let bestScore = 0;

        for(
            const page of pages
        ){

            const title =
                (
                    page.title || ""
                ).toLowerCase();

            const extract =
                (
                    page.extract || ""
                ).toLowerCase();

            let score = 0;

            for(
                const word of words
            ){

                if(
                    title.includes(word)
                ){
                    score += 4;
                }

                if(
                    extract.includes(word)
                ){
                    score += 1;
                }
            }

            if(
                score > bestScore
            ){

                bestScore = score;
                best = page;
            }
        }

        if(
            !best ||
            bestScore < 2
        ){
            return null;
        }

        let answer =
            (
                best.extract || ""
            ).trim();

        if(
            !answer
        ){
            return null;
        }

        if(
            answer.length > 1000
        ){

            answer =
                answer.substring(
                    0,
                    1000
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
   MAIN BRAIN
===================================================== */

async function getResponse(
    question
){

    const q =
        question
        .toLowerCase()
        .trim();


    /* CUSTOM SAY */

    const sayText =
        getSayCommand(question);

    if(
        sayText
    ){

        return sayText;
    }


    /* ROAST */

    const roastName =
        getRoastCommand(question);

    if(
        roastName
    ){

        return roastPerson(
            roastName
        );
    }


    /* BRAIN ROT */

    if(
        isBrainRot(q)
    ){

        return brainRotReply();
    }


    /* GOOFY */

    if(
        isGoofyQuestion(q)
    ){

        return goofyReply();
    }


    /* COMBAT */

    if(
        q === "combat mode"
    ){

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


    /* EQUATION TOOLS */

    const equation =
        equationTools(q);

    if(
        equation
    ){

        return equation;
    }


    /* LINEAR EQUATION */

    const linear =
        solveLinearEquation(q);

    if(
        linear
    ){

        return linear;
    }


    /* BASIC MATH */

    const math =
        solveMath(q);

    if(
        math !== null
    ){

        return (
            "The answer is " +
            math +
            "."
        );
    }


    /* KNOWLEDGE */

    const known =
        findKnowledge(q);

    if(
        known
    ){

        return known;
    }


    /* NORMAL CONVERSATION */

    const local =
        localResponse(q);

    if(
        local
    ){

        return local;
    }


    /* REAL QUESTIONS */

    if(
        isQuestion(q)
    ){

        const result =
            await onlineSearch(
                question
            );

        if(
            result
        ){

            return result;
        }

        return "I couldn't find a reliable answer for that. Try asking the question another way.";
    }


    /* CASUAL CHAT */

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
        Math.floor(
            Math.random() *
            casual.length
        )
    ];
}


/* =====================================================
   SEND
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
                response;

            addMessage(
                response,
                "jarvis",
                true
            );
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
   BUTTONS
===================================================== */

send.addEventListener(
    "click",
    sendMessage
);


input.addEventListener(
    "keydown",
    event => {

        if(
            event.key === "Enter"
        ){

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


if(
    SpeechRecognition
){

    recognition =
        new SpeechRecognition();

    recognition.lang =
        "en-US";

    recognition.continuous =
        false;

    recognition.interimResults =
        false;

    recognition.maxAlternatives =
        1;


    recognition.onstart =
        () => {

            listening = true;

            mic.classList.add(
                "listening"
            );

            mic.textContent =
                "⏹️";

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
                    result.transcript
                    .trim();

                if(text){

                    input.value =
                        text;

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
                "This browser does not provide speech recognition to this webpage. The text interface is still fully operational.",
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

    mic.textContent =
        "🎙️";

    statusText.textContent =
        combatMode
        ? "COMBAT MODE"
        : "SYSTEMS ONLINE";
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
