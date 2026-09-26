<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">

<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="JARVIS">
<meta name="theme-color" content="#02070d">

<title>J.A.R.V.I.S.</title>

<style>
* {
    box-sizing: border-box;
    -webkit-tap-highlight-color: transparent;
}

html,
body {
    margin: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    background: #02070d;
    color: #d9fbff;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
}

body {
    background:
        radial-gradient(
            circle at 50% 42%,
            #073447 0%,
            #02131d 30%,
            #02070d 68%,
            #000000 100%
        );
}

body::before {
    content: "";
    position: fixed;
    inset: 0;
    pointer-events: none;

    background:
        linear-gradient(
            rgba(0, 220, 255, 0.035) 1px,
            transparent 1px
        ),
        linear-gradient(
            90deg,
            rgba(0, 220, 255, 0.035) 1px,
            transparent 1px
        );

    background-size: 28px 28px;
}

.app {
    width: 100%;
    height: 100%;
    max-width: 900px;
    margin: auto;

    padding:
        calc(env(safe-area-inset-top) + 18px)
        18px
        calc(env(safe-area-inset-bottom) + 14px);

    display: flex;
    flex-direction: column;
}

.top {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.brand {
    font-size: 25px;
    font-weight: 800;
    letter-spacing: 5px;

    color: #c8fbff;

    text-shadow:
        0 0 8px #00dfff,
        0 0 22px #00aaff;
}

.online {
    color: #65ff9a;
    font-size: 12px;
    letter-spacing: 2px;
}

.dot {
    display: inline-block;

    width: 8px;
    height: 8px;

    border-radius: 50%;

    background: #65ff9a;

    box-shadow:
        0 0 8px #65ff9a,
        0 0 18px #65ff9a;

    margin-right: 7px;
}

.coreArea {
    flex: 1;
    min-height: 0;

    display: flex;
    flex-direction: column;

    align-items: center;
    justify-content: center;
}

.core {
    width: min(65vw, 310px);
    height: min(65vw, 310px);

    position: relative;

    display: grid;
    place-items: center;
}

.ring {
    position: absolute;

    border-radius: 50%;

    border: 1px solid rgba(0, 224, 255, 0.8);

    box-shadow:
        0 0 22px rgba(0, 224, 255, 0.25),
        inset 0 0 20px rgba(0, 224, 255, 0.1);
}

.r1 {
    inset: 3%;
    border-width: 2px;
}

.r2 {
    inset: 13%;
    border-style: dashed;
}

.r3 {
    inset: 24%;
    border-width: 3px;
}

.r4 {
    inset: 35%;

    border-color: #8af7ff;

    box-shadow:
        0 0 35px #00dfff,
        inset 0 0 25px #00dfff;
}

.core::before {
    content: "";

    width: 27%;
    height: 27%;

    border-radius: 50%;

    background: #eaffff;

    box-shadow:
        0 0 18px #ffffff,
        0 0 50px #00eaff,
        0 0 95px #00aaff;
}

.core::after {
    content: "";

    position: absolute;

    width: 100%;
    height: 1px;

    background:
        linear-gradient(
            90deg,
            transparent,
            #00eaff,
            transparent
        );

    box-shadow: 0 0 18px #00eaff;
}

.panel {
    width: 100%;
    max-height: 170px;

    overflow-y: auto;

    margin-top: 20px;
    padding: 14px;

    border-radius: 14px;

    border: 1px solid rgba(0, 224, 255, 0.35);

    background: rgba(0, 15, 24, 0.72);

    box-shadow:
        0 0 30px rgba(0, 180, 255, 0.08);
}

.who {
    margin-bottom: 6px;

    color: #6befff;

    font-size: 10px;
    letter-spacing: 2px;
}

.answer {
    font-size: 15px;
    line-height: 1.45;

    white-space: pre-wrap;
}

.inputRow {
    display: flex;
    gap: 8px;
}

input {
    flex: 1;
    min-width: 0;

    padding: 13px;

    border-radius: 12px;

    border: 1px solid rgba(0, 224, 255, 0.5);

    outline: none;

    background: rgba(0, 20, 30, 0.9);

    color: white;

    font-size: 16px;
}

input:focus {
    box-shadow:
        0 0 15px rgba(0, 224, 255, 0.2);
}

button {
    border: 1px solid #00dfff;

    border-radius: 12px;

    background: rgba(0, 160, 210, 0.16);

    color: #bffaff;

    font-weight: 700;

    padding: 0 16px;
}

button:active {
    background: rgba(0, 220, 255, 0.3);
}

.actions {
    display: flex;
    gap: 8px;

    margin-top: 8px;
}

.actions button {
    flex: 1;

    height: 38px;

    font-size: 12px;
}

.footer {
    margin-top: 9px;

    text-align: center;

    color: #477d8a;

    font-size: 9px;

    letter-spacing: 2px;
}
</style>
</head>

<body>

<div class="app">

    <div class="top">

        <div class="brand">
            J.A.R.V.I.S.
        </div>

        <div class="online">
            <span class="dot"></span>
            ONLINE
        </div>

    </div>


    <div class="coreArea">

        <div class="core">

            <div class="ring r1"></div>
            <div class="ring r2"></div>
            <div class="ring r3"></div>
            <div class="ring r4"></div>

        </div>


        <div class="panel">

            <div class="who">
                JARVIS
            </div>

            <div id="answer" class="answer">
                Good day. All systems are online. How may I assist?
            </div>

        </div>

    </div>


    <div class="inputRow">

        <input
            id="input"
            autocomplete="off"
            placeholder="Ask JARVIS anything..."
        >

        <button id="send">
            SEND
        </button>

    </div>


    <div class="actions">

        <button id="voice">
            🔊 VOICE
        </button>

        <button id="clear">
            CLEAR
        </button>

    </div>


    <div class="footer">
        PERSONAL JARVIS • LOCAL WEB APP
    </div>

</div>


<script>

/* =========================================================
   JARVIS
   Local conversational assistant
   ========================================================= */

const answer = document.getElementById("answer");
const input = document.getElementById("input");

let speechEnabled = true;


/* =========================================================
   CONVERSATION MEMORY
   ========================================================= */

let memory = {
    name: "",
    lastQuestion: "",
    lastTopic: "",
    lastAnswer: ""
};


/* =========================================================
   VOICE
   ========================================================= */

function speak(text) {

    if (!speechEnabled) {
        return;
    }

    if (!("speechSynthesis" in window)) {
        return;
    }

    speechSynthesis.cancel();

    const speech = new SpeechSynthesisUtterance(text);

    speech.lang = "en-GB";
    speech.rate = 0.88;
    speech.pitch = 0.78;
    speech.volume = 1;

    speechSynthesis.speak(speech);
}


/* =========================================================
   TEXT HELPERS
   ========================================================= */

function clean(text) {

    return text
        .toLowerCase()
        .trim()
        .replace(/[?!.,]+$/g, "");
}


function contains(text, words) {

    for (const word of words) {

        if (text.includes(word)) {
            return true;
        }

    }

    return false;
}


/* =========================================================
   MATH ENGINE
   ========================================================= */

function solveMath(text) {

    let expression = text
        .toLowerCase()
        .replace(/what is/g, "")
        .replace(/calculate/g, "")
        .replace(/solve/g, "")
        .replace(/equals/g, "")
        .replace(/equal to/g, "")
        .replace(/=/g, "")
        .trim();


    if (!expression) {
        return null;
    }


    if (!/^[0-9+\-*/().%\s]+$/.test(expression)) {
        return null;
    }


    if (!/[+\-*/%]/.test(expression)) {
        return null;
    }


    try {

        const tokens = expression.match(
            /(?:\d+(?:\.\d+)?)|[+\-*/%()]/g
        );


        if (!tokens) {
            return null;
        }


        let position = 0;


        function expressionPart() {

            let value = term();


            while (
                tokens[position] === "+" ||
                tokens[position] === "-"
            ) {

                const operator = tokens[position++];

                const next = term();


                if (operator === "+") {

                    value += next;

                } else {

                    value -= next;

                }

            }


            return value;
        }


        function term() {

            let value = factor();


            while (
                tokens[position] === "*" ||
                tokens[position] === "/"
            ) {

                const operator = tokens[position++];

                const next = factor();


                if (operator === "*") {

                    value *= next;

                } else {

                    if (next === 0) {
                        throw new Error("divide");
                    }

                    value /= next;
                }

            }


            return value;
        }


        function factor() {

            if (tokens[position] === "-") {

                position++;

                return -factor();
            }


            if (tokens[position] === "(") {

                position++;

                const value = expressionPart();


                if (tokens[position] !== ")") {
                    throw new Error("parentheses");
                }


                position++;

                return value;
            }


            const value = Number(tokens[position++]);


            if (!Number.isFinite(value)) {
                throw new Error("number");
            }


            if (tokens[position] === "%") {

                position++;

                return value / 100;
            }


            return value;
        }


        const result = expressionPart();


        if (
            position !== tokens.length ||
            !Number.isFinite(result)
        ) {

            return null;
        }


        return Number.isInteger(result)
            ? String(result)
            : String(Number(result.toFixed(8)));

    } catch {

        return null;
    }
}


/* =========================================================
   TIME
   ========================================================= */

function getTime() {

    return new Intl.DateTimeFormat(
        undefined,
        {
            hour: "numeric",
            minute: "2-digit"
        }
    ).format(new Date());
}


/* =========================================================
   DATE
   ========================================================= */

function getDate() {

    return new Intl.DateTimeFormat(
        undefined,
        {
            weekday: "long",
            month: "long",
            day: "numeric",
            year: "numeric"
        }
    ).format(new Date());
}


/* =========================================================
   GREETINGS
   ========================================================= */

function greeting(text) {

    if (
        /^(hi|hello|hey|yo|sup|what's up|whats up)\b/
        .test(text)
    ) {

        return "Hello. JARVIS is online and ready.";

    }


    if (text.includes("good morning")) {

        return "Good morning. All systems are online.";

    }


    if (text.includes("good afternoon")) {

        return "Good afternoon. JARVIS is standing by.";

    }


    if (text.includes("good evening")) {

        return "Good evening. How may I assist?";

    }


    return null;
}


/* =========================================================
   CONVERSATIONAL BRAIN
   ========================================================= */

function respond(raw) {

    const text = clean(raw);


    if (!text) {

        return "I'm listening.";

    }


    /*
     NAME MEMORY
    */

    const nameMatch = text.match(
        /(?:my name is|call me)\s+([a-zA-Z0-9_-]+)/
    );


    if (nameMatch) {

        memory.name = nameMatch[1];

        return (
            "Understood. I'll call you " +
            memory.name +
            "."
        );
    }


    if (
        text === "what is my name" ||
        text === "do you know my name"
    ) {

        if (memory.name) {

            return (
                "Your name is " +
                memory.name +
                "."
            );

        }

        return "You haven't told me your name yet.";

    }


    /*
     GREETINGS
    */

    const hello = greeting(text);

    if (hello) {
        return hello;
    }


    /*
     FOLLOW-UP QUESTIONS
    */

    if (
        text === "tell me more" ||
        text === "explain more" ||
        text === "go on" ||
        text === "continue"
    ) {

        if (memory.lastTopic) {

            return (
                "Certainly. We were discussing " +
                memory.lastTopic +
                ". Give me another question about it and I'll continue."
            );

        }

        return "Certainly. What would you like me to explain?";

    }


    if (
        text.includes("what did i just ask") ||
        text.includes("what was my last question")
    ) {

        if (memory.lastQuestion) {

            return (
                "Your last question was: " +
                memory.lastQuestion
            );

        }

        return "This is the first question in our current session.";

    }


    /*
     MATH
    */

    const mathResult = solveMath(text);

    if (mathResult !== null) {

        memory.lastTopic = "mathematics";

        return (
            "The answer is " +
            mathResult +
            "."
        );
    }


    /*
     IDENTITY
    */

    if (
        contains(text, [
            "who are you",
            "what are you",
            "what is jarvis",
            "tell me about yourself"
        ])
    ) {

        memory.lastTopic = "JARVIS";

        return (
            "I am JARVIS, your personal digital assistant. " +
            "I can handle calculations, time, dates, system information, " +
            "conversation, and several useful commands locally."
        );
    }


    /*
     NAME
    */

    if (text.includes("your name")) {

        return "My designation is J.A.R.V.I.S.";

    }


    /*
     TIME
    */

    if (
        text === "time" ||
        text.includes("what time") ||
        text.includes("current time")
    ) {

        memory.lastTopic = "the current time";

        return (
            "The current time is " +
            getTime() +
            "."
        );
    }


    /*
     DATE
    */

    if (
        text === "date" ||
        text.includes("what date") ||
        text.includes("today's date") ||
        text.includes("what day")
    ) {

        memory.lastTopic = "today's date";

        return (
            "Today is " +
            getDate() +
            "."
        );
    }


    /*
     STATUS
    */

    if (
        contains(text, [
            "status",
            "system status",
            "systems online",
            "systems"
        ])
    ) {

        memory.lastTopic = "system status";

        return (
            "All primary systems are online. " +
            "Core stable. Interface nominal. " +
            "Conversation system ready."
        );
    }


    /*
     ARMOR
    */

    if (text.includes("armor")) {

        memory.lastTopic = "armor systems";

        return (
            "Armor systems are standing by. " +
            "Diagnostics report nominal."
        );
    }


    /*
     REACTOR
    */

    if (
        text.includes("reactor") ||
        text.includes("power")
    ) {

        memory.lastTopic = "reactor power";

        return (
            "Arc reactor simulation is stable. " +
            "Power systems are nominal."
        );
    }


    /*
     JOKES
    */

    if (
        text.includes("tell me a joke") ||
        text === "joke" ||
        text.includes("make me laugh")
    ) {

        memory.lastTopic = "jokes";

        return (
            "Why did the computer get cold? " +
            "It left its Windows open."
        );
    }


    /*
     THANKS
    */

    if (
        contains(text, [
            "thank you",
            "thanks",
            "thx"
        ])
    ) {

        return "You're welcome. Always at your service.";

    }


    /*
     SLANG
    */

    if (
        text.includes("slang") ||
        text.includes("talk normal") ||
        text.includes("talk casual")
    ) {

        return (
            "Got you. Casual mode enabled. " +
            "What's good?"
        );
    }


    /*
     WEATHER
    */

    if (
        text.includes("weather") ||
        text.includes("temperature outside")
    ) {

        memory.lastTopic = "weather";

        return (
            "I can handle the JARVIS interface locally, " +
            "but live weather requires an online weather service."
        );
    }


    /*
     HELP
    */

    if (
        text === "help" ||
        text.includes("what can you do")
    ) {

        return (
            "I can handle math, percentages, time, dates, " +
            "system status, armor, reactor power, jokes, " +
            "basic conversation, name memory, and follow-up questions."
        );
    }


    /*
     GOODBYE
    */

    if (
        text.includes("goodbye") ||
        text === "bye" ||
        text.includes("good night")
    ) {

        return "Until next time. JARVIS standing by.";

    }


    /*
     POSITIVE CONVERSATION
    */

    if (
        contains(text, [
            "that's cool",
            "thats cool",
            "awesome",
            "nice",
            "cool"
        ])
    ) {

        return "Indeed. Glad I could assist.";

    }


    /*
     CONFUSION
    */

    if (
        contains(text, [
            "i don't understand",
            "i dont understand",
            "what do you mean"
        ])
    ) {

        return (
            "No problem. I'll explain it more simply. " +
            "Tell me which part is confusing."
        );
    }


    /*
     USER FEELING
    */

    if (
        contains(text, [
            "i'm bored",
            "im bored"
        ])
    ) {

        return (
            "Then let's change that. " +
            "I can tell you a joke, help with math, " +
            "or we can talk about something you're interested in."
        );
    }


    /*
     UNKNOWN REQUEST
    */

    return (
        "I understand you're asking about \"" +
        raw +
        "\". My local knowledge is limited for that topic, " +
        "but I'm ready for another question."
    );
}


/* =========================================================
   RUN JARVIS
   ========================================================= */

function runJarvis() {

    const text = input.value.trim();


    if (!text) {
        return;
    }


    const response = respond(text);


    memory.lastQuestion = text;
    memory.lastAnswer = response;


    answer.textContent = response;


    speak(response);


    input.value = "";

    input.focus();
}


/* =========================================================
   SEND
   ========================================================= */

document.getElementById("send").onclick = runJarvis;


input.addEventListener(
    "keydown",
    function(event) {

        if (event.key === "Enter") {

            runJarvis();

        }

    }
);


/* =========================================================
   VOICE BUTTON
   ========================================================= */

document.getElementById("voice").onclick =
function() {

    speechEnabled = !speechEnabled;


    this.textContent =
        speechEnabled
        ? "🔊 VOICE"
        : "🔇 VOICE OFF";


    if (
        !speechEnabled &&
        "speechSynthesis" in window
    ) {

        speechSynthesis.cancel();

    }

};


/* =========================================================
   CLEAR
   ========================================================= */

document.getElementById("clear").onclick =
function() {

    answer.textContent =
        "Interface cleared. JARVIS is standing by.";

    memory.lastQuestion = "";
    memory.lastTopic = "";
    memory.lastAnswer = "";

    input.focus();

};

</script>

</body>
</html>
