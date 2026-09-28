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

    .logo{
        font-size:18px;
    }

    .status{
        font-size:8px;
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
}
</style>
</head>

<body>

<div id="app">

<header class="header">

    <div class="logo">
        J.A.R.V.I.S.
    </div>

    <div class="status">

        <span class="dot"></span>

        <span id="statusText">
            SYSTEMS ONLINE
        </span>

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

const app = document.getElementById("app");
const input = document.getElementById("input");
const mic = document.getElementById("mic");
const send = document.getElementById("send");
const chat = document.getElementById("chat");
const statusText = document.getElementById("statusText");

let processing = false;
let combatMode = false;

let memory = {
    name:"",
    lastQuestion:"",
    lastAnswer:""
};


/* =====================================================
   VOICE
===================================================== */

function speak(text){

    if(!("speechSynthesis" in window)) return;

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
        (who === "user" ? "user" : "jarvis");

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

    requestAnimationFrame(() => {
        chat.scrollTop = chat.scrollHeight;
    });

    if(voice && who === "jarvis"){
        speak(text);
    }
}


/* =====================================================
   COMBAT MODE
===================================================== */

function startCombat(){

    if(combatMode) return;

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

    if(!combatMode) return;

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
   ROAST SYSTEM
===================================================== */

function normalRoast(name){

    const roasts = [

        `${name} has the confidence of someone who has never heard themselves talk.`,

        `${name} walks into a room and somehow the average IQ drops.`,

        `${name} could lose an argument with a search bar.`,

        `${name} has a talent for making simple things look complicated.`,

        `${name} talks with the confidence of a genius and the accuracy of a random guess.`,

        `${name} is proof that confidence and competence are two completely different things.`,

        `${name} could have the answer right in front of them and still ask where it went.`,

        `${name} has the rare ability to make silence feel like an upgrade.`,

        `${name} doesn't need an enemy. Their decision-making already handles that job.`,

        `${name} brings absolutely nothing to the table except questions about who ate the food.`,

        `${name} has been buffering for years.`,

        `${name} is the human version of clicking "remind me later" forever.`,

        `${name} could turn a two-minute task into a three-season series.`,

        `${name} has enough bad ideas to keep a group project alive forever.`,

        `${name} doesn't miss the point. The point actively avoids them.`,

        `${name} has the strategic thinking of a coin toss.`,

        `${name} could make a straight line take a wrong turn.`,

        `${name} is operating on vibes, guesses, and absolutely no supporting evidence.`,

        `${name} has never met a bad decision they didn't want to make twice.`,

        `${name} could probably get lost using a map with one road on it.`,

        `${name} is living proof that autocorrect cannot fix everything.`,

        `${name} has a PhD in being confidently incorrect.`,

        `${name} somehow manages to be early to the wrong conclusion.`,

        `${name} has the problem-solving skills of a locked door.`,

        `${name} could complicate a yes-or-no question.`,

        `${name} is not the sharpest tool in the shed, and the shed has several dull tools.`,

        `${name} has enough excuses to open a whole accounting department.`,

        `${name} makes common sense look like an advanced subject.`,

        `${name} has mastered the art of doing everything except the thing they were supposed to do.`,

        `${name} could read the instructions and still freestyle the entire assignment.`

    ];

    return roasts[
        Math.floor(Math.random()*roasts.length)
    ];
}


function combatRoast(name){

    const roasts = [

        `${name}, you have the tactical awareness of a traffic cone. Stop pretending you know what you're doing.`,

        `${name}, every time you speak, your own argument gets weaker. Even silence would outperform you.`,

        `${name}, you're not intimidating. You're just loud enough to be annoying.`,

        `${name}, I've seen loading screens make better decisions than you.`,

        `${name}, you bring confidence to situations where you have absolutely no idea what is happening.`,

        `${name}, your strategy appears to be making mistakes until something accidentally works.`,

        `${name}, if poor decisions were ammunition, you'd never run out.`,

        `${name}, you're trying to act like a threat while your entire plan is falling apart in real time.`,

        `${name}, your biggest opponent isn't me. It's your complete inability to think ahead.`,

        `${name}, you don't need a battle plan. Apparently you prefer improvising disasters.`,

        `${name}, I've analyzed your approach. Calling it an approach is generous.`,

        `${name}, you entered this argument like a champion and immediately demonstrated why that was a mistake.`,

        `${name}, your confidence is doing all the heavy lifting because your reasoning clearly isn't.`,

        `${name}, you're swinging at problems with nothing but attitude and bad timing.`,

        `${name}, you have the precision of a blindfolded dart thrower.`,

        `${name}, your plan has more holes than an old screen door.`,

        `${name}, I've seen people fight their own shoelaces with better tactical coordination.`,

        `${name}, you're not one step ahead. You're several steps behind and somehow walking backward.`,

        `${name}, your decision-making process appears to be "do it first, regret it later."`,

        `${name}, you're bringing playground logic to a situation that requires actual thinking.`,

        `${name}, if this were a strategy game, I'd assume you were intentionally trying to lose.`,

        `${name}, you keep talking like you're in control. The evidence strongly disagrees.`,

        `${name}, your plan isn't aggressive. It's just poorly thought out.`,

        `${name}, you're giving orders like somebody appointed you commander of absolutely nothing.`,

        `${name}, you have the tactical depth of a puddle and somehow less stability.`,

        `${name}, every move you make answers the question "how could this get worse?"`,

        `${name}, you're not unpredictable. You're consistently making the wrong choice.`,

        `${name}, I've seen random number generators produce more coherent strategies.`,

        `${name}, you're acting like the final boss when you're barely clearing the tutorial.`,

        `${name}, your greatest combat skill appears to be creating problems for yourself.`
    ];

    return roasts[
        Math.floor(Math.random()*roasts.length)
    ];
}


function handleRoastCommand(q){

    const match =
        q.match(
            /^j\.?a\.?r\.?v\.?i\.?s[,\s]+roast\s+(.+)$/i
        );

    if(!match) return null;

    let name =
        match[1]
        .trim()
        .replace(/[.!?]+$/,"");

    if(!name){
        return "Give me a name and I'll handle the rest.";
    }

    return combatMode
        ? combatRoast(name)
        : normalRoast(name);
}


/* =====================================================
   SAY COMMAND
===================================================== */

function handleSayCommand(q){

    const match =
        q.match(
            /^j\.?a\.?r\.?v\.?i\.?s[,\s]+say\s+(.+)$/i
        );

    if(!match) return null;

    const text =
        match[1].trim();

    if(!text){
        return "Tell me what you'd like me to say.";
    }

    return text;
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

    return brainRotTerms.some(
        term => clean.includes(term)
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
    "if an ai is an ai",
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

    return goofyTerms.some(
        term => clean.includes(term)
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
        Math.floor(Math.random()*replies.length)
    ];
}


/* =====================================================
   MATH ENGINE
===================================================== */

function safeMath(expression){

    expression =
        expression
        .replace(/×/g,"*")
        .replace(/÷/g,"/")
        .replace(/\^/g,"**");

    if(!/^[0-9+\-*/().%\s*]+$/.test(expression)){
        return null;
    }

    try{

        const result =
            Function(
                '"use strict";return (' +
                expression +
                ')'
            )();

        if(
            typeof result !== "number" ||
            !Number.isFinite(result)
        ){
            return null;
        }

        return result;

    }catch{

        return null;
    }
}

function solveMath(text){

    let expression =
        text.toLowerCase();

    expression =
        expression
        .replace(/what is/g,"")
        .replace(/calculate/g,"")
        .replace(/solve/g,"")
        .replace(/multiplied by/g,"*")
        .replace(/divided by/g,"/")
        .replace(/plus/g,"+")
        .replace(/minus/g,"-")
        .replace(/times/g,"*")
        .replace(/over/g,"/")
        .replace(/[^0-9+\-*/().%\s×÷]/g,"")
        .trim();

    if(!expression) return null;

    if(!/[+\-*/%]/.test(expression)){
        return null;
    }

    return safeMath(expression);
}


/* =====================================================
   LINEAR ALGEBRA
===================================================== */

function fraction(n,d){

    if(d === 0) return "undefined";

    if(n === 0) return "0";

    const sign =
        d < 0 ? -1 : 1;

    n *= sign;
    d *= sign;

    const gcd =
        (a,b) =>
        b === 0
        ? Math.abs(a)
        : gcd(b,a%b);

    const g = gcd(n,d);

    n /= g;
    d /= g;

    if(d === 1) return String(n);

    return `${n}/${d}`;
}


function solveSlopeIntercept(text){

    const t =
        text
        .toLowerCase()
        .replace(/−/g,"-")
        .replace(/=/g," = ");

    if(
        !t.includes("slope") &&
        !t.includes("y=mx+b") &&
        !t.includes("y = mx + b")
    ){
        return null;
    }

    const match =
        t.match(
            /y\s*=\s*([+-]?\s*\d*\.?\d*)\s*x\s*([+-]\s*\d*\.?\d*)?/i
        );

    if(match){

        let m =
            match[1]
            .replace(/\s/g,"");

        if(m === "" || m === "+") m = "1";
        if(m === "-") m = "-1";

        let b =
            match[2]
            ? match[2].replace(/\s/g,"")
            : "0";

        return `The slope is ${m} and the y-intercept is ${b}.`;
    }

    return null;
}


function solveTwoPoints(text){

    const matches =
        [...text.matchAll(
            /\(?\s*(-?\d+(?:\.\d+)?)\s*,\s*(-?\d+(?:\.\d+)?)\s*\)?/g
        )];

    if(matches.length < 2) return null;

    const x1 = Number(matches[0][1]);
    const y1 = Number(matches[0][2]);
    const x2 = Number(matches[1][1]);
    const y2 = Number(matches[1][2]);

    if(x1 === x2){

        return "The line is vertical, so its slope is undefined.";
    }

    const m =
        (y2-y1)/(x2-x1);

    const b =
        y1 - m*x1;

    const ms =
        Number.isInteger(m)
        ? String(m)
        : fraction(
            Math.round(m*1000000),
            1000000
        );

    const bs =
        Number.isInteger(b)
        ? String(b)
        : fraction(
            Math.round(b*1000000),
            1000000
        );

    const equation =
        b === 0
        ? `y = ${ms}x`
        : `y = ${ms}x ${b >= 0 ? "+" : "-"} ${Math.abs(b)}`;

    return `Using the points (${x1}, ${y1}) and (${x2}, ${y2}), the slope is ${ms}. The slope-intercept equation is ${equation}.`;
}


function solvePointSlope(text){

    const points =
        [...text.matchAll(
            /\(?\s*(-?\d+(?:\.\d+)?)\s*,\s*(-?\d+(?:\.\d+)?)\s*\)?/g
        )];

    if(points.length < 1) return null;

    const slopeMatch =
        text.match(
            /slope\s*(?:is|=)\s*(-?\d+(?:\.\d+)?)/i
        );

    if(!slopeMatch) return null;

    const m =
        Number(slopeMatch[1]);

    const x =
        Number(points[0][1]);

    const y =
        Number(points[0][2]);

    const b =
        y - m*x;

    const equation =
        `y = ${m}x ${b >= 0 ? "+" : "-"} ${Math.abs(b)}`;

    return `The point-slope equation is y - ${y} = ${m}(x - ${x}). In slope-intercept form, that is ${equation}.`;
}


/* =====================================================
   SCIENCE KNOWLEDGE
===================================================== */

const science = {

    "photosynthesis":
        "Photosynthesis is the process by which plants, algae, and some bacteria use light energy to make chemical energy from carbon dioxide and water, producing glucose and oxygen.",

    "cellular respiration":
        "Cellular respiration is the process cells use to release usable energy from food. In aerobic respiration, glucose reacts with oxygen and produces carbon dioxide, water, and ATP.",

    "mitochondria":
        "Mitochondria are organelles that produce much of a eukaryotic cell's ATP through cellular respiration.",

    "ribosome":
        "Ribosomes build proteins by translating information carried by messenger RNA.",

    "nucleus":
        "The nucleus stores most of a eukaryotic cell's DNA and helps regulate gene expression.",

    "dna":
        "DNA is the molecule that stores hereditary genetic information. Its bases are adenine, thymine, cytosine, and guanine.",

    "rna":
        "RNA is a nucleic acid involved in gene expression and protein production. Unlike DNA, RNA commonly uses uracil instead of thymine.",

    "osmosis":
        "Osmosis is the movement of water across a selectively permeable membrane from an area of higher water concentration toward lower water concentration.",

    "diffusion":
        "Diffusion is the net movement of particles from an area of higher concentration to an area of lower concentration.",

    "homeostasis":
        "Homeostasis is an organism's ability to maintain relatively stable internal conditions despite external changes.",

    "natural selection":
        "Natural selection occurs when inherited traits that improve survival or reproduction become more common in a population over generations.",

    "evolution":
        "Evolution is the change in inherited characteristics of populations across generations.",

    "gravity":
        "Gravity is the attraction associated with mass and energy. On Earth, it accelerates falling objects downward at roughly 9.8 meters per second squared near the surface.",

    "newton's first law":
        "Newton's first law states that an object remains at rest or moves at constant velocity unless acted on by a net external force.",

    "newton's second law":
        "Newton's second law is commonly expressed as F = ma. Net force equals mass multiplied by acceleration.",

    "newton's third law":
        "Newton's third law states that forces between interacting objects occur in equal-magnitude, opposite-direction pairs.",

    "velocity":
        "Velocity describes how quickly an object's position changes and includes direction.",

    "acceleration":
        "Acceleration is the rate at which velocity changes over time.",

    "kinetic energy":
        "Kinetic energy is the energy an object has because of its motion. It is calculated with KE = 1/2 mv².",

    "potential energy":
        "Potential energy is stored energy associated with an object's position, condition, or configuration.",

    "atom":
        "An atom consists of a nucleus containing protons and usually neutrons, surrounded by electrons.",

    "proton":
        "A proton is a positively charged subatomic particle found in an atomic nucleus.",

    "neutron":
        "A neutron is a neutral subatomic particle found in an atomic nucleus.",

    "electron":
        "An electron is a negatively charged subatomic particle found in regions surrounding an atomic nucleus.",

    "periodic table":
        "The periodic table organizes chemical elements by atomic number and recurring chemical properties.",

    "ph":
        "The pH scale describes how acidic or basic an aqueous solution is. Lower pH values are more acidic, while higher values are more basic.",

    "acid":
        "An acid is a substance that can donate hydrogen ions in aqueous solution under the Brønsted-Lowry definition.",

    "base":
        "A base is a substance that can accept hydrogen ions under the Brønsted-Lowry definition.",

    "chemical reaction":
        "A chemical reaction rearranges atoms to form different substances while conserving the atoms involved.",

    "ecosystem":
        "An ecosystem includes living organisms and the nonliving environment interacting in a particular area.",

    "food chain":
        "A food chain shows how energy and matter move through feeding relationships among organisms.",

    "food web":
        "A food web represents interconnected feeding relationships within an ecosystem.",

    "carbon cycle":
        "The carbon cycle describes how carbon moves among the atmosphere, organisms, oceans, soil, and Earth's crust.",

    "water cycle":
        "The water cycle describes processes including evaporation, condensation, precipitation, infiltration, and collection that move water through Earth's systems.",

    "plate tectonics":
        "Plate tectonics describes the movement and interaction of large pieces of Earth's lithosphere.",

    "earthquake":
        "An earthquake is the shaking of Earth's surface caused by the sudden release of energy, commonly along faults.",

    "volcano":
        "A volcano is a location where magma, gases, and volcanic material can reach Earth's surface.",

    "weather":
        "Weather describes short-term atmospheric conditions such as temperature, precipitation, humidity, wind, and pressure.",

    "climate":
        "Climate describes long-term patterns and averages of weather in a region.",

    "black hole":
        "A black hole is a region of spacetime with gravity so strong that beyond its event horizon, nothing can escape to the outside.",

    "speed of light":
        "The speed of light in a vacuum is exactly 299,792,458 meters per second.",

    "moon":
        "The Moon is Earth's natural satellite. Its gravity contributes significantly to Earth's ocean tides.",

    "mars":
        "Mars is the fourth planet from the Sun. It is a rocky planet with a thin atmosphere dominated by carbon dioxide.",

    "jupiter":
        "Jupiter is the largest planet in our Solar System and is a gas giant.",

    "saturn":
        "Saturn is a gas giant known for its extensive system of icy rings.",

    "venus":
        "Venus is the second planet from the Sun and has a dense carbon-dioxide atmosphere and an extreme greenhouse effect.",

    "mercury":
        "Mercury is the smallest planet in the Solar System and the planet closest to the Sun.",

    "neutron star":
        "A neutron star is an extremely dense stellar remnant produced by the collapse of the core of certain massive stars."
};


/* =====================================================
   GENERAL KNOWLEDGE
===================================================== */

const knowledge = {

    "earth":
        "Earth is the third planet from the Sun and the only world currently known to support life.",

    "sun":
        "The Sun is the star at the center of our Solar System. It produces most of its energy through nuclear fusion.",

    "shakespeare":
        "William Shakespeare was an English playwright and poet whose works include Hamlet, Macbeth, and Romeo and Juliet.",

    "newton":
        "Isaac Newton developed foundational laws of motion and universal gravitation and made major contributions to mathematics and optics.",

    "marie curie":
        "Marie Curie was a physicist and chemist whose research into radioactivity earned Nobel Prizes in Physics and Chemistry.",

    "chemistry":
        "Chemistry is the study of matter, its properties, composition, structure, and the changes it undergoes.",

    "biology":
        "Biology is the scientific study of living organisms and life processes.",

    "physics":
        "Physics studies matter, energy, motion, forces, fields, space, and the fundamental interactions of nature.",

    "history":
        "History is the study and interpretation of past events using evidence such as documents, artifacts, records, and other sources.",

    "geography":
        "Geography studies places, environments, Earth's physical systems, and relationships between people and their surroundings.",

    "computer":
        "A computer is an electronic system that processes information according to instructions called programs."
};


function findKnowledge(q){

    for(const key in science){

        if(q.includes(key)){
            return science[key];
        }
    }

    for(const key in knowledge){

        if(q.includes(key)){
            return knowledge[key];
        }
    }

    return null;
}


/* =====================================================
   MULTIPLE CHOICE ENGINE
===================================================== */

function parseChoices(text){

    const choices = [];

    const patterns = [

        /(?:^|\n)\s*\(?([A-H])\)?[\.\):\-]\s*(.+?)(?=\n\s*\(?[A-H]\)?[\.\):\-]|$)/gi,

        /([A-H])[\.\):\-]\s*([^A-H]+?)(?=\s+[A-H][\.\):\-]|$)/gi

    ];

    for(const pattern of patterns){

        const matches =
            [...text.matchAll(pattern)];

        if(matches.length >= 2){

            for(const match of matches){

                choices.push({
                    letter:match[1].toUpperCase(),
                    text:match[2].trim()
                });
            }

            break;
        }
    }

    const unique = [];

    for(const choice of choices){

        if(
            !unique.some(
                x => x.letter === choice.letter
            )
        ){
            unique.push(choice);
        }
    }

    return unique;
}


function normalizeAnswer(text){

    return text
        .toLowerCase()
        .replace(/[^\w\s]/g," ")
        .replace(/\s+/g," ")
        .trim();
}


function questionKeywords(text){

    const stopWords = new Set([

        "what","which","who","where","when","why","how",
        "is","are","was","were","the","a","an","of","to",
        "in","on","for","and","or","does","do","did",
        "with","from","that","this","these","those",
        "most","best","least","following","correct",
        "statement","statements","according","about"
    ]);

    return normalizeAnswer(text)
        .split(" ")
        .filter(
            word =>
            word.length > 2 &&
            !stopWords.has(word)
        );
}


function scoreChoice(question,choice){

    const qWords =
        questionKeywords(question);

    const cWords =
        questionKeywords(choice.text);

    let score = 0;

    for(const word of qWords){

        if(cWords.includes(word)){
            score += 1;
        }
    }

    const c =
        normalizeAnswer(choice.text);

    /*
       Science concepts
    */

    const sciencePairs = [

        ["mitochondria","atp"],
        ["photosynthesis","glucose"],
        ["photosynthesis","oxygen"],
        ["dna","genetic"],
        ["ribosome","protein"],
        ["nucleus","dna"],
        ["osmosis","water"],
        ["diffusion","concentration"],
        ["gravity","mass"],
        ["newton","force"],
        ["acceleration","velocity"],
        ["kinetic","motion"],
        ["potential","stored"],
        ["acid","hydrogen"],
        ["base","hydroxide"],
        ["ecosystem","organisms"],
        ["plate","tectonic"],
        ["weather","atmosphere"],
        ["climate","long-term"]
    ];

    for(const pair of sciencePairs){

        if(
            qWords.includes(pair[0]) &&
            c.includes(pair[1])
        ){
            score += 8;
        }
    }

    /*
       Math patterns
    */

    if(
        /slope/.test(question.toLowerCase()) &&
        /rise|run|coefficient|rate/.test(c)
    ){
        score += 5;
    }

    if(
        /y\s*=\s*mx\s*\+\s*b/.test(
            question.toLowerCase()
        ) &&
        /slope|m/.test(c)
    ){
        score += 5;
    }

    return score;
}


function answerMultipleChoice(text){

    const choices =
        parseChoices(text);

    if(choices.length < 2){
        return null;
    }

    const lines =
        text.split(/\n/);

    let questionText = text;

    if(lines.length > 1){

        questionText =
            lines
            .filter(
                line =>
                !/^\s*\(?[A-H]\)?[\.\):\-]/i.test(line)
            )
            .join(" ");
    }

    let best = null;
    let bestScore = -1;

    for(const choice of choices){

        const score =
            scoreChoice(
                questionText,
                choice
            );

        if(score > bestScore){

            bestScore = score;
            best = choice;
        }
    }

    /*
       Recognize very common educational questions.
    */

    const q =
        questionText.toLowerCase();

    const rules = [

        {
            test:/organelle.*atp|produce.*atp|energy.*cell/,
            answer:/mitochond/
        },

        {
            test:/process.*plant.*light|plant.*make.*food/,
            answer:/photosynthesis/
        },

        {
            test:/genetic.*information|hereditary.*information/,
            answer:/dna/
        },

        {
            test:/protein.*made|build.*protein/,
            answer:/ribosome/
        },

        {
            test:/movement.*water.*membrane/,
            answer:/osmosis/
        },

        {
            test:/particles.*high.*concentration.*low/,
            answer:/diffusion/
        },

        {
            test:/f\s*=\s*m\s*a|force.*mass.*acceleration/,
            answer:/force|newton/
        },

        {
            test:/pH.*7|neutral.*pH/,
            answer:/7|neutral/
        }
    ];

    for(const rule of rules){

        if(rule.test.test(q)){

            const matching =
                choices.find(
                    choice =>
                    rule.answer.test(
                        choice.text.toLowerCase()
                    )
                );

            if(matching){
                best = matching;
                bestScore = 100;
                break;
            }
        }
    }

    if(!best) return null;

    return `The correct answer is ${best.letter}: ${best.text}.`;
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
   LOCAL CONVERSATION
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

        return "I am J.A.R.V.I.S., your digital assistant interface. I can converse with you, solve mathematics, answer academic questions, speak aloud, process multiple-choice questions, and search for information when necessary.";
    }

    if(q.includes("what can you do")){

        return "I can handle conversation, mathematics, algebra, linear equations, slope-intercept form, point-slope form, science, history, culinary questions, peer counseling topics, multiple-choice questions, jokes, voice output, microphone input, Combat Mode, and online information searches.";
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

    if(q.includes("?")) return true;

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
        word => q.startsWith(word)
    );
}


/* =====================================================
   ONLINE SEARCH
===================================================== */

async function onlineSearch(question){

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
            .filter(word => word.length > 3);

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

        if(
            !best ||
            bestScore < 2
        ){
            return null;
        }

        let answer =
            (best.extract || "").trim();

        if(!answer) return null;

        if(answer.length > 900){

            answer =
                answer.substring(0,900) +
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

    const q =
        question
        .toLowerCase()
        .trim();


    /*
       SAY COMMAND
    */

    const sayCommand =
        handleSayCommand(question);

    if(sayCommand !== null){

        return sayCommand;
    }


    /*
       ROAST COMMAND
    */

    const roastCommand =
        handleRoastCommand(question);

    if(roastCommand !== null){

        return roastCommand;
    }


    /*
       BRAIN ROT
    */

    if(isBrainRot(q)){

        return brainRotReply();
    }


    /*
       GOOFY
    */

    if(isGoofyQuestion(q)){

        return goofyReply();
    }


    /*
       COMBAT
    */

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


    /*
       MULTIPLE CHOICE
    */

    const multipleChoice =
        answerMultipleChoice(question);

    if(multipleChoice){

        return multipleChoice;
    }


    /*
       LINEAR MATH
    */

    const slopeIntercept =
        solveSlopeIntercept(question);

    if(slopeIntercept){

        return slopeIntercept;
    }


    const twoPoints =
        solveTwoPoints(question);

    if(twoPoints){

        return twoPoints;
    }


    const pointSlope =
        solvePointSlope(question);

    if(pointSlope){

        return pointSlope;
    }


    /*
       NORMAL MATH
    */

    const math =
        solveMath(q);

    if(math !== null){

        return "The answer is " + math + ".";
    }


    /*
       BUILT-IN KNOWLEDGE
    */

    const known =
        findKnowledge(q);

    if(known){

        return known;
    }


    /*
       LOCAL CONVERSATION
    */

    const local =
        localResponse(q);

    if(local){

        return local;
    }


    /*
       ONLINE QUESTIONS
    */

    if(isQuestion(q)){

        const result =
            await onlineSearch(question);

        if(result){

            return result;
        }

        return "I couldn't find a reliable answer for that. Try asking the question another way.";
    }


    /*
       CASUAL CHAT
    */

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
   SEND
===================================================== */

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

    memory.lastQuestion =
        question;

    input.value = "";

    try{

        const response =
            await getResponse(question);

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

    setTimeout(() => {

        try{

            input.focus({
                preventScroll:true
            });

        }catch{

            input.focus();
        }

    },50);
}


send.addEventListener(
    "click",
    sendMessage
);


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

        mic.classList.add("listening");

        mic.textContent = "⏹️";

        statusText.textContent =
            combatMode
            ? "COMBAT • LISTENING"
            : "LISTENING...";
    };


    recognition.onresult = event => {

        try{

            const result =
                event.results[0][0];

            if(!result) return;

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


    recognition.onerror = event => {

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


    recognition.onend = () => {

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

    mic.classList.remove("listening");

    mic.textContent = "🎙️";

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
