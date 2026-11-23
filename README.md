index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>ONYX-OS // ZANE v18.0</title>
  <style>
    :root {
      --bg: #07080a;
      --panel: #0e1116;
      --border: #1e2530;
      --accent: #00f59b;
      --accent-dim: rgba(0, 245, 155, 0.14);
      --cyan: #38bdf8;
      --text: #e2e8f0;
      --muted: #64748b;
    }
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      background: var(--bg);
      color: var(--text);
      font-family: 'Courier New', Courier, monospace;
      min-height: 100dvh;
      display: flex;
      flex-direction: column;
      overflow-x: hidden;
    }
    canvas#matrix {
      position: fixed;
      inset: 0;
      z-index: 0;
      opacity: 0.18;
      pointer-events: none;
      display: none;
    }
    /* Boot Screen */
    #boot-screen {
      position: fixed;
      inset: 0;
      background: var(--bg);
      z-index: 50;
      display: flex;
      flex-direction: column;
      justify-content: center;
      padding: 24px;
      cursor: pointer;
      user-select: none;
    }
    .boot-title {
      color: var(--accent);
      font-size: 0.95rem;
      font-weight: bold;
      letter-spacing: 1px;
      margin-bottom: 18px;
    }
    .bar-wrap {
      width: 100%;
      height: 8px;
      border: 1px solid var(--border);
      border-radius: 999px;
      overflow: hidden;
      margin-bottom: 18px;
      background: #0b0d11;
    }
    .bar-fill {
      height: 100%;
      width: 0%;
      background: var(--accent);
      box-shadow: 0 0 12px var(--accent);
      transition: width 0.25s ease;
    }
    .bypass-btn {
      color: var(--cyan);
      font-size: 0.85rem;
      text-decoration: underline;
      background: none;
      border: none;
      text-align: left;
      font-family: inherit;
      padding: 8px 0;
      cursor: pointer;
    }
    /* Main HUD */
    #hud {
      position: relative;
      z-index: 10;
      display: none;
      flex-direction: column;
      min-height: 100dvh;
      max-width: 720px;
      width: 100%;
      margin: 0 auto;
      padding: 16px;
      gap: 14px;
    }
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: var(--panel);
      border: 1px solid var(--border);
      padding: 12px 14px;
      border-radius: 8px;
    }
    .badge {
      background: var(--accent-dim);
      color: var(--accent);
      border: 1px solid var(--accent);
      padding: 3px 8px;
      border-radius: 4px;
      font-size: 0.75rem;
      font-weight: bold;
    }
    .stats {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 10px;
    }
    .card {
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 12px;
    }
    .card-label {
      color: var(--muted);
      font-size: 0.72rem;
      margin-bottom: 4px;
    }
    .card-val {
      color: var(--accent);
      font-size: 1rem;
      font-weight: bold;
    }
    .terminal {
      flex: 1;
      background: var(--panel);
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 14px;
      display: flex;
      flex-direction: column;
      min-height: 280px;
    }
    #output {
      flex: 1;
      overflow-y: auto;
      font-size: 0.88rem;
      line-height: 1.55;
      margin-bottom: 12px;
      white-space: pre-wrap;
    }
    .line-sys { color: var(--accent); }
    .line-cmd { color: var(--cyan); margin-top: 8px; }
    .chips {
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin-bottom: 10px;
    }
    .chip {
      background: #131820;
      color: var(--text);
      border: 1px solid var(--border);
      padding: 6px 10px;
      border-radius: 6px;
      font-family: inherit;
      font-size: 0.8rem;
      cursor: pointer;
    }
    .chip:active {
      border-color: var(--accent);
      color: var(--accent);
    }
    .input-row {
      display: flex;
      align-items: center;
      gap: 8px;
      border-top: 1px solid var(--border);
      padding-top: 10px;
    }
    .prompt { color: var(--accent); font-weight: bold; }
    input#cmd {
      flex: 1;
      background: transparent;
      border: none;
      color: var(--text);
      font-family: inherit;
      font-size: 0.95rem;
      outline: none;
    }
    button#run-btn {
      background: var(--accent);
      color: #050709;
      border: none;
      border-radius: 6px;
      padding: 6px 12px;
      font-family: inherit;
      font-weight: bold;
      font-size: 0.8rem;
      cursor: pointer;
    }
    body.overclock {
      --accent: #f43f5e;
      --accent-dim: rgba(244, 63, 94, 0.16);
    }
  </style>
</head>
<body>
  <canvas id="matrix"></canvas>

  <div id="boot-screen">
    <div class="boot-title">● ONYX-BIOS v18.0.26 // SECURE BOOT</div>
    <div class="bar-wrap"><div class="bar-fill" id="bar"></div></div>
    <button class="bypass-btn" id="bypass-btn">[TAP HERE OR WAIT 2S TO LAUNCH TERMINAL]</button>
  </div>

  <div id="hud">
    <div class="header">
      <div>
        <div style="font-weight:bold; font-size:0.95rem;">ONYX EDGE // ZANE_OS</div>
        <div style="color:var(--muted); font-size:0.75rem;">USER: KRISH // BUILD 30.11.2008</div>
      </div>
      <span class="badge">LEVEL 18 READY</span>
    </div>

    <div class="stats">
      <div class="card">
        <div class="card-label">COUNTDOWN TO v18.0</div>
        <div class="card-val" id="countdown">CALCULATING...</div>
      </div>
      <div class="card">
        <div class="card-label">SYSTEM STATUS</div>
        <div class="card-val" id="sys-mode">NOMINAL // DARK</div>
      </div>
    </div>

    <div class="terminal">
      <div id="output"></div>
      <div class="chips">
        <button class="chip" data-cmd="gift">gift</button>
        <button class="chip" data-cmd="quest">quest</button>
        <button class="chip" data-cmd="matrix">matrix</button>
        <button class="chip" data-cmd="overclock">overclock</button>
        <button class="chip" data-cmd="help">help</button>
      </div>
      <div class="input-row">
        <span class="prompt">&gt;</span>
        <input id="cmd" type="text" placeholder="Type a command or tap a chip..." autocomplete="off" />
        <button id="run-btn">RUN</button>
      </div>
    </div>
  </div>

  <script>
    const bootScreen = document.getElementById('boot-screen');
    const hud = document.getElementById('hud');
    const bar = document.getElementById('bar');
    const output = document.getElementById('output');
    const cmdInput = document.getElementById('cmd');
    let booted = false;

    function launchHUD() {
      if (booted) return;
      booted = true;
      bootScreen.style.display = 'none';
      hud.style.display = 'flex';
      printLine('ONYX-OS v18.0 initialized. Welcome, Zane.', 'line-sys');
      printLine('Tap any quick-command chip below or type "help" to explore.');
    }

    let progress = 0;
    const bootTimer = setInterval(() => {
      progress += 25;
      bar.style.width = Math.min(progress, 100) + '%';
      if (progress >= 100) {
        clearInterval(bootTimer);
        setTimeout(launchHUD, 300);
      }
    }, 350);

    bootScreen.addEventListener('click', launchHUD);
    bootScreen.addEventListener('touchstart', launchHUD, { passive: true });

    function updateCountdown() {
      const now = new Date();
      const target = new Date('2026-11-30T00:00:00+05:30');
      const diff = target - now;
      const el = document.getElementById('countdown');
      if (diff <= 0) {
        el.textContent = 'UNLOCKED // HAPPY 18TH!';
        return;
      }
      const d = Math.floor(diff / (1000 * 60 * 60 * 24));
      const h = Math.floor((diff / (1000 * 60 * 60)) % 24);
      const m = Math.floor((diff / (1000 * 60)) % 60);
      const s = Math.floor((diff / 1000) % 60);
      el.textContent = `${d}d ${h}h ${m}m ${s}s`;
    }
    setInterval(updateCountdown, 1000);
    updateCountdown();

    function printLine(text, cls = '') {
      const div = document.createElement('div');
      if (cls) div.className = cls;
      div.textContent = text;
      output.appendChild(div);
      output.scrollTop = output.scrollHeight;
    }

    const commands = {
      help: () => {
        printLine('Available commands:\n• gift      - Decrypt 18th birthday payload\n• quest     - Load Level 18 milestone targets\n• matrix    - Toggle neon code rain\n• overclock - Toggle Crimson Overclock theme\n• clear     - Clear terminal screen', 'line-sys');
      },
      gift: () => {
        printLine('[DECRYPTING PAYLOAD FOR KRISH // ALIAS: ZANE...]\n\nHappy 18th Birthday in advance! You officially unlock Level 18 on November 30.\nHere is to crushing Class 12, dominating your entrance exams, building viral edits on Onyx Edge, and shipping your own AI/agentic projects.\n\nKeep building—this repo is yours to customize forever.', 'line-sys');
      },
      quest: () => {
        printLine('[ACTIVE LEVEL 18 QUESTS]\n[✓] Deploy first live GitHub Pages web app\n[ ] Master Class 12 PCMB + IT finals\n[ ] Smash entrance exam targets\n[ ] Ship next-level Onyx Edge content', 'line-sys');
      },
      matrix: () => {
        const c = document.getElementById('matrix');
        const active = c.style.display === 'block';
        c.style.display = active ? 'none' : 'block';
        printLine(active ? 'Matrix rain disabled.' : 'Matrix rain enabled.', 'line-sys');
      },
      overclock: () => {
        document.body.classList.toggle('overclock');
        const oc = document.body.classList.contains('overclock');
        document.getElementById('sys-mode').textContent = oc ? 'OVERCLOCK // MAX' : 'NOMINAL // DARK';
        printLine(oc ? 'WARNING: Crimson Overclock engaged.' : 'System restored to Emerald Nominal mode.', 'line-sys');
      },
      clear: () => {
        output.innerHTML = '';
      }
    };

    function runCommand(raw) {
      const cmd = raw.trim().toLowerCase();
      if (!cmd) return;
      printLine('> ' + cmd, 'line-cmd');

      
