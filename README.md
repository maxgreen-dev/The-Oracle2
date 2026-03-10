# The-Oracle2
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Mila Researcher × Startup Matching</title>
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@300;400;500;600;700&family=IBM+Plex+Sans:wght@300;400;500;600;700&display=swap" rel="stylesheet" />
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react/18.2.0/umd/react.production.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/react-dom/18.2.0/umd/react-dom.production.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/babel-standalone/7.23.2/babel.min.js"></script>
  <style>
    /* ── Oracle / Palantir Design Tokens ── */
    @import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@300;400;500;600;700&family=IBM+Plex+Sans:wght@300;400;500;600;700&display=swap');

    :root {
      --p-black:     #000000;
      --p-dark:      #0a0a0a;
      --p-dark2:     #111111;
      --p-dark3:     #1a1a1a;
      --p-border:    #222222;
      --p-border2:   #2e2e2e;
      --p-white:     #ffffff;
      --p-off-white: #e8e8e8;
      --p-dim:       #888888;
      --p-dimmer:    #555555;
      --p-accent:    #c8ff00;
      --p-accent2:   #a8d400;
      /* Keep mila vars for app shell */
      --mila-purple:     #3d1152;
      --mila-purple-mid: #5c2278;
      --mila-purple-lt:  #7c3aad;
      --mila-purple-bg:  #f5f0fa;
      --mila-purple-brd: #e0d0f0;
      --mila-green:      #00b38a;
      --mila-green-lt:   #e6f8f4;
      --mila-grey-bg:    #f4f4f4;
      --mila-grey-brd:   #e0e0e0;
      --mila-text:       #1a1a1a;
      --mila-text-mid:   #4a4a4a;
      --mila-text-lt:    #888;
      --mila-white:      #ffffff;
    }

    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
    html, body { height: 100%; }

    body {
      background: var(--p-black);
      color: var(--p-white);
      font-family: 'IBM Plex Sans', sans-serif;
      overflow-x: hidden;
    }

    ::-webkit-scrollbar { width: 2px; height: 2px; }
    ::-webkit-scrollbar-track { background: #0a0a0a; }
    ::-webkit-scrollbar-thumb { background: #333; }

    /* ── Landing page ── */
    #landing {
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      background: var(--p-black);
    }

    /* Subtle grid overlay */
    .bg-grid {
      display: block;
      position: fixed;
      inset: 0;
      pointer-events: none;
      z-index: 0;
      background-image:
        linear-gradient(rgba(255,255,255,0.025) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.025) 1px, transparent 1px);
      background-size: 80px 80px;
    }
    .bg-glow { display: none; }

    /* ── Navbar ── */
    nav {
      position: fixed;
      top: 0; left: 0; right: 0;
      z-index: 50;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 60px;
      height: 64px;
      border-bottom: 1px solid var(--p-border);
      background: rgba(0,0,0,0.92);
      backdrop-filter: blur(12px);
    }

    .nav-logo {
      display: flex;
      align-items: center;
      gap: 10px;
      font-family: 'IBM Plex Mono', monospace;
      font-size: 13px;
      font-weight: 600;
      color: var(--p-white);
      letter-spacing: 0.12em;
      text-transform: uppercase;
    }
    .nav-logo-dot {
      width: 6px; height: 6px;
      border-radius: 50%;
      background: var(--p-accent);
      display: inline-block;
    }

    .nav-links {
      display: flex;
      align-items: center;
      gap: 40px;
    }

    .nav-link {
      font-family: 'IBM Plex Mono', monospace;
      font-size: 11px;
      font-weight: 500;
      color: var(--p-dim);
      text-decoration: none;
      cursor: pointer;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      transition: color 0.2s;
    }
    .nav-link:hover { color: var(--p-white); }

    .nav-cta {
      background: transparent;
      color: var(--p-white);
      border: 1px solid var(--p-border2);
      padding: 9px 22px;
      font-family: 'IBM Plex Mono', monospace;
      font-size: 11px;
      font-weight: 600;
      letter-spacing: 0.1em;
      text-transform: uppercase;
      cursor: pointer;
      transition: all 0.2s;
    }
    .nav-cta:hover { border-color: var(--p-accent); color: var(--p-accent); }

    /* ── HERO — The Oracle reveal ── */
    .hero {
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 100px 60px 80px;
      background: var(--p-black);
      position: relative;
      overflow: hidden;
      min-height: 100vh;
    }

    /* Radial light that fades in behind title */
    .hero-light {
      position: absolute;
      top: 50%; left: 50%;
      transform: translate(-50%, -60%);
      width: 800px; height: 500px;
      border-radius: 50%;
      background: radial-gradient(ellipse, rgba(200,255,0,0.04) 0%, rgba(200,255,0,0.01) 40%, transparent 70%);
      opacity: 0;
      animation: lightReveal 3s 1.2s ease-out forwards;
      pointer-events: none;
    }

    @keyframes lightReveal {
      0%   { opacity: 0; transform: translate(-50%, -60%) scale(0.6); }
      100% { opacity: 1; transform: translate(-50%, -60%) scale(1); }
    }

    /* Scanline effect */
    .hero::after {
      content: '';
      position: absolute;
      inset: 0;
      background: repeating-linear-gradient(
        0deg,
        transparent,
        transparent 2px,
        rgba(0,0,0,0.08) 2px,
        rgba(0,0,0,0.08) 4px
      );
      pointer-events: none;
    }

    .hero-eyebrow {
      font-family: 'IBM Plex Mono', monospace;
      font-size: 11px;
      font-weight: 500;
      letter-spacing: 0.3em;
      text-transform: uppercase;
      color: var(--p-dimmer);
      margin-bottom: 32px;
      opacity: 0;
      animation: fadeUp 1.2s 0.4s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }

    /* "Meet" small label above title */
    .hero-meet {
      font-family: 'IBM Plex Mono', monospace;
      font-size: 13px;
      font-weight: 400;
      letter-spacing: 0.5em;
      text-transform: uppercase;
      color: var(--p-dimmer);
      margin-bottom: 12px;
      opacity: 0;
      animation: fadeUp 1s 0.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }

    /* "The Oracle" massive title with glow emergence */
    .hero-title {
      font-family: 'IBM Plex Sans', sans-serif;
      font-size: clamp(72px, 12vw, 160px);
      font-weight: 700;
      line-height: 0.92;
      letter-spacing: -0.04em;
      color: var(--p-white);
      margin-bottom: 0;
      opacity: 0;
      text-shadow: 0 0 0px rgba(200,255,0,0);
      animation: titleReveal 2.4s 1s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }

    @keyframes titleReveal {
      0%   { opacity: 0; transform: translateY(30px); text-shadow: 0 0 0px rgba(200,255,0,0); filter: blur(8px); }
      40%  { opacity: 0.6; filter: blur(2px); text-shadow: 0 0 60px rgba(200,255,0,0.15); }
      100% { opacity: 1; transform: translateY(0); text-shadow: 0 0 80px rgba(200,255,0,0.08); filter: blur(0); }
    }

    /* Horizontal rule below title */
    .hero-rule {
      width: 60px;
      height: 1px;
      background: var(--p-accent);
      margin: 40px auto;
      opacity: 0;
      animation: fadeUp 0.8s 2.2s ease forwards;
    }

    .hero-sub {
      font-family: 'IBM Plex Sans', sans-serif;
      font-size: 14px;
      font-weight: 300;
      line-height: 1.9;
      color: var(--p-dim);
      max-width: 380px;
      margin-bottom: 48px;
      letter-spacing: 0.03em;
      opacity: 0;
      animation: fadeUp 0.8s 2.5s ease forwards;
    }

    .hero-actions {
      display: flex;
      gap: 16px;
      align-items: center;
      opacity: 0;
      animation: fadeUp 0.8s 2.8s ease forwards;
    }

    .btn-primary {
      background: var(--p-white);
      color: var(--p-black);
      border: none;
      padding: 14px 36px;
      font-family: 'IBM Plex Mono', monospace;
      font-size: 11px;
      font-weight: 600;
      letter-spacing: 0.15em;
      text-transform: uppercase;
      cursor: pointer;
      transition: all 0.2s;
    }
    .btn-primary:hover { background: var(--p-accent); }

    .btn-ghost {
      background: transparent;
      color: var(--p-dim);
      border: 1px solid var(--p-border2);
      padding: 14px 36px;
      font-family: 'IBM Plex Mono', monospace;
      font-size: 11px;
      font-weight: 500;
      letter-spacing: 0.15em;
      text-transform: uppercase;
      cursor: pointer;
      transition: all 0.2s;
    }
    .btn-ghost:hover { border-color: var(--p-dimmer); color: var(--p-white); }

    /* ── Stats bar ── */
    .stats-bar {
      display: flex;
      justify-content: center;
      gap: 0;
      border-top: 1px solid var(--p-border);
      border-bottom: 1px solid var(--p-border);
      opacity: 0;
      animation: fadeUp 0.8s 3.2s ease forwards;
    }

    .stat {
      flex: 1;
      text-align: center;
      padding: 28px 40px;
      border-right: 1px solid var(--p-border);
    }
    .stat:last-child { border-right: none; }

    .stat-num {
      font-family: 'IBM Plex Mono', monospace;
      font-size: 22px;
      font-weight: 600;
      color: var(--p-white);
      line-height: 1;
      letter-spacing: -0.02em;
    }
    .stat-label {
      font-family: 'IBM Plex Mono', monospace;
      font-size: 10px;
      font-weight: 400;
      color: var(--p-dimmer);
      margin-top: 8px;
      letter-spacing: 0.15em;
      text-transform: uppercase;
    }

    /* ── Features section ── */
    .features {
      padding: 120px 80px;
      background: var(--p-dark);
      position: relative;
      z-index: 1;
    }

    .section-label {
      font-family: 'IBM Plex Mono', monospace;
      font-size: 10px;
      font-weight: 500;
      letter-spacing: 0.3em;
      text-transform: uppercase;
      color: var(--p-accent);
      margin-bottom: 20px;
    }

    .section-title {
      font-size: clamp(24px, 3vw, 38px);
      font-weight: 600;
      color: var(--p-white);
      line-height: 1.15;
      letter-spacing: -0.02em;
      margin-bottom: 64px;
      max-width: 500px;
    }

    .features-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
      gap: 1px;
      border: 1px solid var(--p-border);
    }

    .feature-card {
      background: var(--p-dark2);
      border: none;
      padding: 36px 32px;
      transition: background 0.2s;
    }
    .feature-card:hover { background: var(--p-dark3); }

    .feature-icon {
      font-size: 18px;
      margin-bottom: 20px;
      opacity: 0.6;
    }

    .feature-title {
      font-family: 'IBM Plex Mono', monospace;
      font-size: 13px;
      font-weight: 600;
      color: var(--p-white);
      margin-bottom: 12px;
      letter-spacing: 0.05em;
    }

    .feature-desc {
      font-size: 13px;
      line-height: 1.8;
      color: var(--p-dim);
    }

    /* ── How it works ── */
    .how-it-works {
      padding: 120px 80px;
      background: var(--p-black);
      border-top: 1px solid var(--p-border);
      border-bottom: 1px solid var(--p-border);
      position: relative;
      z-index: 1;
    }

    .steps {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 0;
      margin-top: 0;
      border: 1px solid var(--p-border);
    }

    .step {
      position: relative;
      padding: 40px 36px;
      border-right: 1px solid var(--p-border);
    }
    .step:last-child { border-right: none; }

    .step-num {
      font-family: 'IBM Plex Mono', monospace;
      font-size: 10px;
      font-weight: 600;
      color: var(--p-accent);
      letter-spacing: 0.2em;
      line-height: 1;
      margin-bottom: 20px;
    }

    .step-title {
      font-size: 18px;
      font-weight: 600;
      color: var(--p-white);
      margin-bottom: 14px;
      letter-spacing: -0.01em;
    }

    .step-desc {
      font-size: 13px;
      line-height: 1.8;
      color: var(--p-dim);
    }

    /* ── CTA section ── */
    .cta-section {
      padding: 120px 80px;
      text-align: center;
      background: var(--p-dark);
      border-top: 1px solid var(--p-border);
      position: relative;
      z-index: 1;
    }

    .cta-section .section-label { color: var(--p-accent); }

    .cta-section .section-title {
      margin: 0 auto 20px;
      text-align: center;
      color: var(--p-white);
    }

    .cta-sub {
      font-size: 13px;
      color: var(--p-dimmer);
      margin-bottom: 48px;
      font-family: 'IBM Plex Mono', monospace;
      letter-spacing: 0.05em;
    }

    /* ── Footer ── */
    footer {
      background: var(--p-black);
      border-top: 1px solid var(--p-border);
      padding: 28px 80px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      position: relative;
      z-index: 1;
    }

    .footer-logo {
      font-family: 'IBM Plex Mono', monospace;
      font-size: 11px;
      font-weight: 500;
      color: var(--p-dimmer);
      display: flex;
      align-items: center;
      gap: 8px;
      letter-spacing: 0.12em;
      text-transform: uppercase;
    }
    .footer-logo-dot { width: 5px; height: 5px; border-radius: 50%; background: var(--p-accent); display: inline-block; }

    .footer-note {
      font-family: 'IBM Plex Mono', monospace;
      font-size: 10px;
      color: var(--p-dimmer);
      letter-spacing: 0.1em;
    }

    /* ── App shell ── */
    #app-shell {
      display: none;
      height: 100vh;
      flex-direction: column;
      background: var(--mila-grey-bg);
    }

    #app-shell.active {
      display: flex;
    }

    /* ── Animations ── */
    @keyframes fadeUp {
      from { opacity: 0; transform: translateY(20px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    @keyframes slideIn {
      from { opacity: 0; transform: translateX(-12px); }
      to   { opacity: 1; transform: translateX(0); }
    }

    /* ── React app styles (Mila palette) ── */
    .r-card { transition: all 0.22s cubic-bezier(.4,0,.2,1); }
    .r-card:hover { transform: translateY(-2px); }
    .r-tbtn { cursor: pointer; transition: all 0.2s; border: none; background: none; }
    .r-chip { display: inline-block; padding: 2px 9px; border-radius: 4px; font-size: 11px; font-family: 'Inter',sans-serif; font-weight: 500; letter-spacing: 0.01em; }
    .r-glow { box-shadow: 0 2px 12px rgba(61,17,82,0.08); }
    .r-sel { box-shadow: 0 0 0 2px var(--mila-purple), 0 4px 20px rgba(61,17,82,0.2) !important; }
    .r-mbar { transition: width 0.9s cubic-bezier(.4,0,.2,1); }
    .r-abtn { background: none; border: none; cursor: pointer; padding: 4px 6px; border-radius: 4px; font-size: 13px; transition: background 0.15s; }
    .r-abtn:hover { background: var(--mila-grey-bg); }
    select { appearance: none; }
    input::placeholder { color: #bbb; }
    @keyframes r-fu { from { opacity:0; transform:translateY(8px) } to { opacity:1; transform:none } }
    @keyframes r-toast { 0%,100% { opacity:0; transform:translateY(12px) } 15%,85% { opacity:1; transform:none } }
    .r-toast { animation: r-toast 3.5s ease forwards; }
    @keyframes r-spin { from { transform:rotate(0deg) } to { transform:rotate(360deg) } }

    @media (max-width: 768px) {
      nav { padding: 0 24px; }
      .hero { padding: 100px 24px 60px; }
      .hero-title { font-size: clamp(56px, 15vw, 100px); }
      .stats-bar { flex-wrap: wrap; }
      .stat { flex: 1 1 calc(50% - 1px); }
      .features, .how-it-works, .cta-section { padding: 80px 24px; }
      footer { padding: 24px; flex-direction: column; gap: 12px; text-align: center; }
    }
  </style>
</head>
<body>

<!-- ── Background ── -->
<div class="bg-grid"></div>
<div class="bg-glow"></div>

<!-- ══════════════════════════════════════════
     LANDING PAGE
══════════════════════════════════════════ -->
<div id="landing">
  <nav>
    <div class="nav-logo"><span class="nav-logo-dot"></span> The Oracle</div>
    <div class="nav-links">
      <a class="nav-link" onclick="scrollToSection('features')">Capabilities</a>
      <a class="nav-link" onclick="scrollToSection('how')">Protocol</a>
      <button class="nav-cta" onclick="launchApp()">Access →</button>
    </div>
  </nav>

  <!-- Hero -->
  <section class="hero">
    <div class="hero-light"></div>
    <div class="hero-eyebrow">Mila · Intelligence Matching System</div>
    <div class="hero-meet">Meet</div>
    <h1 class="hero-title">The Oracle</h1>
    <div class="hero-rule"></div>
    <p class="hero-sub">Precision alignment between Mila AI researchers and the startups that need them most.</p>
    <div class="hero-actions">
      <button class="btn-primary" onclick="launchApp()">Begin →</button>
      <button class="btn-ghost" onclick="scrollToSection('how')">How it works</button>
    </div>
  </section>

  <!-- Stats -->
  <div class="stats-bar">
    <div class="stat">
      <div class="stat-num">100%</div>
      <div class="stat-label">Alignment scoring</div>
    </div>
    <div class="stat">
      <div class="stat-num">Excel</div>
      <div class="stat-label">Bulk import</div>
    </div>
    <div class="stat">
      <div class="stat-num">Instant</div>
      <div class="stat-label">Match results</div>
    </div>
    <div class="stat">
      <div class="stat-num">Free</div>
      <div class="stat-label">No account needed</div>
    </div>
  </div>

  <!-- Features -->
  <section class="features" id="features">
    <div class="section-label">Capabilities</div>
    <h2 class="section-title">Everything needed to find the right match</h2>
    <div class="features-grid">
      <div class="feature-card">
        <div class="feature-icon">📊</div>
        <div class="feature-title">Excel Import</div>
        <div class="feature-desc">Upload your researcher database directly from Excel. Supports Researcher Name, University/Affiliation, H-index, Core Research Focus, and Industry Experience columns.</div>
      </div>
      <div class="feature-card">
        <div class="feature-icon">⚡</div>
        <div class="feature-title">Instant Matching</div>
        <div class="feature-desc">Select a startup and instantly see every researcher ranked by alignment percentage. Matching tags light up so you see exactly what aligned.</div>
      </div>
      <div class="feature-card">
        <div class="feature-icon">🔬</div>
        <div class="feature-title">Researcher Profiles</div>
        <div class="feature-desc">Track H-index, institutional affiliation, industry experience, and availability status. Filter matches to only show researchers open to collaboration.</div>
      </div>
      <div class="feature-card">
        <div class="feature-icon">🚀</div>
        <div class="feature-title">Startup Management</div>
        <div class="feature-desc">Add startups with funding stage, description, and AI needs. Use the same terminology as your researchers' focus areas for precise matching.</div>
      </div>
      <div class="feature-card">
        <div class="feature-icon">🎯</div>
        <div class="feature-title">Precision Scoring</div>
        <div class="feature-desc">Alignment calculated as the overlap between a startup's AI needs and a researcher's focus areas — no black box, fully transparent scoring.</div>
      </div>
      <div class="feature-card">
        <div class="feature-icon">✏️</div>
        <div class="feature-title">Editable & Flexible</div>
        <div class="feature-desc">Add, edit, or delete any startup or researcher at any time. Mix manual entry with bulk Excel uploads for maximum flexibility.</div>
      </div>
    </div>
  </section>

  <!-- How it works -->
  <section class="how-it-works" id="how">
    <div class="section-label">Protocol</div>
    <h2 class="section-title">From upload to match in three steps</h2>
    <div class="steps">
      <div class="step">
        <div class="step-num">01</div>
        <div class="step-title">Import researchers</div>
        <div class="step-desc">Upload your Excel sheet or add researchers manually. Each researcher gets a profile with focus areas, H-index, and availability.</div>
      </div>
      <div class="step">
        <div class="step-num">02</div>
        <div class="step-title">Add startups</div>
        <div class="step-desc">Create startup profiles with funding stage and AI needs. Consistent terminology with researcher focus areas yields the best results.</div>
      </div>
      <div class="step">
        <div class="step-num">03</div>
        <div class="step-title">Find your matches</div>
        <div class="step-desc">Press Match and let The Oracle score every researcher against every startup — top 20 candidates per startup, with rationale.</div>
      </div>
    </div>
  </section>

  <!-- CTA -->
  <section class="cta-section">
    <div class="section-label">Ready</div>
    <h2 class="section-title">Start matching today</h2>
    <p class="cta-sub">No sign-up. No account. Open the tool and begin.</p>
    <button class="btn-primary" onclick="launchApp()">Access The Oracle →</button>
  </section>

  <!-- Footer -->
  <footer>
    <div class="footer-logo"><span class="footer-logo-dot"></span> The Oracle · Mila Ventures</div>
    <div class="footer-note">© Mila – Quebec Artificial Intelligence Institute</div>
  </footer>
</div>

<!-- ══════════════════════════════════════════
     APP SHELL
══════════════════════════════════════════ -->
<div id="app-shell">
  <div id="react-root" style="flex:1; overflow:hidden; display:flex; flex-direction:column;"></div>
</div>

<!-- ══════════════════════════════════════════
     REACT APP
══════════════════════════════════════════ -->
<script type="text/babel">
const { useState, useRef, useCallback, useEffect } = React;

const uid = () => Date.now().toString(36) + Math.random().toString(36).slice(2);
const initials = name => name.trim().split(/\s+/).filter(Boolean).map(w => w[0]).join("").slice(0, 2).toUpperCase() || "?";
const stageColor = { "Pre-seed": "#7c3aad", "Seed": "#00b38a", "Series A": "#2255cc", "Series B": "#5c2278", "Series C+": "#3d1152" };

function scoreMatch(startup, researcher) {
  const norm = s => s.toLowerCase().trim();
  let focusPts = 0;
  if (startup.needs?.length && researcher.focus?.length) {
    const overlap = startup.needs.filter(n => researcher.focus.some(f => norm(f) === norm(n))).length;
    focusPts = (overlap / startup.needs.length) * 3;
  }
  let hPts = 0;
  if (researcher.hIndex) {
    if      (researcher.hIndex >= 50) hPts = 1.0;
    else if (researcher.hIndex >= 35) hPts = 0.8;
    else if (researcher.hIndex >= 20) hPts = 0.6;
    else if (researcher.hIndex >= 10) hPts = 0.4;
    else                              hPts = 0.2;
  }
  let indPts = 0;
  if (researcher.industryExp && researcher.industryExp.trim().length > 2) {
    const exp = researcher.industryExp.toLowerCase();
    const strongSignals = /\d+\s*year|\bgoogle\b|\bmeta\b|\bmicrosoft\b|\bamazon\b|\bapple\b|\bopenai\b|\bnvidia\b|\bsenior\b|\blead\b|\bdirector\b|\bfounded\b|\bco-founder\b|\bcto\b|\bceo\b/;
    indPts = strongSignals.test(exp) ? 1.0 : 0.5;
  }
  const raw = focusPts + hPts + indPts;
  const grade = Math.min(5, Math.round(raw * 10) / 10);
  const pct = Math.round((focusPts / 3) * 100);
  return { pct, grade, rationale: null };
}

function gradeColor(g) {
  if (g >= 4.5) return "#3d1152";
  if (g >= 3.5) return "#00b38a";
  if (g >= 2.5) return "#5c2278";
  if (g >= 1.5) return "#7c3aad";
  return "#555";
}

function GradeStars({ grade }) {
  const full  = Math.floor(grade);
  const half  = grade - full >= 0.4 ? 1 : 0;
  const empty = 5 - full - half;
  return (
    <span style={{ letterSpacing: 1, fontSize: 13 }}>
      {"★".repeat(full)}<span style={{ opacity: 0.45 }}>{"★".repeat(half)}</span><span style={{ opacity: 0.15 }}>{"★".repeat(empty)}</span>
    </span>
  );
}

// ── Claude AI matching engine ─────────────────────────────────────────────────
async function runAIMatching(startups, researchers, onProgress) {
  const results = {};

  for (let si = 0; si < startups.length; si++) {
    const startup = startups[si];
    onProgress({ stage: "matching", startupIndex: si, startupName: startup.name, total: startups.length });

    const researcherList = researchers.map((r, idx) =>
      `[${idx}] ${r.name} | ${r.institution || "—"} | H-index: ${r.hIndex ?? "unknown"} | Focus: ${r.focus.join(", ") || "—"} | Industry/Entrepreneurship: ${r.industryExp || "—"}`
    ).join("\n");

    const prompt = `You are an expert at matching AI researchers with startups. Evaluate how well each researcher would fit as a collaborator or advisor at this startup.

STARTUP:
Name: ${startup.name}
Stage: ${startup.stage}
Description: ${startup.desc || "—"}
AI Needs: ${startup.needs.join(", ") || "—"}

RESEARCHERS (index | name | institution | h-index | research focus | industry/entrepreneurship):
${researcherList}

Score EVERY researcher from 0.0 to 5.0:
- 5.0 = exceptional fit (research directly relevant, strong track record, industry experience)
- 3.0 = decent fit (partial overlap or transferable skills)
- 0.0 = no fit at all

Select the TOP 20 by score. Respond ONLY with a JSON array of exactly 20 objects, ordered highest score first:
[{"idx": <integer>, "grade": <0.0-5.0>, "rationale": "<max 12 words>"},...]

Return ONLY the JSON array. No markdown fences, no explanation.`;

    try {
      const response = await fetch("https://api.anthropic.com/v1/messages", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          model: "claude-sonnet-4-20250514",
          max_tokens: 1500,
          messages: [{ role: "user", content: prompt }]
        })
      });
      const data = await response.json();
      const text = (data.content || []).map(b => b.text || "").join("").trim();
      const clean = text.replace(/^```[\w]*\n?/, "").replace(/\n?```$/, "").trim();
      const parsed = JSON.parse(clean);
      results[startup.id] = parsed.map(item => ({
        researcherId: researchers[item.idx]?.id,
        grade: Math.min(5, Math.max(0, parseFloat(item.grade) || 0)),
        rationale: item.rationale || ""
      })).filter(x => x.researcherId);
    } catch (err) {
      console.error("AI matching failed for", startup.name, err);
      results[startup.id] = researchers
        .map(r => ({ researcherId: r.id, ...scoreMatch(startup, r) }))
        .sort((a, b) => b.grade - a.grade)
        .slice(0, 20)
        .map(x => ({ researcherId: x.researcherId, grade: x.grade, rationale: "Scored by keyword matching (AI unavailable)." }));
    }
  }

  onProgress({ stage: "done" });
  return results;
}


function parseResearcherSheet(workbook) {
  const sheet = workbook.Sheets[workbook.SheetNames[0]];
  const rows = XLSX.utils.sheet_to_json(sheet, { defval: "" });
  const results = [], errors = [];
  rows.forEach((row, i) => {
    const name = (row["Researcher Name"] || "").trim();
    if (!name) { errors.push(`Row ${i + 2}: missing "Researcher Name"`); return; }
    const focusRaw = (row["Core Research Focus"] || "").toString().trim();
    const focus = focusRaw ? focusRaw.split(/[,;|\/]+/).map(s => s.trim()).filter(Boolean) : [];
    const hIndex = parseInt(row["H-index"]) || null;
    results.push({ id: uid(), name, institution: (row["University/Affiliation"] || "").trim(), hIndex, industryExp: (row["Industry Experience / Entrepreneurship"] || row["Industry experience"] || "").toString().trim(), focus, open: true });
  });
  return { results, errors };
}

function parseStartupSheet(workbook) {
  const sheet = workbook.Sheets[workbook.SheetNames[0]];
  const rows = XLSX.utils.sheet_to_json(sheet, { defval: "" });
  const results = [], errors = [];
  const VALID_STAGES = ["Pre-seed","Seed","Series A","Series B","Series C+"];
  rows.forEach((row, i) => {
    const name = (row["Startup Name"] || "").trim();
    if (!name) { errors.push(`Row ${i + 2}: missing "Startup Name"`); return; }
    const stageRaw = (row["Stage"] || "").trim();
    const stage = VALID_STAGES.find(s => s.toLowerCase() === stageRaw.toLowerCase()) || "Seed";
    const needsRaw = (row["AI Needs"] || "").toString().trim();
    const needs = needsRaw ? needsRaw.split(/[,;|\/]+/).map(s => s.trim()).filter(Boolean) : [];
    results.push({ id: uid(), name, stage, desc: (row["Description"] || "").trim(), needs });
  });
  return { results, errors };
}

const S = { fontFamily: "'Inter',sans-serif", fontWeight: 700 };
const M = { fontFamily: "'Inter',sans-serif" };
const INP = { width:"100%", background:"#fff", border:"1.5px solid #d0d0d0", borderRadius:6, padding:"9px 12px", color:"#1a1a1a", fontFamily:"'Inter',sans-serif", fontSize:13, outline:"none", boxSizing:"border-box" };
const LBL = { fontFamily:"'Inter',sans-serif", fontSize:11, fontWeight:600, color:"#666", letterSpacing:"0.08em", textTransform:"uppercase", display:"block", marginBottom:6 };

function TagInput({ tags, onChange, placeholder, color="#3d1152", bg="#f5f0fa", border="#d0b8e8" }) {
  const [input, setInput] = useState("");
  const add = () => { const v = input.trim(); if (v && !tags.includes(v)) onChange([...tags, v]); setInput(""); };
  return (
    <div style={{ border:"1.5px solid #d0d0d0", borderRadius:6, padding:"8px 10px", background:"#fff", minHeight:44 }}>
      <div style={{ display:"flex", flexWrap:"wrap", gap:6, marginBottom: tags.length ? 8 : 0 }}>
        {tags.map(t => (
          <span key={t} style={{ display:"inline-flex", alignItems:"center", gap:5, padding:"3px 10px", borderRadius:4, fontSize:12, ...M, background:bg, color, border:`1px solid ${border}` }}>
            {t}<span onClick={() => onChange(tags.filter(x => x !== t))} style={{ cursor:"pointer", color:"#999", fontSize:14 }}>×</span>
          </span>
        ))}
      </div>
      <div style={{ display:"flex", gap:6 }}>
        <input value={input} onChange={e => setInput(e.target.value)}
          onKeyDown={e => { if (e.key==="Enter"||e.key===",") { e.preventDefault(); add(); } }}
          placeholder={placeholder||"Type and press Enter"}
          style={{ flex:1, background:"transparent", border:"none", outline:"none", color:"#1a1a1a", ...M, fontSize:13 }} />
        <button onClick={add} style={{ background:"#f0eaf7", border:"1px solid #d0b8e8", borderRadius:4, color:"#5c2278", fontSize:11, padding:"2px 10px", cursor:"pointer", ...M, fontWeight:600 }}>+</button>
      </div>
    </div>
  );
}

function Modal({ title, onClose, children, wide }) {
  return (
    <div style={{ position:"fixed", inset:0, background:"rgba(61,17,82,0.55)", zIndex:200, display:"flex", alignItems:"center", justifyContent:"center", padding:24 }} onClick={onClose}>
      <div onClick={e => e.stopPropagation()} style={{ background:"#fff", border:"1px solid #e0e0e0", borderRadius:8, padding:"28px 32px", width:"100%", maxWidth: wide ? 700 : 500, boxShadow:"0 16px 60px rgba(61,17,82,0.2)", maxHeight:"92vh", overflowY:"auto" }}>
        <div style={{ display:"flex", alignItems:"center", justifyContent:"space-between", marginBottom:24 }}>
          <div style={{ ...S, fontSize:20, color:"#3d1152" }}>{title}</div>
          <button onClick={onClose} style={{ background:"none", border:"none", color:"#999", fontSize:24, cursor:"pointer", lineHeight:1 }}>×</button>
        </div>
        {children}
      </div>
    </div>
  );
}

function StartupForm({ initial, onSave, onClose }) {
  const [form, setForm] = useState(initial || { name:"", stage:"Seed", desc:"", needs:[] });
  const set = (k,v) => setForm(f => ({...f,[k]:v}));
  const valid = form.name.trim() && form.needs.length > 0;
  return (
    <div style={{ display:"flex", flexDirection:"column", gap:16 }}>
      <div><label style={LBL}>STARTUP NAME *</label><input style={INP} value={form.name} onChange={e => set("name", e.target.value)} placeholder="e.g. Verbly" /></div>
      <div>
        <label style={LBL}>STAGE</label>
        <select style={{...INP, cursor:"pointer"}} value={form.stage} onChange={e => set("stage", e.target.value)}>
          {Object.keys(stageColor).map(s => <option key={s}>{s}</option>)}
        </select>
      </div>
      <div><label style={LBL}>DESCRIPTION</label><input style={INP} value={form.desc} onChange={e => set("desc", e.target.value)} placeholder="One-line description" /></div>
      <div>
        <label style={LBL}>AI NEEDS * <span style={{ color:"#333" }}>(used for matching)</span></label>
        <TagInput tags={form.needs} onChange={v => set("needs", v)} placeholder="e.g. NLP, Computer Vision…" />
      </div>
      <div style={{ display:"flex", gap:10, marginTop:4 }}>
        <button onClick={onClose} style={{ flex:1, padding:"10px", borderRadius:8, border:"1px solid #e0e0e0", background:"#fff", color:"#888", ...M, fontSize:12, cursor:"pointer" }}>Cancel</button>
        <button onClick={() => valid && onSave({...form, id: form.id||uid()})} disabled={!valid}
          style={{ flex:2, padding:"10px", borderRadius:8, border:"none", background: valid?"#f59e0b":"#1e1e2a", color: valid?"#fff":"#bbb", ...M, fontSize:12, fontWeight:600, cursor: valid?"pointer":"not-allowed" }}>
          Save Startup
        </button>
      </div>
    </div>
  );
}

function ResearcherForm({ initial, onSave, onClose }) {
  const [form, setForm] = useState(initial || { name:"", institution:"", hIndex:null, industryExp:"", focus:[], open:true });
  const set = (k,v) => setForm(f => ({...f,[k]:v}));
  const valid = form.name.trim();
  return (
    <div style={{ display:"flex", flexDirection:"column", gap:16 }}>
      <div style={{ display:"grid", gridTemplateColumns:"1fr 1fr", gap:12 }}>
        <div style={{ gridColumn:"1/-1" }}><label style={LBL}>RESEARCHER NAME *</label><input style={INP} value={form.name} onChange={e => set("name", e.target.value)} placeholder="e.g. Dr. Priya Nair" /></div>
        <div><label style={LBL}>UNIVERSITY / AFFILIATION</label><input style={INP} value={form.institution} onChange={e => set("institution", e.target.value)} placeholder="e.g. MIT CSAIL" /></div>
        <div><label style={LBL}>H-INDEX</label><input style={INP} type="number" value={form.hIndex||""} onChange={e => set("hIndex", e.target.value ? parseInt(e.target.value) : null)} placeholder="e.g. 42" /></div>
        <div style={{ gridColumn:"1/-1" }}><label style={LBL}>INDUSTRY EXPERIENCE / ENTREPRENEURSHIP</label><input style={INP} value={form.industryExp||""} onChange={e => set("industryExp", e.target.value)} placeholder="e.g. 5 years at Google Brain, co-founded 2 startups" /></div>
      </div>
      <div>
        <label style={LBL}>CORE RESEARCH FOCUS <span style={{ color:"#333" }}>(used for matching)</span></label>
        <TagInput tags={form.focus} onChange={v => set("focus", v)} placeholder="e.g. NLP, Reinforcement Learning…" />
      </div>
      <div>
        <label style={LBL}>AVAILABILITY</label>
        <div style={{ display:"flex", gap:8 }}>
          {[true,false].map(val => (
            <button key={String(val)} onClick={() => set("open", val)} style={{ flex:1, padding:"9px", borderRadius:8, border:`1px solid ${form.open===val?(val?"#10b981":"#ef4444"):"#2a2a3a"}`, background: form.open===val?(val?"#10b98115":"#ef444415"):"#0d0d18", color: form.open===val?(val?"#10b981":"#ef4444"):"#444", ...M, fontSize:12, cursor:"pointer", transition:"all 0.15s" }}>
              {val ? "● Available" : "○ Unavailable"}
            </button>
          ))}
        </div>
      </div>
      <div style={{ display:"flex", gap:10, marginTop:4 }}>
        <button onClick={onClose} style={{ flex:1, padding:"10px", borderRadius:8, border:"1px solid #e0e0e0", background:"#fff", color:"#888", ...M, fontSize:12, cursor:"pointer" }}>Cancel</button>
        <button onClick={() => valid && onSave({...form, id: form.id||uid()})} disabled={!valid}
          style={{ flex:2, padding:"10px", borderRadius:8, border:"none", background: valid?"#3d1152":"#e0e0e0", color: valid?"#fff":"#bbb", ...M, fontSize:12, fontWeight:600, cursor: valid?"pointer":"not-allowed" }}>
          Save Researcher
        </button>
      </div>
    </div>
  );
}

function ExcelUploadModal({ onImport, onClose, mode="researcher" }) {
  const isStartup = mode === "startup";
  const accentColor = isStartup ? "#f59e0b" : "#10b981";
  const accentBg    = isStartup ? "#f59e0b15" : "#10b98115";
  const accentBrd   = isStartup ? "#f59e0b33" : "#10b98133";

  const COLUMNS = isStartup
    ? [["Startup Name","Required — startup's name"],["Stage","Pre-seed / Seed / Series A / Series B / Series C+"],["Description","One-line description"],["AI Needs","Comma-separated (used for matching)"]]
    : [["Researcher Name","Required"],["University/Affiliation","Institution"],["H-index","Numeric"],["Core Research Focus","Comma-separated (used for matching)"],["Industry Experience / Entrepreneurship","Free text"]];

  const PREVIEW_HEADERS = isStartup
    ? ["Name","Stage","Description","AI Needs"]
    : ["Name","Affiliation","H-index","Research Focus","Industry / Entrepreneurship"];

  const renderRow = (r, i) => isStartup
    ? (
      <tr key={r.id} style={{ borderBottom:"1px solid #f0f0f0", background: i%2===0?"transparent":"#faf8fc" }}>
        <td style={{ padding:"9px 14px", color:"#e0dcd4", whiteSpace:"nowrap" }}>{r.name}</td>
        <td style={{ padding:"9px 14px" }}><span style={{ background:`${stageColor[r.stage]||"#888"}22`, color:stageColor[r.stage]||"#888", borderRadius:999, padding:"2px 9px", fontSize:10, ...M }}>{r.stage}</span></td>
        <td style={{ padding:"9px 14px", color:"#666", maxWidth:160, overflow:"hidden", textOverflow:"ellipsis", whiteSpace:"nowrap" }}>{r.desc||"—"}</td>
        <td style={{ padding:"9px 14px" }}>
          <div style={{ display:"flex", flexWrap:"wrap", gap:4 }}>
            {r.needs.length ? r.needs.map(n => <span key={n} style={{ background:"#f0ecf8", color:"#5c2278", border:"1px solid #d0b8e8", borderRadius:4, padding:"1px 8px", fontSize:10 }}>{n}</span>) : <span style={{ color:"#333" }}>—</span>}
          </div>
        </td>
      </tr>
    ) : (
      <tr key={r.id} style={{ borderBottom:"1px solid #f0f0f0", background: i%2===0?"transparent":"#faf8fc" }}>
        <td style={{ padding:"9px 14px", color:"#e0dcd4", whiteSpace:"nowrap" }}>{r.name}</td>
        <td style={{ padding:"9px 14px", color:"#777", whiteSpace:"nowrap" }}>{r.institution||"—"}</td>
        <td style={{ padding:"9px 14px", color: r.hIndex?"#f59e0b":"#444", textAlign:"center" }}>{r.hIndex??"—"}</td>
        <td style={{ padding:"9px 14px" }}>
          <div style={{ display:"flex", flexWrap:"wrap", gap:4 }}>
            {r.focus.length ? r.focus.map(f => <span key={f} style={{ background:"#f0ecf8", color:"#5c2278", border:"1px solid #d0b8e8", borderRadius:4, padding:"1px 8px", fontSize:10 }}>{f}</span>) : <span style={{ color:"#333" }}>—</span>}
          </div>
        </td>
        <td style={{ padding:"9px 14px", color:"#666", maxWidth:140, overflow:"hidden", textOverflow:"ellipsis", whiteSpace:"nowrap" }}>{r.industryExp||"—"}</td>
      </tr>
    );

  const [dragging, setDragging] = useState(false);
  const [preview, setPreview] = useState(null);
  const [parseError, setParseError] = useState(null);
  const fileRef = useRef();

  const processFile = file => {
    setParseError(null); setPreview(null);
    const reader = new FileReader();
    reader.onload = e => {
      try {
        const wb = XLSX.read(e.target.result, { type:"array" });
        const { results, errors } = isStartup ? parseStartupSheet(wb) : parseResearcherSheet(wb);
        setPreview({ results, errors, filename: file.name });
      } catch(err) { setParseError("Could not parse file: " + err.message); }
    };
    reader.readAsArrayBuffer(file);
  };

  const onDrop = useCallback(e => {
    e.preventDefault(); setDragging(false);
    const file = e.dataTransfer.files[0];
    if (file) processFile(file);
  }, []);

  const entityLabel = isStartup ? "startup" : "researcher";

  return (
    <Modal title={`Import ${isStartup ? "Startups" : "Researchers"} from Excel`} onClose={onClose} wide>
      {!preview ? (
        <>
          <div onDragOver={e => { e.preventDefault(); setDragging(true); }} onDragLeave={() => setDragging(false)} onDrop={onDrop}
            onClick={() => fileRef.current.click()}
            style={{ border:`2px dashed ${dragging ? accentColor : "#d0d0d0"}`, borderRadius:6, padding:"44px 32px", textAlign:"center", cursor:"pointer", background: dragging ? accentBg : "#f9f9f9", transition:"all 0.2s", marginBottom:20 }}>
            <div style={{ fontSize:36, marginBottom:12 }}>📊</div>
            <div style={{ ...S, fontSize:18, color:"#ccc", marginBottom:8 }}>Drop your Excel or CSV file here</div>
            <div style={{ ...M, fontSize:12, color:"#444" }}>or click to browse · .xlsx / .xls / .csv</div>
            <input ref={fileRef} type="file" accept=".xlsx,.xls,.csv" onChange={e => { if(e.target.files[0]) processFile(e.target.files[0]); }} style={{ display:"none" }} />
          </div>

          {/* Google Sheets hint */}
          <div style={{ background: accentBg, border:`1px solid ${accentBrd}`, borderRadius:6, padding:"12px 16px", marginBottom:16, display:"flex", alignItems:"flex-start", gap:10 }}>
            <span style={{ fontSize:16, flexShrink:0 }}>💡</span>
            <div style={{ ...M, fontSize:11, color:"#888", lineHeight:1.7 }}>
              <span style={{ color: accentColor, fontWeight:600 }}>Using Google Sheets?</span> Open your sheet → File → Download → Microsoft Excel (.xlsx), then upload that file here.
            </div>
          </div>

          <div style={{ background:"#fff", border:"1px solid #e0e0e0", borderRadius:6, padding:"16px 20px" }}>
            <div style={{ ...M, fontSize:11, color:"#888", letterSpacing:"0.08em", marginBottom:12 }}>EXPECTED COLUMN HEADERS</div>
            <div style={{ display:"grid", gridTemplateColumns:"auto 1fr", gap:"8px 16px", alignItems:"start" }}>
              {COLUMNS.map(([col, desc]) => (
                <React.Fragment key={col}>
                  <span style={{ ...M, fontSize:11, color: accentColor, background: accentBg, border:`1px solid ${accentBrd}`, borderRadius:6, padding:"2px 9px", whiteSpace:"nowrap" }}>{col}</span>
                  <span style={{ ...M, fontSize:11, color:"#444", paddingTop:3 }}>{desc}</span>
                </React.Fragment>
              ))}
            </div>
          </div>
          {parseError && <div style={{ marginTop:14, ...M, fontSize:12, color:"#ef4444", background:"#ef444410", border:"1px solid #ef444430", borderRadius:8, padding:"10px 14px" }}>{parseError}</div>}
        </>
      ) : (
        <>
          <div style={{ display:"flex", alignItems:"center", gap:10, marginBottom:16, padding:"12px 16px", background:"#f9f9f9", borderRadius:6, border:"1px solid #e0e0e0" }}>
            <span style={{ fontSize:20 }}>📄</span>
            <div>
              <div style={{ ...M, fontSize:12, color:"#ccc" }}>{preview.filename}</div>
              <div style={{ ...M, fontSize:11, color:"#555" }}>{preview.results.length} {entityLabel}{preview.results.length!==1?"s":""} found</div>
            </div>
            <button onClick={() => setPreview(null)} style={{ marginLeft:"auto", background:"none", border:"1px solid #d0d0d0", borderRadius:4, color:"#888", ...M, fontSize:11, padding:"4px 12px", cursor:"pointer" }}>← Re-upload</button>
          </div>
          {preview.errors.length > 0 && (
            <div style={{ marginBottom:14, background:"#fff8e8", border:"1px solid #f59e0b55", borderRadius:8, padding:"10px 14px" }}>
              <div style={{ ...M, fontSize:11, color:"#b87800", marginBottom:6 }}>⚠ {preview.errors.length} row{preview.errors.length!==1?"s":""} skipped</div>
              {preview.errors.map((e,i) => <div key={i} style={{ ...M, fontSize:11, color:"#888" }}>{e}</div>)}
            </div>
          )}
          <div style={{ border:"1px solid #1e1e2a", borderRadius:10, overflow:"hidden", marginBottom:20 }}>
            <div style={{ overflowX:"auto", maxHeight:280, overflowY:"auto" }}>
              <table style={{ width:"100%", borderCollapse:"collapse", ...M, fontSize:11 }}>
                <thead>
                  <tr style={{ background:"#f4f4f4", borderBottom:"1px solid #e0e0e0" }}>
                    {PREVIEW_HEADERS.map(h => (
                      <th key={h} style={{ padding:"10px 14px", textAlign:"left", color:"#888", letterSpacing:"0.05em", whiteSpace:"nowrap" }}>{h}</th>
                    ))}
                  </tr>
                </thead>
                <tbody>{preview.results.map((r,i) => renderRow(r,i))}</tbody>
              </table>
            </div>
          </div>
          <div style={{ display:"flex", gap:10 }}>
            <button onClick={onClose} style={{ flex:1, padding:"11px", borderRadius:8, border:"1px solid #e0e0e0", background:"#fff", color:"#888", ...M, fontSize:12, cursor:"pointer" }}>Cancel</button>
            <button onClick={() => onImport(preview.results)} disabled={preview.results.length===0}
              style={{ flex:2, padding:"11px", borderRadius:8, border:"none", background: preview.results.length ? accentColor : "#e0e0e0", color: preview.results.length?"#fff":"#bbb", ...M, fontSize:12, fontWeight:700, cursor: preview.results.length?"pointer":"not-allowed" }}>
              Import {preview.results.length} {entityLabel.charAt(0).toUpperCase()+entityLabel.slice(1)}{preview.results.length!==1?"s":""} →
            </button>
          </div>
        </>
      )}
    </Modal>
  );
}

function App() {
  const [view, setView] = useState("match");
  const [browseTab, setBrowseTab] = useState("startups");
  const [startups, setStartups] = useState([]);
  const [researchers, setResearchers] = useState([]);
  const [selected, setSelected] = useState(null);
  const [filterAvailable, setFilterAvailable] = useState(false);
  const [hoveredMatch, setHoveredMatch] = useState(null);
  const [modal, setModal] = useState(null);
  const [deleteConfirm, setDeleteConfirm] = useState(null);
  const [showExcel, setShowExcel] = useState(false);
  const [showStartupExcel, setShowStartupExcel] = useState(false);
  const [toast, setToast] = useState(null);

  // AI matching state
  const [aiResults, setAiResults] = useState(null);   // { startupId: [{researcherId, grade, rationale}] }
  const [aiProgress, setAiProgress] = useState(null); // null | { stage, startupIndex, startupName, total }
  const [aiRan, setAiRan] = useState(false);

  const saveStartup = s => { setStartups(p => p.find(x=>x.id===s.id) ? p.map(x=>x.id===s.id?s:x) : [...p,s]); setModal(null); };
  const saveResearcher = r => { setResearchers(p => p.find(x=>x.id===r.id) ? p.map(x=>x.id===r.id?r:x) : [...p,r]); setModal(null); };
  const doDelete = () => {
    if (deleteConfirm.type==="startup") { setStartups(p=>p.filter(x=>x.id!==deleteConfirm.item.id)); if(selected===deleteConfirm.item.id) setSelected(null); }
    else setResearchers(p=>p.filter(x=>x.id!==deleteConfirm.item.id));
    setDeleteConfirm(null);
  };
  const handleImport = results => {
    setResearchers(results); setShowExcel(false);
    setAiResults(null); setAiRan(false);
    setToast(`✓ Imported ${results.length} researcher${results.length!==1?"s":""}`);
    setTimeout(() => setToast(null), 3500);
  };
  const handleStartupImport = results => {
    setStartups(results); setShowStartupExcel(false);
    setAiResults(null); setAiRan(false);
    setToast(`✓ Imported ${results.length} startup${results.length!==1?"s":""}`);
    setTimeout(() => setToast(null), 3500);
  };

  const triggerAIMatching = async () => {
    if (!startups.length || !researchers.length) return;
    setAiResults(null); setAiRan(false);
    setAiProgress({ stage: "starting" });
    try {
      const results = await runAIMatching(startups, researchers, setAiProgress);
      setAiResults(results);
      setAiRan(true);
      // Auto-select first startup
      if (!selected && startups.length) setSelected(startups[0].id);
    } catch(e) {
      console.error(e);
    }
    setAiProgress(null);
  };

  const selectedStartup = startups.find(s=>s.id===selected);

  // Build matches for selected startup
  const matches = (() => {
    if (!selectedStartup) return [];
    if (aiResults && aiResults[selectedStartup.id]) {
      return aiResults[selectedStartup.id]
        .map(item => {
          const r = researchers.find(x => x.id === item.researcherId);
          if (!r) return null;
          const { pct } = scoreMatch(selectedStartup, r);
          return { ...r, grade: item.grade, rationale: item.rationale, pct, aiScored: true };
        })
        .filter(Boolean)
        .filter(r => !filterAvailable || r.open);
    }
    return researchers
      .map(r => ({ ...r, ...scoreMatch(selectedStartup, r) }))
      .filter(r => !filterAvailable || r.open)
      .sort((a,b) => b.grade - a.grade)
      .slice(0, 20);
  })();

  return (
    <div style={{ height:"100%", display:"flex", flexDirection:"column", background:"#f4f4f4", color:"#1a1a1a" }}>

      {toast && (
        <div className="r-toast" style={{ position:"fixed", bottom:28, left:"50%", transform:"translateX(-50%)", background:"#00b38a", color:"#fff", ...M, fontSize:13, fontWeight:600, padding:"10px 24px", borderRadius:4, zIndex:300, pointerEvents:"none", boxShadow:"0 8px 32px rgba(0,179,138,0.3)", whiteSpace:"nowrap" }}>
          {toast}
        </div>
      )}

      {/* Header */}
      <div style={{ borderBottom:"1px solid #e0e0e0", padding:"0 32px", height:64, display:"flex", alignItems:"center", justifyContent:"space-between", background:"#fff", flexShrink:0 }}>
        <div style={{ display:"flex", alignItems:"center", gap:24 }}>
          <button onClick={() => window.showLanding()} style={{ background:"none", border:"none", cursor:"pointer", ...M, fontSize:13, fontWeight:500, color:"#888", display:"flex", alignItems:"center", gap:6 }}>← Home</button>
          <div style={{ width:1, height:20, background:"#e0e0e0" }}></div>
          <div style={{ display:"flex", alignItems:"center", gap:8, ...S, fontSize:17, color:"#3d1152" }}>
            <span style={{ width:8, height:8, borderRadius:"50%", background:"#00b38a", display:"inline-block" }}></span>
            Mila Researcher Matching
          </div>
        </div>
        <div style={{ display:"flex", gap:2, background:"#f4f4f4", borderRadius:6, padding:3, border:"1px solid #e0e0e0" }}>
          {["match","manage"].map(v => (
            <button key={v} className="r-tbtn" onClick={() => setView(v)} style={{ padding:"7px 18px", borderRadius:4, ...M, fontSize:12, fontWeight:600, textTransform:"uppercase", letterSpacing:"0.06em", color: view===v?"#fff":"#888", background: view===v?"#3d1152":"transparent" }}>{v}</button>
          ))}
        </div>
      </div>

      {/* ── MATCH VIEW ── */}
      {view==="match" && (
        <div style={{ display:"flex", flex:1, overflow:"hidden", position:"relative" }}>

          {/* AI Progress overlay */}
          {aiProgress && (
            <div style={{ position:"absolute", inset:0, background:"rgba(61,17,82,0.92)", zIndex:50, display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center", gap:24, backdropFilter:"blur(8px)" }}>
              <div style={{ fontSize:48, animation:"r-spin 1.2s linear infinite" }}>⚙️</div>
              <div style={{ ...S, fontSize:26, fontWeight:900, color:"#fff", textAlign:"center" }}>
                Matching Mila Scientists<br/><span style={{ color:"#00b38a" }}>with Startups…</span>
              </div>
              {aiProgress.stage==="matching" && (
                <div style={{ textAlign:"center" }}>
                  <div style={{ ...M, fontSize:13, color:"rgba(255,255,255,0.6)", marginBottom:12 }}>
                    Processing <span style={{ color:"#f0ece4" }}>{aiProgress.startupName}</span>
                  </div>
                  <div style={{ width:320, height:4, background:"#e8e0f0", borderRadius:2, overflow:"hidden" }}>
                    <div style={{ height:"100%", background:"linear-gradient(90deg,#00b38a,#00d4a8)", borderRadius:2, width:`${((aiProgress.startupIndex+1)/aiProgress.total)*100}%`, transition:"width 0.5s" }} />
                  </div>
                  <div style={{ ...M, fontSize:11, color:"rgba(255,255,255,0.4)", marginTop:8 }}>{aiProgress.startupIndex+1} / {aiProgress.total} startups</div>
                </div>
              )}
              {aiProgress.stage==="starting" && <div style={{ ...M, fontSize:13, color:"rgba(255,255,255,0.5)" }}>Preparing data…</div>}
            </div>
          )}

          {/* Left panel */}
          <div style={{ width:280, borderRight:"1px solid #e0e0e0", overflowY:"auto", padding:"16px 12px", flexShrink:0, display:"flex", flexDirection:"column", background:"#fff" }}>

            {/* THE BIG BUTTON */}
            <button
              onClick={triggerAIMatching}
              disabled={!startups.length || !researchers.length || !!aiProgress}
              style={{
                width:"100%", marginBottom:16, padding:"14px 10px",
                background: (!startups.length||!researchers.length||!!aiProgress) ? "#e8e0f0" : "linear-gradient(135deg,#3d1152,#7c3aad)",
                border:"none", borderRadius:12, cursor: (!startups.length||!researchers.length||!!aiProgress)?"not-allowed":"pointer",
                color: (!startups.length||!researchers.length||!!aiProgress)?"#bbb":"#fff",
                fontFamily:"'Inter',sans-serif", fontSize:13, fontWeight:800, lineHeight:1.4,
                boxShadow: (!startups.length||!researchers.length||!!aiProgress)?"none":"0 4px 24px rgba(249,115,22,0.4)",
                transition:"all 0.2s", textAlign:"center",
                letterSpacing:"-0.01em",
              }}
            >
              {aiProgress ? "⚙️ Matching in progress…" : "Match Mila Scientists and Create Mila Startups!!!"}
            </button>

            {aiRan && (
              <div style={{ ...M, fontSize:10, color:"#00b38a", background:"#e6f8f4", border:"1px solid #00b38a33", borderRadius:8, padding:"7px 10px", marginBottom:12, textAlign:"center" }}>
                ✓ AI matching complete — select a startup
              </div>
            )}

            <div style={{ display:"flex", justifyContent:"space-between", alignItems:"center", marginBottom:10, padding:"0 4px" }}>
              <div style={{ ...M, fontSize:10, color:"#444", letterSpacing:"0.15em" }}>STARTUPS ({startups.length})</div>
              <button onClick={() => setModal({type:"startup",editing:null})} style={{ ...M, fontSize:11, color:"#3d1152", background:"#f5f0fa", border:"1px solid #d0b8e8", borderRadius:4, padding:"4px 10px", cursor:"pointer" }}>+ Add</button>
            </div>
            {startups.length===0
              ? <div style={{ textAlign:"center", ...M, fontSize:12, color:"#2a2a3a", marginTop:40, lineHeight:2 }}>No startups yet.<br/>Add one in Manage.</div>
              : startups.map(s => {
                const hasAI = aiResults && aiResults[s.id];
                return (
                  <div key={s.id} className={`r-card r-glow ${selected===s.id?"r-sel":""}`} onClick={() => setSelected(s.id)}
                    style={{ marginBottom:8, padding:"12px 13px", borderRadius:10, background: selected===s.id?"#f5f0fa":"#f9f9f9", cursor:"pointer", border:"1px solid transparent" }}>
                    <div style={{ display:"flex", alignItems:"center", gap:9 }}>
                      <div style={{ width:32, height:32, borderRadius:7, flexShrink:0, background: selected===s.id?"#f5f0fa":"#f4f4f4", border:`1px solid ${selected===s.id?"#3d1152":"#e0e0e0"}`, display:"flex", alignItems:"center", justifyContent:"center", ...M, fontSize:11, color: selected===s.id?"#3d1152":"#999", fontWeight:700 }}>
                        {initials(s.name)}
                      </div>
                      <div style={{ flex:1, minWidth:0 }}>
                        <div style={{ ...S, fontSize:13, fontWeight:700, color: selected===s.id?"#3d1152":"#555", whiteSpace:"nowrap", overflow:"hidden", textOverflow:"ellipsis" }}>{s.name}</div>
                        <div style={{ display:"flex", alignItems:"center", gap:5 }}>
                          <span className="r-chip" style={{ background:`${stageColor[s.stage]||"#888"}22`, color:stageColor[s.stage]||"#888" }}>{s.stage}</span>
                          {hasAI && <span style={{ fontSize:10, color:"#3d1152" }}>✦ AI</span>}
                        </div>
                      </div>
                    </div>
                    {s.needs.length>0 && <div style={{ ...M, fontSize:10, color:"#bbb", marginTop:7, whiteSpace:"nowrap", overflow:"hidden", textOverflow:"ellipsis" }}>{s.needs.join(" · ")}</div>}
                  </div>
                );
              })}
          </div>

          {/* Right panel */}
          <div style={{ flex:1, overflowY:"auto", padding:"22px 28px", background:"#f4f4f4" }}>
            {!selectedStartup ? (
              <div style={{ height:"100%", display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center", gap:16, textAlign:"center" }}>
                {!startups.length || !researchers.length ? (
                  <>
                    <div style={{ fontSize:44, opacity:0.15 }}>🚀</div>
                    <div style={{ ...S, fontSize:20, color:"#333" }}>Add startups and researchers first</div>
                    <button onClick={() => setView("manage")} style={{ ...M, fontSize:12, color:"#3d1152", background:"#f5f0fa", border:"1px solid #d0b8e8", borderRadius:4, padding:"9px 20px", cursor:"pointer" }}>Go to Manage →</button>
                  </>
                ) : (
                  <>
                    <div style={{ ...S, fontSize:20, color:"#3d1152" }}>Ready to match</div>
                    <div style={{ ...M, fontSize:12, color:"#bbb" }}>{startups.length} startups · {researchers.length} researchers loaded</div>
                    <button
                      onClick={triggerAIMatching}
                      style={{ padding:"16px 32px", background:"linear-gradient(135deg,#3d1152,#7c3aad)", border:"none", borderRadius:4, color:"#fff", fontFamily:"Inter,sans-serif", fontSize:15, fontWeight:900, cursor:"pointer", boxShadow:"0 4px 24px rgba(61,17,82,0.3)", letterSpacing:"-0.01em" }}>
                      Match Mila Scientists and Create Mila Startups!!!
                    </button>
                  </>
                )}
              </div>
            ) : researchers.length===0 ? (
              <div style={{ height:"100%", display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center", gap:12, textAlign:"center" }}>
                <div style={{ fontSize:38, opacity:0.15 }}>🔬</div>
                <div style={{ ...S, fontSize:20, color:"#ccc" }}>No researchers yet</div>
                <button onClick={() => { setView("manage"); setBrowseTab("researchers"); }} style={{ ...M, fontSize:12, color:"#00b38a", background:"#e6f8f4", border:"1px solid #00b38a33", borderRadius:8, padding:"9px 20px", cursor:"pointer" }}>Go to Manage →</button>
              </div>
            ) : (
              <>
                <div style={{ display:"flex", alignItems:"flex-start", justifyContent:"space-between", marginBottom:20 }}>
                  <div>
                    <div style={{ display:"flex", alignItems:"center", gap:10 }}>
                      <div style={{ ...S, fontSize:22, fontWeight:900, color:"#fff" }}>
                        {selectedStartup.name}
                      </div>
                      {aiResults?.[selectedStartup.id]
                        ? <span style={{ ...M, fontSize:10, color:"#3d1152", background:"#f5f0fa", border:"1px solid #d0b8e8", borderRadius:4, padding:"3px 9px" }}>✦ AI-scored</span>
                        : <span style={{ ...M, fontSize:10, color:"#888", background:"#f4f4f4", border:"1px solid #e0e0e0", borderRadius:4, padding:"3px 9px" }}>keyword scoring</span>}
                    </div>
                    {selectedStartup.needs.length
                      ? <div style={{ ...M, fontSize:11, color:"#888", marginTop:5 }}>Needs: {selectedStartup.needs.join(" · ")}</div>
                      : <div style={{ ...M, fontSize:11, color:"#f59e0b88", marginTop:5 }}>⚠ No AI needs defined</div>}
                    <div style={{ ...M, fontSize:11, color:"#bbb", marginTop:3 }}>Top {matches.length} researchers shown</div>
                  </div>
                  <label style={{ display:"flex", alignItems:"center", gap:8, cursor:"pointer", ...M, fontSize:11, color:"#555", flexShrink:0, marginTop:4 }}>
                    <div onClick={() => setFilterAvailable(f=>!f)} style={{ width:32, height:17, borderRadius:10, background: filterAvailable?"#3d1152":"#e0e0e0", border:"1px solid #ccc", position:"relative", transition:"background 0.2s", cursor:"pointer" }}>
                      <div style={{ position:"absolute", top:2, left: filterAvailable?15:2, width:11, height:11, borderRadius:"50%", background:"#fff", transition:"left 0.2s" }} />
                    </div>
                    Available only
                  </label>
                </div>
                <div style={{ display:"grid", gap:10 }}>
                  {matches.map((r,i) => (
                    <div key={r.id} className="r-card r-glow"
                      onMouseEnter={() => setHoveredMatch(r.id)} onMouseLeave={() => setHoveredMatch(null)}
                      style={{ background: hoveredMatch===r.id?"#f5f0fa":"#fff", borderRadius:12, padding:"16px 20px", border:`1px solid ${r.grade>=4.5?"#3d115240":r.grade>=3.5?"#00b38a40":"#e0e0e0"}`, animation:`r-fu .35s ${i*.04}s both` }}>
                      <div style={{ display:"flex", alignItems:"center", gap:12, marginBottom: r.rationale ? 8 : 10 }}>
                        {/* Rank badge */}
                        <div style={{ width:22, flexShrink:0, ...M, fontSize:11, color:"#ccc", textAlign:"center" }}>#{i+1}</div>
                        <div style={{ width:38, height:38, borderRadius:9, flexShrink:0, background: r.grade>=4.5?"#f5f0fa":"#f4f4f4", border:`2px solid ${r.grade>=4.5?"#3d1152":r.grade>=3.5?"#00b38a":"#e0e0e0"}`, display:"flex", alignItems:"center", justifyContent:"center", ...M, fontSize:11, color: r.grade>=4.5?"#3d1152":r.grade>=3.5?"#00b38a":"#bbb", fontWeight:700 }}>
                          {initials(r.name)}
                        </div>
                        <div style={{ flex:1, minWidth:0 }}>
                          <div style={{ display:"flex", alignItems:"center", gap:7, flexWrap:"wrap" }}>
                            <span style={{ ...S, fontSize:15, fontWeight:700, color:"#f0ece4" }}>{r.name}</span>
                            {r.grade>=4.5 && <span className="r-chip" style={{ background:"#f5f0fa", color:"#3d1152" }}>★ Top Match</span>}
                            {r.hIndex && <span className="r-chip" style={{ background:"#e8f0ff", color:"#2255cc" }}>H-index {r.hIndex}</span>}
                            <span className="r-chip" style={{ background: r.open?"#e6f8f4":"#ffeaea", color: r.open?"#00b38a":"#e53e3e", marginLeft:"auto" }}>{r.open?"● Available":"○ Unavailable"}</span>
                          </div>
                          <div style={{ ...M, fontSize:11, color:"#888", marginTop:2 }}>{[r.institution,r.industryExp].filter(Boolean).join(" · ")}</div>
                        </div>
                        <div style={{ textAlign:"right", flexShrink:0 }}>
                          <div style={{ ...S, fontSize:26, fontWeight:900, color: gradeColor(r.grade), lineHeight:1 }}>{r.grade.toFixed(1)}<span style={{ fontSize:11, fontWeight:400, color:"#444" }}>/5</span></div>
                          <div style={{ color: gradeColor(r.grade), margin:"3px 0 1px" }}><GradeStars grade={r.grade} /></div>
                        </div>
                      </div>
                      {/* AI rationale */}
                      {r.rationale && (
                        <div style={{ ...M, fontSize:11, color:"#666", background:"#f5f0fa", border:"1px solid #d0b8e8", borderRadius:7, padding:"6px 10px", marginBottom:9, fontStyle:"italic" }}>
                          ✦ {r.rationale}
                        </div>
                      )}
                      <div style={{ height:2, background:"#e8e0f0", borderRadius:2, overflow:"hidden", marginBottom:9 }}>
                        <div className="r-mbar" style={{ height:"100%", borderRadius:2, width:`${(r.grade/5)*100}%`, background: r.grade>=4.5?"linear-gradient(90deg,#3d1152,#7c3aad)":r.grade>=3.5?"linear-gradient(90deg,#00b38a,#00d4a8)":r.grade>=2.5?"linear-gradient(90deg,#5c2278,#9b4dca)":"#e0e0e0" }} />
                      </div>
                      <div style={{ display:"flex", flexWrap:"wrap", gap:5 }}>
                        {r.focus.map(f => {
                          const matched = selectedStartup.needs.some(n=>n.toLowerCase().trim()===f.toLowerCase().trim());
                          return <span key={f} className="r-chip" style={{ background: matched?"#f5f0fa":"#f4f4f4", color: matched?"#3d1152":"#999", border:`1px solid ${matched?"#3d115244":"#e0e0e0"}` }}>{f}</span>;
                        })}
                      </div>
                    </div>
                  ))}
                </div>
              </>
            )}
          </div>
        </div>
      )}

      {/* ── MANAGE VIEW ── */}
      {view==="manage" && (
        <div style={{ flex:1, overflowY:"auto", padding:"22px 32px", background:"#f4f4f4" }}>
          <div style={{ display:"flex", gap:3, background:"#f4f4f4", borderRadius:6, padding:3, border:"1px solid #e0e0e0", width:"fit-content", marginBottom:20 }}>
            {["startups","researchers"].map(t => (
              <button key={t} className="r-tbtn" onClick={() => setBrowseTab(t)} style={{ padding:"7px 18px", borderRadius:6, ...M, fontSize:11, letterSpacing:"0.08em", textTransform:"uppercase", color: browseTab===t?"#fff":"#888", background: browseTab===t?"#3d1152":"transparent", fontWeight: browseTab===t?700:400 }}>{t}</button>
            ))}
          </div>

          {browseTab==="startups" && <>
            <div style={{ display:"flex", alignItems:"center", justifyContent:"space-between", marginBottom:16 }}>
              <div style={{ ...M, fontSize:11, color:"#444" }}>{startups.length} startup{startups.length!==1?"s":""}</div>
              <div style={{ display:"flex", gap:8 }}>
                <button onClick={() => setShowStartupExcel(true)} style={{ background:"#f5f0fa", border:"1px solid #d0b8e8", borderRadius:4, color:"#5c2278", ...M, fontSize:12, fontWeight:600, padding:"8px 16px", cursor:"pointer" }}>📊 Import Excel</button>
                <button onClick={() => setModal({type:"startup",editing:null})} style={{ background:"#3d1152", border:"none", borderRadius:4, color:"#fff", ...M, fontSize:12, fontWeight:700, padding:"8px 18px", cursor:"pointer" }}>+ Add Manually</button>
              </div>
            </div>
            {startups.length===0
              ? <div style={{ display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center", height:240, gap:14, textAlign:"center" }}>
                  <div style={{ fontSize:36, opacity:0.15 }}>🚀</div>
                  <div style={{ ...S, fontSize:18, color:"#aaa" }}>No startups yet</div>
                  <div style={{ display:"flex", gap:10 }}>
                    <button onClick={() => setShowStartupExcel(true)} style={{ ...M, fontSize:12, color:"#3d1152", background:"#f5f0fa", border:"1px solid #d0b8e8", borderRadius:4, padding:"9px 18px", cursor:"pointer" }}>📊 Import from Excel</button>
                    <button onClick={() => setModal({type:"startup",editing:null})} style={{ ...M, fontSize:12, color:"#888", background:"#f4f4f4", border:"1px solid #e0e0e0", borderRadius:4, padding:"9px 18px", cursor:"pointer" }}>+ Add Manually</button>
                  </div>
                </div>
              : <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fill,minmax(280px,1fr))", gap:12 }}>
                  {startups.map((s,i) => (
                    <div key={s.id} className="r-card r-glow" style={{ background:"#fff", borderRadius:8, padding:"16px", border:"1px solid #e0e0e0", animation:`r-fu .35s ${i*.05}s both` }}>
                      <div style={{ display:"flex", alignItems:"center", gap:10, marginBottom:10 }}>
                        <div style={{ width:36, height:36, borderRadius:8, flexShrink:0, background:"#f4f4f4", border:"1px solid #e0e0e0", display:"flex", alignItems:"center", justifyContent:"center", ...M, fontSize:11, color:"#999", fontWeight:700 }}>{initials(s.name)}</div>
                        <div style={{ flex:1, minWidth:0 }}>
                          <div style={{ ...S, fontSize:14, fontWeight:700, color:"#1a1a1a", whiteSpace:"nowrap", overflow:"hidden", textOverflow:"ellipsis" }}>{s.name}</div>
                          <span className="r-chip" style={{ background:`${stageColor[s.stage]||"#888"}22`, color:stageColor[s.stage]||"#888" }}>{s.stage}</span>
                        </div>
                        <div style={{ display:"flex", gap:2 }}>
                          <button className="r-abtn" onClick={() => setModal({type:"startup",editing:s})} style={{ color:"#666" }}>✎</button>
                          <button className="r-abtn" onClick={() => setDeleteConfirm({type:"startup",item:s})} style={{ color:"#ef4444" }}>✕</button>
                        </div>
                      </div>
                      {s.desc && <div style={{ ...M, fontSize:11, color:"#444", marginBottom:10, lineHeight:1.6 }}>{s.desc}</div>}
                      <div style={{ display:"flex", flexWrap:"wrap", gap:5 }}>
                        {s.needs.length ? s.needs.map(n => <span key={n} className="r-chip" style={{ background:"#f0ecf8", color:"#5c2278", border:"1px solid #d0b8e8" }}>{n}</span>) : <span style={{ ...M, fontSize:11, color:"#aaa" }}>No needs defined</span>}
                      </div>
                    </div>
                  ))}
                </div>
            }
          </>}

          {browseTab==="researchers" && <>
            <div style={{ display:"flex", alignItems:"center", justifyContent:"space-between", marginBottom:16 }}>
              <div style={{ ...M, fontSize:11, color:"#444" }}>{researchers.length} researcher{researchers.length!==1?"s":""}</div>
              <div style={{ display:"flex", gap:8 }}>
                <button onClick={() => setShowExcel(true)} style={{ background:"#e6f8f4", border:"1px solid #00b38a44", borderRadius:4, color:"#00b38a", ...M, fontSize:12, fontWeight:600, padding:"8px 16px", cursor:"pointer" }}>📊 Import Excel</button>
                <button onClick={() => setModal({type:"researcher",editing:null})} style={{ background:"#00b38a", border:"none", borderRadius:4, color:"#fff", ...M, fontSize:12, fontWeight:700, padding:"8px 18px", cursor:"pointer" }}>+ Add Manually</button>
              </div>
            </div>
            {researchers.length===0
              ? <div style={{ display:"flex", flexDirection:"column", alignItems:"center", justifyContent:"center", height:280, gap:14, textAlign:"center" }}>
                  <div style={{ fontSize:36, opacity:0.15 }}>🔬</div>
                  <div style={{ ...S, fontSize:18, color:"#aaa" }}>No researchers yet</div>
                  <div style={{ display:"flex", gap:10 }}>
                    <button onClick={() => setShowExcel(true)} style={{ ...M, fontSize:12, color:"#00b38a", background:"#e6f8f4", border:"1px solid #00b38a33", borderRadius:8, padding:"9px 18px", cursor:"pointer" }}>📊 Import from Excel</button>
                    <button onClick={() => setModal({type:"researcher",editing:null})} style={{ ...M, fontSize:12, color:"#888", background:"#f4f4f4", border:"1px solid #e0e0e0", borderRadius:4, padding:"9px 18px", cursor:"pointer" }}>+ Add Manually</button>
                  </div>
                </div>
              : <div style={{ display:"grid", gridTemplateColumns:"repeat(auto-fill,minmax(300px,1fr))", gap:12 }}>
                  {researchers.map((r,i) => (
                    <div key={r.id} className="r-card r-glow" style={{ background:"#fff", borderRadius:8, padding:"16px", border:`1px solid ${r.open?"#00b38a33":"#e0e0e0"}`, animation:`r-fu .35s ${i*.05}s both` }}>
                      <div style={{ display:"flex", alignItems:"center", gap:10, marginBottom:10 }}>
                        <div style={{ width:36, height:36, borderRadius:8, flexShrink:0, background: r.open?"#e6f8f4":"#f4f4f4", border:`1px solid ${r.open?"#00b38a44":"#e0e0e0"}`, display:"flex", alignItems:"center", justifyContent:"center", ...M, fontSize:11, color: r.open?"#00b38a":"#bbb", fontWeight:700 }}>{initials(r.name)}</div>
                        <div style={{ flex:1, minWidth:0 }}>
                          <div style={{ ...S, fontSize:14, fontWeight:700, color:"#1a1a1a", whiteSpace:"nowrap", overflow:"hidden", textOverflow:"ellipsis" }}>{r.name}</div>
                          <div style={{ ...M, fontSize:10, color:"#444" }}>{r.institution||"—"}</div>
                        </div>
                        <div style={{ display:"flex", gap:3, alignItems:"center" }}>
                          {r.hIndex && <span className="r-chip" style={{ background:"#e8f0ff", color:"#2255cc" }}>H {r.hIndex}</span>}
                          <span className="r-chip" style={{ background: r.open?"#e6f8f4":"#ffeaea", color: r.open?"#00b38a":"#e53e3e" }}>{r.open?"Open":"Closed"}</span>
                          <button className="r-abtn" onClick={() => setModal({type:"researcher",editing:r})} style={{ color:"#666" }}>✎</button>
                          <button className="r-abtn" onClick={() => setDeleteConfirm({type:"researcher",item:r})} style={{ color:"#ef4444" }}>✕</button>
                        </div>
                      </div>
                      {r.industryExp && <div style={{ ...M, fontSize:11, color:"#444", marginBottom:9 }}>{r.industryExp}</div>}
                      <div style={{ display:"flex", flexWrap:"wrap", gap:5 }}>
                        {r.focus.length ? r.focus.map(f => <span key={f} className="r-chip" style={{ background:"#f0ecf8", color:"#888", border:"1px solid #e0e0e0" }}>{f}</span>) : <span style={{ ...M, fontSize:11, color:"#aaa" }}>No focus defined</span>}
                      </div>
                    </div>
                  ))}
                </div>
            }
          </>}
        </div>
      )}

      {modal?.type==="startup" && <Modal title={modal.editing?"Edit Startup":"Add Startup"} onClose={() => setModal(null)}><StartupForm initial={modal.editing} onSave={saveStartup} onClose={() => setModal(null)} /></Modal>}
      {modal?.type==="researcher" && <Modal title={modal.editing?"Edit Researcher":"Add Researcher"} onClose={() => setModal(null)} wide><ResearcherForm initial={modal.editing} onSave={saveResearcher} onClose={() => setModal(null)} /></Modal>}
      {showExcel && <ExcelUploadModal onImport={handleImport} onClose={() => setShowExcel(false)} mode="researcher" />}
      {showStartupExcel && <ExcelUploadModal onImport={handleStartupImport} onClose={() => setShowStartupExcel(false)} mode="startup" />}
      {deleteConfirm && (
        <Modal title="Confirm Delete" onClose={() => setDeleteConfirm(null)}>
          <div style={{ ...M, fontSize:13, color:"#888", marginBottom:24, lineHeight:1.8 }}>Delete <span style={{ color:"#f0ece4", fontWeight:600 }}>{deleteConfirm.item.name}</span>? This cannot be undone.</div>
          <div style={{ display:"flex", gap:10 }}>
            <button onClick={() => setDeleteConfirm(null)} style={{ flex:1, padding:"10px", borderRadius:8, border:"1px solid #e0e0e0", background:"#fff", color:"#888", ...M, fontSize:12, cursor:"pointer" }}>Cancel</button>
            <button onClick={doDelete} style={{ flex:1, padding:"10px", borderRadius:8, border:"none", background:"#e53e3e", color:"#fff", ...M, fontSize:12, fontWeight:600, cursor:"pointer" }}>Delete</button>
          </div>
        </Modal>
      )}
    </div>
  );
}

ReactDOM.createRoot(document.getElementById('react-root')).render(<App />);
</script>

<script>
  function launchApp() {
    document.getElementById('landing').style.display = 'none';
    document.getElementById('app-shell').classList.add('active');
    document.body.style.overflow = 'hidden';
  }

  function scrollToSection(id) {
    document.getElementById(id)?.scrollIntoView({ behavior: 'smooth' });
  }

  window.showLanding = function() {
    document.getElementById('app-shell').classList.remove('active');
    document.getElementById('landing').style.display = 'flex';
    document.body.style.overflow = '';
  };
</script>

</body>
</html>
