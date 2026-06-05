<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>🎙️ SpeakUp – AI Speaking Coach</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700;800&display=swap" rel="stylesheet"/>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
  body {
    min-height: 100vh;
    background: linear-gradient(135deg, #0f0c29, #302b63, #24243e);
    font-family: 'DM Sans', 'Segoe UI', sans-serif;
    color: #f0eeff;
    display: flex;
    flex-direction: column;
  }
  @keyframes bounce {
    0%, 80%, 100% { transform: translateY(0); }
    40% { transform: translateY(-10px); }
  }
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }
  ::-webkit-scrollbar { width: 5px; }
  ::-webkit-scrollbar-thumb { background: rgba(255,255,255,0.1); border-radius: 10px; }

  /* HEADER */
  #header {
    width: 100%;
    background: rgba(255,255,255,0.05);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid rgba(255,255,255,0.08);
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 14px 24px;
    position: sticky;
    top: 0;
    z-index: 10;
  }
  .logo { font-size: 22px; font-weight: 800; letter-spacing: -0.5px; color: #c4b5fd; }
  .badge {
    background: rgba(196,181,253,0.15);
    border: 1px solid rgba(196,181,253,0.3);
    border-radius: 20px;
    padding: 4px 12px;
    font-size: 13px;
    color: #c4b5fd;
  }
  #resetBtn {
    background: transparent;
    border: 1px solid rgba(255,255,255,0.2);
    border-radius: 8px;
    color: #aaa;
    padding: 6px 14px;
    cursor: pointer;
    font-size: 13px;
    font-family: inherit;
    display: none;
  }
  #resetBtn:hover { background: rgba(255,255,255,0.05); }

  /* CONTAINER */
  #container {
    width: 100%;
    max-width: 760px;
    margin: 0 auto;
    padding: 40px 20px 20px;
    flex: 1;
    display: flex;
    flex-direction: column;
  }

  /* SECTION TITLES */
  .section-title {
    text-align: center;
    font-size: 28px;
    font-weight: 800;
    margin-bottom: 8px;
    background: linear-gradient(90deg, #c4b5fd, #818cf8);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }
  .section-sub {
    text-align: center;
    color: #9ca3af;
    font-size: 15px;
    margin-bottom: 36px;
  }

  /* GRID CARDS */
  .grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 16px;
  }
  .card {
    background: rgba(255,255,255,0.05);
    border: 2px solid rgba(255,255,255,0.08);
    border-radius: 18px;
    padding: 28px 24px;
    cursor: pointer;
    transition: all 0.2s ease;
    display: flex;
    flex-direction: column;
    gap: 8px;
    animation: fadeIn 0.3s ease;
  }
  .card:hover {
    background: rgba(255,255,255,0.09);
    border-color: rgba(196,181,253,0.4);
    transform: translateY(-2px);
  }
  .card-icon { font-size: 36px; }
  .card-label { font-size: 17px; font-weight: 700; }
  .card-sub { font-size: 13px; color: #9ca3af; }

  /* PHASE SCREENS */
  .phase { display: none; flex: 1; flex-direction: column; }
  .phase.active { display: flex; }

  /* CHAT */
  #chatArea {
    flex: 1;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 20px;
    padding-bottom: 20px;
    min-height: 300px;
    max-height: calc(100vh - 280px);
  }
  .msg-user {
    align-self: flex-end;
    background: linear-gradient(135deg, #6d28d9, #4f46e5);
    border-radius: 18px 18px 4px 18px;
    padding: 14px 18px;
    max-width: 75%;
    font-size: 15px;
    line-height: 1.6;
    animation: fadeIn 0.25s ease;
  }
  .msg-ai {
    align-self: flex-start;
    max-width: 85%;
    animation: fadeIn 0.25s ease;
  }
  .bubble {
    background: rgba(255,255,255,0.07);
    border: 1px solid rgba(255,255,255,0.1);
    border-radius: 18px 18px 18px 4px;
    padding: 14px 18px;
    font-size: 15px;
    line-height: 1.7;
  }
  .feedback {
    margin-top: 10px;
    background: rgba(99,102,241,0.12);
    border: 1px solid rgba(99,102,241,0.25);
    border-radius: 12px;
    padding: 14px 16px;
    font-size: 13px;
    line-height: 1.8;
    color: #c4b5fd;
    white-space: pre-line;
  }
  .listen-btn {
    margin-top: 8px;
    background: transparent;
    border: 1px solid rgba(255,255,255,0.15);
    border-radius: 20px;
    color: #9ca3af;
    padding: 5px 14px;
    cursor: pointer;
    font-size: 12px;
    font-family: inherit;
  }
  .listen-btn:hover { background: rgba(255,255,255,0.05); }

  /* LOADING DOTS */
  .loading-dots {
    display: flex;
    gap: 6px;
    padding: 16px;
    align-self: flex-start;
  }
  .dot {
    width: 8px; height: 8px;
    border-radius: 50%;
    background: #818cf8;
  }
  .dot:nth-child(1) { animation: bounce 1.2s ease-in-out 0s infinite; }
  .dot:nth-child(2) { animation: bounce 1.2s ease-in-out 0.2s infinite; }
  .dot:nth-child(3) { animation: bounce 1.2s ease-in-out 0.4s infinite; }

  /* INPUT AREA */
  .input-area {
    border-top: 1px solid rgba(255,255,255,0.08);
    padding-top: 16px;
    margin-top: 8px;
  }
  .error-msg {
    color: #f87171;
    font-size: 13px;
    padding: 6px 4px;
    line-height: 1.5;
    display: none;
  }
  .input-row {
    display: flex;
    gap: 10px;
    align-items: flex-end;
  }
  #userInput {
    flex: 1;
    background: rgba(255,255,255,0.06);
    border: 1px solid rgba(255,255,255,0.12);
    border-radius: 14px;
    color: #f0eeff;
    font-size: 15px;
    padding: 12px 16px;
    resize: none;
    outline: none;
    font-family: inherit;
    line-height: 1.5;
    min-height: 50px;
    max-height: 140px;
  }
  #userInput::placeholder { color: #555; }
  #userInput:focus { border-color: rgba(196,181,253,0.4); }
  .icon-btn {
    background: rgba(255,255,255,0.08);
    border: none;
    border-radius: 50%;
    width: 48px; height: 48px;
    cursor: pointer;
    font-size: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    transition: all 0.2s;
    color: white;
  }
  #micBtn.listening { background: #ef4444; }
  #micBtn:hover { background: rgba(255,255,255,0.15); }
  #micBtn.listening:hover { background: #dc2626; }
  #sendBtn {
    background: linear-gradient(135deg, #6d28d9, #4f46e5);
    border: none;
    border-radius: 50%;
    width: 48px; height: 48px;
    cursor: pointer;
    font-size: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
    color: white;
    transition: all 0.2s;
  }
  #sendBtn:disabled { background: rgba(99,102,241,0.3); cursor: not-allowed; }
  #sendBtn:not(:disabled):hover { opacity: 0.85; }

  @media (max-width: 480px) {
    .grid { grid-template-columns: 1fr; }
    #container { padding: 24px 14px 14px; }
    .section-title { font-size: 22px; }
  }
</style>
</head>
<body>

<!-- HEADER -->
<div id="header">
  <div class="logo">🎙️ SpeakUp</div>
  <div id="headerRight">
    <span class="badge">AI Speaking Coach</span>
  </div>
</div>

<div id="container">

  <!-- PHASE 1: Language -->
  <div class="phase active" id="phaseLang">
    <div class="section-title">Choose Your Language</div>
    <div class="section-sub">Select the language you want to practice</div>
    <div class="grid">
      <div class="card" onclick="selectLang('en')">
        <div class="card-icon">🇺🇸</div>
        <div class="card-label">English</div>
        <div class="card-sub">American English</div>
      </div>
      <div class="card" onclick="selectLang('de')">
        <div class="card-icon">🇩🇪</div>
        <div class="card-label">Deutsch</div>
        <div class="card-sub">German</div>
      </div>
    </div>
  </div>

  <!-- PHASE 2: Topic -->
  <div class="phase" id="phaseTopic">
    <div class="section-title" id="topicTitle">Choose a Topic</div>
    <div class="section-sub">Pick what you'd like to talk about today</div>
    <div class="grid" id="topicGrid"></div>
  </div>

  <!-- PHASE 3: Chat -->
  <div class="phase" id="phaseChat">
    <div id="chatArea"></div>
    <div class="input-area">
      <div class="error-msg" id="micError"></div>
      <div class="error-msg" id="speakError"></div>
      <div class="input-row">
        <textarea id="userInput" rows="2" placeholder="Type your response..." onkeydown="handleKey(event)"></textarea>
        <button class="icon-btn" id="micBtn" onclick="toggleMic()" title="Speak">🎤</button>
        <button id="sendBtn" onclick="sendMessage()" disabled title="Send">➤</button>
      </div>
    </div>
  </div>

</div>

<script>
const TOPICS = {
  en: [
    { id:"daily",  label:"Daily Life",    icon:"☀️", prompt:"Let's talk about daily routine, hobbies, and everyday activities." },
    { id:"travel", label:"Travel",         icon:"✈️", prompt:"Let's discuss travel experiences, dream destinations, and adventures." },
    { id:"work",   label:"Work & Career",  icon:"💼", prompt:"Let's talk about jobs, career goals, and professional topics." },
    { id:"food",   label:"Food & Cooking", icon:"🍳", prompt:"Let's discuss favorite foods, recipes, and culinary experiences." },
    { id:"tech",   label:"Technology",     icon:"💻", prompt:"Let's talk about technology, gadgets, and the internet." },
    { id:"culture",label:"Culture & Arts", icon:"🎨", prompt:"Let's discuss movies, music, books, and art." },
  ],
  de: [
    { id:"daily",  label:"Alltag",           icon:"☀️", prompt:"Lass uns über den Alltag, Hobbys und tägliche Aktivitäten sprechen." },
    { id:"travel", label:"Reisen",            icon:"✈️", prompt:"Lass uns über Reiseerfahrungen und Traumziele sprechen." },
    { id:"work",   label:"Arbeit & Karriere", icon:"💼", prompt:"Lass uns über Job, Karriereziele und berufliche Themen sprechen." },
    { id:"food",   label:"Essen & Kochen",    icon:"🍳", prompt:"Lass uns über Lieblingsessen und Rezepte sprechen." },
    { id:"tech",   label:"Technologie",       icon:"💻", prompt:"Lass uns über Technologie und Gadgets sprechen." },
    { id:"culture",label:"Kultur & Kunst",    icon:"🎨", prompt:"Lass uns über Filme, Musik und Kunst sprechen." },
  ]
};

const SYSTEM = {
  en: (t) => `You are a friendly English speaking practice coach.
Topic: ${t}

After each user message respond with:
1. A natural conversational reply (2-4 sentences).
2. Then the exact line "---FEEDBACK---" followed by:
✅ What you did well: [note]
📝 Corrections: [corrections or "No mistakes!"]
💡 Better phrase: [suggestion]
🔤 Pronunciation tip: [tip]`,

  de: (t) => `Du bist ein freundlicher Deutschlerncoach.
Thema: ${t}

Nach jeder Nachricht antworte mit:
1. Einer natürlichen Antwort auf Deutsch (2-4 Sätze).
2. Dann der genauen Zeile "---FEEDBACK---" gefolgt von:
✅ Das hast du gut gemacht: [Hinweis]
📝 Korrekturen: [Korrekturen oder "Keine Fehler!"]
💡 Besserer Ausdruck: [Vorschlag]
🔤 Aussprache-Tipp: [Tipp]`
};

let lang = null, topic = null, history = [], isListening = false, recognition = null;

function showPhase(id) {
  document.querySelectorAll('.phase').forEach(p => p.classList.remove('active'));
  document.getElementById(id).classList.add('active');
}

function selectLang(l) {
  lang = l;
  const grid = document.getElementById('topicGrid');
  grid.innerHTML = '';
  TOPICS[l].forEach(t => {
    const d = document.createElement('div');
    d.className = 'card';
    d.innerHTML = `<div class="card-icon">${t.icon}</div><div class="card-label">${t.label}</div>`;
    d.onclick = () => startTopic(t);
    grid.appendChild(d);
  });
  showPhase('phaseTopic');
}

async function startTopic(t) {
  topic = t;
  history = [];
  document.getElementById('chatArea').innerHTML = '';
  showPhase('phaseChat');

  // Update header
  document.getElementById('headerRight').innerHTML = `
    <span class="badge">${lang === 'en' ? 'English' : 'Deutsch'} · ${t.icon} ${t.label}</span>
    <button id="resetBtn" onclick="resetApp()" style="background:transparent;border:1px solid rgba(255,255,255,0.2);border-radius:8px;color:#aaa;padding:6px 14px;cursor:pointer;font-size:13px;margin-left:8px;font-family:inherit">↩ Restart</button>
  `;

  document.getElementById('userInput').placeholder = lang === 'en' ? 'Type your response...' : 'Schreibe deine Antwort...';
  showLoading(true);

  const initMsg = lang === 'en'
    ? `Hello! I'd like to practice speaking about: ${t.label}. Please start our conversation!`
    : `Hallo! Ich möchte über ${t.label} sprechen. Bitte starte unser Gespräch!`;

  history.push({ role: 'user', content: initMsg });
  const resp = await callClaude();
  history.push({ role: 'assistant', content: resp });
  showLoading(false);
  const { reply, feedback } = parse(resp);
  appendAI(reply, feedback);
}

async function sendMessage() {
  const inp = document.getElementById('userInput');
  const text = inp.value.trim();
  if (!text) return;
  inp.value = '';
  inp.style.height = '';
  document.getElementById('sendBtn').disabled = true;

  appendUser(text);
  history.push({ role: 'user', content: text });
  showLoading(true);

  const resp = await callClaude();
  history.push({ role: 'assistant', content: resp });
  showLoading(false);
  const { reply, feedback } = parse(resp);
  appendAI(reply, feedback);
}

async function callClaude() {
  try {
    const res = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        model: 'claude-sonnet-4-20250514',
        max_tokens: 1000,
        system: SYSTEM[lang](topic.prompt),
        messages: history
      })
    });
    const data = await res.json();
    return data.content?.[0]?.text || '(No response)';
  } catch(e) {
    return 'Connection error. Please try again.\n---FEEDBACK---\n❌ Could not reach AI.';
  }
}

function parse(text) {
  const parts = text.split('---FEEDBACK---');
  return { reply: parts[0].trim(), feedback: parts[1]?.trim() || null };
}

function appendUser(text) {
  const el = document.createElement('div');
  el.className = 'msg-user';
  el.textContent = text;
  document.getElementById('chatArea').appendChild(el);
  scrollChat();
}

function appendAI(reply, feedback) {
  const wrap = document.createElement('div');
  wrap.className = 'msg-ai';

  const bubble = document.createElement('div');
  bubble.className = 'bubble';
  bubble.textContent = reply;
  wrap.appendChild(bubble);

  const btn = document.createElement('button');
  btn.className = 'listen-btn';
  btn.textContent = '🔊 Listen';
  btn.onclick = () => speakText(reply, btn);
  wrap.appendChild(btn);

  if (feedback) {
    const fb = document.createElement('div');
    fb.className = 'feedback';
    fb.textContent = feedback;
    wrap.appendChild(fb);
  }

  document.getElementById('chatArea').appendChild(wrap);
  scrollChat();
}

let loadingEl = null;
function showLoading(show) {
  if (show) {
    loadingEl = document.createElement('div');
    loadingEl.className = 'loading-dots';
    loadingEl.innerHTML = '<div class="dot"></div><div class="dot"></div><div class="dot"></div>';
    document.getElementById('chatArea').appendChild(loadingEl);
    scrollChat();
  } else if (loadingEl) {
    loadingEl.remove();
    loadingEl = null;
  }
}

function scrollChat() {
  const c = document.getElementById('chatArea');
  c.scrollTop = c.scrollHeight;
}

function handleKey(e) {
  if (e.key === 'Enter' && !e.shiftKey) {
    e.preventDefault();
    sendMessage();
  }
}

document.getElementById('userInput').addEventListener('input', function() {
  document.getElementById('sendBtn').disabled = !this.value.trim();
  this.style.height = 'auto';
  this.style.height = Math.min(this.scrollHeight, 140) + 'px';
});

// ── MIC ──
async function toggleMic() {
  if (isListening) {
    recognition?.stop();
    return;
  }
  hideErr('micError');
  const SR = window.SpeechRecognition || window.webkitSpeechRecognition;
  if (!SR) {
    showErr('micError', '❌ আপনার ব্রাউজার মাইক সাপোর্ট করে না। Chrome বা Edge ব্যবহার করুন।');
    return;
  }
  try {
    await navigator.mediaDevices.getUserMedia({ audio: true });
  } catch(e) {
    showErr('micError', '❌ মাইক পারমিশন দিন: ব্রাউজারের অ্যাড্রেস বারে 🔒 আইকন → Microphone → Allow');
    return;
  }
  recognition = new SR();
  recognition.lang = lang === 'en' ? 'en-US' : 'de-DE';
  recognition.interimResults = true;
  recognition.onresult = (e) => {
    const t = Array.from(e.results).map(r => r[0].transcript).join('');
    document.getElementById('userInput').value = t;
    document.getElementById('sendBtn').disabled = !t.trim();
  };
  recognition.onstart = () => {
    isListening = true;
    document.getElementById('micBtn').classList.add('listening');
    document.getElementById('micBtn').textContent = '⏹';
  };
  recognition.onend = () => {
    isListening = false;
    document.getElementById('micBtn').classList.remove('listening');
    document.getElementById('micBtn').textContent = '🎤';
  };
  recognition.onerror = (e) => {
    isListening = false;
    document.getElementById('micBtn').classList.remove('listening');
    document.getElementById('micBtn').textContent = '🎤';
    if (e.error === 'not-allowed') showErr('micError', '❌ মাইক পারমিশন দিন।');
    else if (e.error === 'network') showErr('micError', '❌ নেটওয়ার্ক সমস্যা।');
    else showErr('micError', '❌ মাইক এরর: ' + e.error);
  };
  recognition.start();
}

// ── SPEAK ──
function speakText(text, btn) {
  hideErr('speakError');
  if (!window.speechSynthesis) {
    showErr('speakError', '❌ আপনার ব্রাউজার Text-to-Speech সাপোর্ট করে না।');
    return;
  }
  window.speechSynthesis.cancel();
  const utter = new SpeechSynthesisUtterance(text);
  utter.lang = lang === 'en' ? 'en-US' : 'de-DE';
  utter.rate = 0.9;
  utter.volume = 1;
  if (btn) {
    utter.onstart = () => { btn.textContent = '🔊 Playing...'; btn.disabled = true; };
    utter.onend = () => { btn.textContent = '🔊 Listen'; btn.disabled = false; };
    utter.onerror = () => {
      btn.textContent = '🔊 Listen';
      btn.disabled = false;
      showErr('speakError', '❌ অডিও বাজানো যায়নি। ডিভাইসের ভলিউম চেক করুন।');
    };
  }
  setTimeout(() => window.speechSynthesis.speak(utter), 100);
}

function showErr(id, msg) {
  const el = document.getElementById(id);
  el.textContent = msg;
  el.style.display = 'block';
}
function hideErr(id) {
  const el = document.getElementById(id);
  el.style.display = 'none';
}

function resetApp() {
  lang = null; topic = null; history = [];
  window.speechSynthesis?.cancel();
  recognition?.stop();
  document.getElementById('chatArea').innerHTML = '';
  document.getElementById('userInput').value = '';
  document.getElementById('sendBtn').disabled = true;
  document.getElementById('headerRight').innerHTML = '<span class="badge">AI Speaking Coach</span>';
  showPhase('phaseLang');
}
</script>
</body>
</html>
