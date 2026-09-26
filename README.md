<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="JARVIS">
<meta name="theme-color" content="#02070d">

<title>J.A.R.V.I.S.</title>

<style>
* {
    box-sizing: border-box;
}

html, body {
    margin: 0;
    width: 100%;
    height: 100%;
    background: #02070d;
    color: #d9fbff;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    overflow: hidden;
}

body {
    background:
        radial-gradient(circle at 50% 42%, #073447 0%, #02131d 30%, #02070d 68%, #000 100%);
}

body::before {
    content: "";
    position: fixed;
    inset: 0;
    pointer-events: none;
    background:
        linear-gradient(rgba(0,220,255,.035) 1px, transparent 1px),
        linear-gradient(90deg, rgba(0,220,255,.035) 1px, transparent 1px);
    background-size: 28px 28px;
}

.app {
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
    align-items: center;
    justify-content: space-between;
}

.brand {
    font-size: 25px;
    font-weight: 800;
    letter-spacing: 5px;
    color: #bffaff;
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
    box-shadow: 0 0 12px #65ff9a;
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
    border: 1px solid rgba(0,224,255,.8);
    box-shadow:
        0 0 22px rgba(0,224,255,.25),
        inset 0 0 20px rgba(0,224,255,.1);
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
    background: linear-gradient(
        90deg,
        transparent,
        #00eaff,
        transparent
    );
    box-shadow: 0 0 18px #00eaff;
}

.panel {
    width: 100%;
    max-height: 160px;
    overflow: auto;
    margin-top: 20px;
    padding: 14px;
    border-radius: 14px;
    border: 1px solid rgba(0,224,255,.35);
    background: rgba(0,15,24,.72);
    box-shadow: 0 0 30px rgba(0,180,255,.08);
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
    border: 1px solid rgba(0,224,255,.5);
    outline: none;
    background: rgba(0,20,30,.9);
    color: white;
    font-size: 16px;
}

input:focus {
    box-shadow: 0 0 15px rgba(0,224,255,.2);
}

button {
    border: 1px solid #00dfff;
    border-radius: 12px;
    background: rgba(0,160,210,.16);
    color: #bffaff;
    font-weight: 700;
    padding: 0 16px;
}

button:active {
    background: rgba(0,220,255,.3);
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
        <div class="brand">J.A.R.V.I.S.</div>

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
            <div class="who">JARVIS</div>
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

        <button id="send">SEND</button>
    </div>

    <div class="actions">
        <button id="voice">🔊 VOICE</button>
        <button id="clear">CLEAR</button>
    </div>

    <div class="footer">
        PERSONAL JARVIS • LOCAL WEB APP
    </div>

</div>

<script>

const answer = document.getElementById("answer");
const input = document.getElementById("input");

let speechOn = true;


/* ---------------- VOICE ---------------- */

function speak(text) {

    if (!speechOn) return;

    if (!("speechSynthesis" in window)) return;

    speechSynthesis.cancel();

    const voice = new SpeechSynthesisUtterance(text);

    voice.lang = "en-GB";
    voice.rate = 0.88;
    voice.pitch = 0.78;
    voice.volume = 1;

    speechSynthesis.speak(voice);
}


/* ---------------- CLEAN TEXT ---------------- */

function clean(text) {

    return text
        .toLowerCase()
        .trim()
        .replace(/[?!.,]+$/g, "");
}


/* ---------------- NUMBER FORMAT ---------------- */

function formatNumber(number) {

    if (Number.isInteger(number)) {
        return String(number);
    }

    return String(Number(number.toFixed(8)));
}


/* ---------------- MATH ---------------- */

function solveMath(text) {

    let expression = text
        .toLowerCase()
        .replace(/what is/g, "")
        .replace(/calculate/g, "")
        .replace(/solve/g, "")
        .replace(/equals/g, "")
        .replace(/=/g, "")
        .trim();

    if (!expression) return null;

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

        if (!tokens) return null;

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

        return formatNumber(result);

    } catch {

        return null;
    }
}


/* ---------------- JARVIS BRAIN ---------------- */

function respond(raw) {

    const text = clean(raw);

    if (!text) {
        return "I'm listening.";
    }


    /* MATH */

    const math = solveMath(text);

    if (math !== null) {
        return "The answer is " + math + ".";
    }


    /* GREETINGS */

    if (
        /^(hi|hello|hey|yo|sup|good morning|good afternoon|good evening)\b/
        .test(text)
    ) {

        return "Hello. JARVIS is online and ready.";
    }


    /* IDENTITY */

    if (
        text.includes("who are you") ||
        text.includes("what are you")
    ) {

        return "I am JARVIS, your personal digital assistant.";
    }


    if (text.includes("your name")) {

        return "My designation is J.A.R.V.I.S.";
    }


    /* TIME */

    if (text.includes("time")) {

        const time = new Intl.DateTimeFormat(
            undefined,
            {
                hour: "numeric",
                minute: "2-digit"
            }
        ).format(new Date());

        return "The current time is " + time + ".";
    }


    /* DATE */

    if (
        text.includes("date") ||
        text.includes("day is it")
    ) {

        const date = new Intl.DateTimeFormat(
            undefined,
            {
                weekday: "long",
                month: "long",
                day: "numeric",
                year: "numeric"
            }
        ).format(new Date());

        return "Today is " + date + ".";
    }


    /* STATUS */

    if (
        text.includes("status") ||
        text.includes("systems")
    ) {

        return "All primary systems are online. Core stable. Interface nominal.";
    }


    /* ARMOR */

    if (text.includes("armor")) {

        return "Armor systems are standing by. All primary systems report nominal.";
    }


    /* REACTOR */

    if (
        text.includes("reactor") ||
        text.includes("power")
    ) {

        return "Arc reactor simulation is stable. Power systems are nominal.";
    }


    /* JOKE */

    if (text.includes("joke")) {

        return "Why did the computer get cold? It left its Windows open.";
    }


    /* THANKS */

    if (text.includes("thank")) {

        return "You're welcome. Always at your service.";
    }


    /* SLANG */

    if (text.includes("slang")) {

        return "Slang mode enabled. What's good? I got you.";
    }


    /* WEATHER */

    if (text.includes("weather")) {

        return "I can handle the JARVIS interface locally, but live weather needs an online weather service.";
    }


    /* HELP */

    if (text.includes("help")) {

        return "Try asking me the time, date, a math problem, my status, armor, reactor, or a joke.";
    }


    /* GOODBYE */

    if (
        text.includes("bye") ||
        text.includes("good night")
    ) {

        return "Until next time. JARVIS standing by.";
    }


    /* UNKNOWN */

    return "I understand the request, but my local knowledge is limited. Try asking me for the time, date, math, system status, armor, reactor, or a joke.";
}


/* ---------------- RUN ---------------- */

function runJarvis() {

    const text = input.value.trim();

    if (!text) return;

    const response = respond(text);

    answer.textContent = response;

    speak(response);

    input.value = "";

    input.focus();
}


/* ---------------- BUTTONS ---------------- */

document.getElementById("send").onclick = runJarvis;


input.addEventListener("keydown", function(event) {

    if (event.key === "Enter") {
        runJarvis();
    }

});


document.getElementById("voice").onclick = function() {

    speechOn = !speechOn;

    this.textContent =
        speechOn
        ? "🔊 VOICE"
        : "🔇 VOICE OFF";

    if (!speechOn && "speechSynthesis" in window) {
        speechSynthesis.cancel();
    }

};


document.getElementById("clear").onclick = function() {

    answer.textContent =
        "Interface cleared. JARVIS is standing by.";

    input.focus();

};

</script>

</body>
</html>