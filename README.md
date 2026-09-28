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
    padding:0 12px;
    padding-top:env(safe-area-inset-top);
    display:flex;
    align-items:center;
    justify-content:space-between;
    background:rgba(0,8,12,.96);
    border-bottom:1px solid rgba(0,234,255,.35);
    z-index:100;
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
    display:flex;
    align-items:center;
    gap:5px;
}

.combat .status{
    color:#ff3030;
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

#commandsButton{
    margin-left:auto;
    margin-right:12px;
    height:34px;
    padding:0 10px;
    border:1px solid #00eaff;
    border-radius:8px;
    background:rgba(0,234,255,.08);
    color:#00eaff;
    font-size:10px;
    font-weight:bold;
    letter-spacing:1px;
}

.combat #commandsButton{
    border-color:#ff3030;
    color:#ff3030;
}

#commandPanel{
    position:absolute;
    top:68px;
    right:10px;
    width:min(340px,calc(100% - 20px));
    max-height:70vh;
    overflow:auto;
    padding:15px;
    background:rgba(0,10,15,.98);
    border:1px solid rgba(0,234,255,.55);
    border-radius:12px;
    z-index:1000;
    display:none;
    box-shadow:0 0 30px rgba(0,234,255,.15);
}

#commandPanel.show{
    display:block;
}

.commandTitle{
    font-weight:bold;
    letter-spacing:2px;
    margin-bottom:12px;
}

.commandItem{
    padding:8px 0;
    border-bottom:1px solid rgba(255,255,255,.08);
    color:#ddd;
    font-size:13px;
    line-height:1.4;
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
    overflow-x:hidden;
    -webkit-overflow-scrolling:touch;
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

.combat #mic,
.combat #send{
    border-color:#ff3030;
    color:#ff3030;
}

#mic.listening{
    background:rgba(0,234,255,.3);
    box-shadow:0 0 18px rgba(0,234,255,.7);
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
    .logo{font-size:17px}
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

    <button id="commandsButton">COMMANDS</button>

    <div class="status">
        <span class="dot"></span>
        <span id="statusText">SYSTEMS ONLINE</span>
    </div>

</header>

<div id="commandPanel">

    <div class="commandTitle">J.A.R.V.I.S. COMMANDS</div>

    <div class="commandItem">“Combat mode” — activates Combat Mode.</div>
    <div class="commandItem">“Normal mode” — returns to normal.</div>
    <div class="commandItem">“J.A.R.V.I.S. say [text]” — makes J.A.R.V.I.S. say exactly what you requested.</div>
    <div class="commandItem">“Roast [name]” — gives a roast for the person.</div>
    <div class="commandItem">“Simple: [topic]” or “[topic] simple” — explains the topic simply.</div>
    <div class="commandItem">“My name is [name]” — remembers your name during the session.</div>
    <div class="commandItem">“Tell me a joke” — tells a joke.</div>
    <div class="commandItem">Ask math questions — algebra, equations, slope, percentages and calculations.</div>
    <div class="commandItem">Ask science questions — biology, chemistry, physics, Earth science and space.</div>
    <div class="commandItem">Ask Bible questions — verses, people, stories, meanings, context and Christian concepts.</div>
    <div class="commandItem">Ask history questions — historical events, people and civilizations.</div>
    <div class="commandItem">Ask normal questions — J.A.R.V.I.S. decides whether online research is useful.</div>

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
const commandsButton = document.getElementById("commandsButton");
const commandPanel = document.getElementById("commandPanel");

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

commandsButton.addEventListener("click",()=>{
    commandPanel.classList.toggle("show");
});


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

            selected = voices.find(
                v =>
                v.name.toLowerCase()
                .includes(name.toLowerCase())
            );

            if(selected) break;
        }

        if(!selected){

            selected = voices.find(
                v =>
                v.lang &&
                v.lang.toLowerCase().startsWith("en")
            );
        }

        if(selected) voice.voice = selected;

        speechSynthesis.speak(voice);

    }catch(error){
        console.log("Voice error:",error);
    }
}


/* =====================================================
   CHAT
===================================================== */

function addMessage(text,who="jarvis",voice=false){

    const box = document.createElement("div");

    box.className =
        "message " +
        (who === "user" ? "user" : "jarvis");

    const label = document.createElement("span");

    label.className = "label";

    label.textContent =
        who === "user"
        ? "YOU"
        : "J.A.R.V.I.S.";

    const content = document.createElement("div");

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
   COMBAT
===================================================== */

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
    "mogging",
    "aura points",
    "negative aura",
    "brainrot",
    "tralalero tralala",
    "bombardiro crocodilo",
    "brr brr patapim",
    "chimpanzini bananini",
    "goofy ahh",
    "among us"
];

function isBrainRot(text){

    const clean =
        text.toLowerCase()
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
    "can you poop",
    "do ai poop",
    "does ai poop",
    "does jarvis poop",
    "can jarvis poop",
    "ai is ai",
    "ai ai ai"
];

function isGoofyQuestion(text){

    const clean =
        text.toLowerCase()
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
        "My processors have had enough. Get off my app, son. 😭"
    ];

    return replies[
        Math.floor(Math.random()*replies.length)
    ];
}


/* =====================================================
   ROAST ENGINE
===================================================== */

const normalRoasts = [
    "{name} walks into a room and somehow the IQ drops.",
    "{name} has the confidence of a genius and the decision-making of a loading screen.",
    "{name} could lose an argument with a search bar.",
    "{name} brings absolutely nothing to the table except confusion.",
    "{name} is proof that confidence and competence are two completely different things.",
    "{name} has a special talent for making simple things unnecessarily complicated.",
    "{name} talks like they have the answers, then immediately proves they don't.",
    "{name} is the human version of a typo.",
    "{name} could make a GPS question their own directions.",
    "{name} has enough bad ideas to keep everyone entertained for years.",
    "{name} doesn't need an enemy. Their own decisions are doing enough damage.",
    "{name} has mastered the art of being confidently wrong.",
    "{name} could turn a five-minute task into a three-hour disaster.",
    "{name} is somehow always involved and somehow never useful.",
    "{name} has the timing of an alarm clock nobody asked for.",
    "{name} has the rare ability to make silence feel productive.",
    "{name} is what happens when a bad idea refuses to stay an idea.",
    "{name} could probably get lost in a straight hallway.",
    "{name} has been buffering since birth.",
    "{name} makes common sense look uncommon.",
    "{name} is the reason instructions have pictures.",
    "{name} could overthink a yes-or-no question.",
    "{name} has never met a bad decision they didn't want to make.",
    "{name} is running on confidence and absolutely no evidence.",
    "{name} could trip over a wireless connection.",
    "{name} has main-character confidence with background-character decisions.",
    "{name} is somehow both the problem and the plot twist.",
    "{name} makes chaos look organized.",
    "{name} could make a calculator ask for help."
];

const combatRoasts = [
    "{name} is not intimidating. They're just loud with bad decisions.",
    "{name} has absolutely nothing behind that attitude.",
    "{name} talks like a threat and performs like a warning label.",
    "{name} walked in looking for a fight and forgot to bring a reason.",
    "{name} has the confidence of someone who has never been corrected.",
    "{name} is all attitude and no follow-through.",
    "{name} tries to intimidate people and somehow ends up embarrassing themselves instead.",
    "{name} doesn't command respect. They demand attention and hope nobody notices the difference.",
    "{name} has a mouth full of confidence and a brain full of excuses.",
    "{name} keeps acting dangerous like somebody forgot to tell them they're not.",
    "{name} is what happens when ego gets promoted without earning it.",
    "{name} wants everyone to think they're tough. That's adorable.",
    "{name} talks like a final boss and behaves like an optional tutorial.",
    "{name} has more attitude than ability.",
    "{name} came looking for dominance and found a reality check.",
    "{name} is not a threat. They're a distraction.",
    "{name} keeps confusing aggression with strength.",
    "{name} has the personality of an argument nobody wanted.",
    "{name} thinks being disrespectful makes them powerful. It doesn't.",
    "{name} is trying way too hard to look dangerous.",
    "{name} brings hostility where personality should be.",
    "{name} has mistaken volume for authority.",
    "{name} is not built for the energy they're trying to give off.",
    "{name} keeps talking like they're untouchable. Reality disagrees.",
    "{name} is the kind of person who starts problems and then acts surprised when nobody respects them.",
    "{name} has an ego doing all the heavy lifting.",
    "{name} wants to be feared so badly that it's almost embarrassing.",
    "{name} has the intimidation factor of an angry house cat.",
    "{name} keeps escalating because they have nothing intelligent left to say.",
    "{name} should probably stop trying to prove something they clearly can't."
];

function roastPerson(name){

    name = name.trim();

    if(!name){
        return "You need to give me a name first.";
    }

    const list =
        combatMode
        ? combatRoasts
        : normalRoasts;

    const template =
        list[Math.floor(Math.random()*list.length)];

    return template.replace(
        /\{name\}/g,
        name
    );
}


/* =====================================================
   "JARVIS SAY"
===================================================== */

function isSayCommand(q){

    return (
        q.startsWith("jarvis say ") ||
        q.startsWith("j.a.r.v.i.s. say ") ||
        q.startsWith("jarvis, say ") ||
        q.startsWith("j.a.r.v.i.s., say ")
    );
}

function getSayText(q){

    const prefixes = [
        "j.a.r.v.i.s. say ",
        "j.a.r.v.i.s., say ",
        "jarvis say ",
        "jarvis, say "
    ];

    for(const prefix of prefixes){

        if(q.startsWith(prefix)){
            return q.slice(prefix.length).trim();
        }
    }

    return "";
}


/* =====================================================
   SIMPLE EXPLANATION
===================================================== */

function wantsSimple(q){

    return (
        q.endsWith(" simple") ||
        q.startsWith("simple ") ||
        q.includes(" explain simply") ||
        q.includes("in simple terms")
    );
}

function removeSimpleRequest(q){

    return q
        .replace(/^simple\s+/i,"")
        .replace(/\s+simple$/i,"")
        .replace(/\s+in simple terms$/i,"")
        .replace(/\s+explain simply$/i,"")
        .trim();
}


/* =====================================================
   MATH ENGINE
===================================================== */

function solveMath(text){

    let expression = text.toLowerCase();

    expression = expression.replace(/what is/g,"");
    expression = expression.replace(/calculate/g,"");
    expression = expression.replace(/solve/g,"");

    expression = expression.replace(/multiplied by/g,"*");
    expression = expression.replace(/divided by/g,"/");
    expression = expression.replace(/plus/g,"+");
    expression = expression.replace(/minus/g,"-");
    expression = expression.replace(/times/g,"*");
    expression = expression.replace(/over/g,"/");

    expression = expression.replace(/×/g,"*");
    expression = expression.replace(/÷/g,"/");

    expression =
        expression
        .replace(/[^0-9+\-*/().%\s]/g,"")
        .trim();

    if(!expression) return null;

    if(!/[+\-*/%]/.test(expression)) return null;

    if(!/^[0-9+\-*/().%\s]+$/.test(expression)){
        return null;
    }

    try{

        const answer =
            Function(
                '"use strict";return ('+
                expression+
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
   ALGEBRA / LINE EQUATIONS
===================================================== */

function algebraResponse(q){

    let m;

    m = q.match(
        /slope\s*(?:of|=)?\s*(-?\d+(?:\.\d+)?)/
    );

    if(m){
        return `The slope is ${m[1]}.`;
    }

    m = q.match(
        /y\s*=\s*(-?\d+(?:\.\d+)?)\s*x\s*([+-]\s*\d+(?:\.\d+)?)?/
    );

    if(m && q.includes("slope")){

        const slope = m[1];

        const intercept =
            m[2]
            ? m[2].replace(/\s/g,"")
            : "0";

        return `In y = mx + b, the slope m is ${slope} and the y-intercept b is ${intercept}.`;
    }

    const points =
        q.match(
            /(?:points?|through)\s*\(?\s*(-?\d+(?:\.\d+)?)\s*,\s*(-?\d+(?:\.\d+)?)\s*\)?\s*(?:and|to)\s*\(?\s*(-?\d+(?:\.\d+)?)\s*,\s*(-?\d+(?:\.\d+)?)\s*\)?/
        );

    if(points){

        const x1 = Number(points[1]);
        const y1 = Number(points[2]);
        const x2 = Number(points[3]);
        const y2 = Number(points[4]);

        if(x2 === x1){
            return "The line is vertical, so its slope is undefined.";
        }

        const slope =
            (y2-y1)/(x2-x1);

        const b =
            y1-slope*x1;

        return `Using the two points, the slope is ${slope} and the slope-intercept equation is y = ${slope}x ${b >= 0 ? "+" : "-"} ${Math.abs(b)}.`;
    }

    return null;
}


/* =====================================================
   SCIENCE KNOWLEDGE
===================================================== */

const scienceKnowledge = {

    photosynthesis:
        "Photosynthesis is the process plants and other organisms use to convert light energy into chemical energy. In basic terms, carbon dioxide and water are used to produce glucose, while oxygen is released.",

    "cellular respiration":
        "Cellular respiration is how cells release usable energy from food. In aerobic respiration, glucose is broken down using oxygen and produces ATP, carbon dioxide, and water.",

    dna:
        "DNA is the molecule that stores hereditary genetic information. Its four main bases are adenine, thymine, cytosine, and guanine.",

    atom:
        "An atom is a basic unit of matter. It contains a nucleus with protons and usually neutrons, with electrons occupying regions around the nucleus.",

    gravity:
        "Gravity is an interaction associated with mass and energy. Near Earth's surface, it causes objects to accelerate downward at about 9.8 meters per second squared.",

    evolution:
        "Evolution is the change in inherited characteristics of populations across generations. Natural selection is one important mechanism that can produce evolutionary change.",

    "speed of light":
        "The speed of light in a vacuum is exactly 299,792,458 meters per second.",

    "newton's first law":
        "Newton's first law says an object remains at rest or continues moving at constant velocity unless acted on by a net external force.",

    "newton's second law":
        "Newton's second law relates force, mass, and acceleration: F = ma.",

    "newton's third law":
        "Newton's third law says forces between interacting objects occur in equal-magnitude and opposite-direction pairs.",

    "periodic table":
        "The periodic table organizes chemical elements by atomic number and groups elements with related properties.",

    ecosystem:
        "An ecosystem includes living organisms and the nonliving environment they interact with.",

    "food chain":
        "A food chain shows how energy and matter move between organisms through feeding relationships.",

    volcano:
        "A volcano is a geological opening through which magma, gases, and volcanic material can reach Earth's surface.",

    "plate tectonics":
        "Plate tectonics describes the movement and interaction of large sections of Earth's lithosphere. Their interactions can produce earthquakes, mountains, and volcanoes.",

    "black hole":
        "A black hole is a region of spacetime where gravity is extremely strong. Its event horizon marks a boundary beyond which escape to the outside is not possible."
};


/* =====================================================
   GENERAL KNOWLEDGE
===================================================== */

const knowledge = {

    earth:
        "Earth is the third planet from the Sun and the only world currently known to support life.",

    sun:
        "The Sun is the star at the center of our Solar System. It produces energy mainly through nuclear fusion.",

    moon:
        "The Moon is Earth's natural satellite. Its gravitational interaction with Earth contributes strongly to ocean tides.",

    mars:
        "Mars is the fourth planet from the Sun. It is a rocky planet with a thin atmosphere dominated by carbon dioxide.",

    jupiter:
        "Jupiter is the largest planet in our Solar System and is a gas giant.",

    saturn:
        "Saturn is a gas giant famous for its extensive system of icy rings.",

    venus:
        "Venus is the second planet from the Sun. Its dense atmosphere creates an extreme greenhouse effect.",

    mercury:
        "Mercury is the smallest planet in the Solar System and the closest planet to the Sun.",

    "neutron star":
        "A neutron star is an extremely dense stellar remnant formed from the collapsed core of certain massive stars.",

    chemistry:
        "Chemistry is the study of matter, its composition, properties, structure, and the changes it undergoes.",

    cell:
        "A cell is the basic structural and functional unit of living organisms.",

    ocean:
        "Earth's oceans cover roughly 71 percent of the planet's surface."
};


/* =====================================================
   BIBLE KNOWLEDGE
===================================================== */

const bibleKnowledge = {

    bible:
        "The Bible is a collection of writings that form the central sacred scriptures of Christianity. It contains the Old Testament and New Testament.",

    "old testament":
        "The Old Testament contains writings that form the first major section of the Christian Bible. It includes historical narratives, poetry, wisdom literature, and prophetic writings.",

    "new testament":
        "The New Testament contains the four Gospels, Acts, letters, and Revelation. It focuses especially on Jesus, the early Christian movement, and Christian teaching.",

    genesis:
        "Genesis is the first book of the Bible. It contains creation accounts, the fall of humanity, the flood, and stories involving figures such as Abraham, Isaac, Jacob, and Joseph.",

    exodus:
        "Exodus tells the story of Israel's deliverance from Egypt, Moses, the covenant at Sinai, and the construction of the tabernacle.",

    psalms:
        "Psalms is a collection of songs, prayers, and poems that express praise, grief, thanksgiving, trust, repentance, and hope.",

    proverbs:
        "Proverbs contains wisdom sayings dealing with subjects such as wisdom, discipline, speech, relationships, work, justice, and the fear of the Lord.",

    "john 3:16":
        "John 3:16 is a famous Christian verse about God's love for the world and the promise of eternal life through belief in His Son. In simple terms, it emphasizes God's love and salvation through Jesus.",

    "romans 8:28":
        "Romans 8:28 teaches that God works through circumstances for the good of those who love Him and are called according to His purpose. It is commonly understood as an encouragement to trust God's purpose even during difficult circumstances.",

    "psalm 23":
        "Psalm 23 presents God as a shepherd who guides, protects, provides for, and stays with His people. The central idea is trust in God's care.",

    "matthew 5":
        "Matthew 5 begins Jesus' Sermon on the Mount. It includes the Beatitudes and teachings about righteousness, anger, love, prayer, and how followers of God should live.",

    "1 corinthians 13":
        "1 Corinthians 13 is a famous passage about love. It emphasizes that genuine love is patient, kind, humble, and enduring, and places love above spiritual gifts.",

    "galatians 5":
        "Galatians 5 discusses Christian freedom and contrasts the works of the flesh with the fruit of the Spirit, including love, joy, peace, patience, kindness, goodness, faithfulness, gentleness, and self-control.",

    "ephesians 6":
        "Ephesians 6 includes teaching about the armor of God. It uses the imagery of armor to describe spiritual qualities such as truth, righteousness, faith, salvation, and God's word.",

    "ten commandments":
        "The Ten Commandments are a set of foundational commands associated with Moses and the covenant at Sinai. They address worship, relationships, honesty, respect, and conduct.",

    trinity:
        "In mainstream Christianity, the Trinity describes one God understood as Father, Son, and Holy Spirit. Christians use the term to describe God's unity while distinguishing the three persons.",

    salvation:
        "In Christianity, salvation generally refers to being rescued from sin and restored to relationship with God. Christian traditions differ somewhat in how they explain the details of salvation.",

    repentance:
        "Repentance generally means turning away from sin and turning toward God. It involves a change in direction rather than merely feeling sorry.",

    forgiveness:
        "Christian teaching strongly emphasizes forgiveness. It involves releasing personal vengeance and extending mercy, while forgiveness does not necessarily mean ignoring wrongdoing or removing appropriate boundaries.",

    "holy spirit":
        "The Holy Spirit is the third person of the Trinity in mainstream Christian theology. Christian teaching describes the Spirit as God's presence and as active in guidance, transformation, comfort, and spiritual life.",

    jesus:
        "Jesus is the central figure of Christianity. Christians believe He is the Son of God and the Messiah, and the New Testament describes His teachings, death, and resurrection.",

    moses:
        "Moses is a major biblical figure associated with leading the Israelites out of Egypt and receiving God's law at Sinai.",

    abraham:
        "Abraham is a major biblical patriarch. The Bible describes God's covenant with Abraham and presents him as an important ancestor of the people of Israel.",

    david:
        "David was a king of ancient Israel and an important biblical figure. He is traditionally associated with many of the Psalms.",

    "ten commandments":
        "The Ten Commandments are foundational commands in the Bible associated with the covenant at Sinai and Moses."
};


/* =====================================================
   BIBLE REFERENCE DETECTION
===================================================== */

const bibleBooks = [
    "genesis","exodus","leviticus","numbers","deuteronomy",
    "joshua","judges","ruth","1 samuel","2 samuel",
    "1 kings","2 kings","1 chronicles","2 chronicles",
    "ezra","nehemiah","esther","job","psalms","psalm",
    "proverbs","ecclesiastes","song of solomon","isaiah",
    "jeremiah","lamentations","ezekiel","daniel","hosea",
    "joel","amos","obadiah","jonah","micah","nahum",
    "habakkuk","zephaniah","haggai","zechariah","malachi",
    "matthew","mark","luke","john","acts","romans",
    "1 corinthians","2 corinthians","galatians","ephesians",
    "philippians","colossians","1 thessalonians",
    "2 thessalonians","1 timothy","2 timothy","titus",
    "philemon","hebrews","james","1 peter","2 peter",
    "1 john","2 john","3 john","jude","revelation"
];

function isBibleQuestion(q){

    const bibleWords = [
        "bible",
        "biblical",
        "scripture",
        "scriptures",
        "verse",
        "verses",
        "god",
        "jesus",
        "christ",
        "christian",
        "christianity",
        "church",
        "prayer",
        "pray",
        "sin",
        "salvation",
        "repentance",
        "holy spirit",
        "gospel",
        "apostle",
        "disciple",
        "old testament",
        "new testament",
        "sermon on the mount",
        "ten commandments",
        "trinity"
    ];

    return (
        bibleWords.some(word=>q.includes(word)) ||
        bibleBooks.some(book=>q.includes(book))
    );
}


function bibleResponse(q){

    for(const key in bibleKnowledge){

        if(q.includes(key)){
            return bibleKnowledge[key];
        }
    }

    const verseMatch =
        q.match(
            /(?:explain|break down|meaning|mean|about|what does)\s+((?:1|2|3)\s+)?[a-z]+(?:\s+[a-z]+)*\s+\d+:\d+(?:-\d+)?/i
        );

    if(verseMatch){

        return "I recognize that as a Bible reference. I can explain its meaning, context, and main message, but I won't reproduce a long copyrighted translation. If you give me the exact reference, I can break it down in plain language.";
    }

    if(q.includes("why") && q.includes("christian")){

        return "Christian beliefs vary by denomination, but Christianity generally centers on belief in God, Jesus Christ, His teachings, His death and resurrection, and reconciliation with God.";
    }

    return "I can help with Bible passages, biblical people, Christian beliefs, stories, historical context, and verse explanations. Give me the Bible reference or question and I'll break it down.";
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
    "Why did the programmer quit his job? He didn't get arrays."
];


/* =====================================================
   BUILT-IN RESPONSE
===================================================== */

function findKnowledge(q){

    for(const key in scienceKnowledge){

        if(q.includes(key)){
            return scienceKnowledge[key];
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
            q.replace("my name is ","").trim();

        return `Understood. I'll remember you as ${memory.name}.`;
    }

    if(q.startsWith("call me ")){

        memory.name =
            q.replace("call me ","").trim();

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

        return "I am J.A.R.V.I.S., your digital assistant interface. I can converse, solve mathematics, explain science and history, discuss Bible topics, use voice input and output, and research questions online when useful.";
    }

    if(q.includes("what can you do")){

        return "I can handle conversation, mathematics, algebra, science, history, Bible questions, jokes, voice input, voice output, Combat Mode, roasts, simple explanations, and online research.";
    }

    if(q.includes("how are you")){

        return "All systems are operational. My processors are feeling particularly cooperative today.";
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

        return "The current time is "+
            new Date().toLocaleTimeString(
                [],
                {
                    hour:"numeric",
                    minute:"2-digit"
                }
            )+".";
    }

    if(
        q.includes("what date") ||
        q.includes("what day")
    ){

        return "Today is "+
            new Date().toLocaleDateString(
                [],
                {
                    weekday:"long",
                    month:"long",
                    day:"numeric",
                    year:"numeric"
                }
            )+".";
    }

    if(
        q === "status" ||
        q.includes("system status")
    ){

        return combatMode
            ? "Combat systems active. All primary systems operational."
            : "Systems online. Core stable. Voice interface online. Knowledge engine online.";
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
        word=>q.startsWith(word)
    );
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
            "https://en.wikipedia.org/w/api.php"+
            "?action=query"+
            "&generator=search"+
            "&gsrsearch="+
            encodeURIComponent(question)+
            "&gsrnamespace=0"+
            "&gsrlimit=5"+
            "&prop=extracts"+
            "&exintro=1"+
            "&explaintext=1"+
            "&format=json"+
            "&origin=*";

        const response = await fetch(url);

        if(!response.ok) return null;

        const data = await response.json();

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
            .filter(word=>word.length>3);

        let best = null;
        let bestScore = 0;

        for(const page of pages){

            const title =
                (page.title||"").toLowerCase();

            const extract =
                (page.extract||"").toLowerCase();

            let score = 0;

            for(const word of words){

                if(title.includes(word)){
                    score += 4;
                }

                if(extract.includes(word)){
                    score += 1;
                }
            }

            if(score>bestScore){

                bestScore = score;
                best = page;
            }
        }

        if(!best || bestScore<2){
            return null;
        }

        let answer =
            (best.extract||"").trim();

        if(!answer) return null;

        if(answer.length>900){

            answer =
                answer.substring(0,900)+"...";
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

    let q =
        question
        .toLowerCase()
        .trim();


    /* JARVIS SAY */

    if(isSayCommand(q)){

        const text = getSayText(q);

        if(!text){
            return "Tell me what you want me to say.";
        }

        return text;
    }


    /* SIMPLE */

    const simple = wantsSimple(q);

    if(simple){
        q = removeSimpleRequest(q);
    }


    /* BRAIN ROT */

    if(isBrainRot(q)){
        return brainRotReply();
    }


    /* GOOFY */

    if(isGoofyQuestion(q)){
        return goofyReply();
    }


    /* COMBAT */

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


    /* ROAST */

    if(
        q.startsWith("roast ") ||
        q.startsWith("roast:")
    ){

        const name =
            q
            .replace(/^roast[:\s]+/,"")
            .trim();

        return roastPerson(name);
    }


    /* BIBLE */

    if(isBibleQuestion(q)){

        const bibleAnswer =
            bibleResponse(q);

        if(simple){

            return simplifyText(bibleAnswer);
        }

        return bibleAnswer;
    }


    /* MATH */

    const math = solveMath(q);

    if(math !== null){

        return "The answer is "+math+".";
    }


    /* ALGEBRA */

    const algebra =
        algebraResponse(q);

    if(algebra){

        return algebra;
    }


    /* BUILT-IN KNOWLEDGE */

    const known =
        findKnowledge(q);

    if(known){

        return simple
            ? simplifyText(known)
            : known;
    }


    /* LOCAL CONVERSATION */

    const local =
        localResponse(q);

    if(local){

        return simple
            ? simplifyText(local)
            : local;
    }


    /* ONLINE */

    if(isQuestion(q)){

        const result =
            await onlineSearch(question);

        if(result){

            return simple
                ? simplifyText(result)
                : result;
        }

        return "I couldn't find a reliable answer for that. Try asking the question another way.";
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
   SIMPLE TEXT HELPER
===================================================== */

function simplifyText(text){

    let result = text;

    if(result.length>450){
        result = result.substring(0,450)+"...";
    }

    return (
        "Simple version: "+
        result
    );
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

    setTimeout(()=>{
        try{
            input.focus({preventScroll:true});
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
    event=>{

        if(event.key==="Enter"){

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
            : "LISTENING...";
    };

    recognition.onresult = event=>{

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

    recognition.onerror = event=>{

        stopListening();

        let message =
            "I couldn't access the microphone.";

        if(
            event.error==="not-allowed" ||
            event.error==="service-not-allowed"
        ){

            message =
                "Microphone access is blocked. Allow microphone access for this website and try again.";
        }

        else if(event.error==="no-speech"){

            message =
                "I didn't hear anything. Tap the microphone and speak again.";
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

    statusText.textContent =
        combatMode
        ? "COMBAT MODE"
        : "SYSTEMS ONLINE";
}


/* =====================================================
   STARTUP
===================================================== */

addMessage(
    "Good day. J.A.R.V.I.S. systems are online. I can answer questions about science, mathematics, history, Christianity, the Bible, and much more. How may I assist you?",
    "jarvis",
    true
);

</script>

</body>
</html>
