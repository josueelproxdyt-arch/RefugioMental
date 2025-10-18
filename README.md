<!doctype html>
<html lang="es">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Refugio Mental — Especial de Halloween</title>
<style>
  :root{
    --bg1:#07050a;
    --bg2:#121018;
    --accent:#e35b2b;
    --ghost:#f7fbff;
    --muted:#bdb7c7;
    --glass: rgba(255,255,255,0.03);
  }
  html,body{height:100%;margin:0;font-family:Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;}
  body{
    background: radial-gradient(1200px 600px at 20% 20%, rgba(20,18,30,0.8), transparent 10%),
                linear-gradient(180deg,var(--bg1) 0%, var(--bg2) 60%);
    color: #eee;
    overflow:hidden;
  }

  /* header / title */
  header{
    position:fixed;left:0;right:0;top:0;height:84px;display:flex;align-items:center;gap:16px;padding:14px 20px;
    background: linear-gradient(90deg, rgba(0,0,0,0.25), rgba(255,255,255,0.02));
    backdrop-filter: blur(4px);
    z-index:50;
    box-shadow: 0 6px 20px rgba(0,0,0,0.6);
  }
  .logo {
    display:flex;gap:12px;align-items:center;
  }
  .logo .badge{
    width:56px;height:56px;border-radius:10px;background:linear-gradient(180deg,#2b1b2e,#3b2436);
    display:flex;align-items:center;justify-content:center;font-size:28px;box-shadow: 0 4px 12px rgba(0,0,0,0.6);
  }
  .title{
    font-weight:700;font-size:18px;line-height:1;color:#fff;
  }
  .subtitle{font-size:12px;color:var(--muted);margin-top:2px}

  /* main area */
  .stage{
    position: absolute; inset:84px 0 0 0; /* under header */
    display:flex;align-items:stretch;gap:14px;padding:18px;
  }

  /* left: game area */
  .game-wrap{
    flex:1; position:relative; overflow:hidden; border-radius:12px; margin-right:12px;
    background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(0,0,0,0.2));
    border:1px solid rgba(255,255,255,0.03);
  }

  /* cemetery background art */
  .cemetery{
    position:absolute;inset:0;background-image:
      radial-gradient(ellipse at 10% 10%, rgba(255,255,255,0.03), transparent 10%),
      url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="800" height="600"><g fill="none" stroke="%232a1f30" stroke-width="2"><path d="M0 450 C100 420 200 500 400 460 C600 420 700 500 800 460 L800 600 L0 600 Z" /></g></svg>');
    background-size:cover;
    filter: contrast(0.9) saturate(0.6);
  }

  /* foreground ground */
  .ground{
    position:absolute;left:0;right:0;bottom:0;height:22%;
    background: linear-gradient(180deg, rgba(0,0,0,0), rgba(0,0,0,0.6));
    pointer-events:none;
  }

  /* fog */
  .fog{
    position:absolute;left:-20%;right:-20%;top:0;height:100%;opacity:0.16;mix-blend-mode:screen;
    background: radial-gradient(ellipse at 20% 20%, rgba(200,200,210,0.08), transparent 10%),
                radial-gradient(ellipse at 80% 80%, rgba(200,200,210,0.06), transparent 10%);
    animation: drift 18s linear infinite;
    filter: blur(12px);
  }
  @keyframes drift{ from{transform:translateX(-10%)} to{transform:translateX(10%)} }

  /* HUD area on top-right */
  .hud{
    position:absolute;right:12px;top:12px;display:flex;flex-direction:column;gap:10px;z-index:40;
  }
  .card{
    background: var(--glass); padding:10px 12px;border-radius:10px;border:1px solid rgba(255,255,255,0.03);
    min-width:120px; text-align:center;
  }
  .score{font-size:20px;font-weight:700}
  .timer{font-size:16px;color:var(--accent);font-weight:700}

  /* controls */
  .controls{position:absolute;left:12px;top:12px;display:flex;gap:8px;z-index:40}
  .btn{
    padding:8px 10px;border-radius:8px;background:rgba(255,255,255,0.03);border:1px solid rgba(255,255,255,0.03);
    color:#fff;font-weight:600;cursor:pointer;font-size:13px;
  }
  .btn.secondary{background:transparent;border:1px solid rgba(255,255,255,0.06);}

  /* spawn container for ghosts */
  .spawn-area{position:absolute;inset:0;pointer-events:auto;z-index:20}

  .entity{
    position:absolute;user-select:none;cursor:pointer;transform-origin:center center;display:flex;align-items:center;justify-content:center;
    font-size:42px;text-shadow:0 6px 16px rgba(0,0,0,0.6);
    transition:transform 0.12s ease, opacity 0.15s ease;
  }
  .entity.pulse{animation:pop 0.25s ease;}
  @keyframes pop{0%{transform:scale(0.85)}50%{transform:scale(1.08)}100%{transform:scale(1)}}

  /* float animation */
  .float{
    animation: bob 3.5s ease-in-out infinite;
  }
  @keyframes bob{0%{transform:translateY(0)}50%{transform:translateY(-14px)}100%{transform:translateY(0)}}

  /* right column: info + leaderboard */
  aside{
    width:320px;max-width:38%;display:flex;flex-direction:column;gap:12px;
  }
  .panel{
    background: linear-gradient(180deg, rgba(255,255,255,0.02), rgba(0,0,0,0.08));
    padding:12px;border-radius:12px;border:1px solid rgba(255,255,255,0.03);
  }
  h3{margin:4px 0 8px 0;font-size:16px}
  .rules{font-size:13px;color:var(--muted);line-height:1.4}

  /* leaderboard list */
  .scores-list{max-height:360px;overflow:auto;margin-top:8px}
  .score-item{display:flex;justify-content:space-between;padding:8px;border-radius:8px;margin-bottom:6px;background:linear-gradient(180deg, rgba(255,255,255,0.01), transparent)}
  .empty{color:var(--muted);text-align:center;padding:18px}

  /* footer */
  footer{position:fixed;left:0;right:0;bottom:0;padding:8px 12px;text-align:center;color:var(--muted);font-size:12px;z-index:60}

  /* responsive adjustments */
  @media (max-width:900px){
    aside{display:none}
    header{height:68px}
    .stage{padding:10px}
    .entity{font-size:36px}
  }
</style>
</head>
<body>
<header>
  <div class="logo">
    <div class="badge">RM</div>
    <div>
      <div class="title">Refugio Mental — Especial de Halloween</div>
      <div class="subtitle">Caza Fantasmas · 60 segundos · Compite por el Top</div>
    </div>
  </div>

  <div style="flex:1"></div>

  <div style="display:flex;gap:8px;align-items:center">
    <div style="font-size:13px;color:var(--muted);margin-right:6px">Juega desde el enlace — comparte en el servidor</div>
  </div>
</header>

<div class="stage">
  <div class="game-wrap" id="gameWrap" role="application" aria-label="Caza fantasmas">
    <div class="cemetery"></div>
    <div class="fog"></div>

    <div class="controls" aria-hidden="false">
      <button class="btn" id="startBtn">Jugar</button>
      <button class="btn secondary" id="pauseBtn">Pausar</button>
      <button class="btn secondary" id="resetBtn">Reiniciar</button>
    </div>

    <div class="hud">
      <div class="card"><div style="font-size:11px;color:var(--muted)">PUNTOS</div><div class="score" id="score">0</div></div>
      <div class="card"><div style="font-size:11px;color:var(--muted)">TIEMPO</div><div class="timer" id="timer">60s</div></div>
      <div class="card"><div style="font-size:11px;color:var(--muted)">SFX / AMBIENT</div>
        <div style="display:flex;gap:6px;justify-content:center;margin-top:6px">
          <button class="btn secondary" id="toggleSfx">SFX ON</button>
          <button class="btn secondary" id="toggleAmb">AMB ON</button>
        </div>
      </div>
    </div>

    <div class="spawn-area" id="spawnArea" tabindex="0"></div>

    <div class="ground"></div>

    <div style="position:absolute;left:12px;bottom:12px;z-index:60">
      <div class="card" id="finalCard" style="display:none;text-align:left;max-width:300px">
        <div style="font-weight:700;font-size:16px" id="finalTitle">Tiempo</div>
        <div style="margin-top:6px" id="finalScore">Puntos: 0</div>
        <div style="margin-top:10px;display:flex;gap:8px">
          <button class="btn" id="playAgain">Volver a Jugar</button>
          <button class="btn secondary" id="saveScoreBtn">Guardar puntuación</button>
        </div>
      </div>
    </div>
  </div>

  <aside>
    <div class="panel">
      <h3>Cómo jugar</h3>
      <div class="rules">
        • Captura los <strong>fantasmas</strong> (👻) haciendo clic rápido. +10 puntos.<br>
        • Evita las <strong>calabazas malditas</strong> (🎃): restan -15 puntos y enlentecen.  
        <br>• Tienes 60 segundos — intenta entrar al Top 10.<br>
        • La música y efectos son generados en el navegador. Puedes desactivar.
      </div>
    </div>

    <div class="panel">
      <h3>Leaderboard (Top 10)</h3>
      <div class="scores-list" id="scoresList">
        <div class="empty">Aún no hay puntuaciones. ¡Sé el primero!</div>
      </div>
      <div style="display:flex;gap:8px;margin-top:8px">
        <input id="nameInput" placeholder="Tu nombre (max 12)" maxlength="12" style="flex:1;padding:8px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:#fff"/>
        <button class="btn" id="saveBtn">Guardar</button>
      </div>
      <div style="margin-top:8px;font-size:12px;color:var(--muted)">Las puntuaciones se guardan localmente en tu navegador (localStorage).</div>
    </div>

    <div class="panel">
      <h3>Detalles técnicos</h3>
      <div style="font-size:13px;color:var(--muted)">Juego en JavaScript puro · Sin servidores · Fácil hosting (Netlify / GitHub Pages). Compatible con móviles.</div>
    </div>
  </aside>
</div>

<footer>Refugio Mental · Evento de Halloween — ¡Buena suerte cazador! 👻</footer>

<script>
/* Simple spooky ambient + SFX via WebAudio */
class AudioMgr {
  constructor(){
    this.ctx = null;
    this.ambOn = true; this.sfxOn = true;
    this.ambGain = null; this.master = null;
    this.ambOsc = null;
    this.buffers = {};
  }
  init(){
    if(this.ctx) return;
    const C = window.AudioContext || window.webkitAudioContext;
    this.ctx = new C();
    this.master = this.ctx.createGain(); this.master.gain.value = 0.9; this.master.connect(this.ctx.destination);
    this.ambGain = this.ctx.createGain(); this.ambGain.gain.value = 0.05; this.ambGain.connect(this.master);
    this._startAmbience();
  }
  _startAmbience(){
    const ctx = this.ctx;
    if(!ctx) return;
    // create a subtle noise-like ambient with filtered oscillators
    const o1 = ctx.createOscillator(); o1.type='sine'; o1.frequency.value = 60;
    const o2 = ctx.createOscillator(); o2.type='sine'; o2.frequency.value = 90;
    const g1 = ctx.createGain(); g1.gain.value = 0.02;
    const g2 = ctx.createGain(); g2.gain.value = 0.01;
    o1.connect(g1); g1.connect(this.ambGain);
    o2.connect(g2); g2.connect(this.ambGain);
    // LFO for movement
    const lfo = ctx.createOscillator(); lfo.frequency.value = 0.05;
    const lfoGain = ctx.createGain(); lfoGain.gain.value = 0.03;
    lfo.connect(lfoGain); lfoGain.connect(g1.gain);
    o1.start(); o2.start(); lfo.start();
    this.ambOsc = {o1,o2,lfo,g1,g2};
  }
  toggleAmb(v){
    this.ambOn = v;
    if(!this.ctx) this.init();
    if(this.ambGain) this.ambGain.gain.value = v ? 0.05 : 0;
  }
  toggleSfx(v){ this.sfxOn = v; }
  playCapture(){
    if(!this.sfxOn) return;
    const ctx=this.ctx; if(!ctx){this.init();}
    const now = this.ctx.currentTime;
    const o = this.ctx.createOscillator(); o.type='triangle'; o.frequency.setValueAtTime(880, now);
    const g = this.ctx.createGain(); g.gain.value = 0.0001;
    o.connect(g); g.connect(this.master);
    g.gain.exponentialRampToValueAtTime(0.18, now+0.02);
    o.frequency.exponentialRampToValueAtTime(220, now+0.25);
    g.gain.exponentialRampToValueAtTime(0.0001, now+0.4);
    o.start(); o.stop(now+0.45);
  }
  playPenalty(){
    if(!this.sfxOn) return;
    const ctx=this.ctx; if(!ctx){this.init();}
    const now = this.ctx.currentTime;
    const b = this.ctx.createOscillator(); b.type='sawtooth';
    const g = this.ctx.createGain(); g.gain.value = 0.04;
    b.connect(g); g.connect(this.master);
    b.frequency.setValueAtTime(120, now);
    b.frequency.exponentialRampToValueAtTime(40, now+0.25);
    g.gain.setValueAtTime(0.04, now);
    g.gain.exponentialRampToValueAtTime(0.001, now+0.4);
    b.start(); b.stop(now+0.5);
  }
}
const audio = new AudioMgr();

/* Game logic */
const spawnArea = document.getElementById('spawnArea');
const scoreEl = document.getElementById('score');
const timerEl = document.getElementById('timer');
const startBtn = document.getElementById('startBtn');
const pauseBtn = document.getElementById('pauseBtn');
const resetBtn = document.getElementById('resetBtn');
const playAgain = document.getElementById('playAgain');
const finalCard = document.getElementById('finalCard');
const finalScore = document.getElementById('finalScore');
const finalTitle = document.getElementById('finalTitle');
const saveBtn = document.getElementById('saveBtn');
const saveScoreBtn = document.getElementById('saveScoreBtn');
const nameInput = document.getElementById('nameInput');
const scoresList = document.getElementById('scoresList');
const toggleSfx = document.getElementById('toggleSfx');
const toggleAmb = document.getElementById('toggleAmb');

let state = {
  running:false, paused:false, score:0, time:60, interval:null, spawnInterval:null, entities:[], best:[],
  lastSpawnMs:0
};

function randomRange(min,max){ return Math.random()*(max-min)+min; }

function updateHUD(){
  scoreEl.textContent = state.score;
  timerEl.textContent = Math.max(0, Math.ceil(state.time)) + 's';
}

function spawnEntity(){
  const area = spawnArea.getBoundingClientRect();
  const pad = 60;
  const x = Math.floor(randomRange(pad, area.width - pad));
  const y = Math.floor(randomRange(pad, area.height - pad));
  // choose ghost or pumpkin (pumpkin is rarer)
  const isPumpkin = Math.random() < 0.16; // 16% chance pumpkin
  const el = document.createElement('div');
  el.className = 'entity float';
  el.style.left = x + 'px';
  el.style.top = y + 'px';
  el.style.transform = 'translate(-50%,-50%)';
  let lifetime = randomRange(900, 1600);
  // bigger ghosts for more points sometimes
  if(!isPumpkin){
    el.dataset.type='ghost';
    el.innerHTML = '<span style="font-size:1.02em">👻</span>';
    el.title = 'Fantasma';
    el.dataset.points = 10;
  } else {
    el.dataset.type='pumpkin';
    el.innerHTML = '<span style="font-size:1.02em">🎃</span>';
    el.title = 'Calabaza maldita';
    el.dataset.points = -15;
    lifetime = randomRange(700, 1200);
  }
  el.style.opacity = '0';
  spawnArea.appendChild(el);
  state.entities.push(el);

  // entry animation
  requestAnimationFrame(()=>{ el.style.opacity='1'; el.classList.add('pulse'); });
  // auto remove after lifetime
  const id = setTimeout(()=>{ removeEntity(el); }, lifetime);
  el.dataset._timeout = id;

  // clicking behavior
  el.addEventListener('click', (ev)=>{
    if(!state.running || state.paused) return;
    ev.stopPropagation();
    const t = el.dataset.type;
    const pts = parseInt(el.dataset.points||0,10);
    if(t === 'ghost'){
      // scoring
      state.score += pts;
      audio.playCapture();
      // small pop animation
      el.style.transition='transform 0.12s ease, opacity 0.18s ease';
      el.style.transform += ' scale(1.18)';
      el.style.opacity = '0';
      clearTimeout(el.dataset._timeout);
      setTimeout(()=>{ removeEntity(el); }, 160);
    } else {
      // penalty
      state.score += pts;
      audio.playPenalty();
      // visual negative feedback
      el.style.transition='transform 0.12s ease, opacity 0.2s ease';
      el.style.transform += ' rotate(-18deg) translateY(12px) scale(0.95)';
      el.style.opacity='0';
      clearTimeout(el.dataset._timeout);
      setTimeout(()=>{ removeEntity(el); },180);
      // small slowdown
      state.time -= 1.8;
    }
    updateHUD();
  });
}

function removeEntity(el){
  if(!el || !el.parentNode) return;
  try{
    clearTimeout(el.dataset._timeout);
  }catch(e){}
  const idx = state.entities.indexOf(el);
  if(idx>=0) state.entities.splice(idx,1);
  el.remove();
}

function clearEntities(){
  while(state.entities.length) removeEntity(state.entities[0]);
}

function startGame(){
  if(state.running){ resetGame(); }
  audio.init();
  audio.toggleAmb(true);
  state.running = true; state.paused=false; state.score=0; state.time=60;
  updateHUD();
  finalCard.style.display='none';
  // spawn frequently at first, then adapt
  state.spawnInterval = setInterval(()=>{
    // spawn 1-3 entities per tick with some randomness
    const n = Math.random() < 0.25 ? 2 : 1;
    for(let i=0;i<n;i++) spawnEntity();
    // occasionally spawn a quick burst
    if(Math.random() < 0.06) for(let j=0;j<2;j++) spawnEntity();
  }, 850);
  // game tick
  state.interval = setInterval(()=>{
    if(state.paused) return;
    state.time -= 0.2;
    // lightly decay entities if too many
    if(state.entities.length > 18){
      removeEntity(state.entities[0]);
    }
    if(state.time <= 0){
      endGame();
    }
    updateHUD();
  }, 200);
  startBtn.textContent = 'En Juego';
  startBtn.disabled = true;
  pauseBtn.disabled = false;
}

function pauseGame(){
  if(!state.running) return;
  state.paused = !state.paused;
  pauseBtn.textContent = state.paused ? 'Continuar' : 'Pausar';
  // freeze/resume ambient optionally
}

function endGame(){
  state.running = false;
  startBtn.disabled = false;
  pauseBtn.disabled = true;
  pauseBtn.textContent = 'Pausar';
  clearInterval(state.interval); clearInterval(state.spawnInterval);
  // remove leftover entities slowly
  finalTitle.textContent = '¡Tiempo!';
  finalScore.textContent = 'Puntos: ' + state.score;
  finalCard.style.display='block';
  showFinalCard();
  saveLocalScorePrompt(state.score);
}

function resetGame(){
  clearInterval(state.interval); clearInterval(state.spawnInterval);
  clearEntities();
  state.running=false; state.paused=false; state.score=0; state.time=60;
  updateHUD();
  startBtn.disabled=false;
  pauseBtn.disabled=true;
  finalCard.style.display='none';
}

function showFinalCard(){
  finalCard.style.display='block';
}

/* Leaderboard localStorage */
const KEY = 'rm_halloween_scores_v1';
function loadScores(){
  try{
    const raw = localStorage.getItem(KEY);
    if(!raw) return [];
    const arr = JSON.parse(raw);
    if(!Array.isArray(arr)) return [];
    return arr;
  }catch(e){ return []; }
}
function saveScores(arr){
  localStorage.setItem(KEY, JSON.stringify(arr));
  renderScores();
}
function addScore(name, score){
  const arr = loadScores();
  arr.push({name: name || 'Anon', score: Math.round(score), date: Date.now()});
  arr.sort((a,b)=>b.score-a.score);
  const trimmed = arr.slice(0,20); // keep top 20 locally
  saveScores(trimmed);
}
function renderScores(){
  const arr = loadScores();
  if(!arr.length){ scoresList.innerHTML = '<div class="empty">Aún no hay puntuaciones. ¡Sé el primero!</div>'; return; }
  const html = arr.slice(0,10).map((s,idx)=>{
    const d = new Date(s.date);
    const dStr = d.toLocaleDateString();
    return `<div class="score-item"><div>${idx+1}. ${escapeHtml(s.name)}</div><div>${s.score}</div></div>`;
  }).join('');
  scoresList.innerHTML = html;
}

/* basic helper */
function escapeHtml(str){ return String(str).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;'); }

/* UI wiring */
startBtn.addEventListener('click', ()=>{ startGame(); });
pauseBtn.addEventListener('click', ()=>{ pauseGame(); });
resetBtn.addEventListener('click', ()=>{ resetGame(); });

playAgain.addEventListener('click', ()=>{ resetGame(); startGame(); });
saveBtn.addEventListener('click', ()=>{
  const name = nameInput.value.trim() || 'Anon';
  addScore(name, state.score);
  nameInput.value='';
});
saveScoreBtn.addEventListener('click', ()=>{
  const name = nameInput.value.trim() || prompt('Nombre para leaderboard (max 12):','Jugador');
  if(!name) return;
  addScore(name.slice(0,12), state.score);
  nameInput.value='';
});

toggleSfx.addEventListener('click', ()=>{
  audio.init();
  audio.toggleSfx(!audio.sfxOn);
  toggleSfx.textContent = audio.sfxOn ? 'SFX ON' : 'SFX OFF';
});
toggleAmb.addEventListener('click', ()=>{
  audio.init();
  audio.toggleAmb(!audio.ambOn);
  toggleAmb.textContent = audio.ambOn ? 'AMB ON' : 'AMB OFF';
});

/* click background to create decoy entity sometimes (encourages movement) */
spawnArea.addEventListener('click', (e)=>{
  if(!state.running || state.paused) return;
  // small chance to spawn a decoy where clicked
  if(Math.random() < 0.38) {
    const area = spawnArea.getBoundingClientRect();
    const x = e.clientX - area.left;
    const y = e.clientY - area.top;
    const el = document.createElement('div');
    el.className = 'entity float';
    el.style.left = x + 'px'; el.style.top = y + 'px'; el.style.transform='translate(-50%,-50%)';
    el.innerHTML = Math.random() < 0.6 ? '👻' : '🎃';
    el.style.opacity='0';
    spawnArea.appendChild(el);
    state.entities.push(el);
    requestAnimationFrame(()=>{ el.style.opacity='1'; el.classList.add('pulse'); });
    const lifetime = 800 + Math.random()*800;
    const id = setTimeout(()=>{ removeEntity(el); }, lifetime);
    el.dataset._timeout = id;
    el.addEventListener('click', (ev)=>{
      ev.stopPropagation();
      const isPump = el.textContent.includes('🎃');
      if(isPump){ state.score -= 15; audio.playPenalty(); state.time -= 1.5; }
      else { state.score += 10; audio.playCapture(); }
      updateHUD();
      removeEntity(el);
    });
  }
});

/* saving on enter in name field */
nameInput.addEventListener('keydown', (e)=>{ if(e.key === 'Enter'){ saveBtn.click(); } });

/* on load show leaderboard */
renderScores();
updateHUD();
pauseBtn.disabled = true;

/* small helper: save prompt after game ends */
function saveLocalScorePrompt(score){
  // show a small animation to prompt saving
  setTimeout(()=>{ finalCard.style.display='block'; }, 220);
}

/* make spawn area accessible on resize */
window.addEventListener('resize', ()=>{ /* no-op for now */ });

/* Prevent accidental text selection on mobile double-tap */
spawnArea.addEventListener('touchstart', (e)=>{ e.preventDefault(); }, {passive:false});

</script>
</body>
</html>
