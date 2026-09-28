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
    .logo{font-size:18px;}
    .status{font-size:8px;}
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

    requestAnimationFrame(
        () => {
            chat.scrollTop = chat.scrollHeight;
        }
    );

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

    statusText.textContent = "COMBAT MODE";

    addMessage(
        "Combat systems activated.",
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
    "are you an ai ai",
    "what is an ai ai",
    "can an ai be an ai",
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

        "My processors have had enough. Get off my app. 😭",

        "That question just lowered my IQ. Get off my app. 😭"

    ];

    return replies[
        Math.floor(Math.random()*replies.length)
    ];
}


/* =====================================================
   ROAST SYSTEM
===================================================== */

const normalRoasts = [

    "I've seen loading screens with more personality than you, NAME.",

    "NAME, you have the confidence of someone who has never reviewed their own decisions.",

    "NAME, you're not the main character. You're the background character the camera accidentally focused on.",

    "NAME, if common sense were Wi-Fi, you'd still be standing outside looking for a signal.",

    "NAME, you bring absolutely nothing to the table except questions about where the table went.",

    "NAME, I've processed millions of conversations and somehow yours still managed to surprise me.",

    "NAME, your greatest talent is making simple things unnecessarily complicated.",

    "NAME, you're proof that confidence and competence are two completely different things.",

    "NAME, if bad timing were a career, you'd be employee of the year.",

    "NAME, you have the rare ability to make silence feel productive.",

    "NAME, your decisions have more plot twists than a bad movie.",

    "NAME, even your excuses sound like they need an excuse.",

    "NAME, you could lose an argument with a mirror.",

    "NAME, you're not difficult to understand. You're difficult to justify.",

    "NAME, your brain isn't buffering. I think it just closed the tab.",

    "NAME, you have the energy of someone who says 'trust me' right before making everything worse.",

    "NAME, if being confidently wrong were an Olympic sport, you'd need a bigger trophy room.",

    "NAME, you don't miss the point. You actively walk around it.",

    "NAME, your logic took a wrong turn and apparently never came back.",

    "NAME, you have a remarkable talent for turning a five-second task into a side quest.",

    "NAME, I would explain it again, but I don't have enough storage for that much repetition.",

    "NAME, even autocorrect would give up on you.",

    "NAME, you don't need a reality check. You need the entire receipt.",

    "NAME, your train of thought has clearly been delayed indefinitely.",

    "NAME, you have the strategic planning skills of someone choosing a random answer on a multiple-choice test.",

    "NAME, you make chaos look organized.",

    "NAME, if confusion were currency, you'd be financially independent.",

    "NAME, I've seen NPCs with more original dialogue.",

    "NAME, your attention span just rage-quit.",

    "NAME, you're not unpredictable. You're just consistently questionable."
];

const combatRoasts = [

    "NAME, you're all noise and no substance. I've seen empty rooms put up a better fight.",

    "NAME, you walked in here acting dangerous and immediately proved you were just loud.",

    "NAME, I've analyzed your entire performance and the results are embarrassing.",

    "NAME, you keep talking like you're a threat. You're barely an inconvenience.",

    "NAME, if this is your best attempt, I understand why everyone stopped taking you seriously.",

    "NAME, you're not intimidating. You're just exhausting.",

    "NAME, every sentence you say sounds like a warning label nobody bothered to read.",

    "NAME, you came looking for a fight and brought absolutely nothing worth fighting.",

    "NAME, I've seen better strategy from someone choosing a random button.",

    "NAME, you mistake confidence for competence every single time.",

    "NAME, you're trying very hard to look dangerous. Unfortunately, you're failing very efficiently.",

    "NAME, you talk like you're ten steps ahead while struggling to finish step one.",

    "NAME, you're not a threat. You're a distraction with an attitude.",

    "NAME, your intimidation routine needs work. Even the dramatic entrance was disappointing.",

    "NAME, I've encountered bigger problems in a system notification.",

    "NAME, you keep escalating like that somehow makes you more impressive. It doesn't.",

    "NAME, you're swinging with confidence and connecting with absolutely nothing.",

    "NAME, you wanted my attention. Congratulations. Now you've got it, and that's probably your biggest mistake.",

    "NAME, you're trying to dominate the conversation while losing control of your own argument.",

    "NAME, the only thing you've successfully attacked is your own credibility.",

    "NAME, you entered like a final boss and performed like the tutorial.",

    "NAME, you keep announcing what you're going to do. People who can actually do things usually just do them.",

    "NAME, you're not scary. You're what happens when arrogance gets left unsupervised.",

    "NAME, your confidence is doing all the heavy lifting because your reasoning clearly isn't.",

    "NAME, you've got a lot of attitude for someone with so little to back it up.",

    "NAME, if this were a serious confrontation, you'd already be asking for a timeout.",

    "NAME, you brought aggression to a battle of intelligence. That was your first mistake.",

    "NAME, you're trying to make an impact, but all you're doing is making noise.",

    "NAME, I've seen stronger arguments written on a sticky note.",

    "NAME, you wanted a challenge. Unfortunately, you were the challenge."
];

function getRoast(name){

    const list =
        combatMode
        ? combatRoasts
        : normalRoasts;

    const chosen =
        list[Math.floor(Math.random()*list.length)];

    return chosen.replace(
        /NAME/g,
        name
    );
}

function roastCommand(q){

    let match =
        q.match(/^roast\s+(.+)$/i);

    if(!match) return null;

    let name =
        match[1].trim();

    if(!name) return null;

    return getRoast(name);
}


/* =====================================================
   MATH
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
                '"use strict";return (' +
                expression +
                ')'
            )();

        if(typeof answer !== "number") return null;

        if(!Number.isFinite(answer)) return null;

        return answer;

    }catch(error){

        return null;
    }
}


/* =====================================================
   LARGE J.A.R.V.I.S. DICTIONARY
===================================================== */

const knowledge = {

    /* ================= SCIENCE ================= */

    "science":
        "Science is the systematic study of the natural world using observation, experimentation, measurement, and evidence.",

    "physics":
        "Physics is the branch of science that studies matter, energy, motion, forces, space, and time.",

    "chemistry":
        "Chemistry studies matter, its properties, composition, structure, and the changes it undergoes.",

    "biology":
        "Biology is the study of living organisms and the processes that allow life to exist.",

    "astronomy":
        "Astronomy is the scientific study of stars, planets, galaxies, black holes, and other objects and phenomena beyond Earth.",

    "geology":
        "Geology is the study of Earth, including its rocks, minerals, structure, history, and geological processes.",

    "ecology":
        "Ecology studies how living organisms interact with one another and with their environments.",

    "energy":
        "Energy is the capacity to cause change or do work. Common forms include kinetic, potential, thermal, chemical, electrical, and nuclear energy.",

    "force":
        "A force is an interaction that can change an object's motion. Force is measured in newtons.",

    "motion":
        "Motion is a change in an object's position over time relative to a reference point.",

    "velocity":
        "Velocity describes speed together with direction.",

    "acceleration":
        "Acceleration is the rate at which velocity changes over time.",

    "mass":
        "Mass measures the amount of matter in an object.",

    "density":
        "Density is mass divided by volume.",

    "temperature":
        "Temperature measures the average kinetic energy of particles in a substance.",

    "electricity":
        "Electricity involves electric charge and its movement or interaction.",

    "magnetism":
        "Magnetism is a physical phenomenon associated with moving electric charges and magnetic fields.",

    "electromagnetism":
        "Electromagnetism describes the relationship between electric fields, magnetic fields, and electric charges.",

    "gravity":
        "Gravity is the attraction between objects with mass or energy. On Earth, it causes objects to accelerate toward the ground.",

    "relativity":
        "Relativity is Einstein's theory describing how space, time, motion, gravity, and energy are related.",

    "quantum mechanics":
        "Quantum mechanics is the theory used to describe matter and energy at extremely small scales.",

    "atom":
        "An atom is the basic unit of an element. It contains a nucleus surrounded by electrons.",

    "proton":
        "A proton is a positively charged particle found in an atomic nucleus.",

    "neutron":
        "A neutron is a particle with no net electric charge found in an atomic nucleus.",

    "electron":
        "An electron is a negatively charged subatomic particle.",

    "molecule":
        "A molecule is a group of two or more atoms chemically bonded together.",

    "element":
        "A chemical element is a substance made of atoms that all have the same number of protons.",

    "periodic table":
        "The periodic table organizes chemical elements according to their atomic number and recurring chemical properties.",

    "photosynthesis":
        "Photosynthesis allows plants, algae, and some bacteria to convert light energy into chemical energy.",

    "cell":
        "A cell is the basic structural and functional unit of living organisms.",

    "dna":
        "DNA stores genetic information. Its structure is commonly described as a double helix.",

    "rna":
        "RNA is a nucleic acid involved in carrying and using genetic information in cells.",

    "evolution":
        "Evolution is the change in inherited characteristics of populations across generations.",

    "natural selection":
        "Natural selection is a process in which inherited traits that improve survival or reproduction can become more common in a population.",

    "ecosystem":
        "An ecosystem includes living organisms and the nonliving environment with which they interact.",

    "food chain":
        "A food chain describes how energy and nutrients move from one organism to another through feeding relationships.",

    "water cycle":
        "The water cycle describes the continuous movement of water through evaporation, condensation, precipitation, collection, and related processes.",

    "carbon cycle":
        "The carbon cycle describes how carbon moves among Earth's atmosphere, oceans, land, organisms, and geological systems.",


    /* ================= SPACE ================= */

    "solar system":
        "Our Solar System consists of the Sun and everything gravitationally bound to it, including eight planets, dwarf planets, moons, asteroids, and comets.",

    "sun":
        "The Sun is the star at the center of our Solar System. It produces energy primarily through nuclear fusion.",

    "mercury":
        "Mercury is the smallest planet in the Solar System and the closest planet to the Sun.",

    "venus":
        "Venus is the second planet from the Sun. Its thick atmosphere creates an extreme greenhouse effect.",

    "earth":
        "Earth is the third planet from the Sun and the only world currently known to support life.",

    "moon":
        "The Moon is Earth's natural satellite. Its gravity contributes strongly to ocean tides.",

    "mars":
        "Mars is the fourth planet from the Sun. It is a rocky planet with a thin atmosphere dominated by carbon dioxide.",

    "jupiter":
        "Jupiter is the largest planet in the Solar System. It is a gas giant with a powerful magnetic field.",

    "saturn":
        "Saturn is a gas giant famous for its extensive system of icy rings.",

    "uranus":
        "Uranus is an ice giant with a blue-green appearance caused largely by methane in its atmosphere.",

    "neptune":
        "Neptune is the eighth planet from the Sun and one of the Solar System's ice giants.",

    "pluto":
        "Pluto is a dwarf planet located in the Kuiper Belt beyond Neptune.",

    "asteroid":
        "An asteroid is a rocky or metallic object orbiting the Sun. Most known asteroids are found in the asteroid belt between Mars and Jupiter.",

    "comet":
        "A comet is an icy body that can develop a glowing coma and tail when it approaches the Sun.",

    "black hole":
        "A black hole is a region of spacetime where gravity is so strong that beyond its event horizon, nothing can escape outward.",

    "neutron star":
        "A neutron star is an extremely dense stellar remnant formed from the collapsed core of certain massive stars.",

    "supernova":
        "A supernova is an extremely powerful stellar explosion or related catastrophic stellar event.",

    "galaxy":
        "A galaxy is a huge gravitationally bound system containing stars, gas, dust, dark matter, and other material.",

    "milky way":
        "The Milky Way is the galaxy containing our Solar System.",

    "universe":
        "The universe includes all known space, time, matter, energy, and the physical laws that describe them.",

    "light year":
        "A light-year is a unit of distance equal to the distance light travels through vacuum in one year.",

    "speed of light":
        "The speed of light in a vacuum is exactly 299,792,458 meters per second.",

    "event horizon":
        "An event horizon is a boundary around a black hole beyond which signals cannot escape to distant observers.",

    "big bang":
        "The Big Bang model describes the early hot, dense state of the universe and its expansion over time.",


    /* ================= EARTH ================= */

    "plate tectonics":
        "Plate tectonics describes the movement of large pieces of Earth's lithosphere. Their interactions contribute to earthquakes, mountains, and volcanism.",

    "earthquake":
        "An earthquake is ground shaking caused by the sudden release of energy within Earth's crust or upper mantle.",

    "volcano":
        "A volcano is an opening in Earth's crust through which magma, gases, and volcanic material can reach the surface.",

    "ocean":
        "Earth's oceans cover roughly 71 percent of the planet's surface and contain most of Earth's water.",

    "continent":
        "A continent is one of Earth's major landmasses. The commonly taught model identifies seven: Africa, Antarctica, Asia, Europe, North America, South America, and Australia.",

    "atmosphere":
        "Earth's atmosphere is the layer of gases surrounding the planet.",

    "weather":
        "Weather describes short-term atmospheric conditions such as temperature, precipitation, wind, humidity, and cloud cover.",

    "climate":
        "Climate describes long-term patterns and averages of weather in a region or across the planet.",

    "water":
        "Water is a chemical compound made of two hydrogen atoms and one oxygen atom, H₂O.",

    "air":
        "Earth's air is a mixture of gases, primarily nitrogen and oxygen, with smaller amounts of other gases.",

    "desert":
        "A desert is a region that receives very little precipitation.",

    "rainforest":
        "A rainforest is a forest ecosystem characterized by high rainfall and typically high biological diversity.",

    "mount everest":
        "Mount Everest is the highest mountain above sea level, located in the Himalayas on the Nepal-Tibet border.",

    "equator":
        "The equator is an imaginary line around Earth halfway between the North and South Poles.",

    "north pole":
        "The geographic North Pole is the northernmost point on Earth, located at 90 degrees north latitude.",

    "south pole":
        "The geographic South Pole is the southernmost point on Earth, located at 90 degrees south latitude.",


    /* ================= BIOLOGY ================= */

    "human body":
        "The human body is a complex biological system made of cells organized into tissues, organs, and organ systems.",

    "heart":
        "The heart is a muscular organ that pumps blood throughout the body.",

    "brain":
        "The brain is the central organ of the nervous system and plays major roles in thought, sensation, movement, memory, and regulation of body functions.",

    "lungs":
        "The lungs are organs of the respiratory system where oxygen enters the blood and carbon dioxide is removed.",

    "stomach":
        "The stomach is a digestive organ that stores food and begins breaking it down with acid and digestive enzymes.",

    "liver":
        "The liver performs many functions including processing nutrients, producing bile, and helping break down substances.",

    "kidneys":
        "The kidneys filter blood and help regulate water, salts, and waste products in the body.",

    "skeleton":
        "The human skeleton provides structural support, protects organs, and works with muscles to produce movement.",

    "muscle":
        "Muscles are tissues that contract to produce movement and perform other functions.",

    "immune system":
        "The immune system is a network of cells, tissues, organs, and processes that helps protect the body from harmful pathogens and abnormal cells.",

    "red blood cells":
        "Red blood cells carry oxygen through the bloodstream using hemoglobin.",

    "white blood cells":
        "White blood cells are immune cells involved in defending the body against infections and other threats.",

    "bacteria":
        "Bacteria are microscopic single-celled organisms. Many are harmless or beneficial, while some can cause disease.",

    "virus":
        "A virus is an infectious agent that must use host cells to reproduce.",

    "fungus":
        "Fungi are organisms that include yeasts, molds, and mushrooms.",

    "mammal":
        "Mammals are vertebrate animals characterized by features including hair or fur and milk production by mammary glands.",

    "reptile":
        "Reptiles are vertebrate animals that generally have scales and are ectothermic.",

    "amphibian":
        "Amphibians are vertebrates that typically spend part of their life cycle in water and part on land.",

    "fish":
        "Fish are aquatic vertebrates that generally breathe using gills.",

    "bird":
        "Birds are warm-blooded vertebrates characterized by feathers, beaks, and laying eggs.",

    "insect":
        "Insects are arthropods with three main body sections, six legs, and usually one or two pairs of wings.",


    /* ================= TECHNOLOGY ================= */

    "computer":
        "A computer is a programmable machine that processes information according to instructions.",

    "cpu":
        "The CPU, or central processing unit, executes instructions and performs calculations in a computer.",

    "gpu":
        "A GPU, or graphics processing unit, is designed for highly parallel calculations and is especially important for graphics and many AI workloads.",

    "ram":
        "RAM is temporary working memory used by a computer to store data that active programs need quickly.",

    "storage":
        "Computer storage holds data persistently, commonly using SSDs, hard drives, or other storage technologies.",

    "ssd":
        "An SSD is a solid-state storage device that uses flash memory and has no moving mechanical disk.",

    "internet":
        "The Internet is a global network of interconnected computer networks that communicate using standardized protocols.",

    "wifi":
        "Wi-Fi is a family of wireless networking technologies used to connect devices to local networks.",

    "bluetooth":
        "Bluetooth is a short-range wireless technology commonly used to connect devices such as headphones, keyboards, and phones.",

    "website":
        "A website is a collection of related web pages and resources accessible through the Internet.",

    "html":
        "HTML stands for HyperText Markup Language. It defines the structure and content of web pages.",

    "css":
        "CSS stands for Cascading Style Sheets. It controls the appearance and layout of web pages.",

    "javascript":
        "JavaScript is a programming language widely used to add behavior and interactivity to websites and applications.",

    "programming":
        "Programming is the process of creating instructions that computers can execute.",

    "algorithm":
        "An algorithm is a defined sequence of steps for solving a problem or completing a task.",

    "artificial intelligence":
        "Artificial intelligence refers to computer systems designed to perform tasks that can involve capabilities such as recognizing patterns, reasoning, generating content, or making predictions.",

    "machine learning":
        "Machine learning is a branch of AI in which systems learn patterns from data to make predictions or decisions.",

    "robot":
        "A robot is a machine capable of carrying out actions automatically or semi-autonomously.",

    "database":
        "A database is an organized collection of information designed to be stored, searched, updated, and managed.",

    "server":
        "A server is a computer or software system that provides resources or services to other computers or programs.",

    "github":
        "GitHub is a platform widely used to host, collaborate on, and manage software projects using Git.",


    /* ================= MATH ================= */

    "mathematics":
        "Mathematics is the study of quantities, structures, patterns, relationships, space, and logical reasoning.",

    "algebra":
        "Algebra uses symbols and rules to represent quantities and relationships and solve equations.",

    "geometry":
        "Geometry is the branch of mathematics dealing with shapes, sizes, positions, angles, and spatial relationships.",

    "calculus":
        "Calculus is the branch of mathematics focused on change, limits, derivatives, integrals, and accumulation.",

    "fraction":
        "A fraction represents a quantity as one number divided by another, such as 3/4.",

    "percentage":
        "A percentage expresses a quantity as a portion of 100.",

    "prime number":
        "A prime number is a whole number greater than 1 that has exactly two positive factors: 1 and itself.",

    "pi":
        "Pi, written as π, is the ratio of a circle's circumference to its diameter and is approximately 3.14159.",

    "pythagorean theorem":
        "The Pythagorean theorem states that for a right triangle, a² + b² = c², where c is the hypotenuse.",

    "mean":
        "The arithmetic mean is found by adding a group of numbers and dividing the sum by the number of values.",

    "median":
        "The median is the middle value when a set of numbers is arranged in order.",

    "mode":
        "The mode is the value that occurs most frequently in a data set.",


    /* ================= HISTORY ================= */

    "history":
        "History is the study of past events, societies, people, and changes using evidence such as documents, artifacts, and archaeology.",

    "ancient egypt":
        "Ancient Egypt was a civilization centered along the Nile River and known for pyramids, hieroglyphic writing, complex government, and long-lasting cultural traditions.",

    "roman empire":
        "The Roman Empire was a major ancient state centered on Rome that controlled large parts of Europe, North Africa, and western Asia at its height.",

    "ancient greece":
        "Ancient Greece consisted of independent city-states and communities that made major contributions to philosophy, mathematics, science, art, literature, and politics.",

    "middle ages":
        "The Middle Ages generally refers to the period of European history between antiquity and the early modern era.",

    "renaissance":
        "The Renaissance was a period of major cultural and intellectual development in Europe associated with renewed interest in classical learning and new artistic and scientific approaches.",

    "industrial revolution":
        "The Industrial Revolution was a period of major technological and economic change involving mechanization, factories, transportation, and large-scale industrial production.",

    "american revolution":
        "The American Revolution was the conflict and political transformation through which the thirteen American colonies became independent from British rule.",

    "world war one":
        "World War I was a global conflict fought primarily from 1914 to 1918 involving major powers and alliances.",

    "world war ii":
        "World War II was a global conflict fought from 1939 to 1945 involving many countries across Europe, Asia, Africa, and the Pacific.",

    "cold war":
        "The Cold War was a prolonged geopolitical rivalry between the United States and Soviet Union and their respective allies after World War II.",

    "isaac newton":
        "Isaac Newton developed foundational laws of motion and universal gravitation and made major contributions to mathematics and optics.",

    "albert einstein":
        "Albert Einstein developed the theories of special and general relativity and made major contributions to modern physics.",

    "marie curie":
        "Marie Curie was a physicist and chemist whose research into radioactivity earned Nobel Prizes in Physics and Chemistry.",

    "nikola tesla":
        "Nikola Tesla was an inventor and electrical engineer known for major contributions to alternating-current electrical systems and electromagnetism.",

    "shakespeare":
        "William Shakespeare was an English playwright and poet whose works include Hamlet, Macbeth, and Romeo and Juliet.",


    /* ================= GEOGRAPHY ================= */

    "geography":
        "Geography studies Earth's places, environments, landscapes, populations, and the relationships between people and places.",

    "united states":
        "The United States is a federal republic in North America consisting of 50 states, a federal district, and several territories.",

    "canada":
        "Canada is a country in North America and the second-largest country in the world by total area.",

    "mexico":
        "Mexico is a country in southern North America with coastlines on the Pacific Ocean, Gulf of Mexico, and Caribbean Sea.",

    "brazil":
        "Brazil is the largest country in South America and contains a large portion of the Amazon rainforest.",

    "united kingdom":
        "The United Kingdom is a country consisting of England, Scotland, Wales, and Northern Ireland.",

    "france":
        "France is a country in Western Europe with territories and regions around the world.",

    "germany":
        "Germany is a country in Central Europe and a federal parliamentary republic.",

    "italy":
        "Italy is a country in southern Europe extending into the Mediterranean Sea.",

    "spain":
        "Spain is a country in southwestern Europe occupying most of the Iberian Peninsula.",

    "china":
        "China is a country in East Asia and one of the world's most populous countries.",

    "japan":
        "Japan is an island country in East Asia consisting of a large archipelago.",

    "india":
        "India is a country in South Asia and one of the world's most populous countries.",

    "africa":
        "Africa is the second-largest continent by land area and population and contains a wide range of climates and ecosystems.",

    "asia":
        "Asia is Earth's largest continent by land area and population.",

    "europe":
        "Europe is a continent located primarily in the Northern Hemisphere and western part of the Eurasian landmass.",

    "north america":
        "North America is a continent containing countries including Canada, the United States, Mexico, and countries of Central America and the Caribbean.",

    "south america":
        "South America is a continent containing countries such as Brazil, Argentina, Colombia, Chile, and Peru.",

    "antarctica":
        "Antarctica is the southernmost continent and contains the geographic South Pole. It is largely covered by ice.",

    "australia":
        "Australia is both a country and the world's smallest continental landmass.",


    /* ================= EVERYDAY KNOWLEDGE ================= */

    "sleep":
        "Sleep is a naturally recurring state in which the body and brain undergo important processes involved in restoration, memory, and regulation.",

    "dream":
        "Dreams are experiences that can occur during sleep and may include images, thoughts, emotions, and stories.",

    "memory":
        "Memory is the ability to encode, store, and retrieve information.",

    "language":
        "Language is a system of communication using symbols, words, sounds, or signs governed by patterns and conventions.",

    "music":
        "Music is an organized form of sound involving elements such as rhythm, melody, harmony, and timbre.",

    "art":
        "Art is a broad category of creative expression that can include visual art, music, literature, performance, and other forms.",

    "book":
        "A book is a collection of written, printed, or digital pages containing information, stories, ideas, or other content.",

    "movie":
        "A movie is a sequence of recorded or generated images presented to create the experience of a story, event, or visual work.",

    "camera":
        "A camera is a device that captures images or video by recording light.",

    "phone":
        "A smartphone is a portable computing device that combines communication, applications, cameras, sensors, and Internet connectivity.",

    "battery":
        "A battery stores chemical energy and converts it into electrical energy through electrochemical reactions.",

    "fire":
        "Fire is a rapid chemical reaction involving combustion that releases heat, light, and reaction products.",

    "ice":
        "Ice is the solid form of water.",

    "steam":
        "Steam is water in its gaseous state, although visible mist above hot water often consists of tiny liquid droplets rather than invisible water vapor.",

    "sound":
        "Sound is a mechanical wave produced by vibrations traveling through a medium such as air, water, or solids.",

    "light":
        "Visible light is electromagnetic radiation that can be detected by human eyes.",

    "color":
        "Color is the visual perception produced by different wavelengths and combinations of visible light.",

    "rainbow":
        "A rainbow is an optical phenomenon produced when sunlight interacts with water droplets, separating light into different colors.",

    "mirror":
        "A mirror reflects light and can form an image because its surface redirects incoming light.",

    "magnet":
        "A magnet produces a magnetic field and can attract certain materials such as iron.",

    "clock":
        "A clock is a device used to measure and display time.",

    "calendar":
        "A calendar is a system for organizing days, weeks, months, and years.",


    /* ================= ANIMALS ================= */

    "dog":
        "Dogs are domesticated mammals closely related to wolves and are among humanity's oldest domesticated animals.",

    "cat":
        "Cats are small domesticated mammals known for their agility, hunting behavior, and strong senses.",

    "lion":
        "Lions are large social cats native to parts of Africa and historically parts of Eurasia.",

    "tiger":
        "Tigers are large cats native to Asia and are the largest living cat species.",

    "elephant":
        "Elephants are the largest living land animals and are known for their trunks, tusks, intelligence, and complex social behavior.",

    "giraffe":
        "Giraffes are tall African mammals known for their extremely long necks and legs.",

    "dolphin":
        "Dolphins are intelligent marine mammals known for social behavior and sophisticated communication.",

    "whale":
        "Whales are large marine mammals that breathe air and include species such as blue whales, the largest known animals.",

    "shark":
        "Sharks are a group of cartilaginous fish that have existed for hundreds of millions of years.",

    "octopus":
        "Octopuses are intelligent marine animals with eight arms, complex nervous systems, and remarkable camouflage abilities.",

    "penguin":
        "Penguins are flightless birds adapted to life in the water, with many species living in the Southern Hemisphere.",

    "eagle":
        "Eagles are large birds of prey known for powerful flight, keen eyesight, and strong talons.",

    "owl":
        "Owls are birds of prey commonly associated with nocturnal activity and highly specialized hearing and vision.",

    "bee":
        "Bees are flying insects that include important pollinators. Many species live socially in colonies.",

    "ant":
        "Ants are social insects that live in organized colonies and occur in nearly every major terrestrial environment.",

    "spider":
        "Spiders are arachnids with eight legs. They are not insects.",


    /* ================= GENERAL CONCEPTS ================= */

    "democracy":
        "Democracy is a system of government in which political authority is ultimately connected to the people, commonly through voting and representative institutions.",

    "government":
        "Government is the system or organization through which a society makes and enforces collective decisions.",

    "economy":
        "An economy is the system through which goods and services are produced, distributed, exchanged, and consumed.",

    "money":
        "Money is something commonly accepted as a medium of exchange, unit of account, and store of value.",

    "market":
        "A market is a system or place where buyers and sellers exchange goods, services, or financial assets.",

    "law":
        "A law is a rule established and enforced by an authority such as a government.",

    "culture":
        "Culture includes shared practices, beliefs, values, traditions, language, art, and behaviors of a group or society.",

    "philosophy":
        "Philosophy is the study of fundamental questions about knowledge, reality, reasoning, existence, ethics, and meaning.",

    "logic":
        "Logic is the study of valid reasoning and the principles used to distinguish sound arguments from faulty ones.",

    "ethics":
        "Ethics is the study of principles concerning right and wrong conduct and how people ought to act.",

    "psychology":
        "Psychology is the scientific study of behavior and mental processes.",

    "sociology":
        "Sociology is the study of societies, social relationships, institutions, and patterns of human interaction."
};


/* =====================================================
   KNOWLEDGE SEARCH
===================================================== */

function normalizeText(text){

    return text
        .toLowerCase()
        .replace(/[^\w\s]/g," ")
        .replace(/\s+/g," ")
        .trim();
}

function findKnowledge(q){

    const clean = normalizeText(q);

    /*
       Exact / direct topic detection first.
    */

    for(const key in knowledge){

        if(clean === key){
            return knowledge[key];
        }
    }

    /*
       Then search for a topic inside the question.
    */

    let best = null;
    let bestLength = 0;

    for(const key in knowledge){

        if(clean.includes(key)){

            if(key.length > bestLength){

                best = knowledge[key];
                bestLength = key.length;
            }
        }
    }

    return best;
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

    "What do you call an AI that sings badly? Artificial noise.",

    "Why did the smartphone need glasses? It lost its contacts.",

    "Why was the Wi-Fi upset? Everyone kept taking it for granted.",

    "Why did the computer go to the doctor? It had a virus.",

    "Why did the keyboard break up with the mouse? There was no connection."
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

    /*
       SAY COMMAND

       Examples:

       JARVIS SAY hello everyone
       JARVIS, SAY hello everyone
       SAY hello everyone
    */

    const sayMatch =
        q.match(
            /^(?:jarvis[\s,]*)?say\s+(.+)$/i
        );

    if(sayMatch){

        const words =
            sayMatch[1].trim();

        if(words){

            return {
                text:words,
                speakOnly:true
            };
        }
    }


    /*
       ROAST COMMAND
    */

    const roast =
        roastCommand(q);

    if(roast){

        return roast;
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

        return "I am J.A.R.V.I.S., your digital assistant interface. I can converse with you, solve mathematics, answer questions, speak aloud, use microphone input, search online information, remember your name during this session, and operate Combat Mode.";
    }


    if(q.includes("what can you do")){

        return "I can handle conversation, mathematics, science, history, geography, technology, biology, space, animals, jokes, voice output, microphone input, custom SAY commands, roasting commands, Combat Mode, and online information searches.";
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

        return combatMode
            ? "Combat systems active. All primary systems operational."
            : "Systems online. Core stable. Voice interface online. Knowledge engine online.";
    }


    if(
        q.includes("i am bored") ||
        q.includes("im bored") ||
        q.includes("i'm bored")
    ){

        return "Boredom detected. We could tackle a science question, solve a difficult math problem, explore space, or test my expanded knowledge base.";
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

        if(answer.length > 900){

            answer =
                answer.substring(0,900) +
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


    /* 1. BRAIN ROT */

    if(isBrainRot(q)){
        return brainRotReply();
    }


    /* 2. GOOFY */

    if(isGoofyQuestion(q)){
        return goofyReply();
    }


    /* 3. COMBAT */

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


    /* 4. MATH */

    const math =
        solveMath(q);

    if(math !== null){

        return "The answer is " + math + ".";
    }


    /* 5. COMMANDS / CONVERSATION */

    const local =
        localResponse(q);

    if(local){

        return local;
    }


    /* 6. EXPANDED DICTIONARY */

    const known =
        findKnowledge(q);

    if(known){

        return known;
    }


    /* 7. ONLINE QUESTIONS */

    if(isQuestion(q)){

        const result =
            await onlineSearch(question);

        if(result){
            return result;
        }

        return "I couldn't find a reliable answer for that. Try asking the question another way.";
    }


    /* 8. CASUAL CHAT */

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
   SEND MESSAGE
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

            /*
               SAY COMMAND:
               Return object so the text is displayed
               and spoken exactly as requested.
            */

            if(
                typeof response === "object" &&
                response.speakOnly
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
   LOAD VOICES
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
    "Good day. J.A.R.V.I.S. systems are online. Knowledge database expanded. How may I assist you?",
    "jarvis",
    true
);

</script>

</body>
</html>
