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
    padding:0 15px;
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

#commandButton{
    margin-left:12px;
    height:36px;
    padding:0 10px;
    border:1px solid #00eaff;
    border-radius:8px;
    background:rgba(0,234,255,.08);
    color:#00eaff;
    font-size:11px;
    font-weight:bold;
    cursor:pointer;
}

.combat #commandButton{
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
    padding:16px;
    background:rgba(0,10,15,.98);
    border:1px solid #00eaff;
    border-radius:12px;
    box-shadow:0 0 25px rgba(0,234,255,.25);
    z-index:1000;
    display:none;
}

#commandPanel.show{
    display:block;
}

#commandPanel h2{
    margin:0 0 12px;
    font-size:16px;
}

.command{
    padding:8px 0;
    border-bottom:1px solid rgba(0,234,255,.12);
    font-size:13px;
    line-height:1.4;
}

.command b{
    color:#fff;
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
    background:radial-gradient(
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
    background:radial-gradient(
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
    padding:5px 15px 120px;
    -webkit-overflow-scrolling:touch;
}

.message{
    max-width:900px;
    margin:0 auto 12px;
    padding:12px 15px;
    border-radius:10px;
    line-height:1.5;
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
}

#input:focus{
    border-color:#00eaff;
    box-shadow:0 0 12px rgba(0,234,255,.2);
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
    0%,100%{
        transform:translate(-50%,-50%) scale(.9);
    }
    50%{
        transform:translate(-50%,-50%) scale(1.1);
    }
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

    <div style="display:flex;align-items:center;">

        <div class="status">
            <span class="dot"></span>
            <span id="statusText">SYSTEMS ONLINE</span>
        </div>

        <button id="commandButton">
            COMMANDS
        </button>

    </div>

</header>

<div id="commandPanel">

    <h2>J.A.R.V.I.S. COMMANDS</h2>

    <div class="command">
        <b>Combat Mode</b><br>
        Activates combat mode.
    </div>

    <div class="command">
        <b>Normal Mode</b><br>
        Returns to normal mode.
    </div>

    <div class="command">
        <b>J.A.R.V.I.S. Say...</b><br>
        Makes J.A.R.V.I.S. say exactly what follows.
    </div>

    <div class="command">
        <b>J.A.R.V.I.S. Roast [name]</b><br>
        Gives the person a roast.
    </div>

    <div class="command">
        <b>J.A.R.V.I.S. Roast [name] Combat</b><br>
        Uses the harsher combat roast style.
    </div>

    <div class="command">
        <b>Simple</b><br>
        Put <b>simple</b> at the end of a question for an easy-to-understand explanation.
    </div>

    <div class="command">
        <b>My name is...</b><br>
        Stores your name for the conversation.
    </div>

    <div class="command">
        <b>Status</b><br>
        Shows system status.
    </div>

    <div class="command">
        <b>Joke</b><br>
        Tells a joke.
    </div>

    <div class="command">
        <b>Math</b><br>
        Handles arithmetic and many algebra problems.
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
const commandButton = document.getElementById("commandButton");
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

commandButton.addEventListener("click",() => {
    commandPanel.classList.toggle("show");
});


/* =====================================================
   VOICE
===================================================== */

function speak(text){

    if(!("speechSynthesis" in window)){
        return;
    }

    try{

        speechSynthesis.cancel();

        const utterance =
            new SpeechSynthesisUtterance(text);

        utterance.rate = .88;
        utterance.pitch = .72;
        utterance.volume = 1;

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
                    voice =>
                    voice.name
                    .toLowerCase()
                    .includes(name.toLowerCase())
                );

            if(selected) break;
        }

        if(!selected){

            selected =
                voices.find(
                    voice =>
                    voice.lang &&
                    voice.lang
                    .toLowerCase()
                    .startsWith("en")
                );
        }

        if(selected){
            utterance.voice = selected;
        }

        speechSynthesis.speak(utterance);

    }catch(error){
        console.log(error);
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

    statusText.textContent = "COMBAT MODE";

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

    statusText.textContent = "SYSTEMS ONLINE";

    addMessage(
        "Combat mode terminated. Systems returning to normal.",
        "jarvis",
        true
    );
}


/* =====================================================
   J.A.R.V.I.S. SAY
===================================================== */

function sayCommand(text){

    const match =
        text.match(
            /^(?:j\.?a\.?r\.?v\.?i\.?s\.?)\s+say\s+(.+)$/i
        );

    if(!match){
        return null;
    }

    const words = match[1].trim();

    if(!words){
        return "Tell me what you want me to say.";
    }

    return words;
}


/* =====================================================
   SIMPLE MODE
===================================================== */

function detectSimpleMode(text){

    const clean =
        text
        .trim()
        .replace(/[.!?]+$/g,"")
        .trim();

    if(/\bsimple$/i.test(clean)){
        return true;
    }

    return false;
}


function removeSimpleCommand(text){

    return text
        .trim()
        .replace(
            /\s+simple[.!?]*$/i,
            ""
        )
        .trim();
}


/*
   These instructions make the local engine deliberately
   explain things in an easier way when "simple" is used.
*/

function simplifyAnswer(answer){

    if(!answer) return answer;

    const replacements = [

        [
            "Photosynthesis is the process by which",
            "Photosynthesis is how"
        ],

        [
            "gravitational attraction",
            "gravity pulling things toward each other"
        ],

        [
            "electromagnetic radiation",
            "energy that travels through space, like light"
        ],

        [
            "organism",
            "living thing"
        ],

        [
            "approximately",
            "about"
        ],

        [
            "therefore",
            "so"
        ],

        [
            "however",
            "but"
        ],

        [
            "fundamental",
            "basic"
        ],

        [
            "velocity",
            "speed in a particular direction"
        ],

        [
            "hypothesis",
            "an idea that can be tested"
        ],

        [
            "ecosystem",
            "a community of living things and their environment"
        ],

        [
            "magnitude",
            "size"
        ],

        [
            "calculate",
            "figure out"
        ]
    ];

    let result = answer;

    replacements.forEach(pair => {
        result =
            result.replace(
                new RegExp(pair[0],"gi"),
                pair[1]
            );
    });

    return result;
}


/* =====================================================
   ROAST ENGINE
===================================================== */

const playfulRoasts = [

    "{name}, I've seen loading screens with more personality than you.",

    "{name}, you're not the main character. You're barely in the background.",

    "{name}, your confidence is doing way more work than your abilities.",

    "{name}, you bring absolutely nothing to the table except confusion.",

    "{name}, even autocorrect gives up when you start typing.",

    "{name}, you have the energy of someone who loses an argument with a search bar.",

    "{name}, I've heard smarter conversations from a broken alarm clock.",

    "{name}, you're proof that having an opinion and having a point are two different things.",

    "{name}, if common sense were Wi-Fi, you'd have no signal.",

    "{name}, you somehow make silence sound intelligent.",

    "{name}, your comebacks need a software update.",

    "{name}, I've seen NPCs with better dialogue.",

    "{name}, you could trip over a wireless connection.",

    "{name}, you're not useless, but the loading screen has a better chance of helping.",

    "{name}, your brain really said 'I'll improvise' and never recovered.",

    "{name}, you're the human equivalent of a typo.",

    "{name}, even your excuses sound unfinished.",

    "{name}, you talk like your thoughts are still buffering.",

    "{name}, your logic took a wrong turn and never came back.",

    "{name}, you have the confidence of a genius and the evidence of a potato.",

    "{name}, if being confused was a career, you'd be CEO.",

    "{name}, you make simple things look like advanced mathematics.",

    "{name}, your attention span has the stability of a notification popup.",

    "{name}, you somehow turn every conversation into a side quest.",

    "{name}, I've processed your argument. Unfortunately, there wasn't much to process.",

    "{name}, your greatest talent is making people appreciate mute buttons.",

    "{name}, you're not hard to understand. You're just hard to take seriously.",

    "{name}, your brain has unlimited storage and somehow still has no useful files.",

    "{name}, you don't need a comeback. You need a restart.",

    "{name}, you're living proof that confidence can exist without supporting evidence."
];


const combatRoasts = [

    "{name}, you walked in acting dangerous and immediately proved you were all talk.",

    "{name}, your entire attitude is built on confidence you haven't earned.",

    "{name}, you talk like you're intimidating, but nobody's buying it.",

    "{name}, I've heard tougher words from someone asking for permission.",

    "{name}, you keep trying to act superior while giving everyone reasons to laugh at you.",

    "{name}, you're not a threat. You're an inconvenience with an ego.",

    "{name}, every time you open your mouth, your own reputation takes damage.",

    "{name}, you mistake being loud for being respected.",

    "{name}, your attitude entered the room before your common sense did.",

    "{name}, you have the confidence of someone who has never been corrected.",

    "{name}, stop pretending you're intimidating. You're making this embarrassing.",

    "{name}, you're trying way too hard to look tough, and that's exactly why it isn't working.",

    "{name}, your ego is enormous for someone bringing so little to the conversation.",

    "{name}, you don't command attention. You demand patience.",

    "{name}, your biggest opponent isn't me. It's your own terrible judgment.",

    "{name}, you came looking for a fight and brought nothing worth fighting over.",

    "{name}, all that attitude and somehow still no substance.",

    "{name}, you're not feared. You're tolerated.",

    "{name}, you keep acting like the final boss when you're barely the tutorial.",

    "{name}, your threats have the impact of a notification nobody opens.",

    "{name}, you want respect without doing anything respectable.",

    "{name}, you're confusing aggression with strength, and everyone can see it.",

    "{name}, your mouth keeps writing checks your actions can't cash.",

    "{name}, you're trying to dominate a conversation you can't even control.",

    "{name}, you're not intimidating. You're just exhausting.",

    "{name}, the toughest thing about you is listening to you pretend you're tough.",

    "{name}, your entire strategy is attitude and somehow even that is failing.",

    "{name}, you act like everyone should fear you when most people are just waiting for you to finish talking.",

    "{name}, if arrogance were ability, you'd finally be impressive.",

    "{name}, you've got a lot of aggression for someone with so little substance."
];


function roastPerson(name,combat=false){

    name = name.trim();

    if(!name){
        return "Give me a name to roast.";
    }

    const list =
        combat
        ? combatRoasts
        : playfulRoasts;

    const template =
        list[
            Math.floor(
                Math.random() * list.length
            )
        ];

    return template.replace(
        /\{name\}/g,
        name
    );
}


function roastCommand(text){

    const match =
        text.match(
            /^(?:j\.?a\.?r\.?v\.?i\.?s\.?)\s+roast\s+(.+)$/i
        );

    if(!match){
        return null;
    }

    let target = match[1].trim();

    let requestedCombat =
        /\s+combat$/i.test(target);

    if(requestedCombat){
        target =
            target.replace(
                /\s+combat$/i,
                ""
            ).trim();
    }

    return roastPerson(
        target,
        combatMode || requestedCombat
    );
}


/* =====================================================
   MATH ENGINE
===================================================== */

function solveMath(text){

    let expression = text.toLowerCase();

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
        .replace(/×/g,"*")
        .replace(/÷/g,"/");

    expression =
        expression
        .replace(
            /[^0-9+\-*/().%\s]/g,
            ""
        )
        .trim();

    if(!expression) return null;

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
   KNOWLEDGE
===================================================== */

const knowledge = {

    "black hole":
        "A black hole is a region of space where gravity is extremely strong. Once something crosses the event horizon, it cannot escape back out.",

    "earth":
        "Earth is the third planet from the Sun and the only planet currently known to support life.",

    "sun":
        "The Sun is the star at the center of our Solar System. It produces its energy mainly through nuclear fusion.",

    "moon":
        "The Moon is Earth's natural satellite. Its gravity is one of the main causes of Earth's ocean tides.",

    "gravity":
        "Gravity is the attraction between objects that have mass. Earth's gravity pulls objects toward the planet.",

    "atom":
        "An atom is a basic unit of matter. It contains a nucleus made of protons and usually neutrons, with electrons around it.",

    "dna":
        "DNA stores genetic information used by living organisms. Its famous structure is a double helix.",

    "photosynthesis":
        "Photosynthesis is the process plants and some other organisms use to turn light energy into chemical energy. Plants generally use sunlight, water, and carbon dioxide to make glucose and release oxygen.",

    "evolution":
        "Evolution is the change in inherited characteristics of populations over generations.",

    "mars":
        "Mars is the fourth planet from the Sun. It is a rocky planet with a thin atmosphere mostly made of carbon dioxide.",

    "jupiter":
        "Jupiter is the largest planet in our Solar System. It is a gas giant with a powerful magnetic field.",

    "saturn":
        "Saturn is a gas giant famous for its large system of icy rings.",

    "venus":
        "Venus is the second planet from the Sun. Its thick atmosphere traps heat extremely efficiently.",

    "mercury":
        "Mercury is the smallest planet in our Solar System and the planet closest to the Sun.",

    "neutron star":
        "A neutron star is an extremely dense remnant left behind after certain massive stars collapse.",

    "speed of light":
        "The speed of light in a vacuum is exactly 299,792,458 meters per second.",

    "plate tectonics":
        "Plate tectonics describes the movement of large pieces of Earth's outer rocky layer. Their movement can cause earthquakes, volcanoes, and mountain building.",

    "newton":
        "Isaac Newton developed important laws describing motion and gravity and made major contributions to mathematics and optics.",

    "chemistry":
        "Chemistry is the study of matter, its properties, how it is structured, and how it changes.",

    "cell":
        "A cell is the basic structural and functional unit of living organisms.",

    "volcano":
        "A volcano is an opening in Earth's crust through which magma, gases, and volcanic material can reach the surface.",

    "ocean":
        "Earth's oceans cover about 71 percent of the planet's surface and contain most of Earth's water.",

    "mitosis":
        "Mitosis is a type of cell division that produces two genetically similar daughter cells.",

    "meiosis":
        "Meiosis is a special type of cell division that produces cells with half the usual number of chromosomes.",

    "photosynthesis":
        "Photosynthesis allows plants to use light energy to make chemical energy from water and carbon dioxide, producing oxygen as a byproduct.",

    "ecosystem":
        "An ecosystem is a community of living organisms interacting with each other and with their physical environment.",

    "food chain":
        "A food chain shows how energy and nutrients move from one organism to another through eating.",

    "kinetic energy":
        "Kinetic energy is the energy an object has because it is moving.",

    "potential energy":
        "Potential energy is stored energy an object has because of its position, condition, or arrangement.",

    "newton's first law":
        "Newton's first law says an object will remain at rest or continue moving at constant velocity unless an outside force changes its motion.",

    "newton's second law":
        "Newton's second law describes the relationship between force, mass, and acceleration: F = ma.",

    "newton's third law":
        "Newton's third law says forces come in pairs. When one object pushes on another, the second object pushes back with an equal and opposite force.",

    "mitochondria":
        "Mitochondria are structures inside many cells that produce much of the cell's usable energy.",

    "nucleus":
        "The nucleus is a structure in eukaryotic cells that contains most of the cell's DNA.",

    "ribosome":
        "Ribosomes are cellular structures that build proteins.",

    "democracy":
        "Democracy is a system of government in which political power is exercised by the people, directly or through representatives.",

    "photosynthesis equation":
        "A simplified photosynthesis equation is: carbon dioxide + water + light energy → glucose + oxygen.",

    "algebra":
        "Algebra is a branch of mathematics that uses symbols and variables to represent numbers and relationships.",

    "slope":
        "Slope describes how steep a line is. It is commonly calculated as rise divided by run.",

    "slope intercept form":
        "Slope-intercept form is y = mx + b. The m represents the slope and b represents the y-intercept.",

    "point slope form":
        "Point-slope form is y - y₁ = m(x - x₁). It is useful when you know a line's slope and one point on the line.",

    "x intercept":
        "The x-intercept is the point where a graph crosses the x-axis. At that point, y equals zero.",

    "y intercept":
        "The y-intercept is the point where a graph crosses the y-axis. At that point, x equals zero."
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

    "Why did the programmer quit his job? He didn't get arrays.",

    "What do you call an AI that sings badly? Artificial noise."

];


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

        if(!best || bestScore < 2){
            return null;
        }

        let answer =
            (best.extract || "").trim();

        if(!answer){
            return null;
        }

        if(answer.length > 1000){
            answer =
                answer.substring(0,1000) +
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

        return "I am J.A.R.V.I.S., your digital assistant interface. I can converse with you, solve mathematics, explain science and other subjects, use voice output, understand microphone input, and search for information when a question requires it.";
    }

    if(q.includes("what can you do")){

        return "I can handle conversation, mathematics, science, history, geography, general knowledge, jokes, voice output, microphone input, Combat Mode, custom speech commands, roasts, simpler explanations, and online information searches.";
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

    return null;
}


/* =====================================================
   MAIN BRAIN
===================================================== */

async function getResponse(originalQuestion){

    let question =
        originalQuestion.trim();

    /*
       SIMPLE MODE

       This is detected FIRST so:

       "What is gravity simple"

       becomes:

       question = "What is gravity"
       simpleMode = true
    */

    const simpleMode =
        detectSimpleMode(question);

    if(simpleMode){

        question =
            removeSimpleCommand(question);
    }

    const q =
        question.toLowerCase().trim();


    /* J.A.R.V.I.S. SAY */

    const say =
        sayCommand(question);

    if(say !== null){

        return say;
    }


    /* ROAST */

    const roast =
        roastCommand(question);

    if(roast !== null){

        return roast;
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


    /* MATH */

    const math =
        solveMath(q);

    if(math !== null){

        let answer =
            "The answer is " +
            math +
            ".";

        if(simpleMode){

            answer =
                "The answer is " +
                math +
                ". That's it — just this number.";

        }

        return answer;
    }


    /* KNOWLEDGE */

    const known =
        findKnowledge(q);

    if(known){

        if(simpleMode){

            return simplifyAnswer(known);
        }

        return known;
    }


    /* LOCAL CONVERSATION */

    const local =
        localResponse(q);

    if(local){

        if(simpleMode){

            return simplifyAnswer(local);
        }

        return local;
    }


    /* ONLINE SEARCH */

    if(isQuestion(q)){

        const result =
            await onlineSearch(question);

        if(result){

            if(simpleMode){

                return simplifyAnswer(result);
            }

            return result;
        }

        return simpleMode
            ? "I couldn't find a reliable answer. Try asking the question in a different way, and I'll explain it as simply as I can."
            : "I couldn't find a reliable answer for that. Try asking the question another way.";
    }


    /* CASUAL */

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
                "This browser does not provide speech recognition to this webpage.",
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
