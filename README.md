<script>
const chat = document.getElementById("chat");
const input = document.getElementById("input");
const send = document.getElementById("send");

const memory = {
    name: "",
    lastQuestion: "",
    lastAnswer: "",
    lastTopic: "",
    lastSource: ""
};

/* =========================
   JARVIS VOICE
========================= */

function speak(text) {
    if (!("speechSynthesis" in window)) return;

    speechSynthesis.cancel();

    const clean = text
        .replace(/https?:\/\/\S+/g, "")
        .replace(/Source:.*$/i, "");

    const utterance = new SpeechSynthesisUtterance(clean);

    utterance.rate = 0.88;
    utterance.pitch = 0.72;
    utterance.volume = 1;

    const voices = speechSynthesis.getVoices();

    const preferred = [
        "Daniel",
        "Alex",
        "Arthur",
        "Google UK English Male",
        "Microsoft George",
        "Microsoft Ryan"
    ];

    let selected = null;

    for (const name of preferred) {
        selected = voices.find(v =>
            v.name.toLowerCase().includes(name.toLowerCase())
        );

        if (selected) break;
    }

    if (!selected) {
        selected = voices.find(v =>
            /en-GB|en-US/i.test(v.lang) &&
            /male|daniel|alex|arthur|george|ryan/i.test(v.name)
        );
    }

    if (selected) utterance.voice = selected;

    speechSynthesis.speak(utterance);
}

if ("speechSynthesis" in window) {
    speechSynthesis.onvoiceschanged = () => {};
}


/* =========================
   BASIC HELPERS
========================= */

function normalize(text) {
    return text
        .toLowerCase()
        .replace(/[’']/g, "'")
        .replace(/[?!.,;:]/g, " ")
        .replace(/\s+/g, " ")
        .trim();
}

function addMessage(text, who = "jarvis") {
    const div = document.createElement("div");

    div.className = who === "user"
        ? "user-message"
        : "jarvis-message";

    div.textContent = text;

    chat.appendChild(div);
    chat.scrollTop = chat.scrollHeight;
}

function cleanQuestion(q) {
    return q
        .replace(/^(hey\s+)?jarvis[\s,:-]*/i, "")
        .replace(/^(please\s+)?/i, "")
        .trim();
}


/* =========================
   MATH ENGINE
========================= */

function calculateMath(expression) {

    let e = expression
        .toLowerCase()
        .replace(/what is/g, "")
        .replace(/calculate/g, "")
        .replace(/solve/g, "")
        .replace(/equals/g, "")
        .replace(/equal to/g, "")
        .replace(/multiplied by/g, "*")
        .replace(/times/g, "*")
        .replace(/divided by/g, "/")
        .replace(/plus/g, "+")
        .replace(/minus/g, "-")
        .replace(/over/g, "/")
        .replace(/percent of/g, "%")
        .replace(/percentage of/g, "%")
        .replace(/×/g, "*")
        .replace(/÷/g, "/")
        .replace(/\^/g, "**")
        .replace(/\s+/g, "");

    if (!e) return null;

    // Square roots
    e = e.replace(/sqrt\(([\d.]+)\)/g, "Math.sqrt($1)");
    e = e.replace(/√([\d.]+)/g, "Math.sqrt($1)");

    // Pi
    e = e.replace(/\bpi\b/g, "Math.PI");

    // Only allow safe mathematical characters
    if (!/^[0-9+\-*/().%a-zA-Z*]+$/.test(e)) {
        return null;
    }

    if (!/[0-9]/.test(e)) return null;

    try {
        const result = Function('"use strict"; return (' + e + ')')();

        if (
            typeof result !== "number" ||
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

function looksLikeMath(q) {
    const n = normalize(q);

    return (
        /^(what is|calculate|solve|evaluate)\b/.test(n) &&
        /\d/.test(n)
    ) ||
    (
        /^[0-9\s+\-*/().%^×÷]+$/.test(n) &&
        /\d/.test(n)
    );
}


/* =========================
   DATE / TIME
========================= */

function currentTime() {
    return new Date().toLocaleTimeString([], {
        hour: "numeric",
        minute: "2-digit"
    });
}

function currentDate() {
    return new Date().toLocaleDateString([], {
        weekday: "long",
        month: "long",
        day: "numeric",
        year: "numeric"
    });
}


/* =========================
   BUILT-IN SCIENCE KNOWLEDGE
========================= */

const knowledge = {

    "black hole": {
        keywords: [
            "black hole",
            "black holes",
            "event horizon",
            "singularity",
            "schwarzschild"
        ],
        answer:
        "A black hole is a region of spacetime where gravity is so strong that nothing that crosses its event horizon can escape, including light. Most black holes form when very massive stars collapse at the end of their lives. The boundary around a black hole is called the event horizon. At the center, classical general relativity predicts a singularity, although physicists do not yet have a complete theory combining gravity with quantum mechanics to describe that region."
    },

    "gravity": {
        keywords: [
            "gravity",
            "gravitational force"
        ],
        answer:
        "Gravity is the interaction that causes objects with mass and energy to attract one another. Newton described gravity as a force, while Einstein's general relativity describes gravity as the curvature of spacetime caused by mass and energy."
    },

    "relativity": {
        keywords: [
            "relativity",
            "einstein relativity",
            "special relativity",
            "general relativity"
        ],
        answer:
        "Einstein's theory of relativity has two major parts. Special relativity describes motion at high speeds and shows that space and time are connected. General relativity describes gravity as the curvature of spacetime caused by matter and energy."
    },

    "atom": {
        keywords: [
            "atom",
            "atoms",
            "atomic structure"
        ],
        answer:
        "An atom is the basic unit of ordinary matter. It has a nucleus containing protons and usually neutrons, surrounded by electrons. The number of protons determines which chemical element the atom belongs to."
    },

    "dna": {
        keywords: [
            "dna",
            "deoxyribonucleic acid",
            "genetic code",
            "genetics"
        ],
        answer:
        "DNA is the molecule that stores genetic information in living organisms. It contains instructions used by cells to build proteins and carry out biological functions. DNA has a double-helix structure made from four main bases: adenine, thymine, cytosine and guanine."
    },

    "evolution": {
        keywords: [
            "evolution",
            "natural selection",
            "darwin evolution"
        ],
        answer:
        "Biological evolution is the change in inherited characteristics of populations over generations. Natural selection is one major mechanism of evolution: traits that improve survival or reproduction can become more common in a population over time."
    },

    "photosynthesis": {
        keywords: [
            "photosynthesis",
            "plants make food",
            "how plants get energy"
        ],
        answer:
        "Photosynthesis is the process used by plants, algae and some microorganisms to convert light energy into chemical energy. Plants use carbon dioxide and water to produce sugars, while releasing oxygen as a byproduct."
    },

    "solar system": {
        keywords: [
            "solar system",
            "planets",
            "our planets"
        ],
        answer:
        "The Solar System consists of the Sun and everything gravitationally bound to it, including eight planets, dwarf planets, moons, asteroids, comets and other objects. The planets, in order from the Sun, are Mercury, Venus, Earth, Mars, Jupiter, Saturn, Uranus and Neptune."
    },

    "light": {
        keywords: [
            "speed of light",
            "what is light",
            "light travels"
        ],
        answer:
        "Light is electromagnetic radiation that can travel through empty space. In a vacuum, light travels at about 299,792 kilometers per second, or about 186,282 miles per second."
    },

    "atoms vs molecules": {
        keywords: [
            "molecule",
            "molecules",
            "atom vs molecule",
            "difference between atoms and molecules"
        ],
        answer:
        "An atom is a single unit of an element. A molecule consists of two or more atoms chemically bonded together. For example, an oxygen molecule contains two oxygen atoms, while a water molecule contains two hydrogen atoms and one oxygen atom."
    }
};


/* =========================
   HISTORY KNOWLEDGE
========================= */

const historyKnowledge = {

    "ancient egypt": {
        keywords: [
            "ancient egypt",
            "egyptian civilization",
            "pharaohs",
            "pyramids"
        ],
        answer:
        "Ancient Egypt was a civilization centered along the Nile River in northeastern Africa. It developed over thousands of years and is known for its writing system, monumental architecture, complex religion, mathematics and powerful kingdoms. The pyramids at Giza were built during Egypt's Old Kingdom."
    },

    "roman empire": {
        keywords: [
            "roman empire",
            "ancient rome",
            "roman history"
        ],
        answer:
        "The Roman Empire developed from the Roman Republic and eventually controlled a vast territory around the Mediterranean and beyond. Rome became one of the most influential civilizations in European, North African and Middle Eastern history. The Western Roman Empire traditionally ended in 476 CE."
    },

    "american revolution": {
        keywords: [
            "american revolution",
            "revolutionary war",
            "american revolutionary war"
        ],
        answer:
        "The American Revolution was the conflict in which the thirteen American colonies fought against British rule. The war began in 1775, and the United States declared independence in 1776. The conflict ended with the Treaty of Paris in 1783."
    },

    "world war 2": {
        keywords: [
            "world war 2",
            "world war ii",
            "ww2",
            "second world war"
        ],
        answer:
        "World War II was a global conflict fought from 1939 to 1945. The major Allied powers included the United States, Soviet Union, United Kingdom, China and France, while Germany, Italy and Japan were the principal Axis powers. The war ended in 1945 after Germany and Japan surrendered."
    },

    "industrial revolution": {
        keywords: [
            "industrial revolution",
            "industrialization"
        ],
        answer:
        "The Industrial Revolution was a period of major technological and economic change that began in Britain during the 18th century and spread to other regions. Mechanized production, factories, steam power and new transportation systems transformed manufacturing and society."
    }
};


/* =========================
   PEOPLE KNOWLEDGE
========================= */

const peopleKnowledge = {

    "albert einstein": {
        keywords: [
            "albert einstein",
            "einstein"
        ],
        answer:
        "Albert Einstein was a German-born theoretical physicist who developed the theory of relativity and made major contributions to modern physics. His 1905 work helped establish special relativity and explained the photoelectric effect, for which he received the 1921 Nobel Prize in Physics."
    },

    "isaac newton": {
        keywords: [
            "isaac newton",
            "newton"
        ],
        answer:
        "Isaac Newton was an English mathematician and physicist whose work helped establish classical mechanics. His laws of motion and law of universal gravitation became foundations of physics. He also made major contributions to mathematics and optics."
    },

    "marie curie": {
        keywords: [
            "marie curie",
            "madame curie"
        ],
        answer:
        "Marie Curie was a Polish-born physicist and chemist who conducted pioneering research on radioactivity. She discovered the elements polonium and radium and became the first person to receive two Nobel Prizes."
    },

    "leonardo da vinci": {
        keywords: [
            "leonardo da vinci",
            "leonardo"
        ],
        answer:
        "Leonardo da Vinci was an Italian Renaissance artist, engineer, inventor and scientist. He is famous for works such as the Mona Lisa and The Last Supper and for notebooks containing studies of anatomy, mechanics, nature and engineering."
    },

    "william shakespeare": {
        keywords: [
            "william shakespeare",
            "shakespeare"
        ],
        answer:
        "William Shakespeare was an English playwright and poet associated with the Elizabethan and Jacobean periods. His works include plays such as Hamlet, Macbeth and Romeo and Juliet, and he is one of the most studied writers in the English language."
    },

    "george washington": {
        keywords: [
            "george washington",
            "washington"
        ],
        answer:
        "George Washington was an American military leader and statesman who commanded the Continental Army during the American Revolution and became the first president of the United States under the Constitution."
    }
};


/* =========================
   FIND BUILT-IN KNOWLEDGE
========================= */

function findKnowledge(question) {

    const n = normalize(question);

    for (const key in knowledge) {

        const item = knowledge[key];

        for (const keyword of item.keywords) {

            if (n.includes(keyword)) {
                return item.answer;
            }
        }
    }

    for (const key in historyKnowledge) {

        const item = historyKnowledge[key];

        for (const keyword of item.keywords) {

            if (n.includes(keyword)) {
                return item.answer;
            }
        }
    }

    for (const key in peopleKnowledge) {

        const item = peopleKnowledge[key];

        for (const keyword of item.keywords) {

            if (n.includes(keyword)) {
                return item.answer;
            }
        }
    }

    return null;
}


/* =========================
   QUESTION CLEANING
========================= */

function extractTopic(question) {

    let q = question.toLowerCase();

    q = q
        .replace(/^(hey\s+)?jarvis[\s,:-]*/i, "")
        .replace(/^(can you|could you|would you|please)\s+/i, "")
        .replace(/^(tell me|tell me about)\s+/i, "")
        .replace(/^(explain|describe)\s+/i, "")
        .replace(/^(what is|what's|whats)\s+/i, "")
        .replace(/^(who is|who's|whos)\s+/i, "")
        .replace(/^(where is|where's|wheres)\s+/i, "")
        .replace(/^(when was|when did|when is)\s+/i, "")
        .replace(/^(how does|how do|how did|how can)\s+/i, "")
        .replace(/^(why does|why did|why is|why are)\s+/i, "")
        .replace(/^(what are|what were)\s+/i, "")
        .replace(/[?!.,]/g, " ")
        .replace(/\s+/g, " ")
        .trim();

    return q;
}


/* =========================
   WIKIPEDIA SEARCH
========================= */

async function wikipediaSearch(question) {

    let topic = extractTopic(question);

    if (!topic || topic.length < 2) {
        return null;
    }

    // Remove conversational leftovers
    topic = topic
        .replace(/\bthe\b/g, " ")
        .replace(/\babout\b/g, " ")
        .replace(/\bmean\b/g, " ")
        .replace(/\bmeans\b/g, " ")
        .replace(/\bwork\b/g, " ")
        .replace(/\bworks\b/g, " ")
        .replace(/\bexactly\b/g, " ")
        .replace(/\bin simple terms\b/g, " ")
        .replace(/\s+/g, " ")
        .trim();

    if (!topic) return null;

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

        const response = await fetch(searchURL);

        if (!response.ok) return null;

        const data = await response.json();

        if (
            !data.query ||
            !data.query.search ||
            data.query.search.length === 0
        ) {
            return null;
        }

        const result = data.query.search[0];

        const title = result.title;

        const summaryURL =
            "https://en.wikipedia.org/api/rest_v1/page/summary/" +
            encodeURIComponent(title.replace(/ /g, "_"));

        const summaryResponse = await fetch(summaryURL);

        if (!summaryResponse.ok) return null;

        const summary = await summaryResponse.json();

        if (!summary.extract) return null;

        let answer = summary.extract;

        // Keep answers readable
        if (answer.length > 1800) {
            answer = answer.substring(0, 1800) + "...";
        }

        memory.lastTopic = title;
        memory.lastSource = "Wikipedia";

        return answer +
            "\n\nSource: Wikipedia — " +
            title;

    } catch {
        return null;
    }
}


/* =========================
   CONVERSATION
========================= */

const jokes = [
    "Why did the computer get cold? It left its Windows open.",
    "Why was the math book sad? It had too many problems.",
    "Why do programmers prefer dark mode? Because light attracts bugs.",
    "Why did the scientist install a doorbell? They wanted to conduct more experiments.",
    "I told my computer I needed a break. Now it won't stop sending me vacation ads.",
    "Why did the photon refuse to check a bag? It was traveling light.",
    "What do you call an educated tube? A graduated cylinder.",
    "Why did the astronaut break up with the moon? It needed space.",
    "Why was the equal sign so humble? It knew it wasn't less than or greater than anyone.",
    "I would tell you a chemistry joke, but I know I wouldn't get a reaction.",
    "Why don't scientists trust atoms? Because they make up everything.",
    "Why did the computer go to the doctor? It had a virus.",
    "Why did the mathematician love fractions? They were a big part of the relationship.",
    "What do planets like to read? Comet books.",
    "Why was the history teacher calm? They knew everything would eventually be in the past.",
    "Why did the robot go on vacation? It needed to recharge.",
    "Why did the telescope get promoted? It had a great outlook.",
    "Why did the calculator break up with the pencil? It needed someone who could handle more advanced functions.",
    "What does a black hole eat for breakfast? Whatever crosses its event horizon.",
    "Why did the electron get in trouble? It was acting negatively.",
    "Why did the moon skip dinner? It was already full.",
    "Why was the geometry teacher so calm? They had everything under control.",
    "What did one volcano say to the other? I lava you.",
    "Why did the computer sit in the shade? It didn't want to overheat.",
    "Why did the historian bring a ladder? They wanted to reach the past."
];

function randomJoke() {
    return jokes[Math.floor(Math.random() * jokes.length)];
}


/* =========================
   LOCAL CONVERSATION BRAIN
========================= */

function localBrain(question) {

    const n = normalize(question);

    // Greetings
    if (
        /^(hi|hello|hey|yo|sup|good morning|good afternoon|good evening)\b/.test(n)
    ) {
        return memory.name
            ? `Good to see you, ${memory.name}. Systems are online.`
            : "Good to see you. JARVIS is online and ready.";
    }

    // Identity
    if (
        n.includes("who are you") ||
        n.includes("what are you") ||
        n.includes("your name")
    ) {
        return "I am JARVIS, your personal AI assistant. I can calculate, explain science and history, search for information, remember parts of our conversation, tell jokes and answer general questions.";
    }

    // Name
    const nameMatch = question.match(
        /(?:my name is|call me|you can call me)\s+([a-zA-Z0-9_-]+)/i
    );

    if (nameMatch) {
        memory.name = nameMatch[1];
        return `Understood. I'll call you ${memory.name}.`;
    }

    if (
        n.includes("what is my name") ||
        n.includes("do you know my name")
    ) {
        return memory.name
            ? `Your name is ${memory.name}.`
            : "You haven't told me your name yet.";
    }

    // Time
    if (
        n.includes("what time") ||
        n === "time"
    ) {
        return `The current time is ${currentTime()}.`;
    }

    // Date
    if (
        n.includes("what date") ||
        n.includes("today's date") ||
        n === "date"
    ) {
        return `Today is ${currentDate()}.`;
    }

    // Math
    if (looksLikeMath(question)) {

        const result = calculateMath(question);

        if (result !== null) {
            return `The answer is ${result}.`;
        }
    }

    // Jokes
    if (
        n.includes("tell me a joke") ||
        n.includes("tell me another joke") ||
        n === "joke" ||
        n.includes("make me laugh")
    ) {
        return randomJoke();
    }

    // Previous question
    if (
        n.includes("what did i ask") ||
        n.includes("my last question")
    ) {
        return memory.lastQuestion
            ? `Your last question was: "${memory.lastQuestion}"`
            : "We haven't had a previous question in this session.";
    }

    // Follow-up
    if (
        n === "tell me more" ||
        n === "more" ||
        n === "go on" ||
        n === "continue" ||
        n === "explain more"
    ) {

        if (memory.lastTopic) {
            return `Certainly. You were asking about ${memory.lastTopic}. I can give you more information if you ask a specific part you want explained.`;
        }

        return "Certainly. Tell me what subject you want me to expand on.";
    }

    // Thanks
    if (
        n === "thanks" ||
        n === "thank you" ||
        n.includes("thanks jarvis")
    ) {
        return "You're welcome.";
    }

    // Stop voice
    if (
        n === "stop talking" ||
        n === "stop speaking" ||
        n === "be quiet"
    ) {
        speechSynthesis.cancel();
        return "Voice output stopped.";
    }

    // Bored
    if (
        n.includes("i'm bored") ||
        n.includes("im bored")
    ) {
        return "I have an idea. Ask me about a black hole, an ancient civilization, a famous scientist, a mathematical problem, or something you've always wondered about.";
    }

    // Compliments
    if (
        n.includes("you're smart") ||
        n.includes("you are smart") ||
        n.includes("good job")
    ) {
        return "I'll take that as a successful systems check.";
    }

    // Confusion
    if (
        n === "i don't understand" ||
        n === "i dont understand"
    ) {
        return "No problem. Give me the subject and I'll explain it in simpler terms.";
    }

    return null;
}


/* =========================
   MAIN ANSWER ENGINE
========================= */

async function answerQuestion(question) {

    const cleaned = cleanQuestion(question);

    // 1. Conversation
    const local = localBrain(cleaned);

    if (local) {
        memory.lastAnswer = local;
        return local;
    }

    // 2. Built-in knowledge
    const builtIn = findKnowledge(cleaned);

    if (builtIn) {
        memory.lastTopic = extractTopic(cleaned);
        memory.lastSource = "JARVIS knowledge";
        memory.lastAnswer = builtIn;
        return builtIn;
    }

    // 3. Wikipedia / general knowledge
    const needsKnowledge = (
        cleaned.includes("?") ||
        /^(what|who|where|when|why|how|which|can|is|are|was|were|did|does|do)\b/i.test(cleaned) ||
        /history|science|scientist|math|mathematics|physics|chemistry|biology|earth|planet|space|country|person|people|war|event|invent|invention/i.test(cleaned)
    );

    if (needsKnowledge) {

        const wiki = await wikipediaSearch(cleaned);

        if (wiki) {
            memory.lastAnswer = wiki;
            return wiki;
        }
    }

    // 4. Conversational fallback
    return "I don't have a reliable answer for that yet. Try asking me in another way, or give me the subject and I'll search my knowledge sources.";
}


/* =========================
   SEND MESSAGE
========================= */

async function sendMessage() {

    const question = input.value.trim();

    if (!question) return;

    addMessage(question, "user");

    input.value = "";

    memory.lastQuestion = question;

    const thinking = document.createElement("div");
    thinking.className = "jarvis-message";
    thinking.textContent = "Processing...";
    chat.appendChild(thinking);
    chat.scrollTop = chat.scrollHeight;

    const answer = await answerQuestion(question);

    thinking.remove();

    addMessage(answer, "jarvis");

    speak(answer);
}


/* =========================
   BUTTON + ENTER
========================= */

send.addEventListener("click", sendMessage);

input.addEventListener("keydown", function(event) {

    if (event.key === "Enter") {
        event.preventDefault();
        sendMessage();
    }
});


/* =========================
   STARTUP
========================= */

setTimeout(() => {

    const startup =
        "JARVIS online. Knowledge systems initialized. " +
        "Ask me about science, mathematics, history, people, Earth, space, or anything you're curious about.";

    addMessage(startup, "jarvis");

}, 300);

</script>
