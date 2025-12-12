<!doctype html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>لوحة المشرف - بطولة E-Football (Admin Demo)</title>
  <style>
    :root{
      --bg:#0f1724;
      --card:#0b1220;
      --muted:#9aa4b2;
      --accent:#06b6d4;
      --glass: rgba(255,255,255,0.03);
    }
    *{box-sizing:border-box;font-family:Inter, "Noto Sans Arabic", system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;}
    html,body{height:100%;margin:0;background:linear-gradient(180deg,#071024 0%,var(--bg) 100%);color:#e6eef6}
    .wrap{max-width:1100px;margin:28px auto;padding:20px;}
    header{display:flex;gap:16px;align-items:center;margin-bottom:18px}
    header .brand{display:flex;flex-direction:column}
    h1{margin:0;font-size:20px}
    p.lead{margin:0;color:var(--muted);font-size:13px}

    .grid{display:grid;grid-template-columns:1fr 420px;gap:18px}
    .panel{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));padding:16px;border-radius:12px;box-shadow:0 6px 18px rgba(2,6,23,0.6)}
    .panel h2{margin:0 0 12px 0;font-size:16px}
    .small{font-size:13px;color:var(--muted)}

    /* Left column - Admin form + participants list */
    .form-row{display:flex;gap:8px;margin-bottom:10px;align-items:center}
    label{font-size:13px;color:var(--muted);min-width:92px}
    input[type="text"]{flex:1;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:inherit}
    button{background:var(--accent);border:none;color:#042027;padding:10px 12px;border-radius:8px;cursor:pointer}
    button.ghost{background:transparent;border:1px solid rgba(255,255,255,0.06);color:var(--muted)}
    .muted{color:var(--muted);font-size:13px;margin-top:6px}

    /* Logo selector modal-like area */
    .logos-grid{display:grid;grid-template-columns:repeat(6,1fr);gap:8px;max-height:320px;overflow:auto;padding:6px;background:var(--glass);border-radius:8px}
    .logo-item{display:flex;flex-direction:column;align-items:center;gap:8px;padding:8px;border-radius:8px;cursor:pointer;border:1px dashed transparent}
    .logo-item.selected{outline:3px solid rgba(6,182,212,0.12);border-color:rgba(6,182,212,0.25);background:linear-gradient(180deg, rgba(6,182,212,0.02), transparent)}
    .logo-thumb{width:64px;height:64px;border-radius:8px;display:flex;align-items:center;justify-content:center;font-weight:700}
    .logo-name{font-size:11px;text-align:center;color:var(--muted);max-width:80px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap}

    /* Participants list */
    .participants{display:grid;gap:8px;max-height:420px;overflow:auto;padding-top:6px}
    .participant{display:flex;align-items:center;gap:10px;padding:8px;border-radius:10px;background:rgba(255,255,255,0.02);border:1px solid rgba(255,255,255,0.02)}
    .p-thumb{width:48px;height:48px;border-radius:8px;display:flex;align-items:center;justify-content:center;font-weight:700}
    .p-name{font-size:14px}
    .trash{margin-left:auto;background:transparent;border:0;color:#ffb4b4;cursor:pointer}

    /* Right column - preview + actions */
    .preview{display:flex;flex-direction:column;gap:12px}
    .preview-box{background:linear-gradient(180deg, rgba(255,255,255,0.02), transparent);padding:12px;border-radius:10px}
    .small-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:8px}
    footer{margin-top:18px;text-align:center;color:var(--muted);font-size:13px}
    a.link{color:var(--accent);text-decoration:none}
    /* scrollbar small */
    .logos-grid::-webkit-scrollbar, .participants::-webkit-scrollbar{height:8px;width:8px}
    .logos-grid::-webkit-scrollbar-thumb, .participants::-webkit-scrollbar-thumb{background:rgba(255,255,255,0.04);border-radius:8px}
    @media(max-width:980px){
      .grid{grid-template-columns:1fr}
      .logos-grid{grid-template-columns:repeat(5,1fr)}
    }
    @media(max-width:520px){
      .logos-grid{grid-template-columns:repeat(4,1fr)}
    }
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div style="width:56px;height:56px;border-radius:10px;background:linear-gradient(135deg,var(--accent),#7c3aed);display:flex;align-items:center;justify-content:center;font-weight:800">EF</div>
      <div class="brand">
        <h1>لوحة المشرف — تجريبي (Admin Panel)</h1>
        <p class="lead">أضف المتبارين، اختر شعار الفريق من شبكة الـ32 شعارًا أو ارفع شعارًا جديدًا — التجربة تعمل على GitHub Pages</p>
      </div>
    </header>

    <div class="grid">
      <div class="panel">
        <h2>إضافة متبارٍ جديد</h2>

        <div class="form-row">
          <label>اسم المتبارٍ</label>
          <input id="participantName" type="text" placeholder="مثال: محمد عبد الله" />
        </div>

        <div class="form-row" style="align-items:flex-start">
          <label>اختيار شعار الفريق</label>
          <div style="flex:1">
            <div style="display:flex;gap:8px;margin-bottom:8px">
              <button id="openLogosBtn" class="ghost">اختيار من الشبكة</button>
              <label class="ghost" style="padding:10px;border-radius:8px;cursor:pointer">
                رفع شعار جديد
                <input id="uploadLogoInput" type="file" accept="image/*" style="display:none" />
              </label>
              <button id="clearLogoBtn" class="ghost">بدون شعار</button>
            </div>

            <div id="logosContainer" class="logos-grid" aria-hidden="false"></div>
            <div class="muted">اضغط على شعار لاختياره. أي شعار ترفعه يظهر فورًا في الشبكة.</div>
          </div>
        </div>

        <div style="display:flex;gap:8px;margin-top:12px">
          <button id="addParticipantBtn">أضف المتبارٍ</button>
          <button id="resetBtn" class="ghost">إعادة تعيين البيانات</button>
        </div>

        <div style="margin-top:14px" class="muted">
          الحفظ يتم محليًا في المتصفح (localStorage) — مناسب لتجربة GitHub Pages. لإدارة حقيقية، سنربط قاعدة بيانات لاحقًا.
        </div>

        <hr style="margin:16px 0;border:none;border-top:1px solid rgba(255,255,255,0.03)" />

        <h2>قائمة المشاركين</h2>
        <div id="participantsList" class="participants"></div>
      </div>

      <aside class="panel preview">
        <div class="preview-box">
          <h2>معاينة سريعة</h2>
          <div style="display:flex;gap:12px;align-items:center">
            <div id="previewThumb" class="p-thumb" style="width:72px;height:72px;border-radius:12px;background:linear-gradient(135deg,var(--accent),#7c3aed)">?</div>
            <div>
              <div id="previewName" style="font-weight:700;font-size:16px">لا يوجد متبارٍ مختار</div>
              <div class="small muted" id="previewTeam">الشعار: لا شيء</div>
            </div>
          </div>
        </div>

        <div class="preview-box">
          <h2>شبكة الشعارات المتاحة (32)</h2>
          <div class="small muted">هذه شعارات افتراضية (placeholders). لاحقًا ستستبدلها بشعارات حقيقية عبر لوحة المشرف.</div>
          <div id="miniLogos" class="small-grid" style="margin-top:8px"></div>
        </div>

        <div class="preview-box">
          <h2>الإجراءات</h2>
          <button id="exportBtn" class="ghost">تصدير المشاركين (JSON)</button>
          <button id="importBtn" class="ghost">استيراد JSON</button>
          <input id="importFile" type="file" accept="application/json" style="display:none" />
        </div>
      </aside>
    </div>

    <footer>
      تجربة سريعة: احفظ الملف `index.html` في مستودع GitHub وفعل GitHub Pages لعرضها. — إذا تريد أجهز لك نسخة Node/Firebase لاحقًا أضيفها.
    </footer>
  </div>

  <script>
    /* ========= بيانات الفرق النهائية (32 فريقًا) =========
       هذه هي قائمة الـ32 فريقًا المتفق عليها (مع معرف فريد slug).
       iconDataUrl سيُملأ بقيم data URL للـ placeholder SVG تلقائيًا.
    */
    const TEAM_LIST = [
      { id: "inter_milan", name: "Inter Milan" },
      { id: "ac_milan", name: "AC Milan" },
      { id: "as_roma", name: "AS Roma" },
      { id: "napoli", name: "Napoli" },
      { id: "atalanta", name: "Atalanta" },
      { id: "lazio", name: "Lazio" },
      { id: "como", name: "Como" },

      { id: "fc_barcelona", name: "FC Barcelona" },
      { id: "real_madrid", name: "Real Madrid" },

      { id: "celtic", name: "Celtic" },

      { id: "man_city", name: "Manchester City" },
      { id: "liverpool", name: "Liverpool" },
      { id: "arsenal", name: "Arsenal" },
      { id: "chelsea", name: "Chelsea" },
      { id: "man_united", name: "Manchester United" },

      { id: "lille", name: "Lille" },
      { id: "psg", name: "Paris Saint-Germain" },
      { id: "ol_lyon", name: "Olympique Lyon" },
      { id: "as_monaco", name: "AS Monaco" },
      { id: "olympique_marseille", name: "Olympique Marseille" },

      { id: "botafogo", name: "Botafogo" },
      { id: "fluminense", name: "Fluminense" },
      { id: "palmeiras", name: "Palmeiras" },
      { id: "sao_paulo", name: "São Paulo" },
      { id: "santos", name: "Santos" },
      { id: "flamengo", name: "Flamengo" },

      { id: "borussia_dortmund", name: "Borussia Dortmund" },
      { id: "bayer_leverkusen", name: "Bayer Leverkusen" },
      { id: "bayern_munich", name: "Bayern Munich" },
      { id: "eintracht_frankfurt", name: "Eintracht Frankfurt" },

      { id: "fc_porto", name: "FC Porto" },
      { id: "sporting_cp", name: "Sporting CP" }
    ];

    // Storage keys
    const KEY_PARTICIPANTS = "ef_admin_participants_v1";
    const KEY_TEAMS = "ef_admin_teams_v1";

    // small helpers
    const $ = (id) => document.getElementById(id);

    // load or init teams (with placeholder icons)
    function initTeams() {
      const saved = localStorage.getItem(KEY_TEAMS);
      if (saved) return JSON.parse(saved);
      // create placeholder svg dataURLs for each team
      const teams = TEAM_LIST.map(t => ({ ...t, iconDataUrl: generatePlaceholderSVGDataUrl(t) }));
      localStorage.setItem(KEY_TEAMS, JSON.stringify(teams));
      return teams;
    }

    function generatePlaceholderSVGDataUrl(team) {
      const initials = getInitials(team.name);
      // choose color deterministic by id
      const hue = Math.abs(hashCode(team.id)) % 360;
      const bg = `hsl(${hue} 70% 35%)`;
      const fg = "#fff";
      const svg = `<svg xmlns='http://www.w3.org/2000/svg' width='220' height='220'>
        <rect width='100%' height='100%' rx='18' fill='${bg}' />
        <text x='50%' y='52%' font-family='Arial, Helvetica, sans-serif' font-size='64' fill='${fg}' text-anchor='middle' dominant-baseline='middle' font-weight='700'>${escapeXml(initials)}</text>
      </svg>`;
      return `data:image/svg+xml;base64,${btoa(svg)}`;
    }

    function getInitials(name){
      // Arabic/English friendly: pick first letters of up to two words
      const parts = name.replace(/\(|\)|\./g,"").split(/[\s\-]+/).filter(Boolean);
      if(parts.length===1) return parts[0].slice(0,2).toUpperCase();
      return (parts[0][0]+parts[1][0]).toUpperCase();
    }
    function escapeXml(s){ return s.replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }
    function hashCode(str){ let h=0; for(let i=0;i<str.length;i++){h=(h<<5)-h+str.charCodeAt(i);h=h&h;} return h; }

    // participants management
    function loadParticipants(){ const s = localStorage.getItem(KEY_PARTICIPANTS); return s?JSON.parse(s):[] }
    function saveParticipants(list){ localStorage.setItem(KEY_PARTICIPANTS, JSON.stringify(list)); }

    // State
    let TEAMS = initTeams();
    let selectedTeamId = null; // id of selected logo
    let participants = loadParticipants();

    // Render logos grid (main)
    function renderLogosGrid() {
      const container = $("logosContainer");
      container.innerHTML = "";
      TEAMS.forEach(team => {
        const el = document.createElement("div");
        el.className = "logo-item";
        el.title = team.name;
        el.dataset.teamId = team.id;
        el.innerHTML = `
          <div class="logo-thumb" style="background-image:url('${team.iconDataUrl}');background-size:cover;background-position:center;"></div>
          <div class="logo-name">${team.name}</div>
        `;
        el.addEventListener("click", ()=> {
          selectedTeamId = team.id;
          updateSelectionUI();
          updatePreview();
        });
        container.appendChild(el);
      });
      updateSelectionUI();
    }

    function updateSelectionUI(){
      document.querySelectorAll(".logo-item").forEach(it=>{
        it.classList.toggle("selected", it.dataset.teamId === selectedTeamId);
      });
      // mini grid
      const mini = $("miniLogos");
      mini.innerHTML="";
      TEAMS.slice(0,9).forEach(t=>{
        const d = document.createElement("div");
        d.style.display="flex";d.style.alignItems="center";d.style.justifyContent="center";
        d.innerHTML = `<div style="width:56px;height:56px;border-radius:8px;background-image:url('${t.iconDataUrl}');background-size:cover;background-position:center"></div>`;
        mini.appendChild(d);
      });
    }

    // render participants list
    function renderParticipants(){
      const list = $("participantsList");
      list.innerHTML = "";
      if(participants.length===0){
        list.innerHTML = `<div class="muted">لا يوجد مشاركين حتى الآن</div>`;
        return;
      }
      participants.forEach((p, idx) => {
        const team = TEAMS.find(t=>t.id===p.teamId);
        const div = document.createElement("div");
        div.className = "participant";
        const imgUrl = p.iconUrl || (team?team.iconDataUrl:null);
        div.innerHTML = `
          <div class="p-thumb" style="background-image:url('${imgUrl || ''}');background-size:cover;background-position:center">${!imgUrl? "?" : ""}</div>
          <div>
            <div class="p-name">${escapeHtml(p.name)}</div>
            <div class="small muted">${team?team.name: (p.teamName || 'بدون فريق')}</div>
          </div>
          <button class="trash" title="حذف" data-idx="${idx}">🗑️</button>
        `;
        list.appendChild(div);
      });
      // attach delete events
      list.querySelectorAll(".trash").forEach(b=>{
        b.addEventListener("click", (e)=>{
          const idx = Number(b.dataset.idx);
          participants.splice(idx,1);
          saveParticipants(participants);
          renderParticipants();
        });
      });
    }

    function escapeHtml(s){ return (s||"").replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;"); }

    // preview
    function updatePreview(){
      const previewThumb = $("previewThumb");
      const previewName = $("previewName");
      const previewTeam = $("previewTeam");
      const name = $("participantName").value.trim();
      const team = TEAMS.find(t=>t.id===selectedTeamId);
      previewName.textContent = name || "لا يوجد متبارٍ مختار";
      previewTeam.textContent = team? ("الشعار: "+team.name) : "الشعار: لا شيء";
      const imgUrl = team ? team.iconDataUrl : null;
      previewThumb.style.backgroundImage = imgUrl ? `url('${imgUrl}')` : "none";
      previewThumb.textContent = imgUrl? "": "?";
    }

    // events
    document.addEventListener("DOMContentLoaded", ()=>{
      renderLogosGrid();
      renderParticipants();
      updatePreview();

      $("participantName").addEventListener("input", updatePreview);

      $("openLogosBtn").addEventListener("click", ()=>{
        const container = $("logosContainer");
        container.scrollIntoView({behavior:"smooth", block:"center"});
      });

      $("addParticipantBtn").addEventListener("click", ()=>{
        const name = $("participantName").value.trim();
        if(!name){ alert("أدخل اسم المتبارٍ أولاً"); return; }
        const team = TEAMS.find(t=>t.id===selectedTeamId);
        const iconUrl = (team && team.iconDataUrl) || null;
        const p = { id: "p_"+Date.now(), name, teamId: team?team.id:null, teamName: team?team.name:null, iconUrl };
        participants.push(p);
        saveParticipants(participants);
        renderParticipants();
        // reset form
        $("participantName").value = "";
        selectedTeamId = null;
        updateSelectionUI();
        updatePreview();
      });

      $("resetBtn").addEventListener("click", ()=>{
        if(!confirm("هل تريد إعادة تعيين المشاركين (سيُحذف كل شيء مخزون محلياً)؟")) return;
        participants = [];
        localStorage.removeItem(KEY_PARTICIPANTS);
        renderParticipants();
      });

      // upload logo handling
      $("uploadLogoInput").addEventListener("change", async (ev)=>{
        const f = ev.target.files[0];
        if(!f) return;
        if(f.size > 2_500_000){ alert("حجم الملف كبير، الرجاء اختيار ملف أصغر من ~2.5MB"); return; }
        const reader = new FileReader();
        reader.onload = () => {
          // add to TEAMS as a custom logo entry (new team if no selection)
          // we add a new team entry auto-generated id
          const newTeamId = "custom_"+Date.now();
          const newTeam = { id: newTeamId, name: f.name.replace(/\.[^/.]+$/, ""), iconDataUrl: reader.result };
          TEAMS.unshift(newTeam);
          // persist teams
          localStorage.setItem(KEY_TEAMS, JSON.stringify(TEAMS));
          // set selected to new one
          selectedTeamId = newTeamId;
          renderLogosGrid();
          renderParticipants();
          updatePreview();
        };
        reader.readAsDataURL(f);
        // clear input
        ev.target.value = "";
      });

      // clear logo button -> set no selection
      $("clearLogoBtn").addEventListener("click", ()=>{
        selectedTeamId = null;
        updateSelectionUI();
        updatePreview();
      });

      // export JSON
      $("exportBtn").addEventListener("click", ()=>{
        const blob = new Blob([JSON.stringify(participants, null, 2)], {type:"application/json;charset=utf-8"});
        const url = URL.createObjectURL(blob);
        const a = document.createElement("a");
        a.href = url;
        a.download = "participants.json";
        a.click();
        URL.revokeObjectURL(url);
      });

      // import JSON
      $("importBtn").addEventListener("click", ()=> $("importFile").click());
      $("importFile").addEventListener("change", (e)=>{
        const f = e.target.files[0];
        if(!f) return;
        const r = new FileReader();
        r.onload = () => {
          try{
            const data = JSON.parse(r.result);
            if(!Array.isArray(data)) throw new Error("الملف ليس مصفوفة JSON");
            // basic validation
            data.forEach(item => {
              if(!item.name) throw new Error("كل عنصر يجب أن يحتوي حقل name");
            });
            participants = data.map(it => ({ id: it.id || "p_"+Date.now()+Math.random(), name: it.name, teamId: it.teamId || null, teamName: it.teamName || null, iconUrl: it.iconUrl || null }));
            saveParticipants(participants);
            renderParticipants();
          }catch(err){
            alert("خطأ في استيراد الملف: "+err.message);
          }
        };
        r.readAsText(f);
      });
    });
  </script>
</body>
</html>


/* init render */
renderAll();
</script>
</body>
</html>


