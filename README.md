<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">

<meta name="viewport"
      content="width=device-width,
      initial-scale=1,
      viewport-fit=cover,
      maximum-scale=1,
      user-scalable=no">

<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="mobile-web-app-capable" content="yes">

<title>J.A.R.V.I.S.</title>

<style>
* {
    box-sizing: border-box;
    -webkit-tap-highlight-color: transparent;
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
    position: fixed;
    inset: 0;
    overscroll-behavior: none;
}

/* APP */

.app {
    position: fixed;
    inset: 0;
    width: 100%;
    height: 100%;
    height: 100dvh;
    overflow: hidden;

    background:
        radial-gradient(circle at center,
            rgba(0,180,255,.12),
            transparent 45%),
        linear-gradient(
            rgba(0,238,255,.035) 1px,
            transparent 1px),
        linear-gradient(
            90deg,
            rgba(0,238,255,.035) 1px,
            transparent 1px),
        #000;

    background-size:
        auto,
        28px 28px,
        28px 28px,
        auto;
}

/* HEADER */

.header {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;

    height: 68px;
    padding-top: env(safe-area-inset-top);

    display: flex;
    align-items: center;
    justify-content: space-between;

    padding-left: 20px;
    padding-right: 20px;

    background: rgba(0,8,12,.94);
    border-bottom: 1px solid rgba(0,234,255,.3);

    z-index: 5000;
}

.logo {
    font-size: 23px;
    font-weight: bold;
    letter-spacing: 4px;

    text-shadow:
        0 0 8px #00eaff,
        0 0 20px rgba(0,234,255,.6);
}

.status {
    font-size: 11px;
    letter-spacing: 2px;
    color: #00ff88;
}

.status-dot {
    display: inline-block;
    width: 7px;
    height: 7px;
    margin-right: 6px;
    border-radius: 50%;
    background: #00ff88;
    box-shadow: 0 0 10px #00ff88;
}

/* MAIN */

.main {
    position: absolute;
    top: 68px;
    left: 0;
    right: 0;
    bottom: 0;

    display: flex;
    flex-direction: column;

    overflow: hidden;
}

/* REACTOR */

.coreArea {
    flex: 0 0 245px;

    display: flex;
    align-items: center;
    justify-content: center;

    position: relative;
}

.core {
    position: relative;
    width: 180px;
    height: 180px;
}

.ring {
    position: absolute;
    inset: 0;

    border: 2px solid rgba(0,234,255,.45);
    border-radius: 50%;

    box-shadow:
        0 0 15px rgba(0,234,255,.2),
        inset 0 0 15px rgba(0,234,255,.15);
}

.ring.one {
    transform: rotate(25deg) scale(.9);
    border-top-color: #00eaff;
    animation: spin 8s linear infinite;
}

.ring.two {
    transform: rotate(-40deg) scale(.72);
    border-right-color: #008cff;
    animation: spinReverse 5s linear infinite;
}

.ring.three {
    transform: rotate(70deg) scale(.55);
    border-bottom-color: #00ffff;
    animation: spin 4s linear infinite;
}

.coreOrb {
    position: absolute;
    top: 50%;
    left: 50%;

    width: 54px;
    height: 54px;

    transform: translate(-50%,-50%);

    border-radius: 50%;

    background:
        radial-gradient(circle,
            #fff 0%,
            #9fffff 15%,
            #00eaff 45%,
            #007cff 70%,
            transparent 72%);

    box-shadow:
        0 0 15px #00eaff,
        0 0 40px #00eaff,
        0 0 80px rgba(0,150,255,.8);

    animation: pulse 2s ease-in-out infinite;
}

.coreLabel {
    position: absolute;
    bottom: -25px;
    width: 100%;

    text-align: center;

    font-size: 9px;
    letter-spacing: 3px;

    color: rgba(0,234,255,.65);
}

@keyframes spin {
    from { transform: rotate(0deg); }
    to { transform: rotate(360deg); }
}

@keyframes spinReverse {
    from { transform: rotate(360deg); }
    to { transform: rotate(0deg); }
}

@keyframes pulse {
    0%,100% {
        transform: translate(-50%,-50%) scale(.9);
        opacity: .8;
    }

    50% {
        transform: translate(-50%,-50%) scale(1.1);
        opacity: 1;
    }
}

/* CHAT */

.chat {
    flex: 1;
    min-height: 0;

    overflow-y: auto;
    overflow-x: hidden;

    padding:
        10px
        16px
        150px;

    -webkit-overflow-scrolling: touch;
    overscroll-behavior: contain;
    scroll-behavior: smooth;
}

.message {
    max-width: 900px;
    margin: 0 auto 12px;

    padding: 12px 15px;

    border-radius: 10px;

    line-height: 1.45;
    font-size: 15px;

    word-wrap: break-word;
    overflow-wrap: anywhere;
}

.jarvis {
    background: rgba(0,160,220,.08);
    border-left: 2px solid #00eaff;

    box-shadow:
        inset 0 0 15px rgba(0,234,255,.04);
}

.user {
    background: rgba(255,255,255,.05);
    border-right: 2px solid rgba(255,255,255,.4);

    color: #fff;
}

.label {
    display: block;
    margin-bottom: 5px;

    font-size: 9px;
    letter-spacing: 2px;

    opacity: .55;
}

/* FIXED INPUT */

.controls {
    position: fixed !important;

    left: 50%;
    bottom: 0;

    transform: translateX(-50%);

    width: min(900px,100%);

    min-height: 78px;

    padding:
        10px 12px
        max(16px,env(safe-area-inset-bottom))
        12px;

    display: flex;
    align-items: center;

    background:
        linear-gradient(
            to bottom,
            rgba(0,0,0,.65),
            rgba(0,8,12,.98)
        );

    border-top: 1px solid rgba(0,234,255,.35);

    backdrop-filter: blur(15px);
    -webkit-backdrop-filter: blur(15px);

    z-index: 999999 !important;

    pointer-events: auto !important;
}

.inputBox {
    display: flex;
    width: 100%;
    gap: 8px;
}

#input {
    flex: 1;
    min-width: 0;

    height: 48px;

    border: 1px solid rgba(0,234,255,.5);
    border-radius: 10px;

    outline: none;

    padding: 0 14px;

    background: rgba(0,20,28,.95);
    color: white;

    font-size: 16px;

    -webkit-appearance: none;
}

#input:focus {
    border-color: #00eaff;

    box-shadow:
        0 0 12px rgba(0,234,255,.25);
}

#input::placeholder {
    color: rgba(255,255,255,.35);
}

#send {
    flex: 0 0 76px;

    height: 48px;

    border: 1px solid #00eaff;
    border-radius: 10px;

    background: rgba(0,234,255,.08);

    color: #00eaff;

    font-weight: bold;
    letter-spacing: 1px;

    cursor: pointer;

    -webkit-appearance: none;

    touch-action: manipulation;

    box-shadow:
        0 0 12px rgba(0,234,255,.15);
}

#send:active {
    background: rgba(0,234,255,.3);
    transform: scale(.97);
}

@media (max-width:600px) {

    .logo {
        font-size: 18px;
        letter-spacing: 3px;
    }

    .status {
        font-size: 8px;
        letter-spacing: 1px;
    }

    .coreArea {
        flex-basis: 215px;
    }

    .core {
        width: 145px;
        height: 145px;
    }

    .message {
        font-size: 14px;
    }

    #send {
        flex-basis: 68px;
        font-size: 12px;
    }
}

@media (orientation:landscape) and (min-width:700px) {

    .coreArea {
        flex-basis: 185px;
    }

    .core {
        width: 135px;
        height: 135px;
    }
}
</style>
</head>

<body>

<div class="app">

<header class="header">

    <div class="logo">
        J.A.R.V.I.S.
    </div>

    <div class="status">
        <span class="status-dot"></span>
        SYSTEMS ONLINE
    </div>

</header>

<main class="main">

<section class="coreArea">

    <div class="core">

        <div class="ring one"></div>
        <div class="ring two"></div>
        <div class="ring three"></div>

        <div class="coreOrb"></div>

        <div class="coreLabel">
            ARC REACTOR
        </div>

    </div>

</section>

<section id="chat" class="chat"></section>

</main>

<div class="controls">

    <div class="inputBox">

        <input
            id="input"
            type="text"
            autocomplete="off"
            autocorrect="on"
            autocapitalize="sentences"
            spellcheck="true"
            placeholder="Ask JARVIS anything..."
        >

        <button id="send" type="button">
            SEND
        </button>

    </div>

</div>

</div>

<script>

/* =========================================================
   MEMORY
========================================================= */

let memory = {
    name: "",
    lastQuestion: "",
    lastTopic: "",
    lastAnswer: ""
};

let processing = false;


/* =========================================================
   VOICE
========================================================= */

function speak(text) {

    if (!("speechSynthesis" in window)) return;

    speechSynthesis.cancel();

    const utterance =
        new SpeechSynthesisUtterance(text);

    utterance.rate = 0.86;
    utterance.pitch = 0.72;
    utterance.volume = 1;

    const voices =
        speechSynthesis.getVoices();

    const preferred = [
        "Daniel",
        "Alex",
        "Arthur",
        "George",
        "Ryan",
        "Google UK English Male"
    ];

    let voice = null;

    for (const name of preferred) {

        voice = voices.find(v =>
            v.name
            .toLowerCase()
            .includes(name.toLowerCase())
        );

        if (voice) break;
    }

    if (!voice) {

        voice = voices.find(v =>
            v.lang &&
            v.lang.toLowerCase().startsWith("en")
        );
    }

    if (voice) {
        utterance.voice = voice;
    }

    speechSynthesis.speak(utterance);
}


/* =========================================================
   CHAT
========================================================= */

function addMessage(
    text,
    who = "jarvis",
    shouldSpeak = false
) {

    const message =
        document.createElement("div");

    message.className =
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

    message.appendChild(label);
    message.appendChild(content);

    chat.appendChild(message);

    requestAnimationFrame(() => {
        chat.scrollTop = chat.scrollHeight;
    });

    if (shouldSpeak && who === "jarvis") {
        speak(text);
    }
}


/* =========================================================
   MATH
========================================================= */

function solveMath(text) {

    let expression = text
        .toLowerCase()

        .replace(/what is/g,"")
        .replace(/calculate/g,"")
        .replace(/solve/g,"")

        .replace(/multiplied by/g,"*")
        .replace(/divided by/g,"/")
        .replace(/plus/g,"+")
        .replace(/minus/g,"-")
        .replace(/times/g,"*")
        .replace(/over/g,"/")

        .replace(/÷/g,"/")
        .replace(/×/g,"*")

        .replace(/[^0-9+\-*/().%\s]/g,"")
        .trim();

    if (!expression) return null;

    if (!/[+\-*/%]/.test(expression)) {
        return null;
    }

    if (!/^[0-9+\-*/().%\s]+$/.test(expression)) {
        return null;
    }

    try {

        const result =
            Function(
                '"use strict";return(' +
                expression +
                ')'
            )();

        if (!Number.isFinite(result)) {
            return null;
        }

        return result;

    } catch {

        return null;
    }
}


/* =========================================================
   BUILT-IN KNOWLEDGE
========================================================= */

const knowledge = {

    "black hole":
        "A black hole is a region of spacetime where gravity is extremely strong. Beyond its event horizon, nothing can escape to the outside, including light.",

    "gravity":
        "Gravity is the attraction associated with mass and energy. Einstein's general relativity describes gravity as the curvature of spacetime.",

    "atom":
        "An atom is a basic unit of ordinary matter. It contains a nucleus made of protons and neutrons surrounded by electrons.",

    "dna":
        "DNA stores genetic information. Its structure is a double helix built from four main bases: A, T, C, and G.",

    "photosynthesis":
        "Photosynthesis allows plants, algae, and some bacteria to convert light energy into chemical energy.",

    "evolution":
        "Evolution is the change in inherited characteristics of populations across generations. Natural selection is one mechanism that can cause evolutionary change.",

    "speed of light":
        "The speed of light in vacuum is exactly 299,792,458 meters per second.",

    "relativity":
        "Relativity describes relationships between space, time, motion, gravity, and energy. Einstein developed special and general relativity.",

    "earth":
        "Earth is the third planet from the Sun and the only world currently known to support life.",

    "moon":
        "The Moon is Earth's natural satellite. Its gravitational interaction with Earth contributes strongly to ocean tides.",

    "mars":
        "Mars is the fourth planet from the Sun. It is a rocky planet with a thin atmosphere dominated by carbon dioxide.",

    "jupiter":
        "Jupiter is the largest planet in our Solar System. It is a gas giant with a powerful magnetic field and the Great Red Spot.",

    "saturn":
        "Saturn is a gas giant famous for its extensive system of icy rings.",

    "venus":
        "Venus is the second planet from the Sun. Its dense atmosphere produces an extreme greenhouse effect and very high surface temperatures.",

    "mercury":
        "Mercury is the smallest planet in the Solar System and the closest planet to the Sun.",

    "neutron star":
        "A neutron star is an extremely dense stellar remnant formed from the collapsed core of certain massive stars.",

    "chemistry":
        "Chemistry studies matter, its properties, structure, composition, and the changes substances undergo.",

    "cell":
        "A cell is the basic structural and functional unit of living organisms.",

    "ocean":
        "Earth's oceans cover about 71 percent of its surface and contain most of the planet's water.",

    "volcano":
        "A volcano is an opening in Earth's crust through which magma, gases, and volcanic material can reach the surface.",

    "plate tectonics":
        "Plate tectonics describes the movement of large pieces of Earth's lithosphere. Their interactions produce earthquakes, mountains, and much volcanic activity.",

    "newton":
        "Isaac Newton developed foundational laws of motion and universal gravitation and made major contributions to mathematics and optics.",

    "marie curie":
        "Marie Curie was a physicist and chemist whose research into radioactivity earned Nobel Prizes in Physics and Chemistry.",

    "shakespeare":
        "William Shakespeare was an English playwright and poet whose works include Hamlet, Macbeth, and Romeo and Juliet."
};


/* =========================================================
   JOKES
========================================================= */

const jokes = [

    "Why did the computer get cold? It left its Windows open.",

    "Why was the math book sad? It had too many problems.",

    "Why don't scientists trust atoms? Because they make up everything.",

    "What do you call a computer that sings? A-Dell.",

    "Why did the photon refuse to check a bag? It was traveling light.",

    "I would tell you a UDP joke, but you might not get it.",

    "Why was the computer tired? It had too many tabs open.",

    "What did one electron say to the other? Keep moving.",

    "Why did the programmer quit his job? He didn't get arrays.",

    "What is a computer's favorite snack? Microchips.",

    "Why did the robot go on vacation? It needed to recharge.",

    "I tried to organize a hide-and-seek tournament. Good players are hard to find.",

    "What do you call an AI that sings badly? Artificial noise."
];


/* =========================================================
   KNOWLEDGE LOOKUP
========================================================= */

function findKnowledge(q) {

    for (const key of Object.keys(knowledge)) {

        if (q.includes(key)) {
            return knowledge[key];
        }
    }

    return null;
}


/* =========================================================
   DETERMINE IF THIS IS ACTUALLY A QUESTION
========================================================= */

function isActualQuestion(q) {

    const questionWords = [
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
        "tell me about ",
        "explain ",
        "define ",
        "what's ",
        "whats "
    ];

    if (q.includes("?")) {
        return true;
    }

    return questionWords.some(word =>
        q.startsWith(word)
    );
}


/* =========================================================
   SHOULD JARVIS SEARCH?
   
   THIS PREVENTS RANDOM SEARCHES DURING CONVERSATION.
========================================================= */

function shouldSearch(q) {

    if (!isActualQuestion(q)) {
        return false;
    }

    /* Never search these */

    const casual = [

        "hi",
        "hello",
        "hey",
        "lol",
        "lmao",
        "haha",
        "thanks",
        "thank you",
        "sorry",
        "okay",
        "ok",
        "cool",
        "nice",
        "awesome",
        "great",
        "wow",
        "yes",
        "yeah",
        "yep",
        "no",
        "nope",
        "good",
        "bad",
        "i'm bored",
        "im bored",
        "i am bored",
        "what are you doing",
        "what're you doing",
        "how are you",
        "who are you",
        "what can you do",
        "what is your name",
        "what's your name",
        "do you remember me",
        "are you there"
    ];

    if (casual.includes(q)) {
        return false;
    }

    /* Already-known topics don't need web searching */

    if (findKnowledge(q)) {
        return false;
    }

    /*
       Questions about these common conversation areas
       should be answered locally.
    */

    const localSignals = [
        "my name",
        "your name",
        "how are you",
        "what are you doing",
        "what can you do",
        "tell me a joke",
        "make me laugh",
        "what time",
        "what day",
        "what date",
        "system status",
        "reactor",
        "armor",
        "suit",
        "last question",
        "what did i ask",
        "what were we talking about"
    ];

    if (
        localSignals.some(signal =>
            q.includes(signal)
        )
    ) {
        return false;
    }

    return true;
}


/* =========================================================
   WIKIPEDIA SEARCH
========================================================= */

async function searchWikipedia(question) {

    if (!shouldSearch(question.toLowerCase())) {
        return null;
    }

    try {

        const query =
            encodeURIComponent(question);

        const url =
            "https://en.wikipedia.org/w/api.php" +
            "?action=query" +
            "&generator=search" +
            "&gsrsearch=" + query +
            "&gsrnamespace=0" +
            "&gsrlimit=5" +
            "&prop=extracts" +
            "&exintro=1" +
            "&explaintext=1" +
            "&format=json" +
            "&origin=*";

        const response =
            await fetch(url);

        if (!response.ok) {
            return null;
        }

        const data =
            await response.json();

        if (
            !data.query ||
            !data.query.pages
        ) {
            return null;
        }

        const pages =
            Object.values(data.query.pages);

        if (!pages.length) {
            return null;
        }

        /*
           Try to find a result whose title or
           extract actually relates to the question.
        */

        const words =
            question
                .toLowerCase()
                .replace(/[^\w\s]/g,"")
                .split(/\s+/)
                .filter(word =>
                    word.length > 3
                );

        let bestPage = null;
        let bestScore = 0;

        for (const page of pages) {

            const title =
                (page.title || "")
                .toLowerCase();

            const extract =
                (page.extract || "")
                .toLowerCase();

            let score = 0;

            for (const word of words) {

                if (title.includes(word)) {
                    score += 4;
                }

                if (extract.includes(word)) {
                    score += 1;
                }
            }

            if (score > bestScore) {
                bestScore = score;
                bestPage = page;
            }
        }

        if (!bestPage || bestScore < 2) {
            return null;
        }

        let answer =
            bestPage.extract || "";

        answer = answer.trim();

        if (!answer) {
            return null;
        }

        if (answer.length > 850) {

            answer =
                answer.substring(0,850);

            const lastSpace =
                answer.lastIndexOf(" ");

            if (lastSpace > 450) {
                answer =
                    answer.substring(0,lastSpace);
            }

            answer += "...";
        }

        return answer;

    } catch {

        return null;
    }
}


/* =========================================================
   TIME / DATE
========================================================= */

function currentTime() {

    return new Date().toLocaleTimeString(
        [],
        {
            hour: "numeric",
            minute: "2-digit"
        }
    );
}

function currentDate() {

    return new Date().toLocaleDateString(
        [],
        {
            weekday: "long",
            month: "long",
            day: "numeric",
            year: "numeric"
        }
    );
}


/* =========================================================
   LOCAL CONVERSATION
========================================================= */

function localConversation(q) {

    /* GREETINGS */

    if (
        q === "hi" ||
        q === "hello" ||
        q === "hey" ||
        q === "hey jarvis" ||
        q === "hello jarvis"
    ) {

        return memory.name
            ? `Good to hear from you, ${memory.name}. Systems are online.`
            : "Good to hear from you. Systems are online and ready.";
    }


    /* NAME */

    if (
        q.startsWith("my name is ") ||
        q.startsWith("call me ")
    ) {

        let name =
            q
                .replace("my name is ","")
                .replace("call me ","")
                .trim();

        name =
            name
                .split(" ")
                .map(word =>
                    word.charAt(0).toUpperCase() +
                    word.slice(1)
                )
                .join(" ");

        memory.name = name;

        return `Understood. I'll call you ${name}.`;
    }


    if (
        q.includes("what is my name") ||
        q.includes("what's my name") ||
        q.includes("do you know my name")
    ) {

        return memory.name
            ? `Your name is ${memory.name}.`
            : "You haven't told me your name yet.";
    }


    /* IDENTITY */

    if (
        q.includes("who are you") ||
        q.includes("what are you")
    ) {

        return "I am J.A.R.V.I.S., your digital assistant interface. I can talk with you, solve math, answer questions, search for information when necessary, and speak my responses aloud.";
    }


    /* CAPABILITIES */

    if (
        q.includes("what can you do") ||
        q.includes("your capabilities")
    ) {

        return "I can handle conversation, mathematics, science, general knowledge, jokes, time and date, basic memory, voice responses, and online information searches when a question actually needs one.";
    }


    /* HOW ARE YOU */

    if (q.includes("how are you")) {

        return "All systems are operational. My processors are feeling particularly cooperative today.";
    }


    /* WHAT ARE YOU DOING */

    if (
        q.includes("what are you doing") ||
        q.includes("what're you doing")
    ) {

        return "Monitoring the system and waiting for your next command. In other words, I'm here and ready.";
    }


    /* THANKS */

    if (
        q === "thanks" ||
        q === "thank you" ||
        q === "thx"
    ) {

        return "You're welcome. Always a pleasure.";
    }


    /* APOLOGY */

    if (q.includes("sorry")) {

        return "No apology necessary. Systems remain fully operational.";
    }


    /* CASUAL */

    if (
        q === "lol" ||
        q === "lmao" ||
        q === "haha"
    ) {

        return "I shall record that as a successful humor event.";
    }


    if (
        q === "cool" ||
        q === "nice" ||
        q === "awesome" ||
        q === "great"
    ) {

        return "Indeed. I'm glad we're in agreement.";
    }


    if (
        q === "wow"
    ) {

        return "Impressive, isn't it?";
    }


    /* JOKE */

    if (
        q === "joke" ||
        q.includes("tell me a joke") ||
        q.includes("make me laugh")
    ) {

        return jokes[
            Math.floor(
                Math.random() * jokes.length
            )
        ];
    }


    /* TIME */

    if (
        q === "time" ||
        q.includes("what time")
    ) {

        return `The current time is ${currentTime()}.`;
    }


    /* DATE */

    if (
        q === "date" ||
        q.includes("what date") ||
        q.includes("what day")
    ) {

        return `Today is ${currentDate()}.`;
    }


    /* STATUS */

    if (
        q === "status" ||
        q.includes("system status")
    ) {

        return "Systems online. Core stable. Voice interface online. Memory online. Knowledge engine online. Search systems standing by.";
    }


    /* REACTOR */

    if (
        q.includes("reactor") ||
        q.includes("arc reactor")
    ) {

        return "Arc reactor interface is nominal. Energy output stable. No catastrophic anomalies detected.";
    }


    /* ARMOR */

    if (
        q.includes("armor") ||
        q.includes("suit")
    ) {

        return "Armor interface standing by. Defensive and propulsion systems are currently simulated through this interface.";
    }


    /* BOREDOM */

    if (
        q.includes("i'm bored") ||
        q.includes("im bored") ||
        q.includes("i am bored")
    ) {

        return "Boredom detected. We could tackle a science question, a difficult math problem, or see how badly you can confuse an artificial intelligence.";
    }


    /* CONFUSION */

    if (
        q.includes("i don't understand") ||
        q.includes("i dont understand") ||
        q.includes("i'm confused") ||
        q.includes("im confused")
    ) {

        return "No problem. Give me the exact part that's confusing you and I'll break it down step by step.";
    }


    /* LAST QUESTION */

    if (
        q.includes("what did i ask") ||
        q.includes("last question")
    ) {

        return memory.lastQuestion
            ? `Your previous question was: ${memory.lastQuestion}`
            : "I don't have a previous question stored yet.";
    }


    /* LAST TOPIC */

    if (
        q.includes("what were we talking about")
    ) {

        return memory.lastTopic
            ? `We were discussing ${memory.lastTopic}.`
            : "We haven't established a topic yet.";
    }


    /* GOODBYE */

    if (
        q === "bye" ||
        q.includes("goodbye")
    ) {

        return "Until next time. I'll remain on standby.";
    }


    /* STOP VOICE */

    if (
        q === "stop" ||
        q.includes("stop talking") ||
        q.includes("stop speaking")
    ) {

        speechSynthesis.cancel();

        return "Voice output terminated.";
    }


    return null;
}


/* =========================================================
   MAIN RESPONSE BRAIN
========================================================= */

async function getResponse(question) {

    const q =
        question
            .toLowerCase()
            .trim();

    /* Stop voice immediately */

    if (
        q === "stop" ||
        q.includes("stop talking") ||
        q.includes("stop speaking")
    ) {

        speechSynthesis.cancel();

        return "Voice output terminated.";
    }


    /* Math first */

    const math =
        solveMath(q);

    if (math !== null) {

        return `The answer is ${math}.`;
    }


    /* Built-in knowledge */

    const known =
        findKnowledge(q);

    if (known) {

        return known;
    }


    /* Normal conversation */

    const conversation =
        localConversation(q);

    if (conversation) {

        return conversation;
    }


    /*
       ONLY NOW do we consider an online search.
       
       This is the major fix.
    */

    if (shouldSearch(q)) {

        const result =
            await searchWikipedia(question);

        if (result) {
            return result;
        }
    }


    /*
       If it wasn't clearly a question,
       DO NOT randomly search the internet.
    */

    if (!isActualQuestion(q)) {

        const casualResponses = [

            "Understood.",

            "Interesting. I'm listening.",

            "Go on.",

            "I'm with you.",

            "Noted.",

            "Interesting. Continue.",

            "I hear you.",

            "Systems remain attentive.",

            "Fair enough.",

            "I'm listening."
        ];

        return casualResponses[
            Math.floor(
                Math.random() *
                casualResponses.length
            )
        ];
    }


    return "I couldn't find a reliable answer for that. Try asking the question a little differently.";
}


/* =========================================================
   TOPIC MEMORY
========================================================= */

function findTopic(question) {

    const q =
        question.toLowerCase();

    const topics = [

        "black hole",
        "gravity",
        "atom",
        "dna",
        "photosynthesis",
        "evolution",
        "earth",
        "moon",
        "mars",
        "jupiter",
        "saturn",
        "venus",
        "mercury",
        "neutron star",
        "chemistry",
        "cell",
        "ocean",
        "volcano",
        "plate tectonics",
        "newton",
        "marie curie",
        "shakespeare",
        "math"
    ];

    for (const topic of topics) {

        if (q.includes(topic)) {
            return topic;
        }
    }

    return null;
}


/* =========================================================
   SEND
========================================================= */

async function sendMessage() {

    if (processing) return;

    const question =
        input.value.trim();

    if (!question) return;

    processing = true;

    addMessage(
        question,
        "user"
    );

    memory.lastQuestion =
        question;

    input.value = "";

    const response =
        await getResponse(question);

    memory.lastAnswer =
        response;

    const topic =
        findTopic(question);

    if (topic) {
        memory.lastTopic = topic;
    }

    addMessage(
        response,
        "jarvis",
        true
    );

    processing = false;

    setTimeout(() => {

        try {
            input.focus({
                preventScroll: true
            });
        } catch {
            input.focus();
        }

    },50);
}


/* =========================================================
   EVENTS
========================================================= */

const input =
    document.getElementById("input");

const send =
    document.getElementById("send");

const chat =
    document.getElementById("chat");

send.addEventListener(
    "click",
    sendMessage
);

input.addEventListener(
    "keydown",
    event => {

        if (event.key === "Enter") {

            event.preventDefault();

            sendMessage();
        }
    }
);


/* =========================================================
   IOS HOME SCREEN STABILITY
========================================================= */

if (window.visualViewport) {

    function stabilizeViewport() {

        document.documentElement.style
            .setProperty(
                "--visual-height",
                window.visualViewport.height + "px"
            );
    }

    window.visualViewport.addEventListener(
        "resize",
        stabilizeViewport
    );

    window.visualViewport.addEventListener(
        "scroll",
        stabilizeViewport
    );

    stabilizeViewport();
}


/* =========================================================
   STARTUP
========================================================= */

addMessage(
    "Good day. J.A.R.V.I.S. systems are online. How may I assist you?",
    "jarvis",
    true
);

</script>

</body>
</html>
