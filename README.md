<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Cielo — Te amo</title>
  <style>
    :root{
      --bg1:#ff9a9e; --bg2:#fad0c4; --accent:#ff2f63;
    }
    *{box-sizing:border-box}
    html,body{height:100%;margin:0;font-family:system-ui,Segoe UI,Roboto,Arial}
    body{
      background: linear-gradient(135deg,var(--bg1),var(--bg2));
      overflow:hidden;
      display:flex;
      align-items:center;
      justify-content:center;
      padding:24px;
    }
    .stage{
      width:100%;
      max-width:980px;
      text-align:center;
      color:white;
      position:relative;
      border-radius:16px;
      padding:40px 24px;
      box-shadow:0 12px 40px rgba(0,0,0,0.12);
      background: linear-gradient(180deg, rgba(255,255,255,0.06), rgba(255,255,255,0.02));
      backdrop-filter: blur(6px);
    }

    h1{
      font-size:clamp(28px,6vw,54px);
      margin:0 0 12px 0;
      letter-spacing:1px;
      text-shadow:0 6px 20px rgba(0,0,0,0.25);
    }
    p.lead{margin:0 0 18px 0;font-size:18px;opacity:0.95}

    /* botones */
    .controls{display:flex;gap:10px;justify-content:center;flex-wrap:wrap;margin-top:12px}
    button{
      background:linear-gradient(90deg,var(--accent),#ff7aa2);
      border:0;color:white;padding:10px 16px;border-radius:10px;font-weight:700;cursor:pointer;
      box-shadow:0 8px 20px rgba(255,90,130,0.12);
    }
    button.secondary{background:transparent;border:2px solid rgba(255,255,255,0.12);color:white;font-weight:600}

    /* corazones animados */
    .floating-heart{
      position:fixed;
      bottom:-50px;
      pointer-events:none;
      font-size:20px;
      opacity:0.95;
      transform-origin:center;
      will-change:transform,opacity;
    }

    @keyframes floatUp {
      0% { transform: translateY(0) rotate(0deg) scale(1); opacity:1; }
      100% { transform: translateY(-120vh) rotate(30deg) scale(1.4); opacity:0; }
    }

    /* overlay para mostrar texto letra-por-letra opcional */
    .typewriter{font-weight:800; font-size:1.1rem; display:block; margin-top:10px}
    footer{position:relative;margin-top:18px;font-size:13px;opacity:0.9}
    .credit{opacity:0.8;font-size:12px}
  </style>
</head>
<body>
  <div class="stage" id="stage">
    <h1 id="mainMsg">💖 Cielo, te amo 💖</h1>
    <div class="typewriter" id="typewriter"></div>
    <p class="lead">Una sorpresa hecha con cariño — toca los botones para música y corazones.</p>

    <div class="controls">
      <button id="playBtn">🎵 Reproducir música</button>
      <button id="pauseBtn" style="display:none">⏸️ Pausar</button>
      <button class="secondary" id="heartsBtn">✨ Lluvia de corazones</button>
      <button class="secondary" id="typeBtn">✍️ Mostrar mensaje</button>
    </div>

    <footer>
      <div class="credit">Para publicar: sube este archivo y tu `romantic.mp3` al repo y activa GitHub Pages.</div>
    </footer>
  </div>

  <!-- Coloca tu archivo de audio en la misma carpeta del index.html con el nombre "romantic.mp3" -->
  <audio id="audio" src="romantic.mp3" loop preload="auto"></audio>

  <script>
    // Texto central (puedes editar)
    const MAIN_TEXT = "Cielo, te amo con todo mi corazón. Eres mi luz.";
    document.getElementById('mainMsg').textContent = "💖 Cielo, te amo 💖";

    // Typewriter
    const typeEl = document.getElementById('typewriter');
    document.getElementById('typeBtn').addEventListener('click', ()=> {
      typeEl.textContent = '';
      let i = 0;
      const text = MAIN_TEXT;
      const timer = setInterval(()=> {
        typeEl.textContent += text[i++];
        if(i >= text.length) clearInterval(timer);
      }, 60);
    });

    // Corazones
    const spawnHeart = (x) => {
      const el = document.createElement('div');
      el.className = 'floating-heart';
      el.innerText = '❤';
      const size = 12 + Math.random()*40;
      el.style.fontSize = size + 'px';
      el.style.left = (x ?? (Math.random()*100)) + 'vw';
      el.style.animation = `floatUp ${4 + Math.random()*3}s linear`;
      el.style.opacity = 0.95;
      document.body.appendChild(el);
      setTimeout(()=> el.remove(), 8000);
    };

    // Lluvia de corazones
    document.getElementById('heartsBtn').addEventListener('click', ()=>{
      for(let i=0;i<28;i++){
        setTimeout(()=> spawnHeart(Math.random()*100), i*80);
      }
    });

    // Música
    const audio = document.getElementById('audio');
    const playBtn = document.getElementById('playBtn');
    const pauseBtn = document.getElementById('pauseBtn');
    playBtn.addEventListener('click', async ()=>{
      try{
        await audio.play();
        playBtn.style.display='none';
        pauseBtn.style.display='inline-block';
      }catch(e){
        alert('Tu navegador bloqueó la reproducción automática. Toca la página o sube la canción correctamente.');
      }
    });
    pauseBtn.addEventListener('click', ()=>{
      audio.pause();
      pauseBtn.style.display='none';
      playBtn.style.display='inline-block';
    });

    // Pequeña lluvia constante (baja intensidad)
    setInterval(()=>{ if(Math.random()<0.4) spawnHeart(Math.random()*100); }, 900);
  </script>
</body>
</html>
