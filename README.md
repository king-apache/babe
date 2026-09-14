<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>for you</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Fraunces:ital,wght@0,300;0,500;0,600;1,400;1,500&family=Quicksand:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --ink:#2B2420;
    --paper:#FBF6EF;
    --paper-soft:#F3ECE1;
  }
  *{box-sizing:border-box; -webkit-tap-highlight-color:transparent;}
  html,body{margin:0;padding:0;}
  body{
    font-family:'Quicksand',sans-serif;
    color:var(--ink);
    background:var(--paper);
    overflow-x:hidden;
    scroll-behavior:smooth;
  }
  h1,h2,.poem{font-family:'Fraunces',serif;}
  @media (prefers-reduced-motion: reduce){
    *{animation-duration:0.001ms !important; animation-iteration-count:1 !important; transition-duration:0.001ms !important;}
  }

  /* ---------- COVER ---------- */
  #cover{
    position:fixed; inset:0; z-index:100;
    display:flex; align-items:center; justify-content:center;
    background:radial-gradient(circle at 50% 30%, #4a3350 0%, #2b1e33 60%, #1c1420 100%);
    transition:transform 1s cubic-bezier(.65,0,.35,1), opacity .9s ease;
  }
  #cover.opened{ transform:translateY(-100%); opacity:0; pointer-events:none; }
  .envelope-wrap{ text-align:center; color:#f3e9f0; padding:20px; }
  .envelope{
    width:150px; height:105px; margin:0 auto 28px;
    position:relative; cursor:pointer;
    filter:drop-shadow(0 12px 24px rgba(0,0,0,.35));
    animation:floatY 3.2s ease-in-out infinite;
  }
  @keyframes floatY{ 0%,100%{transform:translateY(0)} 50%{transform:translateY(-10px)} }
  .envelope .body{
    position:absolute; inset:0; background:#F3E4E9; border-radius:6px;
  }
  .envelope .flap{
    position:absolute; top:0; left:0; width:0; height:0;
    border-left:75px solid transparent; border-right:75px solid transparent;
    border-top:58px solid #E7C6D2; transform-origin:top; transition:transform .5s ease;
  }
  #cover.opening .envelope .flap{ transform:rotateX(180deg); }
  .envelope .heart{
    position:absolute; left:50%; top:55%; transform:translate(-50%,-50%);
    font-size:22px; opacity:.9;
  }
  .tap-hint{ font-size:.85rem; letter-spacing:.04em; opacity:.75; margin-top:6px; }
  .cover-title{ font-family:'Fraunces',serif; font-style:italic; font-weight:400; font-size:1.5rem; margin:0 0 4px; }

  /* ---------- STORY / NAV ---------- */
  #story{ display:none; }
  #story.show{ display:block; }
  .dots{
    position:fixed; right:10px; top:50%; transform:translateY(-50%); z-index:50;
    display:flex; flex-direction:column; gap:9px;
  }
  .dots span{
    width:7px; height:7px; border-radius:50%; background:rgba(43,36,32,.25);
    transition:background .3s, transform .3s;
  }
  .dots span.active{ background:var(--ink); transform:scale(1.4); }

  section.chapter{
    min-height:100svh; width:100%; position:relative; overflow:hidden;
    display:flex; flex-direction:column; align-items:center; justify-content:center;
    padding:64px 22px; text-align:center;
  }
  .eyebrow-badge{
    font-size:.72rem; font-weight:600; letter-spacing:.03em;
    background:rgba(255,255,255,.55); padding:5px 14px; border-radius:20px;
    margin-bottom:18px; backdrop-filter:blur(4px);
  }
  .chapter h2{ font-weight:500; font-size:2rem; line-height:1.15; margin:0 0 18px; max-width:320px; }
  .postcard{
    background:var(--paper); border-radius:14px; padding:20px 22px;
    max-width:320px; margin:22px auto 0; box-shadow:0 10px 30px rgba(0,0,0,.12);
    position:relative;
  }
  .postcard::before{
    content:''; position:absolute; top:10px; right:12px; width:26px; height:26px;
    border:1.5px dashed rgba(0,0,0,.18); border-radius:4px;
  }
  .poem{ font-size:1.05rem; font-style:italic; line-height:1.7; margin:0; white-space:pre-line; }
  .interact-hint{ font-size:.78rem; opacity:.6; margin-top:14px; }

  /* ================= GARDEN ================= */
  .garden{ background:linear-gradient(180deg,#EAF3E1 0%,#DCEAD0 65%,#CFE3C1 100%); }
  .garden-field{ position:relative; width:100%; max-width:340px; height:130px; margin-top:10px; }
  .flower{
    position:absolute; bottom:0; width:34px; height:34px; cursor:pointer;
    transform:translateY(0) scale(1); transition:transform .35s cubic-bezier(.34,1.56,.64,1);
  }
  .flower svg{ width:100%; height:100%; overflow:visible; }
  .flower .petals{ transform-origin:center; transition:transform .4s ease; }
  .flower.bloom .petals{ transform:scale(1.35); }
  .flower.bloom{ transform:translateY(-6px); }
  .flower .burst{ opacity:0; }
  .flower.bloom .burst{ animation:burst .7s ease forwards; }
  @keyframes burst{
    0%{ opacity:1; transform:scale(.3) translateY(0);} 100%{ opacity:0; transform:scale(1.1) translateY(-24px);}
  }

  /* ================= NIGHT SKY ================= */
  .night{ background:radial-gradient(ellipse at 50% 0%,#262a5e 0%,#14163B 55%,#0d0e28 100%); color:#F5EFE0; }
  .night .eyebrow-badge{ background:rgba(255,255,255,.12); color:#F2C879; }
  .sky{ position:relative; width:100%; max-width:340px; height:150px; margin-top:8px; }
  .star{
    position:absolute; width:14px; height:14px; cursor:pointer;
    background:#F2C879; clip-path:polygon(50% 0%,63% 38%,100% 38%,69% 60%,82% 100%,50% 76%,18% 100%,31% 60%,0% 38%,37% 38%);
    opacity:.35; transition:opacity .3s, transform .3s;
  }
  .star.lit{ opacity:1; transform:scale(1.3); filter:drop-shadow(0 0 6px #F2C879); }
  .night .postcard{ background:#1F2450; color:#F5EFE0; }
  .night .postcard::before{ border-color:rgba(255,255,255,.2); }

  /* ================= CATS ================= */
  .cats{ background:linear-gradient(180deg,#FFF3E4 0%,#FFE9CE 100%); }
  .cat-wrap{ cursor:pointer; width:160px; height:150px; margin-top:6px; position:relative; }
  .purr{
    position:absolute; top:-6px; left:50%; transform:translateX(-50%);
    font-size:.75rem; opacity:0; font-weight:600; color:#D98B5F;
  }
  .cat-wrap.petting .purr{ animation:floatUp 1s ease forwards; }
  @keyframes floatUp{ 0%{opacity:0; transform:translate(-50%,0);} 30%{opacity:1;} 100%{opacity:0; transform:translate(-50%,-26px);} }
  .cat-wrap svg{ width:100%; height:100%; }
  .cat-tail{ transform-origin:80% 90%; animation:tailwag 2.6s ease-in-out infinite; }
  @keyframes tailwag{ 0%,100%{transform:rotate(0deg)} 50%{transform:rotate(12deg)} }
  .cat-eye{ transition:transform .15s; }
  .cat-wrap.petting .cat-eye{ transform:scaleY(.15); }

  /* ================= STRAWBERRIES ================= */
  .berries{ background:linear-gradient(180deg,#FFE3E8 0%,#FFD3DC 100%); }
  .patch{ display:flex; gap:14px; margin-top:10px; flex-wrap:wrap; justify-content:center; max-width:320px; }
  .berry{ width:46px; height:46px; cursor:pointer; transition:transform .25s; }
  .berry.picked{ transform:scale(0) rotate(30deg); }
  .basket-count{ font-weight:700; margin-top:16px; font-size:.9rem; }

  /* ================= GHIBLI ================= */
  .ghibli{ background:linear-gradient(180deg,#CDE7F0 0%,#DCEFD8 55%,#F5EFD9 100%); position:relative; }
  .cloud{ position:absolute; background:#fff; border-radius:50px; opacity:.9; filter:blur(.3px); }
  .cloud1{ width:90px; height:30px; top:14%; left:8%; animation:drift 26s linear infinite; }
  .cloud2{ width:60px; height:22px; top:22%; right:10%; animation:drift 34s linear infinite reverse; }
  .cloud3{ width:70px; height:24px; top:8%; left:50%; animation:drift 40s linear infinite; }
  @keyframes drift{ 0%{ transform:translateX(0);} 50%{transform:translateX(24px);} 100%{transform:translateX(0);} }
  .spirit{ width:92px; margin:8px auto 0; animation:bob 3s ease-in-out infinite; }
  @keyframes bob{ 0%,100%{transform:translateY(0)} 50%{transform:translateY(-12px)} }

  /* ================= Y2K ================= */
  .y2k{ background:linear-gradient(160deg,#FFD6F0 0%,#C9F2FF 100%); position:relative; }
  .sticker{ position:absolute; font-size:1.4rem; animation:spin 6s linear infinite; }
  .sticker.s2{ animation-duration:9s; animation-direction:reverse; }
  @keyframes spin{ from{transform:rotate(0)} to{transform:rotate(360deg)} }
  .bubble-title{
    font-family:'Fraunces',serif; font-weight:600; font-size:2.1rem;
    -webkit-text-stroke:1.5px var(--ink); color:#FF5FA2;
    text-shadow:3px 3px 0 #5FD4FF;
  }
  .y2k .postcard{ border:2px dashed #FF5FA2; }

  /* ================= PETS ================= */
  .pets{ background:linear-gradient(180deg,#FBEFDD 0%,#F6E4CB 100%); }
  .pet-row{ display:flex; gap:26px; margin-top:6px; }
  .pet{ width:110px; cursor:pointer; }
  .pet svg{ width:100%; }
  .pet-tail{ transform-origin:15% 85%; animation:wag 1.1s ease-in-out infinite; }
  @keyframes wag{ 0%,100%{transform:rotate(-8deg)} 50%{transform:rotate(18deg)} }
  .pet-name{ font-weight:700; margin-top:6px; font-size:.9rem; }

  /* ================= JOKE ================= */
  .joke{ background:linear-gradient(135deg,#FFE1EC 0%,#E7DBFF 40%,#CFF3FF 70%,#FFF4CF 100%); }
  .badge{
    width:120px; height:120px; border-radius:50%; margin:6px auto 0;
    background:conic-gradient(from 0deg,#FF5FA2,#FFD65F,#5FD4FF,#8CE99A,#FF5FA2);
    display:flex; align-items:center; justify-content:center; cursor:pointer;
    box-shadow:0 8px 22px rgba(0,0,0,.15);
    transition:transform .3s;
  }
  .badge:active{ transform:scale(.93) rotate(8deg); }
  .badge-inner{
    width:96px; height:96px; border-radius:50%; background:var(--paper);
    display:flex; align-items:center; justify-content:center; text-align:center;
    font-size:.62rem; font-weight:700; line-height:1.3; padding:6px;
  }
  .speech{
    max-width:280px; margin:18px auto 0; background:#fff; border-radius:16px;
    padding:12px 16px; font-size:.88rem; font-weight:600; opacity:0; transform:translateY(8px) scale(.95);
    transition:opacity .35s, transform .35s; box-shadow:0 6px 18px rgba(0,0,0,.1);
  }
  .speech.show{ opacity:1; transform:translateY(0) scale(1); }

  /* ================= CLOSING ================= */
  .closing{ background:linear-gradient(180deg,#3A1F2B 0%,#2A1620 100%); color:#F3E6DE; }
  .closing .postcard{ background:#4a2c3a; color:#F3E6DE; }
  .closing .postcard::before{ border-color:rgba(255,255,255,.25); }
  .ring-btn{
    margin-top:26px; border:none; padding:13px 26px; border-radius:30px;
    background:#E8B4BC; color:#3A1F2B; font-family:'Quicksand',sans-serif; font-weight:700;
    font-size:.95rem; cursor:pointer; box-shadow:0 8px 20px rgba(0,0,0,.3);
  }
  .heart-piece{
    position:fixed; top:0; font-size:1.2rem; pointer-events:none; z-index:200;
    animation:fall linear forwards;
  }
  @keyframes fall{ to{ transform:translateY(110vh) rotate(360deg); opacity:0; } }
  .signature{ font-family:'Fraunces',serif; font-style:italic; margin-top:30px; font-size:1rem; opacity:.85; }
</style>
</head>
<body>

<div id="cover">
  <div class="envelope-wrap">
    <p class="cover-title">a small world,</p>
    <p class="cover-title" style="margin-bottom:26px;">made for you</p>
    <div class="envelope" id="envelope">
      <div class="body"></div>
      <div class="flap"></div>
      <div class="heart">💌</div>
    </div>
    <p class="tap-hint">tap the envelope</p>
  </div>
</div>

<div id="story">
  <nav class="dots" id="dots"></nav>

  <!-- GARDEN -->
  <section class="chapter garden" data-name="garden">
    <span class="eyebrow-badge">chapter one · quietly growing</span>
    <h2>Things that grow slow<br>grow the strongest roots.</h2>
    <div class="garden-field" id="gardenField"></div>
    <div class="postcard">
      <p class="poem">You didn't arrive like weather —
loud, sudden, gone by morning.
You arrived like spring does,
one small proof at a time,
until the whole field was you.</p>
    </div>
    <p class="interact-hint">tap the flowers to watch them bloom 🌱</p>
  </section>

  <!-- NIGHT SKY -->
  <section class="chapter night" data-name="night">
    <span class="eyebrow-badge">chapter two · you, at 2am</span>
    <h2>Even on my worst nights,<br>you're the constant one.</h2>
    <div class="sky" id="skyField"></div>
    <div class="postcard">
      <p class="poem">I've mapped a hundred skies
looking for something steady —
turns out I just needed
to look at you instead.</p>
    </div>
    <p class="interact-hint">tap a few stars ✨</p>
  </section>

  <!-- CATS -->
  <section class="chapter cats" data-name="cats">
    <span class="eyebrow-badge">chapter three · soft things</span>
    <h2>You have main-character energy<br>and a house-cat heart.</h2>
    <div class="cat-wrap" id="catWrap">
      <span class="purr">purrrr~</span>
      <svg viewBox="0 0 160 150">
        <ellipse cx="80" cy="105" rx="46" ry="34" fill="#E8AE7A"/>
        <g class="cat-tail"><path d="M118 110 Q 150 100 140 60" stroke="#E8AE7A" stroke-width="12" fill="none" stroke-linecap="round"/></g>
        <circle cx="80" cy="58" r="38" fill="#EDBE8E"/>
        <path d="M52 34 L46 8 L66 30 Z" fill="#EDBE8E"/>
        <path d="M108 34 L114 8 L94 30 Z" fill="#EDBE8E"/>
        <path d="M55 36 L50 16 L64 32 Z" fill="#D98B5F"/>
        <path d="M105 36 L110 16 L96 32 Z" fill="#D98B5F"/>
        <ellipse class="cat-eye" cx="66" cy="58" rx="5" ry="7" fill="#2B2420"/>
        <ellipse class="cat-eye" cx="94" cy="58" rx="5" ry="7" fill="#2B2420"/>
        <path d="M78 68 Q80 72 82 68" stroke="#2B2420" stroke-width="2" fill="none" stroke-linecap="round"/>
        <path d="M60 70 Q50 68 40 72" stroke="#c98a55" stroke-width="1.5" fill="none"/>
        <path d="M60 74 Q50 74 40 78" stroke="#c98a55" stroke-width="1.5" fill="none"/>
        <path d="M100 70 Q110 68 120 72" stroke="#c98a55" stroke-width="1.5" fill="none"/>
        <path d="M100 74 Q110 74 120 78" stroke="#c98a55" stroke-width="1.5" fill="none"/>
      </svg>
    </div>
    <div class="postcard">
      <p class="poem">You're soft with everyone,
but you're softest with me
when no one else is watching —
that's the part I fell for.</p>
    </div>
    <p class="interact-hint">tap to pet the cat 🐾</p>
  </section>

  <!-- STRAWBERRIES -->
  <section class="chapter berries" data-name="berries">
    <span class="eyebrow-badge">chapter four · sweetness</span>
    <h2>Loving you is the easiest<br>sweet thing I do all day.</h2>
    <div class="patch" id="berryPatch"></div>
    <p class="basket-count" id="berryCount">basket: 0 🧺</p>
    <div class="postcard">
      <p class="poem">Some things are sweet in a rush,
gone before you taste them right.
You are sweet the slow way —
the kind I get to keep.</p>
    </div>
    <p class="interact-hint">pick a few strawberries 🍓</p>
  </section>

  <!-- GHIBLI -->
  <section class="chapter ghibli" data-name="ghibli">
    <div class="cloud cloud1"></div><div class="cloud cloud2"></div><div class="cloud cloud3"></div>
    <span class="eyebrow-badge">chapter five · a soft world</span>
    <h2>With you, ordinary days<br>feel like they were animated.</h2>
    <svg class="spirit" viewBox="0 0 100 110">
      <ellipse cx="50" cy="70" rx="36" ry="34" fill="#8FB68A"/>
      <ellipse cx="50" cy="40" rx="26" ry="24" fill="#A8CBA2"/>
      <circle cx="41" cy="38" r="4" fill="#2B2420"/>
      <circle cx="59" cy="38" r="4" fill="#2B2420"/>
      <ellipse cx="35" cy="75" rx="7" ry="5" fill="#fff" opacity=".8"/>
      <ellipse cx="65" cy="75" rx="7" ry="5" fill="#fff" opacity=".8"/>
      <path d="M46 47 Q50 51 54 47" stroke="#2B2420" stroke-width="1.8" fill="none" stroke-linecap="round"/>
    </svg>
    <div class="postcard">
      <p class="poem">Take my hand — the field's this way.
No plot, no rush, no plan.
Just wind, and you, and quiet,
and the good kind of small.</p>
    </div>
  </section>

  <!-- Y2K -->
  <section class="chapter y2k" data-name="y2k">
    <span class="sticker s1" style="top:14%; left:12%;">⭐</span>
    <span class="sticker s2" style="top:18%; right:10%;">💿</span>
    <span class="sticker s1" style="bottom:16%; left:16%;">💗</span>
    <span class="sticker s2" style="bottom:20%; right:14%;">✦</span>
    <span class="eyebrow-badge">chapter six · main character</span>
    <p class="bubble-title">u r so my<br>person fr</p>
    <div class="postcard">
      <p class="poem">no cap, no notes app draft,
just: you make my whole week better,
and I'd pick you on purpose
in literally any decade.(also i love being your court jester my princess)</p>
    </div>
  </section>

  <!-- PETS -->
  <section class="chapter pets" data-name="pets">
    <span class="eyebrow-badge">chapter seven · the whole family</span>
    <h2>Two dogs, two humans,<br>one very full heart.</h2>
    <div class="pet-row">
      <div class="pet" id="macDog">
        <svg viewBox="0 0 120 120">
          <g class="pet-tail"><path d="M20 90 Q4 70 14 50" stroke="#C99154" stroke-width="10" fill="none" stroke-linecap="round"/></g>
          <ellipse cx="65" cy="85" rx="42" ry="28" fill="#C99154"/>
          <circle cx="65" cy="48" r="30" fill="#D9A567"/>
          <path d="M42 34 Q34 55 44 62" stroke="#B87F41" stroke-width="10" fill="none" stroke-linecap="round"/>
          <path d="M88 34 Q96 55 86 62" stroke="#B87F41" stroke-width="10" fill="none" stroke-linecap="round"/>
          <circle cx="56" cy="46" r="3.4" fill="#2B2420"/>
          <circle cx="74" cy="46" r="3.4" fill="#2B2420"/>
          <ellipse cx="65" cy="58" rx="5" ry="4" fill="#2B2420"/>
          <path d="M65 62 Q65 68 58 68 M65 62 Q65 68 72 68" stroke="#2B2420" stroke-width="1.6" fill="none" stroke-linecap="round"/>
        </svg>
        <p class="pet-name">Mackie 🐕</p>
      </div>
      <div class="pet" id="dodoDog">
        <svg viewBox="0 0 120 120">
          <g class="pet-tail" style="transform-origin:85% 85%;"><path d="M96 88 Q112 68 100 46" stroke="#F2EFE7" stroke-width="12" fill="none" stroke-linecap="round"/></g>
          <ellipse cx="55" cy="85" rx="42" ry="28" fill="#F5F2EA"/>
          <circle cx="55" cy="46" r="30" fill="#FBFAF5"/>
          <path d="M32 34 Q40 20 48 32" fill="#FBFAF5"/>
          <path d="M78 34 Q70 20 62 32" fill="#FBFAF5"/>
          <circle cx="47" cy="44" r="3.4" fill="#2B2420"/>
          <circle cx="65" cy="44" r="3.4" fill="#2B2420"/>
          <ellipse cx="56" cy="56" rx="4.5" ry="3.6" fill="#2B2420"/>
          <path d="M56 60 Q56 66 49 66 M56 60 Q56 66 63 66" stroke="#2B2420" stroke-width="1.6" fill="none" stroke-linecap="round"/>
        </svg>
        <p class="pet-name">Dodo 🐾</p>
      </div>
    </div>
    <div class="postcard">
      <p class="poem">Im sorry Mackie doesnt give a fuck but he is possessive and wants you for kisses and food and i dont blame him.
</p>
    </div>
    <p class="interact-hint">tap them to say hi</p>
  </section>

  <!-- JOKE -->
  <section class="chapter joke" data-name="joke">
    <span class="eyebrow-badge">chapter eight · a very serious award</span>
    <h2>For services rendered<br>in the voice department.</h2>
    <div class="badge" id="badge">
      <div class="badge-inner">certified<br>twink™<br></div>
    </div>
    <div class="speech" id="badgeSpeech">i can do gay sassy voice forever(please dont abuse this statement) 🏳️‍🌈🎤</div>
    <div class="postcard">
      <p class="poem">You asked once, for a bit,
and I never really stopped —
some roles you just get typecast in,
and this one I don't mind at all.</p>
    </div>
    <p class="interact-hint">tap the badge</p>
  </section>

  <!-- CLOSING -->
  <section class="chapter closing" data-name="closing">
    <span class="eyebrow-badge" style="background:rgba(255,255,255,.12); color:#E8B4BC;">last page · for now</span>
    <h2>Every version of my future<br>has you already in it.</h2>
    <div class="postcard">
      <p class="poem">Thank you for the mornings,
the arguments we outgrew,
the dogs, the voices, the silliness,
and every ordinary Tuesday
that somehow felt like enough.

I love you. Still. Again. Always.</p>
    </div>
    <button class="ring-btn" id="loveBtn">tap this too</button>
    <p class="signature">— yours, always</p>
  </section>

</div>

<script>
(function(){
  const cover = document.getElementById('cover');
  const envelope = document.getElementById('envelope');
  const story = document.getElementById('story');

  envelope.addEventListener('click', () => {
    cover.classList.add('opening');
    setTimeout(() => {
      cover.classList.add('opened');
      story.classList.add('show');
      initDots();
    }, 550);
  });

  // ---- dots nav ----
  function initDots(){
    const chapters = document.querySelectorAll('.chapter');
    const dots = document.getElementById('dots');
    chapters.forEach((ch,i) => {
      const d = document.createElement('span');
      d.addEventListener('click', () => ch.scrollIntoView({behavior:'smooth'}));
      dots.appendChild(d);
    });
    const dotEls = dots.querySelectorAll('span');
    const obs = new IntersectionObserver((entries) => {
      entries.forEach(e => {
        const idx = Array.from(chapters).indexOf(e.target);
        if(e.isIntersecting){
          dotEls.forEach(d=>d.classList.remove('active'));
          dotEls[idx].classList.add('active');
        }
      });
    }, {threshold:0.55});
    chapters.forEach(ch => obs.observe(ch));
  }

  // ---- garden flowers ----
  const colors = ['#E88CA6','#F2B3C4','#F6D2A6','#C9A6E0'];
  const gardenField = document.getElementById('gardenField');
  for(let i=0;i<7;i++){
    const f = document.createElement('div');
    f.className = 'flower';
    f.style.left = (i*44 + (i%2?6:0)) + 'px';
    const c = colors[i % colors.length];
    f.innerHTML = `<svg viewBox="0 0 40 40">
      <g class="petals">
        <circle cx="20" cy="10" r="7" fill="${c}"/>
        <circle cx="20" cy="30" r="7" fill="${c}"/>
        <circle cx="10" cy="20" r="7" fill="${c}"/>
        <circle cx="30" cy="20" r="7" fill="${c}"/>
        <circle cx="20" cy="20" r="6" fill="#F6E27A"/>
      </g>
      <text class="burst" x="20" y="6" font-size="12" text-anchor="middle">💗</text>
    </svg>`;
    f.addEventListener('click', () => {
      f.classList.remove('bloom'); void f.offsetWidth; f.classList.add('bloom');
    });
    gardenField.appendChild(f);
  }

  // ---- night sky stars ----
  const sky = document.getElementById('skyField');
  const starPos = [[10,20],[60,10],[110,35],[160,5],[210,28],[260,12],[300,40],[40,60],[140,70],[230,65]];
  starPos.forEach(([x,y]) => {
    const s = document.createElement('div');
    s.className = 'star';
    s.style.left = x+'px'; s.style.top = y+'px';
    s.addEventListener('click', () => s.classList.toggle('lit'));
    sky.appendChild(s);
  });

  // ---- cat petting ----
  const cat = document.getElementById('catWrap');
  cat.addEventListener('click', () => {
    cat.classList.remove('petting'); void cat.offsetWidth; cat.classList.add('petting');
  });

  // ---- strawberries ----
  const patch = document.getElementById('berryPatch');
  const countEl = document.getElementById('berryCount');
  let picked = 0;
  for(let i=0;i<6;i++){
    const b = document.createElement('div');
    b.className = 'berry';
    b.innerHTML = `<svg viewBox="0 0 46 46">
      <path d="M14 14 Q23 6 32 14 L23 6Z" fill="#5C8A57"/>
      <path d="M23 16 C10 16 6 30 23 42 C40 30 36 16 23 16Z" fill="#E4536B"/>
      <circle cx="17" cy="24" r="1.4" fill="#FFE8A3"/>
      <circle cx="27" cy="22" r="1.4" fill="#FFE8A3"/>
      <circle cx="21" cy="32" r="1.4" fill="#FFE8A3"/>
      <circle cx="29" cy="30" r="1.4" fill="#FFE8A3"/>
    </svg>`;
    b.addEventListener('click', () => {
      if(b.classList.contains('picked')) return;
      b.classList.add('picked');
      picked++;
      countEl.textContent = 'basket: ' + picked + ' 🧺';
    });
    patch.appendChild(b);
  }

  // ---- dog nudge ----
  ['macDog','dodoDog'].forEach(id => {
    const el = document.getElementById(id);
    el.addEventListener('click', () => {
      el.animate([{transform:'scale(1)'},{transform:'scale(1.08)'},{transform:'scale(1)'}], {duration:350});
    });
  });

  // ---- badge speech ----
  const badge = document.getElementById('badge');
  const speech = document.getElementById('badgeSpeech');
  badge.addEventListener('click', () => {
    speech.classList.toggle('show');
    badge.animate([{transform:'rotate(0deg)'},{transform:'rotate(-10deg)'},{transform:'rotate(0deg)'}], {duration:400});
  });

  // ---- closing confetti hearts ----
  const loveBtn = document.getElementById('loveBtn');
  const hearts = ['💗','💛','💫','🌸','💌'];
  loveBtn.addEventListener('click', () => {
    for(let i=0;i<18;i++){
      const h = document.createElement('div');
      h.className = 'heart-piece';
      h.textContent = hearts[Math.floor(Math.random()*hearts.length)];
      h.style.left = Math.random()*100 + 'vw';
      h.style.animationDuration = (2.2 + Math.random()*1.6) + 's';
      h.style.fontSize = (14 + Math.random()*14) + 'px';
      document.body.appendChild(h);
      setTimeout(() => h.remove(), 4000);
    }
  });
})();
</script>
</body>
</html>
