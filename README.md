# jvox.github.io
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Jvox — Video Editor</title>
<link href="https://fonts.googleapis.com/css2?family=Bebas+Neue&family=DM+Sans:ital,wght@0,300;0,400;0,500;1,300&display=swap" rel="stylesheet">
<style>
  :root {
    --bg: #060f1e;
    --surface: rgba(255,255,255,0.06);
    --surface2: rgba(255,255,255,0.10);
    --border: rgba(255,255,255,0.12);
    --accent: #03132d;
    --accent2: #051f4a;
    --text: #f0ede8;
    --muted: #8ba3c4;
    --muted2: #3a5a80;
  }

  * { margin: 0; padding: 0; box-sizing: border-box; text-shadow: 0 1px 6px rgba(0,0,0,0.55), 0 2px 16px rgba(0,0,0,0.3); }

  html { scroll-behavior: smooth; }

  body {
    background: linear-gradient(135deg, #03132d 0%, #071e42 50%, #0a2456 100%);
    background-attachment: fixed;
    color: var(--text);
    font-family: 'DM Sans', sans-serif;
    font-weight: 300;
    overflow-x: hidden;
    cursor: none;
  }

  /* Custom cursor */
  .cursor {
    width: 10px; height: 10px;
    background: #7eb8ff;
    border-radius: 50%;
    position: fixed;
    top: 0; left: 0;
    pointer-events: none;
    z-index: 9999;
    transition: transform 0.1s ease;
    mix-blend-mode: difference;
  }
  .cursor-ring {
    width: 36px; height: 36px;
    border: 1px solid #7eb8ff;
    border-radius: 50%;
    position: fixed;
    top: 0; left: 0;
    pointer-events: none;
    z-index: 9998;
    transition: transform 0.15s ease, width 0.2s, height 0.2s;
    mix-blend-mode: difference;
    opacity: 0.5;
  }

  /* Nav */
  nav {
    position: fixed; top: 0; left: 0; right: 0;
    padding: 24px 48px;
    display: flex; justify-content: space-between; align-items: center;
    z-index: 100;
    border-bottom: 1px solid transparent;
    transition: border-color 0.3s, background 0.3s;
  }
  nav.scrolled {
    background: rgba(3,19,45,0.85);
    backdrop-filter: blur(12px);
    border-color: var(--border);
  }
  .nav-logo {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 28px;
    letter-spacing: 3px;
    color: #7eb8ff;
    text-decoration: none;
  }
  .nav-links { display: flex; gap: 36px; list-style: none; }
  .nav-links a {
    color: var(--muted);
    text-decoration: none;
    font-size: 13px;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    transition: color 0.2s;
  }
  .nav-links a:hover { color: var(--text); }
  .nav-cta {
    background: #03132d;
    color: #ffffff;
    border: 1px solid rgba(126,184,255,0.3);
    padding: 10px 22px;
    font-size: 12px;
    font-weight: 500;
    letter-spacing: 1px;
    text-transform: uppercase;
    text-decoration: none;
    transition: opacity 0.2s;
  }
  .nav-cta:hover { opacity: 0.85; }

  /* Hero */
  .hero {
    min-height: 100vh;
    display: flex; align-items: center;
    padding: 0 48px;
    position: relative;
    overflow: hidden;
  }
  .hero-bg {
    position: absolute; inset: 0;
    background:
      radial-gradient(ellipse 60% 50% at 80% 50%, rgba(30,100,255,0.15) 0%, transparent 70%),
      radial-gradient(ellipse 40% 40% at 20% 80%, rgba(10,60,200,0.12) 0%, transparent 60%),
      radial-gradient(ellipse 30% 30% at 50% 20%, rgba(80,140,255,0.08) 0%, transparent 60%);
  }
  .hero-grid {
    position: absolute; inset: 0;
    background-image:
      linear-gradient(var(--border) 1px, transparent 1px),
      linear-gradient(90deg, var(--border) 1px, transparent 1px);
    background-size: 60px 60px;
    opacity: 0.3;
    mask-image: radial-gradient(ellipse at center, black 20%, transparent 80%);
  }
  .hero-content { position: relative; z-index: 1; max-width: 900px; }
  .hero-tag {
    display: inline-flex; align-items: center; gap: 8px;
    font-size: 11px; letter-spacing: 2px; text-transform: uppercase;
    color: #7eb8ff; margin-bottom: 32px;
  }
  .hero-tag::before {
    content: '';
    width: 24px; height: 1px;
    background: #7eb8ff;
  }
  h1 {
    font-family: 'Bebas Neue', sans-serif;
    font-size: clamp(72px, 12vw, 160px);
    line-height: 0.9;
    letter-spacing: 2px;
    margin-bottom: 32px;
  }
  h1 .accent { color: #7eb8ff; }
  h1 .outline {
    -webkit-text-stroke: 1px var(--muted2);
    color: transparent;
  }
  .hero-desc {
    font-size: 17px;
    color: var(--muted);
    max-width: 420px;
    line-height: 1.8;
    margin-bottom: 48px;
  }
  .hero-actions { display: flex; gap: 16px; align-items: center; }
  .btn-primary {
    background: #03132d;
    color: #ffffff;
    border: 1px solid rgba(126,184,255,0.4);
    padding: 16px 36px;
    font-size: 13px;
    font-weight: 500;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    text-decoration: none;
    transition: opacity 0.2s, transform 0.2s;
    display: inline-block;
  }
  .btn-primary:hover { opacity: 0.85; transform: translateY(-1px); }
  .btn-ghost {
    border: 1px solid var(--border);
    color: var(--muted);
    padding: 16px 36px;
    font-size: 13px;
    letter-spacing: 1.5px;
    text-transform: uppercase;
    text-decoration: none;
    transition: border-color 0.2s, color 0.2s;
    display: inline-block;
  }
  .btn-ghost:hover { border-color: var(--muted); color: var(--text); }
  .hero-stats {
    position: absolute; right: 48px; bottom: 80px;
    display: flex; flex-direction: column; gap: 32px;
    z-index: 1;
  }
  .stat { text-align: right; }
  .stat-num {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 42px;
    color: var(--text);
    letter-spacing: 1px;
    line-height: 1;
  }
  .stat-label { font-size: 11px; color: var(--muted); letter-spacing: 1.5px; text-transform: uppercase; }

  /* Section base */
  section { padding: 120px 48px; }
  .section-tag {
    font-size: 11px; letter-spacing: 2px; text-transform: uppercase;
    color: #7eb8ff; margin-bottom: 20px;
    display: flex; align-items: center; gap: 10px;
  }
  .section-tag::before { content: ''; width: 20px; height: 1px; background: #7eb8ff; }
  h2 {
    font-family: 'Bebas Neue', sans-serif;
    font-size: clamp(42px, 6vw, 72px);
    letter-spacing: 2px;
    line-height: 1;
    margin-bottom: 16px;
  }

  /* Services */
  .services { background: rgba(255,255,255,0.02); backdrop-filter: blur(4px); }
  .services-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
    gap: 1px;
    background: var(--border);
    margin-top: 64px;
    border: 1px solid var(--border);
  }
  .service-card {
    background: rgba(255,255,255,0.04);
    backdrop-filter: blur(12px);
    padding: 40px 32px;
    transition: background 0.2s;
    position: relative;
    overflow: hidden;
  }
  .service-card::after {
    content: '';
    position: absolute; bottom: 0; left: 0;
    width: 0; height: 2px;
    background: #7eb8ff;
    transition: width 0.3s ease;
  }
  .service-card:hover { background: rgba(255,255,255,0.08); backdrop-filter: blur(16px); }
  .service-card:hover::after { width: 100%; }
  .service-num {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 48px;
    color: var(--muted2);
    line-height: 1;
    margin-bottom: 20px;
  }
  .service-title {
    font-size: 16px; font-weight: 500;
    color: var(--text); margin-bottom: 12px;
  }
  .service-desc { font-size: 13px; color: var(--muted); line-height: 1.7; }
  .service-price {
    margin-top: 24px;
    font-size: 13px; color: #7eb8ff;
    letter-spacing: 0.5px;
  }

  /* Portfolio */
  .portfolio-header {
    display: flex; justify-content: space-between; align-items: flex-end;
    margin-bottom: 64px; flex-wrap: wrap; gap: 24px;
  }
  .portfolio-sub { font-size: 14px; color: var(--muted); max-width: 300px; line-height: 1.7; }
  .portfolio-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 16px;
  }
  .video-card {
    position: relative;
    background: rgba(255,255,255,0.04);
    backdrop-filter: blur(12px);
    border: 1px solid var(--border);
    overflow: hidden;
    transition: border-color 0.2s, transform 0.2s;
    text-decoration: none;
    display: block;
  }
  .video-card:hover { border-color: var(--accent); transform: translateY(-4px); }
  .video-thumb {
    width: 100%; aspect-ratio: 16/9;
    background: var(--surface2);
    position: relative;
    overflow: hidden;
  }
  .video-thumb-inner {
    width: 100%; height: 100%;
    display: flex; align-items: center; justify-content: center;
    font-size: 13px; color: var(--muted2);
    letter-spacing: 1px;
  }
  .play-btn {
    width: 48px; height: 48px;
    border: 1px solid rgba(126,184,255,0.4);
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    position: absolute;
    top: 50%; left: 50%;
    transform: translate(-50%, -50%);
    transition: background 0.2s, border-color 0.2s;
  }
  .video-card:hover .play-btn { background: #7eb8ff; border-color: #7eb8ff; }
  .play-icon {
    width: 0; height: 0;
    border-top: 7px solid transparent;
    border-bottom: 7px solid transparent;
    border-left: 12px solid rgba(126,184,255,0.7);
    margin-left: 3px;
    transition: border-left-color 0.2s;
  }
  .video-card:hover .play-icon { border-left-color: #0a0a0a; }
  .video-label {
    position: absolute; top: 12px; left: 12px;
    background: rgba(10,10,10,0.8);
    font-size: 10px; letter-spacing: 1.5px; text-transform: uppercase;
    color: #7eb8ff; padding: 4px 10px;
    backdrop-filter: blur(4px);
  }
  .video-info { padding: 20px 24px; }
  .video-title { font-size: 15px; font-weight: 500; color: var(--text); margin-bottom: 6px; }
  .video-meta { font-size: 12px; color: var(--muted); letter-spacing: 0.5px; }
  .video-placeholder {
    width: 100%; aspect-ratio: 16/9;
    background: linear-gradient(135deg, var(--surface2) 0%, var(--surface) 100%);
    display: flex; flex-direction: column;
    align-items: center; justify-content: center; gap: 8px;
  }
  .placeholder-line {
    height: 1px; background: var(--border);
    width: 80%; opacity: 0.5;
  }
  .placeholder-text { font-size: 11px; color: var(--muted2); letter-spacing: 1px; text-transform: uppercase; }

  /* About */
  .about { background: rgba(255,255,255,0.02); backdrop-filter: blur(4px); }
  .about-inner {
    display: grid; grid-template-columns: 1fr 1fr;
    gap: 80px; align-items: center;
  }
  .about-visual {
    position: relative;
    aspect-ratio: 1;
    max-width: 420px;
  }
  .about-box {
    width: 100%; height: 100%;
    border: 1px solid var(--border);
    background: rgba(255,255,255,0.06);
    backdrop-filter: blur(16px);
    display: flex; align-items: center; justify-content: center;
    flex-direction: column; gap: 8px;
  }
  .about-monogram {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 100px; color: var(--muted2);
    letter-spacing: 4px; line-height: 1;
  }
  .about-box-label { font-size: 11px; color: var(--muted2); letter-spacing: 2px; text-transform: uppercase; }
  .about-accent-line {
    position: absolute; bottom: -16px; right: -16px;
    width: 60%; height: 60%;
    border: 1px solid #7eb8ff;
    opacity: 0.2;
    z-index: -1;
  }
  .about-desc { font-size: 15px; color: var(--muted); line-height: 1.9; margin: 24px 0 36px; }
  .about-tags { display: flex; flex-wrap: wrap; gap: 8px; }
  .tag {
    padding: 6px 14px;
    border: 1px solid var(--border);
    font-size: 11px; color: var(--muted);
    letter-spacing: 1px; text-transform: uppercase;
    transition: border-color 0.2s, color 0.2s;
  }
  .tag:hover { border-color: #7eb8ff; color: #7eb8ff; }

  /* Contact */
  .contact-inner {
    display: grid; grid-template-columns: 1fr 1fr;
    gap: 80px; align-items: start;
  }
  .contact-big {
    font-family: 'Bebas Neue', sans-serif;
    font-size: clamp(48px, 7vw, 96px);
    letter-spacing: 2px; line-height: 0.95;
    margin-bottom: 32px;
  }
  .contact-big .accent { color: #7eb8ff; }
  .contact-links { display: flex; flex-direction: column; gap: 0; padding-top: 8px; }
  .contact-link {
    display: flex; align-items: center; gap: 16px;
    text-decoration: none; color: var(--muted);
    font-size: 14px; padding: 20px 0;
    border-bottom: 1px solid var(--border);
    transition: color 0.2s;
  }
  .contact-link:hover { color: var(--text); }
  .contact-link:hover .link-arrow { color: #7eb8ff; transform: translate(4px, -4px); }
  .contact-platform {
    font-size: 11px; letter-spacing: 1.5px; text-transform: uppercase;
    color: var(--muted2); min-width: 100px;
  }
  .contact-value { flex: 1; }
  .link-arrow { transition: color 0.2s, transform 0.2s; font-size: 16px; }
  .contact-form-side { padding-top: 8px; }
  .form-group { margin-bottom: 20px; }
  .form-group label { display: block; font-size: 11px; letter-spacing: 1.5px; text-transform: uppercase; color: var(--muted); margin-bottom: 8px; }
  .form-group input,
  .form-group textarea,
  .form-group select {
    width: 100%; background: rgba(255,255,255,0.06);
    backdrop-filter: blur(8px);
    border: 1px solid var(--border);
    color: var(--text); padding: 14px 16px;
    font-family: 'DM Sans', sans-serif;
    font-size: 14px; font-weight: 300;
    outline: none; transition: border-color 0.2s;
    appearance: none;
  }
  .form-group input:focus,
  .form-group textarea:focus,
  .form-group select:focus { border-color: var(--accent); }
  .form-group textarea { min-height: 120px; resize: vertical; }
  .form-group select option { background: var(--surface2); }
  .form-submit {
    width: 100%; background: #03132d;
    color: #ffffff;
    border: 1px solid rgba(126,184,255,0.3); border: none; padding: 16px;
    font-family: 'DM Sans', sans-serif;
    font-size: 13px; font-weight: 500;
    letter-spacing: 2px; text-transform: uppercase;
    cursor: pointer; transition: opacity 0.2s;
  }
  .form-submit:hover { opacity: 0.85; }

  /* Footer */
  footer {
    padding: 32px 48px;
    border-top: 1px solid var(--border);
    display: flex; justify-content: space-between; align-items: center;
    font-size: 12px; color: var(--muted2);
    flex-wrap: wrap; gap: 12px;
  }
  .footer-logo {
    font-family: 'Bebas Neue', sans-serif;
    font-size: 20px; letter-spacing: 3px;
    color: var(--muted2);
  }

  /* Divider line */
  .divider { height: 1px; background: var(--border); margin: 0 48px; }

  /* Animations */
  @keyframes fadeUp {
    from { opacity: 0; transform: translateY(30px); }
    to { opacity: 1; transform: translateY(0); }
  }
  .hero-content > * { animation: fadeUp 0.8s ease forwards; opacity: 0; }
  .hero-tag { animation-delay: 0.1s; }
  h1 { animation-delay: 0.2s; }
  .hero-desc { animation-delay: 0.35s; }
  .hero-actions { animation-delay: 0.5s; }
  .hero-stats { animation: fadeUp 0.8s 0.6s ease forwards; opacity: 0; }

  @media (max-width: 768px) {
    nav { padding: 20px 24px; }
    .nav-links { display: none; }
    section { padding: 80px 24px; }
    .hero { padding: 0 24px; }
    .hero-stats { display: none; }
    .about-inner,
    .contact-inner { grid-template-columns: 1fr; gap: 40px; }
    .about-visual { max-width: 100%; }
    footer { padding: 24px; }
    .divider { margin: 0 24px; }
  }
</style>
</head>
<body>

<div class="cursor" id="cursor"></div>
<div class="cursor-ring" id="cursorRing"></div>

<!-- Nav -->
<nav id="nav">
  <a href="#" class="nav-logo">JVOX</a>
  <ul class="nav-links">
    <li><a href="#services">Services</a></li>
    <li><a href="#work">Work</a></li>
    <li><a href="#about">About</a></li>
    <li><a href="#contact">Contact</a></li>
  </ul>
  <a href="#contact" class="nav-cta">Hire me</a>
</nav>

<!-- Hero -->
<section class="hero" id="home">
  <div class="hero-bg"></div>
  <div class="hero-grid"></div>
  <div class="hero-content">
    <div class="hero-tag">Available for projects worldwide</div>
    <h1>
      <span class="accent">CLEAN</span><br>
      <span class="outline">MINIMAL</span><br>
      EDITS
    </h1>
    <p class="hero-desc">Short-form video editor from the Philippines. I turn raw footage into polished Reels, TikToks, and Shorts that let your story speak for itself.</p>
    <div class="hero-actions">
      <a href="#work" class="btn-primary">View my work</a>
      <a href="#contact" class="btn-ghost">Get in touch</a>
    </div>
  </div>
  <div class="hero-stats">
    <div class="stat">
      <div class="stat-num">1YR</div>
      <div class="stat-label">Editing experience</div>
    </div>
    <div class="stat">
      <div class="stat-num">1–2</div>
      <div class="stat-label">Day turnaround</div>
    </div>
    <div class="stat">
      <div class="stat-num">$10</div>
      <div class="stat-label">Starting rate</div>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- Services -->
<section class="services" id="services">
  <div class="section-tag">What I offer</div>
  <h2>SERVICES</h2>
  <div class="services-grid">
    <div class="service-card">
      <div class="service-num">01</div>
      <div class="service-title">Short-form editing</div>
      <div class="service-desc">Reels, TikToks, and YouTube Shorts edited with clean cuts, smooth pacing, and a minimal style that keeps viewers watching.</div>
      <div class="service-price">From $10 USD / ₱500</div>
    </div>
    <div class="service-card">
      <div class="service-num">02</div>
      <div class="service-title">Captions & subtitles</div>
      <div class="service-desc">Accurate, well-timed captions that match your audio — styled cleanly to fit your brand without cluttering the frame.</div>
      <div class="service-price">Add-on available</div>
    </div>
    <div class="service-card">
      <div class="service-num">03</div>
      <div class="service-title">Music selection</div>
      <div class="service-desc">Royalty-free music chosen to match your video's mood and pacing from platforms like YouTube Audio Library and similar sources.</div>
      <div class="service-price">Add-on available</div>
    </div>
    <div class="service-card">
      <div class="service-num">04</div>
      <div class="service-title">Motion graphics</div>
      <div class="service-desc">Clean text animations, lower thirds, and simple graphic elements that enhance your video without overwhelming the content.</div>
      <div class="service-price">Add-on available</div>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- Portfolio -->
<section id="work">
  <div class="portfolio-header">
    <div>
      <div class="section-tag">Selected work</div>
      <h2>PORTFOLIO</h2>
    </div>
    <p class="portfolio-sub">Replace these placeholders with your real project thumbnails and links once you have work to show.</p>
  </div>
  <div class="portfolio-grid">

    <a href="#" class="video-card">
      <div class="video-thumb">
        <div class="video-placeholder">
          <div class="placeholder-text">Your project here</div>
          <div class="placeholder-line"></div>
          <div class="placeholder-text">Replace with thumbnail</div>
        </div>
        <div class="play-btn"><div class="play-icon"></div></div>
        <div class="video-label">Reels</div>
      </div>
      <div class="video-info">
        <div class="video-title">Project title here</div>
        <div class="video-meta">Platform · Style · Duration</div>
      </div>
    </a>

    <a href="#" class="video-card">
      <div class="video-thumb">
        <div class="video-placeholder">
          <div class="placeholder-text">Your project here</div>
          <div class="placeholder-line"></div>
          <div class="placeholder-text">Replace with thumbnail</div>
        </div>
        <div class="play-btn"><div class="play-icon"></div></div>
        <div class="video-label">TikTok</div>
      </div>
      <div class="video-info">
        <div class="video-title">Project title here</div>
        <div class="video-meta">Platform · Style · Duration</div>
      </div>
    </a>

    <a href="#" class="video-card">
      <div class="video-thumb">
        <div class="video-placeholder">
          <div class="placeholder-text">Your project here</div>
          <div class="placeholder-line"></div>
          <div class="placeholder-text">Replace with thumbnail</div>
        </div>
        <div class="play-btn"><div class="play-icon"></div></div>
        <div class="video-label">Shorts</div>
      </div>
      <div class="video-info">
        <div class="video-title">Project title here</div>
        <div class="video-meta">Platform · Style · Duration</div>
      </div>
    </a>

    <a href="#" class="video-card">
      <div class="video-thumb">
        <div class="video-placeholder">
          <div class="placeholder-text">Your project here</div>
          <div class="placeholder-line"></div>
          <div class="placeholder-text">Replace with thumbnail</div>
        </div>
        <div class="play-btn"><div class="play-icon"></div></div>
        <div class="video-label">Vlog</div>
      </div>
      <div class="video-info">
        <div class="video-title">Project title here</div>
        <div class="video-meta">Platform · Style · Duration</div>
      </div>
    </a>

    <a href="#" class="video-card">
      <div class="video-thumb">
        <div class="video-placeholder">
          <div class="placeholder-text">Your project here</div>
          <div class="placeholder-line"></div>
          <div class="placeholder-text">Replace with thumbnail</div>
        </div>
        <div class="play-btn"><div class="play-icon"></div></div>
        <div class="video-label">Podcast</div>
      </div>
      <div class="video-info">
        <div class="video-title">Project title here</div>
        <div class="video-meta">Platform · Style · Duration</div>
      </div>
    </a>

    <a href="#" class="video-card">
      <div class="video-thumb">
        <div class="video-placeholder">
          <div class="placeholder-text">Your project here</div>
          <div class="placeholder-line"></div>
          <div class="placeholder-text">Replace with thumbnail</div>
        </div>
        <div class="play-btn"><div class="play-icon"></div></div>
        <div class="video-label">Brand</div>
      </div>
      <div class="video-info">
        <div class="video-title">Project title here</div>
        <div class="video-meta">Platform · Style · Duration</div>
      </div>
    </a>

  </div>
</section>

<div class="divider"></div>

<!-- About -->
<section class="about" id="about">
  <div class="about-inner">
    <div class="about-visual">
      <div class="about-box">
        <div class="about-monogram">JVX</div>
        <div class="about-box-label">Jarel · Jvox</div>
      </div>
      <div class="about-accent-line"></div>
    </div>
    <div>
      <div class="section-tag">Who I am</div>
      <h2>ABOUT<br><span style="color:var(--accent)">ME</span></h2>
      <p class="about-desc">I'm Jarel — a short-form video editor from the Philippines operating as Jvox. Over the past year I've been developing my craft through personal projects including vlogs, podcast clips, and recap videos. I'm now ready to bring that same focus and attention to detail to your content.<br><br>My editing style is minimal by design. I believe the best edit doesn't distract from the message — it supports it. I'm available to work with clients worldwide and deliver within 1–2 days.</p>
      <div class="about-tags">
        <span class="tag">CapCut</span>
        <span class="tag">Short-form</span>
        <span class="tag">Clean & minimal</span>
        <span class="tag">Captions</span>
        <span class="tag">Motion graphics</span>
        <span class="tag">Music selection</span>
        <span class="tag">Fast turnaround</span>
        <span class="tag">Remote worldwide</span>
      </div>
    </div>
  </div>
</section>

<div class="divider"></div>

<!-- Contact -->
<section id="contact">
  <div class="contact-inner">
    <div>
      <div class="section-tag">Work with me</div>
      <div class="contact-big">LET'S<br><span class="accent">WORK</span><br>TOGETHER</div>
    </div>
    <div class="contact-links">
      <a href="mailto:jvoxinmotion@gmail.com" class="contact-link">
        <span class="contact-platform">Email</span>
        <span class="contact-value">jvoxinmotion@gmail.com</span>
        <span class="link-arrow">↗</span>
      </a>
      <a href="https://instagram.com/jvoxinmotion" target="_blank" class="contact-link">
        <span class="contact-platform">Instagram</span>
        <span class="contact-value">@jvoxinmotion</span>
        <span class="link-arrow">↗</span>
      </a>
      <a href="https://tiktok.com/@jvox.inmotion" target="_blank" class="contact-link">
        <span class="contact-platform">TikTok</span>
        <span class="contact-value">@jvox.inmotion</span>
        <span class="link-arrow">↗</span>
      </a>
      <a href="#" class="contact-link">
        <span class="contact-platform">Location</span>
        <span class="contact-value">Philippines · Available worldwide</span>
      </a>
    </div>
  </div>
</section>

<!-- Footer -->
<footer>
  <div class="footer-logo">JVOX</div>
  <span>Jarel · Video Editor · Philippines</span>
  <span>Available worldwide · Fast turnaround</span>
</footer>

<script>
  const cursor = document.getElementById('cursor');
  const ring = document.getElementById('cursorRing');
  let mx = 0, my = 0, rx = 0, ry = 0;

  document.addEventListener('mousemove', e => {
    mx = e.clientX; my = e.clientY;
    cursor.style.transform = `translate(${mx - 5}px, ${my - 5}px)`;
  });

  function animateRing() {
    rx += (mx - rx - 18) * 0.12;
    ry += (my - ry - 18) * 0.12;
    ring.style.transform = `translate(${rx}px, ${ry}px)`;
    requestAnimationFrame(animateRing);
  }
  animateRing();

  document.querySelectorAll('a, button').forEach(el => {
    el.addEventListener('mouseenter', () => {
      cursor.style.transform += ' scale(2)';
      ring.style.width = '54px'; ring.style.height = '54px';
    });
    el.addEventListener('mouseleave', () => {
      ring.style.width = '36px'; ring.style.height = '36px';
    });
  });

  const nav = document.getElementById('nav');
  window.addEventListener('scroll', () => {
    nav.classList.toggle('scrolled', window.scrollY > 40);
  });
</script>
</body>
</html>
