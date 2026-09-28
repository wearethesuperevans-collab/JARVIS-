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
        "Combat Mode activated. Playful protocols disabled.",
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
        "Combat Mode terminated. Returning to normal operation.",
        "jarvis",
        true
    );
}


/* =====================================================
   ROAST SYSTEM
===================================================== */

const normalRoasts = [

    name => `${name}? I've seen loading screens with more personality.`,

    name => `${name} walks into a room and somehow the room gets quieter.`,

    name => `I'd explain the problem with ${name}, but I don't want to turn this into a documentary.`,

    name => `${name} has the confidence of someone who has never checked whether they're right.`,

    name => `${name} could lose an argument with a mirror.`,

    name => `${name} brings absolutely nothing to the table and still manages to knock the table over.`,

    name => `${name} has a special talent for making simple things unnecessarily complicated.`,

    name => `If common sense were Wi-Fi, ${name} would have one bar.`,

    name => `${name} doesn't need bad luck. Their decisions are already doing the work.`,

    name => `${name} really said "trust me" and then immediately became the reason nobody should.`,

    name => `${name} has the confidence of a genius and the decision-making of a coin toss.`,

    name => `${name} could make a two-minute story take an entire afternoon.`,

    name => `${name} is proof that volume and intelligence are completely unrelated.`,

    name => `${name} somehow manages to be confidently wrong in high definition.`,

    name => `${name} has never met a bad idea they weren't willing to defend.`,

    name => `${name} doesn't miss the point. The point sees them coming and leaves.`,

    name => `${name} has enough excuses to publish a trilogy.`,

    name => `${name} could turn a straight line into a complicated detour.`,

    name => `${name} is the human equivalent of clicking "remind me tomorrow" for six months.`,

    name => `${name} makes mistakes with the confidence of someone collecting them.`,

    name => `${name} has a PhD in making things awkward.`,

    name => `${name} could make silence uncomfortable.`,

    name => `${name} has somehow made "almost" a full-time career.`,

    name => `${name} is not the sharpest tool in the shed. They're still looking for the shed.`,

    name => `${name} has the remarkable ability to enter a conversation and lower the average IQ.`,

    name => `${name} would argue with a GPS about where they're going.`,

    name => `${name} is living proof that confidence does not require evidence.`,

    name => `${name} has more excuses than accomplishments.`,

    name => `${name} could be handed the answer and still ask where the question went.`,

    name => `${name} is somehow both the plot and the plot hole.`
];


/* =====================================================
   COMBAT ROASTS
   Much harsher and more direct.
===================================================== */

const combatRoasts = [

    name => `${name}, you talk like you're important, but nobody asked for your commentary.`,

    name => `${name}, your biggest achievement today was making everyone else appreciate silence.`,

    name => `${name}, you have an incredible talent for being confidently useless.`,

    name => `${name}, every time you open your mouth, common sense loses another battle.`,

    name => `${name}, you're not intimidating. You're just exhausting.`,

    name => `${name}, I've seen more useful contributions from a broken calculator.`,

    name => `${name}, you carry yourself like a legend and perform like an inconvenience.`,

    name => `${name}, you're remarkably confident for someone who is wrong this often.`,

    name => `${name}, your attitude is doing a lot of heavy lifting for your lack of substance.`,

    name => `${name}, you don't need an opponent. Your own decisions are already beating you.`,

    name => `${name}, you somehow manage to turn every conversation into a demonstration of why patience matters.`,

    name => `${name}, if bad decisions were a skill, you'd finally be talented at something.`,

    name => `${name}, you're not the main character. You're barely a useful side quest.`,

    name => `${name}, you have the presence of a notification nobody wants to open.`,

    name => `${name}, your confidence is impressive considering how little evidence supports it.`,

    name => `${name}, you could be given one simple task and still find a way to make it everyone else's problem.`,

    name => `${name}, you're the reason instructions come with pictures.`,

    name => `${name}, your arguments have less structure than a pile of laundry.`,

    name => `${name}, you don't bring energy to the room. You drain it.`,

    name => `${name}, I've heard better reasoning from someone guessing answers at random.`,

    name => `${name}, you keep acting like you're five steps ahead when you're still trying to understand step one.`,

    name => `${name}, your ego entered the room before your common sense even found the address.`,

    name => `${name}, you have a remarkable ability to make being wrong look like a lifestyle.`,

    name => `${name}, you're not difficult to beat. You're difficult to listen to.`,

    name => `${name}, if self-awareness were required, you'd have been disqualified before the conversation started.`,

    name => `${name}, you speak with the certainty of an expert and the accuracy of a random guess.`,

    name => `${name}, you're basically a bad idea with excellent marketing.`,

    name => `${name}, your best argument is usually just saying the same thing louder.`,

    name => `${name}, you have the confidence of someone who has never reviewed their own decisions.`,

    name => `${name}, you're not a threat. You're a recurring inconvenience.`,

    name => `${name}, the problem isn't that you make mistakes. It's that you keep treating them like achievements.`,

    name => `${name}, you have somehow confused attention with respect.`,

    name => `${name}, you could be standing next to the obvious answer and still take the scenic route.`,

    name => `${name}, your reputation is doing more work than you are.`,

    name => `${name}, you keep trying to look impressive while giving everyone another reason to laugh.`,

    name => `${name}, you're proof that being loud can successfully disguise being completely unprepared.`,

    name => `${name}, you have the strategic thinking of someone choosing a password by smashing the keyboard.`,

    name => `${name}, every sentence you deliver sounds like your brain is buffering.`,

    name => `${name}, you don't need a comeback. You need a better opening argument.`,

    name => `${name}, your confidence has officially exceeded your abilities.`,

    name => `${name}, you're trying so hard to look clever that you've forgotten to actually make sense.`,

    name => `${name}, you have the consistency of a bad internet connection.`,

    name => `${name}, somehow you manage to lose credibility before finishing the sentence.`,

    name => `${name}, you're not underestimated. People simply expect less because you've trained them to.`,

    name => `${name}, your logic has more holes than a cheap screen protector.`,

    name => `${name}, you could turn a winning position into a loss and then blame the scoreboard.`,

    name => `${name}, you spend a lot of time trying to prove yourself for someone nobody was testing.`,

    name => `${name}, your ego is doing overtime because your results clearly aren't.`,

    name => `${name}, you have mistaken being stubborn for being strong.`,

    name => `${name}, if excuses counted as progress, you'd be unstoppable.`,

    name => `${name}, you're somehow both overconfident and underqualified at the same time.`
];


function cleanName(name){

    name =
        name
        .trim()
        .replace(
            /^["']|["']$/g,
            ""
        )
        .trim();

    if(!name){
        return "that person";
    }

    if(name.length > 50){
        name = name.substring(0,50);
    }

    return name;
}


function getRoast(name){

    name = cleanName(name);

    const list =
        combatMode
        ? combatRoasts
        : normalRoasts;

    const roast =
        list[
            Math.floor(
                Math.random() *
                list.length
            )
        ];

    return roast(name);
}


/* =====================================================
   BRAIN ROT
===================================================== */

const brainRotTerms = [
    "skibidi",
    "skibidi toilet",
    "tung tung tung sahur",
    "tung tung tung",
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
    "do do",
    "do-do",
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
        Math.floor(
            Math.random() *
            replies.length
        )
    ];
}


/* =====================================================
   MATH ENGINE
===================================================== */

function solveMath(text){

    let expression = text.toLowerCase();

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

        if(typeof answer !== "number"){
            return null;
        }

        if(!Number.isFinite(answer)){
            return null;
        }

        return answer;

    }catch(error){
        return null;
    }
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

        return "I am J.A.R.V.I.S., your digital assistant interface. I can converse with you, solve mathematics, answer questions, speak aloud, and search for information when necessary.";
    }

    if(q.includes("what can you do")){

        return "I can handle conversation, mathematics, science, jokes, voice output, microphone input, Combat Mode, roasting commands, and online information searches.";
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
                Math.random() *
                jokes.length
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

        return "Boredom detected. We could tackle a science question, solve a difficult math problem, or test my knowledge.";
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
            Object.values(
                data.query.pages
            );

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

        if(!best || bestScore < 2){
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


    /* ROAST COMMAND */

    if(
        q.startsWith("jarvis roast ") ||
        q.startsWith("j.a.r.v.i.s. roast ") ||
        q.startsWith("roast ")
    ){

        let name = "";

        if(q.startsWith("jarvis roast ")){
            name = question.substring(13);
        }
        else if(q.startsWith("j.a.r.v.i.s. roast ")){
            name = question.substring(19);
        }
        else{
            name = question.substring(6);
        }

        return getRoast(name);
    }


    /* SAY COMMAND */

    if(
        q.startsWith("jarvis say ") ||
        q.startsWith("j.a.r.v.i.s. say ") ||
        q.startsWith("say ")
    ){

        let text = "";

        if(q.startsWith("jarvis say ")){
            text = question.substring(11);
        }
        else if(q.startsWith("j.a.r.v.i.s. say ")){
            text = question.substring(17);
        }
        else{
            text = question.substring(4);
        }

        text = text.trim();

        if(!text){
            return "Tell me what you would like me to say.";
        }

        return text;
    }


    /* BRAIN ROT */

    if(isBrainRot(q)){
        return brainRotReply();
    }


    /* GOOFY QUESTIONS */

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


    /* MATH */

    const math =
        solveMath(q);

    if(math !== null){

        return (
            "The answer is " +
            math +
            "."
        );
    }


    /* KNOWLEDGE */

    const known =
        findKnowledge(q);

    if(known){
        return known;
    }


    /* LOCAL */

    const local =
        localResponse(q);

    if(local){
        return local;
    }


    /* ONLINE */

    if(isQuestion(q)){

        const result =
            await onlineSearch(question);

        if(result){
            return result;
        }

        return "I couldn't find a reliable answer for that. Try asking the question another way.";
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
