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

    padding:
        9px
        10px
        max(12px,env(safe-area-inset-bottom));

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

    requestAnimationFrame(
        () => {
            chat.scrollTop =
                chat.scrollHeight;
        }
    );

    if(voice && who === "jarvis"){
        speak(text);
    }
}


/* =====================================================
   SAY COMMAND
===================================================== */

function extractSayCommand(text){

    const patterns = [
        /^jarvis\s+say\s+(.+)$/i,
        /^j\.a\.r\.v\.i\.s\.\s+say\s+(.+)$/i,
        /^j\.a\.r\.v\.i\.s\s+say\s+(.+)$/i,
        /^say\s+(.+)$/i
    ];

    for(const pattern of patterns){

        const match =
            text.trim().match(pattern);

        if(match && match[1]){

            return match[1].trim();
        }
    }

    return null;
}


/* =====================================================
   ROAST SYSTEM
===================================================== */

const playfulRoasts = [

    "That name has the same energy as a phone at 1% battery: technically alive, but nobody expects much from it.",

    "I've seen loading screens with more personality than that.",

    "That name walked into the room and somehow lowered the Wi-Fi signal.",

    "If confidence were currency, that name would still be asking for a loan.",

    "That name sounds like it was generated by a random username button.",

    "Even autocorrect looked at that name and decided it wasn't worth fixing.",

    "That name has main-character confidence with background-character results.",

    "I've processed billions of calculations and somehow that name is still confusing.",

    "That name is proof that being memorable and being impressive are two completely different things.",

    "That name has the charisma of a forgotten password.",

    "If common sense were a subscription, that name would be on the free trial.",

    "That name has been carrying absolutely nothing and still looks tired.",

    "That name gives off strong 'forgot why I walked into the room' energy.",

    "That name could lose an argument with a loading icon.",

    "That name is not a red flag. It's the entire warning screen.",

    "That name has the confidence of someone who has never checked their own work.",

    "That name is built like a plot twist nobody asked for.",

    "That name has the energy of a group project member who says 'what are we doing?' five minutes before the deadline.",

    "That name could make a tutorial look complicated.",

    "That name is the human equivalent of clicking 'remind me later' forever.",

    "That name has the tactical awareness of a shopping cart with one broken wheel.",

    "That name doesn't need an enemy. It already has itself.",

    "That name is what happens when confidence loads faster than competence.",

    "That name has absolutely mastered the art of being confidently incorrect.",

    "That name is giving premium confidence with a free-trial skill set.",

    "That name could walk into a room full of mirrors and still blame somebody else.",

    "That name has the decision-making skills of a coin with both sides saying 'bad idea'.",

    "That name is somehow both the problem and the unnecessary sequel.",

    "That name has the energy of someone who says 'trust me' right before making everything worse.",

    "That name is impressive in the same way a warning label is impressive."
];


const combatRoasts = [

    "Listen carefully. That name walks around with the confidence of a champion and the decision-making of someone who has never met a consequence.",

    "That name has mistaken being loud for being intimidating. The difference is obvious.",

    "I've seen stronger arguments made by a disconnected controller.",

    "That name talks like a threat and performs like a technical difficulty.",

    "That name doesn't bring pressure into a room. It brings problems that somebody else has to solve.",

    "That name has the audacity of a genius and the preparation of someone who forgot the entire plan.",

    "That name wants respect without doing anything that would actually earn it.",

    "That name isn't intimidating. It's just extremely committed to being annoying.",

    "That name has confidence without the evidence to support it.",

    "That name spends so much time trying to look dangerous that it forgets to actually be competent.",

    "That name is what happens when ego gets promoted before ability.",

    "That name entered the conversation like a final boss and immediately started acting like a tutorial.",

    "That name has all the aggression of a war speech and none of the follow-through.",

    "That name doesn't dominate conversations. It just refuses to notice when everyone stopped listening.",

    "That name has the strategic depth of a puddle.",

    "That name keeps bringing confidence to situations where preparation would've been more useful.",

    "That name is proof that sounding certain and being correct are completely unrelated skills.",

    "That name could turn an easy victory into a complicated explanation for why it wasn't their fault.",

    "That name has the attitude of someone who expects the scoreboard to apologize.",

    "That name talks like consequences are something that happen to other people.",

    "That name has been overestimating itself so consistently that it's practically a routine.",

    "That name isn't a serious threat. It's a recurring inconvenience.",

    "That name has one impressive skill: making every situation unnecessarily harder.",

    "That name has enough ego to fill a room and enough sense to leave none for anyone else.",

    "That name doesn't need more confidence. It needs evidence.",

    "That name keeps trying to act like the final boss when the room hasn't even finished the tutorial.",

    "That name has the combat instincts of someone arguing with a stop sign.",

    "That name brings intensity to everything because apparently competence wasn't available.",

    "That name has the rare ability to make preparation look optional.",

    "That name isn't scary. It's just extremely confident about being wrong."
];


function roastPerson(name){

    name =
        name
        .trim()
        .replace(/\s+/g," ");

    if(!name){
        return "Give me a name first.";
    }

    const list =
        combatMode
        ? combatRoasts
        : playfulRoasts;

    const roast =
        list[
            Math.floor(
                Math.random() * list.length
            )
        ];

    return `${name}, ${roast}`;
}


function extractRoastCommand(text){

    const match =
        text.match(
            /^(?:jarvis\s+|j\.a\.r\.v\.i\.s\.\s+)?roast\s+(.+)$/i
        );

    if(!match){
        return null;
    }

    return match[1].trim();
}


/* =====================================================
   COMBAT
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
        "Combat Mode activated.",
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
        "Combat Mode terminated. Systems returning to normal.",
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
    "rizz",
    "gyatt",
    "fanum tax",
    "ohio",
    "mewing",
    "looksmax",
    "mog",
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
        Math.floor(
            Math.random() * replies.length
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
    "are you ai ai",
    "what is an ai ai"
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

        "Bro... get off my app. 😭",

        "Sir, respectfully, get off my app. 😭",

        "J.A.R.V.I.S. is requesting that you get off my app, son. 😭",

        "My processors have had enough. 😭",

        "That question just lowered my IQ. 😭"

    ];

    return replies[
        Math.floor(
            Math.random() * replies.length
        )
    ];
}


/* =====================================================
   NUMBER PARSER
===================================================== */

function parseNumber(value){

    value =
        String(value)
        .trim()
        .replace(/,/g,"");

    if(value.includes("/")){

        const parts =
            value.split("/");

        if(parts.length === 2){

            const a =
                parseFloat(parts[0]);

            const b =
                parseFloat(parts[1]);

            if(
                Number.isFinite(a) &&
                Number.isFinite(b) &&
                b !== 0
            ){
                return a / b;
            }
        }
    }

    const n =
        parseFloat(value);

    return Number.isFinite(n)
        ? n
        : null;
}


/* =====================================================
   FRACTION FORMATTER
===================================================== */

function gcd(a,b){

    a = Math.abs(a);
    b = Math.abs(b);

    while(b){

        const temp = b;

        b = a % b;
        a = temp;
    }

    return a;
}

function fraction(value){

    if(!Number.isFinite(value)){
        return "undefined";
    }

    if(Number.isInteger(value)){
        return String(value);
    }

    const sign =
        value < 0 ? "-" : "";

    value =
        Math.abs(value);

    const precision = 1000000;

    let numerator =
        Math.round(value * precision);

    let denominator =
        precision;

    const divisor =
        gcd(numerator,denominator);

    numerator /= divisor;
    denominator /= divisor;

    return sign +
        numerator +
        "/" +
        denominator;
}


/* =====================================================
   BASIC MATH
===================================================== */

function solveMath(text){

    let expression =
        text.toLowerCase();

    expression =
        expression
        .replace(/what is/g,"")
        .replace(/calculate/g,"")
        .replace(/compute/g,"")
        .replace(/solve/g,"")
        .replace(/equals/g,"")
        .replace(/multiplied by/g,"*")
        .replace(/divided by/g,"/")
        .replace(/plus/g,"+")
        .replace(/minus/g,"-")
        .replace(/times/g,"*")
        .replace(/over/g,"/")
        .replace(/×/g,"*")
        .replace(/÷/g,"/");

    expression =
        expression
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

        return answer;

    }catch{
        return null;
    }
}


/* =====================================================
   ALGEBRA ENGINE
===================================================== */

function cleanEquation(text){

    return text
        .toLowerCase()
        .replace(/[−–—]/g,"-")
        .replace(/×/g,"*")
        .replace(/\s+/g," ")
        .trim();
}


/*
   Converts simple linear expression into:

   ax + b

   Examples:

   2x + 3
   -4x - 7
   x + 5
   -x + 2
   3x
   5
*/

function linearExpression(expr){

    expr =
        expr
        .replace(/\s/g,"")
        .replace(/\*/g,"");

    if(!expr){
        return {a:0,b:0};
    }

    expr =
        expr
        .replace(/-/g,"+-");

    if(expr.startsWith("+")){
        expr = expr.substring(1);
    }

    const terms =
        expr.split("+");

    let a = 0;
    let b = 0;

    for(let term of terms){

        if(!term){
            continue;
        }

        if(term.includes("x")){

            term =
                term.replace(/x/g,"");

            if(term === "" || term === "+"){
                a += 1;
            }
            else if(term === "-"){
                a -= 1;
            }
            else{
                const n =
                    parseFloat(term);

                if(!Number.isFinite(n)){
                    return null;
                }

                a += n;
            }

        }else{

            const n =
                parseFloat(term);

            if(!Number.isFinite(n)){
                return null;
            }

            b += n;
        }
    }

    return {a,b};
}


/* =====================================================
   SOLVE LINEAR EQUATION
===================================================== */

function solveLinearEquation(equation){

    equation =
        cleanEquation(equation);

    equation =
        equation
        .replace(/^solve\s+/,"")
        .replace(/^x\s*=\s*/,"x=");

    if(!equation.includes("=")){
        return null;
    }

    const sides =
        equation.split("=");

    if(sides.length !== 2){
        return null;
    }

    const left =
        linearExpression(sides[0]);

    const right =
        linearExpression(sides[1]);

    if(!left || !right){
        return null;
    }

    const coefficient =
        left.a - right.a;

    const constant =
        right.b - left.b;

    if(coefficient === 0){

        if(constant === 0){

            return {
                type:"all",
                text:"There are infinitely many solutions."
            };

        }

        return {
            type:"none",
            text:"There is no solution."
        };
    }

    const x =
        constant / coefficient;

    return {
        type:"value",
        value:x,
        text:
            "x = " +
            fraction(x)
    };
}


/* =====================================================
   SLOPE FROM TWO POINTS
===================================================== */

function slopeFromPoints(text){

    const match =
        text.match(
            /\(\s*(-?\d+(?:\.\d+)?)\s*,\s*(-?\d+(?:\.\d+)?)\s*\)\s*(?:and|,)\s*\(\s*(-?\d+(?:\.\d+)?)\s*,\s*(-?\d+(?:\.\d+)?)\s*\)/i
        );

    if(!match){
        return null;
    }

    const x1 =
        parseFloat(match[1]);

    const y1 =
        parseFloat(match[2]);

    const x2 =
        parseFloat(match[3]);

    const y2 =
        parseFloat(match[4]);

    if(x1 === x2){

        return {
            vertical:true,
            text:"The slope is undefined because the line is vertical."
        };
    }

    const m =
        (y2-y1)/(x2-x1);

    return {
        vertical:false,
        slope:m,
        text:
            "m = (y₂ − y₁) / (x₂ − x₁)\n" +
            "m = (" + y2 + " − " + y1 + ") / (" +
            x2 + " − " + x1 + ")\n" +
            "m = " + fraction(m)
    };
}


/* =====================================================
   EXTRACT LINEAR EQUATION
===================================================== */

function parseStandardForm(text){

    const cleaned =
        cleanEquation(text);

    if(!cleaned.includes("=")){
        return null;
    }

    const parts =
        cleaned.split("=");

    if(parts.length !== 2){
        return null;
    }

    const left =
        parts[0]
        .replace(/\*/g,"");

    const right =
        parts[1]
        .replace(/\*/g,"");

    /*
       Ax + By = C
    */

    const combined =
        linearExpression(left);

    const rightNumber =
        parseFloat(right);

    if(
        !combined ||
        !Number.isFinite(rightNumber)
    ){
        return null;
    }

    /*
       linearExpression gives ax+b.
       For standard form:
       Ax + By = C

       This engine focuses on one-variable
       linear equations and y=mx+b.
    */

    return {
        a:combined.a,
        b:combined.b,
        c:rightNumber
    };
}


/* =====================================================
   SLOPE-INTERCEPT
===================================================== */

function slopeInterceptFromEquation(text){

    const eq =
        cleanEquation(text);

    if(!eq.includes("=")){
        return null;
    }

    const parts =
        eq.split("=");

    if(parts.length !== 2){
        return null;
    }

    let left = parts[0];
    let right = parts[1];

    /*
       If y is on the right:
       2x + 3 = y
       move it mentally to y = 2x + 3
    */

    if(right.includes("y") && !left.includes("y")){

        const temp = left;
        left = right;
        right = temp;
    }

    if(!left.includes("y")){
        return null;
    }

    /*
       Handle:
       y = 2x + 3
       y = -4x - 7
       y = x + 2
       y = -x + 9
    */

    const yMatch =
        left.match(/^y$/);

    if(yMatch){

        const rightLinear =
            linearExpression(right);

        if(!rightLinear){
            return null;
        }

        return {
            m:rightLinear.a,
            b:rightLinear.b
        };
    }

    /*
       Handle:
       2y = 4x + 8
       -3y = 6x - 9
    */

    const yCoefficientMatch =
        left.match(/^(-?\d*\.?\d*)y$/);

    if(
        yCoefficientMatch &&
        right.includes("x")
    ){

        let coefficient =
            yCoefficientMatch[1];

        if(
            coefficient === "" ||
            coefficient === "+"
        ){
            coefficient = 1;
        }
        else if(coefficient === "-"){
            coefficient = -1;
        }
        else{
            coefficient =
                parseFloat(coefficient);
        }

        const r =
            linearExpression(right);

        if(!r || coefficient === 0){
            return null;
        }

        return {
            m:r.a/coefficient,
            b:r.b/coefficient
        };
    }

    /*
       Standard form:
       Ax + By = C
    */

    const standard =
        left.match(
            /^(-?\d*\.?\d*)x([+-]\d*\.?\d*)y$/
        );

    if(standard){

        let A =
            standard[1];

        let B =
            standard[2];

        const C =
            parseFloat(right);

        if(A === "" || A === "+"){
            A = 1;
        }
        else if(A === "-"){
            A = -1;
        }
        else{
            A = parseFloat(A);
        }

        B =
            parseFloat(B);

        if(
            !Number.isFinite(A) ||
            !Number.isFinite(B) ||
            !Number.isFinite(C) ||
            B === 0
        ){
            return null;
        }

        return {
            m:-A/B,
            b:C/B
        };
    }

    return null;
}


/* =====================================================
   FORMAT LINE
===================================================== */

function formatSlopeIntercept(m,b){

    let result =
        "y = ";

    if(m === 1){
        result += "x";
    }
    else if(m === -1){
        result += "-x";
    }
    else{
        result += fraction(m) + "x";
    }

    if(b > 0){
        result += " + " + fraction(b);
    }
    else if(b < 0){
        result += " - " + fraction(Math.abs(b));
    }

    return result;
}


/* =====================================================
   POINT-SLOPE
===================================================== */

function pointSlope(text){

    const point =
        text.match(
            /\(\s*(-?\d+(?:\.\d+)?)\s*,\s*(-?\d+(?:\.\d+)?)\s*\)/i
        );

    if(!point){
        return null;
    }

    const mMatch =
        text.match(
            /slope\s*(?:is|=)\s*(-?\d+(?:\.\d+)?(?:\/\d+)?)/i
        );

    if(!mMatch){
        return null;
    }

    const x =
        parseFloat(point[1]);

    const y =
        parseFloat(point[2]);

    const m =
        parseNumber(mMatch[1]);

    if(m === null){
        return null;
    }

    const b =
        y - m*x;

    return {
        m,
        b,
        text:
            "Slope-intercept form: " +
            formatSlopeIntercept(m,b)
    };
}


/* =====================================================
   X INTERCEPT
===================================================== */

function xIntercept(text){

    const line =
        slopeInterceptFromEquation(text);

    if(!line){
        return null;
    }

    if(line.m === 0){

        if(line.b === 0){

            return {
                text:"Every point on the line has y = 0."
            };

        }

        return {
            text:"This horizontal line has no x-intercept."
        };
    }

    const x =
        -line.b / line.m;

    return {
        value:x,
        text:
            "The x-intercept is (" +
            fraction(x) +
            ", 0)."
    };
}


/* =====================================================
   Y INTERCEPT
===================================================== */

function yIntercept(text){

    const line =
        slopeInterceptFromEquation(text);

    if(!line){
        return null;
    }

    return {
        value:line.b,
        text:
            "The y-intercept is (0, " +
            fraction(line.b) +
            ")."
    };
}


/* =====================================================
   CONVERT EQUATION
===================================================== */

function algebraResponse(question){

    const q =
        cleanEquation(question);


    /*
       SLOPE BETWEEN TWO POINTS
    */

    if(
        q.includes("slope") &&
        (
            q.includes("between") ||
            q.includes("points")
        )
    ){

        const result =
            slopeFromPoints(q);

        if(result){
            return result.text;
        }
    }


    /*
       EXPLICIT X-INTERCEPT
    */

    if(
        q.includes("x-intercept") ||
        q.includes("x intercept")
    ){

        const equation =
            q
            .replace(/what is/g,"")
            .replace(/find/g,"")
            .replace(/the x-intercept/g,"")
            .replace(/the x intercept/g,"")
            .trim();

        const result =
            xIntercept(equation);

        if(result){
            return result.text;
        }
    }


    /*
       EXPLICIT Y-INTERCEPT
    */

    if(
        q.includes("y-intercept") ||
        q.includes("y intercept")
    ){

        const equation =
            q
            .replace(/what is/g,"")
            .replace(/find/g,"")
            .replace(/the y-intercept/g,"")
            .replace(/the y intercept/g,"")
            .trim();

        const result =
            yIntercept(equation);

        if(result){
            return result.text;
        }
    }


    /*
       CONVERT TO SLOPE INTERCEPT
    */

    if(
        q.includes("slope-intercept") ||
        q.includes("slope intercept") ||
        q.includes("y=mx+b") ||
        q.includes("y = mx + b")
    ){

        const equation =
            q
            .replace(/convert/g,"")
            .replace(/to slope-intercept form/g,"")
            .replace(/to slope intercept form/g,"")
            .replace(/write in slope-intercept form/g,"")
            .replace(/write in slope intercept form/g,"")
            .trim();

        const result =
            slopeInterceptFromEquation(equation);

        if(result){

            return (
                "Slope-intercept form is:\n\n" +
                formatSlopeIntercept(
                    result.m,
                    result.b
                ) +
                "\n\n" +
                "Slope (m) = " +
                fraction(result.m) +
                "\n" +
                "Y-intercept (b) = " +
                fraction(result.b)
            );
        }
    }


    /*
       POINT-SLOPE
    */

    if(
        q.includes("point-slope") ||
        q.includes("point slope")
    ){

        const result =
            pointSlope(q);

        if(result){
            return result.text;
        }
    }


    /*
       SOLVE ALGEBRA EQUATION
    */

    if(
        q.includes("=") &&
        (
            q.includes("solve") ||
            /[0-9x]\s*=/.test(q) ||
            /=\s*[0-9x]/.test(q)
        )
    ){

        const equation =
            q
            .replace(/^solve\s*/,"")
            .trim();

        const result =
            solveLinearEquation(equation);

        if(result){

            if(result.type === "value"){

                return (
                    "Solving the equation:\n\n" +
                    equation +
                    "\n\n" +
                    "x = " +
                    fraction(result.value)
                );
            }

            return result.text;
        }
    }


    /*
       SLOPE OF AN EQUATION
    */

    if(
        q.includes("slope") &&
        q.includes("=")
    ){

        const result =
            slopeInterceptFromEquation(q);

        if(result){

            return (
                "The slope is m = " +
                fraction(result.m) +
                "."
            );
        }
    }


    return null;
}


/* =====================================================
   CONVERSION ENGINE
===================================================== */

const conversionUnits = {

    "miles to kilometers": value => value * 1.609344,
    "kilometers to miles": value => value / 1.609344,

    "feet to meters": value => value * 0.3048,
    "meters to feet": value => value / 0.3048,

    "inches to centimeters": value => value * 2.54,
    "centimeters to inches": value => value / 2.54,

    "yards to meters": value => value * 0.9144,
    "meters to yards": value => value / 0.9144,

    "pounds to kilograms": value => value * 0.45359237,
    "kilograms to pounds": value => value / 0.45359237,

    "ounces to grams": value => value * 28.349523125,
    "grams to ounces": value => value / 28.349523125,

    "gallons to liters": value => value * 3.785411784,
    "liters to gallons": value => value / 3.785411784,

    "fahrenheit to celsius": value => (value-32)*5/9,
    "celsius to fahrenheit": value => value*9/5+32,

    "celsius to kelvin": value => value+273.15,
    "kelvin to celsius": value => value-273.15,

    "minutes to hours": value => value/60,
    "hours to minutes": value => value*60,

    "seconds to minutes": value => value/60,
    "minutes to seconds": value => value*60,

    "days to hours": value => value*24,
    "hours to days": value => value/24
};


function conversionResponse(text){

    const q =
        text
        .toLowerCase()
        .replace(/,/g,"")
        .trim();

    const match =
        q.match(
            /^(-?\d+(?:\.\d+)?)\s*(.+?)\s+(?:to|in)\s+(.+)$/
        );

    if(!match){
        return null;
    }

    const value =
        parseFloat(match[1]);

    let from =
        match[2].trim();

    let to =
        match[3].trim();

    const aliases = {
        mi:"miles",
        mile:"miles",
        km:"kilometers",
        kilometer:"kilometers",
        ft:"feet",
        foot:"feet",
        m:"meters",
        meter:"meters",
        in:"inches",
        inch:"inches",
        cm:"centimeters",
        centimeter:"centimeters",
        yd:"yards",
        yard:"yards",
        lb:"pounds",
        pound:"pounds",
        kg:"kilograms",
        kilogram:"kilograms",
        oz:"ounces",
        ounce:"ounces",
        g:"grams",
        gram:"grams",
        gal:"gallons",
        gallon:"gallons",
        l:"liters",
        liter:"liters",
        liters:"liters",
        c:"celsius",
        f:"fahrenheit",
        k:"kelvin",
        sec:"seconds",
        second:"seconds",
        min:"minutes",
        minute:"minutes",
        hr:"hours",
        hour:"hours",
        day:"days"
    };

    from =
        aliases[from] || from;

    to =
        aliases[to] || to;

    const key =
        from + " to " + to;

    if(
        conversionUnits[key] &&
        Number.isFinite(value)
    ){

        const result =
            conversionUnits[key](value);

        return (
            value +
            " " +
            from +
            " = " +
            Number(result.toFixed(8)) +
            " " +
            to +
            "."
        );
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
        "Earth's oceans cover roughly 71 percent of the planet's surface and contain most of Earth's water."
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
   LOCAL RESPONSE
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

        return "I am J.A.R.V.I.S., your digital assistant interface. I can converse with you, solve mathematics and algebra, analyze linear equations, convert units, speak aloud, and search for information.";
    }

    if(q.includes("what can you do")){

        return "I can handle conversation, mathematics, algebra, slope-intercept form, point-slope form, standard form, linear equations, intercepts, unit conversions, science, history, jokes, voice output, microphone input, Combat Mode, roasting commands, and online information searches.";
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
                Math.random() * jokes.length
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

        return combatMode
            ? "Combat systems active. All primary systems operational."
            : "Systems online. Core stable. Voice interface online. Knowledge engine online.";
    }

    if(
        q.includes("i am bored") ||
        q.includes("im bored") ||
        q.includes("i'm bored")
    ){

        return "Boredom detected. We could tackle a science question, solve an algebra problem, analyze a linear equation, or test my knowledge.";
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
        "find ",
        "solve "
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
            .filter(
                word => word.length > 3
            );

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

        if(!answer){
            return null;
        }

        if(answer.length > 1200){

            answer =
                answer.substring(0,1200) +
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

async function getResponse(question){

    const q =
        question
        .toLowerCase()
        .trim();


    /*
       SAY COMMAND
    */

    const sayText =
        extractSayCommand(question);

    if(sayText){

        return {
            text:sayText,
            speakText:sayText
        };
    }


    /*
       ROAST COMMAND
    */

    const roastName =
        extractRoastCommand(question);

    if(roastName){

        const roast =
            roastPerson(roastName);

        return {
            text:roast,
            speakText:roast
        };
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
       ADVANCED ALGEBRA
    */

    const algebra =
        algebraResponse(question);

    if(algebra){
        return algebra;
    }


    /*
       UNIT CONVERSION
    */

    const conversion =
        conversionResponse(question);

    if(conversion){
        return conversion;
    }


    /*
       BASIC MATH
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
       NORMAL CHAT
    */

    const local =
        localResponse(q);

    if(local){
        return local;
    }


    /*
       REAL QUESTIONS
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
       CASUAL
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
        Math.floor(
            Math.random() * casual.length
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
            await getResponse(question);

        if(response){

            let text = response;
            let voiceText = response;

            if(
                typeof response === "object"
            ){

                text =
                    response.text;

                voiceText =
                    response.speakText ||
                    response.text;
            }

            memory.lastAnswer =
                text;

            addMessage(
                text,
                "jarvis",
                false
            );

            speak(voiceText);
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


    recognition.onstart =
        () => {

            listening = true;

            mic.classList.add("listening");

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
    "Good day. J.A.R.V.I.S. systems are online. Algebra, linear equations, conversions, voice commands, and knowledge systems are ready. How may I assist you?",
    "jarvis",
    true
);

</script>

</body>
</html>
