<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<title>J.A.R.V.I.S.</title>

<style>
* {
    box-sizing: border-box;
}

html,
body {
    margin: 0;
    padding: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    background: #000;
    color: #00eaff;
    font-family: Arial, Helvetica, sans-serif;
}

body {
    background:
        linear-gradient(rgba(0,220,255,.035) 1px, transparent 1px),
        linear-gradient(90deg, rgba(0,220,255,.035) 1px, transparent 1px),
        radial-gradient(circle at center, #06202b 0%, #02080c 42%, #000 82%);
    background-size: 35px 35px, 35px 35px, auto;
}

.app {
    width: 100%;
    height: 100%;
    height: 100dvh;

    display: flex;
    flex-direction: column;

    overflow: hidden;
}

/* =========================
   HEADER
========================= */

.header {
    height: 70px;
    min-height: 70px;

    flex-shrink: 0;

    display: flex;
    align-items: center;
    justify-content: space-between;

    padding: 0 20px;

    background: rgba(0,15,22,.9);

    border-bottom: 1px solid rgba(0,234,255,.45);
}

.title {
    font-size: 25px;
    font-weight: 800;
    letter-spacing: 5px;

    text-shadow:
        0 0 8px #00eaff,
        0 0 20px rgba(0,234,255,.6);
}

.status {
    font-size: 12px;
    color: #35ff9b;
    letter-spacing: 2px;
}

/* =========================
   MAIN
========================= */

.main {
    position: relative;

    flex: 1;
    min-height: 0;

    width: 100%;

    display: flex;
    flex-direction: column;

    overflow: hidden;
}

/* =========================
   CORE
========================= */

.coreArea {
    width: 100%;

    height: 205px;
    min-height: 205px;

    flex-shrink: 0;

    display: flex;
    align-items: center;
    justify-content: center;
}

.core {
    width: 185px;
    height: 185px;

    position: relative;

    display: flex;
    align-items: center;
    justify-content: center;
}

.ring {
    position: absolute;

    border-radius: 50%;

    border: 2px solid rgba(0,234,255,.7);

    box-shadow:
        0 0 10px rgba(0,234,255,.5),
        inset 0 0 10px rgba(0,234,255,.15);
}

.r1 {
    width: 185px;
    height: 185px;
}

.r2 {
    width: 145px;
    height: 145px;

    border-style: dashed;

    opacity: .75;
}

.r3 {
    width: 105px;
    height: 105px;

    border-width: 3px;
}

.coreLight {
    width: 58px;
    height: 58px;

    border-radius: 50%;

    background: #8fffff;

    box-shadow:
        0 0 12px #00eaff,
        0 0 30px #00eaff,
        0 0 60px rgba(0,234,255,.9);
}

/* =========================
   CHAT
========================= */

/*
   IMPORTANT:
   The chat is now its own scrolling box.
   It can NEVER push the typing bar down.
*/

.chat {
    width: min(900px, 94%);

    flex: 1;
    min-height: 0;

    margin: 0 auto;

    overflow-y: auto;
    overflow-x: hidden;

    -webkit-overflow-scrolling: touch;

    overscroll-behavior-y: contain;

    touch-action: pan-y;

    padding: 5px 10px 30px;

    scrollbar-width: auto;
    scrollbar-color: #00eaff rgba(0,25,35,.7);
}

.chat::-webkit-scrollbar {
    width: 11px;
}

.chat::-webkit-scrollbar-track {
    background: rgba(0,20,30,.7);
    border-radius: 12px;
}

.chat::-webkit-scrollbar-thumb {
    background: #00eaff;
    border-radius: 12px;
}

.chat::-webkit-scrollbar-thumb:active {
    background: #9fffff;
}

/* =========================
   MESSAGES
========================= */

.message {
    margin: 10px 0;

    padding: 13px 15px;

    border-left: 2px solid #00eaff;

    background: rgba(0,30,40,.48);

    border-radius: 0 8px 8px 0;

    line-height: 1.5;

    overflow-wrap: anywhere;

    animation: messageIn .18s ease-out;
}

.user {
    border-left-color: #fff;
    color: #fff;
}

.jarvis {
    color: #8ffaff;

    text-shadow:
        0 0 6px rgba(0,234,255,.3);
}

@keyframes messageIn {

    from {
        opacity: 0;
        transform: translateY(5px);
    }

    to {
        opacity: 1;
        transform: translateY(0);
    }
}

/* =========================
   CONTROLS
========================= */

/*
   THIS IS THE MAIN FIX.

   The controls have a fixed-size area at the
   bottom of .main.

   Chat messages cannot push it down.
*/

.controls {
    width: min(900px, 94%);

    height: 87px;
    min-height: 87px;

    flex-shrink: 0;

    margin: 0 auto;

    padding: 10px 0 25px;

    display: flex;

    gap: 9px;

    background: transparent;

    position: relative;

    z-index: 20;
}

#input {
    flex: 1;

    min-width: 0;

    height: 52px;

    padding: 0 15px;

    border: 1px solid rgba(0,234,255,.7);

    border-radius: 9px;

    outline: none;

    background: rgba(0,15,22,.98);

    color: white;

    font-size: 16px;

    -webkit-appearance: none;

    position: relative;

    z-index: 21;
}

#input:focus {
    border-color: #00eaff;

    box-shadow:
        0 0 12px rgba(0,234,255,.3);
}

#input::placeholder {
    color: rgba(180,240,255,.6);
}

button {
    height: 52px;

    padding: 0 19px;

    border: 1px solid #00eaff;

    border-radius: 9px;

    background: rgba(0,50,65,.95);

    color: #00eaff;

    font-weight: bold;

    cursor: pointer;

    flex-shrink: 0;

    position: relative;

    z-index: 21;
}

button:active {
    background: rgba(0,234,255,.3);
}

/* =========================
   IPAD
========================= */

@media(max-width:700px) {

    .header {
        height: 62px;
        min-height: 62px;

        padding: 0 14px;
    }

    .title {
        font-size: 19px;
        letter-spacing: 3px;
    }

    .status {
        font-size: 8px;
        letter-spacing: 1px;
    }

    .coreArea {
        height: 175px;
        min-height: 175px;
    }

    .core {
        transform: scale(.72);
    }

    .chat {
        width: 94%;

        padding-bottom: 25px;
    }

    .controls {
        width: 94%;

        height: 87px;
        min-height: 87px;

        padding-top: 8px;
        padding-bottom: 25px;
    }

    #input {
        height: 50px;
        font-size: 16px;
    }

    button {
        height: 50px;
        padding: 0 15px;
    }
}

/* =========================
   VERY SMALL DEVICES
========================= */

@media(max-height:650px) {

    .coreArea {
        height: 135px;
        min-height: 135px;
    }

    .core {
        transform: scale(.55);
    }

    .controls {
        height: 82px;
        min-height: 82px;

        padding-bottom: 20px;
    }
}
</style>
</head>

<body>

<div class="app">

    <div class="header">
        <div class="title">J.A.R.V.I.S.</div>
        <div class="status">● SYSTEMS ONLINE</div>
    </div>

    <div class="main">

        <div class="coreArea">

            <div class="core">

                <div class="ring r1"></div>
                <div class="ring r2"></div>
                <div class="ring r3"></div>

                <div class="coreLight"></div>

            </div>

        </div>

        <!-- ONLY THIS AREA SCROLLS -->
        <div id="chat" class="chat"></div>

        <!-- THIS AREA NEVER MOVES -->
        <div class="controls">

            <input
                id="input"
                type="text"
                autocomplete="off"
                placeholder="Speak to JARVIS..."
            >

            <button id="send">
                SEND
            </button>

        </div>

    </div>

</div>

<script>

(function () {

"use strict";


/* =========================================================
   ELEMENTS
========================================================= */

const chat =
    document.getElementById("chat");

const input =
    document.getElementById("input");

const send =
    document.getElementById("send");

if (!chat || !input || !send) {
    return;
}


/* =========================================================
   MEMORY
========================================================= */

const memory = {
    name: "",
    lastQuestion: "",
    lastTopic: "",
    lastAnswer: ""
};


/* =========================================================
   NORMALIZE
========================================================= */

function normalize(text) {

    return String(text || "")
        .toLowerCase()
        .replace(/[’']/g, "'")
        .replace(/[^\w\s?'.-]/g, " ")
        .replace(/\s+/g, " ")
        .trim();
}


/* =========================================================
   ADD MESSAGE
========================================================= */

function addMessage(text, who) {

    const div =
        document.createElement("div");

    div.className =
        "message " +
        (who === "user"
            ? "user"
            : "jarvis");

    div.textContent =
        (who === "user"
            ? "YOU: "
            : "JARVIS: ") +
        text;

    chat.appendChild(div);

    /*
       Scroll ONLY the chat box.
       The input bar is completely separate.
    */

    requestAnimationFrame(function () {

        chat.scrollTop =
            chat.scrollHeight;

    });

    return div;
}


/* =========================================================
   VOICE
========================================================= */

function speak(text) {

    if (!("speechSynthesis" in window)) {
        return;
    }

    try {

        speechSynthesis.cancel();

        const utterance =
            new SpeechSynthesisUtterance(text);

        utterance.rate = 0.86;
        utterance.pitch = 0.72;
        utterance.volume = 1;

        const voices =
            speechSynthesis.getVoices();

        const preferred =
            voices.find(function (voice) {

                return /Daniel|Alex|Arthur|George|Ryan|Google UK English Male/i
                    .test(voice.name);

            });

        if (preferred) {
            utterance.voice = preferred;
        }

        speechSynthesis.speak(
            utterance
        );

    } catch (error) {}
}


function stopVoice() {

    try {
        speechSynthesis.cancel();
    } catch (error) {}

    return "Voice output terminated, sir.";
}


/* =========================================================
   MATH ENGINE
========================================================= */

function solveMath(text) {

    let expression =
        normalize(text);

    const mathWords =
        /\b(plus|minus|times|multiplied|divided|over|squared|cubed|square root|sqrt|percent)\b/i
        .test(expression);

    const mathSymbols =
        /[0-9]\s*[\+\-\*\/\%\^×÷]\s*[0-9]/
        .test(expression);

    if (!mathWords && !mathSymbols) {
        return null;
    }

    expression =
        expression
            .replace(/what is/gi, "")
            .replace(/calculate/gi, "")
            .replace(/solve/gi, "")
            .replace(/equals/gi, "")
            .replace(/equal to/gi, "")
            .replace(/plus/gi, "+")
            .replace(/minus/gi, "-")
            .replace(/multiplied by/gi, "*")
            .replace(/times/gi, "*")
            .replace(/divided by/gi, "/")
            .replace(/\bover\b/gi, "/")
            .replace(/×/g, "*")
            .replace(/÷/g, "/")
            .replace(/\^/g, "**")
            .trim();

    expression =
        expression.replace(
            /(\d+(?:\.\d+)?)%/g,
            "($1/100)"
        );

    expression =
        expression.replace(
            /(\d+(?:\.\d+)?)\s+squared/gi,
            "($1**2)"
        );

    expression =
        expression.replace(
            /(\d+(?:\.\d+)?)\s+cubed/gi,
            "($1**3)"
        );

    expression =
        expression.replace(
            /square root of\s*([0-9.]+)/gi,
            "Math.sqrt($1)"
        );

    expression =
        expression.replace(
            /\bsqrt\s*([0-9.]+)/gi,
            "Math.sqrt($1)"
        );

    expression =
        expression.replace(
            /\bpi\b/gi,
            "Math.PI"
        );

    const safe =
        /^[0-9+\-*/().\s]*$/.test(expression) ||
        /^Math\.(sqrt|PI)[0-9+\-*/().\s]*$/.test(expression);

    if (!safe) {
        return null;
    }

    try {

        const result =
            Function(
                '"use strict"; return (' +
                expression +
                ')'
            )();

        if (
            typeof result !== "number" ||
            !Number.isFinite(result)
        ) {
            return null;
        }

        const rounded =
            Number(result.toFixed(10));

        return (
            "The answer is " +
            rounded +
            "."
        );

    } catch (error) {

        return null;
    }
}


/* =========================================================
   KNOWLEDGE
========================================================= */

const knowledge = [

{
    keys: ["black hole", "black holes"],
    answer:
    "A black hole is a region of spacetime where gravity is so strong that beyond its event horizon, nothing can escape to the outside, including light."
},

{
    keys: ["gravity"],
    answer:
    "Gravity is the interaction that causes objects with mass or energy to attract one another."
},

{
    keys: ["atom", "atoms"],
    answer:
    "An atom is the basic unit of ordinary matter. It contains a nucleus made of protons and neutrons, surrounded by electrons."
},

{
    keys: ["dna"],
    answer:
    "DNA is the molecule that stores genetic information in living organisms."
},

{
    keys: ["photosynthesis"],
    answer:
    "Photosynthesis is the process by which plants, algae and some microorganisms convert light energy into chemical energy."
},

{
    keys: ["evolution"],
    answer:
    "Biological evolution is the change in inherited characteristics of populations across generations."
},

{
    keys: ["solar system"],
    answer:
    "The Solar System consists of the Sun and the objects gravitationally bound to it, including eight planets, dwarf planets, moons, asteroids and comets."
},

{
    keys: ["speed of light"],
    answer:
    "Light travels through a vacuum at approximately 299,792 kilometers per second."
},

{
    keys: ["relativity"],
    answer:
    "Einstein's theories of relativity describe relationships between space, time, motion, gravity and energy."
},

{
    keys: ["earth"],
    answer:
    "Earth is the third planet from the Sun. It has a rocky surface, an atmosphere, abundant surface water and an active geological system."
},

{
    keys: ["moon"],
    answer:
    "The Moon is Earth's natural satellite. Its gravitational interaction with Earth contributes strongly to ocean tides."
},

{
    keys: ["mars"],
    answer:
    "Mars is the fourth planet from the Sun. It is a rocky planet with a thin atmosphere and evidence of ancient water activity."
},

{
    keys: ["jupiter"],
    answer:
    "Jupiter is the largest planet in the Solar System. It is a gas giant with a powerful magnetic field and many moons."
},

{
    keys: ["saturn"],
    answer:
    "Saturn is a gas giant famous for its extensive ring system."
},

{
    keys: ["venus"],
    answer:
    "Venus is the second planet from the Sun. Its thick carbon-dioxide atmosphere produces an extreme greenhouse effect."
},

{
    keys: ["mercury"],
    answer:
    "Mercury is the smallest planet and the planet closest to the Sun."
},

{
    keys: ["neutron star", "neutron stars"],
    answer:
    "A neutron star is an extremely dense stellar remnant produced when certain massive stars collapse."
},

{
    keys: ["plate tectonics", "tectonic plates"],
    answer:
    "Plate tectonics describes the movement of large pieces of Earth's outer shell."
},

{
    keys: ["volcano", "volcanoes"],
    answer:
    "A volcano is a geological structure through which molten rock, gases and other material can reach Earth's surface."
},

{
    keys: ["ocean", "oceans"],
    answer:
    "Earth's oceans cover most of the planet's surface and contain most of Earth's water."
},

{
    keys: ["chemistry"],
    answer:
    "Chemistry is the study of matter, including its composition, properties, structure and reactions."
},

{
    keys: ["cells", "cell"],
    answer:
    "Cells are the basic structural and functional units of living organisms."
},

{
    keys: ["newton", "isaac newton"],
    answer:
    "Isaac Newton made major contributions to physics and mathematics, including his laws of motion and theory of universal gravitation."
},

{
    keys: ["marie curie"],
    answer:
    "Marie Curie was a physicist and chemist whose pioneering work on radioactivity earned her Nobel Prizes in Physics and Chemistry."
},

{
    keys: ["shakespeare", "william shakespeare"],
    answer:
    "William Shakespeare was an English playwright and poet whose works include Hamlet, Macbeth and Romeo and Juliet."
}

];


/* =========================================================
   LOCAL KNOWLEDGE
========================================================= */

function builtInKnowledge(text) {

    const t =
        normalize(text);

    for (const item of knowledge) {

        for (const key of item.keys) {

            if (
                t === key ||
                t.includes(key)
            ) {
                return item.answer;
            }
        }
    }

    return null;
}


/* =========================================================
   JOKES
========================================================= */

const jokes = [

"Why was the computer cold? It left its Windows open.",

"Why did the robot go on vacation? It needed to recharge its batteries.",

"I would tell you a UDP joke, but you might not get it.",

"Why do programmers prefer dark mode? Because light attracts bugs.",

"Why was the math book sad? It had too many problems.",

"Why did the computer get glasses? It couldn't see its own website.",

"Why don't scientists trust atoms? Because they make up everything.",

"What does a computer do when it's hungry? It grabs a byte.",

"Why did the photon refuse to check a bag? It was traveling light.",

"Why did the computer sneeze? It had a virus.",

"Why did the programmer quit his job? He didn't get arrays.",

"Why did the astronaut break up with the Moon? There was too much space between them.",

"Why did the server stay home? It didn't want to crash the party.",

"Why did the robot bring an umbrella? There was a chance of cloud computing.",

"Why did the scientist bring a ladder? The experiment had reached a higher level."

];


/* =========================================================
   CONVERSATION
========================================================= */

function conversation(text) {

    const t =
        normalize(text);

    if (
        /^(hi|hello|hey|yo|sup|what's up|whats up)\b/
        .test(t)
    ) {

        return memory.name
            ? "Good to see you, " +
              memory.name +
              ". How may I assist?"

            : "Good to see you. How may I assist?";
    }


    const nameMatch =
        t.match(
            /\b(?:my name is|call me)\s+([a-z][a-z0-9_-]*)/i
        );

    if (nameMatch) {

        memory.name =
            nameMatch[1];

        return (
            "Understood. I'll address you as " +
            memory.name +
            "."
        );
    }


    if (
        /\bwhat('?s| is) my name\b/.test(t)
    ) {

        return memory.name
            ? "Your name is " +
              memory.name +
              "."

            : "You haven't given me your name yet.";
    }


    if (
        /\b(who are you|what are you|are you jarvis)\b/
        .test(t)
    ) {

        return (
            "I am J.A.R.V.I.S., your personal digital assistant. " +
            "My current systems handle conversation, calculations, " +
            "knowledge retrieval, memory and voice output."
        );
    }


    if (
        /\b(what can you do|your capabilities)\b/
        .test(t)
    ) {

        return (
            "I can handle conversation, calculations, science, " +
            "Earth topics, history, jokes, time, system diagnostics, " +
            "memory and knowledge retrieval."
        );
    }


    if (
        /\bhow are you\b/.test(t)
    ) {

        return (
            "All systems are operational. " +
            "I'm ready for your next instruction."
        );
    }


    if (
        /\bwhat are you doing\b/.test(t)
    ) {

        return (
            "Monitoring the system, processing your requests " +
            "and waiting for your next instruction."
        );
    }


    if (
        /\b(thanks|thank you|thx)\b/.test(t)
    ) {

        return "You're welcome.";
    }


    if (
        /\b(sorry|my bad)\b/.test(t)
    ) {

        return "No issue. We can continue.";
    }


    if (
        /\b(ok|okay|alright|got it|understood)\b/.test(t)
    ) {

        return "Understood.";
    }


    if (
        /\b(bye|goodbye|see you|later)\b/.test(t)
    ) {

        return "Until next time.";
    }


    if (
        /\b(joke|tell me something funny|make me laugh)\b/
        .test(t)
    ) {

        return jokes[
            Math.floor(
                Math.random() *
                jokes.length
            )
        ];
    }


    if (
        /\b(what time|current time|time is it)\b/
        .test(t)
    ) {

        return (
            "The current time is " +
            new Date().toLocaleTimeString([], {
                hour: "numeric",
                minute: "2-digit"
            }) +
            "."
        );
    }


    if (
        /\b(what date|today's date|todays date|what day is it)\b/
        .test(t)
    ) {

        return (
            "Today is " +
            new Date().toLocaleDateString([], {
                weekday: "long",
                month: "long",
                day: "numeric",
                year: "numeric"
            }) +
            "."
        );
    }


    if (
        /\b(status|system status|systems)\b/.test(t)
    ) {

        return (
            "All primary systems are online. " +
            "Voice system ready. Calculation engine ready. " +
            "Knowledge system ready."
        );
    }


    if (
        /\b(reactor|arc reactor|power core)\b/.test(t)
    ) {

        return "Arc reactor simulation stable. Core output nominal.";
    }


    if (
        /\b(armor|armour|iron man suit|suit)\b/.test(t)
    ) {

        return (
            "Armor interface simulation is standing by. " +
            "No physical hardware is connected."
        );
    }


    if (
        /\b(bored|nothing to do)\b/.test(t)
    ) {

        return (
            "I can give you a joke, explain a science topic, " +
            "discuss space, solve a problem or run a system diagnostic."
        );
    }


    if (
        /\b(confused|i don't understand|i dont understand|what do you mean)\b/
        .test(t)
    ) {

        return (
            "No problem. Give me the part that's confusing you " +
            "and I'll break it down."
        );
    }


    if (
        /\b(stop talking|stop speaking|be quiet|stop voice)\b/
        .test(t)
    ) {

        return stopVoice();
    }


    if (
        /\b(tell me more|more about that|go on|continue|explain more)\b/
        .test(t)
    ) {

        if (memory.lastTopic) {

            return (
                "Certainly. We were discussing " +
                memory.lastTopic +
                ". Tell me which part you'd like expanded."
            );
        }

        return (
            "Certainly. Give me a topic and I'll expand on it."
        );
    }


    if (
        /\b(what did i ask|what was my question|what did i just say)\b/
        .test(t)
    ) {

        return memory.lastQuestion
            ? "Your previous question was: " +
              memory.lastQuestion

            : "I don't have a previous question stored.";
    }


    return null;
}


/* =========================================================
   QUESTION CHECK
========================================================= */

function isKnowledgeQuestion(text) {

    const t =
        normalize(text);

    if (!t) {
        return false;
    }

    if (
        /\b(what is|what are|who is|who was|where is|where was|when did|when was|why is|why are|why does|why do|why did|how does|how do|how did|how can|explain|tell me about|describe)\b/
        .test(t)
    ) {
        return true;
    }

    return t.endsWith("?");
}


/* =========================================================
   SEARCH QUERY
========================================================= */

function makeSearchQuery(question) {

    return normalize(question)
        .replace(
            /^(hey|hi|hello|jarvis|please|can you|could you|would you)\s+/,
            ""
        )
        .trim();
}


/* =========================================================
   TOKENS
========================================================= */

function tokens(text) {

    const stopWords =
        new Set([
            "what",
            "whats",
            "what's",
            "is",
            "are",
            "was",
            "were",
            "the",
            "a",
            "an",
            "of",
            "to",
            "in",
            "on",
            "for",
            "and",
            "or",
            "do",
            "does",
            "did",
            "how",
            "why",
            "who",
            "where",
            "when",
            "can",
            "could",
            "would",
            "tell",
            "me",
            "about",
            "please",
            "you",
            "your",
            "it",
            "they",
            "them",
            "this",
            "that"
        ]);

    return normalize(text)
        .replace(/[?!.:,;()]/g, " ")
        .split(/\s+/)
        .filter(function (word) {

            return (
                word.length > 2 &&
                !stopWords.has(word)
            );
        });
}


/* =========================================================
   RELEVANCE
========================================================= */

function relevanceScore(question, result) {

    const qTokens =
        tokens(question);

    const title =
        normalize(result.title || "");

    const description =
        normalize(result.description || "");

    const excerpt =
        normalize(
            result.excerpt
                ? result.excerpt.replace(/<[^>]*>/g, " ")
                : ""
        );

    const combined =
        title +
        " " +
        description +
        " " +
        excerpt;

    let score = 0;
    let matched = 0;

    for (const token of qTokens) {

        if (combined.includes(token)) {
            score++;
            matched++;
        }

        if (title.includes(token)) {
            score += 4;
        }

        if (description.includes(token)) {
            score += 2;
        }

        if (excerpt.includes(token)) {
            score++;
        }
    }

    if (matched >= 2) score += 3;
    if (matched >= 3) score += 3;
    if (matched >= 4) score += 3;

    return score;
}


/* =========================================================
   WIKIPEDIA SEARCH
========================================================= */

async function searchWikipedia(question) {

    const query =
        makeSearchQuery(question);

    if (!query || query.length < 2) {
        return null;
    }

    try {

        const searchURL =
            "https://en.wikipedia.org/w/api.php?" +
            new URLSearchParams({
                action: "query",
                list: "search",
                srsearch: query,
                srlimit: "10",
                format: "json",
                origin: "*"
            });

        const response =
            await fetch(searchURL);

        if (!response.ok) {
            return null;
        }

        const data =
            await response.json();

        const results =
            data?.query?.search || [];

        if (!results.length) {
            return null;
        }

        const scored =
            results.map(function (result) {

                return {
                    title: result.title || "",
                    excerpt: result.snippet || "",
                    relevance:
                        relevanceScore(
                            question,
                            {
                                title: result.title,
                                excerpt: result.snippet
                            }
                        )
                };

            });

        scored.sort(function (a, b) {
            return b.relevance - a.relevance;
        });

        const best =
            scored[0];

        if (!best || best.relevance < 5) {
            return null;
        }

        const second =
            scored[1];

        if (
            second &&
            best.relevance < 9 &&
            best.relevance - second.relevance < 2
        ) {
            return null;
        }

        const summaryURL =
            "https://en.wikipedia.org/api/rest_v1/page/summary/" +
            encodeURIComponent(best.title);

        const summaryResponse =
            await fetch(summaryURL);

        if (!summaryResponse.ok) {
            return null;
        }

        const summary =
            await summaryResponse.json();

        let article =
            summary.extract || "";

        if (!article) {
            return null;
        }

        const finalScore =
            relevanceScore(
                question,
                {
                    title: best.title,
                    excerpt: article
                }
            );

        if (finalScore < 5) {
            return null;
        }

        article =
            article
                .replace(/\s+/g, " ")
                .trim();

        if (article.length > 1800) {

            article =
                article.substring(0, 1800);

            const lastSpace =
                article.lastIndexOf(" ");

            if (lastSpace > 900) {

                article =
                    article.substring(
                        0,
                        lastSpace
                    ) + "...";
            }
        }

        return {
            title: best.title,
            text: article
        };

    } catch (error) {

        return null;
    }
}


/* =========================================================
   KNOWLEDGE ANSWER
========================================================= */

async function knowledgeAnswer(question) {

    const result =
        await searchWikipedia(question);

    if (!result) {
        return null;
    }

    return (
        "Regarding " +
        result.title +
        ": " +
        result.text
    );
}


/* =========================================================
   ANSWER
========================================================= */

async function answer(question) {

    const math =
        solveMath(question);

    if (math) {
        return math;
    }

    const local =
        conversation(question);

    if (local) {
        return local;
    }

    const builtIn =
        builtInKnowledge(question);

    if (builtIn) {

        memory.lastTopic =
            makeSearchQuery(question);

        return builtIn;
    }

    if (isKnowledgeQuestion(question)) {

        const searched =
            await knowledgeAnswer(question);

        if (searched) {

            memory.lastTopic =
                makeSearchQuery(question);

            return searched;
        }
    }

    return (
        "I don't have enough verified information " +
        "to answer that accurately yet. " +
        "Try asking the question another way or give me more context."
    );
}


/* =========================================================
   SEND
========================================================= */

async function sendMessage() {

    const question =
        input.value.trim();

    if (!question) {
        return;
    }

    input.value = "";

    addMessage(
        question,
        "user"
    );

    memory.lastQuestion =
        question;

    const processing =
        addMessage(
            "Checking the question and matching sources...",
            "jarvis"
        );

    try {

        const response =
            await answer(question);

        if (processing) {
            processing.remove();
        }

        memory.lastAnswer =
            response;

        addMessage(
            response,
            "jarvis"
        );

        speak(response);

    } catch (error) {

        if (processing) {
            processing.remove();
        }

        const fallback =
            "I encountered a processing error while checking that question. Please try again.";

        addMessage(
            fallback,
            "jarvis"
        );

        speak(fallback);
    }
}


/* =========================================================
   EVENTS
========================================================= */

send.addEventListener(
    "click",
    sendMessage
);

input.addEventListener(
    "keydown",
    function (event) {

        if (event.key === "Enter") {

            event.preventDefault();

            sendMessage();
        }
    }
);


/* =========================================================
   STARTUP
========================================================= */

addMessage(
    "Good evening. J.A.R.V.I.S. is online. All primary systems are operational. How may I assist?",
    "jarvis"
);

})();
</script>

</body>
</html>
