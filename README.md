<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>J.A.R.V.I.S.</title>

<style>
* {
    box-sizing: border-box;
}

html, body {
    margin: 0;
    width: 100%;
    height: 100%;
    background: #000;
    color: #00eaff;
    font-family: Arial, Helvetica, sans-serif;
    overflow: hidden;
}

body {
    background:
        linear-gradient(rgba(0,220,255,.035) 1px, transparent 1px),
        linear-gradient(90deg, rgba(0,220,255,.035) 1px, transparent 1px),
        radial-gradient(circle at center, #06202b 0%, #02080c 42%, #000 80%);
    background-size: 35px 35px, 35px 35px, auto;
}

.app {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
}

.header {
    height: 72px;
    border-bottom: 1px solid rgba(0,234,255,.45);
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0 20px;
    background: rgba(0,15,22,.75);
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

.main {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: 0;
}

.coreArea {
    height: 300px;
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-shrink: 0;
}

.core {
    width: 190px;
    height: 190px;
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

.chat {
    width: min(900px, 94%);
    flex: 1;
    min-height: 100px;
    overflow-y: auto;
    padding: 10px 5px 20px;
    scrollbar-width: thin;
    scrollbar-color: #00dff5 transparent;
}

.message {
    margin: 10px 0;
    padding: 12px 15px;
    border-left: 2px solid #00eaff;
    background: rgba(0,30,40,.45);
    line-height: 1.45;
    border-radius: 0 8px 8px 0;
}

.user {
    border-left-color: #fff;
    color: #fff;
}

.jarvis {
    color: #8ffaff;
    text-shadow: 0 0 6px rgba(0,234,255,.3);
}

.controls {
    width: min(900px, 94%);
    display: flex;
    gap: 8px;
    padding: 12px 0 18px;
    flex-shrink: 0;
}

#input {
    flex: 1;
    min-width: 0;
    padding: 14px;
    border: 1px solid rgba(0,234,255,.6);
    border-radius: 8px;
    outline: none;
    background: rgba(0,15,22,.9);
    color: white;
    font-size: 16px;
}

#input:focus {
    box-shadow: 0 0 12px rgba(0,234,255,.3);
}

button {
    padding: 0 18px;
    border: 1px solid #00eaff;
    border-radius: 8px;
    background: rgba(0,50,65,.7);
    color: #00eaff;
    font-weight: bold;
    cursor: pointer;
}

button:active {
    background: rgba(0,234,255,.25);
}

@media(max-width:600px) {

    .title {
        font-size: 19px;
        letter-spacing: 3px;
    }

    .status {
        font-size: 9px;
    }

    .coreArea {
        height: 235px;
    }

    .core {
        transform: scale(.78);
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

        <div id="chat" class="chat"></div>

        <div class="controls">
            <input
                id="input"
                autocomplete="off"
                placeholder="Speak to JARVIS..."
                type="text"
            >

            <button id="send">SEND</button>
        </div>

    </div>
</div>

<script>
(function () {

"use strict";

/* =========================================================
   ELEMENTS
========================================================= */

const chat = document.getElementById("chat");
const input = document.getElementById("input");
const send = document.getElementById("send");

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
   NORMALIZATION
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
   CHAT
========================================================= */

function addMessage(text, who) {

    const div = document.createElement("div");

    div.className =
        "message " +
        (who === "user" ? "user" : "jarvis");

    div.textContent =
        (who === "user" ? "YOU: " : "JARVIS: ") +
        text;

    chat.appendChild(div);

    chat.scrollTop = chat.scrollHeight;

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

        const voice =
            new SpeechSynthesisUtterance(text);

        voice.rate = 0.86;
        voice.pitch = 0.72;
        voice.volume = 1;

        const voices =
            speechSynthesis.getVoices();

        const preferred =
            voices.find(v =>
                /Daniel|Alex|Arthur|George|Ryan|Google UK English Male/i
                    .test(v.name)
            );

        if (preferred) {
            voice.voice = preferred;
        }

        speechSynthesis.speak(voice);

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
   IMPORTANT: MATH NEVER GOES TO SEARCH.
========================================================= */

function solveMath(text) {

    let expression = normalize(text);

    const hasMathWords =
        /\b(plus|minus|times|multiplied|divided|over|squared|cubed|square root|sqrt|percent)\b/i
        .test(expression);

    const hasMathSymbols =
        /[0-9]\s*[\+\-\*\/\%\^×÷]\s*[0-9]/
        .test(expression);

    if (!hasMathWords && !hasMathSymbols) {
        return null;
    }

    expression = expression
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
        .replace(/divided into/gi, "/")
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
            /(\d+(?:\.\d+)?)\s*(?:squared)/gi,
            "($1**2)"
        );

    expression =
        expression.replace(
            /(\d+(?:\.\d+)?)\s*(?:cubed)/gi,
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

    /*
       Reject anything that isn't mathematical syntax.
    */

    const allowed =
        /^[0-9+\-*/().\s]*$/.test(expression) ||
        /^Math\.(sqrt|PI)[0-9+\-*/().\s]*$/.test(expression);

    if (!allowed) {
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
   BUILT-IN KNOWLEDGE
========================================================= */

const knowledge = [

{
    keys: ["black hole", "black holes"],
    answer:
    "A black hole is a region of spacetime where gravity is so strong that, beyond its event horizon, nothing can escape to the outside, including light. Black holes can form from the collapse of massive stars, while supermassive black holes exist at the centers of galaxies."
},

{
    keys: ["gravity"],
    answer:
    "Gravity is the interaction that causes objects with mass or energy to attract one another. On Earth, gravity pulls objects toward the planet's center and helps keep the Moon in orbit."
},

{
    keys: ["atom", "atoms"],
    answer:
    "An atom is the basic unit of ordinary matter. It contains a nucleus made of protons and neutrons, surrounded by electrons."
},

{
    keys: ["dna"],
    answer:
    "DNA is the molecule that stores genetic information in living organisms. The sequence of its chemical bases carries biological instructions used by cells."
},

{
    keys: ["photosynthesis"],
    answer:
    "Photosynthesis is the process by which plants, algae and some microorganisms convert light energy into chemical energy. Plants generally use carbon dioxide and water and release oxygen."
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
    "Mars is the fourth planet from the Sun. It is a rocky planet with a thin atmosphere, polar ice deposits, enormous volcanoes and evidence of ancient water activity."
},

{
    keys: ["jupiter"],
    answer:
    "Jupiter is the largest planet in the Solar System. It is a gas giant with a powerful magnetic field and many moons."
},

{
    keys: ["saturn"],
    answer:
    "Saturn is a gas giant famous for its extensive ring system. The rings are composed primarily of ice particles mixed with rocky material."
},

{
    keys: ["venus"],
    answer:
    "Venus is the second planet from the Sun. Its thick carbon-dioxide atmosphere produces an extreme greenhouse effect and makes its surface extremely hot."
},

{
    keys: ["mercury"],
    answer:
    "Mercury is the smallest planet and the planet closest to the Sun. It has a heavily cratered surface and a very thin exosphere."
},

{
    keys: ["neutron star", "neutron stars"],
    answer:
    "A neutron star is an extremely dense stellar remnant produced when certain massive stars collapse. It can contain roughly the mass of the Sun compressed into a city-sized object."
},

{
    keys: ["plate tectonics", "tectonic plates", "tectonic plate"],
    answer:
    "Plate tectonics describes the movement of large pieces of Earth's outer shell. Their interactions help produce earthquakes, volcanoes, mountain ranges and ocean basins."
},

{
    keys: ["volcano", "volcanoes"],
    answer:
    "A volcano is a geological structure through which molten rock, gases and other material can reach or erupt onto Earth's surface."
},

{
    keys: ["ocean", "oceans"],
    answer:
    "Earth's oceans cover most of the planet's surface and contain most of Earth's water. They strongly influence weather, climate and marine ecosystems."
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
    "Einstein's theories of relativity describe relationships between space, time, motion, gravity and energy. Special relativity concerns high-speed motion, while general relativity describes gravity through curved spacetime."
},

{
    keys: ["evolution"],
    answer:
    "Biological evolution is the change in inherited characteristics of populations across generations. Natural selection is one mechanism that can produce evolutionary change."
},

{
    keys: ["chemistry"],
    answer:
    "Chemistry is the study of matter, including its composition, properties, structure and the reactions through which substances change."
},

{
    keys: ["cells", "cell"],
    answer:
    "Cells are the basic structural and functional units of living organisms. Some organisms consist of a single cell, while humans are made of trillions of cells."
},

{
    keys: ["newton", "isaac newton"],
    answer:
    "Isaac Newton made major contributions to physics and mathematics. His laws of motion and theory of universal gravitation became foundations of classical mechanics."
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
   BUILT-IN KNOWLEDGE MATCHING
========================================================= */

function builtInKnowledge(text) {

    const t = normalize(text);

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

"Why did the robot cross the road? Its navigation system calculated that it was the optimal route.",

"Why don't scientists trust atoms? Because they make up everything.",

"What does a computer do when it's hungry? It grabs a byte.",

"Why did the photon refuse to check a bag? It was traveling light.",

"Why did the computer sneeze? It had a virus.",

"Why did the programmer quit his job? He didn't get arrays.",

"Why was the calculator confident? It could always count on itself.",

"Why did the astronaut break up with the Moon? There was too much space between them.",

"Why did the server stay home? It didn't want to crash the party.",

"Why did the robot bring an umbrella? There was a chance of cloud computing.",

"Why did the scientist bring a ladder? The experiment had reached a higher level.",

"Why did the AI remain calm? It had excellent processing discipline.",

"Why did the computer go to therapy? It had too many unresolved processes.",

"Why did the programmer wear glasses? Because he couldn't C#."
];


/* =========================================================
   LOCAL CONVERSATION
========================================================= */

function conversation(text) {

    const t = normalize(text);

    if (
        /^(hi|hello|hey|yo|sup|what's up|whats up)\b/.test(t)
    ) {
        return memory.name
            ? "Good to see you, " + memory.name + ". How may I assist?"
            : "Good to see you. How may I assist?";
    }

    const nameMatch =
        t.match(
            /\b(?:my name is|call me)\s+([a-z][a-z0-9_-]*)/i
        );

    if (nameMatch) {

        memory.name = nameMatch[1];

        return (
            "Understood. I'll address you as " +
            memory.name +
            "."
        );
    }

    if (
        /\bwhat('?s| is) my name\b/.test(t) ||
        /\bdo you know my name\b/.test(t)
    ) {

        return memory.name
            ? "Your name is " + memory.name + "."
            : "You haven't given me your name yet.";
    }

    if (
        /\b(who are you|what are you|are you jarvis)\b/.test(t)
    ) {
        return "I am J.A.R.V.I.S., your personal digital assistant. My current systems handle conversation, calculations, knowledge retrieval, memory and voice output.";
    }

    if (
        /\b(what can you do|your capabilities)\b/.test(t)
    ) {
        return "I can handle conversation, calculations, science, Earth topics, history, jokes, time, system diagnostics, memory and knowledge retrieval.";
    }

    if (
        /\bhow are you\b/.test(t)
    ) {
        return "All systems are operational. I'm ready for your next instruction.";
    }

    if (
        /\bwhat are you doing\b/.test(t)
    ) {
        return "Monitoring the system, processing your requests and waiting for your next instruction.";
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
        /\b(joke|tell me something funny|make me laugh)\b/.test(t)
    ) {
        return jokes[
            Math.floor(Math.random() * jokes.length)
        ];
    }

    if (
        /\b(what time|current time|time is it)\b/.test(t)
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
        /\b(what date|today's date|todays date|what day is it)\b/.test(t)
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
        return "All primary systems are online. Voice system ready. Calculation engine ready. Knowledge system ready.";
    }

    if (
        /\b(reactor|arc reactor|power core)\b/.test(t)
    ) {
        return "Arc reactor simulation stable. Core output nominal.";
    }

    if (
        /\b(armor|armour|iron man suit|suit)\b/.test(t)
    ) {
        return "Armor interface simulation is standing by. No physical hardware is connected.";
    }

    if (
        /\b(bored|nothing to do)\b/.test(t)
    ) {
        return "I can give you a joke, explain a science topic, discuss space, solve a problem or run a system diagnostic.";
    }

    if (
        /\b(confused|i don't understand|i dont understand|what do you mean)\b/.test(t)
    ) {
        return "No problem. Give me the part that's confusing you and I'll break it down.";
    }

    if (
        /\b(stop talking|stop speaking|be quiet|stop voice)\b/.test(t)
    ) {
        return stopVoice();
    }

    if (
        /\b(tell me more|more about that|go on|continue|explain more)\b/.test(t)
    ) {

        if (memory.lastAnswer) {

            return (
                "Certainly. The previous topic was " +
                memory.lastTopic +
                ". Tell me which part you'd like expanded."
            );
        }

        return "Certainly. Give me a topic and I'll expand on it.";
    }

    if (
        /\b(what did i ask|what was my question|what did i just say)\b/.test(t)
    ) {

        return memory.lastQuestion
            ? "Your previous question was: " +
              memory.lastQuestion
            : "I don't have a previous question stored.";
    }

    return null;
}


/* =========================================================
   QUESTION DETECTION
========================================================= */

function isKnowledgeQuestion(text) {

    const t = normalize(text);

    if (!t) {
        return false;
    }

    if (
        /\b(what is|what are|who is|who was|where is|where was|when did|when was|why is|why are|why does|why do|why did|how does|how do|how did|how can|explain|tell me about|describe)\b/
        .test(t)
    ) {
        return true;
    }

    if (t.endsWith("?")) {
        return true;
    }

    return false;
}


/* =========================================================
   REMOVE QUESTION FILLER
========================================================= */

function makeSearchQuery(question) {

    let q = normalize(question);

    q = q
        .replace(
            /^(hey|hi|hello|jarvis|please|can you|could you|would you)\s+/,
            ""
        )
        .replace(
            /^(tell me|tell me about|explain|describe)\s+/,
            ""
        )
        .trim();

    /*
       Keep important question words.
       We DON'T reduce everything to one keyword.
       The complete subject + relationship matters.
    */

    return q;
}


/* =========================================================
   TOKENIZE
========================================================= */

function tokens(text) {

    const stopWords = new Set([
        "what",
        "what's",
        "whats",
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
        .filter(word =>
            word.length > 2 &&
            !stopWords.has(word)
        );
}


/* =========================================================
   RELEVANCE SCORING
========================================================= */

function relevanceScore(question, result) {

    const questionTokens =
        tokens(question);

    if (questionTokens.length === 0) {
        return 0;
    }

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
        title + " " +
        description + " " +
        excerpt;

    const words =
        new Set(tokens(combined));

    let score = 0;

    for (const token of questionTokens) {

        if (words.has(token)) {
            score += 1;
        }

        /*
           Title matches are much more important.
        */

        if (title.includes(token)) {
            score += 3;
        }

        /*
           Description matches are stronger than
           ordinary excerpt matches.
        */

        if (description.includes(token)) {
            score += 2;
        }
    }

    /*
       Reward multiple independent matches.
    */

    const matched =
        questionTokens.filter(token =>
            combined.includes(token)
        ).length;

    if (matched >= 2) {
        score += 3;
    }

    if (matched >= 3) {
        score += 3;
    }

    if (matched >= 4) {
        score += 3;
    }

    return score;
}


/* =========================================================
   SEARCH WIKIPEDIA
========================================================= */

async function searchWikipedia(question) {

    const query =
        makeSearchQuery(question);

    if (!query || query.length < 2) {
        return null;
    }

    try {

        /*
           Wikimedia's REST search endpoint returns
           multiple pages instead of us blindly taking
           the first result.
        */

        const url =
            "https://en.wikipedia.org/w/rest.php/v1/search/page?" +
            new URLSearchParams({
                q: query,
                limit: "10"
            });

        const response =
            await fetch(url);

        if (!response.ok) {
            return null;
        }

        const data =
            await response.json();

        const pages =
            Array.isArray(data.pages)
                ? data.pages
                : [];

        if (pages.length === 0) {
            return null;
        }

        /*
           Score EVERY result.
        */

        const scored =
            pages.map(page => ({
                ...page,
                relevance:
                    relevanceScore(question, page)
            }));

        scored.sort(
            (a, b) =>
                b.relevance - a.relevance
        );

        /*
           The best result must actually have
           meaningful overlap with the question.
        */

        const best =
            scored[0];

        if (!best || best.relevance < 5) {
            return null;
        }

        /*
           If the top result barely beats the
           second result, require more confidence.
        */

        const second =
            scored[1];

        if (
            second &&
            best.relevance < 8 &&
            best.relevance - second.relevance < 2
        ) {
            return null;
        }

        /*
           Fetch the best article.
        */

        const title =
            best.title;

        if (!title) {
            return null;
        }

        const pageURL =
            "https://en.wikipedia.org/w/rest.php/v1/page/" +
            encodeURIComponent(title);

        const pageResponse =
            await fetch(pageURL);

        if (!pageResponse.ok) {
            return null;
        }

        const pageData =
            await pageResponse.json();

        /*
           Different versions of the API can expose
           content differently, so check several fields.
        */

        let articleText = "";

        if (
            typeof pageData.source === "string"
        ) {
            articleText =
                pageData.source;
        }

        if (
            !articleText &&
            typeof pageData.extract === "string"
        ) {
            articleText =
                pageData.extract;
        }

        /*
           If full article content isn't available,
           use the summary endpoint.
        */

        if (!articleText) {

            const summaryURL =
                "https://en.wikipedia.org/api/rest_v1/page/summary/" +
                encodeURIComponent(title);

            const summaryResponse =
                await fetch(summaryURL);

            if (summaryResponse.ok) {

                const summary =
                    await summaryResponse.json();

                articleText =
                    summary.extract || "";
            }
        }

        if (!articleText) {
            return null;
        }

        /*
           Make sure the actual article still
           contains relevant question terms.
        */

        const articleScore =
            relevanceScore(
                question,
                {
                    title: title,
                    description:
                        best.description || "",
                    excerpt:
                        articleText.substring(0, 3000)
                }
            );

        if (articleScore < 5) {
            return null;
        }

        /*
           Return only a useful amount.
        */

        articleText =
            articleText
                .replace(/\s+/g, " ")
                .trim();

        if (articleText.length > 1800) {

            articleText =
                articleText.substring(0, 1800);

            const lastSpace =
                articleText.lastIndexOf(" ");

            if (lastSpace > 900) {
                articleText =
                    articleText.substring(
                        0,
                        lastSpace
                    ) + "...";
            }
        }

        return {
            title: title,
            text: articleText,
            score: articleScore
        };

    } catch (error) {

        return null;
    }
}


/* =========================================================
   SEARCH ANSWER
========================================================= */

async function knowledgeAnswer(question) {

    const result =
        await searchWikipedia(question);

    if (!result) {
        return null;
    }

    /*
       The answer explicitly identifies the
       matching subject so JARVIS doesn't sound
       like it is answering a different question.
    */

    return (
        "Regarding " +
        result.title +
        ": " +
        result.text
    );
}


/* =========================================================
   MASTER ANSWER ENGINE
========================================================= */

async function answer(question) {

    /*
       PRIORITY ORDER

       1. Math
       2. Conversation
       3. Built-in knowledge
       4. Verified search
       5. Honest fallback

       This prevents ordinary conversation from
       accidentally triggering search.
    */

    const mathAnswer =
        solveMath(question);

    if (mathAnswer) {
        return mathAnswer;
    }


    const localConversation =
        conversation(question);

    if (localConversation) {
        return localConversation;
    }


    const localKnowledge =
        builtInKnowledge(question);

    if (localKnowledge) {

        memory.lastTopic =
            makeSearchQuery(question);

        return localKnowledge;
    }


    /*
       Only actual knowledge questions reach
       the search engine.
    */

    if (isKnowledgeQuestion(question)) {

        const result =
            await knowledgeAnswer(question);

        if (result) {

            memory.lastTopic =
                makeSearchQuery(question);

            return result;
        }
    }


    /*
       Don't invent an answer.
    */

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
