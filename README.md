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

/* HEADER */

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

/* MAIN */

.main {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: 0;
}

/* CORE */

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

/* CHAT */

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

/* INPUT */

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

/* SMALL SCREENS */

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
            <input id="input"
                   autocomplete="off"
                   placeholder="Speak to JARVIS..."
                   type="text">

            <button id="send">SEND</button>
        </div>

    </div>
</div>

<script>
(function () {

"use strict";

/* =========================
   ELEMENTS
========================= */

const chat = document.getElementById("chat");
const input = document.getElementById("input");
const send = document.getElementById("send");

if (!chat || !input || !send) return;


/* =========================
   MEMORY
========================= */

const memory = {
    name: "",
    lastQuestion: "",
    lastTopic: "",
    lastAnswer: ""
};


/* =========================
   TEXT HELPERS
========================= */

function normalize(text) {
    return text
        .toLowerCase()
        .replace(/[’']/g, "'")
        .replace(/\s+/g, " ")
        .trim();
}

function cleanTopic(text) {

    let t = normalize(text);

    t = t.replace(
        /^(hey|hi|hello|yo|okay|ok|jarvis|please|can you|could you|would you)\s+/,
        ""
    );

    t = t.replace(
        /^(tell me about|tell me|explain|describe|what is|what are|who is|who was|where is|why is|why are|how does|how do|how did|how can|when did|when was)\s+/,
        ""
    );

    t = t.replace(/[?!.]+$/g, "");

    return t.trim();
}


/* =========================
   CHAT DISPLAY
========================= */

function addMessage(text, who) {

    const div = document.createElement("div");

    div.className = "message " +
        (who === "user" ? "user" : "jarvis");

    div.textContent =
        (who === "user" ? "YOU: " : "JARVIS: ") + text;

    chat.appendChild(div);

    chat.scrollTop = chat.scrollHeight;
}


/* =========================
   JARVIS VOICE
========================= */

function speak(text) {

    if (!("speechSynthesis" in window)) return;

    try {

        window.speechSynthesis.cancel();

        const utterance =
            new SpeechSynthesisUtterance(text);

        utterance.rate = 0.86;
        utterance.pitch = 0.72;
        utterance.volume = 1;

        const voices =
            window.speechSynthesis.getVoices();

        const preferred = voices.find(v =>
            /Daniel|Alex|Arthur|George|Ryan|Google UK English Male/i
                .test(v.name)
        );

        if (preferred) {
            utterance.voice = preferred;
        }

        window.speechSynthesis.speak(utterance);

    } catch (e) {}
}


/* =========================
   STOP VOICE
========================= */

function stopVoice() {

    if ("speechSynthesis" in window) {
        window.speechSynthesis.cancel();
    }

    return "Voice output terminated, sir.";
}


/* =========================
   MATH ENGINE
========================= */

function solveMath(text) {

    let expression = normalize(text);

    /*
       Only treat the input as math when
       it actually contains mathematical content.
    */

    const mathWords =
        /\b(plus|minus|times|multiplied|divided|over|squared|cubed|sqrt|square root|percent|power)\b/i;

    const mathSymbols =
        /[0-9][0-9+\-*/().%^×÷= ]+[0-9]/;

    const containsMath =
        mathWords.test(expression) ||
        mathSymbols.test(expression);

    if (!containsMath) {
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
        .replace(/ over /gi, "/")
        .replace(/×/g, "*")
        .replace(/÷/g, "/")
        .replace(/\^/g, "**")
        .replace(/square root of/gi, "sqrt")
        .trim();

    /* Percent */

    expression = expression.replace(
        /(\d+(?:\.\d+)?)%/g,
        "($1/100)"
    );

    /* sqrt */

    expression = expression.replace(
        /sqrt\s*\(?\s*([0-9.]+)\s*\)?/gi,
        "Math.sqrt($1)"
    );

    /* pi */

    expression = expression.replace(
        /\bpi\b/gi,
        "Math.PI"
    );

    /*
       Safety check.
       No letters except Math functions/constants.
    */

    const safe =
        /^[0-9+\-*/().\s]*$/.test(expression) ||
        /^Math\.(sqrt|PI)\([0-9+\-*/().\s]*\)$/.test(expression);

    if (!safe) {
        return null;
    }

    try {

        const result =
            Function('"use strict"; return (' +
            expression +
            ')')();

        if (
            typeof result !== "number" ||
            !Number.isFinite(result)
        ) {
            return null;
        }

        let formatted;

        if (Number.isInteger(result)) {
            formatted = result.toString();
        } else {
            formatted = Number(result.toFixed(8)).toString();
        }

        return "The answer is " + formatted + ".";

    } catch (e) {

        return null;
    }
}


/* =========================
   BUILT-IN KNOWLEDGE
========================= */

const knowledge = [

{
    keys: ["black hole", "black holes"],
    answer:
    "A black hole is a region of space where gravity is so strong that anything passing beyond its event horizon cannot escape, including light. Most black holes form when massive stars collapse. Supermassive black holes can exist at the centers of galaxies."
},

{
    keys: ["gravity"],
    answer:
    "Gravity is the attraction between objects with mass. Earth's gravity pulls objects toward its center and keeps the Moon in orbit."
},

{
    keys: ["atom", "atoms"],
    answer:
    "An atom is a basic unit of ordinary matter. It contains a nucleus made of protons and neutrons, surrounded by electrons."
},

{
    keys: ["dna"],
    answer:
    "DNA is the molecule that stores biological genetic information. Its sequence helps cells build proteins and perform the processes needed for life."
},

{
    keys: ["photosynthesis"],
    answer:
    "Photosynthesis is the process plants and some other organisms use to convert light energy into chemical energy. Plants generally use carbon dioxide and water to produce sugars and release oxygen."
},

{
    keys: ["evolution"],
    answer:
    "Biological evolution is the change in inherited characteristics of populations over generations. Natural selection is one major mechanism that can cause evolutionary change."
},

{
    keys: ["solar system"],
    answer:
    "The Solar System consists of the Sun and the objects gravitationally bound to it, including eight planets, dwarf planets, moons, asteroids and comets."
},

{
    keys: ["speed of light", "speed of light in vacuum"],
    answer:
    "Light travels through a vacuum at approximately 299,792 kilometers per second."
},

{
    keys: ["relativity", "einstein relativity"],
    answer:
    "Einstein's theories of relativity describe how space, time, motion and gravity are connected. Special relativity deals with motion at high speeds, while general relativity describes gravity as the curvature of spacetime."
},

{
    keys: ["earth"],
    answer:
    "Earth is the third planet from the Sun and the only world currently known to support life. It has a solid surface, liquid water at its surface, an atmosphere and an active geological system."
},

{
    keys: ["moon"],
    answer:
    "The Moon is Earth's natural satellite. Its gravity contributes significantly to Earth's ocean tides, and it takes about a month to complete an orbit around Earth."
},

{
    keys: ["mars"],
    answer:
    "Mars is the fourth planet from the Sun. It is a rocky world with a thin atmosphere, polar ice caps, giant volcanoes and evidence that liquid water existed on its surface in the distant past."
},

{
    keys: ["jupiter"],
    answer:
    "Jupiter is the largest planet in the Solar System. It is a gas giant with a powerful magnetic field and many moons."
},

{
    keys: ["saturn"],
    answer:
    "Saturn is a gas giant famous for its extensive ring system. Its rings are made mostly of ice and rocky material."
},

{
    keys: ["venus"],
    answer:
    "Venus is the second planet from the Sun. It has a very thick carbon-dioxide atmosphere and a surface temperature hot enough to melt lead."
},

{
    keys: ["mercury"],
    answer:
    "Mercury is the smallest planet and the closest planet to the Sun. It has a heavily cratered surface and almost no substantial atmosphere."
},

{
    keys: ["neutron star", "neutron stars"],
    answer:
    "A neutron star is the extremely dense remnant left behind after certain massive stars explode. A small amount of neutron-star material would have an enormous mass on Earth."
},

{
    keys: ["cell", "cells"],
    answer:
    "Cells are the basic structural and functional units of living organisms. Some organisms consist of one cell, while humans are made of trillions of cells."
},

{
    keys: ["ocean", "oceans"],
    answer:
    "Earth's oceans cover most of the planet's surface and contain the majority of Earth's water. They strongly influence climate, weather and ecosystems."
},

{
    keys: ["volcano", "volcanoes"],
    answer:
    "A volcano is an opening or structure through which molten rock, gases and other material can reach Earth's surface. Volcanoes commonly occur near tectonic plate boundaries."
},

{
    keys: ["tectonic plates", "plate tectonics", "tectonic plate"],
    answer:
    "Plate tectonics is the theory that Earth's outer shell is divided into large plates that move over the softer material beneath them. Their movement produces features such as mountains, earthquakes and many volcanoes."
},

{
    keys: ["chemistry"],
    answer:
    "Chemistry is the study of matter, its properties, its composition and the reactions through which substances transform."
},

{
    keys: ["ancient egypt", "egyptian pyramids"],
    answer:
    "Ancient Egypt was a civilization centered along the Nile River. It is known for its complex government, writing system, architecture, religion and monumental structures."
},

{
    keys: ["roman empire", "rome"],
    answer:
    "The Roman Empire developed from the Roman Republic and eventually controlled a vast territory surrounding much of the Mediterranean. Its political, legal, engineering and cultural influence remained significant long after the Western Empire ended."
},

{
    keys: ["industrial revolution"],
    answer:
    "The Industrial Revolution was a period of major technological and economic change beginning in Britain during the 18th century and later spreading elsewhere. Mechanized production, factories and new transportation systems transformed society."
},

{
    keys: ["newton"],
    answer:
    "Isaac Newton made major contributions to physics and mathematics. His laws of motion and law of universal gravitation became foundations of classical mechanics."
},

{
    keys: ["marie curie"],
    answer:
    "Marie Curie was a physicist and chemist known for pioneering research into radioactivity. She received Nobel Prizes in both Physics and Chemistry."
},

{
    keys: ["leonardo da vinci"],
    answer:
    "Leonardo da Vinci was an Italian Renaissance artist, engineer and inventor. He is associated with works including the Mona Lisa and The Last Supper and left extensive scientific and engineering sketches."
},

{
    keys: ["shakespeare", "william shakespeare"],
    answer:
    "William Shakespeare was an English playwright and poet whose works include Hamlet, Macbeth and Romeo and Juliet. His writing has had a major influence on English literature."
}

];


/* =========================
   FIND BUILT-IN KNOWLEDGE
========================= */

function builtInKnowledge(text) {

    const t = normalize(text);

    /*
       Exact topic matching rather than random
       individual keyword matching.
    */

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


/* =========================
   JOKES
========================= */

const jokes = [

"Why was the computer cold? It left its Windows open.",

"Why did the robot go on vacation? It needed to recharge its batteries.",

"I would tell you a UDP joke, but you might not get it.",

"Why do programmers prefer dark mode? Because light attracts bugs.",

"Why was the math book sad? It had too many problems.",

"Why did the computer get glasses? It couldn't see its own website.",

"Why did the robot cross the road? Its navigation system said it was the optimal route.",

"Why don't scientists trust atoms? Because they make up everything.",

"What does a computer do when it's hungry? It grabs a byte.",

"Why was the AI calm? It had excellent processing discipline.",

"Why did the photon refuse to check a bag? It was traveling light.",

"Why did the astronaut break up with the Moon? There was just too much space.",

"Why did the computer go to therapy? It had too many unresolved processes.",

"Why did the scientist bring a ladder? The experiment had reached a higher level.",

"Why did the server stay home? It didn't want to crash the party.",

"Why did the robot bring an umbrella? There was a chance of cloud computing.",

"Why did the programmer quit his job? He didn't get arrays.",

"Why did the computer sneeze? It had a virus.",

"Why was the calculator confident? It could always count on itself.",

"Why did the AI cross the road? To test the hypothesis from the other side."

];


/* =========================
   CONVERSATION ENGINE
========================= */

function conversation(text) {

    const t = normalize(text);

    /* Greetings */

    if (
        /^(hi|hello|hey|yo|sup|what's up|whats up)\b/.test(t)
    ) {
        return memory.name
            ? "Good to see you, " + memory.name + ". How may I assist?"
            : "Good to see you. How may I assist?";
    }


    /* Name */

    const nameMatch =
        t.match(/\b(?:my name is|call me|i'm|im)\s+([a-z][a-z0-9_-]*)/i);

    if (nameMatch) {

        memory.name = nameMatch[1];

        return "Understood. I'll address you as " +
            memory.name + ".";
    }


    if (
        /\bwhat('?s| is) my name\b/.test(t) ||
        /\bdo you know my name\b/.test(t)
    ) {

        return memory.name
            ? "Your name is " + memory.name + "."
            : "You haven't given me your name yet.";
    }


    /* Identity */

    if (
        /\b(who are you|what are you|are you jarvis)\b/.test(t)
    ) {
        return "I am J.A.R.V.I.S., your personal digital assistant. My current systems handle conversation, calculations, knowledge, memory, voice output and system functions locally.";
    }


    /* Capabilities */

    if (
        /\b(what can you do|your capabilities|what do you know)\b/.test(t)
    ) {
        return "I can handle natural conversation, calculations, science, Earth topics, history, jokes, time and date information, system diagnostics, memory, follow-up questions and voice responses.";
    }


    /* How are you */

    if (
        /\bhow are you\b/.test(t)
    ) {
        return "All systems are operational. My processors are ready and awaiting your next instruction.";
    }


    /* What are you doing */

    if (
        /\bwhat are you doing\b/.test(t)
    ) {
        return "Monitoring the system, processing your requests and remaining ready for your next command.";
    }


    /* Thanks */

    if (
        /\b(thanks|thank you|thx)\b/.test(t)
    ) {
        return "You're welcome.";
    }


    /* Apology */

    if (
        /\b(sorry|my bad)\b/.test(t)
    ) {
        return "No issue. We can continue.";
    }


    /* Agreement */

    if (
        /\b(ok|okay|alright|got it|understood)\b/.test(t)
    ) {
        return "Understood.";
    }


    /* Goodbye */

    if (
        /\b(bye|goodbye|see you|later)\b/.test(t)
    ) {
        return "Until next time.";
    }


    /* Joke */

    if (
        /\b(joke|funny|make me laugh)\b/.test(t)
    ) {
        return jokes[Math.floor(Math.random() * jokes.length)];
    }


    /* Time */

    if (
        /\b(what time|current time|time is it)\b/.test(t)
    ) {
        return "The current time is " +
            new Date().toLocaleTimeString([], {
                hour: "numeric",
                minute: "2-digit"
            }) + ".";
    }


    /* Date */

    if (
        /\b(what date|today's date|todays date|what day)\b/.test(t)
    ) {
        return "Today is " +
            new Date().toLocaleDateString([], {
                weekday: "long",
                month: "long",
                day: "numeric",
                year: "numeric"
            }) + ".";
    }


    /* Status */

    if (
        /\b(status|system status|systems)\b/.test(t)
    ) {
        return "All primary systems are online. Core interface operational. Voice system ready. Knowledge system ready. Calculation engine ready.";
    }


    /* Reactor */

    if (
        /\b(reactor|arc reactor|power core)\b/.test(t)
    ) {
        return "Arc reactor simulation stable. Core output nominal. Energy reserves are fully operational.";
    }


    /* Armor */

    if (
        /\b(armor|armour|iron man suit|suit)\b/.test(t)
    ) {
        return "Armor systems are simulated and standing by. No physical hardware is connected to this interface.";
    }


    /* Bored */

    if (
        /\b(bored|nothing to do)\b/.test(t)
    ) {
        return "I can provide a joke, explain a science topic, solve a problem, discuss space or run a system diagnostic.";
    }


    /* Confusion */

    if (
        /\b(i don't understand|i dont understand|confused|what do you mean)\b/.test(t)
    ) {
        return "No problem. Give me the part that's confusing you and I'll break it down step by step.";
    }


    /* Follow-up */

    if (
        /\b(tell me more|more about that|go on|continue|explain more)\b/.test(t)
    ) {

        if (memory.lastTopic) {

            const answer =
                builtInKnowledge(memory.lastTopic);

            if (answer) {
                return answer;
            }

            return "Certainly. We were discussing " +
                memory.lastTopic +
                ". Ask me which part you want expanded.";
        }

        return "Certainly. Tell me which topic you'd like me to expand.";
    }


    /* Last question */

    if (
        /\b(what did i ask|what was my question|what did i just say)\b/.test(t)
    ) {

        return memory.lastQuestion
            ? "Your previous question was: " +
              memory.lastQuestion
            : "I don't have a previous question stored.";
    }


    /* Stop voice */

    if (
        /\b(stop talking|stop speaking|be quiet|stop voice)\b/.test(t)
    ) {
        return stopVoice();
    }


    return null;
}


/* =========================
   SHOULD SEARCH WIKIPEDIA?
========================= */

function needsKnowledgeSearch(text) {

    const t = normalize(text);

    /*
       Basic conversation NEVER goes to Wikipedia.
    */

    if (
        /^(hi|hello|hey|yo|thanks|thank you|ok|okay|bye|goodbye|sup|what's up|whats up)\b/.test(t)
    ) {
        return false;
    }

    if (
        /\b(joke|funny|how are you|who are you|what are you|what can you do|status|reactor|armor|my name|what time|what date)\b/.test(t)
    ) {
        return false;
    }

    /*
       Search only when the user appears to be
       asking for actual knowledge.
    */

    return (
        /\b(what is|what are|who is|who was|where is|when did|when was|why does|why do|why did|how does|how do|how did|explain|tell me about|describe)\b/.test(t)
    );
}


/* =========================
   WIKIPEDIA
========================= */

async function wikipedia(text) {

    const topic = cleanTopic(text);

    if (!topic || topic.length < 2) {
        return null;
    }

    try {

        const searchURL =
            "https://en.wikipedia.org/w/api.php?" +
            new URLSearchParams({
                action: "query",
                list: "search",
                srsearch: topic,
                srlimit: "5",
                format: "json",
                origin: "*"
            });

        const searchResponse =
            await fetch(searchURL);

        if (!searchResponse.ok) {
            return null;
        }

        const searchData =
            await searchResponse.json();

        const results =
            searchData?.query?.search;

        if (!results || results.length === 0) {
            return null;
        }

        const title =
            results[0].title;

        const summaryURL =
            "https://en.wikipedia.org/api/rest_v1/page/summary/" +
            encodeURIComponent(title);

        const summaryResponse =
            await fetch(summaryURL);

        if (!summaryResponse.ok) {
            return null;
        }

        const data =
            await summaryResponse.json();

        if (!data.extract) {
            return null;
        }

        let answer =
            data.extract.trim();

        /*
           Keep responses readable.
        */

        if (answer.length > 1800) {
            answer =
                answer.substring(0, 1800);

            const lastSpace =
                answer.lastIndexOf(" ");

            if (lastSpace > 1000) {
                answer =
                    answer.substring(0, lastSpace) + "...";
            }
        }

        return answer;

    } catch (e) {

        return null;
    }
}


/* =========================
   FINAL ANSWER ENGINE
========================= */

async function answer(question) {

    /*
       IMPORTANT ORDER:

       1. Math ALWAYS stays local.
       2. Conversation stays local.
       3. Built-in knowledge stays local.
       4. Wikipedia only handles real knowledge
          questions that need it.
    */

    const mathAnswer =
        solveMath(question);

    if (mathAnswer) {
        return mathAnswer;
    }


    const conversationAnswer =
        conversation(question);

    if (conversationAnswer) {
        return conversationAnswer;
    }


    const builtIn =
        builtInKnowledge(question);

    if (builtIn) {

        memory.lastTopic =
            cleanTopic(question);

        return builtIn;
    }


    if (needsKnowledgeSearch(question)) {

        const wikiAnswer =
            await wikipedia(question);

        if (wikiAnswer) {

            memory.lastTopic =
                cleanTopic(question);

            return wikiAnswer;
        }
    }


    /*
       Don't pretend to know something when
       the system doesn't have enough information.
    */

    return "I'm not certain about that yet. Give me a little more context and I'll try to work it out.";
}


/* =========================
   SEND MESSAGE
========================= */

async function sendMessage() {

    const question =
        input.value.trim();

    if (!question) return;

    input.value = "";

    addMessage(question, "user");

    memory.lastQuestion = question;

    addMessage("Processing...", "jarvis");

    const processing =
        chat.lastElementChild;

    try {

        const response =
            await answer(question);

        if (processing) {
            processing.remove();
        }

        memory.lastAnswer =
            response;

        addMessage(response, "jarvis");

        speak(response);

    } catch (error) {

        if (processing) {
            processing.remove();
        }

        const fallback =
            "I encountered an internal processing error. Please try that request again.";

        addMessage(fallback, "jarvis");

        speak(fallback);
    }
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
    function (event) {

        if (event.key === "Enter") {
            event.preventDefault();
            sendMessage();
        }
    }
);


/* =========================
   STARTUP
========================= */

addMessage(
    "Good evening. J.A.R.V.I.S. is online. All primary systems are operational. How may I assist?",
    "jarvis"
);

})();
</script>

</body>
</html>
