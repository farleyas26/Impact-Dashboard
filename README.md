<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ashley's Impact & Investment Dashboard</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;600;700&family=DM+Sans:wght@300;400;500;600&display=swap');
:root{--bg:#0f0f14;--surface:#17171f;--surface2:#1e1e28;--surface3:#25252f;--border:rgba(255,255,255,0.07);--text:#f0eff5;--muted:#8886a0;--accent:#c8a96e;--accent2:#7c6fff;--accent3:#4ecfa4;--accent4:#e06b7d;--accent5:#5bbfdf;}
*{box-sizing:border-box;margin:0;padding:0;}
body{background:var(--bg);color:var(--text);font-family:'DM Sans',sans-serif;font-size:14px;min-height:100vh;overflow-x:hidden;}
body::before{content:'';position:fixed;top:-200px;left:-200px;width:600px;height:600px;background:radial-gradient(circle,rgba(124,111,255,0.07) 0%,transparent 70%);pointer-events:none;z-index:0;}
body::after{content:'';position:fixed;bottom:-200px;right:-200px;width:700px;height:700px;background:radial-gradient(circle,rgba(200,169,110,0.06) 0%,transparent 70%);pointer-events:none;z-index:0;}
.shell{position:relative;z-index:1;display:grid;grid-template-columns:220px 1fr;min-height:100vh;}
.sidebar{background:var(--surface);border-right:1px solid var(--border);padding:32px 0;display:flex;flex-direction:column;position:sticky;top:0;height:100vh;overflow-y:auto;}
.sidebar-logo{padding:0 24px 28px;border-bottom:1px solid var(--border);margin-bottom:20px;}
.sidebar-logo .name{font-family:'Playfair Display',serif;font-size:18px;font-weight:700;color:var(--accent);letter-spacing:.02em;line-height:1.2;}
.sidebar-logo .role{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.1em;margin-top:4px;}
.nav-section{padding:0 16px;margin-bottom:8px;}
.nav-label{font-size:10px;text-transform:uppercase;letter-spacing:.12em;color:var(--muted);padding:8px 8px 6px;}
.nav-item{display:flex;align-items:center;gap:10px;padding:9px 12px;border-radius:8px;cursor:pointer;color:var(--muted);font-size:13.5px;font-weight:400;transition:all .2s;border:1px solid transparent;}
.nav-item:hover{background:var(--surface2);color:var(--text);}
.nav-item.active{background:linear-gradient(135deg,rgba(200,169,110,.12),rgba(124,111,255,.08));border-color:rgba(200,169,110,.2);color:var(--accent);font-weight:500;}
.nav-icon{font-size:15px;width:18px;text-align:center;}
.sidebar-footer{margin-top:auto;padding:20px 24px 0;border-top:1px solid var(--border);}
.sidebar-footer .year-badge{font-size:11px;color:var(--muted);text-transform:uppercase;letter-spacing:.1em;}
.main{padding:36px 40px;overflow-y:auto;}
.page{display:none;animation:fadeUp .3s ease;}
.page.active{display:block;}
@keyframes fadeUp{from{opacity:0;transform:translateY(12px)}to{opacity:1;transform:translateY(0)}}
.page-header{display:flex;align-items:flex-end;justify-content:space-between;margin-bottom:32px;}
.page-title{font-family:'Playfair Display',serif;font-size:28px;font-weight:600;line-height:1.1;color:var(--text);}
.page-subtitle{font-size:13px;color:var(--muted);margin-top:4px;}
.stats-row{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:16px;margin-bottom:28px;}
.stat-card{background:var(--surface);border:1px solid var(--border);border-radius:12px;padding:20px;position:relative;overflow:hidden;transition:border-color .2s;}
.stat-card:hover{border-color:rgba(200,169,110,.25);}
.stat-card::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;background:var(--c,var(--accent));opacity:.6;}
.stat-label{font-size:11px;text-transform:uppercase;letter-spacing:.1em;color:var(--muted);margin-bottom:8px;}
.stat-value{font-family:'Playfair Display',serif;font-size:30px;font-weight:700;color:var(--text);line-height:1;}
.stat-sub{font-size:12px;color:var(--muted);margin-top:5px;}
.grid-2{display:grid;grid-template-columns:1fr 1fr;gap:20px;margin-bottom:24px;}
.grid-3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:20px;margin-bottom:24px;}
.grid-65-35{display:grid;grid-template-columns:1.8fr 1fr;gap:20px;margin-bottom:24px;}
.card{background:var(--surface);border:1px solid var(--border);border-radius:14px;padding:22px;transition:border-color .2s;}
.card:hover{border-color:rgba(255,255,255,.12);}
.card-title{font-size:12px;text-transform:uppercase;letter-spacing:.1em;color:var(--muted);margin-bottom:18px;display:flex;align-items:center;gap:8px;}
.card-title span{color:var(--accent);}
.chart-wrap{position:relative;height:220px;}
.chart-wrap-tall{position:relative;height:280px;}
.data-table{width:100%;border-collapse:collapse;font-size:13px;}
.data-table th{text-align:left;font-size:10px;text-transform:uppercase;letter-spacing:.1em;color:var(--muted);padding:0 12px 10px 0;border-bottom:1px solid var(--border);font-weight:500;}
.data-table td{padding:10px 12px 10px 0;border-bottom:1px solid rgba(255,255,255,.04);color:var(--text);vertical-align:middle;}
.data-table tr:last-child td{border-bottom:none;}
.data-table tr:hover td{background:rgba(255,255,255,.02);}
.badge{display:inline-block;padding:3px 9px;border-radius:20px;font-size:11px;font-weight:500;letter-spacing:.03em;}
.badge-gold{background:rgba(200,169,110,.15);color:var(--accent);border:1px solid rgba(200,169,110,.25);}
.badge-purple{background:rgba(124,111,255,.15);color:var(--accent2);border:1px solid rgba(124,111,255,.25);}
.badge-green{background:rgba(78,207,164,.12);color:var(--accent3);border:1px solid rgba(78,207,164,.2);}
.badge-red{background:rgba(224,107,125,.12);color:var(--accent4);border:1px solid rgba(224,107,125,.2);}
.badge-blue{background:rgba(91,191,223,.12);color:var(--accent5);border:1px solid rgba(91,191,223,.2);}
.badge-gray{background:rgba(255,255,255,.06);color:var(--muted);border:1px solid rgba(255,255,255,.08);}
.form-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px;margin-bottom:16px;}
.form-full{grid-column:1/-1;}
.form-group{display:flex;flex-direction:column;gap:5px;}
.form-label{font-size:11px;text-transform:uppercase;letter-spacing:.08em;color:var(--muted);font-weight:500;}
.form-input,.form-select,.form-textarea{background:var(--surface2);border:1px solid var(--border);border-radius:8px;padding:9px 12px;color:var(--text);font-family:'DM Sans',sans-serif;font-size:13px;transition:border-color .2s;outline:none;width:100%;}
.form-input:focus,.form-select:focus,.form-textarea:focus{border-color:rgba(200,169,110,.4);}
.form-textarea{resize:vertical;min-height:70px;}
.form-select option{background:var(--surface2);}
.btn{padding:9px 20px;border-radius:8px;font-family:'DM Sans',sans-serif;font-size:13px;font-weight:500;cursor:pointer;border:none;transition:all .2s;}
.btn-primary{background:var(--accent);color:#0f0f14;}
.btn-primary:hover{background:#d4b97a;transform:translateY(-1px);}
.btn-ghost{background:transparent;color:var(--muted);border:1px solid var(--border);}
.btn-ghost:hover{background:var(--surface2);color:var(--text);}
.btn-danger{background:rgba(224,107,125,.15);color:var(--accent4);border:1px solid rgba(224,107,125,.25);}
.btn-danger:hover{background:rgba(224,107,125,.25);}
.btn-row{display:flex;gap:10px;margin-top:4px;}
.pill-tabs{display:flex;gap:8px;background:var(--surface2);border-radius:10px;padding:4px;margin-bottom:22px;width:fit-content;}
.pill-tab{padding:7px 16px;border-radius:7px;font-size:13px;cursor:pointer;color:var(--muted);font-weight:400;transition:all .2s;border:none;background:none;font-family:'DM Sans',sans-serif;}
.pill-tab.active{background:var(--surface);color:var(--text);font-weight:500;box-shadow:0 1px 6px rgba(0,0,0,.3);}
.log-item{display:flex;gap:14px;padding:14px 0;border-bottom:1px solid rgba(255,255,255,.04);align-items:flex-start;}
.log-item:last-child{border-bottom:none;}
.log-icon{width:36px;height:36px;border-radius:9px;display:flex;align-items:center;justify-content:center;font-size:15px;flex-shrink:0;}
.log-meta{flex:1;min-width:0;}
.log-title{font-size:13.5px;font-weight:500;color:var(--text);line-height:1.3;}
.log-detail{font-size:12px;color:var(--muted);margin-top:3px;}
.log-right{text-align:right;flex-shrink:0;}
.log-date{font-size:11px;color:var(--muted);}
.log-reach{font-size:12px;color:var(--accent3);margin-top:3px;}
.filter-row{display:flex;gap:10px;margin-bottom:20px;align-items:center;flex-wrap:wrap;}
.filter-row .form-select{width:auto;min-width:130px;}
.filter-row .form-input{width:200px;}
.spacer{flex:1;}
.scroll-list{max-height:320px;overflow-y:auto;padding-right:4px;}
.scroll-list::-webkit-scrollbar{width:3px;}
.scroll-list::-webkit-scrollbar-thumb{background:var(--surface3);border-radius:2px;}
.empty-state{text-align:center;padding:40px 20px;color:var(--muted);font-size:13px;}
.empty-icon{font-size:32px;margin-bottom:10px;opacity:.5;}
.modal-overlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,.7);z-index:100;align-items:center;justify-content:center;backdrop-filter:blur(4px);}
.modal-overlay.open{display:flex;}
.modal{background:var(--surface);border:1px solid var(--border);border-radius:16px;padding:28px;width:560px;max-width:95vw;max-height:90vh;overflow-y:auto;animation:fadeUp .25s ease;}
.modal-header{display:flex;align-items:center;justify-content:space-between;margin-bottom:22px;}
.modal-title{font-family:'Playfair Display',serif;font-size:20px;font-weight:600;}
.modal-close{cursor:pointer;font-size:20px;line-height:1;border:none;background:none;color:var(--muted);transition:color .2s;}
.modal-close:hover{color:var(--text);}
.toast-container{position:fixed;bottom:24px;right:24px;z-index:200;display:flex;flex-direction:column;gap:8px;}
.toast{background:var(--surface2);border:1px solid var(--border);border-radius:10px;padding:12px 18px;font-size:13px;color:var(--text);animation:slideIn .25s ease;display:flex;align-items:center;gap:8px;}
@keyframes slideIn{from{opacity:0;transform:translateX(20px)}to{opacity:1;transform:translateX(0)}}
.goal-card{background:var(--surface2);border-radius:10px;padding:16px;margin-bottom:12px;border-left:3px solid var(--c,var(--accent));}
.goal-name{font-size:13.5px;font-weight:500;margin-bottom:6px;}
.goal-descriptor{font-size:12px;color:var(--muted);line-height:1.5;}
.progress-bar{height:5px;background:var(--surface3);border-radius:3px;margin-top:8px;overflow:hidden;}
.progress-fill{height:100%;border-radius:3px;background:var(--accent);transition:width 1s ease;}
</style>
</head>
<body>
<div class="shell">

<!-- SIDEBAR -->
<aside class="sidebar">
  <div class="sidebar-logo">
    <div class="name">Ashley</div>
    <div class="role">Impact &amp; Investment Tracker</div>
  </div>
  <div class="nav-section">
    <div class="nav-label">Overview</div>
    <div class="nav-item active" onclick="goTo('overview',this)"><span class="nav-icon">◈</span> Dashboard</div>
  </div>
  <div class="nav-section">
    <div class="nav-label">My Impact</div>
    <div class="nav-item" onclick="goTo('talks',this)"><span class="nav-icon">🎤</span> Talks, Panels &amp; Interviews</div>
    <div class="nav-item" onclick="goTo('writing',this)"><span class="nav-icon">✍️</span> Writing</div>
    <div class="nav-item" onclick="goTo('consulting',this)"><span class="nav-icon">💡</span> Consultations</div>
  </div>
  <div class="nav-section">
    <div class="nav-label">Investments</div>
    <div class="nav-item" onclick="goTo('budget',this)"><span class="nav-icon">📊</span> Budget Tracker</div>
    <div class="nav-item" onclick="goTo('portfolio',this)"><span class="nav-icon">🌐</span> Portfolio</div>
    <div class="nav-item" onclick="goTo('strategy',this)"><span class="nav-icon">🗺️</span> Strategic Goals</div>
  </div>
  <div class="sidebar-footer">
    <div class="year-badge">2025 – 2027 Cycle</div>
    <div style="margin-top:14px;">
      <button class="btn btn-ghost" style="width:100%;font-size:12px;padding:8px 12px;" onclick="openExportModal()">⬇ Export to Excel</button>
    </div>
  </div>
</aside>

<!-- MAIN -->
<main class="main">

<!-- OVERVIEW -->
<div class="page active" id="page-overview">
  <div class="page-header">
    <div><div class="page-title">Good morning, Ashley ☀️</div><div class="page-subtitle">Here's your impact and investment snapshot</div></div>
    <div style="font-size:12px;color:var(--muted);" id="today-date"></div>
  </div>
  <div class="stats-row">
    <div class="stat-card" style="--c:var(--accent)"><div class="stat-label">Talks, Panels &amp; Interviews</div><div class="stat-value" id="ov-talks">0</div><div class="stat-sub">across all venues</div></div>
    <div class="stat-card" style="--c:var(--accent3)"><div class="stat-label">Pieces Written</div><div class="stat-value" id="ov-writing">0</div><div class="stat-sub">articles, reports &amp; posts</div></div>
    <div class="stat-card" style="--c:var(--accent5)"><div class="stat-label">Consultations</div><div class="stat-value" id="ov-consult">0</div><div class="stat-sub">internal &amp; external</div></div>
    <div class="stat-card" style="--c:var(--accent4)"><div class="stat-label">2026 Budget</div><div class="stat-value">$10M</div><div class="stat-sub">annual investment cap</div></div>
  </div>
  <div class="grid-65-35">
    <div class="card"><div class="card-title"><span>◈</span> Impact Activity — Last 12 Months</div><div class="chart-wrap"><canvas id="chart-activity"></canvas></div></div>
    <div class="card"><div class="card-title"><span>◈</span> Impact by Type</div><div class="chart-wrap"><canvas id="chart-types"></canvas></div></div>
  </div>
  <div class="grid-2">
    <div class="card"><div class="card-title"><span>◈</span> Recent Impact Entries</div><div class="scroll-list" id="recent-impact-list"><div class="empty-state"><div class="empty-icon">📋</div>No entries yet</div></div></div>
    <div class="card"><div class="card-title"><span>◈</span> 2026 Investment Forecast by Goal</div><div id="goal-budget-list"></div></div>
  </div>
</div>

<!-- TALKS PANELS INTERVIEWS -->
<div class="page" id="page-talks">
  <div class="page-header">
    <div><div class="page-title">Talks, Panels &amp; Interviews</div><div class="page-subtitle">Keynotes, panels, conference talks, media interviews, podcasts, press quotes</div></div>
    <button class="btn btn-primary" onclick="openModal('talks')">+ Add Entry</button>
  </div>
  <div class="stats-row">
    <div class="stat-card" style="--c:var(--accent)"><div class="stat-label">Total Entries</div><div class="stat-value" id="stat-talks-total">0</div></div>
    <div class="stat-card" style="--c:var(--accent2)"><div class="stat-label">This Year</div><div class="stat-value" id="stat-talks-year">0</div></div>
    <div class="stat-card" style="--c:var(--accent3)"><div class="stat-label">Keynotes &amp; Talks</div><div class="stat-value" id="stat-talks-keynote">0</div></div>
    <div class="stat-card" style="--c:var(--accent2)"><div class="stat-label">Panels &amp; Chairs</div><div class="stat-value" id="stat-talks-panels">0</div></div>
    <div class="stat-card" style="--c:var(--accent5)"><div class="stat-label">Interviews &amp; Press</div><div class="stat-value" id="stat-talks-interview">0</div></div>
  </div>
  <div class="filter-row">
    <select class="form-select" id="filter-talks-type" onchange="renderTalksList()">
      <option value="">All Types</option>
      <option>Keynote</option><option>Conference Talk</option>
      <option>Panelist</option><option>Moderator</option><option>Chair</option><option>Discussant</option>
      <option>Media Interview</option><option>Podcast</option><option>Webinar</option>
      <option>Quoted / Press</option><option>Peer Reviewer</option><option>Contributor</option><option>Other</option>
    </select>
    <select class="form-select" id="filter-talks-year" onchange="renderTalksList()">
      <option value="">All Years</option>
      <option>2022</option><option>2023</option><option>2024</option><option>2025</option><option>2026</option><option>2027</option>
    </select>
    <div class="spacer"></div>
    <input type="text" class="form-input" placeholder="🔍  Search..." id="search-talks" oninput="renderTalksList()">
  </div>
  <div class="card"><div id="talks-list"><div class="empty-state"><div class="empty-icon">🎤</div>No entries yet.</div></div></div>
</div>

<!-- WRITING -->
<div class="page" id="page-writing">
  <div class="page-header">
    <div><div class="page-title">Writing</div><div class="page-subtitle">Articles, reports, blog posts, op-eds, and policy briefs</div></div>
    <button class="btn btn-primary" onclick="openModal('writing')">+ Add Entry</button>
  </div>
  <div class="stats-row">
    <div class="stat-card" style="--c:var(--accent3)"><div class="stat-label">Total Pieces</div><div class="stat-value" id="stat-writing-total">0</div></div>
    <div class="stat-card" style="--c:var(--accent)"><div class="stat-label">Reports</div><div class="stat-value" id="stat-writing-reports">0</div></div>
    <div class="stat-card" style="--c:var(--accent2)"><div class="stat-label">Articles / Op-Eds</div><div class="stat-value" id="stat-writing-articles">0</div></div>
    <div class="stat-card" style="--c:var(--accent5)"><div class="stat-label">Blog Posts</div><div class="stat-value" id="stat-writing-blogs">0</div></div>
  </div>
  <div class="filter-row">
    <select class="form-select" id="filter-writing-type" onchange="renderWritingList()">
      <option value="">All Types</option>
      <option>Research Report</option><option>Article</option><option>Op-Ed</option>
      <option>Blog Post</option><option>Policy Brief</option><option>Other</option>
    </select>
    <select class="form-select" id="filter-writing-status" onchange="renderWritingList()">
      <option value="">All Status</option><option>Published</option><option>In Review</option><option>Draft</option>
    </select>
    <input type="text" class="form-input" placeholder="🔍  Search..." id="search-writing" oninput="renderWritingList()">
  </div>
  <div class="card"><div id="writing-list"><div class="empty-state"><div class="empty-icon">✍️</div>No writing entries yet.</div></div></div>
</div>

<!-- CONSULTING -->
<div class="page" id="page-consulting">
  <div class="page-header">
    <div><div class="page-title">Expert Consultations</div><div class="page-subtitle">Internal and external consulting, advisory, and expert input</div></div>
    <button class="btn btn-primary" onclick="openModal('consulting')">+ Add Entry</button>
  </div>
  <div class="stats-row">
    <div class="stat-card" style="--c:var(--accent5)"><div class="stat-label">Total</div><div class="stat-value" id="stat-consult-total">0</div></div>
    <div class="stat-card" style="--c:var(--accent)"><div class="stat-label">Internal</div><div class="stat-value" id="stat-consult-internal">0</div></div>
    <div class="stat-card" style="--c:var(--accent2)"><div class="stat-label">External</div><div class="stat-value" id="stat-consult-external">0</div></div>
    <div class="stat-card" style="--c:var(--accent3)"><div class="stat-label">This Month</div><div class="stat-value" id="stat-consult-month">0</div></div>
  </div>
  <div class="filter-row">
    <select class="form-select" id="filter-consult-type" onchange="renderConsultList()">
      <option value="">Internal &amp; External</option><option>Internal</option><option>External</option>
    </select>
    <input type="text" class="form-input" placeholder="🔍  Search..." id="search-consult" oninput="renderConsultList()">
  </div>
  <div class="card"><div id="consult-list"><div class="empty-state"><div class="empty-icon">💡</div>No consultations yet.</div></div></div>
</div>

<!-- BUDGET -->
<div class="page" id="page-budget">
  <div class="page-header"><div><div class="page-title">Budget Tracker</div><div class="page-subtitle">2022–2029 investment forecast from your planning file</div></div></div>
  <div class="stats-row">
    <div class="stat-card" style="--c:var(--accent)"><div class="stat-label">Annual Budget</div><div class="stat-value">$10M</div><div class="stat-sub">per year</div></div>
    <div class="stat-card" style="--c:var(--accent3)"><div class="stat-label">2026 Forecasted</div><div class="stat-value">$7.0M</div><div class="stat-sub">active investments</div></div>
    <div class="stat-card" style="--c:var(--accent5)"><div class="stat-label">2026 Available</div><div class="stat-value">$975K</div><div class="stat-sub">uncommitted</div></div>
    <div class="stat-card" style="--c:var(--accent4)"><div class="stat-label">APC Wind-Down</div><div class="stat-value">$500K</div><div class="stat-sub">2026 target</div></div>
  </div>
  <div class="grid-2">
    <div class="card"><div class="card-title"><span>◈</span> Investment Portfolio by Year ($M)</div><div class="chart-wrap-tall"><canvas id="chart-budget"></canvas></div></div>
    <div class="card"><div class="card-title"><span>◈</span> 2026 Budget Allocation</div><div class="chart-wrap-tall"><canvas id="chart-budget-pie"></canvas></div></div>
  </div>
  <div class="card">
    <div class="card-title"><span>◈</span> Investment Pipeline</div>
    <div class="filter-row" style="margin-bottom:14px;">
      <select class="form-select" id="filter-inv-status" onchange="renderInvTable()">
        <option value="">All Statuses</option>
        <option>Yes</option><option>Yes - in progress</option><option>Yes - likely to renew</option>
        <option>Probable</option><option>Under Consideration</option><option>Closed</option><option>No</option>
      </select>
      <select class="form-select" id="filter-inv-geo" onchange="renderInvTable()">
        <option value="">All Regions</option>
        <option>USA</option><option>UK</option><option>Europe</option><option>Africa</option><option>Canada</option>
      </select>
      <input type="text" class="form-input" placeholder="🔍  Search partner..." id="search-inv" oninput="renderInvTable()">
    </div>
    <div style="overflow-x:auto;">
      <table class="data-table">
        <thead><tr><th>Partner</th><th>Type</th><th>Decision</th><th>Location</th><th>2025 ($)</th><th>2026 ($)</th><th>2027 ($)</th><th>Impact</th><th>Geo Reach</th></tr></thead>
        <tbody id="inv-tbody"></tbody>
      </table>
    </div>
  </div>
</div>

<!-- PORTFOLIO -->
<div class="page" id="page-portfolio">
  <div class="page-header"><div><div class="page-title">Investment Portfolio</div><div class="page-subtitle">Full partner landscape</div></div></div>
  <div class="pill-tabs">
    <button class="pill-tab active" onclick="setPortfolioTab('active',this)">✅ Active</button>
    <button class="pill-tab" onclick="setPortfolioTab('pipeline',this)">🔄 Pipeline</button>
    <button class="pill-tab" onclick="setPortfolioTab('closed',this)">🔒 Closed / Declined</button>
  </div>
  <div class="grid-2">
    <div class="card"><div class="card-title"><span>◈</span> Partners by Organization Type</div><div class="chart-wrap"><canvas id="chart-org-type"></canvas></div></div>
    <div class="card"><div class="card-title"><span>◈</span> Geographic Distribution</div><div class="chart-wrap"><canvas id="chart-geo"></canvas></div></div>
  </div>
  <div class="card"><div id="portfolio-list"></div></div>
</div>

<!-- STRATEGY -->
<div class="page" id="page-strategy">
  <div class="page-header"><div><div class="page-title">Strategic Goals 2025–2027</div><div class="page-subtitle">Your four pillars and investment alignment</div></div></div>
  <div class="stats-row">
    <div class="stat-card" style="--c:var(--accent)"><div class="stat-label">Strategic Pillars</div><div class="stat-value">4</div></div>
    <div class="stat-card" style="--c:var(--accent2)"><div class="stat-label">Active Projects</div><div class="stat-value">20+</div></div>
    <div class="stat-card" style="--c:var(--accent3)"><div class="stat-label">3-Year Budget</div><div class="stat-value">$30M</div></div>
    <div class="stat-card" style="--c:var(--accent5)"><div class="stat-label">Partner Orgs</div><div class="stat-value">25+</div></div>
  </div>
  <div class="grid-2" style="margin-bottom:24px;">
    <div class="goal-card" style="--c:var(--accent)"><div class="goal-name">🏛️ Demonstrate Foundation Leadership</div><div class="goal-descriptor">Participate and lead in community efforts to affect change in the open research ecosystem.</div><div style="margin-top:10px;display:flex;gap:8px;flex-wrap:wrap;"><span class="badge badge-gold">cOAlition S</span><span class="badge badge-gold">ORFG</span><span class="badge badge-gold">Creative Commons</span><span class="badge badge-gold">ASAPbio</span></div><div style="margin-top:10px;font-size:12px;color:var(--muted);">18% of $30M · ~$5.4M</div><div class="progress-bar"><div class="progress-fill" style="width:18%;background:var(--accent);"></div></div></div>
    <div class="goal-card" style="--c:var(--accent2)"><div class="goal-name">🌍 Foster Equity &amp; Inclusion in Research</div><div class="goal-descriptor">Enable read/publish for all researchers globally. Support PRC models and diamond open access.</div><div style="margin-top:10px;display:flex;gap:8px;flex-wrap:wrap;"><span class="badge badge-purple">F1000/VeriXiv</span><span class="badge badge-purple">PLOS</span><span class="badge badge-purple">PREreview</span><span class="badge badge-purple">RR\ID</span></div><div style="margin-top:10px;font-size:12px;color:var(--muted);">29% of $30M · ~$8.7M</div><div class="progress-bar"><div class="progress-fill" style="width:29%;background:var(--accent2);"></div></div></div>
    <div class="goal-card" style="--c:var(--accent3)"><div class="goal-name">⚙️ Effective Policy Implementation</div><div class="goal-descriptor">Upgrade technology efficiency. Increase grant matching to 95%. Ensure outputs are broadly available.</div><div style="margin-top:10px;display:flex;gap:8px;flex-wrap:wrap;"><span class="badge badge-green">OA.Works</span><span class="badge badge-green">OpenAlex</span><span class="badge badge-green">Zendesk</span></div><div style="margin-top:10px;font-size:12px;color:var(--muted);">10% of $30M · ~$3M</div><div class="progress-bar"><div class="progress-fill" style="width:10%;background:var(--accent3);"></div></div></div>
    <div class="goal-card" style="--c:var(--accent4)"><div class="goal-name">🚀 Innovate &amp; Challenge the Status Quo</div><div class="goal-descriptor">Oversee policy shape and vision. Leverage AI for curation and peer review. New preprint models.</div><div style="margin-top:10px;display:flex;gap:8px;flex-wrap:wrap;"><span class="badge badge-red">PREreview</span><span class="badge badge-red">RR\ID</span><span class="badge badge-red">UNESCO</span></div><div style="margin-top:10px;font-size:12px;color:var(--muted);">14% of $30M · ~$4.2M</div><div class="progress-bar"><div class="progress-fill" style="width:14%;background:var(--accent4);"></div></div></div>
  </div>
  <div class="card">
    <div class="card-title"><span>◈</span> 5-Year Outcome Framework</div>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-top:4px;">
      <div style="background:var(--surface2);border-radius:10px;padding:16px;"><div style="font-size:12px;font-weight:600;color:var(--accent);margin-bottom:8px;">Leadership &amp; Policy Influence</div><div style="font-size:12px;color:var(--muted);line-height:1.6;">Evidence-driven policies adopted across key partner countries. Foundation recognized as trusted policy advisor.</div></div>
      <div style="background:var(--surface2);border-radius:10px;padding:16px;"><div style="font-size:12px;font-weight:600;color:var(--accent2);margin-bottom:8px;">Scientific Knowledge Ecosystem</div><div style="font-size:12px;color:var(--muted);line-height:1.6;">All funded outputs discoverable and systematically interlinked. Preprints treated as first-class research objects.</div></div>
      <div style="background:var(--surface2);border-radius:10px;padding:16px;"><div style="font-size:12px;font-weight:600;color:var(--accent3);margin-bottom:8px;">Policy Implementation &amp; Accountability</div><div style="font-size:12px;color:var(--muted);line-height:1.6;">Streamlined workflows and dashboards tracking implementation. Research impact measured by reuse and downstream uptake.</div></div>
      <div style="background:var(--surface2);border-radius:10px;padding:16px;"><div style="font-size:12px;font-weight:600;color:var(--accent4);margin-bottom:8px;">Innovation &amp; Systems Modernization</div><div style="font-size:12px;color:var(--muted);line-height:1.6;">Early adopters establish new models for open and interoperable practices. AI-enabled tools connect preprints, publications, datasets.</div></div>
    </div>
  </div>
</div>

</main>
</div>

<!-- MODAL -->
<div class="modal-overlay" id="modal-overlay" onclick="closeModal(event)">
  <div class="modal" id="modal-content"></div>
</div>
<div class="toast-container" id="toasts"></div>

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


let currentPortfolioTab = 'active';

// ── NAV ──
function goTo(page, el) {
  document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
  document.getElementById('page-' + page).classList.add('active');
  if (el) el.classList.add('active');
  if (page === 'overview')  renderOverview();
  if (page === 'talks')     renderTalksList();
  if (page === 'writing')   renderWritingList();
  if (page === 'consulting') renderConsultList();
  if (page === 'budget')    renderBudgetCharts();
  if (page === 'portfolio') renderPortfolio();
}

// ── MODAL ──
function openModal(type, existingId) {
  const titles = {talks:'Add Talk, Panel or Interview', writing:'Add Writing', consulting:'Add Consultation'};
  const editTitles = {talks:'Edit Entry', writing:'Edit Writing', consulting:'Edit Consultation'};
  const existing = existingId ? data[type].find(e => e.id === existingId) : null;
  const modal = document.getElementById('modal-content');
  modal.innerHTML = buildModalForm(type, existing ? editTitles[type] : titles[type], existing);
  if (existing) {
    modal.querySelector('.btn-primary').setAttribute('onclick', `saveEntry('${type}', ${existingId})`);
  }
  document.getElementById('modal-overlay').classList.add('open');
}
function closeModal(e) { if (e.target === document.getElementById('modal-overlay')) forceCloseModal(); }
function forceCloseModal() { document.getElementById('modal-overlay').classList.remove('open'); }

function buildModalForm(type, title, existing) {
  const today = new Date().toISOString().split('T')[0];
  const forms = {
    talks: `<div class="form-grid">
      <div class="form-group"><label class="form-label">Title / Topic</label><input class="form-input" id="m-title" placeholder="Talk title or topic"></div>
      <div class="form-group"><label class="form-label">Type</label><select class="form-select" id="m-type">
        <option>Keynote</option><option>Conference Talk</option><option>Panelist</option><option>Moderator</option>
        <option>Chair</option><option>Discussant</option><option>Media Interview</option><option>Podcast</option>
        <option>Webinar</option><option>Quoted / Press</option><option>Peer Reviewer</option><option>Other</option>
      </select></div>
      <div class="form-group"><label class="form-label">Event / Venue</label><input class="form-input" id="m-venue" placeholder="Conference or platform"></div>
      <div class="form-group"><label class="form-label">Date</label><input class="form-input" type="date" id="m-date" value="${today}"></div>
      <div class="form-group"><label class="form-label">Audience / Context</label><input class="form-input" id="m-reach" placeholder="e.g. 200 attendees, or co-panelists"></div>
      <div class="form-group"><label class="form-label">Link / Recording</label><input class="form-input" id="m-link" placeholder="https://..."></div>
      <div class="form-group form-full"><label class="form-label">Notes</label><textarea class="form-textarea" id="m-notes" placeholder="Key themes, outcomes, follow-ups..."></textarea></div>
    </div>`,
    writing: `<div class="form-grid">
      <div class="form-group"><label class="form-label">Title</label><input class="form-input" id="m-title" placeholder="Article or report title"></div>
      <div class="form-group"><label class="form-label">Type</label><select class="form-select" id="m-type">
        <option>Research Report</option><option>Article</option><option>Op-Ed</option>
        <option>Blog Post</option><option>Policy Brief</option><option>Other</option>
      </select></div>
      <div class="form-group"><label class="form-label">Publication / Platform</label><input class="form-input" id="m-venue" placeholder="Where published?"></div>
      <div class="form-group"><label class="form-label">Date</label><input class="form-input" type="date" id="m-date" value="${today}"></div>
      <div class="form-group"><label class="form-label">Status</label><select class="form-select" id="m-reach"><option>Published</option><option>In Review</option><option>Draft</option></select></div>
      <div class="form-group"><label class="form-label">Link</label><input class="form-input" id="m-link" placeholder="https://..."></div>
      <div class="form-group form-full"><label class="form-label">Notes</label><textarea class="form-textarea" id="m-notes" placeholder="Abstract or key findings..."></textarea></div>
    </div>`,
    consulting: `<div class="form-grid">
      <div class="form-group"><label class="form-label">Topic / Request</label><input class="form-input" id="m-title" placeholder="What were you consulted on?"></div>
      <div class="form-group"><label class="form-label">Type</label><select class="form-select" id="m-type"><option>Internal</option><option>External</option></select></div>
      <div class="form-group"><label class="form-label">Requesting Team / Org</label><input class="form-input" id="m-venue" placeholder="Who requested it?"></div>
      <div class="form-group"><label class="form-label">Date</label><input class="form-input" type="date" id="m-date" value="${today}"></div>
      <div class="form-group"><label class="form-label">Duration (hours)</label><input class="form-input" type="number" id="m-reach" placeholder="e.g. 2" step="0.5"></div>
      <div class="form-group"><label class="form-label">Follow-up / Outcome</label><input class="form-input" id="m-link" placeholder="Action items or decision"></div>
      <div class="form-group form-full"><label class="form-label">Notes</label><textarea class="form-textarea" id="m-notes" placeholder="Context, recommendations, key takeaways..."></textarea></div>
    </div>`
  };
  const html = `
    <div class="modal-header">
      <div class="modal-title">${title}</div>
      <button class="modal-close" onclick="forceCloseModal()">✕</button>
    </div>
    ${forms[type]}
    <div class="btn-row">
      <button class="btn btn-primary" onclick="saveEntry('${type}')">Save Entry</button>
      <button class="btn btn-ghost" onclick="forceCloseModal()">Cancel</button>
    </div>`;
  if (existing) {
    setTimeout(() => {
      if (document.getElementById('m-title'))  document.getElementById('m-title').value  = existing.title  || '';
      if (document.getElementById('m-type'))   document.getElementById('m-type').value   = existing.type   || '';
      if (document.getElementById('m-venue'))  document.getElementById('m-venue').value  = existing.venue  || '';
      if (document.getElementById('m-date'))   document.getElementById('m-date').value   = existing.date   || '';
      if (document.getElementById('m-reach'))  document.getElementById('m-reach').value  = existing.reach  || '';
      if (document.getElementById('m-link'))   document.getElementById('m-link').value   = existing.link   || '';
      if (document.getElementById('m-notes'))  document.getElementById('m-notes').value  = existing.notes  || '';
    }, 30);
  }
  return html;
}

function saveEntry(type, editId) {
  const title = document.getElementById('m-title')?.value;
  const eType = document.getElementById('m-type')?.value;
  const venue = document.getElementById('m-venue')?.value;
  const date  = document.getElementById('m-date')?.value;
  const reach = document.getElementById('m-reach')?.value;
  const link  = document.getElementById('m-link')?.value;
  const notes = document.getElementById('m-notes')?.value;
  if (!title || !date) { toast('⚠️ Please fill in title and date.'); return; }
  if (editId) {
    const idx = data[type].findIndex(e => e.id === editId);
    if (idx !== -1) data[type][idx] = {...data[type][idx], title, type: eType, venue, date, reach, link, notes};
    toast('✅ Entry updated!');
  } else {
    data[type].push({id: Date.now(), title, type: eType, venue, date, reach, link, notes});
    toast('✅ Entry saved!');
  }
  forceCloseModal();
  goTo(type, document.querySelector(`.nav-item[onclick="goTo('${type}',this)"]`));
  updateOverviewStats();
}

function deleteEntry(type, id) {
  data[type] = data[type].filter(e => e.id !== id);
  renderTalksList(); renderWritingList(); renderConsultList();
  updateOverviewStats();
  toast('🗑 Entry removed.');
}

// ── ICONS / COLOURS ──
const typeIconMap = {
  'Keynote':'🎯','Conference Talk':'🎤','Media Interview':'📺','Podcast':'🎧','Webinar':'💻',
  'Quoted / Press':'💬','Peer Reviewer':'🔍','Contributor':'🖊️',
  'Panelist':'🪑','Moderator':'🎙️','Chair':'👑','Discussant':'💬',
  'Research Report':'📄','Article':'📰','Op-Ed':'✍️','Blog Post':'📝','Policy Brief':'📋',
  'Internal':'🏛️','External':'🌐'
};
const typeBgMap = {
  'Keynote':'rgba(200,169,110,0.15)','Conference Talk':'rgba(200,169,110,0.1)',
  'Media Interview':'rgba(91,191,223,0.15)','Podcast':'rgba(91,191,223,0.1)',
  'Webinar':'rgba(124,111,255,0.12)',
  'Quoted / Press':'rgba(78,207,164,0.12)','Peer Reviewer':'rgba(91,191,223,0.1)','Contributor':'rgba(200,169,110,0.1)',
  'Panelist':'rgba(124,111,255,0.15)','Moderator':'rgba(78,207,164,0.12)',
  'Chair':'rgba(200,169,110,0.12)','Discussant':'rgba(91,191,223,0.1)',
  'Research Report':'rgba(78,207,164,0.12)','Article':'rgba(78,207,164,0.1)',
  'Op-Ed':'rgba(200,169,110,0.12)','Blog Post':'rgba(91,191,223,0.12)',
  'Policy Brief':'rgba(124,111,255,0.12)',
  'Internal':'rgba(91,191,223,0.15)','External':'rgba(78,207,164,0.12)'
};

// ── RENDER LOG LIST ──
function fmtDate(d) {
  if (!d) return '';
  return new Date(d + 'T00:00:00').toLocaleDateString('en-US',{month:'short',day:'numeric',year:'numeric'});
}
function fmtMoney(n) {
  if (!n) return '—';
  if (n >= 1000000) return '$' + (n/1000000).toFixed(1) + 'M';
  if (n >= 1000) return '$' + (n/1000).toFixed(0) + 'K';
  return '$' + n;
}

function renderLogList(container, items, type) {
  if (!items.length) {
    container.innerHTML = '<div class="empty-state"><div class="empty-icon">📋</div>No entries found.</div>';
    return;
  }
  container.innerHTML = items.map(e => `
    <div class="log-item">
      <div class="log-icon" style="background:${typeBgMap[e.type]||'rgba(255,255,255,0.06)'}">${typeIconMap[e.type]||'📌'}</div>
      <div class="log-meta">
        <div class="log-title">${e.title}</div>
        <div class="log-detail">${e.venue||'—'} · <span class="badge badge-gray" style="padding:2px 7px">${e.type}</span></div>
        ${e.notes ? `<div class="log-detail" style="margin-top:4px;font-style:italic;opacity:0.7">${e.notes.substring(0,80)}${e.notes.length>80?'...':''}</div>` : ''}
      </div>
      <div class="log-right">
        <div class="log-date">${fmtDate(e.date)}</div>
        <div class="log-reach">${e.reach ? (type==='consulting' ? e.reach+'h' : (type==='writing' ? e.reach : e.reach)) : ''}</div>
        <div style="display:flex;gap:6px;margin-top:8px;justify-content:flex-end;">
          <button class="btn btn-ghost" style="padding:4px 10px;font-size:11px;" onclick="openModal('${type}',${e.id})">✏️ Edit</button>
          <button class="btn btn-danger" style="padding:4px 10px;font-size:11px;" onclick="deleteEntry('${type}',${e.id})">Delete</button>
        </div>
      </div>
    </div>`).join('');
}

function filterItems(items, searchId, ...filterIds) {
  const q = document.getElementById(searchId)?.value.toLowerCase() || '';
  let filtered = items;
  if (q) filtered = filtered.filter(e => JSON.stringify(e).toLowerCase().includes(q));
  filterIds.forEach(fid => {
    const val = document.getElementById(fid)?.value;
    if (val) filtered = filtered.filter(e => Object.values(e).some(v => String(v) === val));
  });
  return filtered;
}

function renderTalksList() {
  const items = filterItems(data.talks, 'search-talks', 'filter-talks-type', 'filter-talks-year');
  renderLogList(document.getElementById('talks-list'), items, 'talks');
  const yr = new Date().getFullYear().toString();
  document.getElementById('stat-talks-total').textContent = data.talks.length;
  document.getElementById('stat-talks-year').textContent = data.talks.filter(e=>e.date?.startsWith(yr)).length;
  document.getElementById('stat-talks-keynote').textContent = data.talks.filter(e=>['Keynote','Conference Talk','Webinar'].includes(e.type)).length;
  document.getElementById('stat-talks-panels').textContent = data.talks.filter(e=>['Panelist','Moderator','Chair','Discussant'].includes(e.type)).length;
  document.getElementById('stat-talks-interview').textContent = data.talks.filter(e=>['Media Interview','Podcast','Quoted / Press'].includes(e.type)).length;
}

function renderWritingList() {
  const items = filterItems(data.writing, 'search-writing', 'filter-writing-type');
  const st = document.getElementById('filter-writing-status')?.value;
  const filtered = st ? items.filter(e=>e.reach===st) : items;
  renderLogList(document.getElementById('writing-list'), filtered, 'writing');
  document.getElementById('stat-writing-total').textContent = data.writing.length;
  document.getElementById('stat-writing-reports').textContent = data.writing.filter(e=>e.type==='Research Report').length;
  document.getElementById('stat-writing-articles').textContent = data.writing.filter(e=>['Article','Op-Ed'].includes(e.type)).length;
  document.getElementById('stat-writing-blogs').textContent = data.writing.filter(e=>e.type==='Blog Post').length;
}

function renderConsultList() {
  const items = filterItems(data.consulting, 'search-consult', 'filter-consult-type');
  renderLogList(document.getElementById('consult-list'), items, 'consulting');
  const now = new Date(); const mon = `${now.getFullYear()}-${String(now.getMonth()+1).padStart(2,'0')}`;
  document.getElementById('stat-consult-total').textContent = data.consulting.length;
  document.getElementById('stat-consult-internal').textContent = data.consulting.filter(e=>e.type==='Internal').length;
  document.getElementById('stat-consult-external').textContent = data.consulting.filter(e=>e.type==='External').length;
  document.getElementById('stat-consult-month').textContent = data.consulting.filter(e=>e.date?.startsWith(mon)).length;
}

// ── OVERVIEW ──
function updateOverviewStats() {
  document.getElementById('ov-talks').textContent = data.talks.length;
  document.getElementById('ov-writing').textContent = data.writing.length;
  document.getElementById('ov-consult').textContent = data.consulting.length;
}

let activityChart, typesChart;
function renderOverview() {
  updateOverviewStats();
  const allEntries = [
    ...data.talks.map(e=>({...e,cat:'talks'})),
    ...data.writing.map(e=>({...e,cat:'writing'})),
    ...data.consulting.map(e=>({...e,cat:'consulting'}))
  ].sort((a,b)=>new Date(b.date)-new Date(a.date)).slice(0,6);

  const rl = document.getElementById('recent-impact-list');
  rl.innerHTML = allEntries.length ? allEntries.map(e=>`
    <div class="log-item">
      <div class="log-icon" style="background:${typeBgMap[e.type]||'rgba(255,255,255,0.06)'}">${typeIconMap[e.type]||'📌'}</div>
      <div class="log-meta"><div class="log-title">${e.title}</div><div class="log-detail">${e.venue||'—'}</div></div>
      <div class="log-right"><div class="log-date">${fmtDate(e.date)}</div></div>
    </div>`).join('') : '<div class="empty-state"><div class="empty-icon">📋</div>No entries yet.</div>';

  const goals = [
    {label:'Foundation Leadership',pct:18,color:'var(--accent)',val:'$1.8M'},
    {label:'Equity & Inclusion',pct:29,color:'var(--accent2)',val:'$2.9M'},
    {label:'Policy Implementation',pct:10,color:'var(--accent3)',val:'$1.0M'},
    {label:'Innovate & Challenge',pct:14,color:'var(--accent4)',val:'$1.4M'},
    {label:'APC Wind-Down',pct:5,color:'var(--accent5)',val:'$0.5M'},
  ];
  document.getElementById('goal-budget-list').innerHTML = goals.map(g=>`
    <div style="margin-bottom:12px;">
      <div style="display:flex;justify-content:space-between;font-size:12px;margin-bottom:4px;">
        <span style="color:var(--text)">${g.label}</span><span style="color:${g.color};font-weight:600">${g.val}</span>
      </div>
      <div class="progress-bar"><div class="progress-fill" style="width:${g.pct}%;background:${g.color};"></div></div>
    </div>`).join('');

  const now = new Date();
  const months = [];
  for (let i=11;i>=0;i--) {
    const d = new Date(now.getFullYear(),now.getMonth()-i,1);
    months.push({label:d.toLocaleString('default',{month:'short'}),key:`${d.getFullYear()}-${String(d.getMonth()+1).padStart(2,'0')}`});
  }
  const allAct = [...data.talks,...data.writing,...data.consulting];
  const chartData = months.map(m=>allAct.filter(e=>e.date?.startsWith(m.key)).length);

  if (activityChart) activityChart.destroy();
  activityChart = new Chart(document.getElementById('chart-activity').getContext('2d'), {
    type:'bar',
    data:{labels:months.map(m=>m.label),datasets:[{label:'Activities',data:chartData,backgroundColor:'rgba(200,169,110,0.25)',borderColor:'rgba(200,169,110,0.7)',borderWidth:1.5,borderRadius:5}]},
    options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:'rgba(255,255,255,0.04)'},ticks:{color:'#8886a0',font:{size:11}}},y:{grid:{color:'rgba(255,255,255,0.04)'},ticks:{color:'#8886a0',font:{size:11},stepSize:1}}}}
  });

  if (typesChart) typesChart.destroy();
  const typeCounts = [data.talks.length,data.writing.length,data.consulting.length];
  typesChart = new Chart(document.getElementById('chart-types').getContext('2d'), {
    type:'doughnut',
    data:{labels:['Talks, Panels & Interviews','Writing','Consultations'],datasets:[{data:typeCounts.every(v=>v===0)?[1,1,1]:typeCounts,backgroundColor:['rgba(200,169,110,0.7)','rgba(78,207,164,0.7)','rgba(91,191,223,0.7)'],borderColor:'var(--surface)',borderWidth:3,hoverOffset:6}]},
    options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{position:'bottom',labels:{color:'#8886a0',font:{size:11},padding:12,boxWidth:12}}},cutout:'62%'}
  });
}

// ── BUDGET ──
let budgetChart, budgetPieChart;
function renderBudgetCharts() {
  if (budgetChart) budgetChart.destroy();
  budgetChart = new Chart(document.getElementById('chart-budget').getContext('2d'), {
    type:'bar',
    data:{
      labels:[2022,2023,2024,2025,2026,2027,2028,2029],
      datasets:[
        {label:'Project Investments ($M)',data:[0.2,3.575,4.275,5.325,7.025,4.45,2.15,0.1],backgroundColor:'rgba(124,111,255,0.6)',borderRadius:4},
        {label:'APC Payments ($M)',data:[6.3,5.52,5.86,3.01,0.5,0.25,0.1,0.05],backgroundColor:'rgba(224,107,125,0.5)',borderRadius:4},
        {label:'Misc / Other ($M)',data:[1.2,1.2,1.2,1.2,1.5,1.5,1.5,1.5],backgroundColor:'rgba(91,191,223,0.4)',borderRadius:4},
        {label:'Budget Cap ($M)',data:[10,10,10,10,10,10,10,10],type:'line',borderColor:'rgba(200,169,110,0.6)',backgroundColor:'transparent',borderWidth:2,borderDash:[5,3],pointRadius:3,pointBackgroundColor:'var(--accent)'}
      ]
    },
    options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{labels:{color:'#8886a0',font:{size:11},padding:10,boxWidth:12}}},scales:{x:{stacked:true,grid:{color:'rgba(255,255,255,0.04)'},ticks:{color:'#8886a0'}},y:{stacked:true,grid:{color:'rgba(255,255,255,0.04)'},ticks:{color:'#8886a0',callback:v=>'$'+v+'M'}}}}
  });

  if (budgetPieChart) budgetPieChart.destroy();
  budgetPieChart = new Chart(document.getElementById('chart-budget-pie').getContext('2d'), {
    type:'doughnut',
    data:{labels:['Project Investments','APC','Misc/Other','Available'],datasets:[{data:[7.025,0.5,1.5,0.975],backgroundColor:['rgba(124,111,255,0.7)','rgba(224,107,125,0.6)','rgba(91,191,223,0.6)','rgba(78,207,164,0.5)'],borderColor:'var(--surface)',borderWidth:3,hoverOffset:6}]},
    options:{responsive:true,maintainAspectRatio:false,cutout:'60%',plugins:{legend:{position:'bottom',labels:{color:'#8886a0',font:{size:11},padding:10,boxWidth:12}},tooltip:{callbacks:{label:ctx=>` ${ctx.label}: $${ctx.raw}M`}}}}
  });
  renderInvTable();
}

function renderInvTable() {
  const q = document.getElementById('search-inv')?.value.toLowerCase()||'';
  const st = document.getElementById('filter-inv-status')?.value||'';
  const geo = document.getElementById('filter-inv-geo')?.value||'';
  const statusColors = {'Yes':'badge-green','Yes - in progress':'badge-blue','Yes - likely to renew':'badge-purple','Probable':'badge-gold','Under Consideration':'badge-gray','Closed':'badge-gray','No':'badge-red'};
  const impactColor = {'Large':'badge-gold','Medium':'badge-purple','Small':'badge-blue','':'badge-gray'};
  const rows = investments.filter(inv=>{
    if (q && !JSON.stringify(inv).toLowerCase().includes(q)) return false;
    if (st && inv.decision!==st) return false;
    if (geo && inv.loc!==geo) return false;
    return true;
  });
  document.getElementById('inv-tbody').innerHTML = rows.map(inv=>`
    <tr>
      <td style="font-weight:500;max-width:180px">${inv.name}</td>
      <td style="color:var(--muted);font-size:12px">${inv.type}</td>
      <td><span class="badge ${statusColors[inv.decision]||'badge-gray'}">${inv.decision}</span></td>
      <td style="color:var(--muted)">${inv.loc}</td>
      <td style="color:var(--accent)">${inv.y2025?fmtMoney(inv.y2025):'—'}</td>
      <td style="color:var(--accent)">${inv.y2026?fmtMoney(inv.y2026):'—'}</td>
      <td style="color:var(--accent)">${inv.y2027?fmtMoney(inv.y2027):'—'}</td>
      <td>${inv.impact?`<span class="badge ${impactColor[inv.impact]||'badge-gray'}">${inv.impact}</span>`:'—'}</td>
      <td style="font-size:12px;color:var(--muted)">${inv.geo||'—'}</td>
    </tr>`).join('');
}

// ── PORTFOLIO ──
let orgChart, geoChart;
function setPortfolioTab(tab, el) {
  currentPortfolioTab = tab;
  document.querySelectorAll('.pill-tab').forEach(t=>t.classList.remove('active'));
  el.classList.add('active');
  renderPortfolio();
}
function renderPortfolio() {
  const filtered = investments.filter(i=>i.status===currentPortfolioTab);
  const list = document.getElementById('portfolio-list');
  list.innerHTML = filtered.length ? `<table class="data-table"><thead><tr><th>Partner</th><th>Organization Type</th><th>Location</th><th>2026 Investment</th><th>Status</th><th>Geo Impact</th></tr></thead><tbody>${filtered.map(inv=>`<tr><td style="font-weight:500">${inv.name}</td><td style="color:var(--muted);font-size:12px">${inv.type}</td><td>${inv.loc}</td><td style="color:var(--accent);font-weight:500">${fmtMoney(inv.y2026)||'—'}</td><td><span class="badge badge-${inv.status==='active'?'green':inv.status==='pipeline'?'gold':'gray'}">${inv.decision}</span></td><td style="font-size:12px;color:var(--muted)">${inv.geo||'—'}</td></tr>`).join('')}</tbody></table>` : '<div class="empty-state"><div class="empty-icon">🌐</div>No partners in this category.</div>';

  const orgCounts = {};
  investments.filter(i=>i.status==='active').forEach(i=>{orgCounts[i.type]=(orgCounts[i.type]||0)+1;});
  if (orgChart) orgChart.destroy();
  orgChart = new Chart(document.getElementById('chart-org-type').getContext('2d'), {
    type:'bar',
    data:{labels:Object.keys(orgCounts).map(l=>l.length>20?l.substring(0,20)+'…':l),datasets:[{data:Object.values(orgCounts),backgroundColor:'rgba(124,111,255,0.55)',borderColor:'rgba(124,111,255,0.8)',borderWidth:1.5,borderRadius:4}]},
    options:{indexAxis:'y',responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false}},scales:{x:{grid:{color:'rgba(255,255,255,0.04)'},ticks:{color:'#8886a0',stepSize:1}},y:{grid:{color:'rgba(255,255,255,0.02)'},ticks:{color:'#8886a0',font:{size:10}}}}}
  });

  const geoCounts = {};
  investments.filter(i=>i.status==='active'&&i.loc).forEach(i=>{geoCounts[i.loc]=(geoCounts[i.loc]||0)+1;});
  if (geoChart) geoChart.destroy();
  geoChart = new Chart(document.getElementById('chart-geo').getContext('2d'), {
    type:'doughnut',
    data:{labels:Object.keys(geoCounts),datasets:[{data:Object.values(geoCounts),backgroundColor:['rgba(200,169,110,0.7)','rgba(124,111,255,0.7)','rgba(78,207,164,0.7)','rgba(91,191,223,0.7)','rgba(224,107,125,0.6)'],borderColor:'var(--surface)',borderWidth:3,hoverOffset:5}]},
    options:{responsive:true,maintainAspectRatio:false,cutout:'55%',plugins:{legend:{position:'bottom',labels:{color:'#8886a0',font:{size:11},padding:8,boxWidth:12}}}}
  });
}

// ── EXPORT ──
function openExportModal() {
  document.getElementById('modal-content').innerHTML = `
    <div class="modal-header"><div class="modal-title">Export to Excel</div><button class="modal-close" onclick="forceCloseModal()">✕</button></div>
    <p style="color:var(--muted);font-size:13px;margin-bottom:20px;line-height:1.6;">Choose which sections to include. Each becomes its own sheet.</p>
    <div style="display:flex;flex-direction:column;gap:10px;margin-bottom:22px;">
      <label style="display:flex;align-items:center;gap:10px;cursor:pointer;font-size:13.5px;"><input type="checkbox" id="exp-talks" checked style="accent-color:var(--accent);width:15px;height:15px;"> 🎤 Talks, Panels &amp; Interviews <span style="color:var(--muted);font-size:12px;">(${data.talks.length} entries)</span></label>
      <label style="display:flex;align-items:center;gap:10px;cursor:pointer;font-size:13.5px;"><input type="checkbox" id="exp-writing" checked style="accent-color:var(--accent);width:15px;height:15px;"> ✍️ Writing <span style="color:var(--muted);font-size:12px;">(${data.writing.length} entries)</span></label>
      <label style="display:flex;align-items:center;gap:10px;cursor:pointer;font-size:13.5px;"><input type="checkbox" id="exp-consulting" checked style="accent-color:var(--accent);width:15px;height:15px;"> 💡 Consultations <span style="color:var(--muted);font-size:12px;">(${data.consulting.length} entries)</span></label>
      <label style="display:flex;align-items:center;gap:10px;cursor:pointer;font-size:13.5px;"><input type="checkbox" id="exp-investments" checked style="accent-color:var(--accent);width:15px;height:15px;"> 📊 Investments <span style="color:var(--muted);font-size:12px;">(${investments.length} entries)</span></label>
    </div>
    <div class="btn-row"><button class="btn btn-primary" onclick="runExport()">⬇ Download Excel</button><button class="btn btn-ghost" onclick="forceCloseModal()">Cancel</button></div>`;
  document.getElementById('modal-overlay').classList.add('open');
}
function runExport() {
  if (typeof XLSX === 'undefined') {
    const s = document.createElement('script');
    s.src = 'https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js';
    s.onload = doExport; s.onerror = ()=>toast('⚠️ Could not load export library.');
    document.head.appendChild(s);
  } else doExport();
}
function doExport() {
  const wb = XLSX.utils.book_new();
  const styleSheet = (ws, n) => {
    const range = XLSX.utils.decode_range(ws['!ref']);
    const cols = [];
    for (let C=range.s.c;C<=range.e.c;C++){let max=10;for(let R=range.s.r;R<=range.e.r;R++){const cell=ws[XLSX.utils.encode_cell({r:R,c:C})];if(cell&&cell.v)max=Math.max(max,String(cell.v).length);}cols.push({wch:Math.min(max+2,50)});}
    ws['!cols']=cols;
  };
  if (document.getElementById('exp-talks')?.checked && data.talks.length) {
    const rows = data.talks.map(e=>({'Title / Topic':e.title,'Type':e.type,'Venue / Event':e.venue,'Date':e.date,'Audience':e.reach,'Link':e.link,'Notes':e.notes}));
    const ws = XLSX.utils.json_to_sheet(rows); styleSheet(ws);
    XLSX.utils.book_append_sheet(wb, ws, 'Talks, Panels & Interviews');
  }
  if (document.getElementById('exp-writing')?.checked && data.writing.length) {
    const rows = data.writing.map(e=>({'Title':e.title,'Type':e.type,'Publication':e.venue,'Date':e.date,'Status':e.reach,'Link':e.link,'Notes':e.notes}));
    const ws = XLSX.utils.json_to_sheet(rows); styleSheet(ws);
    XLSX.utils.book_append_sheet(wb, ws, 'Writing');
  }
  if (document.getElementById('exp-consulting')?.checked && data.consulting.length) {
    const rows = data.consulting.map(e=>({'Topic':e.title,'Type':e.type,'Requesting Org':e.venue,'Date':e.date,'Duration (hrs)':e.reach,'Follow-up':e.link,'Notes':e.notes}));
    const ws = XLSX.utils.json_to_sheet(rows); styleSheet(ws);
    XLSX.utils.book_append_sheet(wb, ws, 'Consultations');
  }
  if (document.getElementById('exp-investments')?.checked) {
    const rows = investments.map(inv=>({'Partner':inv.name,'Org Type':inv.type,'Decision':inv.decision,'Location':inv.loc,'2025 ($)':inv.y2025||0,'2026 ($)':inv.y2026||0,'2027 ($)':inv.y2027||0,'Impact':inv.impact,'Geo Reach':inv.geo,'Status':inv.status}));
    const ws = XLSX.utils.json_to_sheet(rows); styleSheet(ws);
    XLSX.utils.book_append_sheet(wb, ws, 'Investments');
  }
  if (!wb.SheetNames.length) { toast('⚠️ No sections selected.'); return; }
  XLSX.writeFile(wb, `Ashley_Impact_Dashboard_${new Date().toISOString().split('T')[0]}.xlsx`);
  forceCloseModal();
  toast('✅ Excel file downloaded!');
}

// ── TOAST ──
function toast(msg) {
  const c = document.getElementById('toasts');
  const t = document.createElement('div');
  t.className='toast'; t.textContent=msg; c.appendChild(t);
  setTimeout(()=>{t.style.opacity='0';t.style.transition='opacity 0.3s';setTimeout(()=>t.remove(),300);},2800);
}

// ── INIT ──
document.getElementById('today-date').textContent = new Date().toLocaleDateString('en-US',{weekday:'long',year:'numeric',month:'long',day:'numeric'});
renderOverview();
</script>
</body>
</html>
