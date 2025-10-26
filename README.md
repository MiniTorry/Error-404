# Renombrar archivo sin extensión a .css
mv /workspaces/Error-404/Trustme /workspaces/Error-404/Trustme.css

# Crear index.html
cat > /workspaces/Error-404/index.html <<'HTML'
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Error 404 — Love</title>
  <link rel="stylesheet" href="Trustme.css">
</head>
<body>
  <div class="scene">
    <img id="character" src="character.png" alt="character">
    <div id="quote">Do you believe in love at first sight or do you want to see me again?</div>

    <div id="lights" aria-hidden="true"></div>

    <div id="controls">
      <button id="playBtn">Play music</button>
      <button id="muteBtn" style="display:none">Mute</button>
    </div>

    <iframe id="ytPlayer" style="display:none;width:0;height:0" frameborder="0"
      allow="autoplay; encrypted-media" allowfullscreen></iframe>
  </div>

  <script src="script.js"></script>
</body>
</html>
HTML

# Crear Trustme.css
cat > /workspaces/Error-404/Trustme.css <<'CSS'
/* filepath: /workspaces/Error-404/Trustme.css */
*{box-sizing:border-box}
html,body{height:100%;margin:0;font-family:system-ui,Arial,sans-serif;background:#ffdfe8;display:flex;align-items:center;justify-content:center}
.scene{position:relative; width:100%; max-width:900px; height:100vh; max-height:700px; display:flex;align-items:center;justify-content:center;overflow:hidden}

/* personaje (imagen) */
#character{
  width:360px; max-width:45vw; height:auto; z-index:2;
  transform-origin:50% 75%;
  animation: moveArms 1.1s ease-in-out infinite;
  filter: drop-shadow(0 12px 24px rgba(0,0,0,0.18));
}

/* texto centrado */
#quote{
  position:absolute; bottom:8%; left:0; right:0; margin:0 auto;
  width:90%; max-width:900px; text-align:center; z-index:3;
  color:#ff2d95; font-weight:700; font-size:clamp(18px,3.2vw,34px);
  text-shadow:0 3px 10px rgba(255,255,255,0.85); padding:0 20px;
}

/* luces románticas que parpadean */
#lights{
  position:absolute; inset:0; z-index:1; pointer-events:none;
  background:
    radial-gradient(closest-side at 10% 20%, rgba(255,105,180,0.28), transparent 25%),
    radial-gradient(closest-side at 80% 30%, rgba(255,0,94,0.20), transparent 25%),
    radial-gradient(closest-side at 50% 80%, rgba(128,0,128,0.14), transparent 30%);
  mix-blend-mode:screen;
  animation: lightsBlink 1.6s infinite alternate;
  opacity:0.9;
}

/* controles */
#controls{position:absolute; top:12px; right:12px; z-index:4}
#controls button{background:#fff; border-radius:8px; padding:8px 12px; border:1px solid rgba(0,0,0,0.06); cursor:pointer; font-weight:600}

/* animaciones */
@keyframes moveArms {
  0%{ transform: translateY(0) rotate(-2deg); }
  50%{ transform: translateY(-18px) rotate(3deg); }
  100%{ transform: translateY(0) rotate(-2deg); }
}
@keyframes lightsBlink {
  0%{ filter: hue-rotate(0deg) brightness(0.9); opacity:0.6 }
  50%{ filter: hue-rotate(15deg) brightness(1.15); opacity:0.95 }
  100%{ filter: hue-rotate(-10deg) brightness(0.85); opacity:0.7 }
}
CSS

# Crear script.js
cat > /workspaces/Error-404/script.js <<'JS'
// filepath: /workspaces/Error-404/script.js
const playBtn = document.getElementById('playBtn');
const muteBtn = document.getElementById('muteBtn');
const iframe = document.getElementById('ytPlayer');

const VIDEO_ID = 'izGwDsrQ1eQ';
const embedBase = `https://www.youtube.com/embed/${VIDEO_ID}?rel=0&controls=0&loop=1&playlist=${VIDEO_ID}&enablejsapi=1`;

// reproducir cuando el usuario pulse (evita bloqueos de autoplay)
playBtn.addEventListener('click', () => {
  if (!iframe.src) {
    iframe.style.display = 'block';
    iframe.style.width = '1';
    iframe.style.height = '1';
    iframe.src = embedBase + '&autoplay=1';
    playBtn.textContent = 'Playing...';
    muteBtn.style.display = 'inline-block';
  } else {
    playBtn.textContent = 'Playing...';
  }
});

// alternar mute (recarga iframe con mute param)
muteBtn.addEventListener('click', () => {
  const muted = iframe.src.includes('mute=1');
  iframe.src = embedBase + (muted ? '&autoplay=1' : '&autoplay=1&mute=1');
  muteBtn.textContent = muted ? 'Mute' : 'Unmute';
});
JS

# Abrir en el navegador del host
"$BROWSER" "file:///workspaces/Error-404/index.html"
