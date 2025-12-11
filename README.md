<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>eFootball Cup — Editions</title>
<style>
  :root{
    --bg:#0f1113; --card:#1a1d22; --card-2:#22262c; --muted:#9aa0a6;
    --gold:#e6b03a; --accent:#3dd4c9; --text:#e6e9ee;
  }
  *{box-sizing:border-box}
  body{
    margin:0;font-family:Arial,Helvetica,sans-serif;background:var(--bg);color:var(--text);
    -webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale;
  }

  header{display:flex;align-items:center;justify-content:space-between;padding:14px 16px;border-bottom:1px solid #121416}
  h1{margin:0;font-size:18px;color:var(--gold)}
  .controls{display:flex;gap:8px;align-items:center}
  .btn{background:#1f2328;border:0;padding:8px 12px;border-radius:8px;color:var(--text);cursor:pointer}
  .btn.ghost{background:transparent;border:1px solid #24272b}

  main{padding:14px;max-width:1100px;margin:0 auto}

/* ---------- Editions list ---------- */
.edition-card{background:var(--card);border-radius:12px;margin-bottom:14px;overflow:hidden;box-shadow:0 6px 22px rgba(0,0,0,.5)}
.edition-header{display:flex;align-items:center;justify-content:space-between;padding:10px 14px;cursor:pointer}
.edition-title{font-weight:700}
.edition-body{display:none;padding:12px;border-top:1px solid #151719}

/* small note */
.note{font-size:13px;color:var(--muted);text-align:center;margin:8px 0}

/* groups table */
.group-block{background:var(--card-2);padding:10px;border-radius:10px;margin-bottom:10px}
.group-title{color:var(--gold);font-weight:700;margin-bottom:6px}
.table{width:100%;border-collapse:collapse;font-size:13px}
.table th,.table td{padding:8px;border-bottom:1px solid #101214;color:var(--muted)}
.table th{color:var(--gold);text-align:left}

/* advancing table */
.advancing{background:transparent;margin:8px 0}

/* vertical bracket (small & mobile-friendly) */
.bracket-wrap{background:transparent;padding:8px;margin:8px 0;border-radius:8px}
.bracket-col{display:flex;flex-direction:column;gap:8px;align-items:center}
.box-small{width:86%;max-width:380px;background:#1f2530;padding:10px;border-radius:10px;box-shadow:0 6px 16px rgba(0,0,0,.6);text-align:center}
.bracket-label{color:var(--gold);margin-bottom:6px;font-weight:700;font-size:14px}
.team-line{display:flex;justify-content:space-between;align-items:center;font-weight:700;color:var(--text);font-size:14px}
.score{background:#15171a;padding:4px 8px;border-radius:8px;font-weight:800}
.connector{width:2px;height:18px;background:#2a2d32;margin:6px auto;border-radius:2px}

/* center winner display */
.center-winner{
  position:fixed;left:50%;transform:translateX(-50%);
  bottom:18px;z-index:80;background:linear-gradient(180deg,#23262a,#1a1d22);
  padding:12px 18px;border-radius:14px;box-shadow:0 12px 30px rgba(0,0,0,.6);
  text-align:center;min-width:220px;color:var(--text);
  display:none;
}
.center-winner .winner{font-size:18px;font-weight:900;color:var(--accent)}
.center-winner .second,.center-winner .third{font-size:14px;color:var(--muted);margin-top:4px}

/* Admin styles */
.admin-btn{position:fixed;right:14px;bottom:18px;background:#2b2f36;padding:10px 12px;border-radius:999px;border:0;color:var(--text);cursor:pointer;z-index:90}
.admin-login{display:none;position:fixed;left:0;top:0;width:100%;height:100%;background:rgba(0,0,0,.7);z-index:120}
.admin-box{background:#0f1113;padding:18px;border-radius:10px;width:320px;margin:80px auto;color:var(--text);border:1px solid #222}
.field{margin-bottom:8px}
.field label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
.field input, .field select, .field textarea{width:100%;padding:8px;border-radius:6px;background:#121417;border:1px solid #202427;color:var(--text)}

/* admin panel bottom drawer */
.admin-panel{display:none;position:fixed;left:0;right:0;bottom:0;background:#0e0f11;padding:12px;border-top-left-radius:12px;border-top-right-radius:12px;border:1px solid #15171a;z-index:110;max-height:60vh;overflow:auto}

/* responsive */
@media(min-width:900px){
  .box-small{width:380px}
  .center-winner{bottom:28px}
}
@media(max-width:520px){
  .edition-header{padding:12px}
  .box-small{width:92%}
  .admin-box{width:92%;margin:40px auto}
}
</style>
</head>
<body>

<header>
  <h1>eFootball Cup</h1>
  <div class="controls">
    <div style="font-size:13px;color:var(--muted);">Prizes: 1st 50 MAD · 2nd 30 MAD · 3rd 20 MAD</div>
    <button class="btn" id="resetBtn" title="Reset all stored data">Reset Data</button>
  </div>
</header>

<main>

  <!-- ================== Edition 2 (Current) — visible by default, ZEROED ================== -->
  <div class="edition-card" id="edition2-card">
    <div class="edition-header" aria-expanded="true">
      <div class="edition-title">E-Football Cup Two (Current)</div>
      <div style="font-size:13px;color:var(--muted)">Visible by default — zeroed (Player 1..)</div>
    </div>

    <div class="edition-body" style="display:block" id="edition2-body">
      <!-- Groups -->
      <div id="ed2-groups"></div>

      <!-- Advancing -->
      <div class="advancing" id="ed2-adv"></div>

      <!-- Bracket vertical -->
      <div class="bracket-wrap" id="ed2-bracket"></div>
    </div>
  </div>

  <!-- ================== Edition 1 (Previous) — collapsible / accordion ================== -->
  <div class="edition-card" id="edition1-card">
    <div class="edition-header" id="ed1-toggle" role="button" aria-expanded="false">
      <div class="edition-title">▶ E-Football Cup One (Previous Edition)</div>
      <div style="font-size:13px;color:var(--muted)">Click to open / close</div>
    </div>

    <div class="edition-body" id="edition1-body">
      <!-- Groups (original data preserved) -->
      <div id="ed1-groups"></div>

      <!-- Advancing for edition1 -->
      <div class="advancing" id="ed1-adv"></div>

      <!-- Bracket vertical for edition1 -->
      <div class="bracket-wrap" id="ed1-bracket"></div>
    </div>
  </div>

  <div class="note">Tip: Use the Admin Panel (bottom-right) to edit teams & results — saved in your browser.</div>
</main>

<!-- Center winner display (shows Winner / Runner-up / Third) -->
<div class="center-winner" id="centerWinner">
  <div style="font-size:13px;color:var(--muted)">Tournament Results</div>
  <div class="winner" id="cw-winner">Winner: —</div>
  <div class="second" id="cw-second">Runner-up: —</div>
  <div class="third" id="cw-third">Third: —</div>
</div>

<!-- Admin login button -->
<button class="admin-btn" id="openAdmin">Admin</button>

<!-- Admin login modal -->
<div class="admin-login" id="adminLoginModal">
  <div class="admin-box">
    <h3 style="margin:0 0 8px 0">Admin Login</h3>
    <div class="field">
      <label>Password</label>
      <input type="password" id="adminPassInput" placeholder="Enter password">
    </div>
    <div style="display:flex;gap:8px;justify-content:flex-end">
      <button class="btn" id="adminLoginBtn">Login</button>
      <button class="btn ghost" id="adminCancelBtn">Cancel</button>
    </div>
    <div style="margin-top:8px;font-size:12px;color:var(--muted)">Password for demo: <strong>1234</strong></div>
  </div>
</div>

<!-- Admin panel drawer -->
<div class="admin-panel" id="adminPanel">
  <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
    <div style="font-weight:800">Admin Panel</div>
    <div>
      <button class="btn" id="saveAllBtn">Save</button>
      <button class="btn ghost" id="closeAdminPanel">Close</button>
    </div>
  </div>

  <!-- Tabs: choose edition -->
  <div style="display:flex;gap:8px;margin-bottom:10px">
    <button class="btn" id="tabEd2">Edit Cup Two</button>
    <button class="btn ghost" id="tabEd1">Edit Cup One</button>
  </div>

  <!-- Editor area -->
  <div id="adminEditor"></div>

  <div style="margin-top:12px;font-size:13px;color:var(--muted)">Saved locally in browser (localStorage). Use <strong>Reset Data</strong> to clear.</div>
</div>

<script>
/* ===================== Data model =====================
 We keep two editions objects and store them in localStorage keys:
  - efc_ed1  (previous full data as you provided)
  - efc_ed2  (new zeroed template)
 Each edition: { groups: {A:[{name,m,w,d,l,gf,ga},...], ...}, knockout: {qf:[{a,b,ascore,bscore},...], sf:[...], final:{a,b,ascore,bscore}}, results:{winner,second,third} }
======================================================*/

/* --- Default data for Edition 1 (your original full data) --- */
const defaultEd1 = {
  groups: {
    A:[
      {name:"Amine – Napoli", m:4,w:1,d:0,l:3,gf:6,ga:11},
      {name:"Fouad – Netherlands NT", m:4,w:1,d:1,l:2,gf:5,ga:10},
      {name:"Ayoub – Arsenal", m:4,w:1,d:0,l:3,gf:6,ga:15},
      {name:"Hamza – Borussia Dortmund", m:4,w:2,d:1,l:1,gf:10,ga:5},
      {name:"Samir – Paris Saint-Germain", m:4,w:4,d:0,l:0,gf:19,ga:2}
    ],
    B:[
      {name:"Oussama – Malaga", m:4,w:2,d:2,l:0,gf:14,ga:9},
      {name:"Soufiane (Barcelona)", m:4,w:2,d:2,l:0,gf:13,ga:7},
      {name:"Houssam – Algeria NT", m:4,w:0,d:2,l:2,gf:3,ga:6},
      {name:"Oussama – Manchester City", m:4,w:1,d:1,l:2,gf:6,ga:11},
      {name:"Mohamed – Manchester United", m:4,w:0,d:3,l:1,gf:6,ga:9}
    ],
    C:[
      {name:"Moad – São Paulo", m:4,w:2,d:1,l:1,gf:11,ga:8},
      {name:"Elhady – AC Milan", m:4,w:2,d:1,l:1,gf:14,ga:7},
      {name:"BenTaleb – Liverpool", m:4,w:4,d:0,l:0,gf:18,ga:4},
      {name:"Mehdi – Morocco NT", m:4,w:1,d:0,l:3,gf:5,ga:15},
      {name:"Dovix – Senegal NT", m:4,w:0,d:0,l:4,gf:0,ga:14}
    ],
    D:[
      {name:"Khalid – Chelsea", m:4,w:3,d:0,l:1,gf:16,ga:7},
      {name:"Touati (Japan NT)", m:4,w:4,d:0,l:0,gf:18,ga:1},
      {name:"Karim – Bayern Munich", m:4,w:1,d:0,l:3,gf:7,ga:10},
      {name:"Aizen – Inter Milan", m:4,w:0,d:0,l:4,gf:3,ga:19},
      {name:"Said – Real Madrid", m:4,w:2,d:0,l:2,gf:4,ga:14}
    ]
  },
  knockout: null,
  results: {winner:"Touati (Japan NT)", second:"Soufiane (Barcelona)", third:"BenTaleb – Liverpool"}
};

/* --- Default template for Edition 2 (zeroed) --- */
const defaultEd2 = {
  groups: {
    A:[
      {name:"Player 1", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 2", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 3", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 4", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 5", m:0,w:0,d:0,l:0,gf:0,ga:0}
    ],
    B:[
      {name:"Player 6", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 7", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 8", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 9", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 10", m:0,w:0,d:0,l:0,gf:0,ga:0}
    ],
    C:[
      {name:"Player 11", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 12", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 13", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 14", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 15", m:0,w:0,d:0,l:0,gf:0,ga:0}
    ],
    D:[
      {name:"Player 16", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 17", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 18", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 19", m:0,w:0,d:0,l:0,gf:0,ga:0},
      {name:"Player 20", m:0,w:0,d:0,l:0,gf:0,ga:0}
    ]
  },
  knockout: null,
  results: {winner:"", second:"", third:""}
};

/* ---------- storage helpers ---------- */
const STORAGE_ED1 = 'efc_ed1';
const STORAGE_ED2 = 'efc_ed2';

function loadOrInit(){
  const ed1 = JSON.parse(localStorage.getItem(STORAGE_ED1) || 'null') || defaultEd1;
  const ed2 = JSON.parse(localStorage.getItem(STORAGE_ED2) || 'null') || defaultEd2;
  localStorage.setItem(STORAGE_ED1, JSON.stringify(ed1));
  localStorage.setItem(STORAGE_ED2, JSON.stringify(ed2));
  return {ed1, ed2};
}

/* ---------- utility functions ---------- */
function calculatePoints(t){ return (t.w*3) + t.d; }
function sortGroup(arr){
  return [...arr].sort((a,b)=>{
    const pa = calculatePoints(a), pb = calculatePoints(b);
    if(pb!==pa) return pb-pa;
    const gda = a.gf - a.ga, gdb = b.gf - b.ga;
    if(gdb !== gda) return gdb - gda;
    return b.gf - a.gf;
  });
}

/* ---------- render groups ---------- */
function renderGroups(containerId, groupsObj){
  const container = document.getElementById(containerId);
  container.innerHTML = '';
  for(const g in groupsObj){
    const list = groupsObj[g];
    const sorted = sortGroup(list);
    const block = document.createElement('div');
    block.className = 'group-block';
    block.innerHTML = `<div class="group-title">Group ${g}</div>
      <table class="table" aria-label="Group ${g}">
        <thead><tr><th>#</th><th>Team</th><th>M</th><th>W</th><th>D</th><th>L</th><th>G</th><th>Pts</th></tr></thead>
        <tbody>
          ${sorted.map((t,i)=>`<tr>
            <td>${i+1}</td>
            <td>${t.name}</td>
            <td>${t.m}</td>
            <td>${t.w}</td>
            <td>${t.d}</td>
            <td>${t.l}</td>
            <td>${t.gf}:${t.ga}</td>
            <td>${calculatePoints(t)}</td>
          </tr>`).join('')}
        </tbody>
      </table>`;
    container.appendChild(block);
  }
}

/* ---------- get top2 per group ---------- */
function getTop2(groupsObj){
  const arr = [];
  for(const g in groupsObj){
    const sorted = sortGroup(groupsObj[g]);
    arr.push({group:g, first:sorted[0].name, second:sorted[1].name});
  }
  return arr;
}

/* ---------- build vertical bracket ---------- */
function buildBracket(containerEl, groupsObj, knockoutData){
  // compute advancing teams if groups finished
  const allPlayed = Object.values(groupsObj).every(list => list.every(t => t.m >= 4));
  const advancing = getTop2(groupsObj);

  // if knockoutData not provided, generate empty skeleton
  const qfs = knockoutData && knockoutData.qf ? knockoutData.qf : [
    {a: (advancing[0] ? advancing[0].first : ''), b: (advancing[1] ? advancing[1].second : ''), ascore:'', bscore:''},
    {a: (advancing[2] ? advancing[2].first : ''), b: (advancing[3] ? advancing[3].second : ''), ascore:'', bscore:''},
    {a: (advancing[1] ? advancing[1].first : ''), b: (advancing[0] ? advancing[0].second : ''), ascore:'', bscore:''},
    {a: (advancing[3] ? advancing[3].first : ''), b: (advancing[2] ? advancing[2].second : ''), ascore:'', bscore:''},
  ];
  const sf = knockoutData && knockoutData.sf ? knockoutData.sf : [
    {a:'', b:'', ascore:'', bscore:''},
    {a:'', b:'', ascore:'', bscore:''}
  ];
  const final = knockoutData && knockoutData.final ? knockoutData.final : {a:'', b:'', ascore:'', bscore:''};

  containerEl.innerHTML = '';

  // Quarter labels and boxes (stacked)
  const col = document.createElement('div');
  col.className = 'bracket-col';
  // QF label
  const qfLabel = document.createElement('div'); qfLabel.className='bracket-label'; qfLabel.innerText='Quarter-finals';
  col.appendChild(qfLabel);

  qfs.forEach((m, idx)=> {
    const box = document.createElement('div'); box.className='box-small';
    box.innerHTML = `<div class="team-line"><span>${m.a || 'TBD'}</span><span class="score">${m.ascore !== '' ? m.ascore : '-'}</span></div>
                     <div style="height:6px"></div>
                     <div class="team-line"><span>${m.b || 'TBD'}</span><span class="score">${m.bscore !== '' ? m.bscore : '-'}</span></div>`;
    col.appendChild(box);
    // connector down
    if(idx === 1){
      const conn = document.createElement('div'); conn.className='connector'; col.appendChild(conn);
    }
  });

  // Semi
  const sfLabel = document.createElement('div'); sfLabel.className='bracket-label'; sfLabel.innerText='Semi-finals';
  col.appendChild(sfLabel);
  sf.forEach((s, i)=>{
    const box = document.createElement('div'); box.className='box-small';
    box.innerHTML = `<div class="team-line"><span>${s.a || 'TBD'}</span><span class="score">${s.ascore !== '' ? s.ascore : '-'}</span></div>
                     <div style="height:6px"></div>
                     <div class="team-line"><span>${s.b || 'TBD'}</span><span class="score">${s.bscore !== '' ? s.bscore : '-'}</span></div>`;
    col.appendChild(box);
    if(i===0){
      const conn = document.createElement('div'); conn.className='connector'; col.appendChild(conn);
    }
  });

  // Final
  const finalLabel = document.createElement('div'); finalLabel.className='bracket-label'; finalLabel.innerText='Final';
  col.appendChild(finalLabel);
  const boxFinal = document.createElement('div'); boxFinal.className='box-small';
  boxFinal.innerHTML = `<div style="font-size:13px;color:var(--muted)">Match</div>
    <div style="height:8px"></div>
    <div class="team-line"><span>${final.a || 'TBD'}</span><span class="score">${final.ascore !== '' ? final.ascore : '-'}</span></div>
    <div style="height:6px"></div>
    <div class="team-line"><span>${final.b || 'TBD'}</span><span class="score">${final.bscore !== '' ? final.bscore : '-'}</span></div>`;
  col.appendChild(boxFinal);

  // place into container
  containerEl.appendChild(col);

  // return whether advancing visible
  return {allPlayed, qfs, sf, final};
}

/* ---------- render advancing table (top2) ---------- */
function renderAdvancing(containerEl, groupsObj){
  const adv = getTop2(groupsObj);
  containerEl.innerHTML = `<div style="background:var(--card-2);padding:10px;border-radius:8px">
    <div style="font-weight:800;color:var(--gold);margin-bottom:8px">Teams Qualified for Knockout Stage</div>
    <table class="table"><thead><tr><th>Group</th><th>1st</th><th>2nd</th></tr></thead>
    <tbody>${adv.map(a=>`<tr><td style="color:${'#'+Math.floor(Math.random()*0xFFFFFF).toString(16)}">${a.group}</td><td>${a.first}</td><td>${a.second}</td></tr>`).join('')}</tbody>
    </table></div>`;
}

/* ---------- display center winner overlay ---------- */
function showCenterWinner(results){
  const el = document.getElementById('centerWinner');
  if(results && results.winner){
    document.getElementById('cw-winner').innerText = `Winner: ${results.winner}`;
    document.getElementById('cw-second').innerText = `Runner-up: ${results.second || '-'}`;
    document.getElementById('cw-third').innerText = `Third: ${results.third || '-'}`;
    el.style.display = 'block';
  } else {
    el.style.display = 'none';
  }
}

/* ---------- main render for both editions ---------- */
function renderAll(){
  const store = loadOrInit();
  // Edition 2
  renderGroups('ed2-groups', store.ed2.groups);
  renderAdvancing(document.getElementById('ed2-adv'), store.ed2.groups);
  buildBracket(document.getElementById('ed2-bracket'), store.ed2.groups, store.ed2.knockout);

  // Edition 1
  renderGroups('ed1-groups', store.ed1.groups);
  renderAdvancing(document.getElementById('ed1-adv'), store.ed1.groups);
  const br = buildBracket(document.getElementById('ed1-bracket'), store.ed1.groups, store.ed1.knockout);

  // show center winner for edition1 if data present
  if(store.ed1.results && store.ed1.results.winner){
    showCenterWinner(store.ed1.results);
  } else {
    showCenterWinner(null);
  }
}

/* ---------- Admin functionality ---------- */
let currentEdit = 'ed2'; // default tab in admin

function openAdminModal(){ document.getElementById('adminLoginModal').style.display='block'; }
function closeAdminModal(){ document.getElementById('adminLoginModal').style.display='none'; }

document.getElementById('openAdmin').addEventListener('click', openAdminModal);
document.getElementById('adminCancelBtn').addEventListener('click', closeAdminModal);
document.getElementById('adminLoginBtn').addEventListener('click', ()=>{
  const pass = document.getElementById('adminPassInput').value;
  if(pass === '1234'){
    closeAdminModal();
    openAdminPanel();
    loadAdminEditor(); // show ed2 editor by default
  } else {
    alert('Incorrect password');
  }
});

function openAdminPanel(){
  document.getElementById('adminPanel').style.display='block';
}
function closeAdminPanel(){ document.getElementById('adminPanel').style.display='none'; }

document.getElementById('closeAdminPanel').addEventListener('click', closeAdminPanel);
document.getElementById('tabEd2').addEventListener('click', ()=>{ currentEdit='ed2'; loadAdminEditor(); });
document.getElementById('tabEd1').addEventListener('click', ()=>{ currentEdit='ed1'; loadAdminEditor(); });

function loadAdminEditor(){
  const store = loadOrInit();
  const editor = document.getElementById('adminEditor');
  editor.innerHTML = '';
  const data = currentEdit === 'ed2' ? store.ed2 : store.ed1;

  // groups editor
  const groupsKeys = Object.keys(data.groups);
  const groupsHtml = groupsKeys.map(g=>{
    const teams = data.groups[g].map((t, idx)=>`
      <div style="display:flex;gap:8px;margin-bottom:6px;align-items:center">
        <input data-group="${g}" data-idx="${idx}" class="team-name" value="${t.name}">
        <input data-group="${g}" data-idx="${idx}" class="team-m" style="width:64px" value="${t.m}" placeholder="M">
        <input data-group="${g}" data-idx="${idx}" class="team-gf" style="width:64px" value="${t.gf}" placeholder="GF">
        <input data-group="${g}" data-idx="${idx}" class="team-ga" style="width:64px" value="${t.ga}" placeholder="GA">
      </div>`).join('');
    return `<div style="margin-bottom:10px"><div style="font-weight:800;color:var(--gold)">Group ${g}</div>${teams}</div>`;
  }).join('');

  // knockout editor (simple fields)
  const knockoutHtml = `
    <div style="margin-top:8px"><div style="font-weight:800;color:var(--gold)">Knockout Manual (optional)</div>
    <div style="font-size:12px;color:var(--muted);margin-bottom:6px">Fill QF/SF/Final names & scores to override automatic pairing</div>
    <div id="ko-editor"></div>
    </div>`;

  // results editor (winner/2/3)
  const resultsHtml = `<div style="margin-top:8px"><div style="font-weight:800;color:var(--gold)">Top 3 Results</div>
    <div class="field"><label>Winner</label><input id="res-winner" value="${data.results.winner || ''}"></div>
    <div class="field"><label>Runner-up</label><input id="res-second" value="${data.results.second || ''}"></div>
    <div class="field"><label>Third place</label><input id="res-third" value="${data.results.third || ''}"></div>
    <div style="font-size:13px;color:var(--muted)">Press Save to apply changes.</div>
  </div>`;

  editor.innerHTML = groupsHtml + knockoutHtml + resultsHtml;

  // populate knockout editor if exists
  const koEditor = document.getElementById('ko-editor');
  const ko = data.knockout || {qf:[{a:'',b:'',ascore:'',bscore:''},{a:'',b:'',ascore:'',bscore:''},{a:'',b:'',ascore:'',bscore:''},{a:'',b:'',ascore:'',bscore:''}], sf:[{a:'',b:'',ascore:'',bscore:''},{a:'',b:'',ascore:'',bscore:''}], final:{a:'',b:'',ascore:'',bscore:''}};
  let koHtml = '<div style="font-weight:700;margin-bottom:6px">Quarter Finals</div>';
  ko.qf.forEach((m,i)=>{
    koHtml += `<div style="display:flex;gap:6px;margin-bottom:6px;align-items:center">
      <input data-ko="qf" data-idx="${i}" class="ko-a" style="flex:1" value="${m.a}">
      <input data-ko="qf" data-idx="${i}" class="ko-as" style="width:56px" value="${m.ascore}" placeholder="AS">
      <span style="width:12px;text-align:center">:</span>
      <input data-ko="qf" data-idx="${i}" class="ko-bs" style="width:56px" value="${m.bscore}" placeholder="BS">
      <input data-ko="qf" data-idx="${i}" class="ko-b" style="flex:1" value="${m.b}">
    </div>`;
  });
  koHtml += '<div style="font-weight:700;margin-top:8px;margin-bottom:6px">Semi Finals</div>';
  ko.sf.forEach((m,i)=>{
    koHtml += `<div style="display:flex;gap:6px;margin-bottom:6px;align-items:center">
      <input data-ko="sf" data-idx="${i}" class="ko-a" style="flex:1" value="${m.a}">
      <input data-ko="sf" data-idx="${i}" class="ko-as" style="width:56px" value="${m.ascore}">
      <span style="width:12px;text-align:center">:</span>
      <input data-ko="sf" data-idx="${i}" class="ko-bs" style="width:56px" value="${m.bscore}">
      <input data-ko="sf" data-idx="${i}" class="ko-b" style="flex:1" value="${m.b}">
    </div>`;
  });
  koHtml += '<div style="font-weight:700;margin-top:8px;margin-bottom:6px">Final</div>';
  koHtml += `<div style="display:flex;gap:6px;align-items:center">
      <input data-ko="final" data-idx="0" class="ko-a" style="flex:1" value="${ko.final.a}">
      <input data-ko="final" data-idx="0" class="ko-as" style="width:56px" value="${ko.final.ascore}">
      <span style="width:12px;text-align:center">:</span>
      <input data-ko="final" data-idx="0" class="ko-bs" style="width:56px" value="${ko.final.bscore}">
      <input data-ko="final" data-idx="0" class="ko-b" style="flex:1" value="${ko.final.b}">
    </div>`;
  koEditor.innerHTML = koHtml;
}

/* Save admin edits */
document.getElementById('saveAllBtn').addEventListener('click', ()=>{
  const store = loadOrInit();
  const target = currentEdit === 'ed2' ? store.ed2 : store.ed1;

  // collect team name fields
  const nameInputs = document.querySelectorAll('.team-name');
  nameInputs.forEach(inp=>{
    const g = inp.dataset.group, idx = parseInt(inp.dataset.idx,10);
    target.groups[g][idx].name = inp.value.trim() || target.groups[g][idx].name;
  });
  // collect m/gf/ga
  document.querySelectorAll('.team-m').forEach(inp=>{
    const g = inp.dataset.group, idx = parseInt(inp.dataset.idx,10);
    target.groups[g][idx].m = Math.max(0, parseInt(inp.value||0,10));
  });
  document.querySelectorAll('.team-gf').forEach(inp=>{
    const g = inp.dataset.group, idx = parseInt(inp.dataset.idx,10);
    target.groups[g][idx].gf = Math.max(0, parseInt(inp.value||0,10));
  });
  document.querySelectorAll('.team-ga').forEach(inp=>{
    const g = inp.dataset.group, idx = parseInt(inp.dataset.idx,10);
    target.groups[g][idx].ga = Math.max(0, parseInt(inp.value||0,10));
  });

  // collect knockout fields
  const qf = [];
  document.querySelectorAll('input[data-ko="qf"].ko-a').forEach((el,i)=>{
    const a = el.value || '';
    const as = document.querySelectorAll('input[data-ko="qf"].ko-as')[i].value || '';
    const bs = document.querySelectorAll('input[data-ko="qf"].ko-bs')[i].value || '';
    const b = document.querySelectorAll('input[data-ko="qf"].ko-b')[i].value || '';
    qf.push({a:a,b:b,ascore:as,bscore:bs});
  });
  const sf = [];
  document.querySelectorAll('input[data-ko="sf"].ko-a').forEach((el,i)=>{
    const a = el.value || '';
    const as = document.querySelectorAll('input[data-ko="sf"].ko-as')[i].value || '';
    const bs = document.querySelectorAll('input[data-ko="sf"].ko-bs')[i].value || '';
    const b = document.querySelectorAll('input[data-ko="sf"].ko-b')[i].value || '';
    sf.push({a:a,b:b,ascore:as,bscore:bs});
  });
  const finalA = document.querySelector('input[data-ko="final"].ko-a').value || '';
  const finalAS = document.querySelector('input[data-ko="final"].ko-as').value || '';
  const finalBS = document.querySelector('input[data-ko="final"].ko-bs').value || '';
  const finalB = document.querySelector('input[data-ko="final"].ko-b').value || '';
  target.knockout = {qf, sf, final:{a:finalA,b:finalB,ascore:finalAS,bscore:finalBS}};

  // collect results
  target.results.winner = document.getElementById('res-winner').value || '';
  target.results.second = document.getElementById('res-second').value || '';
  target.results.third = document.getElementById('res-third').value || '';

  // save
  localStorage.setItem(STORAGE_ED1, JSON.stringify(store.ed1));
  localStorage.setItem(STORAGE_ED2, JSON.stringify(store.ed2));

  // re-render everything
  renderAll();
  alert('Saved locally.');
});

/* reset button */
document.getElementById('resetBtn').addEventListener('click', ()=>{
  if(confirm('Reset all tournament data to defaults? This will clear your local edits.')){
    localStorage.removeItem(STORAGE_ED1);
    localStorage.removeItem(STORAGE_ED2);
    renderAll();
    alert('Data reset.');
  }
});

/* edit toggle for edition1 */
document.getElementById('ed1-toggle').addEventListener('click', ()=>{
  const body = document.getElementById('edition1-body');
  const head = document.getElementById('ed1-toggle').querySelector('.edition-title');
  if(body.style.display === 'block'){
    body.style.display='none';
    head.innerText = '▶ E-Football Cup One (Previous Edition)';
    // hide center winner when closed
    document.getElementById('centerWinner').style.display='none';
  } else {
    body.style.display='block';
    head.innerText = '▼ E-Football Cup One (Previous Edition)';
    // show winner if exists
    const store = loadOrInit();
    if(store.ed1.results && store.ed1.results.winner) showCenterWinner(store.ed1.results);
  }
});

/* close admin panel automatically when clicking outside (simple) */
window.addEventListener('click', (e)=>{
  const loginModal = document.getElementById('adminLoginModal');
  if(e.target === loginModal) loginModal.style.display='none';
});

/* init render */
renderAll();
</script>
</body>
</html>


