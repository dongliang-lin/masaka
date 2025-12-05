<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>專注發電 Focus Power</title>
  <style>
    :root {
      --night: radial-gradient(circle at 20% 20%, rgba(78, 118, 163, 0.32), transparent 35%),
        radial-gradient(circle at 80% 10%, rgba(111, 92, 160, 0.38), transparent 32%),
        #0d1a2e;
      --board: #0f1727;
      --board-light: #8ff1e3;
      --board-dim: #3e6a75;
      --tree-dark: #0f2a22;
      --tree-shadow: #0a1e18;
      --tree-accent: #1f4235;
      --light-off: #3a4b46;
      --light-on: #c2fbd7;
      --light-glow: 0 0 12px rgba(178, 255, 230, 0.8);
      --gold: #ffd166;
      --panel: rgba(12, 24, 39, 0.6);
      --text: #e7f4ff;
      --muted: #7ea0b9;
      --danger: #f05d5e;
      --success: #76f7bf;
    }

    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      min-height: 100vh;
      color: var(--text);
      background: var(--night);
      font-family: "Noto Sans TC", "Inter", "Segoe UI", system-ui, -apple-system, sans-serif;
      display: flex;
      align-items: stretch;
      justify-content: center;
      padding: 24px 22px 32px;
      overflow: hidden;
    }

    main {
      position: relative;
      width: min(1200px, 100%);
      background: linear-gradient(180deg, rgba(18, 33, 53, 0.8), rgba(12, 24, 39, 0.8));
      border: 1px solid rgba(255, 255, 255, 0.06);
      box-shadow: 0 20px 60px rgba(0, 0, 0, 0.55);
      border-radius: 18px;
      overflow: hidden;
    }

    .scene {
      position: relative;
      height: 100%;
      min-height: 640px;
      padding: 36px 32px 90px;
      display: grid;
      grid-template-columns: 1.3fr 1fr;
      gap: 20px;
      isolation: isolate;
    }

    .sky {
      position: absolute;
      inset: 0;
      background: radial-gradient(circle at 50% 18%, rgba(255, 255, 255, 0.1), transparent 36%), var(--night);
      z-index: 1;
    }

    .sky::after {
      content: "";
      position: absolute;
      inset: 0;
      background-image: radial-gradient(2px 2px at 20% 20%, rgba(255, 255, 255, 0.42), transparent),
        radial-gradient(2px 2px at 80% 35%, rgba(255, 255, 255, 0.28), transparent),
        radial-gradient(1px 1px at 60% 50%, rgba(255, 255, 255, 0.26), transparent),
        radial-gradient(1px 1px at 30% 65%, rgba(255, 255, 255, 0.18), transparent);
      animation: twinkle 6s ease-in-out infinite;
      opacity: 0.8;
    }

    @keyframes twinkle {
      0%,
      100% {
        opacity: 0.6;
      }
      50% {
        opacity: 1;
      }
    }

    .city {
      position: absolute;
      inset: auto 0 0;
      height: 40%;
      z-index: 2;
      overflow: hidden;
      pointer-events: none;
    }

    .building {
      position: absolute;
      bottom: 0;
      width: 180px;
      border-radius: 6px 6px 0 0;
      background: linear-gradient(180deg, rgba(21, 39, 63, 0.9), rgba(10, 18, 28, 0.95));
      box-shadow: inset 0 2px 8px rgba(255, 255, 255, 0.05);
    }

    .building.small {
      width: 120px;
      background: linear-gradient(180deg, rgba(20, 34, 52, 0.85), rgba(6, 12, 20, 0.9));
    }

    .building::after {
      content: "";
      position: absolute;
      inset: 14px 14px;
      background-image: repeating-linear-gradient(90deg, rgba(255, 224, 146, 0.08), rgba(255, 224, 146, 0.08) 12px, transparent 12px, transparent 30px),
        repeating-linear-gradient(180deg, rgba(255, 224, 146, 0.08), rgba(255, 224, 146, 0.08) 12px, transparent 12px, transparent 30px);
      mix-blend-mode: screen;
    }

    .building:nth-child(1) {
      left: 40px;
      height: 38%;
    }

    .building:nth-child(2) {
      left: 200px;
      height: 52%;
      background: linear-gradient(180deg, rgba(18, 36, 61, 0.9), rgba(5, 10, 18, 0.92));
    }

    .building:nth-child(3) {
      left: 380px;
      height: 45%;
    }

    .building:nth-child(4) {
      left: 580px;
      height: 60%;
    }

    .building:nth-child(5) {
      left: 760px;
      height: 42%;
    }

    .snow {
      position: absolute;
      inset: 0;
      z-index: 3;
      pointer-events: none;
      overflow: hidden;
    }

    .snow span {
      position: absolute;
      top: -10px;
      width: 6px;
      height: 6px;
      background: rgba(255, 255, 255, 0.8);
      border-radius: 50%;
      filter: blur(0.3px);
      animation: snow 12s linear infinite;
      opacity: 0.8;
    }

    @keyframes snow {
      0% {
        transform: translateY(-10px);
      }
      100% {
        transform: translateY(120vh);
      }
    }

    .tree-area {
      position: relative;
      z-index: 4;
      display: flex;
      align-items: center;
      justify-content: center;
    }

    .tree {
      position: relative;
      width: 420px;
      height: 520px;
      display: grid;
      place-items: center;
      filter: drop-shadow(0 16px 32px rgba(0, 0, 0, 0.55));
    }

    .tree-body {
      position: absolute;
      bottom: 46px;
      width: 320px;
      height: 420px;
      background: linear-gradient(180deg, var(--tree-accent), var(--tree-dark));
      clip-path: polygon(50% 0%, 84% 24%, 66% 26%, 90% 44%, 68% 46%, 88% 64%, 62% 66%, 50% 90%, 38% 66%, 12% 64%, 32% 46%, 10% 44%, 34% 26%, 16% 24%);
      border-radius: 12px;
      box-shadow: inset 0 -12px 30px rgba(0, 0, 0, 0.35), inset 0 12px 24px rgba(255, 255, 255, 0.04);
      overflow: hidden;
    }

    .tree-body::after {
      content: "";
      position: absolute;
      inset: 0;
      background: radial-gradient(ellipse at 50% 30%, rgba(255, 255, 255, 0.06), transparent 60%);
      mix-blend-mode: screen;
    }

    .tree-shadow {
      position: absolute;
      bottom: 32px;
      width: 240px;
      height: 22px;
      background: radial-gradient(circle, rgba(0, 0, 0, 0.32), transparent 60%);
      filter: blur(8px);
      z-index: -1;
    }

    .tree-star {
      position: absolute;
      top: 40px;
      width: 64px;
      height: 64px;
      background: linear-gradient(145deg, #c3c8ce, #d6d9dd);
      clip-path: polygon(50% 0%, 61% 38%, 100% 38%, 68% 59%, 79% 100%, 50% 76%, 21% 100%, 32% 59%, 0% 38%, 39% 38%);
      filter: drop-shadow(0 0 6px rgba(255, 255, 255, 0.6));
      transition: 0.8s ease;
      transform-origin: center;
      opacity: 0.72;
    }

    .tree-star.lit {
      background: linear-gradient(145deg, #ffe08a, #ffd166, #fff2c6);
      box-shadow: 0 0 20px rgba(255, 209, 102, 0.9), 0 0 40px rgba(255, 232, 165, 0.7);
      animation: pulse 2.4s ease-in-out infinite;
      opacity: 1;
    }

    @keyframes pulse {
      0%,
      100% {
        transform: scale(1);
      }
      50% {
        transform: scale(1.08) rotate(-2deg);
      }
    }

    .light-layers {
      position: absolute;
      inset: 90px 40px 90px;
      display: grid;
      grid-template-rows: repeat(12, 1fr);
      gap: 6px;
      width: 80%;
      z-index: 2;
    }

    .light-layer {
      --spread: 60%;
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 10px;
      width: calc(40% + var(--spread));
      margin: 0 auto;
      opacity: 0.75;
      transition: 0.6s ease;
    }

    .light-layer .bulb {
      width: 12px;
      height: 12px;
      border-radius: 50%;
      background: var(--light-off);
      box-shadow: none;
      opacity: 0.45;
      transition: 0.5s ease;
    }

    .light-layer.active {
      opacity: 1;
    }

    .light-layer.active .bulb {
      background: var(--light-on);
      box-shadow: var(--light-glow);
      opacity: 1;
    }

    .light-layer.final .bulb {
      background: var(--bulb-color, #ffd166);
      box-shadow: 0 0 10px rgba(255, 255, 255, 0.35), 0 0 20px rgba(255, 198, 124, 0.7);
      animation: shimmer 1.6s ease-in-out infinite alternate;
    }

    @keyframes shimmer {
      0% {
        transform: translateY(0);
        filter: saturate(1);
      }
      100% {
        transform: translateY(-1px) scale(1.04);
        filter: saturate(1.15);
      }
    }

    .trunk {
      position: absolute;
      bottom: 10px;
      width: 80px;
      height: 80px;
      background: linear-gradient(180deg, #4a3726, #2d2219);
      border-radius: 6px;
      box-shadow: inset 0 6px 10px rgba(255, 255, 255, 0.08);
    }

    .message {
      margin-top: 20px;
      min-height: 32px;
      text-align: center;
      font-weight: 600;
      letter-spacing: 0.05em;
      color: var(--gold);
      opacity: 0;
      transition: 0.6s ease;
    }

    .message.show {
      opacity: 1;
    }

    .panel {
      position: relative;
      z-index: 4;
      display: flex;
      flex-direction: column;
      align-items: flex-end;
      gap: 16px;
      padding-top: 30px;
    }

    .board {
      width: 100%;
      background: var(--board);
      padding: 28px 20px;
      border-radius: 14px;
      box-shadow: inset 0 0 0 1px rgba(143, 241, 227, 0.16), 0 12px 28px rgba(0, 0, 0, 0.38);
      position: relative;
      overflow: hidden;
    }

    .board::after {
      content: "";
      position: absolute;
      inset: 6px;
      border: 1px dashed rgba(143, 241, 227, 0.16);
      border-radius: 10px;
      pointer-events: none;
    }

    .timer-label {
      letter-spacing: 0.24em;
      color: var(--board-dim);
      font-size: 12px;
      text-transform: uppercase;
      margin-bottom: 10px;
    }

    #timer {
      display: block;
      font-family: "Share Tech Mono", "Roboto Mono", monospace;
      font-size: clamp(64px, 7vw, 86px);
      letter-spacing: 0.16em;
      color: var(--board-light);
      text-shadow: 0 0 8px rgba(143, 241, 227, 0.55);
      text-align: right;
      line-height: 1;
    }

    .controls {
      background: var(--panel);
      padding: 16px;
      border-radius: 12px;
      display: flex;
      gap: 12px;
      flex-wrap: wrap;
      backdrop-filter: blur(6px);
      border: 1px solid rgba(255, 255, 255, 0.08);
      box-shadow: 0 12px 28px rgba(0, 0, 0, 0.38);
    }

    label {
      font-size: 14px;
      color: var(--muted);
    }

    input[type='number'] {
      width: 92px;
      padding: 10px 12px;
      border-radius: 10px;
      border: 1px solid rgba(255, 255, 255, 0.08);
      background: rgba(9, 15, 25, 0.6);
      color: var(--text);
      font-size: 15px;
      outline: none;
    }

    input[type='number']::-webkit-inner-spin-button {
      opacity: 0.5;
    }

    .button-row {
      display: flex;
      gap: 10px;
      flex-wrap: wrap;
    }

    button {
      padding: 12px 16px;
      border-radius: 10px;
      border: 1px solid rgba(255, 255, 255, 0.1);
      background: linear-gradient(180deg, rgba(143, 241, 227, 0.08), rgba(143, 241, 227, 0.02));
      color: var(--text);
      font-weight: 700;
      letter-spacing: 0.05em;
      cursor: pointer;
      transition: 0.25s ease;
      min-width: 110px;
    }

    button:hover {
      transform: translateY(-2px);
      box-shadow: 0 6px 18px rgba(0, 0, 0, 0.35);
    }

    button:active {
      transform: translateY(0);
    }

    .primary {
      background: linear-gradient(180deg, rgba(118, 247, 191, 0.22), rgba(118, 247, 191, 0.08));
      color: #dafced;
      border-color: rgba(118, 247, 191, 0.4);
    }

    .danger {
      background: linear-gradient(180deg, rgba(240, 93, 94, 0.18), rgba(240, 93, 94, 0.08));
      border-color: rgba(240, 93, 94, 0.4);
    }

    .secondary {
      background: linear-gradient(180deg, rgba(126, 160, 185, 0.22), rgba(126, 160, 185, 0.08));
      border-color: rgba(126, 160, 185, 0.35);
    }

    .legend {
      margin-top: 8px;
      color: var(--muted);
      font-size: 13px;
      letter-spacing: 0.05em;
    }

    @media (max-width: 1020px) {
      .scene {
        grid-template-columns: 1fr;
        padding: 28px 24px 82px;
      }

      .tree {
        width: 340px;
        height: 460px;
      }

      .tree-body {
        width: 270px;
        height: 360px;
      }

      .light-layers {
        inset: 80px 34px 80px;
      }

      .panel {
        align-items: center;
      }

      #timer {
        text-align: center;
      }
    }
  </style>
</head>
<body>
  <main>
    <div class="sky"></div>
    <div class="city">
      <div class="building small"></div>
      <div class="building"></div>
      <div class="building small"></div>
      <div class="building"></div>
      <div class="building small"></div>
    </div>
    <div class="snow" id="snow"></div>
    <section class="scene">
      <section class="tree-area" aria-label="專注發電聖誕樹">
        <div class="tree">
          <div class="tree-star" id="treeStar"></div>
          <div class="tree-body"></div>
          <div class="light-layers" id="lightLayers">
            <div class="light-layer" style="--spread: 28%" data-count="4"></div>
            <div class="light-layer" style="--spread: 35%" data-count="5"></div>
            <div class="light-layer" style="--spread: 42%" data-count="5"></div>
            <div class="light-layer" style="--spread: 48%" data-count="6"></div>
            <div class="light-layer" style="--spread: 56%" data-count="6"></div>
            <div class="light-layer" style="--spread: 64%" data-count="7"></div>
            <div class="light-layer" style="--spread: 72%" data-count="7"></div>
            <div class="light-layer" style="--spread: 80%" data-count="8"></div>
            <div class="light-layer" style="--spread: 88%" data-count="8"></div>
            <div class="light-layer" style="--spread: 96%" data-count="9"></div>
            <div class="light-layer" style="--spread: 104%" data-count="9"></div>
            <div class="light-layer" style="--spread: 112%" data-count="10"></div>
          </div>
          <div class="trunk"></div>
          <div class="tree-shadow"></div>
        </div>
        <div class="message" id="message">&nbsp;</div>
      </section>

      <section class="panel" aria-label="倒數控制區">
        <div class="board">
          <div class="timer-label">focus power board</div>
          <span id="timer">25:00</span>
        </div>
        <div class="controls">
          <div>
            <label for="minutes">倒數分鐘</label>
            <input type="number" id="minutes" min="1" max="180" value="25" />
          </div>
          <div class="button-row">
            <button class="primary" id="start">開始專注</button>
            <button class="secondary" id="pause">暫停</button>
            <button class="danger" id="reset">重設</button>
          </div>
          <div class="legend">點亮聖誕樹，為城市發一輪電</div>
        </div>
      </section>
    </section>
  </main>

  <script>
    const timerEl = document.getElementById('timer');
    const minutesInput = document.getElementById('minutes');
    const startBtn = document.getElementById('start');
    const pauseBtn = document.getElementById('pause');
    const resetBtn = document.getElementById('reset');
    const lightLayers = Array.from(document.querySelectorAll('.light-layer'));
    const treeStar = document.getElementById('treeStar');
    const messageEl = document.getElementById('message');

    let totalSeconds = parseInt(minutesInput.value, 10) * 60;
    let remainingSeconds = totalSeconds;
    let timerId = null;
    let running = false;

    function pad(value) {
      return value.toString().padStart(2, '0');
    }

    function formatTime(seconds) {
      const m = Math.floor(seconds / 60);
      const s = seconds % 60;
      return `${pad(m)}:${pad(s)}`;
    }

    function setMessage(text, visible = true) {
      messageEl.textContent = text;
      messageEl.classList.toggle('show', visible);
    }

    function buildBulbs() {
      const palette = ['#ff9f68', '#ffa7c4', '#8ef1b0', '#8cd0ff', '#ffd166', '#c7b9ff'];
      lightLayers.forEach((layer, idx) => {
        const count = Number(layer.dataset.count || 5);
        layer.innerHTML = '';
        for (let i = 0; i < count; i++) {
          const bulb = document.createElement('span');
          bulb.className = 'bulb';
          bulb.style.setProperty('--bulb-color', palette[(idx + i) % palette.length]);
          layer.appendChild(bulb);
        }
      });
    }

    function updateLights(progress) {
      const litLayers = Math.floor(progress * lightLayers.length);
      lightLayers.forEach((layer, index) => {
        const bulbs = layer.querySelectorAll('.bulb');
        if (index < litLayers) {
          layer.classList.add('active');
          bulbs.forEach((bulb) => bulb.style.background = bulb.style.getPropertyValue('--bulb-color') || '');
        } else {
          layer.classList.remove('active', 'final');
          bulbs.forEach((bulb) => bulb.style.background = '');
        }
      });
    }

    function celebrate() {
      treeStar.classList.add('lit');
      lightLayers.forEach((layer) => {
        layer.classList.add('final');
        layer.classList.add('active');
      });
      setMessage('專注完成，聖誕樹亮起來了！');
    }

    function resetTree() {
      treeStar.classList.remove('lit');
      lightLayers.forEach((layer) => {
        layer.classList.remove('active', 'final');
      });
      setMessage('\u00a0', false);
    }

    function updateTimerDisplay() {
      timerEl.textContent = formatTime(remainingSeconds);
    }

    function tick() {
      if (!running) return;
      if (remainingSeconds <= 0) {
        clearInterval(timerId);
        timerId = null;
        running = false;
        celebrate();
        return;
      }

      remainingSeconds -= 1;
      const elapsed = totalSeconds - remainingSeconds;
      const progress = totalSeconds === 0 ? 0 : Math.min(elapsed / totalSeconds, 1);
      updateLights(progress);
      updateTimerDisplay();

      if (remainingSeconds === 0) {
        celebrate();
      }
    }

    function startTimer() {
      if (running) return;
      totalSeconds = Math.max(1, parseInt(minutesInput.value, 10) || 0) * 60;
      if (remainingSeconds === 0 || remainingSeconds === totalSeconds) {
        resetTree();
        remainingSeconds = totalSeconds;
        updateLights(0);
        updateTimerDisplay();
      }
      running = true;
      timerId = setInterval(tick, 1000);
    }

    function pauseTimer() {
      running = false;
      if (timerId) {
        clearInterval(timerId);
        timerId = null;
      }
    }

    function resetTimer() {
      pauseTimer();
      totalSeconds = Math.max(1, parseInt(minutesInput.value, 10) || 0) * 60;
      remainingSeconds = totalSeconds;
      resetTree();
      updateLights(0);
      updateTimerDisplay();
    }

    startBtn.addEventListener('click', startTimer);
    pauseBtn.addEventListener('click', pauseTimer);
    resetBtn.addEventListener('click', resetTimer);

    buildBulbs();
    updateTimerDisplay();
    updateLights(0);

    function createSnow(count = 30) {
      const snow = document.getElementById('snow');
      const width = snow.clientWidth || window.innerWidth;
      const height = snow.clientHeight || window.innerHeight;
      for (let i = 0; i < count; i++) {
        const flake = document.createElement('span');
        const size = Math.random() * 3 + 2;
        const duration = 8 + Math.random() * 6;
        const delay = Math.random() * 8;
        flake.style.left = `${Math.random() * width}px`;
        flake.style.width = `${size}px`;
        flake.style.height = `${size}px`;
        flake.style.animationDuration = `${duration}s`;
        flake.style.animationDelay = `${delay}s`;
        flake.style.opacity = `${0.45 + Math.random() * 0.4}`;
        snow.appendChild(flake);
      }
    }

    createSnow(42);
  </script>
</body>
</html>
