<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ashley — Impact & Investment Dashboard</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;600;700&family=DM+Sans:wght@300;400;500;600&display=swap');

:root {
  --bg:#0f0f14; --surface:#17171f; --surface2:#1e1e28; --surface3:#25252f;
  --border:rgba(255,255,255,0.07); --text:#f0eff5; --muted:#8886a0;
  --gold:#c8a96e; --purple:#7c6fff; --green:#4ecfa4; --red:#e06b7d; --blue:#5bbfdf;
}
*{box-sizing:border-box;margin:0;padding:0;}
body{background:var(--bg);color:var(--text);font-family:'DM Sans',sans-serif;font-size:14px;min-height:100vh;}
body::before{content:'';position:fixed;top:-200px;left:-200px;width:600px;height:600px;background:radial-gradient(circle,rgba(124,111,255,.07),transparent 70%);pointer-events:none;z-index:0;}
body::after{content:'';position:fixed;bottom:-200px;right:-200px;width:700px;height:700px;background:radial-gradient(circle,rgba(200,169,110,.06),transparent 70%);pointer-events:none;z-index:0;}
.shell{position:relative;z-index:1;display:flex;min-height:100vh;}

/* SIDEBAR */
.sidebar{width:220px;flex-shrink:0;background:var(--surface);border-right:1px solid var(--border);display:flex;flex-direction:column;padding:32px 0;position:sticky;top:0;height:100vh;overflow-y:auto;}
.logo{padding:0 24px 24px;border-bottom:1px solid var(--border);margin-bottom:16px;}
.logo-name{font-family:'Playfair Display',serif;font-size:18px;font-weight:700;color:var(--gold);}
.logo-role{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.1em;margin-top:4px;}
.nav-section{padding:0 16px;margin-bottom:4px;}
.nav-label{font-size:10px;text-transform:uppercase;letter-spacing:.12em;color:var(--muted);padding:10px 8px 5px;}
.nav-item{display:flex;align-items:center;gap:10px;padding:9px 12px;border-radius:8px;cursor:pointer;color:var(--muted);font-size:13px;transition:all .18s;border:1px solid transparent;}
.nav-item:hover{background:var(--surface2);color:var(--text);}
.nav-item.active{background:linear-gradient(135deg,rgba(200,169,110,.12),rgba(124,111,255,.08));border-color:rgba(200,169,110,.2);color:var(--gold);font-weight:500;}
.nav-icon{font-size:14px;width:18px;text-align:center;}
.sidebar-foot{margin-top:auto;padding:20px 24px 0;border-top:1px solid var(--border);}
.sidebar-foot .yr{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.1em;}

/* MAIN */
.main{flex:1;padding:36px 40px;min-width:0;}
.page{display:none;}
.page.active{display:block;}

/* PAGE HEADER */
.ph{display:flex;align-items:flex-end;justify-content:space-between;margin-bottom:30px;}
.ph-title{font-family:'Playfair Display',serif;font-size:26px;font-weight:600;line-height:1.1;}
.ph-sub{font-size:13px;color:var(--muted);margin-top:4px;}

/* STAT CARDS */
.stats{display:grid;grid-template-columns:repeat(auto-fit,minmax(145px,1fr));gap:14px;margin-bottom:26px;}
.stat{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:18px;position:relative;overflow:hidden;}
.stat::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;background:var(--c,var(--gold));opacity:.7;}
.stat-label{font-size:10px;text-transform:uppercase;letter-spacing:.1em;color:var(--muted);margin-bottom:7px;}
.stat-value{font-family:'Playfair Display',serif;font-size:28px;font-weight:700;line-height:1;}
.stat-sub{font-size:11px;color:var(--muted);margin-top:4px;}

/* GRID */
.g2{display:grid;grid-template-columns:1fr 1fr;gap:18px;margin-bottom:20px;}
.g6535{display:grid;grid-template-columns:1.7fr 1fr;gap:18px;margin-bottom:20px;}

/* CARD */
.card{background:var(--surface);border:1px solid var(--border);border-radius:14px;padding:20px;}
.card-title{font-size:11px;text-transform:uppercase;letter-spacing:.1em;color:var(--muted);margin-bottom:16px;display:flex;align-items:center;gap:7px;}
.card-title em{color:var(--gold);font-style:normal;}
.chart-box{position:relative;height:220px;}
.chart-box-tall{position:relative;height:270px;}

/* TABLE */
.tbl{width:100%;border-collapse:collapse;font-size:13px;}
.tbl th{text-align:left;font-size:10px;text-transform:uppercase;letter-spacing:.1em;color:var(--muted);padding:0 10px 9px 0;border-bottom:1px solid var(--border);}
.tbl td{padding:9px 10px 9px 0;border-bottom:1px solid rgba(255,255,255,.04);vertical-align:middle;}
.tbl tr:last-child td{border-bottom:none;}
.tbl tr:hover td{background:rgba(255,255,255,.02);}

/* BADGES */
.b{display:inline-block;padding:2px 8px;border-radius:20px;font-size:11px;font-weight:500;}
.b-gold{background:rgba(200,169,110,.15);color:var(--gold);border:1px solid rgba(200,169,110,.25);}
.b-purple{background:rgba(124,111,255,.15);color:var(--purple);border:1px solid rgba(124,111,255,.25);}
.b-green{background:rgba(78,207,164,.12);color:var(--green);border:1px solid rgba(78,207,164,.2);}
.b-red{background:rgba(224,107,125,.12);color:var(--red);border:1px solid rgba(224,107,125,.2);}
.b-blue{background:rgba(91,191,223,.12);color:var(--blue);border:1px solid rgba(91,191,223,.2);}
.b-gray{background:rgba(255,255,255,.06);color:var(--muted);border:1px solid rgba(255,255,255,.08);}

/* FORMS */
.fg{display:grid;grid-template-columns:1fr 1fr;gap:12px;margin-bottom:14px;}
.fgfull{grid-column:1/-1;}
.flabel{font-size:10px;text-transform:uppercase;letter-spacing:.08em;color:var(--muted);margin-bottom:4px;display:block;}
.fin,.fsel,.ftxt{background:var(--surface2);border:1px solid var(--border);border-radius:7px;padding:8px 11px;color:var(--text);font-family:'DM Sans',sans-serif;font-size:13px;outline:none;width:100%;transition:border-color .18s;}
.fin:focus,.fsel:focus,.ftxt:focus{border-color:rgba(200,169,110,.4);}
.ftxt{resize:vertical;min-height:65px;}
.fsel option{background:var(--surface2);}

/* BUTTONS */
.btn{padding:8px 18px;border-radius:7px;font-family:'DM Sans',sans-serif;font-size:13px;font-weight:500;cursor:pointer;border:none;transition:all .18s;}
.btn-p{background:var(--gold);color:#0f0f14;}
.btn-p:hover{background:#d4b97a;}
.btn-g{background:transparent;color:var(--muted);border:1px solid var(--border);}
.btn-g:hover{background:var(--surface2);color:var(--text);}
.btn-d{background:rgba(224,107,125,.15);color:var(--red);border:1px solid rgba(224,107,125,.25);}
.btn-d:hover{background:rgba(224,107,125,.25);}
.btn-row{display:flex;gap:9px;margin-top:6px;}

/* FILTER ROW */
.frow{display:flex;gap:9px;margin-bottom:18px;align-items:center;flex-wrap:wrap;}
.frow .fsel{width:auto;min-width:125px;}
.frow .fin{width:190px;}
.sp{flex:1;}

/* LOG ITEMS */
.log-item{display:flex;gap:13px;padding:13px 0;border-bottom:1px solid rgba(255,255,255,.04);align-items:flex-start;}
.log-item:last-child{border-bottom:none;}
.log-icon{width:34px;height:34px;border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:14px;flex-shrink:0;}
.log-meta{flex:1;min-width:0;}
.log-title{font-size:13px;font-weight:500;line-height:1.3;}
.log-detail{font-size:12px;color:var(--muted);margin-top:3px;}
.log-note{font-size:11px;color:var(--muted);margin-top:3px;font-style:italic;opacity:.75;}
.log-right{text-align:right;flex-shrink:0;}
.log-date{font-size:11px;color:var(--muted);}
.log-actions{display:flex;gap:5px;margin-top:7px;justify-content:flex-end;}
.scroller{max-height:340px;overflow-y:auto;padding-right:3px;}
.scroller::-webkit-scrollbar{width:3px;}
.scroller::-webkit-scrollbar-thumb{background:var(--surface3);border-radius:2px;}

/* PILL TABS */
.ptabs{display:flex;gap:6px;background:var(--surface2);border-radius:9px;padding:4px;margin-bottom:20px;width:fit-content;}
.ptab{padding:6px 15px;border-radius:6px;font-size:13px;cursor:pointer;color:var(--muted);border:none;background:none;font-family:'DM Sans',sans-serif;transition:all .18s;}
.ptab.active{background:var(--surface);color:var(--text);font-weight:500;box-shadow:0 1px 5px rgba(0,0,0,.3);}

/* MODAL */
.overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.72);z-index:100;align-items:center;justify-content:center;backdrop-filter:blur(4px);}
.overlay.open{display:flex;}
.modal{background:var(--surface);border:1px solid var(--border);border-radius:14px;padding:26px;width:540px;max-width:95vw;max-height:90vh;overflow-y:auto;}
.modal-hd{display:flex;align-items:center;justify-content:space-between;margin-bottom:20px;}
.modal-title{font-family:'Playfair Display',serif;font-size:19px;font-weight:600;}
.modal-x{cursor:pointer;font-size:19px;border:none;background:none;color:var(--muted);}
.modal-x:hover{color:var(--text);}

/* TOAST */
.toasts{position:fixed;bottom:22px;right:22px;z-index:200;display:flex;flex-direction:column;gap:7px;}
.toast{background:var(--surface2);border:1px solid var(--border);border-radius:9px;padding:11px 16px;font-size:13px;color:var(--text);}

/* GOAL CARDS */
.goal{background:var(--surface2);border-radius:10px;padding:15px;margin-bottom:11px;border-left:3px solid var(--c,var(--gold));}
.goal-name{font-size:13px;font-weight:500;margin-bottom:5px;}
.goal-desc{font-size:12px;color:var(--muted);line-height:1.55;}
.pbar{height:4px;background:var(--surface3);border-radius:3px;margin-top:9px;overflow:hidden;}
.pfill{height:100%;border-radius:3px;background:var(--gold);}

/* EMPTY */
.empty{text-align:center;padding:38px 20px;color:var(--muted);font-size:13px;}
.empty-icon{font-size:30px;margin-bottom:9px;opacity:.5;}

/* EDITABLE CELLS */
.ec{border-radius:4px;padding:2px 5px;outline:none;transition:background .15s,box-shadow .15s;cursor:text;white-space:nowrap;overflow:hidden;max-width:180px;text-overflow:ellipsis;}
.ec:hover{background:rgba(255,255,255,.05);}
.ec:focus{background:var(--surface2);box-shadow:0 0 0 1px rgba(200,169,110,.4);white-space:normal;overflow:visible;}
.ec-sel{background:var(--surface2);border:1px solid var(--border);border-radius:5px;padding:3px 6px;color:var(--text);font-family:'DM Sans',sans-serif;font-size:11px;outline:none;cursor:pointer;max-width:140px;}
.ec-sel:focus{border-color:rgba(200,169,110,.4);}
.ec-sel option{background:var(--surface2);}
.mv-btn{background:none;border:none;color:var(--muted);cursor:pointer;font-size:11px;padding:1px 5px;border-radius:3px;line-height:1;transition:color .15s,background .15s;}
.mv-btn:hover:not([disabled]){color:var(--gold);background:rgba(200,169,110,.1);}
.mv-btn[disabled]{opacity:.2;cursor:default;}
</style>
</head>
<body>
<div class="shell">

<!-- ═══ SIDEBAR ═══ -->
<aside class="sidebar">
  <div class="logo">
    <div class="logo-name">Ashley</div>
    <div class="logo-role">Impact &amp; Investment</div>
  </div>
  <div class="nav-section">
    <div class="nav-label">Overview</div>
    <div class="nav-item active" onclick="nav('overview',this)"><span class="nav-icon">◈</span>Dashboard</div>
  </div>
  <div class="nav-section">
    <div class="nav-label">My Impact</div>
    <div class="nav-item" onclick="nav('talks',this)"><span class="nav-icon">🎤</span>Talks, Panels &amp; Interviews</div>
    <div class="nav-item" onclick="nav('writing',this)"><span class="nav-icon">✍️</span>Writing</div>
    <div class="nav-item" onclick="nav('consulting',this)"><span class="nav-icon">💡</span>Consultations</div>
  </div>
  <div class="nav-section">
    <div class="nav-label">Investments</div>
    <div class="nav-item" onclick="nav('budget',this)"><span class="nav-icon">📊</span>Budget Tracker</div>
    <div class="nav-item" onclick="nav('portfolio',this)"><span class="nav-icon">🌐</span>Portfolio</div>
    <div class="nav-item" onclick="nav('strategy',this)"><span class="nav-icon">🗺️</span>Strategic Goals</div>
  </div>
  <div class="sidebar-foot">
    <div class="yr">2025 – 2027 Cycle</div>
    <div style="margin-top:13px;"><button class="btn btn-g" style="width:100%;font-size:12px;padding:7px 11px;" onclick="openExport()">⬇ Export to Excel</button></div>
  </div>
</aside>

<!-- ═══ MAIN ═══ -->
<main class="main">

<!-- OVERVIEW -->
<div class="page active" id="page-overview">
  <div class="ph">
    <div><div class="ph-title">Good morning, Ashley ☀️</div><div class="ph-sub">Your impact and investment snapshot</div></div>
    <div style="font-size:12px;color:var(--muted);" id="today-date"></div>
  </div>
  <div class="stats">
    <div class="stat" style="--c:var(--gold)"><div class="stat-label">Talks, Panels &amp; Interviews</div><div class="stat-value" id="ov-talks">0</div></div>
    <div class="stat" style="--c:var(--green)"><div class="stat-label">Pieces Written</div><div class="stat-value" id="ov-writing">0</div></div>
    <div class="stat" style="--c:var(--blue)"><div class="stat-label">Consultations</div><div class="stat-value" id="ov-consult">0</div></div>
    <div class="stat" style="--c:var(--red)"><div class="stat-label">2026 Budget</div><div class="stat-value">$10M</div><div class="stat-sub">annual cap</div></div>
  </div>
  <div class="g6535">
    <div class="card"><div class="card-title"><em>◈</em> Impact Activity — Last 12 Months</div><div class="chart-box" id="bar-overview"></div></div>
    <div class="card"><div class="card-title"><em>◈</em> Impact by Type</div><div class="chart-box" id="donut-overview" style="display:flex;align-items:center;justify-content:center;"></div></div>
  </div>
  <div class="g2">
    <div class="card"><div class="card-title"><em>◈</em> Recent Entries</div><div class="scroller" id="recent-list"><div class="empty"><div class="empty-icon">📋</div>No entries yet.</div></div></div>
    <div class="card"><div class="card-title"><em>◈</em> 2026 Investment by Goal</div><div id="goal-list"></div></div>
  </div>
</div>

<!-- TALKS -->
<div class="page" id="page-talks">
  <div class="ph">
    <div><div class="ph-title">Talks, Panels &amp; Interviews</div><div class="ph-sub">Keynotes, panels, conference talks, media interviews, podcasts, press quotes</div></div>
    <button class="btn btn-p" onclick="openAdd('talks')">+ Add Entry</button>
  </div>
  <div class="stats">
    <div class="stat" style="--c:var(--gold)"><div class="stat-label">Total</div><div class="stat-value" id="st-talks-total">0</div></div>
    <div class="stat" style="--c:var(--purple)"><div class="stat-label">This Year</div><div class="stat-value" id="st-talks-yr">0</div></div>
    <div class="stat" style="--c:var(--green)"><div class="stat-label">Keynotes &amp; Talks</div><div class="stat-value" id="st-talks-kt">0</div></div>
    <div class="stat" style="--c:var(--purple)"><div class="stat-label">Panels &amp; Chairs</div><div class="stat-value" id="st-talks-p">0</div></div>
    <div class="stat" style="--c:var(--blue)"><div class="stat-label">Interviews &amp; Press</div><div class="stat-value" id="st-talks-i">0</div></div>
  </div>
  <div class="frow">
    <select class="fsel" id="f-talks-type" onchange="renderTalks()">
      <option value="">All Types</option>
      <option>Keynote</option><option>Conference Talk</option><option>Panelist</option><option>Moderator</option>
      <option>Chair</option><option>Discussant</option><option>Media Interview</option><option>Podcast</option>
      <option>Webinar</option><option>Quoted / Press</option><option>Peer Reviewer</option><option>Contributor</option><option>Other</option>
    </select>
    <select class="fsel" id="f-talks-yr" onchange="renderTalks()">
      <option value="">All Years</option>
      <option>2022</option><option>2023</option><option>2024</option><option>2025</option><option>2026</option><option>2027</option>
    </select>
    <div class="sp"></div>
    <input class="fin" placeholder="🔍  Search..." id="s-talks" oninput="renderTalks()">
  </div>
  <div class="card"><div id="talks-list"><div class="empty"><div class="empty-icon">🎤</div>No entries yet.</div></div></div>
</div>

<!-- WRITING -->
<div class="page" id="page-writing">
  <div class="ph">
    <div><div class="ph-title">Writing</div><div class="ph-sub">Articles, reports, blog posts, op-eds, policy briefs</div></div>
    <button class="btn btn-p" onclick="openAdd('writing')">+ Add Entry</button>
  </div>
  <div class="stats">
    <div class="stat" style="--c:var(--green)"><div class="stat-label">Total</div><div class="stat-value" id="st-wr-total">0</div></div>
    <div class="stat" style="--c:var(--gold)"><div class="stat-label">Reports</div><div class="stat-value" id="st-wr-rep">0</div></div>
    <div class="stat" style="--c:var(--purple)"><div class="stat-label">Articles / Op-Eds</div><div class="stat-value" id="st-wr-art">0</div></div>
    <div class="stat" style="--c:var(--blue)"><div class="stat-label">Blog Posts</div><div class="stat-value" id="st-wr-blog">0</div></div>
  </div>
  <div class="frow">
    <select class="fsel" id="f-wr-type" onchange="renderWriting()">
      <option value="">All Types</option>
      <option>Research Report</option><option>Article</option><option>Op-Ed</option>
      <option>Blog Post</option><option>Policy Brief</option><option>Other</option>
    </select>
    <select class="fsel" id="f-wr-status" onchange="renderWriting()">
      <option value="">All Status</option><option>Published</option><option>In Review</option><option>Draft</option>
    </select>
    <input class="fin" placeholder="🔍  Search..." id="s-writing" oninput="renderWriting()">
  </div>
  <div class="card"><div id="writing-list"><div class="empty"><div class="empty-icon">✍️</div>No entries yet.</div></div></div>
</div>

<!-- CONSULTING -->
<div class="page" id="page-consulting">
  <div class="ph">
    <div><div class="ph-title">Expert Consultations</div><div class="ph-sub">Internal and external consulting, advisory, expert input</div></div>
    <button class="btn btn-p" onclick="openAdd('consulting')">+ Add Entry</button>
  </div>
  <div class="stats">
    <div class="stat" style="--c:var(--blue)"><div class="stat-label">Total</div><div class="stat-value" id="st-co-total">0</div></div>
    <div class="stat" style="--c:var(--gold)"><div class="stat-label">Internal</div><div class="stat-value" id="st-co-int">0</div></div>
    <div class="stat" style="--c:var(--purple)"><div class="stat-label">External</div><div class="stat-value" id="st-co-ext">0</div></div>
    <div class="stat" style="--c:var(--green)"><div class="stat-label">This Month</div><div class="stat-value" id="st-co-mon">0</div></div>
  </div>
  <div class="frow">
    <select class="fsel" id="f-co-type" onchange="renderConsult()">
      <option value="">All</option><option>Internal</option><option>External</option>
    </select>
    <input class="fin" placeholder="🔍  Search..." id="s-consult" oninput="renderConsult()">
  </div>
  <div class="card"><div id="consult-list"><div class="empty"><div class="empty-icon">💡</div>No entries yet.</div></div></div>
</div>

<!-- BUDGET -->
<div class="page" id="page-budget">
  <div class="ph"><div><div class="ph-title">Budget Tracker</div><div class="ph-sub">2022–2029 investment forecast</div></div></div>
  <div class="stats">
    <div class="stat" style="--c:var(--gold)"><div class="stat-label">Annual Budget</div><div class="stat-value">$10M</div></div>
    <div class="stat" style="--c:var(--green)"><div class="stat-label">2026 Forecasted</div><div class="stat-value">$7.0M</div></div>
    <div class="stat" style="--c:var(--blue)"><div class="stat-label">2026 Available</div><div class="stat-value">$975K</div></div>
    <div class="stat" style="--c:var(--red)"><div class="stat-label">APC Wind-Down</div><div class="stat-value">$500K</div></div>
  </div>
  <div class="g2">
    <div class="card"><div class="card-title"><em>◈</em> Portfolio by Year ($M)</div><div class="chart-box-tall"><canvas id="chart-budget"></canvas></div></div>
    <div class="card"><div class="card-title"><em>◈</em> 2026 Allocation</div><div class="chart-box-tall"><canvas id="chart-budget-pie"></canvas></div></div>
  </div>
  <div class="card" style="margin-bottom:20px;">
    <div class="card-title"><em>◈</em> Investment Pipeline</div>
    <div class="frow" style="margin-bottom:13px;">
      <select class="fsel" id="f-inv-status" onchange="renderInvTable()">
        <option value="">All Statuses</option>
        <option>Yes</option><option>Yes - in progress</option><option>Yes - likely to renew</option>
        <option>Probable</option><option>Under Consideration</option><option>Closed</option><option>No</option>
      </select>
      <select class="fsel" id="f-inv-geo" onchange="renderInvTable()">
        <option value="">All Regions</option>
        <option>USA</option><option>UK</option><option>Europe</option><option>Africa</option><option>Canada</option>
      </select>
      <input class="fin" placeholder="🔍  Search..." id="s-inv" oninput="renderInvTable()">
    </div>
    <div style="overflow-x:auto;">
      <table class="tbl"><thead><tr><th>Partner</th><th>Type</th><th>Decision</th><th>Location</th><th>2025</th><th>2026</th><th>2027</th><th>Impact</th><th>Geo</th><th style="width:36px"></th></tr></thead>
      <tbody id="inv-tbody"></tbody></table>
    </div>
  </div>
</div>

<!-- PORTFOLIO -->
<div class="page" id="page-portfolio">
  <div class="ph"><div><div class="ph-title">Investment Portfolio</div><div class="ph-sub">Full partner landscape</div></div></div>
  <div class="ptabs">
    <button class="ptab active" onclick="setTab('active',this)">✅ Active</button>
    <button class="ptab" onclick="setTab('pipeline',this)">🔄 Pipeline</button>
    <button class="ptab" onclick="setTab('closed',this)">🔒 Closed / Declined</button>
  </div>
  <div class="g2">
    <div class="card"><div class="card-title"><em>◈</em> Partners by Org Type</div><div class="chart-box"><canvas id="chart-org"></canvas></div></div>
    <div class="card"><div class="card-title"><em>◈</em> Geographic Distribution</div><div class="chart-box"><canvas id="chart-geo"></canvas></div></div>
  </div>
  <div class="card"><div id="portfolio-list"></div></div>
</div>

<!-- STRATEGY -->
<div class="page" id="page-strategy">
  <div class="ph"><div><div class="ph-title">Strategic Goals 2025–2027</div><div class="ph-sub">Four pillars and investment alignment</div></div></div>
  <div class="stats">
    <div class="stat" style="--c:var(--gold)"><div class="stat-label">Strategic Pillars</div><div class="stat-value">4</div></div>
    <div class="stat" style="--c:var(--purple)"><div class="stat-label">Active Projects</div><div class="stat-value">20+</div></div>
    <div class="stat" style="--c:var(--green)"><div class="stat-label">3-Year Budget</div><div class="stat-value">$30M</div></div>
    <div class="stat" style="--c:var(--blue)"><div class="stat-label">Partner Orgs</div><div class="stat-value">25+</div></div>
  </div>
  <div class="g2" style="margin-bottom:18px;">
    <div class="goal" style="--c:var(--gold)"><div class="goal-name">🏛️ Demonstrate Foundation Leadership</div><div class="goal-desc">Participate and lead in community efforts to affect change in the open research ecosystem. Share and publicize research.</div><div style="margin-top:9px;display:flex;gap:6px;flex-wrap:wrap;"><span class="b b-gold">cOAlition S</span><span class="b b-gold">ORFG</span><span class="b b-gold">Creative Commons</span><span class="b b-gold">ASAPbio</span></div><div style="margin-top:9px;font-size:12px;color:var(--muted);">18% · ~$5.4M</div><div class="pbar"><div class="pfill" style="width:18%;background:var(--gold);"></div></div></div>
    <div class="goal" style="--c:var(--purple)"><div class="goal-name">🌍 Foster Equity &amp; Inclusion in Research</div><div class="goal-desc">Enable read/publish for all researchers globally. Support PRC models and diamond open access.</div><div style="margin-top:9px;display:flex;gap:6px;flex-wrap:wrap;"><span class="b b-purple">F1000/VeriXiv</span><span class="b b-purple">PLOS</span><span class="b b-purple">PREreview</span><span class="b b-purple">RR\ID</span></div><div style="margin-top:9px;font-size:12px;color:var(--muted);">29% · ~$8.7M</div><div class="pbar"><div class="pfill" style="width:29%;background:var(--purple);"></div></div></div>
    <div class="goal" style="--c:var(--green)"><div class="goal-name">⚙️ Effective Policy Implementation</div><div class="goal-desc">Upgrade technology efficiency. Increase grant matching to 95%. Ensure outputs are broadly available.</div><div style="margin-top:9px;display:flex;gap:6px;flex-wrap:wrap;"><span class="b b-green">OA.Works</span><span class="b b-green">OpenAlex</span><span class="b b-green">Zendesk</span></div><div style="margin-top:9px;font-size:12px;color:var(--muted);">10% · ~$3M</div><div class="pbar"><div class="pfill" style="width:10%;background:var(--green);"></div></div></div>
    <div class="goal" style="--c:var(--red)"><div class="goal-name">🚀 Innovate &amp; Challenge the Status Quo</div><div class="goal-desc">Oversee policy shape and vision. Leverage AI for curation and peer review. New preprint models.</div><div style="margin-top:9px;display:flex;gap:6px;flex-wrap:wrap;"><span class="b b-red">PREreview</span><span class="b b-red">RR\ID</span><span class="b b-red">UNESCO</span></div><div style="margin-top:9px;font-size:12px;color:var(--muted);">14% · ~$4.2M</div><div class="pbar"><div class="pfill" style="width:14%;background:var(--red);"></div></div></div>
  </div>
  <div class="card">
    <div class="card-title"><em>◈</em> 5-Year Outcome Framework</div>
    <div class="g2" style="margin-bottom:0;">
      <div style="background:var(--surface2);border-radius:9px;padding:14px;"><div style="font-size:12px;font-weight:600;color:var(--gold);margin-bottom:7px;">Leadership &amp; Policy Influence</div><div style="font-size:12px;color:var(--muted);line-height:1.6;">Evidence-driven policies adopted across key partner countries. Foundation recognized as trusted policy advisor.</div></div>
      <div style="background:var(--surface2);border-radius:9px;padding:14px;"><div style="font-size:12px;font-weight:600;color:var(--purple);margin-bottom:7px;">Scientific Knowledge Ecosystem</div><div style="font-size:12px;color:var(--muted);line-height:1.6;">All funded outputs discoverable and systematically interlinked. Preprints treated as first-class research objects.</div></div>
      <div style="background:var(--surface2);border-radius:9px;padding:14px;"><div style="font-size:12px;font-weight:600;color:var(--green);margin-bottom:7px;">Policy Implementation &amp; Accountability</div><div style="font-size:12px;color:var(--muted);line-height:1.6;">Streamlined workflows and dashboards tracking implementation. Impact measured by reuse and downstream uptake.</div></div>
      <div style="background:var(--surface2);border-radius:9px;padding:14px;"><div style="font-size:12px;font-weight:600;color:var(--red);margin-bottom:7px;">Innovation &amp; Systems Modernization</div><div style="font-size:12px;color:var(--muted);line-height:1.6;">Early adopters establish new models for open practices. AI-enabled tools connect preprints, publications, datasets.</div></div>
    </div>
  </div>
</div>

</main>
</div>

<!-- MODAL -->
<div class="overlay" id="overlay" onclick="if(event.target===this)closeModal()">
  <div class="modal" id="modal-body"></div>
</div>
<div class="toasts" id="toasts"></div>

<script>
let data = {
  talks: [
    {id:1, title:'eLife – Quoted in blog', type:'Quoted / Press', venue:'eLife', date:'2022-10-07', reach:'', link:'', notes:'Quoted in eLife blog post', audience:'External'},
    {id:2, title:'Nature Technology – Quoted', type:'Quoted / Press', venue:'Nature News', date:'2023-01-20', reach:'', link:'', notes:'Quoted by Dalmeet Singh Chawla', audience:'External'},
    {id:3, title:'Open Science Podcast – Guest', type:'Podcast', venue:'Open Science Podcast', date:'2023-01-31', reach:'', link:'', notes:'Guest with Per Pippin Aspaas', audience:'External'},
    {id:4, title:'Open Research Week – Keynote', type:'Keynote', venue:'LJMU Library (Virtual)', date:'2023-03-06', reach:'', link:'', notes:'Virtual keynote', audience:'External'},
    {id:5, title:'NIH Listening Tour – Speaker', type:'Conference Talk', venue:'NIH (Virtual)', date:'2023-04-12', reach:'', link:'', notes:'Virtual panel', audience:'External'},
    {id:6, title:'SSP – Continuing the OA Transformation: Bringing Funders into the Conversation', type:'Conference Talk', venue:'SSP, Portland', date:'2023-06-01', reach:'', link:'', notes:'Panel speaker in Portland', audience:'External'},
    {id:7, title:'Karger / DORA 10-Year Anniversary Video Interview', type:'Media Interview', venue:'Karger Publishers (Virtual)', date:'2023-05-31', reach:'', link:'', notes:'Video interview', audience:'External'},
    {id:8, title:'Data Curation Network US Webinar', type:'Conference Talk', venue:'Data Curation Network (Virtual)', date:'2023-07-10', reach:'', link:'', notes:'Virtual panel', audience:'External'},
    {id:9, title:'Open Research Funders Group Presentation', type:'Conference Talk', venue:'ORFG (Virtual)', date:'2023-08-16', reach:'', link:'', notes:'External presentation', audience:'External'},
    {id:10, title:'MIT Libraries Workshop – Imagining New Models for Open and Equitable Scholarship', type:'Conference Talk', venue:'MIT Libraries (Virtual)', date:'2023-09-27', reach:'', link:'', notes:'Workshop with Chris Bourg', audience:'External'},
    {id:11, title:'Insights Xchange – Interview', type:'Podcast', venue:'Cactus (Podcast)', date:'2023-08-16', reach:'', link:'', notes:'Podcast with Nikesh Gosalia', audience:'External'},
    {id:12, title:'ResearchResearch – Plan S at 5 (Quoted)', type:'Quoted / Press', venue:'Clarivate', date:'2023-08-21', reach:'', link:'', notes:'Quoted in article', audience:'External'},
    {id:13, title:'UW Open Access Week Panel', type:'Conference Talk', venue:'University of Washington (Virtual)', date:'2023-10-23', reach:'', link:'', notes:'Panel speaker', audience:'External'},
    {id:14, title:'Open Access after the Transformation: The Funder Perspective', type:'Conference Talk', venue:'OASPA (Virtual)', date:'2023-11-24', reach:'', link:'', notes:'Virtual panel', audience:'External'},
    {id:15, title:'Foundation Channels & Engagement Team Presentation', type:'Conference Talk', venue:'Internal (Virtual)', date:'2023-11-15', reach:'', link:'', notes:'External comms team', audience:'External'},
    {id:16, title:'Design for Implementation (DFI) Presentation', type:'Conference Talk', venue:'DFI (Virtual)', date:'2024-02-22', reach:'', link:'', notes:'Lacey LaGrone', audience:'External'},
    {id:17, title:'BMGF Editorial Board Presentation', type:'Conference Talk', venue:'BMGF (Virtual)', date:'2024-03-04', reach:'', link:'', notes:'Amanda Nicols', audience:'Internal'},
    {id:18, title:'CGD Event – Advancing Equity and Innovation in Research Publishing', type:'Conference Talk', venue:'CGD (Virtual)', date:'2024-03-27', reach:'', link:'', notes:'Virtual panel', audience:'External'},
    {id:19, title:'OASPA Webinar – The Impact of Plan S', type:'Conference Talk', venue:'OASPA (Virtual)', date:'2024-04-09', reach:'', link:'', notes:'cOAlition S', audience:'External'},
    {id:20, title:'Subcommittee on Open Science Public Access Implementation – Government Agencies', type:'Conference Talk', venue:'Virtual Panel', date:'2024-04-19', reach:'', link:'', notes:'Panel speaker', audience:'External'},
    {id:21, title:'Council of Science Editors – Community Led Publishing', type:'Conference Talk', venue:'CSE (In Person)', date:'2024-05-05', reach:'', link:'', notes:'Daniela S.', audience:'External'},
    {id:22, title:'iSchool Awards – Equity-Minded Alumni Feature', type:'Quoted / Press', venue:'UW iSchool', date:'2024-05-13', reach:'', link:'', notes:'Featured as alumni', audience:'External'},
    {id:23, title:'Academy of Social Science Policy Roundtable Video', type:'Conference Talk', venue:'Virtual', date:'2024-05-23', reach:'', link:'', notes:'Kathy Bowrey', audience:'External'},
    {id:24, title:'VeriXiv Launch Webinar', type:'Webinar', venue:'Virtual', date:'2024-07-10', reach:'', link:'', notes:'External launch webinar', audience:'External'},
    {id:25, title:'Africarxiv – A Research Funder\'s Open Access Policy', type:'Conference Talk', venue:'Virtual', date:'2024-07-25', reach:'', link:'', notes:'', audience:'External'},
    {id:26, title:'WSJ Interview', type:'Media Interview', venue:'Wall Street Journal (Virtual)', date:'2024-10-03', reach:'', link:'', notes:'', audience:'External'},
    {id:27, title:'Publish, Review, Curate Strategic Discussion – COAR', type:'Conference Talk', venue:'COAR (Virtual)', date:'2024-10-30', reach:'', link:'', notes:'', audience:'External'},
    {id:28, title:'The New BMGF OA Policy – Q&A with Ashley Farley', type:'Webinar', venue:'Virtual', date:'2024-11-06', reach:'', link:'', notes:'', audience:'External'},
    {id:29, title:'Center for Open Science Expert Interview', type:'Media Interview', venue:'Virtual', date:'2024-12-04', reach:'', link:'', notes:'', audience:'External'},
    {id:30, title:'Library Journal Interview', type:'Media Interview', venue:'Library Journal (Virtual)', date:'2025-01-14', reach:'', link:'', notes:'', audience:'External'},
    {id:31, title:'VeriXiv Webinar', type:'Webinar', venue:'Virtual', date:'2025-01-23', reach:'', link:'', notes:'', audience:'External'},
    {id:32, title:'Library Journal – Gates 2025 OA Policy & VeriXiv Intro', type:'Webinar', venue:'Library Journal (Virtual)', date:'2025-02-18', reach:'', link:'', notes:'', audience:'External'},
    {id:33, title:'Design for Implementation Conference – Research into Practice', type:'Conference Talk', venue:'DFI (Virtual)', date:'2025-02-20', reach:'', link:'', notes:'Patients & Providers Taking Back the Power', audience:'External'},
    {id:34, title:'Rerun of VeriXiv Webinar', type:'Webinar', venue:'Virtual', date:'2025-03-12', reach:'', link:'', notes:'', audience:'External'},
    {id:35, title:'CZI Preprints Funder Meeting', type:'Conference Talk', venue:'CZI (In Person)', date:'2025-03-17', reach:'', link:'', notes:'', audience:'External'},
    {id:36, title:'CWTS Preprint Study – Interviewee', type:'Media Interview', venue:'Virtual', date:'2025-02-21', reach:'', link:'', notes:'', audience:'External'},
    {id:37, title:'MLIS 574 Interview', type:'Media Interview', venue:'Virtual', date:'2025-04-22', reach:'', link:'', notes:'', audience:'External'},
    {id:38, title:'OA Policy Roadshow – Immunization Team & Grantees', type:'Conference Talk', venue:'Virtual', date:'2025-05-07', reach:'', link:'', notes:'', audience:'External'},
    {id:39, title:'VeriXiv Webinar (May)', type:'Webinar', venue:'Virtual', date:'2025-05-22', reach:'', link:'', notes:'', audience:'External'},
    {id:40, title:'eLife – Quoted (Retains Academic Support Despite Losing Impact Factor)', type:'Quoted / Press', venue:'eLife', date:'2025-05-07', reach:'', link:'', notes:'', audience:'External'},
    {id:41, title:'Charleston Leadership Breakfast Speaker', type:'Keynote', venue:'Charleston Conference (Virtual)', date:'2025-09-04', reach:'', link:'', notes:'', audience:'External'},
    {id:42, title:'UNGA Science Summit 2025 – Financing Open Research for Global Development', type:'Conference Talk', venue:'UNGA (Virtual)', date:'2025-09-09', reach:'', link:'', notes:'Panel on challenges and opportunities', audience:'External'},
    {id:43, title:'University of Washington NIH RFI Discussion', type:'Conference Talk', venue:'UW (Virtual)', date:'2025-09-09', reach:'', link:'', notes:'', audience:'External'},
    {id:44, title:'Charleston Conference – Public Access, Pronto', type:'Conference Talk', venue:'Charleston Conference (Virtual)', date:'2025-11-05', reach:'', link:'', notes:'Federal funders\' policies and impact on scholarly communication', audience:'External'},
    {id:45, title:'Scholastica Water Cooler Chat', type:'Media Interview', venue:'Scholastica (Virtual)', date:'2025-11-01', reach:'', link:'', notes:'Video interview', audience:'External'},
    {id:46, title:'Share Perspectives on Preprint Recognition in Life Sciences', type:'Podcast', venue:'Virtual (Podcast)', date:'2025-08-13', reach:'', link:'', notes:'', audience:'External'},
    {id:47, title:'London Open Science & Scholarship Festival', type:'Conference Talk', venue:'Virtual', date:'2026-04-21', reach:'', link:'', notes:'Camilla Ridgewell', audience:'External'},
    {id:48, title:'AI-Assisted Evidence Synthesis – Interviewee', type:'Media Interview', venue:'Internal IT (Virtual)', date:'2026-02-23', reach:'', link:'', notes:'Evaluating AI-driven evidence synthesis platforms', audience:'Internal'},
    {id:49, title:'Design for Implementation – State of Open Access', type:'Conference Talk', venue:'Virtual', date:'2026-02-26', reach:'', link:'', notes:'Lacey Lagrone', audience:'External'},
    {id:50, title:'Open Research Funders Group – Lightning Talk', type:'Conference Talk', venue:'Seattle (In Person)', date:'2026-03-13', reach:'', link:'', notes:'', audience:'External'},
    {id:51, title:'Interview with Wiley – Market Research', type:'Media Interview', venue:'Wiley (Virtual)', date:'2026-03-24', reach:'', link:'', notes:'', audience:'External'},
    {id:52, title:'Open Interview', type:'Media Interview', venue:'Virtual', date:'2026-04-21', reach:'', link:'', notes:'', audience:'External'},
    {id:53, title:'OA.Works Quoted – Blog', type:'Quoted / Press', venue:'OA.Works Blog', date:'2023-09-01', reach:'', link:'', notes:'', audience:'External'},
,
{id:100, title:'Open Pharma – Panel Speaker', type:'Panelist', venue:'Open Pharma (Virtual)', date:'2023-02-24', reach:'Open Pharma', link:'', notes:'Virtual panel'},
    {id:101, title:'World Information Architecture Day', type:'Panelist', venue:'Seattle (In Person)', date:'2023-03-04', reach:'WIAD', link:'', notes:'Panel speaker in Seattle'},
    {id:102, title:'ASBMB – Science Policy & Advocacy Session', type:'Panelist', venue:'ASBMB, Seattle', date:'2023-03-28', reach:'ASBMB', link:'', notes:'Panel speaker in Seattle'},
    {id:103, title:'SSP – OA Transformation: Bringing Funders into the Conversation', type:'Panelist', venue:'SSP, Portland', date:'2023-06-01', reach:'SSP', link:'', notes:''},
    {id:104, title:'Council of Science Editors – OA Policies Webinar', type:'Panelist', venue:'CSE (Webinar)', date:'2023-10-01', reach:'CSE', link:'', notes:'October webinar on OA policies'},
    {id:105, title:'OASPA – Equitable OA Practices', type:'Panelist', venue:'OASPA (Virtual)', date:'2024-02-08', reach:'OASPA', link:'', notes:'Malavika Legge, OASPA'},
    {id:106, title:'Women in Entrepreneurship Panel', type:'Panelist', venue:'UW Condon Hall', date:'2024-02-21', reach:'UW', link:'', notes:''},
    {id:107, title:'CGD Event – Advancing Equity and Innovation in Research Publishing', type:'Panelist', venue:'CGD (Virtual)', date:'2024-03-27', reach:'CGD', link:'', notes:''},
    {id:108, title:'Subcommittee on Open Science Public Access Implementation', type:'Panelist', venue:'Government Agencies (Virtual)', date:'2024-04-19', reach:'Government', link:'', notes:'Martin'},
    {id:109, title:'Council of Science Editors – Community Led Publishing', type:'Panelist', venue:'CSE (In Person)', date:'2024-05-05', reach:'CSE', link:'', notes:'Daniela S.'},
    {id:110, title:'UW Open Access Week Panel', type:'Panelist', venue:'UW (Virtual)', date:'2023-10-23', reach:'UW', link:'', notes:'Verletta Kern'},
    {id:111, title:'Open Access after the Transformation: The Funder Perspective', type:'Panelist', venue:'OASPA (Virtual)', date:'2023-11-24', reach:'OASPA', link:'', notes:''},
    {id:112, title:'Gates Foundation OA Ambassador Roundtable', type:'Panelist', venue:'Virtual', date:'2024-07-15', reach:'Gates Foundation', link:'', notes:''},
    {id:113, title:'Research to Reader Conference – OA, Preprints, and the Future of Research Communication', type:'Panelist', venue:'Virtual', date:'2025-02-26', reach:'Research to Reader', link:'', notes:''},
    {id:114, title:'AUPress Panel – Subscribe to Open: How\'s it Going? Where\'s it Going?', type:'Panelist', venue:'AUPress (Virtual)', date:'2025-06-11', reach:'AUPress', link:'', notes:''},
    {id:115, title:'UNGA Science Summit – Financing Open Research for Global Development', type:'Panelist', venue:'UNGA (Virtual)', date:'2025-09-09', reach:'UNGA', link:'', notes:''},
    {id:116, title:'University of Washington NIH RFI Discussion', type:'Panelist', venue:'UW (Virtual)', date:'2025-09-09', reach:'UW', link:'', notes:''},
    {id:117, title:'Charleston Conference – Public Access, Pronto', type:'Panelist', venue:'Charleston Conference', date:'2025-11-05', reach:'Charleston', link:'', notes:''},
    {id:118, title:'Second Diamond OA Community Webinar of 2025', type:'Panelist', venue:'Virtual', date:'2025-10-27', reach:'Bregt Saenen', link:'', notes:''},
    {id:119, title:'London Open Science & Scholarship Festival', type:'Panelist', venue:'Virtual', date:'2026-04-21', reach:'Camilla Ridgewell', link:'', notes:''},
    {id:120, title:'Design for Implementation – State of Open Access', type:'Panelist', venue:'Virtual', date:'2026-02-26', reach:'Lacey Lagrone', link:'', notes:''},
    {id:121, title:'eLife Webinar – Fair & Equitable OA Publishing', type:'Panelist', venue:'eLife (Virtual)', date:'2026-03-05', reach:'Shane Aseop', link:'', notes:''},
    {id:122, title:'Diamond Open Access Community Call – 4 Year Anniversary', type:'Panelist', venue:'Virtual', date:'2026-03-16', reach:'Bregt', link:'', notes:''},
    {id:123, title:'3rd Diamond Open Access – Preprints & Repositories', type:'Chair', venue:'India (In Person)', date:'2026-02-02', reach:'', link:'', notes:''},
    {id:124, title:'3rd Diamond Open Access – Research Assessment', type:'Chair', venue:'India (In Person)', date:'2026-02-04', reach:'', link:'', notes:''}
  ],
  writing: [
    {id:200, title:'ASAPbio Authorship – Blog Post', type:'Blog Post', venue:'ASAPbio', date:'2023-01-30', reach:'Published', link:'', notes:'Authorship on ASAPbio blog'},
    {id:201, title:'Aligning Data Sharing Policies', type:'Article', venue:'Philanthropy News Digest', date:'2023-07-12', reach:'Published', link:'', notes:'Commentary'},
    {id:202, title:'Gates Open Research / UW START – Article', type:'Article', venue:'Gates Open Research', date:'2023-06-30', reach:'Published', link:'', notes:'UW START collaboration'},
    {id:203, title:'OpenPharma Blog Post', type:'Blog Post', venue:'OpenPharma', date:'2023-09-12', reach:'Published', link:'', notes:''},
    {id:204, title:'Gates Open Research – Peer Review Week Blog', type:'Blog Post', venue:'Gates Open Research / F1000', date:'2023-09-25', reach:'Published', link:'', notes:''},
    {id:205, title:'Towards Responsible Publishing', type:'Article', venue:'cOAlition S', date:'2023-11-03', reach:'Published', link:'', notes:'Co-authored with cOAlition S'},
    {id:206, title:'Access to Evidence-Based Care: Trauma & Surgical Literature Costs Across Resource Settings', type:'Article', venue:'Academic Journal', date:'2024-01-22', reach:'Published', link:'', notes:'Lacey LaGrone collaboration'},
    {id:207, title:'Changing the Paradigm for Primary Research Dissemination', type:'Article', venue:'Journal', date:'2024-02-15', reach:'Published', link:'', notes:''},
    {id:208, title:'Blog Post – Advancing Equity and Innovation in Research Publishing: Time for a New Era?', type:'Blog Post', venue:'CGD', date:'2024-03-27', reach:'Published', link:'', notes:'Tom Drake collaboration'},
    {id:209, title:'A Fourth Wave of Open Data? Exploring Scenarios for Open Data and Generative AI', type:'Article', venue:'Academic Journal', date:'2024-05-08', reach:'Published', link:'', notes:'Contributor'},
    {id:210, title:'The Changing Landscape of Open Access Policies and Transformative Agreements (Webinar Report)', type:'Research Report', venue:'External Publication', date:'2025-01-23', reach:'Published', link:'', notes:'Webinar report'},
    {id:211, title:'eLife – Retains Academic Support Despite Losing Impact Factor (Quoted)', type:'Article', venue:'eLife', date:'2025-05-07', reach:'Published', link:'', notes:'Quoted in piece'},
    {id:212, title:'Scholarly Communication Librarianship and Open Knowledge (Cited)', type:'Article', venue:'Academic Book', date:'2023-10-11', reach:'Published', link:'', notes:'Work was cited in the book'},
  ],
  consulting: [
    {id:300, title:'GHLF Meeting – Internal Roadshow', type:'Internal', venue:'Trevor (Internal)', date:'2023-05-01', reach:'1', link:'', notes:'Slidedeck shared; invitation for follow-up team presentations'},
    {id:301, title:'CoS Meeting Presentation', type:'Internal', venue:'Toni (Internal)', date:'2023-06-01', reach:'1', link:'', notes:'Slidedeck shared with invitation for follow-up'},
    {id:302, title:'D&T MNCH Meeting', type:'Internal', venue:'Ginny (Internal)', date:'2023-07-26', reach:'1', link:'', notes:'Slidedeck shared'},
    {id:303, title:'PPP Meeting', type:'Internal', venue:'Kristin Savage (Internal)', date:'2023-07-20', reach:'1', link:'', notes:'Slidedeck shared'},
    {id:304, title:'Wellcome Trust Grant Proposal Review', type:'External', venue:'Wellcome Trust', date:'2023-07-17', reach:'', link:'', notes:'Dr. Philip Jones'},
    {id:305, title:'Inspiring STEM Consulting – Advisor', type:'External', venue:'Inspiring STEM', date:'2024-01-24', reach:'', link:'', notes:'Martin Delahunty'},
    {id:306, title:'CoS Meeting', type:'Internal', venue:'Toni Hoover (Internal)', date:'2023-09-14', reach:'', link:'', notes:''},
    {id:307, title:'GH DDSPM Working Meeting', type:'Internal', venue:'Silpa (Internal)', date:'2024-03-04', reach:'', link:'', notes:''},
    {id:308, title:'D&TS Team Meeting', type:'Internal', venue:'Internal', date:'2024-03-21', reach:'', link:'', notes:''},
    {id:309, title:'IDEV Monthly Team Meeting Roadshow', type:'Internal', venue:'Virtual (Internal)', date:'2024-03-25', reach:'', link:'', notes:''},
    {id:310, title:'CRR Comms Meeting Presentation', type:'Internal', venue:'Kate Davidson (Internal)', date:'2024-04-02', reach:'', link:'', notes:''},
    {id:311, title:'2025 OA Policy Refresh Info Session', type:'Internal', venue:'Internal (Virtual)', date:'2024-03-25', reach:'', link:'', notes:''},
    {id:312, title:'2025 OA Policy Refresh Info Session (March)', type:'Internal', venue:'Internal (Virtual)', date:'2024-03-14', reach:'', link:'', notes:''},
    {id:313, title:'GCS Team Synch – 2025 OA Policy Refresh', type:'Internal', venue:'Laura Sparks (Internal)', date:'2024-04-18', reach:'', link:'', notes:''},
    {id:314, title:'CCO – 2025 OA Policy Refresh', type:'Internal', venue:'Ying Wang (Internal)', date:'2024-04-29', reach:'', link:'', notes:''},
    {id:315, title:'GH Lunch & Learn – 2025 OA Policy Refresh', type:'Internal', venue:'GH Division (In Person)', date:'2024-04-23', reach:'', link:'', notes:''},
    {id:316, title:'May Intent 2 Action DEI Meeting', type:'Internal', venue:'Mac (Internal)', date:'2024-05-28', reach:'', link:'', notes:''},
    {id:317, title:'Advise on Invest in Open Infrastructure Strategy', type:'External', venue:'Kaitlin Thaney (Virtual)', date:'2024-06-13', reach:'', link:'', notes:''},
    {id:318, title:'Program Coordinator Community Forum – OA 2025 Policy Refresh', type:'Internal', venue:'Internal (Virtual)', date:'2024-06-18', reach:'', link:'', notes:''},
    {id:319, title:'ICO OA 2025 Policy Refresh Roadshow', type:'Internal', venue:'Internal (Virtual)', date:'2024-06-19', reach:'', link:'', notes:''},
    {id:320, title:'Taskforce for Global Health NTD – OA 2025 Policy Refresh', type:'Internal', venue:'Internal (Virtual)', date:'2024-06-20', reach:'', link:'', notes:''},
    {id:321, title:'CJ Fellows OA 2025 Policy Refresh', type:'Internal', venue:'Internal (Virtual)', date:'2024-06-26', reach:'', link:'', notes:''},
    {id:322, title:'Gates Foundation OA Ambassador Roundtable', type:'External', venue:'Virtual', date:'2024-07-15', reach:'', link:'', notes:''},
    {id:323, title:'Crop R&D Team Presentation', type:'Internal', venue:'Internal (Virtual)', date:'2024-07-15', reach:'', link:'', notes:''},
    {id:324, title:'PMNCH OA Policy Roadshow', type:'Internal', venue:'Internal (Virtual)', date:'2024-08-13', reach:'', link:'', notes:''},
    {id:325, title:'Malaria Team OA Policy Roadshow', type:'Internal', venue:'Internal (Virtual)', date:'2024-08-14', reach:'', link:'', notes:''},
    {id:326, title:'CWC / Gates AgOne OA Policy Roadshow', type:'External', venue:'Virtual', date:'2024-09-05', reach:'', link:'', notes:''},
    {id:327, title:'GSK OA Policy Roadshow', type:'External', venue:'GSK (Virtual)', date:'2024-09-05', reach:'', link:'', notes:''},
    {id:328, title:'Family Planning Team Meeting – OA Policy Roadshow', type:'Internal', venue:'Internal (Virtual)', date:'2024-09-18', reach:'', link:'', notes:''},
    {id:329, title:'Business Partner OA Policy Roadshow', type:'Internal', venue:'Internal (Virtual)', date:'2024-10-15', reach:'', link:'', notes:''},
    {id:330, title:'OA Lunch & Learn', type:'Internal', venue:'Internal (In Person)', date:'2024-10-24', reach:'', link:'', notes:''},
    {id:331, title:'Family Planning Team Meeting – OA Policy Roadshow (Nov)', type:'Internal', venue:'Internal (Virtual)', date:'2024-11-12', reach:'', link:'', notes:''},
    {id:332, title:'Product Development Week – Open Access for Unlocking Product Development', type:'Internal', venue:'Janet White (In Person)', date:'2025-01-09', reach:'', link:'', notes:''},
    {id:333, title:'IDM All Hands', type:'Internal', venue:'Mandy Izzo (Virtual)', date:'2025-01-22', reach:'', link:'', notes:''},
    {id:334, title:'PATH OA Policy Roadshow', type:'External', venue:'PATH (Virtual)', date:'2025-01-24', reach:'', link:'', notes:''},
    {id:335, title:'GatesMRI Presentation', type:'External', venue:'GatesMRI (Virtual)', date:'2025-01-28', reach:'', link:'', notes:''},
    {id:336, title:'Africa PC Huddle', type:'Internal', venue:'Internal (Virtual)', date:'2025-02-05', reach:'', link:'', notes:''},
    {id:337, title:'CAVD OA Policy Roadshow', type:'External', venue:'CAVD (Virtual)', date:'2025-04-09', reach:'Rosalind', link:'', notes:''},
    {id:338, title:'PATH/JIS OA Policy Roadshow', type:'External', venue:'PATH/JIS (Virtual)', date:'2025-04-10', reach:'', link:'', notes:''},
    {id:339, title:'TRA/Polio OA Policy Roadshow', type:'Internal', venue:'In Person', date:'2025-04-15', reach:'', link:'', notes:''},
    {id:340, title:'OA Policy Roadshow – Women Health Innovations Team', type:'Internal', venue:'In Person', date:'2025-05-07', reach:'', link:'', notes:''},
    {id:341, title:'Research Consulting – Innovations in Scholarly Communication', type:'External', venue:'Research Consulting (Virtual)', date:'2025-05-29', reach:'', link:'', notes:''},
    {id:342, title:'OASPA\'s Next 50% Goal Project – Consultant', type:'External', venue:'OASPA (Virtual)', date:'2025-06-10', reach:'', link:'', notes:''},
    {id:343, title:'GiveWell Policy Consulting', type:'External', venue:'GiveWell (Virtual)', date:'2025-06-17', reach:'', link:'', notes:''},
    {id:344, title:'SCOSS / cOAlition S – Open Infrastructure Business Models', type:'External', venue:'Virtual', date:'2025-03-01', reach:'', link:'', notes:''},
    {id:345, title:'RRIE URAP Workshops', type:'External', venue:'Virtual', date:'2026-04-21', reach:'', link:'', notes:''},
    {id:346, title:'Rapid Reviews Infectious Disease – OA Policy Refresh', type:'External', venue:'Virtual', date:'2024-04-09', reach:'Stefano B', link:'', notes:''},
    {id:347, title:'GH Labs – Gates Open Research & 2025 OA Policy Refresh', type:'External', venue:'Virtual', date:'2024-04-17', reach:'Steve Kern', link:'', notes:''},
    {id:348, title:'Rapid Reviews Infectious Disease Board Meeting', type:'External', venue:'Virtual', date:'2024-05-02', reach:'Stefano Bertozzi', link:'', notes:''},
    {id:349, title:'RIPE Open Access Roadshow', type:'External', venue:'GatesAgOne (Virtual)', date:'2024-05-16', reach:'', link:'', notes:''},
    {id:350, title:'Society Publishers Coalition', type:'External', venue:'FASEB / Darla Henderson (Virtual)', date:'2024-05-16', reach:'', link:'', notes:''},
    {id:351, title:'ENSA – 2025 OA Policy Refresh', type:'External', venue:'GatesAgOne (Virtual)', date:'2024-04-23', reach:'', link:'', notes:''},
    {id:352, title:'CASS – 2025 OA Policy Refresh', type:'External', venue:'GatesAgOne (Virtual)', date:'2024-04-09', reach:'', link:'', notes:''},
    {id:353, title:'AI-Assisted Evidence Synthesis Consultation', type:'Internal', venue:'IT (Virtual)', date:'2026-02-23', reach:'', link:'', notes:'Evaluating AI-driven evidence synthesis platforms'},
    {id:354, title:'Interview – Landscape Analysis of Bibliometric Databases', type:'External', venue:'Virtual', date:'2025-06-12', reach:'', link:'', notes:''},
    {id:355, title:'Swiss OA Strategy Review – Peer Reviewer', type:'External', venue:'Swiss Universities', date:'2024-01-15', reach:'Marc Aeby', link:'', notes:''},
  ]
};

const investments = [
  {name:'F1000 (VeriXiv + GOR)',type:'Preprint Server + PRC',decision:'Yes',loc:'UK',y2025:1250000,y2026:1000000,y2027:0,impact:'Small',geo:'Global',status:'active'},
  {name:'PLOS Public Library of Science',type:'Publisher',decision:'Yes',loc:'USA',y2025:1000000,y2026:1000000,y2027:1000000,impact:'Medium',geo:'Global',status:'active'},
  {name:'OA.Works',type:'Technology Service',decision:'Yes - in progress',loc:'UK',y2025:50000,y2026:1000000,y2027:1000000,impact:'Medium',geo:'None',status:'active'},
  {name:'PREreview',type:'Preprint Peer Review',decision:'Yes',loc:'USA',y2025:750000,y2026:0,y2027:0,impact:'Medium',geo:'Global',status:'active'},
  {name:'CC Creative Commons',type:'Member Org',decision:'Yes - likely to renew',loc:'USA',y2025:750000,y2026:0,y2027:0,impact:'Medium',geo:'Global',status:'active'},
  {name:'RR\\ID',type:'Preprint Peer Review',decision:'Yes - likely to renew',loc:'USA',y2025:500000,y2026:0,y2027:0,impact:'Small',geo:'Global',status:'active'},
  {name:'ScholComm Lab',type:'Research',decision:'Yes',loc:'Canada',y2025:250000,y2026:250000,y2027:250000,impact:'Small',geo:'None',status:'active'},
  {name:'OPERAS Diamond OA',type:'Member Org',decision:'Yes',loc:'Europe',y2025:250000,y2026:250000,y2027:0,impact:'Large',geo:'Global',status:'active'},
  {name:'CC Creative Commons (preprint)',type:'Legal Licenses',decision:'Yes',loc:'USA',y2025:50000,y2026:250000,y2027:0,impact:'Large',geo:'Global',status:'active'},
  {name:'cOAlition S',type:'Member Org',decision:'Yes - in progress',loc:'Europe',y2025:0,y2026:150000,y2027:150000,impact:'Small',geo:'Global North',status:'active'},
  {name:'S2O Community of Practice',type:'Advocacy Group',decision:'Yes',loc:'USA',y2025:75000,y2026:75000,y2027:0,impact:'Large',geo:'Global',status:'active'},
  {name:'ORFG',type:'Member Org',decision:'Yes',loc:'USA',y2025:50000,y2026:50000,y2027:50000,impact:'Medium',geo:'Global North',status:'active'},
  {name:'PKP:SUP',type:'S2O / Technology Service',decision:'Yes',loc:'USA',y2025:50000,y2026:50000,y2027:50000,impact:'Small',geo:'Global',status:'active'},
  {name:'Vivli',type:'Technology Development',decision:'Yes',loc:'USA',y2025:50000,y2026:50000,y2027:50000,impact:'Medium',geo:'Global North',status:'active'},
  {name:'ASAPbio',type:'Advocacy Group',decision:'Yes',loc:'USA',y2025:50000,y2026:50000,y2027:50000,impact:'Small',geo:'Global',status:'active'},
  {name:'Knowledge Futures Group',type:'Technology Service',decision:'Yes',loc:'USA',y2025:25000,y2026:25000,y2027:25000,impact:'Small',geo:'Global',status:'active'},
  {name:'Open Knowledge Maps',type:'Technology Service',decision:'Yes',loc:'Europe',y2025:25000,y2026:25000,y2027:25000,impact:'Small',geo:'Global',status:'active'},
  {name:'UNESCO Open Solutions',type:'Intergovernmental Organization',decision:'Yes - in progress',loc:'Europe',y2025:0,y2026:750000,y2027:750000,impact:'Medium',geo:'Developing Countries',status:'active'},
  {name:'IWA Int\'l Water Assn',type:'S2O Journals',decision:'Yes - in progress',loc:'UK',y2025:0,y2026:100000,y2027:100000,impact:'Small',geo:'Global',status:'active'},
  {name:'Scholarly Communication Lab',type:'Public Institution',decision:'Yes - in progress',loc:'Southeast Asia',y2025:0,y2026:150000,y2027:150000,impact:'Small',geo:'Developing Countries',status:'active'},
  {name:'GRIOS',type:'Member Org',decision:'Yes - in progress',loc:'Europe',y2025:0,y2026:50000,y2027:50000,impact:'Small',geo:'Global',status:'active'},
  {name:'Global Health: Science and Practice',type:'Diamond Journal',decision:'Probable',loc:'USA',y2025:0,y2026:250000,y2027:250000,impact:'Small',geo:'Global North',status:'pipeline'},
  {name:'Open Journals Collective',type:'Diamond Journals',decision:'Probable',loc:'UK',y2025:0,y2026:750000,y2027:0,impact:'',geo:'',status:'pipeline'},
  {name:'Open Educational Resources in Africa',type:'Diamond Journals',decision:'Probable',loc:'Africa',y2025:0,y2026:750000,y2027:500000,impact:'',geo:'Developing Countries',status:'pipeline'},
  {name:'INasp',type:'Charity',decision:'Under Consideration',loc:'UK',y2025:0,y2026:1000000,y2027:750000,impact:'',geo:'',status:'pipeline'},
  {name:'Lyrasis: US Diamond OA',type:'Technology Development',decision:'Closed',loc:'USA',y2025:150000,y2026:0,y2027:0,impact:'Small',geo:'Global North',status:'closed'},
  {name:'National Academy of Sciences',type:'Academic Institution',decision:'Closed',loc:'USA',y2025:0,y2026:0,y2027:0,impact:'Small',geo:'Global North',status:'closed'},
  {name:'BMA/BMJ',type:'Society Publisher',decision:'No',loc:'UK',y2025:0,y2026:500000,y2027:500000,impact:'Small',geo:'Global',status:'closed'},
  {name:'DataCite',type:'Open Data facilitator',decision:'No',loc:'Europe',y2025:0,y2026:150000,y2027:150000,impact:'Small',geo:'Global',status:'closed'},
];

let portfolioTab = 'active';
let _orgChart, _geoChart, _budgetChart, _budgetPieChart;

// ─── ICONS / COLOURS ───────────────────────────────────────────────
const ICONS = {
  'Keynote':'🎯','Conference Talk':'🎤','Panelist':'🪑','Moderator':'🎙️','Chair':'👑','Discussant':'💬',
  'Media Interview':'📺','Podcast':'🎧','Webinar':'💻','Quoted / Press':'💬','Peer Reviewer':'🔍','Contributor':'🖊️',
  'Research Report':'📄','Article':'📰','Op-Ed':'✍️','Blog Post':'📝','Policy Brief':'📋',
  'Internal':'🏛️','External':'🌐'
};
const BGCOL = {
  'Keynote':'rgba(200,169,110,.15)','Conference Talk':'rgba(200,169,110,.1)','Panelist':'rgba(124,111,255,.15)',
  'Moderator':'rgba(78,207,164,.12)','Chair':'rgba(200,169,110,.12)','Discussant':'rgba(91,191,223,.1)',
  'Media Interview':'rgba(91,191,223,.15)','Podcast':'rgba(91,191,223,.1)','Webinar':'rgba(124,111,255,.12)',
  'Quoted / Press':'rgba(78,207,164,.12)','Peer Reviewer':'rgba(91,191,223,.1)','Contributor':'rgba(200,169,110,.1)',
  'Research Report':'rgba(78,207,164,.12)','Article':'rgba(78,207,164,.1)','Op-Ed':'rgba(200,169,110,.12)',
  'Blog Post':'rgba(91,191,223,.12)','Policy Brief':'rgba(124,111,255,.12)',
  'Internal':'rgba(91,191,223,.15)','External':'rgba(78,207,164,.12)'
};

// ─── HELPERS ───────────────────────────────────────────────────────
function fmtDate(d) {
  if (!d) return '';
  try { return new Date(d+'T12:00:00').toLocaleDateString('en-US',{month:'short',day:'numeric',year:'numeric'}); }
  catch(e){ return d; }
}
function fmtMoney(n) {
  if (!n) return '—';
  if (n>=1000000) return '$'+(n/1000000).toFixed(1)+'M';
  if (n>=1000) return '$'+(n/1000).toFixed(0)+'K';
  return '$'+n;
}
function toast(msg) {
  const c=document.getElementById('toasts');
  const t=document.createElement('div'); t.className='toast'; t.textContent=msg; c.appendChild(t);
  setTimeout(()=>{t.style.opacity='0';t.style.transition='opacity .3s';setTimeout(()=>t.remove(),300);},2600);
}

// ─── NAVIGATION ────────────────────────────────────────────────────
function nav(page, el) {
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
  document.getElementById('page-'+page).classList.add('active');
  if(el) el.classList.add('active');
  if(page==='talks')     { renderTalks(); }
  if(page==='writing')   { renderWriting(); }
  if(page==='consulting'){ renderConsult(); }
  if(page==='budget')    { renderBudget(); }
  if(page==='portfolio') { renderPortfolio(); }
  if(page==='overview')  { drawOverviewCharts(); }
}

// ─── OVERVIEW ──────────────────────────────────────────────────────
function updateStats() {
  document.getElementById('ov-talks').textContent   = data.talks.length;
  document.getElementById('ov-writing').textContent = data.writing.length;
  document.getElementById('ov-consult').textContent = data.consulting.length;
}

function drawOverviewCharts() {
  updateStats();

  // Recent entries
  const all = [
    ...data.talks.map(e=>({...e,_cat:'talks'})),
    ...data.writing.map(e=>({...e,_cat:'writing'})),
    ...data.consulting.map(e=>({...e,_cat:'consulting'}))
  ].filter(e=>e&&e.date).sort((a,b)=>new Date(b.date)-new Date(a.date)).slice(0,6);

  const rl = document.getElementById('recent-list');
  rl.innerHTML = all.length ? all.map(e=>`
    <div class="log-item">
      <div class="log-icon" style="background:${BGCOL[e.type]||'rgba(255,255,255,.06)'}">${ICONS[e.type]||'📌'}</div>
      <div class="log-meta"><div class="log-title">${e.title}</div><div class="log-detail">${e.venue||'—'}</div></div>
      <div class="log-right"><div class="log-date">${fmtDate(e.date)}</div></div>
    </div>`).join('') : '<div class="empty"><div class="empty-icon">📋</div>No entries yet.</div>';

  // Goal budget bars
  const goals=[
    {l:'Foundation Leadership',p:18,c:'#c8a96e',v:'$1.8M'},
    {l:'Equity & Inclusion',p:29,c:'#7c6fff',v:'$2.9M'},
    {l:'Policy Implementation',p:10,c:'#4ecfa4',v:'$1.0M'},
    {l:'Innovate & Challenge',p:14,c:'#e06b7d',v:'$1.4M'},
    {l:'APC Wind-Down',p:5,c:'#5bbfdf',v:'$0.5M'},
  ];
  document.getElementById('goal-list').innerHTML = goals.map(g=>`
    <div style="margin-bottom:11px;">
      <div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:3px;">
        <span>${g.l}</span><span style="color:${g.c};font-weight:600">${g.v}</span>
      </div>
      <div class="pbar"><div class="pfill" style="width:${g.p}%;background:${g.c};"></div></div>
    </div>`).join('');

  // ── SVG bar chart (no canvas needed) ──
  const now = new Date();
  const months=[];
  for(let i=11;i>=0;i--){
    const d=new Date(now.getFullYear(),now.getMonth()-i,1);
    months.push({
      label:d.toLocaleString('default',{month:'short'}),
      key:`${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}`
    });
  }
  const allAct=[...data.talks,...data.writing,...data.consulting].filter(e=>e&&e.date);
  const vals=months.map(m=>allAct.filter(e=>e.date.startsWith(m.key)).length);
  const maxV=Math.max(...vals,1);
  const W=520,H=190,pl=28,pr=8,pt=10,pb=28;
  const bw=Math.floor((W-pl-pr)/12)-3;
  const ch=H-pt-pb;
  let svgBars='', svgGrid='';
  for(let g=0;g<=maxV;g++){
    const gy=pt+ch-Math.round((g/maxV)*ch);
    svgGrid+=`<line x1="${pl}" x2="${W-pr}" y1="${gy}" y2="${gy}" stroke="rgba(255,255,255,0.05)" stroke-width="1"/>`;
    svgGrid+=`<text x="${pl-3}" y="${gy+3}" text-anchor="end" font-size="9" fill="#8886a0">${g}</text>`;
  }
  months.forEach((m,i)=>{
    const v=vals[i];
    const x=pl+i*((W-pl-pr)/12)+1;
    const bh=v===0?2:Math.max(3,Math.round((v/maxV)*ch));
    const y=pt+ch-bh;
    svgBars+=`<rect x="${x}" y="${y}" width="${bw}" height="${bh}" rx="2" fill="rgba(200,169,110,0.5)" stroke="rgba(200,169,110,0.75)" stroke-width="1"/>`;
    if(v>0) svgBars+=`<text x="${x+bw/2}" y="${y-3}" text-anchor="middle" font-size="9" fill="#c8a96e">${v}</text>`;
    svgBars+=`<text x="${x+bw/2}" y="${H-4}" text-anchor="middle" font-size="9" fill="#8886a0">${m.label}</text>`;
  });
  document.getElementById('bar-overview').innerHTML=
    `<svg viewBox="0 0 ${W} ${H}" xmlns="http://www.w3.org/2000/svg" style="width:100%;height:100%">${svgGrid}${svgBars}</svg>`;

  // ── SVG donut chart ──
  const counts=[data.talks.length,data.writing.length,data.consulting.length];
  const labels=['Talks, Panels & Interviews','Writing','Consultations'];
  const colors=['#c8a96e','#4ecfa4','#5bbfdf'];
  const tot=counts.reduce((a,b)=>a+b,0)||1;
  const cx=85,cy=85,R=68,ir=46;
  let ang=-Math.PI/2, paths='';
  counts.forEach((v,i)=>{
    const slice=(v/tot)*2*Math.PI;
    const end=ang+slice;
    const x1=cx+R*Math.cos(ang),y1=cy+R*Math.sin(ang);
    const x2=cx+R*Math.cos(end),y2=cy+R*Math.sin(end);
    const ix1=cx+ir*Math.cos(ang),iy1=cy+ir*Math.sin(ang);
    const ix2=cx+ir*Math.cos(end),iy2=cy+ir*Math.sin(end);
    const lg=slice>Math.PI?1:0;
    paths+=`<path d="M${ix1},${iy1}L${x1},${y1}A${R},${R} 0 ${lg},1 ${x2},${y2}L${ix2},${iy2}A${ir},${ir} 0 ${lg},0 ${ix1},${iy1}Z" fill="${colors[i]}" opacity="0.85"/>`;
    ang=end;
  });
  paths+=`<text x="${cx}" y="${cy-5}" text-anchor="middle" font-size="20" font-weight="700" fill="#f0eff5">${tot}</text>`;
  paths+=`<text x="${cx}" y="${cy+11}" text-anchor="middle" font-size="10" fill="#8886a0">TOTAL</text>`;
  let leg='';
  labels.forEach((l,i)=>{
    const ly=185+i*17;
    leg+=`<rect x="5" y="${ly-9}" width="9" height="9" rx="2" fill="${colors[i]}" opacity="0.85"/>`;
    leg+=`<text x="19" y="${ly}" font-size="10" fill="#8886a0">${l}: <tspan fill="#f0eff5" font-weight="500">${counts[i]}</tspan></text>`;
  });
  document.getElementById('donut-overview').innerHTML=
    `<svg viewBox="0 0 195 240" xmlns="http://www.w3.org/2000/svg" style="width:100%;max-width:195px;height:auto">${paths}${leg}</svg>`;
}

// ─── LOG LIST RENDERER ─────────────────────────────────────────────
function renderLogList(containerId, items, type) {
  const c=document.getElementById(containerId);
  if(!items.length){c.innerHTML='<div class="empty"><div class="empty-icon">📋</div>No entries found.</div>';return;}
  c.innerHTML=items.map(e=>`
    <div class="log-item">
      <div class="log-icon" style="background:${BGCOL[e.type]||'rgba(255,255,255,.06)'}">${ICONS[e.type]||'📌'}</div>
      <div class="log-meta">
        <div class="log-title">${e.title}</div>
        <div class="log-detail">${e.venue||'—'} · <span class="b b-gray" style="padding:1px 6px">${e.type}</span></div>
        ${e.notes?`<div class="log-note">${e.notes.substring(0,90)}${e.notes.length>90?'...':''}</div>`:''}
      </div>
      <div class="log-right">
        <div class="log-date">${fmtDate(e.date)}</div>
        <div class="log-actions">
          <button class="btn btn-g" style="padding:3px 9px;font-size:11px;" onclick="openEdit('${type}',${e.id})">✏️ Edit</button>
          <button class="btn btn-d" style="padding:3px 9px;font-size:11px;" onclick="del('${type}',${e.id})">Delete</button>
        </div>
      </div>
    </div>`).join('');
}

function applyFilters(items, searchId, ...selIds) {
  const q=(document.getElementById(searchId)?.value||'').toLowerCase();
  let r=items.filter(e=>e!=null);
  if(q) r=r.filter(e=>JSON.stringify(e).toLowerCase().includes(q));
  selIds.forEach(sid=>{
    const v=document.getElementById(sid)?.value||'';
    if(!v) return;
    if(sid.includes('yr')||sid.includes('year')) r=r.filter(e=>e.date?.startsWith(v));
    else r=r.filter(e=>Object.values(e).some(x=>String(x)===v));
  });
  return r;
}

function renderTalks() {
  const items=applyFilters(data.talks,'s-talks','f-talks-type','f-talks-yr');
  renderLogList('talks-list',items,'talks');
  const yr=String(new Date().getFullYear());
  document.getElementById('st-talks-total').textContent=data.talks.length;
  document.getElementById('st-talks-yr').textContent=data.talks.filter(e=>e.date?.startsWith(yr)).length;
  document.getElementById('st-talks-kt').textContent=data.talks.filter(e=>['Keynote','Conference Talk','Webinar'].includes(e.type)).length;
  document.getElementById('st-talks-p').textContent=data.talks.filter(e=>['Panelist','Moderator','Chair','Discussant'].includes(e.type)).length;
  document.getElementById('st-talks-i').textContent=data.talks.filter(e=>['Media Interview','Podcast','Quoted / Press'].includes(e.type)).length;
}
function renderWriting() {
  let items=applyFilters(data.writing,'s-writing','f-wr-type');
  const st=document.getElementById('f-wr-status')?.value;
  if(st) items=items.filter(e=>e.reach===st);
  renderLogList('writing-list',items,'writing');
  document.getElementById('st-wr-total').textContent=data.writing.length;
  document.getElementById('st-wr-rep').textContent=data.writing.filter(e=>e.type==='Research Report').length;
  document.getElementById('st-wr-art').textContent=data.writing.filter(e=>['Article','Op-Ed'].includes(e.type)).length;
  document.getElementById('st-wr-blog').textContent=data.writing.filter(e=>e.type==='Blog Post').length;
}
function renderConsult() {
  const items=applyFilters(data.consulting,'s-consult','f-co-type');
  renderLogList('consult-list',items,'consulting');
  const now=new Date();
  const mon=`${now.getFullYear()}-${String(now.getMonth()+1).padStart(2,'0')}`;
  document.getElementById('st-co-total').textContent=data.consulting.length;
  document.getElementById('st-co-int').textContent=data.consulting.filter(e=>e.type==='Internal').length;
  document.getElementById('st-co-ext').textContent=data.consulting.filter(e=>e.type==='External').length;
  document.getElementById('st-co-mon').textContent=data.consulting.filter(e=>e.date?.startsWith(mon)).length;
}

// ─── BUDGET / PORTFOLIO (Chart.js) ─────────────────────────────────
function renderBudget() {
  if(_budgetChart) _budgetChart.destroy();
  if(_budgetPieChart) _budgetPieChart.destroy();
  _budgetChart=new Chart(document.getElementById('chart-budget').getContext('2d'),{
    type:'bar',
    data:{labels:[2022,2023,2024,2025,2026,2027,2028,2029],datasets:[
      {label:'Projects ($M)',data:[0.2,3.575,4.275,5.325,7.025,4.45,2.15,0.1],backgroundColor:'rgba(124,111,255,.6)',borderRadius:4},
      {label:'APC ($M)',data:[6.3,5.52,5.86,3.01,0.5,0.25,0.1,0.05],backgroundColor:'rgba(224,107,125,.5)',borderRadius:4},
      {label:'Misc ($M)',data:[1.2,1.2,1.2,1.2,1.5,1.5,1.5,1.5],backgroundColor:'rgba(91,191,223,.4)',borderRadius:4},
      {label:'Budget Cap',data:[10,10,10,10,10,10,10,10],type:'line',borderColor:'rgba(200,169,110,.6)',backgroundColor:'transparent',borderWidth:2,borderDash:[5,3],pointRadius:3,pointBackgroundColor:'#c8a96e'}
    ]},
    options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{labels:{color:'#8886a0',font:{size:11},padding:9,boxWidth:11}}},scales:{x:{stacked:true,grid:{color:'rgba(255,255,255,.04)'},ticks:{color:'#8886a0'}},y:{stacked:true,grid:{color:'rgba(255,255,255,.04)'},ticks:{color:'#8886a0',callback:v=>'$'+v+'M'}}}}
  });
  _budgetPieChart=new Chart(document.getElementById('chart-budget-pie').getContext('2d'),{
    type:'doughnut',
    data:{labels:['Projects','APC','Misc','Available'],datasets:[{data:[7.025,0.5,1.5,0.975],backgroundColor:['rgba(124,111,255,.7)','rgba(224,107,125,.6)','rgba(91,191,223,.6)','rgba(78,207,164,.5)'],borderColor:'#17171f',borderWidth:3,hoverOffset:6}]},
    options:{responsive:true,maintainAspectRatio:false,cutout:'60%',plugins:{legend:{position:'bottom',labels:{color:'#8886a0',font:{size:11},padding:9,boxWidth:11}},tooltip:{callbacks:{label:c=>` ${c.label}: $${c.raw}M`}}}}
  });
  renderInvTable();
}

// ─── INVESTMENT TABLE (editable + reorderable) ───────────────────────
const STATUS_OPTS = ['Yes','Yes - in progress','Yes - likely to renew','Probable','Under Consideration','Closed','No'];
const IMPACT_OPTS = ['Large','Medium','Small',''];
const STATUS_BADGE = {'Yes':'b-green','Yes - in progress':'b-blue','Yes - likely to renew':'b-purple','Probable':'b-gold','Under Consideration':'b-gray','Closed':'b-gray','No':'b-red'};

function renderInvTable() {
  const q=(document.getElementById('s-inv')?.value||'').toLowerCase();
  const st=document.getElementById('f-inv-status')?.value||'';
  const geo=document.getElementById('f-inv-geo')?.value||'';

  // Build filtered index list (we operate on investments array by index)
  const indices = investments.reduce((acc,inv,i)=>{
    if(q && !JSON.stringify(inv).toLowerCase().includes(q)) return acc;
    if(st && inv.decision!==st) return acc;
    if(geo && inv.loc!==geo) return acc;
    acc.push(i);
    return acc;
  },[]);

  const tbody = document.getElementById('inv-tbody');
  tbody.innerHTML = indices.map((idx,pos) => {
    const inv = investments[idx];
    const isFirst = pos===0;
    const isLast  = pos===indices.length-1;
    return `<tr data-idx="${idx}">
      <td><span contenteditable="true" class="ec" onblur="saveCell(${idx},'name',this.textContent.trim())" style="font-weight:500;display:block;min-width:140px">${inv.name}</span></td>
      <td><span contenteditable="true" class="ec" onblur="saveCell(${idx},'type',this.textContent.trim())" style="font-size:12px;color:var(--muted);display:block;min-width:110px">${inv.type}</span></td>
      <td>
        <select class="ec-sel" onchange="saveCell(${idx},'decision',this.value);renderInvTable()">
          ${STATUS_OPTS.map(o=>`<option${inv.decision===o?' selected':''}>${o}</option>`).join('')}
        </select>
      </td>
      <td><span contenteditable="true" class="ec" onblur="saveCell(${idx},'loc',this.textContent.trim())" style="color:var(--muted);display:block;min-width:60px">${inv.loc}</span></td>
      <td><span contenteditable="true" class="ec ec-num" onblur="saveCell(${idx},'y2025',parseNum(this.textContent))" style="color:var(--gold);display:block">${inv.y2025||0}</span></td>
      <td><span contenteditable="true" class="ec ec-num" onblur="saveCell(${idx},'y2026',parseNum(this.textContent))" style="color:var(--gold);display:block">${inv.y2026||0}</span></td>
      <td><span contenteditable="true" class="ec ec-num" onblur="saveCell(${idx},'y2027',parseNum(this.textContent))" style="color:var(--gold);display:block">${inv.y2027||0}</span></td>
      <td>
        <select class="ec-sel" onchange="saveCell(${idx},'impact',this.value)">
          ${IMPACT_OPTS.map(o=>`<option${inv.impact===o?' selected':''}>${o}</option>`).join('')}
        </select>
      </td>
      <td><span contenteditable="true" class="ec" onblur="saveCell(${idx},'geo',this.textContent.trim())" style="font-size:12px;color:var(--muted);display:block;min-width:80px">${inv.geo||''}</span></td>
      <td>
        <div style="display:flex;flex-direction:column;gap:2px;">
          <button class="mv-btn" onclick="moveInv(${idx},-1)" ${isFirst?'disabled':''} title="Move up">▲</button>
          <button class="mv-btn" onclick="moveInv(${idx},1)"  ${isLast?'disabled':''}  title="Move down">▼</button>
        </div>
      </td>
    </tr>`;
  }).join('');
}

function saveCell(idx, field, value) {
  investments[idx][field] = value;
}
function parseNum(s) {
  const n = parseFloat(String(s).replace(/[^0-9.]/g,''));
  return isNaN(n) ? 0 : n;
}
function moveInv(idx, dir) {
  const target = idx + dir;
  if(target < 0 || target >= investments.length) return;
  const tmp = investments[idx];
  investments[idx] = investments[target];
  investments[target] = tmp;
  renderInvTable();
}

function setTab(tab,el){
  portfolioTab=tab;
  document.querySelectorAll('.ptab').forEach(t=>t.classList.remove('active'));
  el.classList.add('active');
  renderPortfolio();
}
function renderPortfolio(){
  const filtered=investments.filter(i=>i.status===portfolioTab);
  document.getElementById('portfolio-list').innerHTML=filtered.length?
    `<table class="tbl"><thead><tr><th>Partner</th><th>Org Type</th><th>Location</th><th>2026 Investment</th><th>Status</th><th>Geo Reach</th></tr></thead><tbody>${
      filtered.map(inv=>`<tr><td style="font-weight:500">${inv.name}</td><td style="color:var(--muted);font-size:12px">${inv.type}</td><td>${inv.loc}</td><td style="color:var(--gold);font-weight:500">${fmtMoney(inv.y2026)||'—'}</td><td><span class="b b-${inv.status==='active'?'green':inv.status==='pipeline'?'gold':'gray'}">${inv.decision}</span></td><td style="font-size:12px;color:var(--muted)">${inv.geo||'—'}</td></tr>`).join('')
    }</tbody></table>`:
    '<div class="empty"><div class="empty-icon">🌐</div>No partners here.</div>';

  if(_orgChart) _orgChart.destroy();
  if(_geoChart) _geoChart.destroy();
  const orgC={};
  investments.filter(i=>i.status==='active').forEach(i=>{orgC[i.type]=(orgC[i.type]||0)+1;});
  _orgChart=new Chart(document.getElementById('chart-org').getContext('2d'),{
    type:'bar',
    data:{labels:Object.keys(orgC).map(l=>l.length>18?l.slice(0,18)+'…':l),datasets:[{data:Object.values(orgC),backgroundColor:'rgba(124,111,255,.55)',borderColor:'rgba(124,111,255,.8)',borderWidth:1.5,borderRadius:4}]},
    options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:'rgba(255,255,255,.04)'},ticks:{color:'#8886a0',stepSize:1}},y:{grid:{color:'rgba(255,255,255,.02)'},ticks:{color:'#8886a0',font:{size:10}}}}}
  });
  const geoC={};
  investments.filter(i=>i.status==='active'&&i.loc).forEach(i=>{geoC[i.loc]=(geoC[i.loc]||0)+1;});
  _geoChart=new Chart(document.getElementById('chart-geo').getContext('2d'),{
    type:'doughnut',
    data:{labels:Object.keys(geoC),datasets:[{data:Object.values(geoC),backgroundColor:['rgba(200,169,110,.7)','rgba(124,111,255,.7)','rgba(78,207,164,.7)','rgba(91,191,223,.7)','rgba(224,107,125,.6)'],borderColor:'#17171f',borderWidth:3,hoverOffset:5}]},
    options:{responsive:true,maintainAspectRatio:false,cutout:'55%',plugins:{legend:{position:'bottom',labels:{color:'#8886a0',font:{size:11},padding:8,boxWidth:11}}}}
  });
}

// ─── MODALS ─────────────────────────────────────────────────────────
const FORM_FIELDS = {
  talks: [
    {id:'m-title',label:'Title / Topic',ph:'Talk title or topic',full:false},
    {id:'m-type',label:'Type',type:'select',opts:['Keynote','Conference Talk','Panelist','Moderator','Chair','Discussant','Media Interview','Podcast','Webinar','Quoted / Press','Peer Reviewer','Other'],full:false},
    {id:'m-venue',label:'Event / Venue',ph:'Conference or platform',full:false},
    {id:'m-date',label:'Date',type:'date',full:false},
    {id:'m-reach',label:'Audience / Context',ph:'e.g. 200 attendees',full:false},
    {id:'m-link',label:'Link / Recording',ph:'https://...',full:false},
    {id:'m-notes',label:'Notes',type:'textarea',ph:'Key themes, outcomes...',full:true},
  ],
  writing: [
    {id:'m-title',label:'Title',ph:'Article or report title',full:false},
    {id:'m-type',label:'Type',type:'select',opts:['Research Report','Article','Op-Ed','Blog Post','Policy Brief','Other'],full:false},
    {id:'m-venue',label:'Publication / Platform',ph:'Where published?',full:false},
    {id:'m-date',label:'Date',type:'date',full:false},
    {id:'m-reach',label:'Status',type:'select',opts:['Published','In Review','Draft'],full:false},
    {id:'m-link',label:'Link',ph:'https://...',full:false},
    {id:'m-notes',label:'Notes',type:'textarea',ph:'Abstract or key findings...',full:true},
  ],
  consulting: [
    {id:'m-title',label:'Topic / Request',ph:'What were you consulted on?',full:false},
    {id:'m-type',label:'Type',type:'select',opts:['Internal','External'],full:false},
    {id:'m-venue',label:'Requesting Team / Org',ph:'Who requested it?',full:false},
    {id:'m-date',label:'Date',type:'date',full:false},
    {id:'m-reach',label:'Duration (hrs)',type:'number',ph:'e.g. 2',full:false},
    {id:'m-link',label:'Follow-up / Outcome',ph:'Action items or decision',full:false},
    {id:'m-notes',label:'Notes',type:'textarea',ph:'Context, recommendations...',full:true},
  ]
};

function buildForm(type) {
  const today=new Date().toISOString().split('T')[0];
  const fields=FORM_FIELDS[type];
  let html='<div class="fg">';
  fields.forEach(f=>{
    const cls=f.full?'fgfull':'';
    html+=`<div class="${cls}"><label class="flabel">${f.label}</label>`;
    if(f.type==='select') html+=`<select class="fsel" id="${f.id}">${f.opts.map(o=>`<option>${o}</option>`).join('')}</select>`;
    else if(f.type==='textarea') html+=`<textarea class="ftxt" id="${f.id}" placeholder="${f.ph||''}"></textarea>`;
    else if(f.type==='date') html+=`<input class="fin" type="date" id="${f.id}" value="${today}">`;
    else if(f.type==='number') html+=`<input class="fin" type="number" id="${f.id}" placeholder="${f.ph||''}" step="0.5">`;
    else html+=`<input class="fin" id="${f.id}" placeholder="${f.ph||''}">`;
    html+='</div>';
  });
  html+='</div>';
  return html;
}

function openAdd(type) {
  const titles={talks:'Add Talk, Panel or Interview',writing:'Add Writing',consulting:'Add Consultation'};
  document.getElementById('modal-body').innerHTML=`
    <div class="modal-hd"><div class="modal-title">${titles[type]}</div><button class="modal-x" onclick="closeModal()">✕</button></div>
    ${buildForm(type)}
    <div class="btn-row"><button class="btn btn-p" onclick="saveNew('${type}')">Save</button><button class="btn btn-g" onclick="closeModal()">Cancel</button></div>`;
  document.getElementById('overlay').classList.add('open');
}

function openEdit(type,id) {
  const e=data[type].find(x=>x.id===id);
  if(!e) return;
  const titles={talks:'Edit Entry',writing:'Edit Writing',consulting:'Edit Consultation'};
  document.getElementById('modal-body').innerHTML=`
    <div class="modal-hd"><div class="modal-title">${titles[type]}</div><button class="modal-x" onclick="closeModal()">✕</button></div>
    ${buildForm(type)}
    <div class="btn-row"><button class="btn btn-p" onclick="saveEdit('${type}',${id})">Save Changes</button><button class="btn btn-g" onclick="closeModal()">Cancel</button></div>`;
  document.getElementById('overlay').classList.add('open');
  setTimeout(()=>{
    ['title','type','venue','date','reach','link','notes'].forEach(k=>{
      const el=document.getElementById('m-'+k);
      if(el) el.value=e[k]||'';
    });
  },20);
}

function closeModal() { document.getElementById('overlay').classList.remove('open'); }

function saveNew(type) {
  const title=document.getElementById('m-title')?.value;
  const date=document.getElementById('m-date')?.value;
  if(!title||!date){toast('⚠️ Title and date are required.');return;}
  data[type].push({
    id:Date.now(),
    title,
    type:document.getElementById('m-type')?.value,
    venue:document.getElementById('m-venue')?.value||'',
    date,
    reach:document.getElementById('m-reach')?.value||'',
    link:document.getElementById('m-link')?.value||'',
    notes:document.getElementById('m-notes')?.value||''
  });
  closeModal(); toast('✅ Entry saved!');
  if(type==='talks') renderTalks();
  if(type==='writing') renderWriting();
  if(type==='consulting') renderConsult();
  updateStats();
}

function saveEdit(type,id) {
  const idx=data[type].findIndex(e=>e.id===id);
  if(idx===-1) return;
  data[type][idx]={...data[type][idx],
    title:document.getElementById('m-title')?.value||'',
    type:document.getElementById('m-type')?.value||'',
    venue:document.getElementById('m-venue')?.value||'',
    date:document.getElementById('m-date')?.value||'',
    reach:document.getElementById('m-reach')?.value||'',
    link:document.getElementById('m-link')?.value||'',
    notes:document.getElementById('m-notes')?.value||''
  };
  closeModal(); toast('✅ Entry updated!');
  if(type==='talks') renderTalks();
  if(type==='writing') renderWriting();
  if(type==='consulting') renderConsult();
  updateStats();
}

function del(type,id) {
  data[type]=data[type].filter(e=>e.id!==id);
  if(type==='talks') renderTalks();
  if(type==='writing') renderWriting();
  if(type==='consulting') renderConsult();
  updateStats(); toast('🗑 Entry removed.');
}

// ─── EXPORT ─────────────────────────────────────────────────────────
function openExport() {
  document.getElementById('modal-body').innerHTML=`
    <div class="modal-hd"><div class="modal-title">Export to Excel</div><button class="modal-x" onclick="closeModal()">✕</button></div>
    <p style="color:var(--muted);font-size:13px;margin-bottom:18px;line-height:1.6;">Choose sections to include — each becomes its own sheet.</p>
    <div style="display:flex;flex-direction:column;gap:10px;margin-bottom:20px;">
      <label style="display:flex;align-items:center;gap:9px;cursor:pointer;font-size:13px;"><input type="checkbox" id="ex-talks" checked style="accent-color:#c8a96e;width:14px;height:14px;"> 🎤 Talks, Panels & Interviews (${data.talks.length})</label>
      <label style="display:flex;align-items:center;gap:9px;cursor:pointer;font-size:13px;"><input type="checkbox" id="ex-writing" checked style="accent-color:#c8a96e;width:14px;height:14px;"> ✍️ Writing (${data.writing.length})</label>
      <label style="display:flex;align-items:center;gap:9px;cursor:pointer;font-size:13px;"><input type="checkbox" id="ex-consult" checked style="accent-color:#c8a96e;width:14px;height:14px;"> 💡 Consultations (${data.consulting.length})</label>
      <label style="display:flex;align-items:center;gap:9px;cursor:pointer;font-size:13px;"><input type="checkbox" id="ex-inv" checked style="accent-color:#c8a96e;width:14px;height:14px;"> 📊 Investments (${investments.length})</label>
    </div>
    <div class="btn-row"><button class="btn btn-p" onclick="runExport()">⬇ Download Excel</button><button class="btn btn-g" onclick="closeModal()">Cancel</button></div>`;
  document.getElementById('overlay').classList.add('open');
}

function runExport() {
  if(typeof XLSX==='undefined'){
    const s=document.createElement('script');
    s.src='https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js';
    s.onload=doExport; s.onerror=()=>toast('⚠️ Could not load export library.');
    document.head.appendChild(s);
  } else doExport();
}

function doExport() {
  const wb=XLSX.utils.book_new();
  const aw=(ws)=>{const rng=XLSX.utils.decode_range(ws['!ref']);const cols=[];for(let C=rng.s.c;C<=rng.e.c;C++){let mx=10;for(let R=rng.s.r;R<=rng.e.r;R++){const cell=ws[XLSX.utils.encode_cell({r:R,c:C})];if(cell&&cell.v)mx=Math.max(mx,String(cell.v).length);}cols.push({wch:Math.min(mx+2,50)});}ws['!cols']=cols;};
  if(document.getElementById('ex-talks')?.checked&&data.talks.length){const ws=XLSX.utils.json_to_sheet(data.talks.map(e=>({'Title':e.title,'Type':e.type,'Venue':e.venue,'Date':e.date,'Audience':e.reach,'Link':e.link,'Notes':e.notes})));aw(ws);XLSX.utils.book_append_sheet(wb,ws,'Talks, Panels & Interviews');}
  if(document.getElementById('ex-writing')?.checked&&data.writing.length){const ws=XLSX.utils.json_to_sheet(data.writing.map(e=>({'Title':e.title,'Type':e.type,'Publication':e.venue,'Date':e.date,'Status':e.reach,'Link':e.link,'Notes':e.notes})));aw(ws);XLSX.utils.book_append_sheet(wb,ws,'Writing');}
  if(document.getElementById('ex-consult')?.checked&&data.consulting.length){const ws=XLSX.utils.json_to_sheet(data.consulting.map(e=>({'Topic':e.title,'Type':e.type,'Org':e.venue,'Date':e.date,'Duration':e.reach,'Follow-up':e.link,'Notes':e.notes})));aw(ws);XLSX.utils.book_append_sheet(wb,ws,'Consultations');}
  if(document.getElementById('ex-inv')?.checked){const ws=XLSX.utils.json_to_sheet(investments.map(i=>({'Partner':i.name,'Org Type':i.type,'Decision':i.decision,'Location':i.loc,'2025':i.y2025||0,'2026':i.y2026||0,'2027':i.y2027||0,'Impact':i.impact,'Geo':i.geo,'Status':i.status})));aw(ws);XLSX.utils.book_append_sheet(wb,ws,'Investments');}
  if(!wb.SheetNames.length){toast('⚠️ Select at least one section.');return;}
  XLSX.writeFile(wb,`Ashley_Dashboard_${new Date().toISOString().split('T')[0]}.xlsx`);
  closeModal(); toast('✅ Excel downloaded!');
}

// ─── INIT ────────────────────────────────────────────────────────────
document.getElementById('today-date').textContent=new Date().toLocaleDateString('en-US',{weekday:'long',year:'numeric',month:'long',day:'numeric'});

// Load Chart.js then render everything
const cjsScript = document.createElement('script');
cjsScript.src = 'https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js';
cjsScript.onload = function() { drawOverviewCharts(); };
document.head.appendChild(cjsScript);
</script>
</body>
</html>
