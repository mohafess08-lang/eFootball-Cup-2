<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>eFootball Cup — Editions</title>
<style>
  :root{
    --bg:#0f1113; --card:#1f2430; --card-2:#262a33; --muted:#9aa0a6;
    --gold:#e6b03a; --accent:#3dd4c9; --text:#e6e9ee;
  }
  *{box-sizing:border-box}
  body{
    margin:0; font-family:Arial, sans-serif; background:var(--bg); color:var(--text);
    -webkit-font-smoothing:antialiased; -moz-osx-font-smoothing:grayscale;
  }
  header{display:flex;align-items:center;justify-content:space-between;padding:18px 6%;gap:12px}
  h1{margin:0;color:var(--gold);font-size:20px}
  .controls{display:flex;gap:10px;align-items:center}
  .btn{
    background:linear-gradient(180deg,#2a2f38,#21252d);border:0;padding:10px 14px;border-radius:10px;color:var(--text);
    cursor:pointer; box-shadow:0 6px 18px rgba(0,0,0,.6); font-weight:600;
  }
  .btn.secondary{background:transparent;border:1px solid #2b3138;color:var(--muted)}
  main{padding:0 4% 60px 4%}

/* ---------- shared layout (groups, matches) ---------- */
.table-container { width:100%; max-width:1200px; margin: 0 auto 20px; }
.group-title { color:var(--gold); font-size:18px; margin: 20px 0 8px 0; text-align:left; }
.table { width:100%; border-collapse: collapse; margin-bottom: 14px; background:transparent; }
.table th, .table td { padding:10px 8px; border-bottom:1px solid #20252b; color:var(--muted); }
.table th { color:var(--gold); text-align:left; }
.team { text-align:left; color:var(--text) }
.matches-panel{display:flex;flex-wrap:wrap;gap:16px;justify-content:space-between;margin-bottom:20px}
.panel{background:var(--card);padding:12px;border-radius:12px;flex:1;min-width:260px;}

/* editable style */
.editable{outline:none;border:none;background:transparent;color:var(--text);font-weight:600}
.match-check{transform:scale(1.15);margin-right:8px}

/* advancing table */
.advancing-teams{max-width:900px;margin:18px auto;padding:12px;border-radius:12px;background:var(--card-2)}
.adv-table{width:100%;border-collapse:collapse;color:var(--muted)}
.adv-table th, .adv-table td{padding:10px;border-bottom:1px solid #20252b}
.adv-table td span.editable{font-weight:700;color:var(--text)}

/* ---------- Bracket (knockout) style similar to image ---------- */
.bracket-wrap{max-width:900px;margin:28px auto;padding:18px;border-radius:12px;background:transparent}
.bracket{display:flex;align-items:flex-start;justify-content:space-between;gap:10px;position:relative}
/* columns: quarters | semis | final | semis (other side) | quarters other side */
.col{display:flex;flex-direction:column;gap:26px;align-items:center;flex:1;min-width:120px}
.col .box{width:180px;background:rgba(34,39,47,.9);padding:14px;border-radius:12px;box-shadow:0 8px 24px rgba(0,0,0,.6);text-align:center;color:var(--muted)}
.col .box .team{display:flex;justify-content:space-between;align-items:center;color:var(--text);font-weight:700}
.col .label{color:var(--gold);margin-bottom:6px;font-size:14px}

/* connectors */
.bracket::before{content:"";position:absolute;left:33%;top:40px;bottom:40px;width:2px;background:#222;transform:translateX(-50%)}
.bracket::after{content:"";position:absolute;right:33%;top:40px;bottom:40px;width:2px;background:#222;transform:translateX(50%)}

/* For nice horizontal connectors between boxes, use pseudo elements on boxes */
.box.connector-right::after{
  content:"";position:absolute;height:2px;width:40px;background:#2b2f36;right:-40px;top:50%;transform:translateY(-50%);border-radius:2px;
}
.box.connector-left::before{
  content:"";position:absolute;height:2px;width:40px;background:#2b2f36;left:-40px;top:50%;transform:translateY(-50%);border-radius:2px;
}

/* to allow absolute pseudo elements, make box relative */
.box{position:relative}

/* small responsive */
@media (max-width:880px){
  .bracket{flex-direction:column;align-items:center}
  .col{width:100%;min-width:unset}
  .bracket::before, .bracket::after{display:none}
  .box.connector-right::after, .box.connector-left::before{display:none}
}

/* small polish */
.note{color:var(--muted);text-align:center;margin-top:8px;font-size:13px}
.footer {text-align:center;color:var(--muted);margin-top:22px;font-size:13px}
</style>
</head>
<body>

<header>
  <h1>eFootball Cup</h1>
  <div class="controls">
    <button id="togglePrev" class="btn">Show Previous Edition</button>
    <button id="downloadBtn" class="btn secondary" title="Copy HTML to clipboard (quick)">Copy HTML</button>
  </div>
</header>

<main>
  <!-- ================== Edition 2 (Current) ================== -->
  <section id="edition-2" class="edition">
    <h2 style="color:var(--accent);margin:6px 0 12px 0">Edition 2 — Current</h2>

    <!-- Groups display (reuse your dynamic display area) -->
    <div class="table-container" id="tables-ed2"></div>

    <!-- Matches panel -->
    <div class="matches-panel" id="matches-panel-ed2">
      <div class="panel">
        <h3 style="margin:0 0 8px 0;color:var(--gold)">Group A</h3>
        <ul style="list-style:none;padding:0;margin:0">
          <li><input type="checkbox" class="match-check" disabled checked> <span class="editable">Amine (Napoli)</span> <span style="color:var(--gold)"> (2:3) </span> <span class="editable">Fouad (Netherlands NT)</span></li>
          <li><input type="checkbox" class="match-check" disabled checked> <span class="editable">Amine (Napoli)</span> <span style="color:var(--gold)"> (4:2) </span> <span class="editable">Ayoub (Arsenal)</span></li>
          <!-- ... keep rest as in your original; shortened here for readability -->
        </ul>
      </div>

      <div class="panel">
        <h3 style="margin:0 0 8px 0;color:var(--gold)">Group B</h3>
        <ul style="list-style:none;padding:0;margin:0">
          <li><input type="checkbox" class="match-check" disabled checked> <span class="editable">Oussama (Malaga)</span> <span style="color:var(--gold)">(3:3)</span> <span class="editable">Soufiane</span></li>
          <!-- ... -->
        </ul>
      </div>

      <div class="panel">
        <h3 style="margin:0 0 8px 0;color:var(--gold)">Group C</h3>
        <ul style="list-style:none;padding:0;margin:0">
          <li><input type="checkbox" class="match-check" disabled checked> <span class="editable">Moad (São Paulo)</span> <span style="color:var(--gold)">(3:3)</span> <span class="editable">Elhady</span></li>
        </ul>
      </div>

      <div class="panel">
        <h3 style="margin:0 0 8px 0;color:var(--gold)">Group D</h3>
        <ul style="list-style:none;padding:0;margin:0">
          <li><input type="checkbox" class="match-check" disabled checked> <span class="editable">Khalid (Chelsea)</span> <span style="color:var(--gold)">(1:4)</span> <span class="editable">Touati</span></li>
        </ul>
      </div>
    </div>

    <!-- Advancing teams table -->
    <div class="advancing-teams">
      <h3 style="margin:0 0 8px 0;color:var(--gold)">Teams Qualified for Knockout Stage</h3>
      <table class="adv-table">
        <thead>
          <tr><th>Group</th><th>1st Place</th><th>2nd Place</th></tr>
        </thead>
        <tbody>
          <tr><td>A</td><td><span contenteditable="true" class="editable">Samir (PSG)</span></td><td><span contenteditable="true" class="editable">Hamza (Dortmund)</span></td></tr>
          <tr><td>B</td><td><span contenteditable="true" class="editable">Soufiane (Barcelona)</span></td><td><span contenteditable="true" class="editable">Oussama (Malaga)</span></td></tr>
          <tr><td>C</td><td><span contenteditable="true" class="editable">BenTaleb (Liverpool)</span></td><td><span contenteditable="true" class="editable">Elhady (AC Milan)</span></td></tr>
          <tr><td>D</td><td><span contenteditable="true" class="editable">Touati (Japan NT)</span><td><span contenteditable="true" class="editable">Khalid (Chelsea)</span></td></tr>
        </tbody>
      </table>
    </div>

    <!-- Bracket (knockout) for Edition 2 -->
    <div class="bracket-wrap" aria-label="Knockout Bracket">
      <div style="text-align:center;color:var(--gold);margin-bottom:8px">Quarter Final → Semi Final → Final</div>
      <div class="bracket">
        <!-- Left Quarter Finals -->
        <div class="col">
          <div class="label">Quarter Final</div>
          <div class="box connector-right">
            <div class="team"><span contenteditable="true">Samir (PSG)</span> <span>-</span></div>
          </div>
          <div class="box connector-right">
            <div class="team"><span contenteditable="true">Soufiane (Barcelona)</span> <span>-</span></div>
          </div>
        </div>

        <!-- Left Semi -->
        <div class="col">
          <div class="label">Semi Final</div>
          <div class="box connector-right">
            <div class="team"><span contenteditable="true">Winner QF1</span> <span>-</span></div>
          </div>
        </div>

        <!-- Final -->
        <div class="col" style="flex:1.2;align-items:center">
          <div class="label" style="color:var(--accent)">Final</div>
          <div class="box" style="width:260px;padding:18px;background:linear-gradient(180deg,#23272f,#1f2227);">
            <div style="font-size:14px;color:var(--muted)">2025-12-18</div>
            <div style="margin:10px 0;font-size:18px;font-weight:800;color:var(--text)"><span contenteditable="true">Touati (Japan NT)</span> <span>vs</span> <span contenteditable="true">Soufiane (Barcelona)</span></div>
            <div style="background:#111;padding:6px 10px;border-radius:999px;display:inline-block;color:var(--text);font-weight:700">05:00 PM</div>
          </div>
        </div>

        <!-- Right Semi -->
        <div class="col">
          <div class="label">Semi Final</div>
          <div class="box connector-left">
            <div class="team"><span contenteditable="true">Winner QF3</span> <span>-</span></div>
          </div>
        </div>

        <!-- Right Quarter Finals -->
        <div class="col">
          <div class="label">Quarter Final</div>
          <div class="box connector-left">
            <div class="team"><span contenteditable="true">BenTaleb (Liverpool)</span> <span>-</span></div>
          </div>
          <div class="box connector-left">
            <div class="team"><span contenteditable="true">Touati (Japan NT)</span> <span>-</span></div>
          </div>
        </div>
      </div> <!-- end bracket -->
      <div class="note">Click any team name to edit. Use the advancing table to keep values synchronized.</div>
    </div>
  </section>

  <!-- ================== Previous Edition (Edition 1) — hidden by default ================== -->
  <section id="edition-1" class="edition" style="display:none;margin-top:18px">
    <h2 style="color:var(--gold);margin-bottom:8px">Previous Edition — Edition 1</h2>

    <!-- Here we paste your full previous edition HTML content.
         For demo I include the original knockout & group summary from your provided code.
         You can replace any content below with the exact HTML you have for edition 1. -->
    <div class="table-container" id="tables-ed1">
      <!-- dynamic groups (could be same structure as edition 2 or different) -->
    </div>

    <div style="max-width:900px;margin:10px auto;background:var(--card);padding:14px;border-radius:12px">
      <h3 style="color:var(--gold);margin-top:0">Edition 1 — Knockout (original)</h3>
      <!-- Recreate the same bracket layout but with data from edition 1 -->
      <div class="bracket-wrap">
        <div class="bracket">
          <div class="col">
            <div class="label">Quarter Final</div>
            <div class="box connector-right"><div class="team"><span contenteditable="true">Palestine</span> <span>-</span></div></div>
            <div class="box connector-right"><div class="team"><span contenteditable="true">Saudi Arabia</span> <span>-</span></div></div>
          </div>
          <div class="col">
            <div class="label">Semi Final</div>
            <div class="box connector-right"><div class="team"><span contenteditable="true">Winner QF (left)</span></div></div>
          </div>
          <div class="col" style="flex:1.2;align-items:center">
            <div class="label" style="color:var(--accent)">Final</div>
            <div class="box" style="width:260px;padding:18px;">
              <div style="font-size:14px;color:var(--muted)">2025-11-12</div>
              <div style="margin:10px 0;font-size:18px;color:var(--text)"><span contenteditable="true">Team A</span> <span>vs</span> <span contenteditable="true">Team B</span></div>
              <div style="background:#111;padding:6px 10px;border-radius:999px;display:inline-block;color:var(--text)">06:00 PM</div>
            </div>
          </div>
          <div class="col">
            <div class="label">Semi Final</div>
            <div class="box connector-left"><div class="team"><span contenteditable="true">Winner QF (right)</span></div></div>
          </div>
          <div class="col">
            <div class="label">Quarter Final</div>
            <div class="box connector-left"><div class="team"><span contenteditable="true">Jordan</span></div></div>
            <div class="box connector-left"><div class="team"><span contenteditable="true">Iraq</span></div></div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <div class="footer">Built for quick GitHub Pages deployment — edit teams & results directly on the page.</div>
</main>

<script>
/* ----------------- Data & dynamic group rendering (kept from your original logic) ----------------- */
const groups_ed2 = {
  A: [
    { name: "Amine – Napoli", m:4,w:1,d:0,l:3,gf:6,ga:11 },
    { name: "Fouad – Netherlands NT", m:4,w:1,d:1,l:2,gf:5,ga:10 },
    { name: "Ayoub – Arsenal", m:4,w:1,d:0,l:3,gf:6,ga:15},
    { name: "Hamza – Borussia Dortmund", m:4,w:2,d:1,l:1,gf:10,ga:5 },
    { name: "Samir – Paris Saint-Germain", m:4,w:4,d:0,l:0,gf:19,ga:2 }
  ],
  B: [
    { name: "Oussama – Malaga", m:4,w:2,d:2,l:0,gf:14,ga:9 },
    { name: "Soufiane (Barcelona)", m:4,w:2,d:2,l:0,gf:13,ga:7 },
    { name: "Houssam – Algeria NT", m:4,w:0,d:2,l:2,gf:3,ga:6 },
    { name: "Oussama – Manchester City", m:4,w:1,d:1,l:2,gf:6,ga:11 },
    { name: "Mohamed – Manchester United", m:4,w:0,d:3,l:1,gf:6,ga:9 }
  ],
  C: [
    { name: "Moad – São Paulo", m:4,w:2,d:1,l:1,gf:11,ga:8 },
    { name: "Elhady – AC Milan", m:4,w:2,d:1,l:1,gf:14,ga:7 },
    { name: "BenTaleb – Liverpool", m:4,w:4,d:0,l:0,gf:18,ga:4 },
    { name: "Mehdi – Morocco NT", m:4,w:1,d:0,l:3,gf:5,ga:15 },
    { name: "Dovix – Senegal NT", m:4,w:0,d:0,l:4,gf:0,ga:14 }
  ],
  D: [
    { name: "Khalid – Chelsea", m:4,w:3,d:0,l:1,gf:16,ga:7 },
    { name: "Touati (Japan NT)", m:4,w:4,d:0,l:0,gf:18,ga:1 },
    { name: "Karim – Bayern Munich", m:4,w:1,d:0,l:3,gf:7,ga:10 },
    { name: "Aizen – Inter Milan", m:4,w:0,d:0,l:4,gf:3,ga:19 },
    { name: "Said – Real Madrid", m:4,w:2,d:0,l:2,gf:4,ga:14 }
  ],
};

function calculatePoints(team){ return team.w*3 + team.d; }

function renderGroups(containerId, groupsObj){
  const container = document.getElementById(containerId);
  container.innerHTML = '';
  for(let g in groupsObj){
    let html = `<div class="group-title">Group ${g}</div><table class="table"><tr><th>#</th><th>Team</th><th>M</th><th>W</th><th>D</th><th>L</th><th>Goals</th><th>Points</th></tr>`;
    let sorted = [...groupsObj[g]].sort((a,b)=>{
      let ptsB = calculatePoints(b), ptsA = calculatePoints(a);
      if(ptsB!==ptsA) return ptsB-ptsA;
      let gdB = b.gf-b.ga, gdA = a.gf-a.ga;
      if(gdB!==gdA) return gdB-gdA;
      return b.gf - a.gf;
    });
    sorted.forEach((t,i)=>{
      html += `<tr><td>${i+1}</td><td class="team">${t.name}</td><td>${t.m}</td><td>${t.w}</td><td>${t.d}</td><td>${t.l}</td><td>${t.gf}:${t.ga}</td><td>${calculatePoints(t)}</td></tr>`;
    });
    html += `</table>`;
    container.innerHTML += html;
  }
}

/* init render for edition 2 groups */
renderGroups('tables-ed2', groups_ed2);

/* Previous edition could reuse same data or different; for demo we call same */
renderGroups('tables-ed1', groups_ed2);

/* ---------------- Toggle previous edition ---------------- */
const toggleBtn = document.getElementById('togglePrev');
const edition1 = document.getElementById('edition-1');
toggleBtn.addEventListener('click', ()=>{
  if(edition1.style.display === 'none' || edition1.style.display === ''){
    edition1.style.display = 'block';
    toggleBtn.innerText = 'Hide Previous Edition';
    edition1.scrollIntoView({behavior:'smooth',block:'center'});
  } else {
    edition1.style.display = 'none';
    toggleBtn.innerText = 'Show Previous Edition';
    window.scrollTo({top:0,behavior:'smooth'});
  }
});

/* ---------- copy HTML quick helper (optional) ---------- */
document.getElementById('downloadBtn').addEventListener('click', async ()=>{
  try{
    const html = document.documentElement.outerHTML;
    await navigator.clipboard.writeText(html);
    alert('HTML copied to clipboard — paste into your index.html and push to GitHub.');
  }catch(e){
    alert('Unable to copy automatically. You can save the page source manually.');
  }
});

/* ----------------- Note for future editions -----------------
To add more editions you can:
- create <section id="edition-3" class="edition" style="display:none"> ... </section>
- add corresponding toggle controls and show/hide logic
- or generate dynamically from an array of editions.
--------------------------------------------------------------------------------- */
</script>
</body>
</html>
