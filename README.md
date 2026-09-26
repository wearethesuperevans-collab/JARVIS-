<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1,viewport-fit=cover">
<meta name="theme-color" content="#02070d">
<title>J.A.R.V.I.S.</title>

<style>
*{box-sizing:border-box}

html,body{
  margin:0;
  width:100%;
  height:100%;
  overflow:hidden;
  background:#010409;
  color:#dffaff;
  font-family:Arial,Helvetica,sans-serif;
}

body{
  display:flex;
  justify-content:center;
}

.app{
  width:100%;
  max-width:900px;
  height:100%;
  display:flex;
  flex-direction:column;
  padding:18px 16px 12px;
  background:
    radial-gradient(circle at 50% 28%,rgba(0,190,255,.13),transparent 34%),
    linear-gradient(180deg,#020b13,#010409 65%);
  border:1px solid rgba(0,220,255,.18);
  box-shadow:inset 0 0 70px rgba(0,180,255,.08);
}

.top{
  display:flex;
  justify-content:space-between;
  align-items:center;
  font-size:12px;
  letter-spacing:2px;
  color:#63eaff;
}

.status{
  color:#65ff9a;
}

.coreWrap{
  height:245px;
  display:flex;
  align-items:center;
  justify-content:center;
  flex-shrink:0;
}

.core{
  width:190px;
  height:190px;
  border-radius:50%;
  position:relative;
  border:2px solid #39ddff;
  box-shadow:
    0 0 18px rgba(0,220,255,.55),
    inset 0 0 25px rgba(0,200,255,.2);
}

.core:before,
.core:after{
  content:"";
  position:absolute;
  border-radius:50%;
  inset:17px;
  border:1px solid rgba(72,235,255,.7);
}

.core:after{
  inset:39px;
  border:2px solid rgba(0,160,255,.75);
}

.orb{
  position:absolute;
  left:50%;
  top:50%;
  width:55px;
  height:55px;
  transform:translate(-50%,-50%);
  border-radius:50%;
  background:#aef8ff;
  box-shadow:
    0 0 14px #5cecff,
    0 0 40px rgba(0,210,255,.8);
}

.ring{
  position:absolute;
  inset:-13px;
  border:1px dashed rgba(64,225,255,.55);
  border-radius:50%;
}

.title{
  text-align:center;
  margin-bottom:8px;
}

.title h1{
  margin:0;
  color:#7ceeff;
  font-size:25px;
  letter-spacing:5px;
  text-shadow:0 0 12px rgba(0,220,255,.75);
}

.title p{
  margin:5px 0 0;
  color:#68ff9b;
  font-size:11px;
  letter-spacing:3px;
}

.chat{
  flex:1;
  min-height:0;
  overflow:auto;
  padding:10px 2px 12px;
}

.msg{
  margin:8px 0;
  padding:11px 13px;
  border:1px solid rgba(75,220,255,.2);
  border-radius:12px;
  background:rgba(0,20,31,.58);
  line-height:1.45;
  white-space:pre-wrap;
  word-break:break-word;
}

.msg.jarvis{
  border-left:3px solid #43e7ff;
}

.msg.user{
  border-right:3px solid #66ff9b;
  text-align:right;
  color:#bfffd3;
}

.label{
  font-size:9px;
  letter-spacing:2px;
  opacity:.65;
  margin-bottom:4px;
}

.inputRow{
  display:flex;
  gap:8px;
  padding-top:8px;
}

.input{
  flex:1;
  min-width:0;
  border:1px solid rgba(66,225,255,.5);
  border-radius:12px;
  background:#03121c;
  color:#fff;
  padding:13px;
  font-size:16px;
  outline:none;
}

.input:focus{
  box-shadow:0 0 12px rgba(0,210,255,.18);
}

button{
  border:1px solid #36ddff;
  background:#06202b;
  color:#bff8ff;
  border-radius:12px;
  padding:0 15px;
  font-weight:700;
  letter-spacing:1px;
}

button:active{
  transform:scale(.98);
}

.hint{
  text-align:center;
  color:#64848e;
  font-size:9px;
  letter-spacing:1px;
  padding:8px 0 0;
}

@media(max-height:650px){
  .coreWrap{height:175px}
  .core{width:130px;height:130px}
  .title h1{font-size:20px}
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
      autocomplete="off"
      placeholder="Ask JARVIS anything…"
    >
    <button id="send">SEND</button>
  </div>

  <div class="hint">
    TRY: “WHAT IS BLACK HOLES?” • “WHAT IS THE WEATHER IN MIAMI?” • “2 + 7 × 4”
  </div>

</div>

<script>
'use strict';

const chat = document.getElementById('chat');
const input = document.getElementById('input');
const send = document.getElementById('send');

const memory = {
  name: localStorage.getItem('jarvis_name') || '',
  lastQuestion: '',
  lastAnswer: '',
  lastTopic: ''
};

function addMessage(who, text) {
  const box = document.createElement('div');
  box.className = 'msg ' + (who === 'You' ? 'user' : 'jarvis');

  const label = document.createElement('div');
  label.className = 'label';
  label.textContent = who.toUpperCase();

  const body = document.createElement('div');
  body.textContent = text;

  box.append(label, body);
  chat.appendChild(box);

  chat.scrollTop = chat.scrollHeight;
}

function speak(text) {
  if (!('speechSynthesis' in window)) return;

  window.speechSynthesis.cancel();

  const clean = text
    .replace(/https?:\/\/\S+/g, '')
    .slice(0, 900);

  const u = new SpeechSynthesisUtterance(clean);

  u.rate = 0.92;
  u.pitch = 0.82;
  u.volume = 1;

  const voices = window.speechSynthesis.getVoices();

  const preferred = voices.find(v =>
    /Alex|Daniel|Arthur|Samantha|Google UK English Male/i.test(v.name)
  );

  if (preferred) {
    u.voice = preferred;
  }

  window.speechSynthesis.speak(u);
}

function reply(text, shouldSpeak = true) {
  memory.lastAnswer = text;

  addMessage('JARVIS', text);

  if (shouldSpeak) {
    speak(text);
  }
}

function normalize(s) {
  return s
    .toLowerCase()
    .replace(/[’]/g, "'")
    .replace(/\s+/g, ' ')
    .trim();
}

/* =========================
   MATH
========================= */

function escapeMath(s) {
  return s
    .replace(/×/g, '*')
    .replace(/÷/g, '/')
    .replace(/−/g, '-')
    .replace(/,/g, '');
}

function calculate(expression) {
  let s = escapeMath(expression).trim();

  if (!s) return null;

  if (!/^[0-9+\-*/().%\s^xX]+$/.test(s)) {
    return null;
  }

  s = s.replace(
    /(\d+(?:\.\d+)?)\s*[xX]\s*(\d+(?:\.\d+)?)/g,
    '$1*$2'
  );

  s = s.replace(/\^/g, '**');

  if (!/[+\-*/%()]/.test(s)) {
    return null;
  }

  try {
    const value = Function(
      '"use strict"; return (' + s + ')'
    )();

    if (typeof value !== 'number' || !Number.isFinite(value)) {
      return null;
    }

    return Number.isInteger(value)
      ? String(value)
      : String(Number(value.toFixed(10)));

  } catch (_) {
    return null;
  }
}

function localMath(q) {
  const cleaned = q
    .replace(/^(what is|calculate|compute|solve)\s+/i, '')
    .replace(/[?=]$/, '')
    .trim();

  if (
    /^[0-9+\-*/().%\s×÷−^xX]+$/.test(cleaned)
  ) {
    return calculate(cleaned);
  }

  return null;
}

/* =========================
   DATE / TIME
========================= */

function formatDate(d) {
  return new Intl.DateTimeFormat(undefined, {
    weekday: 'long',
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  }).format(d);
}

function formatTime(d) {
  return new Intl.DateTimeFormat(undefined, {
    hour: 'numeric',
    minute: '2-digit',
    second: '2-digit'
  }).format(d);
}

/* =========================
   WEATHER
========================= */

function weatherCode(code) {

  const map = {
    0: 'clear sky',
    1: 'mainly clear',
    2: 'partly cloudy',
    3: 'overcast',
    45: 'foggy',
    48: 'foggy',
    51: 'light drizzle',
    53: 'drizzle',
    55: 'heavy drizzle',
    61: 'light rain',
    63: 'rain',
    65: 'heavy rain',
    71: 'light snow',
    73: 'snow',
    75: 'heavy snow',
    80: 'rain showers',
    81: 'rain showers',
    82: 'heavy rain showers',
    95: 'thunderstorms',
    96: 'thunderstorms with hail',
    99: 'thunderstorms with hail'
  };

  return map[code] || 'unknown conditions';
}

async function getWeather(city) {

  const geoURL =
    'https://geocoding-api.open-meteo.com/v1/search?name=' +
    encodeURIComponent(city) +
    '&count=1&language=en&format=json';

  const geoRes = await fetch(geoURL);

  if (!geoRes.ok) {
    throw new Error('geocoding failed');
  }

  const geo = await geoRes.json();

  if (!geo.results || !geo.results.length) {
    return 'I could not find that city. Try a city and state or country, such as “Miami, Florida”.';
  }

  const place = geo.results[0];

  const forecastURL =
    'https://api.open-meteo.com/v1/forecast?' +
    'latitude=' + encodeURIComponent(place.latitude) +
    '&longitude=' + encodeURIComponent(place.longitude) +
    '&current=temperature_2m,apparent_temperature,weather_code,wind_speed_10m' +
    '&temperature_unit=fahrenheit' +
    '&wind_speed_unit=mph' +
    '&timezone=auto';

  const weatherRes = await fetch(forecastURL);

  if (!weatherRes.ok) {
    throw new Error('weather failed');
  }

  const data = await weatherRes.json();
  const c = data.current;

  return (
    'Current weather in ' +
    place.name +
    (place.country ? ', ' + place.country : '') +
    ': ' +
    Math.round(c.temperature_2m) +
    '°F, ' +
    weatherCode(c.weather_code) +
    '. Feels like ' +
    Math.round(c.apparent_temperature) +
    '°F, with winds around ' +
    Math.round(c.wind_speed_10m) +
    ' mph.'
  );
}

function isWeather(q) {
  return /\b(weather|temperature|forecast|rain|raining|snowing|humidity)\b/i.test(q);
}

function weatherCity(q) {
  const m = q.match(
    /(?:weather|temperature|forecast)(?:\s+(?:in|for|at))?\s+(.+?)(?:\?|$)/i
  );

  return m ? m[1].trim() : '';
}

/* =========================
   WIKIPEDIA KNOWLEDGE
========================= */

async function wikipedia(query) {

  const clean = query
    .replace(/^who is\s+/i, '')
    .replace(/^what is\s+/i, '')
    .replace(/^what are\s+/i, '')
    .replace(/^tell me about\s+/i, '')
    .replace(/^explain\s+/i, '')
    .replace(/[?]+$/, '')
    .trim();

  if (!clean || clean.length > 180) {
    return null;
  }

  const url =
    'https://en.wikipedia.org/api/rest_v1/page/summary/' +
    encodeURIComponent(clean.replace(/\s+/g, '_'));

  const res = await fetch(url, {
    headers: {
      'Accept': 'application/json'
    }
  });

  if (!res.ok) {
    return null;
  }

  const data = await res.json();

  if (!data.extract) {
    return null;
  }

  const title = data.title || clean;

  const extract =
    data.extract.length > 1100
      ? data.extract.slice(0, 1100).replace(/\s+\S*$/, '') + '…'
      : data.extract;

  return title + ': ' + extract + '\n\nSource: Wikipedia';
}

/* =========================
   LOCAL JARVIS BRAIN
========================= */

function localAnswer(q) {

  const n = normalize(q);

  if (!n) {
    return 'Awaiting your question.';
  }

  if (
    /^(hi|hello|hey|yo|sup|good morning|good afternoon|good evening)\b/.test(n)
  ) {
    return 'Hello. JARVIS is online and ready.';
  }

  if (/who are you|what are you|your name/.test(n)) {
    return 'I am J.A.R.V.I.S. — your personal browser-based assistant.';
  }

  if (/what can you do|help|commands/.test(n)) {
    return 'I can handle conversation, calculations, date and time, weather, jokes, and factual lookups. Ask me naturally.';
  }

  if (/\b(joke|make me laugh)\b/.test(n)) {

    const jokes = [
      'Why did the computer get cold? It left its Windows open.',
      'Why was the math book sad? It had too many problems.',
      'I told my circuits a joke. They said it needed better delivery.',
      'Why do programmers prefer dark mode? Because light attracts bugs.'
    ];

    return jokes[Math.floor(Math.random() * jokes.length)];
  }

  if (/what time is it|current time|time right now/.test(n)) {
    return 'The current time is ' + formatTime(new Date()) + '.';
  }

  if (
    /what (day|date) is it|today'?s date|current date/.test(n)
  ) {
    return 'Today is ' + formatDate(new Date()) + '.';
  }

  if (
    /what did i (just )?ask|my last question|what was my question/.test(n)
  ) {
    return memory.lastQuestion
      ? 'You asked: “' + memory.lastQuestion + '”'
      : 'You have not asked me a question yet.';
  }

  if (/tell me more|go on|continue|elaborate/.test(n)) {
    return memory.lastTopic
      ? 'Certainly. We were discussing ' +
        memory.lastTopic +
        '. Ask me which part you want expanded.'
      : 'Certainly. Tell me the topic you want me to expand on.';
  }

  const nameMatch = q.match(
    /\b(?:my name is|call me)\s+([A-Za-z][A-Za-z0-9 _'-]{0,30})/i
  );

  if (nameMatch) {

    memory.name = nameMatch[1].trim();

    localStorage.setItem(
      'jarvis_name',
      memory.name
    );

    return 'Understood. I will call you ' +
      memory.name +
      '.';
  }

  if (/what('?s| is) my name|do you know my name/.test(n)) {
    return memory.name
      ? 'Your name is ' + memory.name + '.'
      : 'You have not told me your name yet.';
  }

  if (/thank(s| you)|appreciate it/.test(n)) {
    return 'You are welcome.';
  }

  if (/who made you|who created you/.test(n)) {
    return 'I am a custom JARVIS interface running in your browser. My capabilities come from the code and public services connected to this page.';
  }

  if (/stop talking|be quiet|stop voice/.test(n)) {

    if ('speechSynthesis' in window) {
      window.speechSynthesis.cancel();
    }

    return 'Voice output stopped.';
  }

  const math = localMath(q);

  if (math !== null) {
    return 'The answer is ' + math + '.';
  }

  if (/^(thanks|thx)$/.test(n)) {
    return 'Anytime.';
  }

  return null;
}

/* =========================
   MAIN ANSWER ENGINE
========================= */

async function answer(question) {

  const q = question.trim();

  const local = localAnswer(q);

  if (local) {
    return local;
  }

  if (isWeather(q)) {

    const city = weatherCity(q);

    if (!city) {
      return 'Tell me the city, for example: “What is the weather in Miami?”';
    }

    try {
      return await getWeather(city);
    } catch (_) {
      return 'I could not reach the weather service right now. Check your internet connection and try again.';
    }
  }

  const looksFactual =
    /^(who|what|when|where|why|how|tell me about|explain|define|is|are|can|does|did|which)\b/i.test(q) ||
    q.endsWith('?');

  if (looksFactual) {

    try {

      const result = await wikipedia(q);

      if (result) {
        return result;
      }

    } catch (_) {}
  }

  const n = normalize(q);

  if (/i am bored|i'm bored/.test(n)) {
    return 'I have an idea: ask me a random science question, a math problem, or “tell me something interesting.”';
  }

  if (/something interesting|fun fact|random fact/.test(n)) {
    return 'A day on Venus is longer than a Venusian year: Venus rotates once in about 243 Earth days but orbits the Sun in about 225 days.';
  }

  if (/how smart are you|are you smart/.test(n)) {
    return 'My abilities depend on the tools built into this version. I can reason through programmed tasks and look up many factual topics, but I am not an unlimited AI model.';
  }

  return 'I do not have a reliable answer for that yet. Try rephrasing the question, or ask me about a person, place, science topic, history, math, weather, or another factual subject.';
}

/* =========================
   SEND
========================= */

let busy = false;

async function sendMessage() {

  if (busy) return;

  const q = input.value.trim();

  if (!q) return;

  busy = true;

  input.value = '';

  addMessage('You', q);

  memory.lastQuestion = q;

  try {

    const result = await answer(q);

    memory.lastTopic =
      q.replace(/[?]+$/, '').slice(0, 100);

    reply(result, true);

  } catch (_) {

    reply(
      'Something went wrong while processing that. Please try again.',
      false
    );

  } finally {

    busy = false;
    input.focus();
  }
}

send.addEventListener('click', sendMessage);

input.addEventListener('keydown', e => {
  if (e.key === 'Enter') {
    sendMessage();
  }
});

/* =========================
   STARTUP
========================= */

addMessage(
  'JARVIS',
  memory.name
    ? 'Welcome back, ' + memory.name + '. Systems online. How may I assist?'
    : 'Systems online. How may I assist?'
);

input.focus();

</script>

</body>
</html>
