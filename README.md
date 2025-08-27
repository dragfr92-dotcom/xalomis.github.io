# xalomis.github.io
obleas
<!DOCTYPE html>
<html lang="es" dir="ltr">
<head>
  <meta charset="utf-8" />
  <title>Audio accesible</title>
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    html, body { margin:0; padding:0; font-family:system-ui, Arial, sans-serif; }
    main { min-height:100dvh; display:grid; place-items:center; padding:2rem; }
    .card {
      width:100%; max-width:32rem; text-align:center; border:1px solid #ddd;
      border-radius:1rem; padding:2rem; box-shadow:0 6px 24px rgba(0,0,0,.08);
    }
    button {
      display:inline-block; font-size:1.25rem; padding:1rem 1.5rem; border-radius:999px;
      border:none; cursor:pointer; margin-top:1rem; background:#111; color:#fff;
    }
    .hint { font-size:0.95rem; color:#555; margin-top:0.75rem; }
    .sr-only { position:absolute; left:-9999px; }
  </style>
</head>
<body>
  <main>
    <div class="card" role="region" aria-labelledby="t1" aria-describedby="d1">
      <h1 id="t1">Reproducción de audio</h1>
      <p id="d1">Si el audio no inicia solo por políticas del navegador, toca el botón una vez.</p>

      <!-- Reemplaza estas URLs por tus enlaces públicos -->
      <audio id="audio" preload="auto">
        <source src="C:\Users\dragf\OneDrive\Escritorio\prueba\xalo.ogg" type="audio/ogg" />
        Tu navegador no soporta audio HTML5.
      </audio>

      <button id="playBtn" aria-describedby="assist1">▶︎ Reproducir audio</button>
      <p id="assist1" class="hint" aria-live="assertive">
        Consejo: con lectores de pantalla, este botón se anuncia al abrir la página.
      </p>
      <p class="sr-only" id="liveAnnounce" aria-live="assertive">
        El audio está listo. Toca el botón reproducir para escucharlo.
      </p>
    </div>
  </main>

  <script>
    const audio = document.getElementById('audio');
    const playBtn = document.getElementById('playBtn');

    // Intento de autoplay (puede funcionar en algunos Android)
    (async () => {
      try {
        await audio.play();
        playBtn.textContent = '⏸️ Pausar';
      } catch(e) { /* Bloqueado: 1 toque será suficiente */ }
    })();

    window.addEventListener('load', () => {
      playBtn.focus();
      const live = document.getElementById('liveAnnounce');
      live.textContent = 'El audio está listo. Toca el botón reproducir para escucharlo.';
      if (navigator.vibrate) navigator.vibrate(100);
    });

    playBtn.addEventListener('click', async () => {
      try {
        if (audio.paused) {
          await audio.play();
          playBtn.textContent = '⏸️ Pausar';
        } else {
          audio.pause();
          playBtn.textContent = '▶︎ Reproducir audio';
        }
      } catch (e) {
        alert('No se pudo reproducir el audio. Verifica el volumen o inténtalo de nuevo.');
      }
    });

    audio.addEventListener('play', () => playBtn.textContent = '⏸️ Pausar');
    audio.addEventListener('pause', () => playBtn.textContent = '▶︎ Reproducir audio');
    audio.addEventListener('ended', () => playBtn.textContent = '▶︎ Reproducir otra vez');
  </script>
</body>
</html>
