<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>pydevkrishna — Player Profile</title>
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@400;500;600;700&family=JetBrains+Mono:wght@400;700&display=swap" rel="stylesheet"/>
<style>
  :root {
    --bg: #0a0a0f;
    --bg2: #111118;
    --bg3: #1a1a26;
    --border: rgba(255,255,255,0.07);
    --border2: rgba(255,255,255,0.12);
    --cyan: #00e5ff;
    --purple: #b06aff;
    --pink: #ff4fa3;
    --amber: #ffb800;
    --green: #00ff88;
    --text: #e8e8f0;
    --muted: #6b6b80;
    --card-glow-cyan: 0 0 40px rgba(0,229,255,0.08);
    --card-glow-purple: 0 0 40px rgba(176,106,255,0.08);
  }
  * { margin:0; padding:0; box-sizing:border-box; }
  html { scroll-behavior: smooth; }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Space Grotesk', sans-serif;
    min-height: 100vh;
    overflow-x: hidden;
  }

  /* grid bg */
  body::before {
    content: '';
    position: fixed;
    inset: 0;
    background-image:
      linear-gradient(rgba(0,229,255,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(0,229,255,0.03) 1px, transparent 1px);
    background-size: 40px 40px;
    pointer-events: none;
    z-index: 0;
  }

  .container { max-width: 900px; margin: 0 auto; padding: 2rem 1.25rem; position: relative; z-index: 1; }

  /* ── HERO ── */
  .hero {
    border: 1px solid var(--border2);
    border-radius: 20px;
    background: var(--bg2);
    padding: 2.5rem;
    margin-bottom: 1.5rem;
    position: relative;
    overflow: hidden;
  }
  .hero::before {
    content: '';
    position: absolute;
    top: -60px; right: -60px;
    width: 300px; height: 300px;
    background: radial-gradient(circle, rgba(176,106,255,0.15) 0%, transparent 70%);
    pointer-events: none;
  }
  .hero::after {
    content: '';
    position: absolute;
    bottom: -80px; left: -40px;
    width: 280px; height: 280px;
    background: radial-gradient(circle, rgba(0,229,255,0.1) 0%, transparent 70%);
    pointer-events: none;
  }
  .hero-inner { position: relative; z-index: 1; }
  .player-tag {
    display: inline-flex; align-items: center; gap: 8px;
    background: rgba(0,229,255,0.08);
    border: 1px solid rgba(0,229,255,0.2);
    border-radius: 100px;
    padding: 5px 14px;
    font-family: 'JetBrains Mono', monospace;
    font-size: 12px;
    color: var(--cyan);
    margin-bottom: 1.25rem;
    letter-spacing: 0.05em;
  }
  .player-tag::before { content: '●'; font-size: 8px; animation: pulse 2s infinite; }
  @keyframes pulse { 0%,100%{opacity:1} 50%{opacity:0.3} }

  .hero-grid { display: grid; grid-template-columns: 1fr auto; gap: 2rem; align-items: start; }
  @media(max-width:600px){ .hero-grid { grid-template-columns: 1fr; } }

  .hero-name {
    font-size: clamp(2rem, 6vw, 3.2rem);
    font-weight: 700;
    line-height: 1.1;
    letter-spacing: -0.02em;
    background: linear-gradient(135deg, #fff 30%, var(--purple) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 0.5rem;
  }
  .hero-handle {
    font-family: 'JetBrains Mono', monospace;
    font-size: 15px;
    color: var(--cyan);
    margin-bottom: 1rem;
  }
  .hero-bio {
    font-size: 15px;
    color: #9999b0;
    line-height: 1.7;
    max-width: 520px;
    margin-bottom: 1.5rem;
  }
  .hero-tags { display: flex; flex-wrap: wrap; gap: 8px; }
  .tag {
    font-size: 12px;
    padding: 5px 12px;
    border-radius: 100px;
    border: 1px solid;
    font-weight: 500;
    letter-spacing: 0.02em;
  }
  .tag-cyan  { color: var(--cyan);   border-color: rgba(0,229,255,0.3);   background: rgba(0,229,255,0.06); }
  .tag-purple{ color: var(--purple); border-color: rgba(176,106,255,0.3); background: rgba(176,106,255,0.06); }
  .tag-pink  { color: var(--pink);   border-color: rgba(255,79,163,0.3);  background: rgba(255,79,163,0.06); }
  .tag-amber { color: var(--amber);  border-color: rgba(255,184,0,0.3);   background: rgba(255,184,0,0.06); }

  /* XP bars side */
  .xp-panel {
    min-width: 200px;
    background: rgba(255,255,255,0.03);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 1.25rem;
  }
  @media(max-width:600px){ .xp-panel { min-width: unset; } }
  .xp-title { font-size: 10px; color: var(--muted); letter-spacing: 0.1em; text-transform: uppercase; margin-bottom: 1rem; font-family: 'JetBrains Mono', monospace; }
  .xp-row { margin-bottom: 14px; }
  .xp-label { display: flex; justify-content: space-between; font-size: 12px; margin-bottom: 6px; }
  .xp-label span:first-child { color: #aaa; }
  .xp-label span:last-child { font-family: 'JetBrains Mono', monospace; font-size: 11px; }
  .xp-track { height: 5px; background: rgba(255,255,255,0.07); border-radius: 100px; overflow: hidden; }
  .xp-fill { height: 100%; border-radius: 100px; transition: width 1.5s ease; }
  .xp-cyan   { background: linear-gradient(90deg, var(--cyan), rgba(0,229,255,0.4)); }
  .xp-purple { background: linear-gradient(90deg, var(--purple), rgba(176,106,255,0.4)); }
  .xp-pink   { background: linear-gradient(90deg, var(--pink), rgba(255,79,163,0.4)); }
  .xp-amber  { background: linear-gradient(90deg, var(--amber), rgba(255,184,0,0.4)); }
  .xp-green  { background: linear-gradient(90deg, var(--green), rgba(0,255,136,0.4)); }

  /* ── STAT CARDS ── */
  .stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
    gap: 1rem;
    margin-bottom: 1.5rem;
  }
  .stat-card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 1.25rem;
    text-align: center;
    transition: border-color 0.2s, transform 0.2s;
    cursor: default;
  }
  .stat-card:hover { border-color: var(--border2); transform: translateY(-3px); }
  .stat-icon { font-size: 22px; margin-bottom: 8px; }
  .stat-value {
    font-size: 1.9rem;
    font-weight: 700;
    font-family: 'JetBrains Mono', monospace;
    line-height: 1;
    margin-bottom: 4px;
  }
  .stat-label { font-size: 12px; color: var(--muted); letter-spacing: 0.03em; }
  .sv-cyan   { color: var(--cyan); }
  .sv-purple { color: var(--purple); }
  .sv-pink   { color: var(--pink); }
  .sv-amber  { color: var(--amber); }

  /* ── SECTION HEADERS ── */
  .section-header {
    display: flex; align-items: center; gap: 12px;
    margin-bottom: 1.25rem; margin-top: 0.25rem;
  }
  .section-label {
    font-size: 11px;
    font-family: 'JetBrains Mono', monospace;
    color: var(--muted);
    letter-spacing: 0.12em;
    text-transform: uppercase;
  }
  .section-line { flex: 1; height: 1px; background: var(--border); }

  /* ── PROJECT CARDS ── */
  .projects-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
    gap: 1rem;
    margin-bottom: 1.5rem;
  }
  .project-card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 1.5rem;
    display: flex;
    flex-direction: column;
    transition: border-color 0.25s, transform 0.25s, box-shadow 0.25s;
    position: relative;
    overflow: hidden;
  }
  .project-card::before {
    content: '';
    position: absolute;
    top: 0; left: 0; right: 0;
    height: 2px;
  }
  .project-card.p-cyan::before   { background: linear-gradient(90deg, transparent, var(--cyan), transparent); }
  .project-card.p-purple::before { background: linear-gradient(90deg, transparent, var(--purple), transparent); }
  .project-card.p-pink::before   { background: linear-gradient(90deg, transparent, var(--pink), transparent); }
  .project-card.p-amber::before  { background: linear-gradient(90deg, transparent, var(--amber), transparent); }
  .project-card:hover { border-color: var(--border2); transform: translateY(-4px); }
  .project-card.p-cyan:hover   { box-shadow: 0 8px 40px rgba(0,229,255,0.1); }
  .project-card.p-purple:hover { box-shadow: 0 8px 40px rgba(176,106,255,0.1); }
  .project-card.p-pink:hover   { box-shadow: 0 8px 40px rgba(255,79,163,0.1); }
  .project-card.p-amber:hover  { box-shadow: 0 8px 40px rgba(255,184,0,0.1); }
  .project-header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 10px; }
  .project-name { font-size: 16px; font-weight: 600; }
  .project-date { font-size: 11px; color: var(--muted); font-family: 'JetBrains Mono', monospace; }
  .project-desc { font-size: 13.5px; color: #8888a0; line-height: 1.65; margin-bottom: 14px; flex: 1; }
  .project-stack { display: flex; flex-wrap: wrap; gap: 6px; margin-bottom: 14px; }
  .pill {
    font-size: 11px;
    font-family: 'JetBrains Mono', monospace;
    padding: 3px 9px;
    border-radius: 6px;
    border: 1px solid var(--border2);
    color: #9999b0;
    background: rgba(255,255,255,0.03);
  }
  .project-links { display: flex; gap: 10px; }
  .proj-link {
    font-size: 12px;
    color: var(--muted);
    text-decoration: none;
    display: flex; align-items: center; gap: 5px;
    transition: color 0.2s;
    padding: 5px 12px;
    border: 1px solid var(--border);
    border-radius: 8px;
    background: rgba(255,255,255,0.03);
  }
  .proj-link:hover { color: var(--text); border-color: var(--border2); }

  /* ── ACHIEVEMENTS ── */
  .achievements-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 1rem;
    margin-bottom: 1.5rem;
  }
  .ach-card {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 1.25rem;
    display: flex; gap: 14px; align-items: flex-start;
    transition: transform 0.2s, border-color 0.2s;
  }
  .ach-card:hover { transform: translateY(-3px); border-color: var(--border2); }
  .ach-medal { font-size: 28px; line-height: 1; flex-shrink: 0; }
  .ach-body { }
  .ach-title { font-size: 13px; font-weight: 600; margin-bottom: 3px; }
  .ach-sub { font-size: 12px; color: var(--muted); line-height: 1.5; }
  .ach-rank { font-family: 'JetBrains Mono', monospace; font-size: 11px; }

  /* ── CP TABLE ── */
  .cp-section {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 16px;
    overflow: hidden;
    margin-bottom: 1.5rem;
  }
  .cp-row {
    display: grid;
    grid-template-columns: 2fr 2fr 1fr 2fr;
    padding: 14px 1.5rem;
    align-items: center;
    border-bottom: 1px solid var(--border);
    transition: background 0.15s;
  }
  .cp-row:last-child { border-bottom: none; }
  .cp-row:not(.cp-head):hover { background: rgba(255,255,255,0.02); }
  .cp-head { font-size: 11px; letter-spacing: 0.08em; color: var(--muted); font-family: 'JetBrains Mono', monospace; text-transform: uppercase; }
  .cp-platform { font-size: 14px; font-weight: 600; display: flex; align-items: center; gap: 8px; }
  .cp-handle { font-family: 'JetBrains Mono', monospace; font-size: 13px; color: var(--cyan); text-decoration: none; }
  .cp-handle:hover { text-decoration: underline; }
  .cp-rating { font-family: 'JetBrains Mono', monospace; font-size: 15px; font-weight: 700; }
  .badge {
    display: inline-flex; align-items: center; gap: 5px;
    font-size: 11px; padding: 4px 10px; border-radius: 8px;
    font-weight: 600; letter-spacing: 0.03em;
  }
  .badge-gold   { background: rgba(255,184,0,0.12);    color: var(--amber);  border: 1px solid rgba(255,184,0,0.25); }
  .badge-blue   { background: rgba(0,229,255,0.08);    color: var(--cyan);   border: 1px solid rgba(0,229,255,0.2); }

  @media(max-width:560px){
    .cp-row { grid-template-columns: 1fr 1fr; gap: 6px; }
    .cp-row .cp-rating, .cp-row .badge { grid-column: span 1; }
  }

  /* ── TECH STACK ── */
  .stack-section {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 1.5rem;
    margin-bottom: 1.5rem;
  }
  .stack-group { margin-bottom: 1.25rem; }
  .stack-group:last-child { margin-bottom: 0; }
  .stack-group-label { font-size: 11px; font-family: 'JetBrains Mono', monospace; color: var(--muted); letter-spacing: 0.1em; text-transform: uppercase; margin-bottom: 10px; }
  .tech-chips { display: flex; flex-wrap: wrap; gap: 8px; }
  .tech-chip {
    font-size: 13px;
    padding: 6px 14px;
    border-radius: 10px;
    border: 1px solid var(--border2);
    color: var(--text);
    background: rgba(255,255,255,0.04);
    transition: background 0.15s, border-color 0.15s, transform 0.15s;
    cursor: default;
    display: flex; align-items: center; gap: 6px;
  }
  .tech-chip:hover { background: rgba(255,255,255,0.07); transform: translateY(-2px); }
  .dot { width: 6px; height: 6px; border-radius: 50%; flex-shrink: 0; }
  .dot-cyan   { background: var(--cyan); }
  .dot-purple { background: var(--purple); }
  .dot-pink   { background: var(--pink); }
  .dot-amber  { background: var(--amber); }
  .dot-green  { background: var(--green); }
  .dot-gray   { background: #555; }

  /* ── FOOTER ── */
  .footer {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 2rem;
    text-align: center;
  }
  .footer-title { font-size: 18px; font-weight: 600; margin-bottom: 8px; }
  .footer-sub { font-size: 14px; color: var(--muted); margin-bottom: 1.5rem; }
  .contact-links { display: flex; flex-wrap: wrap; justify-content: center; gap: 10px; }
  .contact-link {
    display: inline-flex; align-items: center; gap: 8px;
    padding: 9px 18px;
    border-radius: 10px;
    border: 1px solid var(--border2);
    font-size: 13px;
    text-decoration: none;
    color: var(--text);
    background: rgba(255,255,255,0.04);
    transition: background 0.15s, transform 0.15s, border-color 0.15s;
  }
  .contact-link:hover { background: rgba(255,255,255,0.08); transform: translateY(-2px); border-color: rgba(255,255,255,0.2); }
  .cl-cyan   { border-color: rgba(0,229,255,0.25);   color: var(--cyan); }
  .cl-purple { border-color: rgba(176,106,255,0.25); color: var(--purple); }
  .cl-pink   { border-color: rgba(255,79,163,0.25);  color: var(--pink); }
  .cl-amber  { border-color: rgba(255,184,0,0.25);   color: var(--amber); }
  .cl-cyan:hover   { background: rgba(0,229,255,0.07); }
  .cl-purple:hover { background: rgba(176,106,255,0.07); }
  .cl-pink:hover   { background: rgba(255,79,163,0.07); }
  .cl-amber:hover  { background: rgba(255,184,0,0.07); }

  /* ── ABOUT BLOCK ── */
  .about-block {
    background: var(--bg2);
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 1.5rem;
    margin-bottom: 1.5rem;
  }
  .about-block pre {
    font-family: 'JetBrains Mono', monospace;
    font-size: 13px;
    line-height: 1.75;
    color: #9999b0;
    white-space: pre-wrap;
    word-break: break-word;
  }
  .kw  { color: var(--purple); }
  .cls { color: var(--cyan); }
  .str { color: var(--green); }
  .num { color: var(--amber); }
  .fn  { color: var(--pink); }
  .comment { color: #444460; }

  /* animations */
  @keyframes fadeUp {
    from { opacity:0; transform:translateY(16px); }
    to   { opacity:1; transform:translateY(0); }
  }
  .hero       { animation: fadeUp 0.5s ease both; }
  .stats-grid { animation: fadeUp 0.5s 0.1s ease both; }
  .about-block{ animation: fadeUp 0.5s 0.15s ease both; }
  .projects-grid{ animation: fadeUp 0.5s 0.2s ease both; }
  .achievements-grid{ animation: fadeUp 0.5s 0.25s ease both; }
</style>
</head>
<body>
<div class="container">

  <!-- HERO -->
  <div class="hero">
    <div class="hero-inner">
      <div class="player-tag">PLAYER PROFILE &nbsp;·&nbsp; ONLINE</div>
      <div class="hero-grid">
        <div>
          <h1 class="hero-name">Krishan Kant<br/>Sharma</h1>
          <div class="hero-handle">@pydevkrishna</div>
          <p class="hero-bio">
            CS undergrad at GL Bajaj, Mathura (AI/ML, 2023–2027). I got into coding through competitive programming
            and got hooked — went from not knowing what a for loop was to hitting Guardian on LeetCode in about a year.
            Now I build real things: RAG pipelines, real-time apps, and ML experiments.
          </p>
          <div class="hero-tags">
            <span class="tag tag-cyan">Full-Stack Dev</span>
            <span class="tag tag-purple">Competitive Programmer</span>
            <span class="tag tag-pink">ML Builder</span>
            <span class="tag tag-amber">Open to Internships</span>
          </div>
        </div>
        <div class="xp-panel">
          <div class="xp-title">// skill.exe</div>
          <div class="xp-row">
            <div class="xp-label"><span>Full-Stack</span><span style="color:var(--cyan)">92</span></div>
            <div class="xp-track"><div class="xp-fill xp-cyan" style="width:92%"></div></div>
          </div>
          <div class="xp-row">
            <div class="xp-label"><span>Algorithms</span><span style="color:var(--purple)">96</span></div>
            <div class="xp-track"><div class="xp-fill xp-purple" style="width:96%"></div></div>
          </div>
          <div class="xp-row">
            <div class="xp-label"><span>ML / AI</span><span style="color:var(--pink)">78</span></div>
            <div class="xp-track"><div class="xp-fill xp-pink" style="width:78%"></div></div>
          </div>
          <div class="xp-row">
            <div class="xp-label"><span>System Design</span><span style="color:var(--amber)">80</span></div>
            <div class="xp-track"><div class="xp-fill xp-amber" style="width:80%"></div></div>
          </div>
          <div class="xp-row" style="margin-bottom:0">
            <div class="xp-label"><span>Problem Solving</span><span style="color:var(--green)">99</span></div>
            <div class="xp-track"><div class="xp-fill xp-green" style="width:99%"></div></div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- STATS -->
  <div class="stats-grid">
    <div class="stat-card">
      <div class="stat-icon">⚔️</div>
      <div class="stat-value sv-amber">2342</div>
      <div class="stat-label">LeetCode Max Rating</div>
    </div>
    <div class="stat-card">
      <div class="stat-icon">🔵</div>
      <div class="stat-value sv-cyan">1603</div>
      <div class="stat-label">Codeforces Max Rating</div>
    </div>
    <div class="stat-card">
      <div class="stat-icon">🧠</div>
      <div class="stat-value sv-purple">2000<span style="font-size:1rem">+</span></div>
      <div class="stat-label">Problems Solved</div>
    </div>
    <div class="stat-card">
      <div class="stat-icon">🏆</div>
      <div class="stat-value sv-pink">Top<span style="font-size:1.1rem"> 0.4%</span></div>
      <div class="stat-label">LeetCode Global Rank</div>
    </div>
  </div>

  <!-- ABOUT -->
  <div class="about-block">
    <pre><span class="kw">class</span> <span class="cls">KrishanKant</span>:
  <span class="kw">def</span> <span class="fn">__init__</span>(self):
    self.college      = <span class="str">"GL Bajaj Group of Institutions (2023–2027)"</span>
    self.degree       = <span class="str">"B.Tech CSE (AI/ML)"</span>
    self.location     = <span class="str">"Mathura, Uttar Pradesh 🇮🇳"</span>
    self.rating       = {<span class="str">"LeetCode"</span>: <span class="num">2342</span>, <span class="str">"Codeforces"</span>: <span class="num">1603</span>}
    self.interests    = [<span class="str">"RAG Systems"</span>, <span class="str">"Real-time Apps"</span>, <span class="str">"Deep Learning"</span>]
    self.looking_for  = <span class="str">"SWE / ML internship opportunities 👀"</span>

  <span class="kw">def</span> <span class="fn">reach_me</span>(self):
    <span class="kw">return</span> <span class="str">"krishnakantvdjs10tha@gmail.com"</span>

<span class="comment"># currently: building AI tools that actually work + grinding contests</span></pre>
  </div>

  <!-- PROJECTS -->
  <div class="section-header">
    <span class="section-label">// projects.log</span>
    <div class="section-line"></div>
  </div>
  <div class="projects-grid">

    <div class="project-card p-cyan">
      <div class="project-header">
        <div class="project-name">🔍 RepoFriend</div>
        <div class="project-date">Dec '25–Feb '26</div>
      </div>
      <p class="project-desc">
        Point it at any GitHub repo and ask questions in plain English. It figures out the relevant files, pulls context, and gives grounded answers — not hallucinations. Handles PR review, commit summaries, and multi-repo chat workspaces.
      </p>
      <div class="project-stack">
        <span class="pill">Python</span><span class="pill">FastAPI</span><span class="pill">Qdrant</span>
        <span class="pill">Next.js</span><span class="pill">Gemini</span><span class="pill">PostgreSQL</span>
      </div>
      <div class="project-links">
        <a href="#" class="proj-link">↗ Live Site</a>
        <a href="#" class="proj-link">⎇ GitHub</a>
      </div>
    </div>

    <div class="project-card p-purple">
      <div class="project-header">
        <div class="project-name">⚔️ CatCode</div>
        <div class="project-date">Oct '25–Nov '25</div>
      </div>
      <p class="project-desc">
        Head-to-head coding battles — get matched with someone, pick a problem, and race. Real-time state over WebSockets, 90+ languages via Monaco Editor, and Gemini reviews both solutions after the match.
      </p>
      <div class="project-stack">
        <span class="pill">FastAPI</span><span class="pill">React</span><span class="pill">TypeScript</span>
        <span class="pill">Redis</span><span class="pill">WebSockets</span><span class="pill">Gemini</span>
      </div>
      <div class="project-links">
        <a href="#" class="proj-link">↗ Live Site</a>
        <a href="#" class="proj-link">⎇ GitHub</a>
      </div>
    </div>

    <div class="project-card p-amber">
      <div class="project-header">
        <div class="project-name">📦 Amazon Sales Predictor</div>
        <div class="project-date">Aug '25–Sep '25</div>
      </div>
      <p class="project-desc">
        PyTorch regression model that predicts fair Amazon product prices from real Kaggle data. Full pipeline from messy CSV to trained model — my first end-to-end ML project built without tutorials.
      </p>
      <div class="project-stack">
        <span class="pill">PyTorch</span><span class="pill">Pandas</span>
        <span class="pill">NumPy</span><span class="pill">Matplotlib</span>
      </div>
      <div class="project-links">
        <a href="#" class="proj-link">📝 Medium</a>
        <a href="#" class="proj-link">⎇ GitHub</a>
      </div>
    </div>

    <div class="project-card p-pink" style="border-style:dashed;">
      <div class="project-header">
        <div class="project-name">🔮 Next Up...</div>
        <div class="project-date">In progress</div>
      </div>
      <p class="project-desc">
        Tinkering with an agentic code reviewer that understands your whole codebase — not just the diff.
        Also experimenting with LLM orchestration patterns and more real-time multiplayer ideas.
      </p>
      <div class="project-stack">
        <span class="pill">???</span><span class="pill">TBD</span><span class="pill">🚧</span>
      </div>
      <div class="project-links">
        <a href="mailto:krishnakantvdjs10tha@gmail.com" class="proj-link">💬 Let's collab</a>
      </div>
    </div>

  </div>

  <!-- ACHIEVEMENTS -->
  <div class="section-header">
    <span class="section-label">// achievements.json</span>
    <div class="section-line"></div>
  </div>
  <div class="achievements-grid">
    <div class="ach-card">
      <div class="ach-medal">🥇</div>
      <div class="ach-body">
        <div class="ach-title">LeetCode Weekly #486</div>
        <div class="ach-sub"><span class="ach-rank" style="color:var(--amber)">Rank #73</span> / 41,284 participants</div>
      </div>
    </div>
    <div class="ach-card">
      <div class="ach-medal">🥈</div>
      <div class="ach-body">
        <div class="ach-title">Codeforces Round 1083</div>
        <div class="ach-sub"><span class="ach-rank" style="color:var(--cyan)">Rank #520</span> / 21,113 participants</div>
      </div>
    </div>
    <div class="ach-card">
      <div class="ach-medal">🤖</div>
      <div class="ach-body">
        <div class="ach-title">Amazon ML Challenge 2025</div>
        <div class="ach-sub"><span class="ach-rank" style="color:var(--purple)">Rank #235</span> / 6000+ teams · 54.21</div>
      </div>
    </div>
    <div class="ach-card">
      <div class="ach-medal">🧠</div>
      <div class="ach-body">
        <div class="ach-title">2000+ Problems</div>
        <div class="ach-sub">Solved across LeetCode, Codeforces, and more</div>
      </div>
    </div>
  </div>

  <!-- CP TABLE -->
  <div class="section-header">
    <span class="section-label">// cp_profiles.db</span>
    <div class="section-line"></div>
  </div>
  <div class="cp-section" style="margin-bottom:1.5rem">
    <div class="cp-row cp-head">
      <div>Platform</div><div>Handle</div><div>Rating</div><div>Status</div>
    </div>
    <div class="cp-row">
      <div class="cp-platform"><span style="font-size:18px">🟡</span> LeetCode</div>
      <div><a href="https://leetcode.com/Krishan-kant-sharma" class="cp-handle" target="_blank">Krishan-kant-sharma</a></div>
      <div class="cp-rating sv-amber">2342</div>
      <div><span class="badge badge-gold">⚔ Guardian · Top 0.4%</span></div>
    </div>
    <div class="cp-row">
      <div class="cp-platform"><span style="font-size:18px">🔵</span> Codeforces</div>
      <div><a href="https://codeforces.com/profile/Krishan-Kant-Sharma" class="cp-handle" target="_blank">Krishan-Kant-Sharma</a></div>
      <div class="cp-rating sv-cyan">1603</div>
      <div><span class="badge badge-blue">⚡ Expert</span></div>
    </div>
  </div>

  <!-- TECH STACK -->
  <div class="section-header">
    <span class="section-label">// tech_stack.yml</span>
    <div class="section-line"></div>
  </div>
  <div class="stack-section">
    <div class="stack-group">
      <div class="stack-group-label">Languages</div>
      <div class="tech-chips">
        <span class="tech-chip"><span class="dot dot-cyan"></span>Python</span>
        <span class="tech-chip"><span class="dot dot-purple"></span>C++</span>
        <span class="tech-chip"><span class="dot dot-amber"></span>JavaScript</span>
        <span class="tech-chip"><span class="dot dot-cyan"></span>TypeScript</span>
        <span class="tech-chip"><span class="dot dot-pink"></span>SQL</span>
      </div>
    </div>
    <div class="stack-group">
      <div class="stack-group-label">Frameworks & ML</div>
      <div class="tech-chips">
        <span class="tech-chip"><span class="dot dot-green"></span>FastAPI</span>
        <span class="tech-chip"><span class="dot dot-cyan"></span>React</span>
        <span class="tech-chip"><span class="dot dot-gray"></span>Next.js</span>
        <span class="tech-chip"><span class="dot dot-pink"></span>PyTorch</span>
        <span class="tech-chip"><span class="dot dot-green"></span>LangChain</span>
        <span class="tech-chip"><span class="dot dot-amber"></span>scikit-learn</span>
      </div>
    </div>
    <div class="stack-group">
      <div class="stack-group-label">Tools & Infra</div>
      <div class="tech-chips">
        <span class="tech-chip"><span class="dot dot-cyan"></span>Docker</span>
        <span class="tech-chip"><span class="dot dot-pink"></span>Redis</span>
        <span class="tech-chip"><span class="dot dot-amber"></span>Firebase</span>
        <span class="tech-chip"><span class="dot dot-gray"></span>Vercel</span>
        <span class="tech-chip"><span class="dot dot-green"></span>Supabase</span>
        <span class="tech-chip"><span class="dot dot-cyan"></span>GCP</span>
        <span class="tech-chip"><span class="dot dot-purple"></span>Git</span>
        <span class="tech-chip"><span class="dot dot-pink"></span>Qdrant</span>
      </div>
    </div>
  </div>

  <!-- FOOTER -->
  <div class="footer">
    <div class="footer-title">Got something interesting? Let's talk.</div>
    <p class="footer-sub">Internship opportunities, collabs, or just a cool idea — always up for it.</p>
    <div class="contact-links">
      <a href="mailto:krishnakantvdjs10tha@gmail.com" class="contact-link cl-pink">✉ Email Me</a>
      <a href="https://linkedin.com" target="_blank" class="contact-link cl-cyan">in LinkedIn</a>
      <a href="https://leetcode.com/Krishan-kant-sharma" target="_blank" class="contact-link cl-amber">⚔ LeetCode</a>
      <a href="https://codeforces.com/profile/Krishan-Kant-Sharma" target="_blank" class="contact-link cl-purple">★ Codeforces</a>
      <a href="https://github.com/pydevkrishna" target="_blank" class="contact-link">⎇ GitHub</a>
    </div>
    <div style="margin-top:1.5rem; font-size:12px; color:var(--muted); font-family:'JetBrains Mono',monospace;">
      📍 Mathura, UP, India &nbsp;·&nbsp; CGPA 7.52 &nbsp;·&nbsp; B.Tech CSE AI/ML
    </div>
  </div>

</div>
</body>
</html>
