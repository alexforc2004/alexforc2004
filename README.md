# Build a polished, self-contained portfolio dashboard preview with functional links and music controls.
from pathlib import Path

html = r'''<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Azzedine Oubaid — Developer Command Center</title>
<style>
:root {
  color-scheme: light dark;
  --site-primary: #00e8f5;
  --site-secondary: #6f42ff;
  --site-background: #080b12;
  --site-on-primary: #041116;
  --site-on-secondary: #ffffff;
  --site-on-background: #f3f7ff;
  --site-font-body: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  --site-font-heading: "Arial", "Helvetica Neue", sans-serif;
  --site-heading-scale: 1;
  --site-spacing-scale: 1;
  --site-radius-scale: 1;
  --site-decoration-opacity: 1;
  --bg: var(--site-background);
  --panel: #101522;
  --panel2: #0d121d;
  --line: rgba(132, 161, 197, .18);
  --muted: #9aa9bf;
  --text: var(--site-on-background);
  --cyan: var(--site-primary);
  --purple: var(--site-secondary);
}
* { box-sizing: border-box; }
html { scroll-behavior: smooth; }
body { margin: 0; background: var(--bg); color: var(--text); font-family: var(--site-font-body); }
#app {
  min-height: 100vh;
  background-color: var(--site-background);
  color: var(--site-on-background);
  overflow: hidden;
  position: relative;
  isolation: isolate;
}
#app::before {
  content: ""; position: absolute; inset: 0 0 auto; height: 620px; z-index: -1;
  opacity: calc(.48 * var(--site-decoration-opacity));
  background: radial-gradient(ellipse at 15% 12%, rgba(0,232,245,.17), transparent 42%),
              radial-gradient(ellipse at 85% 6%, rgba(111,66,255,.20), transparent 40%);
  pointer-events: none;
}
a { color: inherit; text-decoration: none; }
button { font: inherit; cursor: pointer; }
.wrap { width: min(1120px, calc(100% - 40px)); margin-inline: auto; }
.topbar { display:flex; align-items:center; justify-content:space-between; gap:18px; padding:22px 0; border-bottom:1px solid var(--line); }
.brand { display:flex; align-items:center; gap:11px; font-weight:800; letter-spacing:.04em; }
.brand-mark { display:grid; place-items:center; width:38px; height:38px; border-radius:12px; background:linear-gradient(135deg,var(--cyan),var(--purple)); color:#07101b; font-weight:900; }
.brand small { display:block; color:var(--muted); font-size:10px; letter-spacing:.18em; margin-top:2px; }
.nav { display:flex; align-items:center; gap:20px; color:var(--muted); font-size:13px; }
.nav a:hover { color:var(--cyan); }
.status { display:flex; align-items:center; gap:8px; font-size:11px; letter-spacing:.1em; color:#b5c5d8; }
.dot { width:7px; height:7px; background:#39e6a4; border-radius:50%; box-shadow:0 0 12px #39e6a4; }
.hero { padding:72px 0 48px; display:grid; grid-template-columns:1.35fr .65fr; gap:40px; align-items:center; }
.eyebrow { display:flex; align-items:center; gap:10px; color:var(--cyan); font-size:11px; letter-spacing:.22em; font-weight:800; text-transform:uppercase; }
.eyebrow:before { content:""; width:28px; height:1px; background:var(--cyan); }
h1,h2,h3,p { margin-top:0; }
h1 { font-family:var(--site-font-heading); font-size:clamp(40px,6.8vw,76px); line-height:.98; letter-spacing:-.065em; margin:18px 0; font-weight:900; }
h1 span { color:var(--cyan); text-shadow:0 0 30px rgba(0,232,245,.22); }
.lead { max-width:600px; color:#aab7ca; line-height:1.8; font-size:15px; }
.actions { display:flex; flex-wrap:wrap; gap:12px; margin-top:26px; }
.btn { display:inline-flex; align-items:center; justify-content:center; gap:9px; border:1px solid var(--line); border-radius:12px; padding:12px 17px; color:var(--text); background:#111827; font-size:13px; font-weight:750; transition:transform .2s,border-color .2s,box-shadow .2s; }
.btn:hover { transform:translateY(-3px); border-color:var(--cyan); box-shadow:0 8px 28px rgba(0,232,245,.1); }
.btn.primary { background:var(--cyan); color:var(--site-on-primary); border-color:transparent; }
.btn.ghost { background:transparent; }
.terminal { border:1px solid rgba(0,232,245,.24); background:linear-gradient(145deg,rgba(16,22,36,.96),rgba(9,13,22,.94)); border-radius:20px; padding:22px; box-shadow:0 24px 80px rgba(0,0,0,.24); transform:translateY(0); animation:float 6s ease-in-out infinite; }
.term-top { display:flex; gap:6px; padding-bottom:16px; border-bottom:1px solid var(--line); margin-bottom:18px; }
.term-top i { width:8px;height:8px;border-radius:50%;background:#ff5d6c; }
.term-top i:nth-child(2){background:#ffca58}.term-top i:nth-child(3){background:#3fe5a1}
.term-label { color:var(--muted); font-size:11px; margin-left:auto; letter-spacing:.12em; }
.term-line { font:12px/2 "SFMono-Regular",Consolas,monospace; color:#b8c7db; }
.term-line b { color:var(--cyan); font-weight:600; }
.term-line .ok { color:#54e6a5; }
.term-note { margin-top:15px; padding:12px; border-radius:10px; background:rgba(0,232,245,.055); color:#b8c7db; font-size:11px; line-height:1.7; }
.metrics { display:grid; grid-template-columns:repeat(4,1fr); gap:12px; margin:14px 0 68px; }
.metric { padding:18px; border:1px solid var(--line); background:rgba(15,21,34,.72); border-radius:15px; }
.metric strong { display:block; color:var(--cyan); font-size:22px; letter-spacing:-.04em; }
.metric span { display:block; color:var(--muted); font-size:11px; margin-top:6px; }
.section { padding:12px 0 56px; scroll-margin-top:22px; }
.section-head { display:flex; justify-content:space-between; align-items:end; gap:16px; margin-bottom:22px; }
.kicker { color:var(--cyan); font-size:10px; letter-spacing:.2em; font-weight:800; text-transform:uppercase; }
h2 { font-size:clamp(25px,3.5vw,36px); letter-spacing:-.045em; margin:8px 0 0; }
.section-head p { color:var(--muted); font-size:12px; margin:0; }
.grid { display:grid; grid-template-columns:repeat(3,1fr); gap:14px; }
.card { border:1px solid var(--line); border-radius:16px; padding:20px; background:linear-gradient(150deg,rgba(18,25,41,.9),rgba(11,15,25,.9)); transition:transform .22s,border-color .22s; }
.card:hover { transform:translateY(-4px); border-color:rgba(0,232,245,.4); }
.card-icon { display:grid; place-items:center; width:42px;height:42px;border-radius:12px;background:rgba(0,232,245,.09);color:var(--cyan);font-size:20px;margin-bottom:18px; }
.card h3 { font-size:15px; margin-bottom:9px; }
.card p { color:var(--muted); font-size:12px; line-height:1.75; margin-bottom:16px; }
.tags { display:flex; flex-wrap:wrap; gap:7px; }
.tag { padding:5px 8px; border:1px solid var(--line); border-radius:7px; font-size:10px; color:#b9c7d8; }
.skill-layout { display:grid; grid-template-columns:1fr 1fr; gap:14px; }
.skill-box { border:1px solid var(--line); border-radius:16px; padding:22px; background:var(--panel2); }
.skill-box h3 { font-size:14px; margin-bottom:17px; }
.skill-row { margin:14px 0; }
.skill-label { display:flex; justify-content:space-between; color:#b8c7d8; font-size:11px; margin-bottom:8px; }
.track { height:5px; background:#20293a; border-radius:99px; overflow:hidden; }
.track span { display:block;height:100%;width:var(--w);background:linear-gradient(90deg,var(--purple),var(--cyan));border-radius:99px; animation:grow 1.2s ease both; }
.note { color:var(--muted); font-size:11px; line-height:1.7; margin-top:16px; }
.timeline { display:grid; grid-template-columns:repeat(3,1fr); gap:14px; }
.phase { border-left:2px solid var(--purple); padding:2px 0 2px 17px; }
.phase small { color:var(--cyan); font-size:10px; letter-spacing:.12em; }
.phase h3 { margin:9px 0; font-size:14px; }
.phase p { color:var(--muted); font-size:12px; line-height:1.7; }
.connect { border:1px solid rgba(111,66,255,.32); border-radius:20px; padding:28px; background:linear-gradient(110deg,rgba(111,66,255,.12),rgba(0,232,245,.055)); display:flex; align-items:center; justify-content:space-between; gap:22px; }
.connect p { color:var(--muted); font-size:13px; margin:8px 0 0; }
.music { position:sticky; bottom:0; z-index:10; margin-top:18px; border-top:1px solid rgba(0,232,245,.23); background:rgba(7,10,17,.94); backdrop-filter:blur(18px); }
.music-inner { min-height:82px; display:grid; grid-template-columns:1fr auto 1fr; gap:18px; align-items:center; padding:13px 0; }
.track-info { display:flex; align-items:center; gap:12px; min-width:0; }
.album { width:48px;height:48px;border-radius:12px; flex:0 0 auto; display:grid;place-items:center;background:linear-gradient(135deg,#131e37,#6f42ff 55%,#00e8f5);font-size:20px;box-shadow:0 0 24px rgba(111,66,255,.18); }
.track-info strong { display:block; font-size:12px; white-space:nowrap; }
.track-info small { display:block; color:var(--muted); font-size:10px; margin-top:5px; }
.player-controls { display:flex; align-items:center; justify-content:center; gap:10px; }
.control { width:34px;height:34px;display:grid;place-items:center;border:1px solid var(--line);border-radius:50%;background:#111827;color:var(--text); }
.control.play { width:42px;height:42px;background:var(--cyan);color:#061118;border-color:transparent;font-size:16px; }
.control:hover { border-color:var(--cyan); }
.music-right { display:flex; align-items:center; justify-content:flex-end; gap:10px; }
.music-link { font-size:11px; color:var(--cyan); border:1px solid rgba(0,232,245,.28); padding:9px 11px; border-radius:9px; }
.volume { width:75px; accent-color:var(--cyan); }
.wave { height:18px; display:flex; gap:3px; align-items:center; }
.wave i { display:block;width:3px;height:5px;border-radius:5px;background:var(--cyan);opacity:.8; }
.wave.playing i { animation:wave .7s ease-in-out infinite alternate; }
.wave i:nth-child(2n){animation-delay:.18s}.wave i:nth-child(3n){animation-delay:.32s}.wave i:nth-child(4n){animation-delay:.12s}
.footer { padding:26px 0 32px; color:#738197; font-size:10px; display:flex;justify-content:space-between;gap:12px; }
@keyframes float { 0%,100%{transform:translateY(0)}50%{transform:translateY(-7px)} }
@keyframes grow { from{width:0}to{width:var(--w)} }
@keyframes wave { to{height:17px;opacity:1} }
@media(max-width:780px) {
 .hero{grid-template-columns:1fr;padding-top:52px;gap:28px}.terminal{animation:none}
 .metrics{grid-template-columns:repeat(2,1fr);margin-bottom:48px}
 .grid{grid-template-columns:1fr 1fr}.skill-layout,.timeline{grid-template-columns:1fr}
 .nav{display:none}.music-inner{grid-template-columns:1fr auto}.music-right{grid-column:1/-1;justify-content:space-between}
 .connect{align-items:flex-start;flex-direction:column}.section-head{align-items:flex-start;flex-direction:column}
}
@media(max-width:480px) {
 .wrap{width:min(100% - 28px,1120px)}.grid{grid-template-columns:1fr}
 h1{font-size:44px}.music-inner{gap:12px}.track-info{gap:9px}.album{width:42px;height:42px}
 .music-link{font-size:10px}.volume{width:55px}.footer{flex-direction:column}
}
@media(prefers-reduced-motion:reduce) { *,*::before,*::after{animation:none!important;scroll-behavior:auto!important;transition:none!important} }
</style>
</head>
<body>
<div id="app">
  <header class="wrap topbar">
    <a class="brand" href="#home" aria-label="Azzedine Oubaid home">
      <span class="brand-mark">AO</span>
      <span>AZZEDINE OUBAID<small>DEV / SECURITY</small></span>
    </a>
    <nav class="nav" aria-label="Main navigation">
      <a href="#about">About</a><a href="#stack">Tech stack</a><a href="#labs">Labs</a><a href="#contact">Contact</a>
    </nav>
    <div class="status"><span class="dot"></span> OPEN TO OPPORTUNITIES</div>
  </header>

  <main>
    <section class="wrap hero" id="home">
      <div>
        <div class="eyebrow">Full Stack Developer · Cybersecurity</div>
        <h1>Build smart.<br><span>Secure by design.</span></h1>
        <p class="lead">I'm Azzedine Oubaid — a developer focused on building useful web experiences and growing my skills in Linux, networking and application security. Curious by default. Always learning.</p>
        <div class="actions">
          <a class="btn primary" href="https://github.com/alexforc2004?tab=repositories" target="_blank" rel="noreferrer">⌘ Explore projects ↗</a>
          <a class="btn ghost" href="https://www.linkedin.com/in/azzedine-oubaid-393059430" target="_blank" rel="noreferrer">↗ Connect on LinkedIn</a>
          <a class="btn ghost" href="#stack">View tech stack ↓</a>
        </div>
      </div>
      <aside class="terminal" aria-label="Terminal inspired profile">
        <div class="term-top"><i></i><i></i><i></i><span class="term-label">profile.sh</span></div>
        <div class="term-line"><b>azzedine@portfolio</b>:~$ whoami</div>
        <div class="term-line">Azzedine Oubaid</div>
        <div class="term-line"><b>focus</b> = web development</div>
        <div class="term-line"><b>learning</b> = Linux + web security</div>
        <div class="term-line"><b>mindset</b> = build / test / improve</div>
        <div class="term-line"><span class="ok">[OK]</span> learning mode: active<span class="cursor">_</span></div>
        <div class="term-note"># Practice security responsibly<br># Test only systems you own or have permission to assess</div>
      </aside>
    </section>

    <section class="wrap metrics" aria-label="Profile highlights">
      <div class="metric"><strong>FULL STACK</strong><span>Development focus</span></div>
      <div class="metric"><strong>PHP / JS</strong><span>Web technologies</span></div>
      <div class="metric"><strong>LINUX</strong><span>Hands-on learning</span></div>
      <div class="metric"><strong>ETHICAL</strong><span>Authorized security labs</span></div>
    </section>

    <section class="wrap section" id="about">
      <div class="section-head"><div><div class="kicker">01 / Profile</div><h2>About my workflow</h2></div><p>Learn by building. Improve by testing.</p></div>
      <div class="grid">
        <article class="card"><div class="card-icon">⌘</div><h3>Full Stack Development</h3><p>Building responsive interfaces and working with server-side logic, databases and application workflows.</p><div class="tags"><span class="tag">PHP</span><span class="tag">JavaScript</span><span class="tag">SQL</span></div></article>
        <article class="card"><div class="card-icon">⌁</div><h3>Web Security</h3><p>Learning common web vulnerabilities, secure authentication and practical defensive techniques.</p><div class="tags"><span class="tag">OWASP</span><span class="tag">Burp Suite</span><span class="tag">ZAP</span></div></article>
        <article class="card"><div class="card-icon">▣</div><h3>Linux & Networking</h3><p>Exploring command-line workflows, network fundamentals and automation in controlled environments.</p><div class="tags"><span class="tag">Kali Linux</span><span class="tag">TCP/IP</span><span class="tag">Bash</span></div></article>
      </div>
    </section>

    <section class="wrap section" id="stack">
      <div class="section-head"><div><div class="kicker">02 / Toolkit</div><h2>Tech stack</h2></div><p>Technologies I'm learning and using</p></div>
      <div class="skill-layout">
        <div class="skill-box"><h3>◈ Development</h3>
          <div class="skill-row"><div class="skill-label"><span>HTML / CSS</span><span>UI fundamentals</span></div><div class="track"><span style="--w:84%"></span></div></div>
          <div class="skill-row"><div class="skill-label"><span>JavaScript</span><span>Web interactions</span></div><div class="track"><span style="--w:67%"></span></div></div>
          <div class="skill-row"><div class="skill-label"><span>PHP / Laravel</span><span>Backend learning</span></div><div class="track"><span style="--w:58%"></span></div></div>
          <div class="skill-row"><div class="skill-label"><span>MySQL</span><span>Data layer</span></div><div class="track"><span style="--w:62%"></span></div></div>
          <div class="note">Bars are visual placeholders, not verified proficiency scores. Adjust them to match your real experience.</div>
        </div>
        <div class="skill-box"><h3>⌬ Systems & Security</h3>
          <div class="tags"><span class="tag">Linux</span><span class="tag">Kali Linux</span><span class="tag">Docker basics</span><span class="tag">Git</span><span class="tag">Python basics</span><span class="tag">Bash</span><span class="tag">Nmap</span><span class="tag">Wireshark</span><span class="tag">Burp Suite</span><span class="tag">OWASP Top 10</span><span class="tag">Postman</span><span class="tag">Networking</span></div>
          <p class="note">Keep only the tools you have actually used. A focused, honest list makes a stronger profile than an oversized tool catalogue.</p>
        </div>
      </div>
    </section>

    <section class="wrap section" id="labs">
      <div class="section-head"><div><div class="kicker">03 / Current focus</div><h2>Learning roadmap</h2></div><p>Small steps. Real practice.</p></div>
      <div class="timeline">
        <article class="phase"><small>01 — BUILD</small><h3>Full Stack Projects</h3><p>Build usable applications, improve UI quality and practise backend patterns, validation and access control.</p></article>
        <article class="phase"><small>02 — UNDERSTAND</small><h3>Networking & Linux</h3><p>Practise TCP/IP, DNS, ports, shell basics and log inspection using local labs and trusted learning resources.</p></article>
        <article class="phase"><small>03 — TEST SAFELY</small><h3>Web Security Labs</h3><p>Learn vulnerability assessment and reporting with intentionally vulnerable apps and explicitly authorized targets.</p></article>
      </div>
      <div class="actions" style="margin-top:22px">
        <a class="btn" href="https://owasp.org/www-project-top-ten/" target="_blank" rel="noreferrer">OWASP Top 10 ↗</a>
        <a class="btn" href="https://portswigger.net/web-security" target="_blank" rel="noreferrer">Web Security Academy ↗</a>
        <a class="btn" href="https://tryhackme.com/" target="_blank" rel="noreferrer">TryHackMe ↗</a>
      </div>
    </section>

    <section class="wrap section" id="contact">
      <div class="connect">
        <div><div class="kicker">04 / Let's connect</div><h2>Have a project in mind?</h2><p>Explore my repositories or reach out to connect professionally.</p></div>
        <div class="actions" style="margin:0">
          <a class="btn primary" href="https://github.com/alexforc2004" target="_blank" rel="noreferrer">GitHub ↗</a>
          <a class="btn" href="https://www.linkedin.com/in/azzedine-oubaid-393059430" target="_blank" rel="noreferrer">LinkedIn ↗</a>
          <a class="btn" href="https://instagram.com/alexfor2004" target="_blank" rel="noreferrer">Instagram ↗</a>
        </div>
      </div>
    </section>
  </main>

  <section class="music" aria-label="Music player">
    <div class="wrap music-inner">
      <div class="track-info">
        <div class="album" aria-hidden="true">♫</div>
        <div><strong id="track-title">GHOSTFACE / RAYA</strong><small id="track-status">Open the music page to listen</small></div>
        <div class="wave" id="wave" aria-hidden="true"><i></i><i></i><i></i><i></i><i></i><i></i><i></i><i></i></div>
      </div>
      <div class="player-controls">
        <button class="control" id="prev" title="Previous track" aria-label="Previous track">↶</button>
        <button class="control play" id="play" title="Play music page" aria-label="Open music page">▶</button>
        <button class="control" id="next" title="Next track" aria-label="Next track">↷</button>
      </div>
      <div class="music-right">
        <span style="font-size:10px;color:var(--muted)">PLAYER</span>
        <input class="volume" id="volume" type="range" min="0" max="100" value="70" aria-label="Volume (visual only)">
        <a class="music-link" href="https://alexforc2004.github.io/alexforc2004-player/" target="_blank" rel="noreferrer">OPEN MUSIC APP ↗</a>
      </div>
    </div>
  </section>

  <footer class="wrap footer"><span>© 2026 AZZEDINE OUBAID · BUILT WITH INTENT</span><span>BUILD. TEST. SECURE. REPEAT.</span></footer>
</div>
<script>
const musicUrl = "https://alexforc2004.github.io/alexforc2004-player/";
const play = document.getElementById("play");
const wave = document.getElementById("wave");
const status = document.getElementById("track-status");
const title = document.getElementById("track-title");
function openMusic() {
  window.open(musicUrl, "_blank", "noopener,noreferrer");
  status.textContent = "Music app opened in a new tab";
  title.textContent = "GHOSTFACE / RAYA";
  wave.classList.add("playing");
  play.textContent = "↗";
  play.setAttribute("aria-label", "Open music app again");
}
play.addEventListener("click", openMusic);
document.getElementById("prev").addEventListener("click", () => {
  status.textContent = "Track controls are available in the music app";
  window.open(musicUrl, "_blank", "noopener,noreferrer");
});
document.getElementById("next").addEventListener("click", () => {
  status.textContent = "Choose the next track in the music app";
  window.open(musicUrl, "_blank", "noopener,noreferrer");
});
document.getElementById("volume").addEventListener("input", (event) => {
  status.textContent = "Volume is controlled in the music app (" + event.target.value + "% preview)";
});
</script>
</body>
</html>'''

path = Path("/mnt/data/azzedine_portfolio_dashboard.html")
path.write_text(html, encoding="utf-8")
print(f"Created {path} ({path.stat().st_size} bytes)")
