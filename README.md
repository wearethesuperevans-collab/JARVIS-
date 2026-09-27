<script>

/* =========================================================
   J.A.R.V.I.S. CORE
========================================================= */

const app = document.getElementById("app");
const input = document.getElementById("input");
const send = document.getElementById("send");
const mic = document.getElementById("mic");
const chat = document.getElementById("chat");
const statusText = document.getElementById("statusText");

let processing = false;
let combatMode = false;

let memory = {
    name: "",
    lastQuestion: "",
    lastAnswer: "",
    lastTopic: ""
};


/* =========================================================
   VOICE OUTPUT
========================================================= */

function speak(text) {

    if (!window.speechSynthesis) {
        return;
    }

    speechSynthesis.cancel();

    const speech =
        new SpeechSynthesisUtterance(text);

    speech.rate = 0.88;
    speech.pitch = 0.72;
    speech.volume = 1;

    const voices =
        speechSynthesis.getVoices();

    const preferred = [
        "Daniel",
        "Alex",
        "Arthur",
        "Google UK English Male",
        "Microsoft George"
    ];

    let selected = null;

    for (const name of preferred) {

        selected =
            voices.find(v =>
                v.name
                    .toLowerCase()
                    .includes(name.toLowerCase())
            );

        if (selected) break;
    }

    if (!selected) {

        selected =
            voices.find(v =>
                v.lang &&
                v.lang
                    .toLowerCase()
                    .startsWith("en")
            );
    }

    if (selected) {
        speech.voice = selected;
    }

    speechSynthesis.speak(speech);
}


/* =========================================================
   CHAT
========================================================= */

function addMessage(
    text,
    who = "jarvis",
    voice = false
) {

    const message =
        document.createElement("div");

    message.className =
        "message " +
        (who === "user"
            ? "user"
            : "jarvis");

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
        chat.scrollTop =
            chat.scrollHeight;
    });

    if (
        voice &&
        who === "jarvis"
    ) {
        speak(text);
    }
}


/* =========================================================
   COMBAT MODE
========================================================= */

function enableCombatMode() {

    if (combatMode) {
        return;
    }

    combatMode = true;

    app.classList.add("combat");

    statusText.textContent =
        "COMBAT MODE";

    addMessage(
        "Combat initiated.",
        "jarvis",
        true
    );
}


function disableCombatMode() {

    if (!combatMode) {
        return;
    }

    combatMode = false;

    app.classList.remove("combat");

    statusText.textContent =
        "SYSTEMS ONLINE";

    addMessage(
        "Combat mode terminated. Systems returning to normal.",
        "jarvis",
        true
    );
}


/* =========================================================
   BRAIN-ROT DETECTOR 😭
========================================================= */

const brainRotTerms = [

    "skibidi",
    "skibidi toilet",
    "tung tung tung sahur",
    "tung tung tung",
    "sigma",
    "what the sigma",
    "rizz",
    "unspoken rizz",
    "gyatt",
    "gyat",
    "fanum tax",
    "ohio",
    "only in ohio",
    "grimace shake",
    "mewing",
    "looksmaxxing",
    "looksmax",
    "mog",
    "mogging",
    "aura",
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
    "frigo camelo",
    "trippi troppi",
    "brr brr",
    "tralala",
    "sigma boy",
    "sigma male",
    "sigma girl",
    "goofy ahh",
    "goofy",
    "npc",
    "sus",
    "sussy",
    "among us"
];


function isBrainRot(text) {

    const normalized =
        text
            .toLowerCase()
            .replace(/[^\w\s]/g, " ")
            .replace(/\s+/g, " ")
            .trim();

    return brainRotTerms.some(term =>
        normalized.includes(term)
    );
}


function brainRotResponse() {

    const responses = [

        "Wash your brain, son. 😭",

        "Sir... please step away from the brain rot. 😭",

        "J.A.R.V.I.S. detects critical levels of brain rot. Wash your brain, son. 😭",

        "That sentence has caused a 97% decrease in my processor dignity. 😭",

        "Brain-rot levels are exceeding safe operating limits. 😭",

        "I was built for advanced computing, not this. 😭"

    ];

    return responses[
        Math.floor(
            Math.random() *
            responses.length
        )
    ];
}


/* =========================================================
   MICROPHONE
========================================================= */

const SpeechRecognition =
    window.SpeechRecognition ||
    window.webkitSpeechRecognition;

let recognition = null;
let listening = false;


/*
   Some iPad/iPhone browsers do not expose
   SpeechRecognition at all.
*/

if (SpeechRecognition) {

    recognition =
        new SpeechRecognition();

    recognition.lang = "en-US";

    recognition.continuous = false;

    recognition.interimResults = false;

    recognition.maxAlternatives = 1;


    recognition.onstart = () => {

        listening = true;

        mic.classList.add("listening");

        mic.textContent = "⏹️";

        statusText.textContent =
            combatMode
                ? "COMBAT • LISTENING"
                : "LISTENING...";
    };


    recognition.onresult = event => {

        const result =
            event.results[0];

        if (!result || !result[0]) {
            return;
        }

        const spokenText =
            result[0].transcript.trim();

        if (!spokenText) {
            return;
        }

        input.value =
            spokenText;

        stopListening();

        sendMessage();
    };


    recognition.onerror = event => {

        console.log(
            "Speech recognition error:",
            event.error
        );

        stopListening();

        let message =
            "I couldn't access the microphone.";

        if (
            event.error === "not-allowed" ||
            event.error === "service-not-allowed"
        ) {

            message =
                "Microphone access was blocked. Check your browser's microphone permission for this website.";
        }

        else if (
            event.error === "no-speech"
        ) {

            message =
                "I didn't hear anything. Tap the microphone and speak clearly.";
        }

        else if (
            event.error === "audio-capture"
        ) {

            message =
                "I couldn't access an available microphone.";
        }

        else if (
            event.error === "network"
        ) {

            message =
                "The browser's speech-recognition service isn't available right now.";
        }

        addMessage(
            message,
            "jarvis",
            true
        );
    };


    recognition.onend = () => {

        stopListening();
    };

} else {

    /*
       Instead of silently doing nothing,
       tell the user exactly what happened.
    */

    mic.addEventListener(
        "click",
        () => {

            addMessage(
                "This browser doesn't provide speech recognition to J.A.R.V.I.S. Try opening the GitHub Pages site in a browser that supports Web Speech recognition.",
                "jarvis",
                true
            );
        }
    );
}


function stopListening() {

    listening = false;

    mic.classList.remove(
        "listening"
    );

    mic.textContent = "🎙️";

    statusText.textContent =
        combatMode
            ? "COMBAT MODE"
            : "SYSTEMS ONLINE";
}


mic.addEventListener(
    "click",
    () => {

        if (!recognition) {
            return;
        }

        if (listening) {

            try {
                recognition.stop();
            } catch {}

            return;
        }

        try {

            recognition.start();

        } catch (error) {

            console.log(error);

            stopListening();
        }
    }
);


/* =========================================================
   MATH ENGINE
========================================================= */

function solveMath(text) {

    let expression =
        text
            .toLowerCase()

            .replace(
                /what is/g,
                ""
            )

            .replace(
                /calculate/g,
                ""
            )

            .replace(
                /solve/g,
                ""
            )

            .replace(
                /multiplied by/g,
                "*"
            )

            .replace(
                /divided by/g,
                "/"
            )

            .replace(
                /plus/g,
                "+"
            )

            .replace(
                /minus/g,
                "-"
            )

            .replace(
                /times/g,
                "*"
            )

            .replace(
                /over/g,
                "/"
            )

            .replace(
                /×/g,
                "*"
            )

            .replace(
                /÷/g,
                "/"
            )

            .replace(
                /[^0-9+\-*/().%\s]/g,
                ""
            )

            .trim();

    if (!expression) {
        return null;
    }

    if (!/[+\-*/%]/.test(expression)) {
        return null;
    }

    try {

        const result =
            Function(
                '"use strict";return(' +
                expression +
                ')'
            )();

        if (
            typeof result !== "number" ||
            !Number.isFinite(result)
        ) {
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

    "earth":
        "Earth is the third planet from the Sun and the only world currently known to support life.",

    "moon":
        "The Moon is Earth's natural satellite. Its gravitational interaction with Earth contributes strongly to ocean tides.",

    "sun":
        "The Sun is the star at the center of our Solar System. It is a giant sphere of hot plasma powered primarily by nuclear fusion.",

    "mars":
        "Mars is the fourth planet from the Sun. It is a rocky planet with a thin atmosphere dominated by carbon dioxide.",

    "jupiter":
        "Jupiter is the largest planet in our Solar System. It is a gas giant with a powerful magnetic field and the Great Red Spot.",

    "saturn":
        "Saturn is a gas giant famous for its extensive system of icy rings.",

    "venus":
        "Venus is the second planet from the Sun. Its dense atmosphere creates an extreme greenhouse effect.",

    "mercury":
        "Mercury is the smallest planet in the Solar System and the closest planet to the Sun.",

    "atom":
        "An atom is a basic unit of ordinary matter. It contains a nucleus made of protons and neutrons surrounded by electrons.",

    "dna":
        "DNA stores genetic information. Its structure is a double helix built from four main bases: A, T, C, and G.",

    "photosynthesis":
        "Photosynthesis allows plants, algae, and some bacteria to convert light energy into chemical energy.",

    "evolution":
        "Evolution is the change in inherited characteristics of populations across generations.",

    "speed of light":
        "The speed of light in vacuum is exactly 299,792,458 meters per second.",

    "neutron star":
        "A neutron star is an extremely dense stellar remnant formed from the collapsed core of certain massive stars.",

    "plate tectonics":
        "Plate tectonics describes the movement of large pieces of Earth's lithosphere. Their interactions produce earthquakes, mountains, and much volcanic activity.",

    "newton":
        "Isaac Newton developed foundational laws of motion and universal gravitation and made major contributions to mathematics and optics.",

    "marie curie":
        "Marie Curie was a physicist and chemist whose research into radioactivity earned Nobel Prizes in Physics and Chemistry.",

    "shakespeare":
        "William Shakespeare was an English playwright and poet whose works include Hamlet, Macbeth, and Romeo and Juliet."
};


function findKnowledge(question) {

    for (
        const key of Object.keys(knowledge)
    ) {

        if (
            question.includes(key)
        ) {
            return knowledge[key];
        }
    }

    return null;
}


/* =========================================================
   JOKES
========================================================= */

const jokes = [

    "Why did the computer get cold? It left its Windows open.",

    "Why was the math book sad? It had too many problems.",

    "Why don't scientists trust atoms? Because they make up everything.",

    "What do you call a computer that sings? A-Dell.",

    "Why did the photon refuse to check a bag? It was traveling light.",

    "Why was the computer tired? It had too many tabs open.",

    "What is a computer's favorite snack? Microchips.",

    "Why did the robot go on vacation? It needed to recharge.",

    "I would tell you a UDP joke, but you might not get it."

];


/* =========================================================
   LOCAL CONVERSATION
========================================================= */

function localConversation(q) {

    if (
        q === "combat mode"
    ) {

        enableCombatMode();

        return null;
    }


    if (
        q === "normal mode" ||
        q === "exit combat mode"
    ) {

        disableCombatMode();

        return null;
    }


    if (
        q === "hi" ||
        q === "hello" ||
        q === "hey"
    ) {

        return memory.name
            ? `Good to hear from you, ${memory.name}. Systems are online.`
            : "Good to hear from you. Systems are online and ready.";
    }


    if (
        q.startsWith("my name is ")
    ) {

        const name =
            q
                .replace(
                    "my name is ",
                    ""
                )
                .trim();

        memory.name =
            name
                .split(" ")
                .map(
                    word =>
                        word.charAt(0).toUpperCase() +
                        word.slice(1)
                )
                .join(" ");

        return `Understood. I'll call you ${memory.name}.`;
    }


    if (
        q.includes("what is my name") ||
        q.includes("what's my name")
    ) {

        return memory.name
            ? `Your name is ${memory.name}.`
            : "You haven't told me your name yet.";
    }


    if (
        q.includes("who are you") ||
        q.includes("what are you")
    ) {

        return "I am J.A.R.V.I.S., your digital assistant interface. I'm designed to converse with you, solve mathematics, answer questions, and search for information when necessary.";
    }


    if (
        q.includes("how are you")
    ) {

        return "All systems are operational. My processors are feeling particularly cooperative today.";
    }


    if (
        q.includes("what are you doing")
    ) {

        return "Monitoring the system and waiting for your next command.";
    }


    if (
        q === "thanks" ||
        q === "thank you"
    ) {

        return "You're welcome. Always a pleasure.";
    }


    if (
        q === "joke" ||
        q.includes("tell me a joke")
    ) {

        return jokes[
            Math.floor(
                Math.random() *
                jokes.length
            )
        ];
    }


    if (
        q.includes("what time")
    ) {

        return `The current time is ${
            new Date().toLocaleTimeString(
                [],
                {
                    hour: "numeric",
                    minute: "2-digit"
                }
            )
        }.`;
    }


    if (
        q.includes("what date") ||
        q.includes("what day")
    ) {

        return `Today is ${
            new Date().toLocaleDateString(
                [],
                {
                    weekday: "long",
                    month: "long",
                    day: "numeric",
                    year: "numeric"
                }
            )
        }.`;
    }


    if (
        q === "status" ||
        q.includes("system status")
    ) {

        return combatMode
            ? "Combat systems active. All primary systems operational."
            : "Systems online. Core stable. Voice interface online. Knowledge engine online.";
    }


    return null;
}


/* =========================================================
   SEARCH FILTER
========================================================= */

function isRealQuestion(q) {

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

    if (q.includes("?")) {
        return true;
    }

    return starters.some(
        starter =>
            q.startsWith(starter)
    );
}


function shouldSearch(q) {

    /*
       Brain rot and casual conversation
       NEVER gets sent to Wikipedia.
    */

    if (isBrainRot(q)) {
        return false;
    }

    if (!isRealQuestion(q)) {
        return false;
    }

    if (findKnowledge(q)) {
        return false;
    }

    return true;
}


/* =========================================================
   WIKIPEDIA
========================================================= */

async function searchWikipedia(question) {

    if (
        !shouldSearch(
            question.toLowerCase()
        )
    ) {
        return null;
    }

    try {

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
            Object.values(
                data.query.pages
            );

        if (!pages.length) {
            return null;
        }

        const page =
            pages[0];

        let answer =
            page.extract || "";

        if (!answer) {
            return null;
        }

        if (
            answer.length > 900
        ) {

            answer =
                answer.substring(
                    0,
                    900
                ) +
                "...";
        }

        return answer;

    } catch {

        return null;
    }
}


/* =========================================================
   RESPONSE ENGINE
========================================================= */

async function getResponse(question) {

    const q =
        question
            .toLowerCase()
            .trim();


    /*
       BRAIN ROT HAS PRIORITY.
       This prevents Wikipedia from
       hijacking the conversation.
    */

    if (isBrainRot(q)) {

        return brainRotResponse();
    }


    if (
        q === "combat mode"
    ) {

        enableCombatMode();

        return null;
    }


    if (
        q === "normal mode" ||
        q === "exit combat mode"
    ) {

        disableCombatMode();

        return null;
    }


    /*
       MATH IS ALWAYS LOCAL.
       NO SEARCH.
    */

    const math =
        solveMath(q);

    if (math !== null) {

        return `The answer is ${math}.`;
    }


    /*
       BUILT-IN KNOWLEDGE
    */

    const known =
        findKnowledge(q);

    if (known) {
        return known;
    }


    /*
       NORMAL CONVERSATION
    */

    const local =
        localConversation(q);

    if (local) {
        return local;
    }


    /*
       ONLY REAL QUESTIONS
       GET SEARCHED.
    */

    if (
        shouldSearch(q)
    ) {

        const result =
            await searchWikipedia(
                question
            );

        if (result) {
            return result;
        }
    }


    /*
       CASUAL CONVERSATION
       NEVER SEARCHES.
    */

    if (
        !isRealQuestion(q)
    ) {

        const responses = [

            "Understood.",

            "Interesting. I'm listening.",

            "Go on.",

            "I'm with you.",

            "Noted.",

            "Interesting. Continue.",

            "I hear you.",

            "Fair enough.",

            "I'm listening."
        ];

        return responses[
            Math.floor(
                Math.random() *
                responses.length
            )
        ];
    }


    return "I couldn't find a reliable answer for that. Try asking the question another way.";
}


/* =========================================================
   SEND
========================================================= */

async function sendMessage() {

    if (processing) {
        return;
    }

    const question =
        input.value.trim();

    if (!question) {
        return;
    }

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

    if (response) {

        memory.lastAnswer =
            response;

        addMessage(
            response,
            "jarvis",
            true
        );
    }

    processing = false;

    setTimeout(() => {

        try {

            input.focus({
                preventScroll: true
            });

        } catch {

            input.focus();
        }

    }, 50);
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
    event => {

        if (
            event.key === "Enter"
        ) {

            event.preventDefault();

            sendMessage();
        }
    }
);


/* =========================================================
   STARTUP
========================================================= */

addMessage(
    "Good day. J.A.R.V.I.S. systems are online. How may I assist you?",
    "jarvis",
    true
);

</script>
