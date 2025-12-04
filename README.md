
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8" />
  <title>海洋曼陀羅螢幕保護程式</title>
  <style>
    /*
      使用說明：
      1. 將本檔案存成 ocean_mandala_screensaver.html。
      2. 直接用瀏覽器（Chrome / Edge / Safari）開啟。
      3. 按 F11（或瀏覽器全螢幕快捷鍵）進入全螢幕，就可以當作螢幕保護動畫。

      效果：
      - 以「海洋波浪」為意象的黑白曼陀羅線條。
      - 線條會像手繪一樣，從中心向外、逐步被畫出來。
      - 當畫面畫滿後會停留片刻，再緩慢淡出，然後重新開始新一輪繪製。
    */

    html, body {
      margin: 0;
      padding: 0;
      overflow: hidden;
      height: 100%;
      width: 100%;
      background: #fdfcf7; /* 類紙張底色，黑白手繪感 */
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
    }

    canvas {
      display: block;
    }

    #hint {
      position: fixed;
      left: 50%;
      bottom: 20px;
      transform: translateX(-50%);
      color: rgba(0, 0, 0, 0.45);
      font-size: 11px;
      letter-spacing: 0.12em;
      text-transform: uppercase;
      pointer-events: none;
      user-select: none;
    }
  </style>
</head>
<body>
  <canvas id="mandala"></canvas>
  <div id="hint">全螢幕觀看以模擬螢幕保護程式效果</div>

  <script>
    (function () {
      const canvas = document.getElementById('mandala');
      const ctx = canvas.getContext('2d');
      let w, h, cx, cy, baseSize;

      function resize() {
        w = window.innerWidth;
        h = window.innerHeight;
        canvas.width = w;
        canvas.height = h;
        cx = w / 2;
        cy = h / 2;
        baseSize = Math.min(w, h) * 0.45; // 曼陀羅最大半徑
      }

      window.addEventListener('resize', resize);
      resize();

      const TAU = Math.PI * 2;
      const SYMMETRY = 10;          // 曼陀羅對稱數量（可改 8、10、12）
      const SECTOR_ANGLE = TAU / SYMMETRY;

      let strokes = [];             // 每一筆基本曲線（只存一個扇形，再旋轉複製）
      let totalSegments = 0;        // 所有曲線段數加總，用來控制「畫滿前的時間」
      let drawCursor = 0;           // 目前已「畫到」第幾個 segment（逐步繪製）
      let drawSpeed = 300;          // 每秒繪製多少 segment，稍後會依 totalSegments 調整

      let mode = 'draw';            // draw | hold | fade
      let modeTime = 0;             // 當前 mode 已經持續多久（秒）

      function rand(min, max) {
        return min + Math.random() * (max - min);
      }

      function generateStrokes() {
        strokes = [];
        totalSegments = 0;
        drawCursor = 0;
        mode = 'draw';
        modeTime = 0;

        const layers = 10;           // 曼陀羅的「圈數」
        const perLayer = 18;         // 每一圈的基本波浪曲線數量
        const minR = baseSize * 0.08;
        const maxR = baseSize * 1.0;

        for (let layer = 0; layer < layers; layer++) {
          const tLayer = layers <= 1 ? 0 : layer / (layers - 1);
          const baseR = minR + (maxR - minR) * tLayer; // 該圈大致半徑
          const amp = baseR * rand(0.05, 0.16);        // 波浪振幅

          for (let j = 0; j < perLayer; j++) {
            const points = [];
            const segs = Math.floor(rand(45, 80));     // 一筆線要分成幾小段

            // 一筆線在單一扇形中的角度範圍
            const arcSpan = SECTOR_ANGLE * rand(0.55, 0.9);
            const centerAngle = rand(-SECTOR_ANGLE * 0.25, SECTOR_ANGLE * 0.25);
            const startAngle = centerAngle - arcSpan / 2;

            const waveFreq = rand(1.5, 3.5);           // 波浪頻率
            const radialJitter = baseR * 0.04;         // 半徑抖動，模擬手感

            for (let i = 0; i <= segs; i++) {
              const t = i / segs; // 0 ~ 1
              const ease = 0.5 - 0.5 * Math.cos(t * Math.PI); // 線性 + 一點點緩和

              const angle = startAngle + arcSpan * ease;
              let r = baseR + amp * Math.sin(t * TAU * waveFreq + rand(-0.4, 0.4));
              r += rand(-radialJitter, radialJitter);

              // 讓比較外圈的線條有一點「海面浪花」的碎裂感
              if (tLayer > 0.6) {
                r += amp * 0.4 * Math.sin(t * TAU * rand(2, 4));
              }

              points.push({ r, angle });
            }

            strokes.push({ points });
            totalSegments += segs;
          }
        }

        // 依照總段數調整繪製速度，約 60 秒畫滿
        const targetDrawSeconds = 60;
        drawSpeed = totalSegments / targetDrawSeconds;
      }

      function drawStrokeSegment(p0, p1, rotation) {
        const a0 = p0.angle + rotation;
        const a1 = p1.angle + rotation;
        const x0 = cx + p0.r * Math.cos(a0);
        const y0 = cy + p0.r * Math.sin(a0);
        const x1 = cx + p1.r * Math.cos(a1);
        const y1 = cy + p1.r * Math.sin(a1);

        ctx.moveTo(x0, y0);
        ctx.lineTo(x1, y1);
      }

      function renderMandala(currentSegments) {
        ctx.save();
        ctx.lineCap = 'round';
        ctx.lineJoin = 'round';
        ctx.strokeStyle = '#111';   // 黑色線條
        ctx.lineWidth = 0.9;        // 基本線寬

        let remaining = currentSegments;

        for (let s = 0; s < strokes.length; s++) {
          if (remaining <= 0) break;
          const { points } = strokes[s];
          const segCount = points.length - 1;
          const drawCount = Math.min(segCount, remaining);
          if (drawCount <= 0) continue;

          for (let sym = 0; sym < SYMMETRY; sym++) {
            const rot = sym * SECTOR_ANGLE;
            ctx.beginPath();
            for (let i = 0; i < drawCount; i++) {
              const p0 = points[i];
              const p1 = points[i + 1];
              drawStrokeSegment(p0, p1, rot);
            }
            ctx.stroke();
          }

          remaining -= drawCount;
        }

        ctx.restore();
      }

      let lastTime = performance.now();

      function loop(now) {
        const dt = (now - lastTime) / 1000;
        lastTime = now;
        modeTime += dt;

        // 背景處理：依不同階段有不同效果
        if (mode === 'fade') {
          // 漸漸回到紙張顏色，保留一點殘影感
          ctx.fillStyle = 'rgba(253, 252, 247, 0.15)';
          ctx.fillRect(0, 0, w, h);
        } else {
          // 繪製階段與停留階段，直接清成乾淨紙面
          ctx.fillStyle = '#fdfcf7';
          ctx.fillRect(0, 0, w, h);
        }

        if (mode === 'draw') {
          drawCursor += drawSpeed * dt;
          if (drawCursor >= totalSegments) {
            drawCursor = totalSegments;
            mode = 'hold';
            modeTime = 0;
          }
          renderMandala(drawCursor);
        } else if (mode === 'hold') {
          // 畫滿後，畫面維持靜止幾秒
          renderMandala(totalSegments);
          if (modeTime > 5) { // 停留 5 秒
            mode = 'fade';
            modeTime = 0;
          }
        } else if (mode === 'fade') {
          // 淡出階段：仍重畫一次完整曼陀羅，但被上面的半透明背景慢慢蓋掉
          renderMandala(totalSegments);
          if (modeTime > 4) { // 約 4 秒淡出
            generateStrokes(); // 重新產生新圖案
          }
        }

        requestAnimationFrame(loop);
      }

      generateStrokes();
      lastTime = performance.now();
      requestAnimationFrame(loop);
    })();
  </script>
</body>
</html>
