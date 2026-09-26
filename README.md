<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<title>JARVIS</title>

<style>
*{box-sizing:border-box}

html,body{
    margin:0;
    width:100%;
    height:100%;
    background:#02070a;
    color:#dffcff;
    font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",sans-serif
}

body{
    overflow:hidden
}

.app{
    height:100%;
    display:flex;
    flex-direction:column;
    background:
        radial-gradient(circle at 50% 28%,#063b48 0,#02151c 30%,#010609 72%);
    position:relative
}

.grid{
    position:absolute;
    inset:0;
    background-image:
        linear-gradient(rgba(0,220,255,.05) 1px,transparent 1px),
        linear-gradient(90deg,rgba(0,220,255,.05) 1px,transparent 1px);
    background-size:32px 32px;
    pointer-events:none
}

.top{
    height:74px;
    padding:14px 18px;
    display:flex;
    justify-content:space-between;
    align-items:center;
    border-bottom:1px solid rgba(0,225,255,.25);
    z-index:2
}

.title{
    font-weight:800;
    letter-spacing:4px;
    color:#8ff5ff;
    font-size:24px;
    text-shadow:0 0 14px rgba(0,220,255,.7)
}

.status{
    font-size:11px;
    letter-spacing:2px;
    color:#68ffad
}

.main{
    flex:1;
    min-height:0;
    display:flex;
    flex-direction:column;
    align-items:center;
    padding:16px
}

.core{
    width:min(270px,60vw);
    aspect-ratio:1;
    border-radius:50%;
    position:relative;
    display:grid;
    place-items:center;
    margin:2px 0 14px
}

.ring{
    position:absolute;
    border-radius:50%;
    border:1px solid rgba(0,225,255,.5);
    inset:0;
    box-shadow:
        0 0 35px rgba(0,180,255,.12),
        inset 0 0 35px rgba(0,180,255,.08)
}

.ring.r2{
    inset:12%;
    border:3px solid rgba(0,225,255,.7)
}

.ring.r3{
    inset:25%;
    border:1px dashed rgba(100,240,255,.7)
}

.orb{
    width:30%;
    aspect-ratio:1;
    border-radius:50%;
    background:#9fffff;
    box-shadow:
        0 0 20px #00d9ff,
        0 0 60px rgba(0,220,255,.8),
        0 0 110px rgba(0,150,255,.35)
}

.label{
    text-align:center;
    letter-spacing:3px;
    color:#8ff5ff;
    font-size:12px
}

.chat{
    width:min(900px,100%);
    flex:1;
    min-height:100px;
    overflow:auto;
    padding:8px 2px 18px
}

.msg{
    max-width:88%;
    padding:11px 14px;
    margin:8px 0;
    border:1px solid rgba(0,220,255,.22);
    border-radius:14px;
    line-height:1.45;
    white-space:pre-wrap
}

.jarvis{
    background:rgba(0,180,220,.08);
    box-shadow:inset 0 0 20px rgba(0,180,255,.03)
}

.user{
    margin-left:auto;
    background:rgba(255,255,255,.05);
    border-color:rgba(255,255,255,.14)
}

.composer{
    width:min(900px,100%);
    display:flex;
    gap:8px;
    padding:10px 0 calc(10px + env(safe-area-inset-bottom));
    z-index:2
}

.input{
    flex:1;
    min-width:0;
    border:1px solid rgba(0,225,255,.35);
    border-radius:14px;
    background:rgba(0,0,0,.45);
    color:white;
    padding:13px 14px;
    outline:none;
    font-size:16px
}

.input:focus{
    border-color:#00e0ff;
    box-shadow:0 0 18px rgba(0,220,255,.15)
}

button{
    border:1px solid rgba(0,225,255,.55);
    background:rgba(0,180,220,.13);
    color:#b9faff;
    border-radius:14px;
    padding:0 17px;
    font-weight:700
}

button:active{
    transform:scale(.97)
}

.hint{
    font-size:10px;
    opacity:.55;
    text-align:center;
    margin-bottom:4px;
    letter-spacing:1px
}
</style>
</head>

<body>

<div class="app">

<div class="grid"></div>

<header class="top">
    <div class="title">J.A.R.V.I.S.</div>
    <div class="status">● SYSTEMS ONLINE</div>
</header>

<main class="main">

<div class="core">
    <div class="ring"></div>
    <div class="ring r2"></div>
    <div class="ring r3"></div>
    <div class="orb"></div>
</div>

<div class="label">
    PERSONAL INTELLIGENCE SYSTEM
</div>

<div id="chat" class="chat"></div>

<div class="hint">
    ASK NATURALLY — SCIENCE • MATH • HISTORY • PEOPLE • EARTH • SPACE
</div>

<div class="composer">

<input
    id="input"
    class="input"
    autocomplete="off"
    placeholder="Ask JARVIS anything…"
>

<button id="send" type="button">
    SEND
</button>

</div>

</main>
</div>


<script>

(function(){

"use strict";


/* =========================
   GET ELEMENTS
========================= */

const chat = document.getElementById("chat");
const input = document.getElementById("input");
const send = document.getElementById("send");

if(!chat || !input || !send){
    return;
}


/* =========================
   MEMORY
========================= */

const memory = {
    name:"",
    lastQuestion:"",
    lastTopic:""
};


/* =========================
   NORMALIZE TEXT
========================= */

function norm(text){

    return String(text || "")
        .toLowerCase()
        .replace(/[’']/g,"'")
        .replace(/[^a-z0-9?%+*/().^×÷√ -]/g," ")
        .replace(/\s+/g," ")
        .trim();

}


/* =========================
   ADD MESSAGE
========================= */

function add(text,who){

    const div = document.createElement("div");

    div.className =
        "msg " +
        (who === "user" ? "user" : "jarvis");

    div.textContent = text;

    chat.appendChild(div);

    chat.scrollTop = chat.scrollHeight;

}


/* =========================
   JARVIS VOICE
========================= */

function speak(text){

    if(!window.speechSynthesis){
        return;
    }

    try{

        speechSynthesis.cancel();

        const utterance =
            new SpeechSynthesisUtterance(
                String(text).replace(/https?:\/\/\S+/g,"")
            );

        utterance.rate = 0.88;
        utterance.pitch = 0.72;
        utterance.volume = 1;

        const voices = speechSynthesis.getVoices();

        const preferred = [
            "Daniel",
            "Alex",
            "Arthur",
            "Google UK English Male",
            "George",
            "Ryan"
        ];

        let voice = null;

        for(const wanted of preferred){

            voice = voices.find(v =>
                v.name
                 .toLowerCase()
                 .includes(wanted.toLowerCase())
            );

            if(voice){
                break;
            }

        }

        if(voice){
            utterance.voice = voice;
        }

        speechSynthesis.speak(utterance);

    }catch(error){

        // Voice failure must never break JARVIS.

    }

}


/* =========================
   MATH ENGINE
========================= */

function math(expression){

    let e = String(expression)
        .toLowerCase()
        .replace(/what is/g,"")
        .replace(/calculate/g,"")
        .replace(/solve/g,"")
        .replace(/evaluate/g,"")
        .replace(/equals/g,"")
        .replace(/equal to/g,"")
        .replace(/multiplied by/g,"*")
        .replace(/times/g,"*")
        .replace(/divided by/g,"/")
        .replace(/plus/g,"+")
        .replace(/minus/g,"-")
        .replace(/over/g,"/")
        .replace(/×/g,"*")
        .replace(/÷/g,"/")
        .replace(/\^/g,"**")
        .replace(/√\s*([0-9.]+)/g,"Math.sqrt($1)")
        .replace(/\bpi\b/g,"Math.PI")
        .replace(/\s+/g,"");

    if(!/[0-9]/.test(e)){
        return null;
    }

    if(!/[+\-*/%]/.test(e)){
        return null;
    }

    if(!/^[0-9+\-*/%().A-Za-z*]+$/.test(e)){
        return null;
    }

    try{

        const result =
            Function(
                '"use strict";return (' + e + ')'
            )();

        if(
            typeof result !== "number" ||
            !Number.isFinite(result)
        ){
            return null;
        }

        return Number.isInteger(result)
            ? String(result)
            : String(Number(result.toFixed(8)));

    }catch{

        return null;

    }

}


/* =========================
   BUILT-IN KNOWLEDGE
========================= */

const facts = [

[
/black holes?|event horizon|singularity/,
"A black hole is a region of spacetime where gravity is so strong that nothing crossing its event horizon can escape, including light. Many black holes form when massive stars collapse. The event horizon is the boundary beyond which escape is impossible according to general relativity."
],

[
/gravity|gravitational/,
"Gravity is the interaction associated with mass and energy. Newton described it as a force, while Einstein's general relativity describes gravity as the curvature of spacetime produced by matter and energy."
],

[
/atom|atomic structure/,
"An atom is a basic unit of ordinary matter. It contains a nucleus made of protons and neutrons, with electrons occupying regions around the nucleus. The number of protons identifies the element."
],

[
/dna|genetics|genetic code/,
"DNA is the molecule that stores hereditary information. Its sequence uses four bases: adenine, thymine, cytosine and guanine. DNA provides instructions used by cells."
],

[
/photosynthesis/,
"Photosynthesis lets plants, algae and some microorganisms convert light energy into chemical energy. They use carbon dioxide and water to make sugars and release oxygen as a byproduct."
],

[
/evolution|natural selection/,
"Evolution is change in inherited characteristics of populations over generations. Natural selection is one mechanism of evolution: inherited traits that improve reproduction can become more common over time."
],

[
/solar system|planets/,
"The Solar System contains the Sun and objects gravitationally bound to it. The eight planets, from the Sun outward, are Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus and Neptune."
],

[
/speed of light|what is light/,
"Light is electromagnetic radiation. In a vacuum it travels at about 299,792 kilometers per second, or about 186,282 miles per second."
],

[
/relativity|special relativity|general relativity/,
"Relativity is Einstein's framework for space, time, motion and gravity. Special relativity describes motion and the relationship between space and time. General relativity describes gravity through curved spacetime."
],

[
/ancient egypt|egyptian civilization|pharaohs|pyramids/,
"Ancient Egypt was a civilization centered along the Nile. It developed complex government, writing, mathematics, religion and monumental architecture. The pyramids at Giza were built during Egypt's Old Kingdom."
],

[
/roman empire|ancient rome|roman history/,
"The Roman Empire grew from Rome's earlier republic into a huge Mediterranean-centered state. Roman law, engineering, government and culture influenced many later societies. The Western Roman Empire traditionally ended in 476 CE."
],

[
/american revolution|revolutionary war/,
"The American Revolution was the conflict in which thirteen British North American colonies fought for independence. Fighting began in 1775, independence was declared in 1776, and the Treaty of Paris ended the war in 1783."
],

[
/world war ?2|world war ii|ww2|second world war/,
"World War II was a global conflict from 1939 to 1945. The principal Allied powers included the United States, Soviet Union, United Kingdom and China, while Germany, Italy and Japan were the principal Axis powers. The war ended in 1945."
],

[
/industrial revolution|industrialization/,
"The Industrial Revolution began in Britain in the 18th century and spread to other regions. Factories, mechanized production, steam power and new transportation transformed economies and societies."
],

[
/albert einstein|einstein/,
"Albert Einstein was a theoretical physicist best known for relativity and major contributions to quantum theory. He received the 1921 Nobel Prize in Physics for his work explaining the photoelectric effect."
],

[
/isaac newton|newton/,
"Isaac Newton was an English mathematician and physicist whose laws of motion and universal gravitation became foundations of classical mechanics. He also made major contributions to mathematics and optics."
],

[
/marie curie|madame curie/,
"Marie Curie was a physicist and chemist who pioneered research on radioactivity. She discovered polonium and radium and became the first person to receive two Nobel Prizes."
],

[
/leonardo da vinci|leonardo/,
"Leonardo da Vinci was an Italian Renaissance artist, engineer and inventor. His surviving notebooks contain studies of anatomy, mechanics, nature and engineering, alongside famous artworks."
],

[
/william shakespeare|shakespeare/,
"William Shakespeare was an English playwright and poet whose works include Hamlet, Macbeth and Romeo and Juliet. He is one of the most studied writers in English."
],

[
/george washington|washington/,
"George Washington commanded the Continental Army during the American Revolution and became the first president of the United States under the Constitution."
],

[
/moon|lunar/,
"The Moon is Earth's natural satellite. It is about 384,400 kilometers from Earth on average and its gravity helps produce Earth's ocean tides."
],

[
/earth|our planet/,
"Earth is the third planet from the Sun and the only world currently known to support life. It has a rocky surface, a nitrogen-and-oxygen-rich atmosphere, liquid water at its surface and an active geological system."
],

[
/mars|red planet/,
"Mars is the fourth planet from the Sun. It is a cold rocky world with a thin atmosphere, polar ice, enormous volcanoes and evidence that liquid water existed on its surface in the distant past."
],

[
/jupiter/,
"Jupiter is the largest planet in the Solar System. It is a gas giant with a powerful magnetic field and a famous atmospheric storm called the Great Red Spot."
],

[
/saturn/,
"Saturn is the sixth planet from the Sun and is famous for its extensive ring system. It is a gas giant composed mostly of hydrogen and helium."
],

[
/venus/,
"Venus is the second planet from the Sun. It has a thick carbon-dioxide atmosphere and a surface temperature hot enough to melt lead, making it the hottest planet in the Solar System."
],

[
/mercury/,
"Mercury is the closest planet to the Sun and the smallest planet in the Solar System. It has a heavily cratered rocky surface and experiences extreme temperature changes."
],

[
/black hole vs neutron star|neutron star/,
"A neutron star is the extremely dense collapsed core left behind by some massive stars after a supernova. A black hole is different because its gravity can create an event horizon from which light cannot escape."
],

[
/chemical reaction|chemistry/,
"A chemical reaction is a process in which substances are transformed into different substances. Atoms are rearranged as chemical bonds break and form."
],

[
/photosynthesis/,
"Photosynthesis converts light energy into chemical energy. Plants use carbon dioxide and water to produce sugars and release oxygen."
],

[
/cell|cells|biology/,
"A cell is the basic structural and functional unit of living organisms. Some organisms consist of one cell, while others are made of trillions of specialized cells."
],

[
/dna/,
"DNA stores genetic information. Its four chemical bases form sequences that cells use as biological instructions."
],

[
/ocean|oceans/,
"Earth has five commonly recognized oceans: the Pacific, Atlantic, Indian, Southern and Arctic Oceans. The Pacific is the largest and deepest."
],

[
/volcano|volcanoes/,
"A volcano is an opening in Earth's crust through which molten rock, gases and other material can reach the surface. Volcanoes are strongly associated with plate tectonics."
],

[
/tectonic plates|plate tectonics/,
"Plate tectonics is the theory that Earth's outer rocky shell is divided into moving plates. Their movement helps create mountains, earthquakes, volcanoes and ocean basins."
]

];


/* =========================
   JOKES
========================= */

const jokes = [

"Why did the computer get cold? It left its Windows open.",

"Why was the math book sad? It had too many problems.",

"Why don't scientists trust atoms? Because they make up everything.",

"Why did the photon refuse to check a bag? It was traveling light.",

"Why did the astronaut need space? Because everyone kept getting in his orbit.",

"I told my computer I needed a break. Now it keeps showing me vacation ads.",

"Why did the history teacher stay calm? Everything was already in the past.",

"Why did the robot go on vacation? It needed to recharge.",

"What do you call an educated tube? A graduated cylinder.",

"Why did the electron get in trouble? It was being negative.",

"Why did the telescope get promoted? It had a great outlook.",

"Why did the astronaut break up with the moon? It needed space.",

"Why was the equal sign so humble? It knew it wasn't greater than anyone.",

"I would tell you a chemistry joke, but I know I wouldn't get a reaction.",

"Why did the computer go to the doctor? It had a virus.",

"Why did the history teacher bring a ladder? To reach the past.",

"What did one volcano say to the other? I lava you.",

"Why did the calculator break up with the pencil? It needed someone who could handle advanced functions.",

"Why did the robot cross the road? Its programming told it to.",

"Why did the moon skip dinner? It was already full."

];


/* =========================
   LOCAL CONVERSATION
========================= */

function local(question){

    const n = norm(question);


    /* GREETINGS */

    if(
        /^(hi|hello|hey|yo|sup|good morning|good afternoon|good evening)\b/
        .test(n)
    ){

        return memory.name
            ? `Good to see you, ${memory.name}. Systems are online.`
            : "Good to see you. JARVIS is online and ready.";

    }


    /* IDENTITY */

    if(
        /who are you|what are you|your name/
        .test(n)
    ){

        return "I am JARVIS, your personal AI assistant. I can calculate, explain science, mathematics and history, search knowledge sources, remember parts of our conversation and continue a discussion naturally.";

    }


    /* NAME */

    const nameMatch = question.match(
        /(?:my name is|call me|you can call me)\s+([a-zA-Z0-9_-]+)/i
    );

    if(nameMatch){

        memory.name = nameMatch[1];

        return `Understood. I'll call you ${memory.name}.`;

    }


    if(
        /what is my name|do you know my name/
        .test(n)
    ){

        return memory.name
            ? `Your name is ${memory.name}.`
            : "You haven't told me your name yet.";

    }


    /* TIME */

    if(
        /what time|^time$/.test(n)
    ){

        return `The current time is ${
            new Date().toLocaleTimeString(
                [],
                {
                    hour:"numeric",
                    minute:"2-digit"
                }
            )
        }.`;

    }


    /* DATE */

    if(
        /what date|today's date|^date$/.test(n)
    ){

        return `Today is ${
            new Date().toLocaleDateString(
                [],
                {
                    weekday:"long",
                    month:"long",
                    day:"numeric",
                    year:"numeric"
                }
            )
        }.`;

    }


    /* JOKES */

    if(
        /tell me a joke|tell me another joke|^joke$|make me laugh/
        .test(n)
    ){

        return jokes[
            Math.floor(Math.random() * jokes.length)
        ];

    }


    /* LAST QUESTION */

    if(
        /what did i ask|my last question/.test(n)
    ){

        return memory.lastQuestion
            ? `Your last question was: "${memory.lastQuestion}"`
            : "There is no previous question in this session.";

    }


    /* FOLLOW-UP */

    if(
        /tell me more|^more$|go on|continue|explain more/
        .test(n)
    ){

        return memory.lastTopic
            ? `Certainly. We were discussing ${memory.lastTopic}. Ask me what part you want to explore and I'll continue.`
            : "Certainly. Tell me the subject you want me to expand on.";

    }


    /* THANKS */

    if(
        /^thanks|thank you/.test(n)
    ){

        return "You're welcome.";

    }


    /* STOP VOICE */

    if(
        /stop talking|stop speaking|be quiet/.test(n)
    ){

        try{
            speechSynthesis.cancel();
        }catch{}

        return "Voice output stopped.";

    }


    /* BORED */

    if(
        /i'?m bored/.test(n)
    ){

        return "Try me. Ask about a black hole, an ancient civilization, a famous person, a math problem, Earth, space, or anything you've wondered about.";

    }


    return null;

}


/* =========================
   WIKIPEDIA SEARCH
========================= */

async function wiki(question){

    let topic = String(question);


    topic = topic
        .replace(
            /^\s*(can you|could you|please|hey jarvis|jarvis)\s+/i,
            ""
        )
        .replace(
            /^(tell me about|explain|describe|what is|what's|who is|who's|where is|when was|when did|why is|why are|why does|how does|how do|what are|what were)\s+/i,
            ""
        )
        .replace(/[?!]/g,"")
        .trim();


    if(topic.length < 2){
        return null;
    }


    if(topic.length > 180){
        topic = topic.slice(0,180);
    }


    try{

        const url =
            "https://en.wikipedia.org/w/api.php?" +
            new URLSearchParams({

                action:"query",
                list:"search",
                srsearch:topic,
                srlimit:"3",
                format:"json",
                origin:"*"

            });


        const response =
            await fetch(url);


        if(!response.ok){
            return null;
        }


        const data =
            await response.json();


        const results =
            data &&
            data.query &&
            data.query.search;


        if(
            !results ||
            results.length === 0
        ){
            return null;
        }


        const title =
            results[0].title;


        const summaryURL =
            "https://en.wikipedia.org/api/rest_v1/page/summary/" +
            encodeURIComponent(
                title.replace(/ /g,"_")
            );


        const summaryResponse =
            await fetch(summaryURL);


        if(!summaryResponse.ok){
            return null;
        }


        const summary =
            await summaryResponse.json();


        if(!summary.extract){
            return null;
        }


        memory.lastTopic = title;


        let answer = summary.extract;


        if(answer.length > 1900){

            answer =
                answer.substring(0,1900) +
                "...";

        }


        return (
            answer +
            "\n\nSource: Wikipedia — " +
            title
        );


    }catch(error){

        return null;

    }

}


/* =========================
   MAIN ANSWER ENGINE
========================= */

async function answer(question){

    /* Conversation */

    const localAnswer =
        local(question);

    if(localAnswer){
        return localAnswer;
    }


    /* Math */

    const mathAnswer =
        math(question);

    if(mathAnswer !== null){

        return `The answer is ${mathAnswer}.`;

    }


    /* Built-in knowledge */

    const normalized =
        norm(question);


    for(const item of facts){

        if(item[0].test(normalized)){

            memory.lastTopic = question;

            return item[1];

        }

    }


    /* Internet knowledge */

    const shouldSearch =

        normalized.includes("?") ||

        /^(what|who|where|when|why|how|which|can|is|are|was|were|did|does|do)\b/
        .test(normalized) ||

        /history|science|scientist|math|mathematics|physics|chemistry|biology|earth|planet|space|country|person|people|war|event|invent|invention|black hole|galaxy|star|ocean|volcano|animal|technology/
        .test(normalized);


    if(shouldSearch){

        const result =
            await wiki(question);

        if(result){

            return result;

        }

    }


    return "I don't have a reliable answer for that yet. Try asking the same idea in different words, and I'll search the knowledge source for it.";

}


/* =========================
   SEND MESSAGE
========================= */

async function sendMessage(){

    const question =
        input.value.trim();


    if(!question){
        return;
    }


    add(
        question,
        "user"
    );


    input.value = "";


    memory.lastQuestion =
        question;


    const processing =
        document.createElement("div");


    processing.className =
        "msg jarvis";


    processing.textContent =
        "Processing...";


    chat.appendChild(
        processing
    );


    chat.scrollTop =
        chat.scrollHeight;


    let answerText;


    try{

        answerText =
            await answer(question);

    }catch(error){

        answerText =
            "I hit a temporary knowledge-system error, but the main JARVIS system is still online.";

    }


    processing.remove();


    add(
        answerText,
        "jarvis"
    );


    speak(
        answerText
    );

}


/* =========================
   BUTTON
========================= */

send.addEventListener(
    "click",
    sendMessage
);


/* =========================
   ENTER KEY
========================= */

input.addEventListener(
    "keydown",
    function(event){

        if(event.key === "Enter"){

            event.preventDefault();

            sendMessage();

        }

    }
);


/* =========================
   STARTUP
========================= */

add(
    "JARVIS online. Ask me naturally about science, mathematics, history, people, Earth, space, or anything you're curious about.",
    "jarvis"
);

})();

</script>

</body>
</html>
