<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="theme-color" content="#02060a">
<title>J.A.R.V.I.S.</title>

<style>
* {
    box-sizing: border-box;
}

html, body {
    margin: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    background: #010409;
    color: #dffaff;
    font-family: Arial, Helvetica, sans-serif;
}

body {
    display: flex;
    justify-content: center;
}

.app {
    width: 100%;
    max-width: 900px;
    height: 100%;
    display: flex;
    flex-direction: column;
    padding: 18px 16px 12px;
    background:
        radial-gradient(
            circle at 50% 28%,
            rgba(0, 190, 255, 0.14),
            transparent 35%
        ),
        linear-gradient(
            180deg,
            #020b13,
            #010409 70%
        );
    border: 1px solid rgba(0, 220, 255, 0.18);
    box-shadow:
        inset 0 0 80px rgba(0, 180, 255, 0.08);
}

.top {
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #63eaff;
    font-size: 12px;
    letter-spacing: 2px;
}

.status {
    color: #65ff9a;
}

.coreWrap {
    height: 245px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
}

.core {
    width: 190px;
    height: 190px;
    position: relative;
    border-radius: 50%;
    border: 2px solid #39ddff;
    box-shadow:
        0 0 18px rgba(0, 220, 255, 0.55),
        inset 0 0 25px rgba(0, 200, 255, 0.2);
}

.core::before {
    content: "";
    position: absolute;
    inset: 17px;
    border-radius: 50%;
    border: 1px solid rgba(72, 235, 255, 0.7);
}

.core::after {
    content: "";
    position: absolute;
    inset: 39px;
    border-radius: 50%;
    border: 2px solid rgba(0, 160, 255, 0.75);
}

.ring {
    position: absolute;
    inset: -13px;
    border: 1px dashed rgba(64, 225, 255, 0.55);
    border-radius: 50%;
}

.orb {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 55px;
    height: 55px;
    transform: translate(-50%, -50%);
    border-radius: 50%;
    background: #aef8ff;
    box-shadow:
        0 0 14px #5cecff,
        0 0 40px rgba(0, 210, 255, 0.8);
}

.title {
    text-align: center;
    margin-bottom: 8px;
}

.title h1 {
    margin: 0;
    color: #7ceeff;
    font-size: 25px;
    letter-spacing: 5px;
    text-shadow: 0 0 12px rgba(0, 220, 255, 0.75);
}

.title p {
    margin: 5px 0 0;
    color: #68ff9b;
    font-size: 11px;
    letter-spacing: 3px;
}

.chat {
    flex: 1;
    min-height: 0;
    overflow-y: auto;
    padding: 10px 2px 12px;
}

.msg {
    margin: 8px 0;
    padding: 11px 13px;
    border: 1px solid rgba(75, 220, 255, 0.2);
    border-radius: 12px;
    background: rgba(0, 20, 31, 0.58);
    line-height: 1.45;
    white-space: pre-wrap;
    word-break: break-word;
}

.msg.jarvis {
    border-left: 3px solid #43e7ff;
}

.msg.user {
    border-right: 3px solid #66ff9b;
    text-align: right;
    color: #bfffd3;
}

.label {
    font-size: 9px;
    letter-spacing: 2px;
    opacity: 0.65;
    margin-bottom: 4px;
}

.inputRow {
    display: flex;
    gap: 8px;
    padding-top: 8px;
}

.input {
    flex: 1;
    min-width: 0;
    border: 1px solid rgba(66, 225, 255, 0.5);
    border-radius: 12px;
    background: #03121c;
    color: white;
    padding: 13px;
    font-size: 16px;
    outline: none;
}

.input:focus {
    box-shadow: 0 0 12px rgba(0, 210, 255, 0.18);
}

button {
    border: 1px solid #36ddff;
    background: #06202b;
    color: #bff8ff;
    border-radius: 12px;
    padding: 0 15px;
    font-weight: bold;
    letter-spacing: 1px;
}

button:active {
    transform: scale(0.98);
}

.hint {
    text-align: center;
    color: #64848e;
    font-size: 9px;
    letter-spacing: 1px;
    padding: 8px 0 0;
}

@media (max-height: 650px) {
    .coreWrap {
        height: 175px;
    }

    .core {
        width: 130px;
        height: 130px;
    }

    .title h1 {
        font-size: 20px;
    }
}
</style>
</head>

<body>

<div class="app">

    <div class="top">
        <span>JARVIS // ONLINE</span>
        <span class="status">● READY</span>
    </div>

    <div class="coreWrap">
        <div class="core">
            <div class="ring"></div>
            <div class="orb"></div>
        </div>
    </div>

    <div class="title">
        <h1>J.A.R.V.I.S.</h1>
        <p>JUST A RATHER VERY INTELLIGENT SYSTEM</p>
    </div>

    <div id="chat" class="chat"></div>

    <div class="inputRow">
        <input
            id="input"
            class="input"
            type="text"
            autocomplete="off"
            placeholder="Ask JARVIS anything..."
        >
        <button id="send">SEND</button>
    </div>

    <div class="hint">
        TRY: "WHAT IS A BLACK HOLE?" • "2 + 7 × 4" • "TELL ME A JOKE"
    </div>

</div>

<script>
"use strict";

/* =========================================
   ELEMENTS
========================================= */

const chat = document.getElementById("chat");
const input = document.getElementById("input");
const send = document.getElementById("send");

/* =========================================
   MEMORY
========================================= */

const memory = {
    name: localStorage.getItem("jarvis_name") || "",
    lastQuestion: "",
    lastAnswer: "",
    lastTopic: ""
};

/* =========================================
   CHAT DISPLAY
========================================= */

function addMessage(who, text) {

    const box = document.createElement("div");

    box.className =
        who === "You"
            ? "msg user"
            : "msg jarvis";

    const label = document.createElement("div");
    label.className = "label";
    label.textContent = who.toUpperCase();

    const body = document.createElement("div");
    body.textContent = text;

    box.appendChild(label);
    box.appendChild(body);

    chat.appendChild(box);

    chat.scrollTop = chat.scrollHeight;
}

/* =========================================
   JARVIS VOICE
========================================= */

function speak(text) {

    if (!("speechSynthesis" in window)) {
        return;
    }

    window.speechSynthesis.cancel();

    const cleanText = text
        .replace(/https?:\/\/\S+/gi, "")
        .slice(0, 1200);

    const voice = new SpeechSynthesisUtterance(cleanText);

    /*
       Deeper/slower JARVIS-style delivery.
       The exact voice depends on voices installed
       on the iPad/browser.
    */

    voice.rate = 0.88;
    voice.pitch = 0.72;
    voice.volume = 1.0;

    const voices =
        window.speechSynthesis.getVoices();

    const preferred =
        voices.find(v =>
            /Daniel|Alex|Arthur|Google UK English Male/i
            .test(v.name)
        );

    if (preferred) {
        voice.voice = preferred;
    }

    window.speechSynthesis.speak(voice);
}

/* Some browsers load voices after page startup. */
if ("speechSynthesis" in window) {
    window.speechSynthesis.onvoiceschanged = function() {};
}

/* =========================================
   REPLY
========================================= */

function reply(text, speakIt = true) {

    memory.lastAnswer = text;

    addMessage("JARVIS", text);

    if (speakIt) {
        speak(text);
    }
}

/* =========================================
   NORMALIZATION
========================================= */

function normalize(text) {

    return text
        .toLowerCase()
        .replace(/[’]/g, "'")
        .replace(/\s+/g, " ")
        .trim();
}

/* =========================================
   MATH
========================================= */

function calculate(expression) {

    let value = expression
        .replace(/×/g, "*")
        .replace(/÷/g, "/")
        .replace(/−/g, "-")
        .replace(/\^/g, "**")
        .replace(/,/g, "")
        .trim();

    /*
       Only mathematical characters are accepted.
       This prevents arbitrary JavaScript from
       being interpreted as a calculation.
    */

    if (!/^[0-9+\-*/().%\s*xX]+$/.test(value)) {
        return null;
    }

    value = value.replace(
        /(\d+(?:\.\d+)?)\s*[xX]\s*(\d+(?:\.\d+)?)/g,
        "$1*$2"
    );

    if (!/[+\-*/%]/.test(value)) {
        return null;
    }

    try {

        const result =
            Function(
                '"use strict"; return (' +
                value +
                ")"
            )();

        if (
            typeof result !== "number" ||
            !Number.isFinite(result)
        ) {
            return null;
        }

        if (Number.isInteger(result)) {
            return String(result);
        }

        return String(
            Number(result.toFixed(10))
        );

    } catch (_) {

        return null;
    }
}

function detectMath(question) {

    let expression = question
        .replace(
            /^(what is|calculate|compute|solve)\s+/i,
            ""
        )
        .replace(/[?=]\s*$/, "")
        .trim();

    if (
        !/^[0-9+\-*/().%\s×÷−^xX]+$/.test(expression)
    ) {
        return null;
    }

    return calculate(expression);
}

/* =========================================
   DATE / TIME
========================================= */

function currentTime() {

    return new Intl.DateTimeFormat(
        undefined,
        {
            hour: "numeric",
            minute: "2-digit",
            second: "2-digit"
        }
    ).format(new Date());
}

function currentDate() {

    return new Intl.DateTimeFormat(
        undefined,
        {
            weekday: "long",
            year: "numeric",
            month: "long",
            day: "numeric"
        }
    ).format(new Date());
}

/* =========================================
   JOKES
========================================= */

const jokes = [

    "Why did the computer get cold? It left its Windows open.",

    "Why was the math book sad? It had too many problems.",

    "Why did the programmer quit his job? He didn't get arrays.",

    "Why do programmers prefer dark mode? Because light attracts bugs.",

    "What do you call a computer that sings? A Dell-icious performer.",

    "Why was the computer tired? It had too many tabs open.",

    "What did the robot say after a long day? I need to recharge my batteries.",

    "Why did the computer go to the doctor? It had a virus.",

    "What is a computer's favorite snack? Microchips.",

    "Why did the smartphone wear glasses? It lost its contacts.",

    "Why was the keyboard always calm? It knew how to keep its keys under control.",

    "Why did the robot cross the road? To recharge on the other side.",

    "What did one byte say to the other? You have a byte on you.",

    "Why did the Wi-Fi break up with the router? There was no connection.",

    "Why was the computer good at sports? It had a strong processor.",

    "Why did the programmer bring a ladder? He wanted to reach the next level.",

    "Why did the computer sit down? It needed a byte to eat.",

    "I tried to teach my computer chess. It beat me at my own game.",

    "My computer told me it needed space. So I deleted some files.",

    "Why don't computers ever get lost? They always know their location.",

    "I told my circuits a joke. They said it needed better delivery.",

    "Why did the robot become a comedian? It had great timing.",

    "What did the CPU say to the GPU? You complete me.",

    "Why did the server blush? Someone accessed its private data.",

    "Why was the computer so confident? It had a lot of RAM."
];

function randomJoke() {

    return jokes[
        Math.floor(
            Math.random() * jokes.length
        )
    ];
}

/* =========================================
   WEATHER
========================================= */

function weatherDescription(code) {

    const descriptions = {

        0: "clear sky",
        1: "mainly clear",
        2: "partly cloudy",
        3: "overcast",

        45: "foggy",
        48: "foggy",

        51: "light drizzle",
        53: "drizzle",
        55: "heavy drizzle",

        61: "light rain",
        63: "rain",
        65: "heavy rain",

        71: "light snow",
        73: "snow",
        75: "heavy snow",

        80: "rain showers",
        81: "rain showers",
        82: "heavy rain showers",

        95: "thunderstorms",
        96: "thunderstorms with hail",
        99: "thunderstorms with hail"
    };

    return descriptions[code] ||
        "unknown conditions";
}

function extractWeatherCity(question) {

    const match = question.match(
        /(?:weather|temperature|forecast)\s+(?:in|for|at)\s+(.+?)(?:\?|$)/i
    );

    if (!match) {
        return "";
    }

    return match[1].trim();
}

async function getWeather(city) {

    const geocodeURL =
        "https://geocoding-api.open-meteo.com/v1/search" +
        "?name=" +
        encodeURIComponent(city) +
        "&count=1&language=en&format=json";

    const geoResponse =
        await fetch(geocodeURL);

    if (!geoResponse.ok) {
        throw new Error("Geocoding failed");
    }

    const geo =
        await geoResponse.json();

    if (
        !geo.results ||
        geo.results.length === 0
    ) {
        return (
            "I could not find that city. " +
            "Try including the state or country."
        );
    }

    const place = geo.results[0];

    const forecastURL =
        "https://api.open-meteo.com/v1/forecast" +
        "?latitude=" +
        encodeURIComponent(place.latitude) +
        "&longitude=" +
        encodeURIComponent(place.longitude) +
        "&current=temperature_2m,apparent_temperature,weather_code,wind_speed_10m" +
        "&temperature_unit=fahrenheit" +
        "&wind_speed_unit=mph" +
        "&timezone=auto";

    const weatherResponse =
        await fetch(forecastURL);

    if (!weatherResponse.ok) {
        throw new Error("Weather failed");
    }

    const data =
        await weatherResponse.json();

    const current =
        data.current;

    return (
        "Current weather in " +
        place.name +
        ": " +
        Math.round(current.temperature_2m) +
        "°F, " +
        weatherDescription(
            current.weather_code
        ) +
        ". Feels like " +
        Math.round(
            current.apparent_temperature
        ) +
        "°F, with winds around " +
        Math.round(
            current.wind_speed_10m
        ) +
        " mph."
    );
}

/* =========================================
   WIKIPEDIA KNOWLEDGE
========================================= */

async function lookupKnowledge(question) {

    let topic = question
        .replace(
            /^who is\s+/i,
            ""
        )
        .replace(
            /^who was\s+/i,
            ""
        )
        .replace(
            /^what is\s+/i,
            ""
        )
        .replace(
            /^what are\s+/i,
            ""
        )
        .replace(
            /^what was\s+/i,
            ""
        )
        .replace(
            /^tell me about\s+/i,
            ""
        )
        .replace(
            /^explain\s+/i,
            ""
        )
        .replace(
            /^define\s+/i,
            ""
        )
        .replace(
            /[?]+$/,
            ""
        )
        .trim();

    if (!topic || topic.length > 180) {
        return null;
    }

    const url =
        "https://en.wikipedia.org/api/rest_v1/page/summary/" +
        encodeURIComponent(
            topic.replace(/\s+/g, "_")
        );

    const response =
        await fetch(url);

    if (!response.ok) {
        return null;
    }

    const data =
        await response.json();

    if (!data.extract) {
        return null;
    }

    let text = data.extract;

    if (text.length > 1400) {
        text =
            text
                .slice(0, 1400)
                .replace(/\s+\S*$/, "") +
            "…";
    }

    return (
        (data.title || topic) +
        ":\n" +
        text +
        "\n\nSource: Wikipedia"
    );
}

/* =========================================
   LOCAL BRAIN
========================================= */

function localBrain(question) {

    const q =
        normalize(question);

    /* Greetings */

    if (
        /^(hi|hello|hey|yo|sup|hiya)\b/.test(q)
    ) {
        return "Hello. JARVIS is online and ready.";
    }

    /* Identity */

    if (
        /who are you|what are you|your name/.test(q)
    ) {
        return (
            "I am J.A.R.V.I.S. — " +
            "your personal browser-based assistant."
        );
    }

    /* Capabilities */

    if (
        /what can you do|what do you do|help me/.test(q)
    ) {
        return (
            "I can handle conversation, " +
            "calculations, dates, time, weather, " +
            "jokes, factual lookups, memory, " +
            "and follow-up questions."
        );
    }

    /* Name */

    const nameMatch =
        question.match(
            /\b(?:my name is|call me)\s+([A-Za-z][A-Za-z0-9 _'-]{0,30})/i
        );

    if (nameMatch) {

        memory.name =
            nameMatch[1].trim();

        localStorage.setItem(
            "jarvis_name",
            memory.name
        );

        return (
            "Understood. I will call you " +
            memory.name +
            "."
        );
    }

    if (
        /what('?s| is) my name|do you know my name/.test(q)
    ) {
        if (memory.name) {
            return (
                "Your name is " +
                memory.name +
                "."
            );
        }

        return (
            "You have not told me your name yet."
        );
    }

    /* Time */

    if (
        /what time is it|current time|time right now/.test(q)
    ) {
        return (
            "The current time is " +
            currentTime() +
            "."
        );
    }

    /* Date */

    if (
        /what day is it|what date is it|today'?s date|current date/.test(q)
    ) {
        return (
            "Today is " +
            currentDate() +
            "."
        );
    }

    /* Math */

    const math =
        detectMath(question);

    if (math !== null) {
        return (
            "The answer is " +
            math +
            "."
        );
    }

    /* Jokes */

    if (
        /\bjoke\b|\bmake me laugh\b|\bfunny\b/.test(q)
    ) {
        return randomJoke();
    }

    /* More jokes */

    if (
        /another joke|one more joke|tell me another/.test(q)
    ) {
        return randomJoke();
    }

    /* Stop voice */

    if (
        /stop talking|stop speaking|stop voice|be quiet/.test(q)
    ) {

        if (
            "speechSynthesis" in window
        ) {
            window.speechSynthesis.cancel();
        }

        return "Voice output stopped.";
    }

    /* Last question */

    if (
        /what did i just ask|what was my question|my last question/.test(q)
    ) {

        if (memory.lastQuestion) {
            return (
                "You asked: \"" +
                memory.lastQuestion +
                "\""
            );
        }

        return (
            "You have not asked me anything yet."
        );
    }

    /* Follow-up */

    if (
        /tell me more|go on|continue|elaborate|explain more/.test(q)
    ) {

        if (memory.lastTopic) {

            return (
                "Certainly. We were discussing " +
                memory.lastTopic +
                ". Ask me which part you want me to expand on."
            );
        }

        return (
            "Certainly. Tell me which topic you want expanded."
        );
    }

    /* Thanks */

    if (
        /^(thanks|thank you|thx|appreciate it)/.test(q)
    ) {
        return "You're welcome.";
    }

    /* Bored */

    if (
        /i am bored|i'm bored/.test(q)
    ) {
        return (
            "I can fix that. Ask me a random science " +
            "question, give me a math problem, or " +
            "tell me to give you a random fact."
        );
    }

    /* Random fact */

    if (
        /random fact|fun fact|something interesting/.test(q)
    ) {
        return (
            "A day on Venus is longer than a Venusian year. " +
            "Venus takes about 243 Earth days to rotate once, " +
            "but about 225 Earth days to orbit the Sun."
        );
    }

    return null;
}

/* =========================================
   MAIN INTELLIGENCE ENGINE
========================================= */

async function answerQuestion(question) {

    const local =
        localBrain(question);

    if (local) {
        return local;
    }

    const q =
        normalize(question);

    /* Weather */

    if (
        /\b(weather|temperature|forecast)\b/.test(q)
    ) {

        const city =
            extractWeatherCity(question);

        if (!city) {
            return (
                "Tell me the city. For example: " +
                "\"What is the weather in Miami?\""
            );
        }

        try {
            return await getWeather(city);
        } catch (_) {
            return (
                "I could not reach the weather service " +
                "right now. Please check your connection " +
                "and try again."
            );
        }
    }

    /*
       Questions likely requiring factual knowledge.
    */

    const factual =
        /^(who|what|when|where|why|how|tell me|explain|define|which|is|are|was|were|did|does|can)\b/i
        .test(question) ||
        question.endsWith("?");

    if (factual) {

        try {

            const knowledge =
                await lookupKnowledge(question);

            if (knowledge) {
                return knowledge;
            }

        } catch (_) {
            /* Continue to local fallback. */
        }
    }

    /* Specific conversational responses */

    if (
        /how smart are you|are you smart/.test(q)
    ) {
        return (
            "My capabilities depend on the systems connected " +
            "to this version of JARVIS. I can calculate, " +
            "remember conversation context, retrieve factual " +
            "information, access weather data, and respond " +
            "with voice. I am not an unlimited AI model."
        );
    }

    if (
        /good morning|good afternoon|good evening/.test(q)
    ) {
        return (
            "Good day. All primary JARVIS systems are online."
        );
    }

    /*
       Unknown question.
    */

    return (
        "I don't have a reliable answer for that yet. " +
        "Try asking me to explain the topic, calculate " +
        "something, look up a person or place, check " +
        "the weather, or tell you a joke."
    );
}

/* =========================================
   SEND SYSTEM
========================================= */

let busy = false;

async function sendMessage() {

    if (busy) {
        return;
    }

    const question =
        input.value.trim();

    if (!question) {
        return;
    }

    busy = true;

    input.value = "";

    addMessage(
        "You",
        question
    );

    memory.lastQuestion =
        question;

    try {

        const result =
            await answerQuestion(
                question
            );

        memory.lastAnswer =
            result;

        memory.lastTopic =
            question
                .replace(/[?]+$/, "")
                .slice(0, 120);

        reply(
            result,
            true
        );

    } catch (_) {

        reply(
            "I encountered a problem processing that request. Please try again.",
            false
        );

    } finally {

        busy = false;
        input.focus();
    }
}

/* =========================================
   BUTTON / ENTER
========================================= */

send.addEventListener(
    "click",
    sendMessage
);

input.addEventListener(
    "keydown",
    function(event) {

        if (event.key === "Enter") {
            sendMessage();
        }
    }
);

/* =========================================
   STARTUP
========================================= */

if (memory.name) {

    addMessage(
        "JARVIS",
        "Welcome back, " +
        memory.name +
        ". Systems online. How may I assist?"
    );

} else {

    addMessage(
        "JARVIS",
        "Systems online. How may I assist?"
    );
}

input.focus();

</script>

</body>
</html>
