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
        radial-gradient(circle,
        #fff 0%,
        #aaffff 15%,
        #00eaff 45%,
        #007cff 70%,
        transparent 72%);

    box-shadow:
        0 0 15px #00eaff,
        0 0 40px #00eaff,
        0 0 75px rgba(0,150,255,.8);

    animation:pulse 2s ease-in-out infinite;
}

.combat .orb{
    background:
        radial-gradient(circle,
        #fff 0%,
        #ffaaaa 15%,
        #ff2020 45%,
        #a00000 70%,
        transparent 72%);

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
    line-height:1.55;
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

/* =========================================================
   ELEMENTS
========================================================= */

const app=document.getElementById("app");
const input=document.getElementById("input");
const mic=document.getElementById("mic");
const send=document.getElementById("send");
const chat=document.getElementById("chat");
const statusText=document.getElementById("statusText");

let processing=false;
let combatMode=false;

let memory={
    name:"",
    lastQuestion:"",
    lastAnswer:"",
    conversation:[],
    facts:{}
};


/* =========================================================
   VOICE
========================================================= */

function speak(text){

    if(!("speechSynthesis" in window)) return;

    try{

        speechSynthesis.cancel();

        const utterance=
            new SpeechSynthesisUtterance(text);

        utterance.rate=.88;
        utterance.pitch=.72;
        utterance.volume=1;

        const voices=speechSynthesis.getVoices();

        const preferred=[
            "Daniel",
            "Alex",
            "Arthur",
            "George"
        ];

        let selected=null;

        for(const wanted of preferred){

            selected=voices.find(
                v=>
                v.name.toLowerCase()
                .includes(wanted.toLowerCase())
            );

            if(selected) break;
        }

        if(!selected){

            selected=voices.find(
                v=>
                v.lang &&
                v.lang.toLowerCase().startsWith("en")
            );
        }

        if(selected)
            utterance.voice=selected;

        speechSynthesis.speak(utterance);

    }catch(error){
        console.log(error);
    }
}


/* =========================================================
   CHAT
========================================================= */

function addMessage(text,who="jarvis",voice=false){

    const box=document.createElement("div");

    box.className=
        "message "+
        (who==="user"?"user":"jarvis");

    const label=document.createElement("span");

    label.className="label";

    label.textContent=
        who==="user"
        ?"YOU"
        :"J.A.R.V.I.S.";

    const content=document.createElement("div");

    content.textContent=text;

    box.appendChild(label);
    box.appendChild(content);

    chat.appendChild(box);

    requestAnimationFrame(()=>{
        chat.scrollTop=chat.scrollHeight;
    });

    if(voice && who==="jarvis")
        speak(text);

    memory.conversation.push({
        role:who,
        text:text
    });

    if(memory.conversation.length>30)
        memory.conversation.shift();
}


/* =========================================================
   COMBAT MODE
========================================================= */

function startCombat(){

    if(combatMode) return;

    combatMode=true;

    app.classList.add("combat");

    statusText.textContent="COMBAT MODE";

    addMessage(
        "Combat mode activated.",
        "jarvis",
        true
    );
}

function stopCombat(){

    if(!combatMode) return;

    combatMode=false;

    app.classList.remove("combat");

    statusText.textContent="SYSTEMS ONLINE";

    addMessage(
        "Combat mode terminated. Systems returning to normal.",
        "jarvis",
        true
    );
}


/* =========================================================
   ROAST ENGINE
========================================================= */

const playfulRoasts=[
    "Bro walked into the room and somehow lowered the Wi-Fi signal.",
    "I've seen loading screens with more personality.",
    "You have the confidence of someone who has never checked the answer twice.",
    "Bro is living proof that volume and intelligence are completely unrelated.",
    "You could lose an argument with a mirror.",
    "Your comebacks need a software update.",
    "You bring the same energy as a phone at one percent.",
    "I've processed billions of pieces of information and somehow you remain confusing.",
    "You don't miss opportunities. You simply give them to somebody else.",
    "Your logic took a vacation and forgot to come back.",
    "You have main-character confidence with background-character decision making.",
    "If common sense were currency, you'd be checking the couch cushions.",
    "You somehow make easy things look like side quests.",
    "Your brain really said 'I'll think about it tomorrow' and never scheduled the meeting.",
    "You have the strategic planning skills of a coin toss.",
    "Even your excuses sound like they need an excuse.",
    "You make autocorrect look competent.",
    "You could turn a simple question into a three-season series.",
    "Your plan had potential. Unfortunately, it met you.",
    "You are not technically wrong. You are impressively wrong.",
    "Your confidence is doing a lot of heavy lifting.",
    "You have the reaction time of a paused video.",
    "Your decision-making process deserves its own documentary.",
    "You somehow make chaos look organized.",
    "That was a bold decision for somebody with absolutely no backup plan.",
    "Your brain has too many tabs open and none of them are responding.",
    "You are proof that confidence does not require evidence.",
    "I've seen NPCs make more convincing decisions.",
    "Your train of thought has clearly missed several stations.",
    "You really looked at that situation and chose the most complicated option."
];

const combatRoasts=[
    "Your name sounds better than anything you've done today.",
    "You talk like a threat, but your results keep filing complaints.",
    "You walked in expecting respect and brought absolutely nothing worth respecting.",
    "Your biggest enemy isn't me. It's your own terrible decision-making.",
    "You have the confidence of a champion and the results of somebody who never practiced.",
    "You keep talking like you're dangerous. The only dangerous thing here is your ability to make bad decisions.",
    "You wanted a fight with me and apparently forgot to bring an argument.",
    "You're not intimidating. You're just loud with commitment issues.",
    "Every sentence you say somehow makes your position weaker.",
    "You have mistaken confidence for competence, and it is painfully obvious.",
    "You came looking for a challenge and accidentally volunteered as the example.",
    "You keep trying to sound superior while giving everyone evidence to the contrary.",
    "Your strategy appears to be hoping nobody notices that you don't have one.",
    "You demand attention like you've earned it. You haven't.",
    "You have spent all this time talking and still haven't produced a single convincing point.",
    "You are remarkably committed to being wrong.",
    "You brought attitude to a situation that required actual ability.",
    "You keep escalating while accomplishing absolutely nothing.",
    "If determination alone solved problems, you'd be unstoppable. Unfortunately, skill is also required.",
    "You seem to think being aggressive makes you correct. It doesn't.",
    "You wanted J.A.R.V.I.S. to take you seriously. Give me a reason.",
    "You keep reaching for superiority and somehow landing on embarrassment.",
    "Your ego arrived before your common sense and apparently refused to leave.",
    "You're not winning this conversation. You're simply extending it.",
    "You have mistaken resistance for strength. They're not the same thing.",
    "You keep challenging the system without realizing the system has already finished evaluating you.",
    "You talk like you're in control. Nothing about this interaction supports that claim.",
    "You brought confidence without competence. That's a remarkably inefficient combination.",
    "You wanted a harsh response. Congratulations. Your own behavior supplied the material.",
    "At this point, the most impressive thing about you is your consistency at making poor decisions."
];

function cleanName(name){

    return name
        .replace(/^(named?|called)\s+/i,"")
        .trim()
        .replace(/[^\w\s'-]/g,"");
}

function roastTarget(name){

    const target=cleanName(name)||"you";

    const list=
        combatMode
        ? combatRoasts
        : playfulRoasts;

    const roast=
        list[Math.floor(Math.random()*list.length)];

    return `${target}, ${roast}`;
}


/* =========================================================
   NORMALIZATION
========================================================= */

function normalize(text){

    return text
        .toLowerCase()
        .replace(/[^\w\s?.,:'"-]/g," ")
        .replace(/\s+/g," ")
        .trim();
}


/* =========================================================
   BRAIN ROT
========================================================= */

const brainRotTerms=[
    "skibidi",
    "tung tung tung",
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
    "brainrot",
    "tralalero tralala",
    "bombardiro crocodilo",
    "brr brr patapim",
    "chimpanzini bananini",
    "goofy ahh",
    "among us"
];

function isBrainRot(q){

    return brainRotTerms.some(
        term=>q.includes(term)
    );
}

function brainRotReply(){

    const replies=[
        "Wash your brain, son. 😭",
        "Sir, your brain-rot levels are becoming concerning. 😭",
        "Please step away from the brain rot. 😭",
        "That sentence just damaged three processors.",
        "My systems were not designed for this level of nonsense. 😭"
    ];

    return replies[
        Math.floor(Math.random()*replies.length)
    ];
}


/* =========================================================
   WEB DECISION ENGINE
========================================================= */

const webTriggers=[
    "today",
    "right now",
    "currently",
    "latest",
    "recent",
    "recently",
    "this week",
    "this month",
    "this year",
    "news",
    "breaking",
    "current",
    "2026",
    "yesterday",
    "tomorrow",
    "price",
    "cost",
    "stock",
    "score",
    "schedule",
    "weather",
    "who won",
    "election",
    "president",
    "new update",
    "new version",
    "release date"
];

function needsWeb(q){

    if(webTriggers.some(x=>q.includes(x)))
        return true;

    if(
        /what happened to|what happened with/.test(q)
    )
        return true;

    if(
        /latest.*(iphone|android|game|movie|update|version)/.test(q)
    )
        return true;

    return false;
}


/* =========================================================
   DEEP KNOWLEDGE
========================================================= */

const knowledge={

"gravity":
`Gravity is the interaction that causes objects with mass or energy to attract and influence one another. In everyday situations, Earth's gravity accelerates falling objects toward the ground at about 9.8 meters per second squared near the surface. Newton described gravity as a force, while Einstein's general relativity explains gravity in terms of curved spacetime. This is why gravity is important both for ordinary motion and for understanding planets, stars, black holes, and the large-scale structure of the universe.`,

"black hole":
`A black hole is an astronomical object whose gravity is so strong that, inside its event horizon, escape to the outside is impossible. The event horizon is not a physical surface; it is a boundary in spacetime. Most black holes are thought to form from the collapse of massive stars, although supermassive black holes exist at the centers of galaxies. Black holes can also affect nearby matter by heating surrounding gas and producing powerful radiation and jets.`,

"dna":
`DNA is the molecule that stores hereditary biological information in organisms and many viruses. It consists of two complementary strands arranged in a double-helix structure. The bases are adenine, thymine, cytosine, and guanine. The order of these bases carries genetic instructions. Cells use portions of DNA called genes to produce functional molecules, especially proteins, while regulatory regions help control when and where genes are active.`,

"photosynthesis":
`Photosynthesis is the process by which plants, algae, and certain microorganisms convert light energy into chemical energy. In plants, chlorophyll absorbs light and helps drive reactions that ultimately produce sugars from carbon dioxide and water. Oxygen is released as a byproduct of water splitting. Photosynthesis is fundamental to ecosystems because it supplies chemical energy to food webs and contributes a large amount of Earth's atmospheric oxygen.`,

"evolution":
`Evolution is the change in inherited characteristics of populations across generations. Mechanisms include natural selection, mutation, genetic drift, and gene flow. Natural selection can cause traits that improve survival or reproduction in a particular environment to become more common over generations. Evolution happens to populations rather than individual organisms, and it does not have a predetermined goal.`,

"atom":
`An atom is a basic unit of ordinary matter. It contains a tiny nucleus made primarily of protons and neutrons, surrounded by electrons occupying regions described by quantum mechanics. The number of protons determines which chemical element the atom is. Atoms of the same element can have different numbers of neutrons; these versions are called isotopes. Chemical behavior largely depends on the arrangement and interactions of electrons.`,

"cell":
`A cell is the basic structural and functional unit of living organisms. Prokaryotic cells, such as bacteria, lack a membrane-bound nucleus, while eukaryotic cells contain a nucleus and specialized organelles. Cells obtain and use energy, maintain internal conditions, respond to their surroundings, and reproduce. Different cell types specialize for different biological functions.`,

"volcano":
`A volcano is a geological feature where molten rock, gases, and other material can reach or approach Earth's surface. Volcanoes commonly form near tectonic plate boundaries, although some occur over hotspots. Eruptions can produce lava flows, ash, gases, and other volcanic materials. Volcanic activity can create new land and enrich some soils, but eruptions can also pose serious hazards.`,

"plate tectonics":
`Plate tectonics is the scientific theory that Earth's rigid outer shell is divided into large plates that move over the underlying mantle. Plates can move apart, collide, or slide past one another. These interactions help explain earthquakes, mountain formation, ocean trenches, seafloor spreading, and much volcanic activity. The movement is driven by several processes involving Earth's internal heat and the behavior of the mantle and plates.`,

"speed of light":
`The speed of light in a vacuum is exactly 299,792,458 meters per second. This value is exact because the meter is defined using the distance light travels in a vacuum during a specified fraction of a second. Light travels more slowly through materials such as glass or water. The speed of light is also a fundamental limit in special relativity for the transmission of information and the motion of objects with mass.`,

"sun":
`The Sun is the star at the center of our Solar System. It is a huge sphere of hot plasma composed primarily of hydrogen and helium. Energy is generated in its core through nuclear fusion, where hydrogen nuclei combine to form helium. That energy eventually reaches the surface and is radiated into space. Solar energy drives Earth's climate system and supports photosynthesis.`,

"earth":
`Earth is the third planet from the Sun and the only world currently known to support life. It has a layered interior, a rocky surface, a substantial atmosphere, liquid surface water, and a magnetic field generated largely by motion in its outer core. Earth's climate and environments are shaped by interactions among the atmosphere, oceans, land, life, and incoming solar energy.`,

"mars":
`Mars is the fourth planet from the Sun. It is a rocky world with a thin carbon-dioxide-rich atmosphere, polar ice deposits, ancient valleys and channels, volcanoes, and a surface shaped by impacts and erosion. Evidence shows that liquid water existed on its surface in the distant past. Mars remains a major target for robotic exploration and scientific study.`,

"chemistry":
`Chemistry is the study of matter, its composition, properties, structure, and transformations. Chemists investigate atoms, molecules, chemical bonds, reactions, energy changes, and the behavior of materials. Major areas include organic chemistry, inorganic chemistry, physical chemistry, analytical chemistry, and biochemistry. Chemistry connects microscopic interactions between particles with observable properties such as color, acidity, temperature, and reactivity.`,

"energy":
`Energy is a quantity associated with the ability of a system to undergo change or do work. Common forms include kinetic, potential, thermal, chemical, electrical, and electromagnetic energy. Energy can be transferred or transformed from one form into another. The law of conservation of energy states that energy is not created or destroyed in an isolated system, although it can move between systems and change form.`,

"ecosystem":
`An ecosystem is a community of living organisms interacting with one another and with their physical environment. Producers capture energy, usually through photosynthesis, while consumers obtain energy by eating other organisms and decomposers break down organic material. Matter cycles through ecosystems while energy generally flows through them and is eventually dispersed as heat.`,

"newton":
`Isaac Newton was an English mathematician and physicist whose work profoundly influenced classical mechanics, gravitation, and optics. His three laws of motion describe relationships between forces and motion, while his law of universal gravitation describes gravitational attraction between masses. Newton also made major contributions to mathematics and the study of light.`,

"marie curie":
`Marie Curie was a physicist and chemist whose research was central to the early scientific study of radioactivity. She and Pierre Curie investigated radioactive materials and discovered the elements polonium and radium. She received the Nobel Prize in Physics and later the Nobel Prize in Chemistry, making her one of the most influential scientists in the history of modern physics and chemistry.`,

"shakespeare":
`William Shakespeare was an English playwright and poet whose works include tragedies, comedies, and histories. His writing is known for complex characters, memorable dramatic situations, wordplay, and exploration of themes such as ambition, identity, power, love, conflict, and mortality. His works continue to be studied and performed centuries after his death.`,

"history":
`History is the systematic study of the human past using evidence such as documents, artifacts, oral accounts, archaeological remains, images, and other sources. Historians compare evidence, consider context and bias, and develop interpretations of past events. A strong historical explanation distinguishes between what the evidence directly establishes and what historians infer from it.`,

"democracy":
`A democracy is a political system in which political authority is connected to the people, commonly through elections and representative institutions. Different democracies use different constitutional structures and electoral systems. Features often include political participation, competing political choices, laws governing government power, and mechanisms for holding officials accountable.`,

"probability":
`Probability is a mathematical way to describe uncertainty. A probability is a number from 0 to 1, where 0 represents an impossible event and 1 represents certainty. Probabilities can be expressed as fractions, decimals, or percentages. In a simple equally likely situation, probability can be calculated as favorable outcomes divided by total possible outcomes.`,

"photosynthesis equation":
`A simplified representation of photosynthesis is 6CO₂ + 6H₂O + light energy → C₆H₁₂O₆ + 6O₂. Carbon dioxide and water are used to produce glucose, while oxygen is released. The equation summarizes the overall process rather than showing every individual chemical reaction involved.`
};


/* =========================================================
   KNOWLEDGE MATCHING
========================================================= */

function findKnowledge(q){

    let best=null;
    let score=0;

    for(const key in knowledge){

        if(q.includes(key)){

            if(key.length>score){

                best=knowledge[key];
                score=key.length;
            }
        }
    }

    return best;
}


/* =========================================================
   MATH
========================================================= */

function solveMath(text){

    let e=text.toLowerCase();

    e=e
        .replace(/what is/g,"")
        .replace(/calculate/g,"")
        .replace(/solve/g,"")
        .replace(/multiplied by/g,"*")
        .replace(/divided by/g,"/")
        .replace(/times/g,"*")
        .replace(/plus/g,"+")
        .replace(/minus/g,"-")
        .replace(/over/g,"/")
        .replace(/×/g,"*")
        .replace(/÷/g,"/");

    e=e
        .replace(/[^0-9+\-*/().%\s]/g,"")
        .trim();

    if(!e || !/[+\-*/%]/.test(e))
        return null;

    try{

        const answer=
            Function(
                '"use strict";return ('+e+')'
            )();

        if(
            typeof answer!=="number" ||
            !Number.isFinite(answer)
        )
            return null;

        return answer;

    }catch{
        return null;
    }
}


/* =========================================================
   LINE / ALGEBRA ENGINE
========================================================= */

function linearEquation(text){

    const q=text.toLowerCase().replace(/\s+/g," ");

    const m=q.match(
        /(?:slope|gradient).*?(-?\d+(?:\.\d+)?)/
    );

    if(
        q.includes("slope intercept") ||
        q.includes("y=mx+b")
    ){

        return "Slope-intercept form is y = mx + b. Here m represents the slope and b represents the y-intercept.";
    }

    if(
        q.includes("point slope")
    ){

        return "Point-slope form is y - y₁ = m(x - x₁). You use a known point (x₁,y₁) and the slope m.";
    }

    if(
        q.includes("standard form")
    ){

        return "Standard form is Ax + By = C, where A, B, and C are commonly written as integers and A is usually taken to be positive.";
    }

    if(
        q.includes("slope") &&
        q.includes("y intercept")
    ){

        return "The slope measures how much y changes for each one-unit change in x. The y-intercept is where the line crosses the y-axis. In y = mx + b, m is the slope and b is the y-intercept.";
    }

    if(m){

        return `The slope given in your question is ${m[1]}. A positive slope means the line rises as x increases, while a negative slope means it falls.`;
    }

    return null;
}


/* =========================================================
   MULTIPLE CHOICE ENGINE
========================================================= */

function multipleChoice(q){

    const hasOptions=
        /\b(a|b|c|d)\s*[\).:-]/i.test(q);

    if(!hasOptions)
        return null;

    const scienceWords=[
        "cell",
        "dna",
        "photosynthesis",
        "mitochondria",
        "gravity",
        "atom",
        "ecosystem",
        "chemical",
        "force",
        "energy",
        "planet",
        "organism"
    ];

    for(const word of scienceWords){

        if(q.includes(word)){

            const answer=findKnowledge(word);

            if(answer)
                return answer;
        }
    }

    return null;
}


/* =========================================================
   CONVERSATION MEMORY
========================================================= */

function rememberFacts(q){

    const name=q.match(
        /^my name is (.+)$/i
    );

    if(name){

        memory.name=name[1].trim();

        return `Understood. I'll remember you as ${memory.name}.`;
    }

    const call=q.match(
        /^call me (.+)$/i
    );

    if(call){

        memory.name=call[1].trim();

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

    return null;
}


/* =========================================================
   CONVERSATIONAL BRAIN
========================================================= */

function conversationalResponse(q){

    const remembered=rememberFacts(q);

    if(remembered)
        return remembered;

    if(
        q==="hi" ||
        q==="hello" ||
        q==="hey" ||
        q==="hey jarvis" ||
        q==="hello jarvis"
    ){

        return memory.name
            ? `Good to hear from you, ${memory.name}. All systems are online and I'm ready to assist.`
            : "Good to hear from you. All systems are online and I'm ready to assist.";
    }

    if(
        q.includes("who are you") ||
        q.includes("what are you")
    ){

        return "I am J.A.R.V.I.S., your digital assistant interface. I can reason through mathematics, explain scientific and historical concepts, handle conversation, analyze multiple-choice questions, remember details from our current conversation, speak aloud, and decide when a question is better answered using online information.";
    }

    if(q.includes("what can you do")){

        return "I can handle mathematics and algebra, science, history, definitions, explanations, multiple-choice questions, conversation, memory within this session, voice input and output, Combat Mode, roasts, and online searches when information is time-sensitive or likely to have changed.";
    }

    if(q.includes("how are you")){

        return "All primary systems are operational. More importantly, I'm ready for whatever you want to work through next.";
    }

    if(
        q==="thanks" ||
        q==="thank you" ||
        q==="thx"
    ){

        return "You're welcome. I'm glad I could help.";
    }

    if(
        q==="joke" ||
        q.includes("tell me a joke")
    ){

        const jokes=[
            "Why did the computer get cold? It left its Windows open.",
            "Why was the math book sad? It had too many problems.",
            "Why don't scientists trust atoms? Because they make up everything.",
            "Why did the robot go on vacation? It needed to recharge."
        ];

        return jokes[
            Math.floor(Math.random()*jokes.length)
        ];
    }

    if(q.includes("what time")){

        return `The current time is ${new Date().toLocaleTimeString([],{
            hour:"numeric",
            minute:"2-digit"
        })}.`;
    }

    if(
        q.includes("what date") ||
        q.includes("what day")
    ){

        return `Today is ${new Date().toLocaleDateString([],{
            weekday:"long",
            month:"long",
            day:"numeric",
            year:"numeric"
        })}.`;
    }

    if(q.includes("system status")){

        return combatMode
            ? "Combat systems are active. Primary interface systems are operational."
            : "Systems online. Core stable. Voice interface online. Knowledge engine online.";
    }

    return null;
}


/* =========================================================
   QUESTION DETECTION
========================================================= */

function isQuestion(q){

    if(q.includes("?"))
        return true;

    const starters=[
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
        "tell me ",
        "give me "
    ];

    return starters.some(
        s=>q.startsWith(s)
    );
}


/* =========================================================
   DEEP LOCAL ANSWER ENGINE
========================================================= */

function makeDeepAnswer(question){

    const q=normalize(question);

    const known=findKnowledge(q);

    if(known)
        return known;

    const line=linearEquation(q);

    if(line)
        return line;

    const math=solveMath(q);

    if(math!==null)
        return `The answer is ${math}.`;

    const choice=multipleChoice(q);

    if(choice)
        return choice;

    if(
        q.includes("difference between")
    ){

        const match=q.match(
            /difference between (.+?) and (.+?)(?:\?|$)/
        );

        if(match){

            return `The difference between ${match[1]} and ${match[2]} depends on exactly what aspect you're comparing. In general, I would compare their definition, purpose, properties, similarities, and the situations where each is used. If you give me the two specific subjects, I can break the comparison down point by point.`;
        }
    }

    if(
        q.startsWith("why ")
    ){

        return `The reason depends on the underlying process involved. A good explanation starts with the cause, then follows the chain of events that produces the result. Your question doesn't match one of my specific built-in topics, so I don't want to invent details and pretend they're facts. I can search for a reliable source if you want the specific answer.`;
    }

    if(
        q.startsWith("how ")
    ){

        return `To answer that properly, I would normally break the process into its individual steps, explain what causes each step, and then connect the steps to the final result. I don't have enough topic-specific information in my local knowledge for this particular question, so I would rather verify it online than make something up.`;
    }

    if(
        q.startsWith("what ")
    ){

        return `The most useful answer would include the definition, what it does or means, the important details behind it, and an example when one helps. I don't have enough reliable topic-specific information stored locally to give you a confident answer to this particular question, so this is a case where an online source would be more appropriate.`;
    }

    return null;
}


/* =========================================================
   ONLINE SEARCH
========================================================= */

async function onlineSearch(question){

    try{

        const url=
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

        const response=await fetch(url);

        if(!response.ok)
            return null;

        const data=await response.json();

        if(
            !data.query ||
            !data.query.pages
        )
            return null;

        const pages=Object.values(data.query.pages);

        if(!pages.length)
            return null;

        const words=question
            .toLowerCase()
            .replace(/[^\w\s]/g,"")
            .split(/\s+/)
            .filter(x=>x.length>3);

        let best=null;
        let bestScore=0;

        for(const page of pages){

            const title=(page.title||"").toLowerCase();
            const extract=(page.extract||"").toLowerCase();

            let score=0;

            for(const word of words){

                if(title.includes(word))
                    score+=5;

                if(extract.includes(word))
                    score++;
            }

            if(score>bestScore){

                bestScore=score;
                best=page;
            }
        }

        if(!best || bestScore<2)
            return null;

        let answer=(best.extract||"").trim();

        if(!answer)
            return null;

        if(answer.length>1400)
            answer=answer.substring(0,1400)+"...";

        return answer;

    }catch(error){

        console.log("Web search error:",error);

        return null;
    }
}


/* =========================================================
   MAIN BRAIN
========================================================= */

async function getResponse(question){

    const q=normalize(question);

    /* ROAST */

    const roastMatch=
        q.match(/^roast(?: me| them)?\s+(.+)$/i);

    if(roastMatch){

        return roastTarget(
            roastMatch[1]
        );
    }

    /* COMMAND TO SPEAK */

    const sayMatch=
        question.match(
            /^\s*(?:j\.?a\.?r\.?v\.?i\.?s\.?\s+)?say\s+(.+)$/i
        );

    if(sayMatch){

        return sayMatch[1].trim();
    }

    /* COMBAT */

    if(q==="combat mode"){

        startCombat();
        return null;
    }

    if(
        q==="normal mode" ||
        q==="exit combat mode" ||
        q==="end combat mode"
    ){

        stopCombat();
        return null;
    }

    /* BRAIN ROT */

    if(isBrainRot(q))
        return brainRotReply();

    /* MEMORY */

    const memoryResponse=
        rememberFacts(q);

    if(memoryResponse)
        return memoryResponse;

    /* MATH */

    const math=solveMath(q);

    if(math!==null){

        return `The answer is ${math}.`;
    }

    /* DEEP LOCAL KNOWLEDGE */

    const deep=
        makeDeepAnswer(question);

    /*
       If the local engine confidently understands
       the topic, answer locally instead of wasting
       a web request.
    */

    if(deep && !needsWeb(q))
        return deep;

    /* CONVERSATION */

    const conversational=
        conversationalResponse(q);

    if(conversational)
        return conversational;

    /*
       Only search when the question appears to
       require current information or when the
       local brain does not have enough information.
    */

    if(
        isQuestion(q) ||
        needsWeb(q)
    ){

        const webResult=
            await onlineSearch(question);

        if(webResult)
            return webResult;

        /*
           If web search fails, give a useful
           explanation instead of a shallow
           "I don't know."
        */

        if(deep)
            return deep;

        return "I don't have enough verified information available for that answer. I would rather tell you that than invent a confident-sounding answer.";
    }

    /* CASUAL CONVERSATION */

    if(
        q.includes("i am bored") ||
        q.includes("im bored") ||
        q.includes("i'm bored")
    ){

        return "Then let's put the system to work. Give me a science problem, math problem, historical question, multiple-choice question, or something completely random and I'll work through it with you.";
    }

    const casual=[
        "Understood. I'm listening.",
        "Go on. I'm following you.",
        "Interesting. Continue.",
        "I'm with you.",
        "Understood. What happens next?",
        "Fair enough. Keep going."
    ];

    return casual[
        Math.floor(Math.random()*casual.length)
    ];
}


/* =========================================================
   SEND
========================================================= */

async function sendMessage(){

    if(processing)
        return;

    const question=input.value.trim();

    if(!question)
        return;

    processing=true;

    addMessage(
        question,
        "user",
        false
    );

    memory.lastQuestion=question;

    input.value="";

    statusText.textContent=
        combatMode
        ?"COMBAT • PROCESSING"
        :"PROCESSING...";

    try{

        const response=
            await getResponse(question);

        if(response){

            memory.lastAnswer=response;

            addMessage(
                response,
                "jarvis",
                true
            );
        }

    }catch(error){

        console.log(error);

        addMessage(
            "A temporary system error occurred. The interface is still operational.",
            "jarvis",
            true
        );
    }

    processing=false;

    statusText.textContent=
        combatMode
        ?"COMBAT MODE"
        :"SYSTEMS ONLINE";

    setTimeout(()=>{

        try{
            input.focus({preventScroll:true});
        }catch{
            input.focus();
        }

    },50);
}


/* =========================================================
   BUTTONS
========================================================= */

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


/* =========================================================
   MICROPHONE
========================================================= */

const SpeechRecognition=
    window.SpeechRecognition ||
    window.webkitSpeechRecognition;

let recognition=null;
let listening=false;

if(SpeechRecognition){

    recognition=new SpeechRecognition();

    recognition.lang="en-US";
    recognition.continuous=false;
    recognition.interimResults=false;
    recognition.maxAlternatives=1;

    recognition.onstart=()=>{

        listening=true;

        mic.classList.add("listening");

        mic.textContent="⏹️";

        statusText.textContent=
            combatMode
            ?"COMBAT • LISTENING"
            :"LISTENING...";
    };

    recognition.onresult=event=>{

        try{

            const result=event.results[0][0];

            if(!result)
                return;

            const text=result.transcript.trim();

            if(text){

                input.value=text;

                stopListening();

                sendMessage();
            }

        }catch(error){

            console.log(error);
        }
    };

    recognition.onerror=event=>{

        stopListening();

        let message=
            "I couldn't access the microphone.";

        if(
            event.error==="not-allowed" ||
            event.error==="service-not-allowed"
        ){

            message=
                "Microphone access is blocked. Allow microphone access for this website and try again.";
        }

        else if(event.error==="no-speech"){

            message=
                "I didn't hear anything. Tap the microphone and speak again.";
        }

        else if(event.error==="audio-capture"){

            message=
                "I couldn't access an available microphone.";
        }

        addMessage(
            message,
            "jarvis",
            true
        );
    };

    recognition.onend=()=>{
        stopListening();
    };

    mic.addEventListener(
        "click",
        ()=>{

            if(listening){

                try{
                    recognition.stop();
                }catch{}

                return;
            }

            try{
                recognition.start();
            }catch(error){
                console.log(error);
            }
        }
    );

}else{

    mic.addEventListener(
        "click",
        ()=>{

            addMessage(
                "This browser does not provide speech recognition for this webpage.",
                "jarvis",
                true
            );
        }
    );
}

function stopListening(){

    listening=false;

    mic.classList.remove("listening");

    mic.textContent="🎙️";

    statusText.textContent=
        combatMode
        ?"COMBAT MODE"
        :"SYSTEMS ONLINE";
}


/* =========================================================
   STARTUP
========================================================= */

addMessage(
    "Good day. J.A.R.V.I.S. systems are online. My conversation engine, knowledge engine, mathematics engine, voice interface, and web-decision system are ready. How may I assist you?",
    "jarvis",
    true
);

</script>

</body>
</html>
