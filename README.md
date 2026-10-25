# black-archive.

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>The Black Archive</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Special+Elite&family=Cormorant+Garamond:ital,wght@0,500;0,700;1,500&display=swap" rel="stylesheet">
<style>
  :root {
    --ink: #08080a;
    --paper: #e9e2d0;
    --paper-dim: #a9a293;
    --panel: #121215;
    --line: #35322d;
    --blood: #c1272d;
    --blood-dim: #6e1a1e;
    --amber: #d8b45a;
    --type: "Special Elite", "Courier New", monospace;
    --serif: "Cormorant Garamond", Georgia, serif;
  }
  * { box-sizing: border-box; margin: 0; padding: 0; }
  html { scroll-behavior: smooth; }
  body {
    background: var(--ink);
    color: var(--paper);
    font-family: var(--serif);
    font-size: 1.2rem;
    line-height: 1.7;
    min-height: 100vh;
    background-image: radial-gradient(ellipse at 50% -10%, #1d1114 0%, transparent 60%);
  }
  /* film grain + scanlines */
  body::after {
    content: "";
    position: fixed; inset: 0; pointer-events: none; z-index: 50;
    background: repeating-linear-gradient(0deg, rgba(255,255,255,.025) 0 1px, transparent 1px 3px);
    mix-blend-mode: overlay;
  }
  a, button { font-family: inherit; }
  :focus-visible { outline: 2px solid var(--amber); outline-offset: 3px; }

  /* ---------- Boot screen ---------- */
  #boot {
    position: fixed; inset: 0; z-index: 100; background: #000;
    display: flex; align-items: center; justify-content: center; padding: 24px;
    transition: opacity .8s;
  }
  #boot.done { opacity: 0; pointer-events: none; }
  #bootText { font-family: var(--type); color: var(--amber); font-size: 1rem; white-space: pre-wrap; max-width: 520px; width: 100%; min-height: 9em; }
  #skip { position: absolute; bottom: 24px; right: 24px; background: none; border: 1px solid var(--line); color: var(--paper-dim); padding: 6px 14px; font-family: var(--type); cursor: pointer; }

  /* ---------- Header ---------- */
  header.top {
    display: flex; justify-content: space-between; align-items: center; gap: 16px; flex-wrap: wrap;
    padding: 18px clamp(16px, 4vw, 48px);
    border-bottom: 1px solid var(--line);
    font-family: var(--type); font-size: .95rem; color: var(--paper-dim);
  }
  header.top nav { display: flex; gap: 22px; }
  header.top a { color: var(--paper-dim); text-decoration: none; }
  header.top a:hover { color: var(--paper); }
  .status { color: var(--blood); }
  .status::before { content: ""; display: inline-block; width: 8px; height: 8px; border-radius: 50%; background: var(--blood); margin-right: 8px; animation: pulse 1.6s infinite; }
  @keyframes pulse { 50% { opacity: .2; } }

  /* ---------- Hero ---------- */
  .hero { text-align: center; padding: clamp(70px, 14vh, 140px) 20px 70px; max-width: 900px; margin: 0 auto; }
  .hero h1 {
    font-weight: 700; font-size: clamp(2.8rem, 10vw, 6.4rem); line-height: 1;
    letter-spacing: .08em; position: relative; display: inline-block;
    text-shadow: 0 0 40px rgba(193,39,45,.25);
  }
  .hero h1.glitch::before, .hero h1.glitch::after {
    content: attr(data-text); position: absolute; left: 0; top: 0; width: 100%; overflow: hidden;
  }
  .hero h1.glitch::before { color: var(--blood); transform: translate(-4px, 0); clip-path: inset(10% 0 60% 0); }
  .hero h1.glitch::after { color: #4fd1d9; transform: translate(4px, 0); clip-path: inset(60% 0 10% 0); }
  .hero p.lede { font-style: italic; color: var(--paper-dim); font-size: 1.5rem; margin-top: 22px; }
  .progress {
    display: inline-flex; gap: 36px; margin-top: 44px; padding: 14px 30px;
    border: 1px solid var(--line); font-family: var(--type); font-size: 1rem; color: var(--paper-dim);
  }
  .progress b { color: var(--amber); font-weight: normal; }

  /* ---------- Case grid ---------- */
  section { max-width: 1120px; margin: 0 auto; padding: 40px 20px 70px; }
  section > h2 { font-size: 2.2rem; font-weight: 700; letter-spacing: .04em; }
  section > p.sub { color: var(--paper-dim); margin-bottom: 34px; }
  .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); gap: 24px; }

  .folder {
    text-align: left; cursor: pointer; color: inherit;
    background: var(--panel); border: 1px solid var(--line);
    padding: 0; display: flex; flex-direction: column; position: relative;
    transition: transform .25s, border-color .25s, box-shadow .25s;
  }
  .folder:hover { transform: translateY(-5px) rotate(-.4deg); border-color: var(--blood); box-shadow: 0 14px 40px rgba(193,39,45,.18); }
  .folder .art { height: 150px; position: relative; overflow: hidden; border-bottom: 1px solid var(--line); }
  .folder .body { padding: 18px 20px 22px; }
  .folder .id { font-family: var(--type); font-size: .9rem; color: var(--blood); }
  .folder h3 { font-size: 1.7rem; line-height: 1.15; margin: 4px 0 8px; }
  .folder p { font-size: 1.05rem; color: var(--paper-dim); line-height: 1.5; }
  .folder .meter { margin-top: 14px; font-family: var(--type); font-size: .9rem; color: var(--amber); }
  .stamp {
    position: absolute; top: 14px; right: -6px; transform: rotate(8deg);
    font-family: var(--type); font-size: 1rem; letter-spacing: .12em;
    border: 2px solid var(--blood); color: var(--blood); padding: 2px 10px; background: rgba(8,8,10,.8);
    display: none;
  }
  .folder.solved .stamp { display: block; }

  /* generated cover art, no image files needed */
  .art::after { content: ""; position: absolute; inset: 0; background: radial-gradient(circle at 50% 120%, rgba(0,0,0,.0), rgba(0,0,0,.65)); }
  .a1 { background: linear-gradient(180deg, #17171b, #050506); }
  .a1 .door { position: absolute; left: 50%; bottom: 0; width: 70px; height: 105px; transform: translateX(-50%); background: #0c0c0f; border: 2px solid #2b2b31; }
  .a1 .door::after { content: ""; position: absolute; inset: 0 auto 0 50%; width: 2px; background: #c1272d; box-shadow: 0 0 22px 8px rgba(193,39,45,.55); animation: flick 3s infinite; }
  .a2 { background: repeating-linear-gradient(90deg, #131316 0 6px, #1a1a1f 6px 12px); }
  .a2 .wave { position: absolute; inset: 50% 10% auto 10%; height: 2px; background: var(--amber); box-shadow: 0 -18px 0 0 var(--amber), 0 18px 0 0 var(--amber), 0 -34px 0 0 var(--blood), 0 34px 0 0 var(--blood); opacity: .8; }
  .a3 { background: linear-gradient(180deg, #0d1218, #05070a); }
  .a3 .beam { position: absolute; left: 50%; top: 24px; width: 220px; height: 80px; background: linear-gradient(90deg, rgba(216,180,90,.55), transparent); clip-path: polygon(0 40%, 100% 0, 100% 100%, 0 60%); transform-origin: left center; animation: sweep 6s infinite linear; }
  .a4 { background: #0d0d10; }
  .a4 .face { position: absolute; left: 50%; top: 50%; width: 80px; height: 100px; transform: translate(-50%, -50%); border-radius: 50% 50% 46% 46%; background: #050506; box-shadow: 0 0 0 1px #2c2c33, 0 0 40px rgba(193,39,45,.25); }
  .a4 .face::before, .a4 .face::after { content: ""; position: absolute; top: 40%; width: 10px; height: 4px; background: var(--blood); box-shadow: 0 0 10px var(--blood); }
  .a4 .face::before { left: 20%; } .a4 .face::after { right: 20%; }
  .a5 { background: linear-gradient(135deg, #1a1612, #0a0907); }
  .a5 .photo { position: absolute; left: 50%; top: 50%; width: 120px; height: 90px; transform: translate(-50%, -50%) rotate(-5deg); background: #cfc7b3; padding: 8px; }
  .a5 .photo::after { content: ""; display: block; height: 100%; background: linear-gradient(180deg, #2a2a2e, #0c0c0e); }
  .a6 { background: #100f0d; }
  .a6 .page { position: absolute; left: 50%; top: 50%; width: 90px; height: 110px; transform: translate(-50%, -50%); background: #d9d1bb; background-image: repeating-linear-gradient(0deg, transparent 0 11px, #00000030 11px 12px); clip-path: polygon(0 0, 100% 0, 100% 62%, 70% 66%, 78% 100%, 0 100%); }
  @keyframes flick { 0%, 92%, 100% { opacity: 1; } 94% { opacity: .2; } 96% { opacity: .9; } 98% { opacity: .3; } }
  @keyframes sweep { 0% { transform: rotate(-30deg); } 50% { transform: rotate(30deg); } 100% { transform: rotate(-30deg); } }

  /* ---------- Case modal ---------- */
  dialog {
    background: var(--panel); color: var(--paper); border: 1px solid var(--blood-dim);
    width: min(680px, 94vw); max-height: 90vh; overflow: auto; margin: auto; padding: 0;
  }
  dialog::backdrop { background: rgba(0,0,0,.82); backdrop-filter: blur(3px); }
  .dlg-head { padding: 26px 30px 12px; border-bottom: 1px solid var(--line); position: relative; }
  .dlg-head .id { font-family: var(--type); color: var(--blood); font-size: .95rem; }
  .dlg-head h2 { font-size: 2.2rem; line-height: 1.1; }
  .close { position: absolute; top: 16px; right: 18px; background: none; border: 1px solid var(--line); color: var(--paper-dim); width: 38px; height: 38px; font-size: 1.3rem; cursor: pointer; }
  .close:hover { color: var(--paper); border-color: var(--paper-dim); }
  .dlg-body { padding: 22px 30px 30px; }
  .dlg-body .story { margin-bottom: 22px; }
  .hint { font-family: var(--type); font-size: .95rem; color: var(--paper-dim); margin-bottom: 14px; }

  .clue {
    display: block; width: 100%; text-align: left; cursor: pointer;
    border: 1px dashed var(--line); background: #0b0b0d; color: var(--paper);
    padding: 14px 18px; margin-bottom: 12px; font-size: 1.15rem; line-height: 1.5; position: relative;
  }
  .clue .redact { background: var(--paper); color: transparent; user-select: none; display: block; border-radius: 1px; transition: background .5s, color .5s; }
  .clue:hover .redact { background: #cfc7b3; }
  .clue.open { border: 1px solid var(--amber); cursor: default; }
  .clue.open .redact { background: transparent; color: var(--paper); user-select: auto; }
  .clue .tag { font-family: var(--type); font-size: .85rem; color: var(--amber); display: none; margin-bottom: 4px; }
  .clue.open .tag { display: block; }
  .fragment { margin-top: 20px; padding: 16px 18px; border: 1px solid var(--blood); background: rgba(193,39,45,.08); font-family: var(--type); display: none; }
  .fragment.show { display: block; animation: reveal .8s; }
  .fragment b { color: var(--amber); font-size: 1.6rem; font-weight: normal; }
  @keyframes reveal { from { opacity: 0; letter-spacing: .4em; } to { opacity: 1; } }

  /* ---------- Vault ---------- */
  .vault { border: 1px solid var(--line); background: var(--panel); padding: 36px clamp(18px, 4vw, 44px); text-align: center; }
  .vault p { color: var(--paper-dim); max-width: 560px; margin: 8px auto 22px; }
  .slots { display: flex; justify-content: center; gap: 8px; margin: 18px 0 22px; flex-wrap: wrap; }
  .slots span { width: 44px; height: 56px; border: 1px solid var(--line); display: flex; align-items: center; justify-content: center; font-family: var(--type); font-size: 1.6rem; color: var(--amber); background: #0b0b0d; }
  .vault form { display: flex; justify-content: center; gap: 10px; flex-wrap: wrap; }
  .vault input { background: #0b0b0d; border: 1px solid var(--line); color: var(--paper); font-family: var(--type); font-size: 1.2rem; padding: 10px 14px; width: 190px; text-align: center; letter-spacing: .3em; }
  .btn { background: var(--paper); color: #111; border: none; padding: 11px 24px; font-family: var(--type); font-size: 1rem; cursor: pointer; }
  .btn:hover { background: var(--amber); }
  #vaultMsg { margin-top: 20px; font-family: var(--type); min-height: 1.6em; }
  #vaultMsg.bad { color: var(--blood); }
  #vaultMsg.good { color: var(--amber); }
  #final { display: none; margin-top: 26px; font-style: italic; font-size: 1.5rem; }
  #final.show { display: block; animation: reveal 1.4s; }
  .btn.reset { background: none; color: var(--paper-dim); border: 1px solid var(--line); margin-top: 28px; }

  footer { text-align: center; padding: 40px 20px 60px; color: var(--paper-dim); font-family: var(--type); font-size: .9rem; border-top: 1px solid var(--line); }

  @media (prefers-reduced-motion: reduce) {
    *, *::before, *::after { animation: none !important; transition: none !important; }
  }
  @media (max-width: 560px) {
    header.top nav { display: none; }
    .progress { gap: 18px; padding: 12px 16px; }
  }
</style>
</head>
<body>

<div id="boot" aria-live="polite">
  <div id="bootText"></div>
  <button id="skip" type="button">Skip</button>
</div>

<header class="top">
  <span>ARCHIVE ACCESS: FILE 001</span>
  <nav><a href="#cases">Case files</a><a href="#vault">The vault</a></nav>
  <span class="status">Live connection</span>
</header>

<div class="hero">
  <h1 class="glitch" id="title" data-text="THE BLACK ARCHIVE">THE BLACK ARCHIVE</h1>
  <p class="lede">Some secrets were never meant to be found.</p>
  <div class="progress" aria-live="polite">
    <span>Cases solved <b id="pCases">0/6</b></span>
    <span>Clues found <b id="pClues">0/18</b></span>
  </div>
</div>

<section id="cases">
  <h2>Case files</h2>
  <p class="sub">Open a file, then click each black bar to declassify the evidence. Solve a case to earn one digit of the vault code.</p>
  <div class="grid" id="grid"></div>
</section>

<section id="vault">
  <div class="vault">
    <h2>The vault</h2>
    <p>Six cases, six digits. Collect them in case order and enter the code to open the last file.</p>
    <div class="slots" id="slots"></div>
    <form id="vaultForm" autocomplete="off">
      <input id="code" inputmode="numeric" maxlength="6" placeholder="------" aria-label="Six digit vault code">
      <button class="btn" type="submit">Unlock</button>
    </form>
    <div id="vaultMsg" role="status"></div>
    <div id="final">“The room was never empty. Someone was always listening.”</div>
    <button class="btn reset" id="reset" type="button">Wipe my progress</button>
  </div>
</section>

<footer>This archive is fiction. Nobody was harmed in the making of these files.</footer>

<dialog id="dlg" aria-labelledby="dTitle">
  <div class="dlg-head">
    <div class="id" id="dId"></div>
    <h2 id="dTitle"></h2>
    <button class="close" id="dClose" type="button" aria-label="Close case file">&times;</button>
  </div>
  <div class="dlg-body">
    <p class="story" id="dStory"></p>
    <p class="hint">Click a black bar to declassify it.</p>
    <div id="dClues"></div>
    <div class="fragment" id="dFrag">CASE SOLVED. Code fragment: <b id="dDigit"></b></div>
  </div>
</dialog>

<script>
  // ---------- Your content: edit this to make it yours ----------
  const CASES = [
    { id: "CASE 001", title: "The Empty Room", art: "a1", digit: "0",
      story: "At 03:17 AM, a recording appeared from a room that had been locked for twenty years.",
      clues: ["The recording contains a clock ticking backwards.", "A whisper repeats one sentence: “Count the doors, not the rooms.”", "The tape was labelled in your handwriting, dated tomorrow."] },
    { id: "CASE 002", title: "The Last Broadcast", art: "a2", digit: "3",
      story: "A radio station that closed in 1998 started transmitting again. Only one listener ever called in.",
      clues: ["The signal comes from a building with no power.", "Every broadcast ends with three short beeps and a long one.", "The caller’s number was disconnected before the station opened."] },
    { id: "CASE 003", title: "The Lighthouse Keeper", art: "a3", digit: "1",
      story: "The lamp turns every night, but the lighthouse has had no keeper since the storm.",
      clues: ["The logbook is written in fresh ink, one line per night.", "The beam always stops on the same patch of empty water.", "The final entry reads: “It is not a ship.”"] },
    { id: "CASE 004", title: "Subject 17", art: "a4", digit: "7",
      story: "A patient file with no name, no photo, and a hospital stamp from a ward that was never built.",
      clues: ["The file lists seventeen visitors. Sixteen are already crossed out.", "The handwriting changes halfway through every page.", "Under UV light, a message appears: “Do not read the last page.”"] },
    { id: "CASE 005", title: "The Photograph", art: "a5", digit: "2",
      story: "A family photo turned up in the archive. Everyone in it is smiling, except the person who took it.",
      clues: ["A shadow in the corner has two more shoulders than it should.", "The back of the photo says: “Thank you for staying quiet.”", "The date printed on the border is a year that hasn’t happened yet."] },
    { id: "CASE 006", title: "The Missing Page", art: "a6", digit: "0",
      story: "Someone tore the last page out of the archive’s master ledger. The ledger kept growing anyway.",
      clues: ["The torn edge matches a page found inside Case 001.", "The ledger counts entries from zero, not one.", "The final line is blank, but the pen has been pressed hard enough to dent the paper."] }
  ];
  const VAULT_CODE = CASES.map(c => c.digit).join("");   // currently 031720 (change the digits above to change it)
  const KEY = "blackArchiveProgress";

  // ---------- State (saved in this browser) ----------
  let state = {};
  try { state = JSON.parse(localStorage.getItem(KEY)) || {}; } catch (e) { state = {}; }
  function save() { try { localStorage.setItem(KEY, JSON.stringify(state)); } catch (e) {} }
  const found = i => state[i] || [];
  const solved = i => found(i).length === CASES[i].clues.length;

  // ---------- Grid ----------
  const grid = document.getElementById("grid");
  const artHTML = { a1: '<div class="door"></div>', a2: '<div class="wave"></div>', a3: '<div class="beam"></div>', a4: '<div class="face"></div>', a5: '<div class="photo"></div>', a6: '<div class="page"></div>' };

  function renderGrid() {
    grid.innerHTML = "";
    CASES.forEach((c, i) => {
      const b = document.createElement("button");
      b.className = "folder" + (solved(i) ? " solved" : "");
      b.type = "button";
      b.innerHTML = '<div class="art ' + c.art + '">' + artHTML[c.art] + '</div>' +
        '<span class="stamp">SOLVED</span>' +
        '<div class="body"><div class="id">' + c.id + '</div><h3>' + c.title + '</h3><p>' + c.story + '</p>' +
        '<div class="meter">Evidence ' + found(i).length + '/' + c.clues.length + '</div></div>';
      b.addEventListener("click", () => openCase(i));
      grid.appendChild(b);
    });
    updateProgress();
  }

  function updateProgress() {
    let cs = 0, cl = 0;
    CASES.forEach((c, i) => { if (solved(i)) cs++; cl += found(i).length; });
    document.getElementById("pCases").textContent = cs + "/" + CASES.length;
    document.getElementById("pClues").textContent = cl + "/" + CASES.reduce((n, c) => n + c.clues.length, 0);
    const slots = document.getElementById("slots");
    slots.innerHTML = "";
    CASES.forEach((c, i) => { const s = document.createElement("span"); s.textContent = solved(i) ? c.digit : "?"; slots.appendChild(s); });
  }

  // ---------- Case dialog ----------
  const dlg = document.getElementById("dlg");
  let current = 0;

  function openCase(i) {
    current = i;
    const c = CASES[i];
    document.getElementById("dId").textContent = c.id;
    document.getElementById("dTitle").textContent = c.title;
    document.getElementById("dStory").textContent = c.story;
    const box = document.getElementById("dClues");
    box.innerHTML = "";
    c.clues.forEach((text, n) => {
      const el = document.createElement("button");
      el.type = "button";
      el.className = "clue" + (found(i).includes(n) ? " open" : "");
      el.innerHTML = '<span class="tag">CLUE ' + (n + 1) + ' DECLASSIFIED</span><span class="redact"></span>';
      el.querySelector(".redact").textContent = text;
      el.addEventListener("click", () => {
        if (el.classList.contains("open")) return;
        el.classList.add("open");
        state[i] = [...new Set([...found(i), n])];
        save(); showFragment(); renderGrid();
      });
      box.appendChild(el);
    });
    showFragment();
    dlg.showModal();
  }

  function showFragment() {
    const f = document.getElementById("dFrag");
    if (solved(current)) {
      document.getElementById("dDigit").textContent = CASES[current].digit;
      f.classList.add("show");
    } else f.classList.remove("show");
  }
  document.getElementById("dClose").addEventListener("click", () => dlg.close());
  dlg.addEventListener("click", e => { if (e.target === dlg) dlg.close(); });

  // ---------- Vault ----------
  const msg = document.getElementById("vaultMsg");
  document.getElementById("vaultForm").addEventListener("submit", e => {
    e.preventDefault();
    const val = document.getElementById("code").value.trim();
    const all = CASES.every((c, i) => solved(i));
    msg.className = "";
    if (val === VAULT_CODE && all) {
      msg.textContent = "ACCESS GRANTED.";
      msg.className = "good";
      document.getElementById("final").classList.add("show");
    } else if (val === VAULT_CODE) {
      msg.textContent = "Right code, but the files aren’t all declassified yet.";
      msg.className = "bad";
    } else {
      msg.textContent = "ACCESS DENIED. The archive is watching.";
      msg.className = "bad";
    }
  });
  document.getElementById("reset").addEventListener("click", () => {
    if (!confirm("Wipe all progress and start over?")) return;
    state = {}; save(); renderGrid();
    document.getElementById("final").classList.remove("show");
    msg.textContent = ""; document.getElementById("code").value = "";
  });

  // ---------- Boot sequence + title glitch ----------
  const boot = document.getElementById("boot");
  const bootEl = document.getElementById("bootText");
  const lines = ["> CONNECTING TO ARCHIVE...", "> VERIFYING CLEARANCE...", "> 03:17:00 SIGNAL DETECTED", "> ACCESS GRANTED"];
  let li = 0, ci = 0, timer;
  function finishBoot() { clearTimeout(timer); boot.classList.add("done"); sessionStorageSafe(); }
  function sessionStorageSafe() { try { sessionStorage.setItem("booted", "1"); } catch (e) {} }
  function type() {
    if (li >= lines.length) { timer = setTimeout(finishBoot, 600); return; }
    if (ci < lines[li].length) { bootEl.textContent += lines[li][ci++]; timer = setTimeout(type, 35); }
    else { bootEl.textContent += "\n"; li++; ci = 0; timer = setTimeout(type, 350); }
  }
  let seen = false;
  try { seen = sessionStorage.getItem("booted") === "1"; } catch (e) {}
  if (seen || window.matchMedia("(prefers-reduced-motion: reduce)").matches) boot.classList.add("done"); else type();
  document.getElementById("skip").addEventListener("click", finishBoot);

  const title = document.getElementById("title");
  setInterval(() => {
    title.classList.add("glitch");
    setTimeout(() => title.classList.remove("glitch"), 180);
  }, 5000);
  title.classList.remove("glitch");

  renderGrid();
</script>
</body>
</html>
