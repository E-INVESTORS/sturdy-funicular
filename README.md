<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>E-Investors Platform</title>
<meta name="theme-color" content="#2c5364" />
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#f4f6f9;
  --brand-1:#0f2027;
  --brand-2:#203a43;
  --accent:#2c5364;
  --muted:#7b8794;
  --card:#fff;
  --radius:12px;
}
*{box-sizing:border-box}
html,body{height:100%;margin:0;font-family:Inter, "Segoe UI", system-ui, sans-serif;background:var(--bg);color:#0b1b2b}
.header {
  background:linear-gradient(90deg,var(--brand-1),var(--brand-2),var(--accent));
  color:white;padding:18px 28px;display:flex;align-items:center;justify-content:space-between;
  gap:16px;
}
.brand {display:flex;align-items:center;gap:12px}
.logo{width:44px;height:44px;border-radius:8px;background:linear-gradient(135deg,#7dd3fc,#60a5fa);display:flex;align-items:center;justify-content:center;font-weight:700;color:#023047}
.header h1{margin:0;font-size:1.15rem}
.header .top-actions {display:flex;gap:12px;align-items:center}
.btn {background:var(--accent);color:white;border:none;padding:10px 14px;border-radius:8px;cursor:pointer}
.btn.secondary {background:transparent;border:1px solid rgba(255,255,255,0.12);color:white}
.container{padding:28px;max-width:1100px;margin:20px auto}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(260px,1fr));gap:18px}
.card{background:var(--card);padding:18px;border-radius:var(--radius);box-shadow:0 6px 18px rgba(12,20,30,0.06)}
.controls{display:flex;gap:10px;align-items:center;margin-bottom:14px}
.input,select{padding:10px;border-radius:8px;border:1px solid #e3e6ea;background:white;width:100%}
.login-wrap{max-width:420px;margin:24px auto}
.form-row{margin-bottom:12px}
.small{font-size:0.9rem;color:var(--muted)}
.role-title{font-weight:600;margin-bottom:10px}
.chatbox{position:fixed;right:20px;bottom:20px;width:320px;border-radius:12px;overflow:hidden;box-shadow:0 12px 35px rgba(2,6,23,0.18);background:var(--card);display:flex;flex-direction:column}
.chat-header{background:var(--accent);color:white;padding:10px;display:flex;justify-content:space-between;align-items:center}
.chat-body{height:220px;overflow:auto;padding:12px;font-size:14px;background:#fafbfd}
.chat-input{display:flex;padding:12px;border-top:1px solid #eef2f6}
.toggle-chat{background:transparent;border:none;color:white;cursor:pointer;font-weight:600}
.footer{margin-top:24px;text-align:center;color:var(--muted);font-size:0.9rem}
.badge{display:inline-block;background:#eef7fb;color:#0b4a6f;padding:6px 10px;border-radius:999px;font-weight:600;font-size:0.85rem}
@media (max-width:520px){.container{padding:16px}}
</style>
</head>
<body>

<header class="header" role="banner">
  <div class="brand">
    <div class="logo" aria-hidden="true">EI</div>
    <div>
      <h1>E-Investors</h1>
      <div class="small">Connecting Investors with high-potential companies</div>
    </div>
  </div>
  <div class="top-actions">
    <button class="btn" id="getStartedBtn">Get Started</button>
    <button class="btn secondary" id="logoutBtn" style="display:none">Logout</button>
  </div>
</header>

<main class="container" id="main">
  <!-- LOGIN -->
  <section id="loginSection" class="login-wrap" aria-labelledby="loginTitle">
    <div class="card">
      <h2 id="loginTitle">Sign in</h2>
      <form id="loginForm" novalidate>
        <div class="form-row">
          <label for="email" class="small">Email</label>
          <input id="email" type="email" class="input" required placeholder="you@example.com" aria-describedby="loginError">
        </div>
        <div class="form-row">
          <label for="password" class="small">Password</label>
          <div style="display:flex;gap:8px">
            <input id="password" type="password" class="input" required placeholder="Password">
            <button type="button" id="togglePw" class="btn secondary" aria-pressed="false">Show</button>
          </div>
        </div>
        <div class="form-row">
          <button class="btn" type="submit">Login</button>
          <div id="loginError" class="small" style="color:#c53030;margin-top:8px" role="status"></div>
        </div>
      </form>
      <div class="small">Use <b>admin@einvestors.com / admin123</b> to demo Admin</div>
    </div>
  </section>

  <!-- APP -->
  <section id="appSection" style="display:none" aria-live="polite">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:12px">
      <div>
        <div class="role-title" id="roleTitle">Dashboard</div>
        <div class="small">Search and explore companies / investors</div>
      </div>
      <div style="display:flex;gap:10px;align-items:center">
        <input id="search" class="input" placeholder="Search by name or state" aria-label="Search">
        <select id="filterType" class="input" style="width:160px">
          <option value="all">All</option>
          <option value="companies">Companies</option>
          <option value="investors">Investors</option>
        </select>
      </div>
    </div>

    <div id="cards" class="grid" aria-live="polite"></div>

    <div class="card" style="margin-top:18px">
      <h3>Financial Performance</h3>
      <div style="max-width:700px">
        <canvas id="financeChart" aria-label="Profit growth chart" role="img"></canvas>
      </div>
    </div>
  </section>

  <div class="footer">Built with care • Accessibility & security improvements applied</div>
</main>

<!-- Chat -->
<div class="chatbox" id="chatbox" aria-hidden="false">
  <div class="chat-header">
    <div><strong>Investree AI</strong></div>
    <div>
      <button class="toggle-chat" id="minimizeChat" title="Minimize chat">_</button>
    </div>
  </div>
  <div class="chat-body" id="chatBody">
    <p><strong>Investree:</strong> Hi! Ask me about companies or investors 📊</p>
  </div>
  <div class="chat-input">
    <input id="chatInput" class="input" placeholder="Ask Investree..." aria-label="Chat input">
    <button class="btn" id="sendChatBtn">Send</button>
  </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
/* ---------- Sample data ---------- */
const users = {
  "admin@einvestors.com": {pass:"admin123", role:"Admin"},
  "investor1@mail.com": {pass:"1234", role:"Investor"},
  "company1@mail.com": {pass:"1234", role:"Company"}
};

const companies = Array.from({length:25},(_,i)=>({
 name:`Company ${i+1}`,
 owner:`Owner ${i+1}`,
 state:["MH","DL","KA","GJ"][i%4],
 turnover:`${Math.floor(Math.random()*500+100)} Cr`,
 margin:`${(Math.random()*40+10).toFixed(2)}%`,
 profit:`${Math.floor(Math.random()*80)} Cr`
}));

const investors = Array.from({length:25},(_,i)=>({
 name:`Investor ${i+1}`,
 state:["MH","DL","KA","TN"][i%4],
 budget:`${Math.floor(Math.random()*300+50)} Cr`,
 verified: i%2 ? "Yes" : "Pending"
}));

/* ---------- DOM references ---------- */
const loginForm = document.getElementById('loginForm');
const loginError = document.getElementById('loginError');
const loginSection = document.getElementById('loginSection');
const appSection = document.getElementById('appSection');
const roleTitle = document.getElementById('roleTitle');
const cardsContainer = document.getElementById('cards');
const getStartedBtn = document.getElementById('getStartedBtn');
const logoutBtn = document.getElementById('logoutBtn');
const searchInput = document.getElementById('search');
const filterType = document.getElementById('filterType');
const togglePw = document.getElementById('togglePw');
const chatBody = document.getElementById('chatBody');
const chatInput = document.getElementById('chatInput');
const sendChatBtn = document.getElementById('sendChatBtn');
const minimizeChat = document.getElementById('minimizeChat');

let currentRole = null;
let chartInstance = null;

/* ---------- utils ---------- */
function escapeHTML(str){
  return String(str).replace(/[&<>"']/g, s => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'})[s]);
}

function clearChildren(el){ while(el.firstChild) el.removeChild(el.firstChild); }

/* ---------- rendering ---------- */
function createCard(data, type){
  const card = document.createElement('div');
  card.className = 'card';
  const title = document.createElement('h4');
  title.textContent = data.name;
  card.appendChild(title);
  const list = document.createElement('div');
  list.className = 'small';
  if(type === 'companies'){
    list.innerHTML = `${escapeHTML(data.owner)}<br>State: ${escapeHTML(data.state)}<br>Turnover: ${escapeHTML(data.turnover)}<br>Margin: ${escapeHTML(data.margin)}<br>Profit: ${escapeHTML(data.profit)}`;
  } else {
    list.innerHTML = `State: ${escapeHTML(data.state)}<br>Budget: ${escapeHTML(data.budget)}<br>Verified: ${escapeHTML(data.verified)}`;
  }
  card.appendChild(list);
  return card;
}

function loadDashboard(role){
  currentRole = role;
  roleTitle.textContent = `${role} Dashboard`;
  clearChildren(cardsContainer);
  let data = role === 'Investor' ? companies : investors;
  data.forEach(d => cardsContainer.appendChild(createCard(d, role === 'Investor' ? 'companies' : 'investors')));
  drawChart();
}

/* ---------- chart ---------- */
function drawChart(){
  const ctx = document.getElementById('financeChart').getContext('2d');
  if(chartInstance){ chartInstance.destroy(); }
  chartInstance = new Chart(ctx, {
    type: 'line',
    data: {
      labels: ["Jan","Feb","Mar","Apr","May","Jun"],
      datasets: [{
        label: "Profit Growth (Cr)",
        data: [10,18,15,25,30,40],
        borderColor: "#2c5364",
        tension: 0.3,
        pointRadius: 3,
        fill: false
      }]
    },
    options: { responsive:true, maintainAspectRatio:false }
  });
}

/* ---------- events ---------- */
getStartedBtn.addEventListener('click', ()=>{ loginSection.scrollIntoView({behavior:'smooth'}); });

togglePw.addEventListener('click', ()=>{
  const pw = document.getElementById('password');
  const showing = pw.type === 'text';
  pw.type = showing ? 'password' : 'text';
  togglePw.textContent = showing ? 'Show' : 'Hide';
  togglePw.setAttribute('aria-pressed', String(!showing));
});

loginForm.addEventListener('submit', (e)=>{
  e.preventDefault();
  const eVal = document.getElementById('email').value.trim();
  const pVal = document.getElementById('password').value;
  if(!eVal || !pVal){ loginError.textContent = 'Please enter email & password'; return; }
  if(users[eVal] && users[eVal].pass === pVal){
    loginError.textContent = '';
    loginSection.style.display = 'none';
    appSection.style.display = 'block';
    logoutBtn.style.display = 'inline-block';
    getStartedBtn.style.display = 'none';
    loadDashboard(users[eVal].role);
  } else {
    loginError.textContent = 'Invalid credentials';
  }
});

logoutBtn.addEventListener('click', ()=> location.reload());

searchInput.addEventListener('input', filterCards);
filterType.addEventListener('change', filterCards);

function filterCards(){
  const q = searchInput.value.trim().toLowerCase();
  const type = filterType.value;
  const data = (type === 'companies') ? companies : (type === 'investors') ? investors : (currentRole === 'Investor' ? companies : investors);
  // fallback when not logged in: show companies by default
  const base = (type === 'all') ? ((currentRole === 'Investor') ? companies.concat(investors) : companies.concat(investors)) : data;
  const filtered = base.filter(d => (d.name + ' ' + (d.state||'')).toLowerCase().includes(q));
  clearChildren(cardsContainer);
  filtered.slice(0, 200).forEach(d => cardsContainer.appendChild(createCard(d, companies.includes(d) ? 'companies' : 'investors')));
}

/* ---------- chat ---------- */
function appendChat(who, text){
  const p = document.createElement('p');
  p.innerHTML = `<strong>${escapeHTML(who)}:</strong> ${escapeHTML(text)}`;
  chatBody.appendChild(p);
  chatBody.scrollTop = chatBody.scrollHeight;
}
sendChatBtn.addEventListener('click', ()=>{
  const msg = chatInput.value.trim();
  if(!msg) return;
  appendChat('You', msg);
  appendChat('Investree', 'I can help analyze profits, margins, and investor matches.');
  chatInput.value = '';
});
chatInput.addEventListener('keydown', (e)=>{ if(e.key === 'Enter'){ sendChatBtn.click(); } });

minimizeChat.addEventListener('click', ()=>{
  const hb = document.getElementById('chatbox');
  const hidden = hb.style.height === '36px';
  if(!hidden){ hb.style.height = '36px'; chatBody.style.display = 'none'; document.querySelector('.chat-input').style.display = 'none'; minimizeChat.textContent = '+'; hb.setAttribute('aria-hidden','true'); }
  else { hb.style.height = ''; chatBody.style.display = ''; document.querySelector('.chat-input').style.display = ''; minimizeChat.textContent = '_'; hb.setAttribute('aria-hidden','false'); }
});

/* ---------- initial render ---------- */
/* show login by default */
loginSection.style.display = '';
appSection.style.display = 'none';
</script>
</body>
</html>
