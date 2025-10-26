<!doctype html>
<html lang="tr">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Özür Sayfası</title>
  <style>
    :root{ --bg:#f8f6ff; --card:#fff; --accent:#ff6b81; --muted:#6b6b6b }
    *{box-sizing:border-box}
    body{font-family:Inter,system-ui,Segoe UI,Roboto,Arial; background:linear-gradient(120deg,#fff,#f0f6ff);min-height:100vh;display:flex;align-items:center;justify-content:center;padding:24px}
    .card{width:100%;max-width:640px;background:var(--card);border-radius:16px;box-shadow:0 10px 30px rgba(17,24,39,0.08);padding:28px; text-align:center}
    h1{margin:0 0 6px;font-size:24px;color:#111}
    p.description{margin:0 0 18px;color:var(--muted)}
    .apology{background:linear-gradient(180deg,#fff,#fbfbff);border:1px solid #eee;padding:18px;border-radius:12px;min-height:120px;display:flex;align-items:center;justify-content:center}
    .apology[contenteditable]{outline:none;cursor:text}
    .controls{display:flex;gap:12px;justify-content:center;margin-top:18px}
    button{padding:10px 18px;border-radius:10px;border:0;font-weight:600;cursor:pointer}
    .yes{background:var(--accent);color:#fff}
    .no{background:#e6e6e6;color:#333}
    .status{margin-top:18px;min-height:40px}
    .hidden{display:none}

    .confetti-wrap{pointer-events:none;position:fixed;left:0;right:0;top:0;bottom:0;overflow:hidden}
    .heart{position:absolute;width:18px;height:18px;transform:translateY(-20px);opacity:0;animation:fall 2200ms linear forwards}
    .heart:before,.heart:after{content:'';position:absolute;width:9px;height:14px;background:var(--accent);border-radius:9px 9px 0 0;transform-origin:50% 100%}
    .heart:before{left:0;transform:rotate(-45deg)}
    .heart:after{right:0;transform:rotate(45deg)}
    @keyframes fall{
      0%{opacity:1;transform:translateY(-10vh) scale(0.7)}
      100%{opacity:1;transform:translateY(80vh) translateX(30px) rotate(360deg) scale(1)}
    }

    @media (max-width:480px){ .card{padding:18px} h1{font-size:20px} }
  </style>
</head>
<body>
  <div class="card">
    <h1>Bir şey söylemek istiyorum...</h1>
    <p class="description">Aşağıya özrünün içeriğini yaz. (İstersen ben de yardımcı olurum.)</p>

    <div id="apology" class="apology" contenteditable="true" aria-label="Özür metni">
      Seni çok üzdüm bebeiş ama ben öyle demek istemedim ben senden bıkarmıyım sen benim geleceğimsin yaşama sebebimsin Lütfen barışalım baby
    </div>

    <div class="controls">
      <button id="yesBtn" class="yes">Evet</button>
      <button id="noBtn" class="no">Hayır</button>
    </div>

    <div id="status" class="status"></div>
  </div>

  <div id="confetti" class="confetti-wrap" aria-hidden="true"></div>

  <script>
    const yesBtn = document.getElementById('yesBtn');
    const noBtn = document.getElementById('noBtn');
    const status = document.getElementById('status');
    const confettiWrap = document.getElementById('confetti');
    const apology = document.getElementById('apology');

    function resetStatus(){ status.innerHTML = ''; confettiWrap.innerHTML = ''; }

    function showYes(){
      resetStatus();
      const text = apology.innerText.trim();
      status.innerHTML = `<strong>Teşekkürler bebişş seni cok seviyorum ❤️</strong><div>${escapeHtml(text)}</div>`;
      launchHearts(18);
      yesBtn.disabled = true; noBtn.disabled = true;
      yesBtn.style.opacity = 0.9; noBtn.style.opacity = 0.5;
    }

    function showNo(){
      resetStatus();
      status.innerHTML = `<strong>Üzgünüm prenses</strong><div>Bebiş hayır deme barışalımmm <button id=retry>Tekrar Dene</button></div>`;
      document.getElementById('retry').addEventListener('click', ()=>{ resetStatus(); apology.focus(); });
    }

    function escapeHtml(unsafe){
      return unsafe.replace(/[&<>\"']/g, function(m){ return ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":"&#039;"})[m]; });
    }

    yesBtn.addEventListener('click', showYes);
    noBtn.addEventListener('click', showNo);

    function launchHearts(n){
      for(let i=0;i<n;i++){
        const heart = document.createElement('div');
        heart.className = 'heart';
        heart.style.left = (20 + Math.random()*60) + '%';
        heart.style.top = (-10 - Math.random()*10) + 'vh';
        heart.style.transform = `translateY(0) rotate(${Math.random()*360}deg)`;
        heart.style.animationDelay = (i*60) + 'ms';
        confettiWrap.appendChild(heart);
      }
      setTimeout(()=>{ confettiWrap.innerHTML = ''; }, 3000 + n*60);
    }

    apology.addEventListener('keydown', (e)=>{
      if(e.key === 'Enter' && (e.ctrlKey || e.metaKey)){
        e.preventDefault(); showYes();
      }
    });

    apology.setAttribute('role','textbox');
    apology.setAttribute('aria-multiline','true');
  </script>
</body>
</html>
