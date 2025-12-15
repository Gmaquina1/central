<!doctype html>
<html lang="pt-br">
<head>
  <meta charset="utf-8"/>
  <meta name="viewport" content="width=device-width,initial-scale=1"/>
  <title>Iluminação • G MÁQUINA</title>
  <link rel="stylesheet" href="css/theme.css"/>

  <style>
    .span-12{grid-column:span 12}
    .span-8{grid-column:span 8}
    .span-6{grid-column:span 6}
    .span-4{grid-column:span 4}

    .titleRow{display:flex; align-items:center; justify-content:space-between; gap:10px; flex-wrap:wrap}
    .titleRow h3{margin:0}

    .sectors{
      display:grid;
      grid-template-columns: repeat(12, 1fr);
      gap:12px;
      margin-top:12px;
    }
    .sector{
      grid-column: span 6;
      border-radius: var(--radius);
      border:1px solid rgba(255,255,255,.12);
      background: rgba(0,0,0,.18);
      padding:12px;
      position:relative;
      overflow:hidden;
    }
    .sector:before{
      content:"";
      position:absolute; inset:-90px;
      background: radial-gradient(420px 200px at 85% 35%, rgba(85,214,190,.12), transparent 60%);
      pointer-events:none;
    }
    .sector > *{position:relative}
    .secTop{display:flex; align-items:center; justify-content:space-between; gap:10px}
    .secName{font-weight:950}
    .secState{
      font-size:12px; padding:6px 10px; border-radius:999px;
      border:1px solid rgba(255,255,255,.14); background: rgba(0,0,0,.22)
    }
    .secState.on{border-color: rgba(46,229,157,.45); background: rgba(46,229,157,.10)}
    .secState.off{border-color: rgba(255,255,255,.12); background: rgba(255,255,255,.04)}

    .row{display:flex; gap:10px; flex-wrap:wrap; margin-top:12px}
    .chip{
      display:inline-flex; align-items:center; gap:6px;
      padding:6px 10px; border-radius:999px;
      border:1px solid rgba(255,255,255,.14);
      background: rgba(0,0,0,.22);
      font-size:12px;
    }

    .switch{
      display:flex; align-items:center; justify-content:space-between; gap:12px;
      padding:12px;
      border-radius: var(--radius);
      border:1px solid rgba(255,255,255,.12);
      background: rgba(0,0,0,.18);
      margin-top:12px;
    }
    .toggle{
      width:54px; height:30px; border-radius:999px;
      border:1px solid rgba(255,255,255,.16);
      background: rgba(255,255,255,.06);
      position:relative; cursor:pointer;
      transition:.18s ease;
    }
    .toggle i{
      position:absolute; top:50%; transform:translateY(-50%);
      left:4px; width:22px; height:22px; border-radius:999px;
      background: rgba(255,255,255,.85);
      transition:.18s ease;
    }
    .toggle.on{border-color: rgba(85,214,190,.55); background: rgba(85,214,190,.14)}
    .toggle.on i{left:28px; background: rgba(46,229,157,.95)}

    .log{
      margin-top:12px;
      border-radius: var(--radius);
      border:1px solid rgba(255,255,255,.12);
      background: rgba(0,0,0,.16);
      overflow:hidden;
    }
    .logItem{
      padding:10px 12px;
      display:flex; justify-content:space-between; gap:10px; flex-wrap:wrap;
      border-top:1px solid rgba(255,255,255,.08);
      font-size:13px;
    }
    .logItem:first-child{border-top:none}
    .time{color:var(--muted)}

    .sliderRow{margin-top:12px; display:grid; gap:10px}
    .range{
      width:100%;
      accent-color: var(--brand2);
    }

    @media (max-width: 980px){
      .span-8,.span-6,.span-4{grid-column:span 12}
      .sector{grid-column:span 12}
    }
  </style>
</head>
<body>

  <div class="topbar">
    <div class="topbar-inner">
      <div class="brand">
        <img src="assets/logo-gmaquina.png" alt="G MÁQUINA">
        <div>
          <div class="t1">G MÁQUINA • Iluminação</div>
          <div class="t2">Setores • cenas • automação</div>
        </div>
      </div>

      <div class="nav">
        <a href="index.html">Central</a>
        <a href="irrigacao.html">Irrigação</a>
        <a href="seguranca.html">Segurança</a>
        <a class="active" href="iluminacao.html">Iluminação</a>
      </div>
    </div>
  </div>

  <div class="container">

    <section class="hero">
      <h1 class="h-title">Iluminação & Cenas</h1>
      <p class="h-sub">
        Controle por setores, cenas prontas (chegada/noite/segurança) e timer.
        Hoje simulado — depois liga no ESP32 de iluminação.
      </p>
      <div class="badges">
        <div class="badge"><span class="dot warn"></span><span>Modo: Simulação</span></div>
        <div class="badge"><span class="dot"></span><span>Módulo: Online</span></div>
        <div class="badge"><span class="dot"></span><span>Automação disponível</span></div>
      </div>
    </section>

    <section class="grid">

      <!-- STATUS -->
      <div class="card span-8">
        <div class="titleRow">
          <h3>💡 Status do Sistema</h3>
          <span class="pill" id="pill">Pronto</span>
        </div>

        <div class="kpis">
          <div class="kpi">Setores ligados: <b id="onCount">0</b></div>
          <div class="kpi">Cena ativa: <b id="scene">—</b></div>
          <div class="kpi">Timer: <b id="timerKpi">OFF</b></div>
        </div>

        <div class="row">
          <button class="btn primary" id="allOn">Ligar tudo</button>
          <button class="btn danger" id="allOff">Desligar tudo</button>
          <button class="btn" id="clearScene">Limpar cena</button>
        </div>

        <div class="switch">
          <div>
            <div style="font-weight:950">Automático por presença</div>
            <div class="small">Se detectar movimento à noite, liga corredor/varanda (simulado)</div>
          </div>
          <div class="toggle" id="auto"><i></i></div>
        </div>

        <div class="sliderRow">
          <div class="small">Brilho global (simulação)</div>
          <input class="range" id="bright" type="range" min="0" max="100" value="70">
          <div class="chip">🔆 Brilho: <b id="brightVal">70%</b></div>
        </div>
      </div>

      <!-- CENAS -->
      <div class="card span-4">
        <div class="titleRow">
          <h3>🎬 Cenas</h3>
          <span class="pill">1 clique</span>
        </div>
        <p class="small" style="margin-top:8px">Cenas típicas de chácara. Dá pra criar quantas quiser.</p>

        <div class="row">
          <button class="btn primary" data-scene="chegada">Chegada</button>
          <button class="btn" data-scene="noite">Noite</button>
          <button class="btn" data-scene="seguranca">Segurança</button>
        </div>

        <hr class="sep">

        <div class="titleRow">
          <h3 style="font-size:16px">⏱️ Timer</h3>
          <span class="pill" id="timerPill">OFF</span>
        </div>

        <div style="margin-top:10px; display:grid; gap:10px">
          <input class="input" id="min" type="number" min="1" max="120" value="10" placeholder="Minutos">
          <button class="btn" id="startTimer">Ativar timer</button>
          <button class="btn ghost" id="stopTimer">Parar timer</button>
        </div>
      </div>

      <!-- SETORES -->
      <div class="card span-12">
        <div class="titleRow">
          <h3>🏷️ Setores</h3>
          <span class="pill"><span id="sCount">6</span> setores</span>
        </div>
        <p class="small" style="margin-top:8px">Cada setor vira um relé/canal no ESP32 de iluminação.</p>

        <div class="sectors" id="sectors"></div>
      </div>

      <!-- LOG -->
      <div class="card span-12">
        <div class="titleRow">
          <h3>🧾 Log</h3>
          <span class="pill">Ativo</span>
        </div>

        <div class="log" id="log"></div>

        <div class="row">
          <button class="btn" id="clearLog">Limpar log</button>
          <a class="btn ghost" href="index.html">⬅ Voltar</a>
        </div>
      </div>

    </section>

    <div class="small" style="margin-top:18px; opacity:.85">
      Próximo bloco: “conectar nos ESP32 de verdade” (API /api/status e /api/control).
    </div>

  </div>

<script>
  const state = {
    auto:false,
    brightness:70,
    scene:null,
    timer:null,
    sectors:[
      {id:1, icon:"🏠", name:"Varanda", on:false},
      {id:2, icon:"🌿", name:"Jardim", on:false},
      {id:3, icon:"🚪", name:"Entrada", on:false},
      {id:4, icon:"🛣️", name:"Corredor", on:false},
      {id:5, icon:"🚜", name:"Galpão", on:false},
      {id:6, icon:"🔦", name:"Refletores", on:false},
    ]
  };

  const sectorsEl = document.getElementById("sectors");
  const logEl = document.getElementById("log");

  const onCountEl = document.getElementById("onCount");
  const sceneEl = document.getElementById("scene");
  const timerKpi = document.getElementById("timerKpi");
  const timerPill = document.getElementById("timerPill");

  const autoToggle = document.getElementById("auto");
  const bright = document.getElementById("bright");
  const brightVal = document.getElementById("brightVal");

  document.getElementById("sCount").textContent = state.sectors.length;

  function nowStr(){
    const d=new Date();
    return d.toLocaleTimeString([], {hour:"2-digit", minute:"2-digit", second:"2-digit"});
  }
  function pushLog(msg){
    const item=document.createElement("div");
    item.className="logItem";
    item.innerHTML=`<div>${msg}</div><div class="time">${nowStr()}</div>`;
    logEl.prepend(item);
    if(logEl.children.length>40) logEl.removeChild(logEl.lastChild);
  }

  function renderKPIs(){
    const onCount = state.sectors.filter(s=>s.on).length;
    onCountEl.textContent = onCount;
    sceneEl.textContent = state.scene ? state.scene.toUpperCase() : "—";
    timerKpi.textContent = state.timer ? "ON" : "OFF";
    timerPill.textContent = state.timer ? "ON" : "OFF";
    autoToggle.classList.toggle("on", state.auto);
    brightVal.textContent = state.brightness + "%";
  }

  function sectorCard(s){
    const st = s.on ? "on" : "off";
    const txt = s.on ? "LIGADO" : "DESLIGADO";
    return `
      <div class="sector" data-id="${s.id}">
        <div class="secTop">
          <div class="secName">${s.icon} ${s.name}</div>
          <div class="secState ${st}">${txt}</div>
        </div>
        <div class="row">
          <button class="btn primary" data-act="on">Ligar</button>
          <button class="btn danger" data-act="off">Desligar</button>
          <button class="btn" data-act="pulse">Pulso 20s</button>
        </div>
      </div>
    `;
  }

  function renderSectors(){
    sectorsEl.innerHTML = state.sectors.map(sectorCard).join("");
  }

  function setAll(v){
    state.sectors.forEach(s=>s.on=v);
    pushLog(v ? "✅ Ação global: <b>LIGAR TUDO</b>" : "🧯 Ação global: <b>DESLIGAR TUDO</b>");
    renderSectors(); renderKPIs();
  }

  function applyScene(name){
    state.scene = name;
    // Cenas:
    // chegada: entrada+varanda+corredor+refletores
    // noite: varanda+corredor+jardim
    // seguranca: refletores + entrada
    const map = {
      chegada: [3,1,4,6],
      noite: [1,4,2],
      seguranca: [6,3]
    };
    const onIds = map[name] || [];
    state.sectors.forEach(s=> s.on = onIds.includes(s.id));
    pushLog(`🎬 Cena aplicada: <b>${name.toUpperCase()}</b>`);
    renderSectors(); renderKPIs();
  }

  // Botões globais
  document.getElementById("allOn").onclick = ()=>setAll(true);
  document.getElementById("allOff").onclick = ()=>setAll(false);
  document.getElementById("clearScene").onclick = ()=>{
    state.scene=null;
    pushLog("🧹 Cena limpa.");
    renderKPIs();
  };

  // Toggle auto
  autoToggle.onclick = ()=>{
    state.auto = !state.auto;
    pushLog(state.auto ? "🤖 Automático por presença <b>ATIVADO</b>." : "🧍 Automático por presença <b>DESATIVADO</b>.");
    renderKPIs();
  };

  // Brilho
  bright.oninput = ()=>{
    state.brightness = Number(bright.value);
    renderKPIs();
  };

  // Cenas
  document.querySelectorAll("button[data-scene]").forEach(btn=>{
    btn.onclick = ()=> applyScene(btn.dataset.scene);
  });

  // Timer
  let timerHandle = null;
  document.getElementById("startTimer").onclick = ()=>{
    const minutes = Math.max(1, Math.min(120, Number(document.getElementById("min").value || 10)));
    pushLog(`⏱️ Timer ligado: <b>${minutes} min</b> (depois desliga tudo).`);
    if(timerHandle) clearTimeout(timerHandle);
    state.timer = Date.now() + minutes*60*1000;
    renderKPIs();
    timerHandle = setTimeout(()=>{
      setAll(false);
      state.timer = null;
      renderKPIs();
      pushLog("⏱️ Timer concluído: sistema desligou tudo.");
    }, 2500); // simulação rápida (2.5s)
  };
  document.getElementById("stopTimer").onclick = ()=>{
    if(timerHandle) clearTimeout(timerHandle);
    timerHandle = null;
    state.timer = null;
    pushLog("⏱️ Timer parado.");
    renderKPIs();
  };

  // Ações por setor
  sectorsEl.addEventListener("click", (ev)=>{
    const btn = ev.target.closest("button[data-act]");
    if(!btn) return;
    const wrap = ev.target.closest(".sector");
    const id = Number(wrap.dataset.id);
    const s = state.sectors.find(x=>x.id===id);
    const act = btn.dataset.act;

    if(act==="on"){ s.on=true; pushLog(`💡 ${s.name}: <b>LIGAR</b>`); }
    if(act==="off"){ s.on=false; pushLog(`💡 ${s.name}: <b>DESLIGAR</b>`); }
    if(act==="pulse"){
      pushLog(`💡 ${s.name}: <b>PULSO 20s</b>`);
      s.on=true; renderSectors(); renderKPIs();
      setTimeout(()=>{ s.on=false; renderSectors(); renderKPIs(); pushLog(`💡 ${s.name}: pulso concluído.`); }, 1200);
      return;
    }

    renderSectors(); renderKPIs();
  });

  // Log
  document.getElementById("clearLog").onclick = ()=>{
    logEl.innerHTML="";
    pushLog("Log limpo.");
  };

  // Simulação: presença à noite
  setInterval(()=>{
    if(!state.auto) return;
    // chance pequena de presença
    if(Math.random() < 0.22){
      pushLog("👣 Presença detectada (simulação).");
      // liga corredor+varanda por 2s
      const ids = [4,1];
      state.sectors.forEach(s=>{
        if(ids.includes(s.id)) s.on = true;
      });
      renderSectors(); renderKPIs();
      setTimeout(()=>{
        state.sectors.forEach(s=>{
          if(ids.includes(s.id) && !state.scene) s.on = false;
        });
        renderSectors(); renderKPIs();
        pushLog("✅ Ação automática finalizada.");
      }, 2200);
    }
  }, 5000);

  // init
  renderSectors();
  renderKPIs();
  pushLog("Painel de iluminação iniciado (simulação).");
</script>
</body>
</html>

<!doctype html>
<html lang="pt-br">
<head>
  <meta charset="utf-8"/>
  <meta name="viewport" content="width=device-width,initial-scale=1"/>
  <title>Central G MÁQUINA • Chácara</title>
  <link rel="stylesheet" href="css/theme.css"/>
<link rel="manifest" href="manifest.json">
<meta name="theme-color" content="#070A0F">

<!-- iPhone: abrir como app -->
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="G MÁQUINA">
<link rel="apple-touch-icon" href="assets/icon-192.png">

</head>
<body>

  <!-- TOPBAR -->
  <div class="topbar">
    <div class="topbar-inner">
      <div class="brand">
        <img src="./assets/logo-gmaquina.png" alt="G MÁQUINA">
        <div>
          <div class="t1">G MÁQUINA • Central da Chácara</div>
          <div class="t2">Automação premium • ESP32 independentes</div>
        </div>
      </div>

      <div class="nav">
        <a class="active" href="index.html">Central</a>
        <a href="irrigacao.html">Irrigação</a>
        <a href="seguranca.html">Segurança</a>
        <a href="iluminacao.html">Iluminação</a>
      </div>
    </div>
  </div>

  <div class="container">

    <!-- HERO -->
    <section class="hero">
      <h1 class="h-title">Controle total, sem depender de um único sistema</h1>
      <p class="h-sub">
        Aqui você comanda tudo: <b>irrigação</b>, <b>segurança</b> e <b>iluminação</b>.
        Cada módulo roda em um ESP32 separado — se um falhar, os outros continuam funcionando.
      </p>

      <div class="badges">
        <div class="badge"><span class="dot"></span><span>Central Online</span></div>
        <div class="badge"><span class="dot warn"></span><span>Modo: Simulação</span></div>
        <div class="badge"><span class="dot"></span><span>G MÁQUINA • Premium</span></div>
      </div>
    </section>

    <!-- GRID DE MÓDULOS -->
    <section class="grid">

      <a class="tile" href="irrigacao.html">
        <div class="pill">Módulo crítico</div>
        <div class="big">💧</div>
        <h3>Irrigação Inteligente</h3>
        <p>Controle por zonas, sensores (umidade/pressão), agenda, automação e alertas.</p>
        <div class="kpis">
          <div class="kpi">Status: <b>Simulação</b></div>
          <div class="kpi">ESP32: <b>Independente</b></div>
          <div class="kpi">Automação: <b>Ativável</b></div>
        </div>
      </a>

      <a class="tile" href="seguranca.html">
        <div class="pill">Módulo crítico</div>
        <div class="big">🛡️</div>
        <h3>Segurança & Perímetro</h3>
        <p>Modo armado/desarmado, sensores, sirene, eventos e monitoramento.</p>
        <div class="kpis">
          <div class="kpi">Status: <b>Simulação</b></div>
          <div class="kpi">ESP32: <b>Independente</b></div>
          <div class="kpi">Eventos: <b>Log</b></div>
        </div>
      </a>

      <a class="tile" href="iluminacao.html">
        <div class="pill">Automação</div>
        <div class="big">💡</div>
        <h3>Iluminação & Cenas</h3>
        <p>Setores, cenas (noite/chegada), timers, presença e economia de energia.</p>
        <div class="kpis">
          <div class="kpi">Status: <b>Simulação</b></div>
          <div class="kpi">ESP32: <b>Independente</b></div>
          <div class="kpi">Cenas: <b>Prontas</b></div>
        </div>
      </a>

    </section>

    <!-- PAINEL EXTRA -->
    <section class="grid">
      <div class="card" style="grid-column: span 12;">
        <div style="display:flex; align-items:center; justify-content:space-between; gap:10px; flex-wrap:wrap">
          <div>
            <h3 style="margin:0">Visão do sistema</h3>
            <p class="small" style="margin-top:6px">
              Próximo passo: ligar cada módulo no seu IP e mostrar status real (online/offline, sensores, logs).
            </p>
          </div>
          <div style="display:flex; gap:10px; flex-wrap:wrap">
            <a class="btn primary" href="irrigacao.html">Abrir Irrigação</a>
            <a class="btn" href="seguranca.html">Abrir Segurança</a>
            <a class="btn" href="iluminacao.html">Abrir Iluminação</a>
          </div>
        </div>

        <hr class="sep">

        <div class="kpis">
          <div class="kpi">Arquitetura: <b>Multi-ESP32</b></div>
          <div class="kpi">Modo: <b>Fail-safe</b></div>
          <div class="kpi">Rede: <b>Local + Expansível</b></div>
          <div class="kpi">Marca: <b>G MÁQUINA</b></div>
        </div>
      </div>
    </section>

    <div class="small" style="margin-top:18px; opacity:.8">
      © <span id="y"></span> G MÁQUINA • Central Web
    </div>

  </div>

  <script>
    document.getElementById("y").textContent = new Date().getFullYear();
  </script>

</body>
</html>

<!doctype html>
<html lang="pt-br">
<head>
  <meta charset="utf-8"/>
  <meta name="viewport" content="width=device-width,initial-scale=1"/>
  <title>Irrigação • G MÁQUINA</title>
  <link rel="stylesheet" href="css/theme.css"/>

  <style>
    .span-12{grid-column:span 12}
    .span-8{grid-column:span 8}
    .span-6{grid-column:span 6}
    .span-4{grid-column:span 4}

    .titleRow{display:flex; align-items:center; justify-content:space-between; gap:10px; flex-wrap:wrap}
    .titleRow h3{margin:0}
    .muted{color:var(--muted)}

    .zones{
      display:grid;
      grid-template-columns: repeat(12, 1fr);
      gap:12px;
      margin-top:12px;
    }
    .zone{
      grid-column: span 6;
      border-radius: var(--radius);
      border:1px solid rgba(255,255,255,.12);
      background: rgba(0,0,0,.18);
      padding:12px;
      position:relative;
      overflow:hidden;
    }
    .zone:before{
      content:"";
      position:absolute; inset:-90px;
      background: radial-gradient(420px 200px at 85% 35%, rgba(85,214,190,.10), transparent 60%);
      pointer-events:none;
    }
    .zone > *{position:relative}
    .zoneTop{display:flex; align-items:center; justify-content:space-between; gap:10px}
    .zoneName{font-weight:900}
    .zoneState{font-size:12px; padding:6px 10px; border-radius:999px; border:1px solid rgba(255,255,255,.14); background: rgba(0,0,0,.22)}
    .zoneState.on{border-color: rgba(46,229,157,.45); background: rgba(46,229,157,.10)}
    .zoneState.off{border-color: rgba(255,255,255,.12); background: rgba(255,255,255,.04)}
    .zoneActions{display:flex; gap:10px; flex-wrap:wrap; margin-top:10px}

    .sensorGrid{display:grid; grid-template-columns: repeat(12, 1fr); gap:12px; margin-top:12px}
    .sensor{
      grid-column: span 6;
      border-radius: var(--radius);
      border:1px solid rgba(255,255,255,.12);
      background: rgba(0,0,0,.18);
      padding:12px;
    }
    .sensorHead{display:flex; align-items:center; justify-content:space-between; gap:10px}
    .sensorVal{font-size:26px; font-weight:950; margin-top:8px}
    .bar{
      height:10px; border-radius:999px;
      border:1px solid rgba(255,255,255,.12);
      background: rgba(255,255,255,.05);
      overflow:hidden;
      margin-top:10px;
    }
    .bar > i{
      display:block; height:100%;
      width:50%;
      background: linear-gradient(90deg, rgba(85,214,190,.65), rgba(230,184,76,.70));
    }

    .log{
      margin-top:12px;
      border-radius: var(--radius);
      border:1px solid rgba(255,255,255,.12);
      background: rgba(0,0,0,.16);
      overflow:hidden;
    }
    .logItem{
      padding:10px 12px;
      display:flex; justify-content:space-between; gap:10px; flex-wrap:wrap;
      border-top:1px solid rgba(255,255,255,.08);
      font-size:13px;
    }
    .logItem:first-child{border-top:none}
    .time{color:var(--muted)}
    .chip{
      display:inline-flex; align-items:center; gap:6px;
      padding:6px 10px; border-radius:999px;
      border:1px solid rgba(255,255,255,.14);
      background: rgba(0,0,0,.22);
      font-size:12px;
    }
    @media (max-width: 980px){
      .span-8,.span-6,.span-4{grid-column:span 12}
      .zone{grid-column:span 12}
      .sensor{grid-column:span 12}
    }
  </style>
</head>
<body>

  <!-- TOPBAR -->
  <div class="topbar">
    <div class="topbar-inner">
      <div class="brand">
        <img src="assets/logo-gmaquina.png" alt="G MÁQUINA">
        <div>
          <div class="t1">G MÁQUINA • Irrigação</div>
          <div class="t2">Painel dedicado • pronto p/ sensores</div>
        </div>
      </div>

      <div class="nav">
        <a href="index.html">Central</a>
        <a class="active" href="irrigacao.html">Irrigação</a>
        <a href="seguranca.html">Segurança</a>
        <a href="iluminacao.html">Iluminação</a>
      </div>
    </div>
  </div>

  <div class="container">

    <section class="hero">
      <h1 class="h-title">Irrigação Inteligente</h1>
      <p class="h-sub">
        Controle por zonas + monitoramento de <b>umidade</b> e <b>pressão</b>.
        Hoje em simulação. Depois a gente liga direto no ESP32.
      </p>
      <div class="badges">
        <div class="badge" id="b1"><span class="dot warn"></span><span>Modo: Simulação</span></div>
        <div class="badge" id="b2"><span class="dot"></span><span>Módulo: Online</span></div>
        <div class="badge"><span class="dot"></span><span>Fail-safe (independente)</span></div>
      </div>
    </section>

    <section class="grid">

      <!-- STATUS -->
      <div class="card span-8">
        <div class="titleRow">
          <h3>📡 Status do Módulo</h3>
          <span class="pill" id="pill">Simulação</span>
        </div>
        <p class="muted" style="margin-top:8px">Resumo do estado atual do sistema de irrigação.</p>

        <div class="kpis">
          <div class="kpi">Modo: <b id="mode">AUTO</b></div>
          <div class="kpi">Zonas ativas: <b id="zonesOn">0</b></div>
          <div class="kpi">Pressão: <b id="pressKpi">—</b></div>
          <div class="kpi">Umidade: <b id="humKpi">—</b></div>
        </div>

        <div style="margin-top:12px; display:flex; gap:10px; flex-wrap:wrap">
          <button class="btn primary" id="allOn">Ligar tudo</button>
          <button class="btn danger" id="allOff">Desligar tudo</button>
          <button class="btn" id="auto">Modo AUTO</button>
          <button class="btn ghost" id="manual">Modo MANUAL</button>
        </div>
      </div>

      <!-- AGENDA (simulação) -->
      <div class="card span-4">
        <div class="titleRow">
          <h3>🗓️ Agenda</h3>
          <span class="pill">Simulação</span>
        </div>
        <p class="muted" style="margin-top:8px">Depois vamos ligar com horários reais e dias da semana.</p>

        <div style="margin-top:12px; display:grid; gap:10px">
          <div class="chip">⏱️ 06:00 • Zona 1 • 10 min</div>
          <div class="chip">⏱️ 16:30 • Zona 2 • 12 min</div>
          <div class="chip">⏱️ 19:10 • Zona 3 • 08 min</div>
        </div>

        <div style="margin-top:12px; display:flex; gap:10px; flex-wrap:wrap">
          <button class="btn" id="simRun">Rodar agenda agora</button>
        </div>
      </div>

      <!-- ZONAS -->
      <div class="card span-12">
        <div class="titleRow">
          <h3>💧 Zonas</h3>
          <span class="pill"><span id="zCount">4</span> zonas</span>
        </div>
        <p class="muted" style="margin-top:8px">Cada zona vira uma válvula/relé no seu ESP32 de irrigação.</p>

        <div class="zones" id="zones"></div>
      </div>

      <!-- SENSORES -->
      <div class="card span-6">
        <div class="titleRow">
          <h3>🌿 Sensor de Umidade</h3>
          <span class="pill" id="humState">Normal</span>
        </div>

        <div class="sensorGrid">
          <div class="sensor">
            <div class="sensorHead">
              <div class="muted">Umidade do solo</div>
              <div class="chip">Sensor futuro</div>
            </div>
            <div class="sensorVal"><span id="humVal">—</span>%</div>
            <div class="bar"><i id="humBar" style="width:50%"></i></div>
            <div class="small" style="margin-top:10px">Faixa ideal (configurável): 35% ~ 55%</div>
          </div>

          <div class="sensor">
            <div class="sensorHead">
              <div class="muted">Recomendação</div>
              <div class="chip">IA local</div>
            </div>
            <div class="sensorVal" style="font-size:18px; font-weight:900" id="humHint">—</div>
            <div class="small" style="margin-top:10px">
              No futuro, isso pode acionar irrigação automática por limite mínimo.
            </div>
          </div>
        </div>
      </div>

      <div class="card span-6">
        <div class="titleRow">
          <h3>🧪 Sensor de Pressão</h3>
          <span class="pill" id="pressState">Normal</span>
        </div>

        <div class="sensorGrid">
          <div class="sensor">
            <div class="sensorHead">
              <div class="muted">Pressão da linha</div>
              <div class="chip">Sensor futuro</div>
            </div>
            <div class="sensorVal"><span id="pressVal">—</span> bar</div>
            <div class="bar"><i id="pressBar" style="width:50%"></i></div>
            <div class="small" style="margin-top:10px">Alerta (configurável): abaixo de 1.2 bar</div>
          </div>

          <div class="sensor">
            <div class="sensorHead">
              <div class="muted">Diagnóstico</div>
              <div class="chip">Proteção</div>
            </div>
            <div class="sensorVal" style="font-size:18px; font-weight:900" id="pressHint">—</div>
            <div class="small" style="margin-top:10px">
              No futuro, se cair a pressão, o sistema pode desligar tudo e avisar.
            </div>
          </div>
        </div>
      </div>

      <!-- LOG -->
      <div class="card span-12">
        <div class="titleRow">
          <h3>🧾 Log de Eventos</h3>
          <span class="pill">Tempo real (simulado)</span>
        </div>
        <p class="muted" style="margin-top:8px">Tudo que acontecer (liga/desliga/alertas) aparece aqui.</p>

        <div class="log" id="log"></div>

        <div style="margin-top:12px; display:flex; gap:10px; flex-wrap:wrap">
          <button class="btn" id="clearLog">Limpar log</button>
          <a class="btn ghost" href="index.html">⬅ Voltar para Central</a>
        </div>
      </div>

    </section>

    <div class="small" style="margin-top:18px; opacity:.85">
      Próximo passo: conectar no ESP32 de irrigação e trocar “simulação” por dados reais.
    </div>

  </div>

<script>
  // ======= ESTADO (simulação agora) =======
  const state = {
    mode: "AUTO",
    zones: [
      { id: 1, name: "Zona 1 • Grama / Jardim", on: false, minutes: 10 },
      { id: 2, name: "Zona 2 • Microaspersores", on: false, minutes: 12 },
      { id: 3, name: "Zona 3 • Horta", on: false, minutes: 8 },
      { id: 4, name: "Zona 4 • Reserva", on: false, minutes: 6 },
    ],
    hum: 44,       // %
    pressure: 1.8  // bar
  };

  const zonesWrap = document.getElementById("zones");
  const logEl = document.getElementById("log");

  const modeEl = document.getElementById("mode");
  const zonesOnEl = document.getElementById("zonesOn");
  const humKpi = document.getElementById("humKpi");
  const pressKpi = document.getElementById("pressKpi");

  const humVal = document.getElementById("humVal");
  const humBar = document.getElementById("humBar");
  const humState = document.getElementById("humState");
  const humHint = document.getElementById("humHint");

  const pressVal = document.getElementById("pressVal");
  const pressBar = document.getElementById("pressBar");
  const pressState = document.getElementById("pressState");
  const pressHint = document.getElementById("pressHint");

  document.getElementById("zCount").textContent = state.zones.length;

  function nowStr(){
    const d = new Date();
    return d.toLocaleTimeString([], {hour:"2-digit", minute:"2-digit", second:"2-digit"});
  }
  function pushLog(msg){
    const item = document.createElement("div");
    item.className = "logItem";
    item.innerHTML = `<div>${msg}</div><div class="time">${nowStr()}</div>`;
    logEl.prepend(item);
    // limite
    if(logEl.children.length > 30) logEl.removeChild(logEl.lastChild);
  }

  function zoneCard(z){
    const onClass = z.on ? "on" : "off";
    const stateText = z.on ? "ATIVA" : "DESLIGADA";
    return `
      <div class="zone" data-zone="${z.id}">
        <div class="zoneTop">
          <div class="zoneName">💦 ${z.name}</div>
          <div class="zoneState ${onClass}">${stateText}</div>
        </div>
        <div class="small" style="margin-top:8px">Duração padrão: <b>${z.minutes} min</b></div>
        <div class="zoneActions">
          <button class="btn primary" data-action="on">Ligar</button>
          <button class="btn danger" data-action="off">Desligar</button>
          <button class="btn" data-action="pulse">Pulso 30s</button>
        </div>
      </div>
    `;
  }

  function renderZones(){
    zonesWrap.innerHTML = state.zones.map(zoneCard).join("");
  }

  function computeSensors(){
    // umidade
    humVal.textContent = state.hum.toFixed(0);
    humBar.style.width = Math.max(0, Math.min(100, state.hum)) + "%";
    humKpi.textContent = state.hum.toFixed(0) + "%";

    if(state.hum < 35){
      humState.textContent = "Baixa";
      humState.style.borderColor = "rgba(230,184,76,.55)";
      humState.style.background = "rgba(230,184,76,.12)";
      humHint.textContent = "Umidade baixa — recomendado irrigar.";
    } else if(state.hum > 65){
      humState.textContent = "Alta";
      humState.style.borderColor = "rgba(85,214,190,.55)";
      humState.style.background = "rgba(85,214,190,.12)";
      humHint.textContent = "Umidade alta — evite irrigação.";
    } else {
      humState.textContent = "Normal";
      humState.style.borderColor = "rgba(46,229,157,.45)";
      humState.style.background = "rgba(46,229,157,.10)";
      humHint.textContent = "Umidade boa — sistema estável.";
    }

    // pressão
    pressVal.textContent = state.pressure.toFixed(2);
    pressBar.style.width = Math.max(0, Math.min(100, (state.pressure/3)*100)) + "%";
    pressKpi.textContent = state.pressure.toFixed(2) + " bar";

    if(state.pressure < 1.2){
      pressState.textContent = "Baixa";
      pressState.style.borderColor = "rgba(255,77,77,.55)";
      pressState.style.background = "rgba(255,77,77,.12)";
      pressHint.textContent = "Pressão baixa — possível vazamento/entupimento/bomba.";
    } else {
      pressState.textContent = "Normal";
      pressState.style.borderColor = "rgba(46,229,157,.45)";
      pressState.style.background = "rgba(46,229,157,.10)";
      pressHint.textContent = "Pressão normal — operação segura.";
    }
  }

  function renderKPIs(){
    modeEl.textContent = state.mode;
    const onCount = state.zones.filter(z=>z.on).length;
    zonesOnEl.textContent = onCount;
  }

  function setMode(m){
    state.mode = m;
    pushLog(`Modo alterado: <b>${m}</b>`);
    renderKPIs();
  }

  // ações globais
  document.getElementById("allOn").onclick = ()=>{
    state.zones.forEach(z=>z.on=true);
    pushLog("Ação global: <b>LIGAR TUDO</b>");
    renderZones(); renderKPIs();
  };
  document.getElementById("allOff").onclick = ()=>{
    state.zones.forEach(z=>z.on=false);
    pushLog("Ação global: <b>DESLIGAR TUDO</b>");
    renderZones(); renderKPIs();
  };
  document.getElementById("auto").onclick = ()=>setMode("AUTO");
  document.getElementById("manual").onclick = ()=>setMode("MANUAL");

  // agenda (simulação)
  document.getElementById("simRun").onclick = ()=>{
    pushLog("Agenda: executando rotina simulada...");
    // liga zona 1 por 2s, depois desliga (só visual)
    const z1 = state.zones[0];
    z1.on = true;
    renderZones(); renderKPIs();
    setTimeout(()=>{
      z1.on = false;
      renderZones(); renderKPIs();
      pushLog("Agenda: finalizada (simulação).");
    }, 2000);
  };

  // ações por zona
  zonesWrap.addEventListener("click", (ev)=>{
    const btn = ev.target.closest("button[data-action]");
    if(!btn) return;
    const zoneEl = ev.target.closest(".zone");
    const id = Number(zoneEl.dataset.zone);
    const z = state.zones.find(x=>x.id===id);
    const act = btn.dataset.action;

    if(act==="on"){ z.on=true; pushLog(`Zona ${id}: <b>LIGAR</b>`); }
    if(act==="off"){ z.on=false; pushLog(`Zona ${id}: <b>DESLIGAR</b>`); }
    if(act==="pulse"){
      pushLog(`Zona ${id}: <b>PULSO 30s</b>`);
      z.on=true; renderZones(); renderKPIs();
      setTimeout(()=>{ z.on=false; renderZones(); renderKPIs(); pushLog(`Zona ${id}: pulso concluído.`); }, 1200);
      return;
    }

    renderZones(); renderKPIs();
  });

  // limpar log
  document.getElementById("clearLog").onclick = ()=>{
    logEl.innerHTML = "";
    pushLog("Log limpo.");
  };

  // simula sensores variando
  setInterval(()=>{
    // umidade cai devagar se zonas off; sobe se alguma zona on
    const anyOn = state.zones.some(z=>z.on);
    state.hum += anyOn ? (Math.random()*1.2) : -(Math.random()*0.8);
    state.hum = Math.max(10, Math.min(90, state.hum));

    // pressão cai se muitas zonas on (simulação)
    const onCount = state.zones.filter(z=>z.on).length;
    state.pressure = 2.0 - (onCount*0.25) + (Math.random()*0.10 - 0.05);
    state.pressure = Math.max(0.6, Math.min(2.6, state.pressure));

    computeSensors();
    // alerta no log se pressão baixa
    if(state.pressure < 1.2){
      pushLog("⚠️ Alerta: <b>Pressão baixa</b> detectada.");
    }
  }, 4500);

  // init
  renderZones();
  renderKPIs();
  computeSensors();
  pushLog("Painel de irrigação iniciado (simulação).");
</script>
</body>
</html>

<!doctype html>
<html lang="pt-br">
<head>
  <meta charset="utf-8"/>
  <meta name="viewport" content="width=device-width,initial-scale=1"/>
  <title>Segurança • G MÁQUINA</title>
  <link rel="stylesheet" href="css/theme.css"/>

  <style>
    .span-12{grid-column:span 12}
    .span-8{grid-column:span 8}
    .span-6{grid-column:span 6}
    .span-4{grid-column:span 4}

    .titleRow{display:flex; align-items:center; justify-content:space-between; gap:10px; flex-wrap:wrap}
    .titleRow h3{margin:0}

    .sensorGrid{display:grid; grid-template-columns: repeat(12, 1fr); gap:12px; margin-top:12px}
    .sensor{
      grid-column: span 6;
      border-radius: var(--radius);
      border:1px solid rgba(255,255,255,.12);
      background: rgba(0,0,0,.18);
      padding:12px;
      position:relative;
      overflow:hidden;
    }
    .sensor:before{
      content:"";
      position:absolute; inset:-90px;
      background: radial-gradient(420px 200px at 85% 35%, rgba(230,184,76,.10), transparent 60%);
      pointer-events:none;
    }
    .sensor > *{position:relative}
    .sHead{display:flex; align-items:center; justify-content:space-between; gap:10px}
    .sVal{font-size:20px; font-weight:950; margin-top:8px}
    .chip{
      display:inline-flex; align-items:center; gap:6px;
      padding:6px 10px; border-radius:999px;
      border:1px solid rgba(255,255,255,.14);
      background: rgba(0,0,0,.22);
      font-size:12px;
    }
    .state{
      padding:6px 10px; border-radius:999px;
      border:1px solid rgba(255,255,255,.14);
      background: rgba(0,0,0,.22);
      font-size:12px;
    }
    .state.ok{border-color: rgba(46,229,157,.45); background: rgba(46,229,157,.10)}
    .state.warn{border-color: rgba(230,184,76,.55); background: rgba(230,184,76,.12)}
    .state.bad{border-color: rgba(255,77,77,.55); background: rgba(255,77,77,.12)}

    .log{
      margin-top:12px;
      border-radius: var(--radius);
      border:1px solid rgba(255,255,255,.12);
      background: rgba(0,0,0,.16);
      overflow:hidden;
    }
    .logItem{
      padding:10px 12px;
      display:flex; justify-content:space-between; gap:10px; flex-wrap:wrap;
      border-top:1px solid rgba(255,255,255,.08);
      font-size:13px;
    }
    .logItem:first-child{border-top:none}
    .time{color:var(--muted)}

    .switch{
      display:flex; align-items:center; justify-content:space-between; gap:12px;
      padding:12px;
      border-radius: var(--radius);
      border:1px solid rgba(255,255,255,.12);
      background: rgba(0,0,0,.18);
      margin-top:12px;
    }
    .toggle{
      width:54px; height:30px; border-radius:999px;
      border:1px solid rgba(255,255,255,.16);
      background: rgba(255,255,255,.06);
      position:relative; cursor:pointer;
      transition:.18s ease;
    }
    .toggle i{
      position:absolute; top:50%; transform:translateY(-50%);
      left:4px; width:22px; height:22px; border-radius:999px;
      background: rgba(255,255,255,.85);
      transition:.18s ease;
    }
    .toggle.on{
      border-color: rgba(85,214,190,.55);
      background: rgba(85,214,190,.14);
    }
    .toggle.on i{left:28px; background: rgba(46,229,157,.95)}
    @media (max-width: 980px){
      .span-8,.span-6,.span-4{grid-column:span 12}
      .sensor{grid-column:span 12}
    }
  </style>
</head>
<body>

  <div class="topbar">
    <div class="topbar-inner">
      <div class="brand">
        <img src="assets/logo-gmaquina.png" alt="G MÁQUINA">
        <div>
          <div class="t1">G MÁQUINA • Segurança</div>
          <div class="t2">Perímetro • sensores • alarmes</div>
        </div>
      </div>

      <div class="nav">
        <a href="index.html">Central</a>
        <a href="irrigacao.html">Irrigação</a>
        <a class="active" href="seguranca.html">Segurança</a>
        <a href="iluminacao.html">Iluminação</a>
      </div>
    </div>
  </div>

  <div class="container">

    <section class="hero">
      <h1 class="h-title">Segurança & Perímetro</h1>
      <p class="h-sub">
        Controle do modo de segurança, sensores e respostas automáticas.
        Hoje em simulação — depois ligamos no ESP32 de segurança.
      </p>
      <div class="badges">
        <div class="badge"><span class="dot warn"></span><span>Modo: Simulação</span></div>
        <div class="badge"><span class="dot"></span><span>Módulo: Online</span></div>
        <div class="badge"><span class="dot"></span><span>Log de eventos ativo</span></div>
      </div>
    </section>

    <section class="grid">

      <!-- STATUS / CONTROLE -->
      <div class="card span-8">
        <div class="titleRow">
          <h3>🛡️ Status do Sistema</h3>
          <span class="pill" id="pill">DESARMADO</span>
        </div>

        <div class="kpis">
          <div class="kpi">Modo: <b id="mode">DESARMADO</b></div>
          <div class="kpi">Sirene: <b id="sirene">OFF</b></div>
          <div class="kpi">Último evento: <b id="lastEvt">—</b></div>
        </div>

        <div style="margin-top:12px; display:flex; gap:10px; flex-wrap:wrap">
          <button class="btn primary" id="arm">Armar</button>
          <button class="btn danger" id="disarm">Desarmar</button>
          <button class="btn" id="panic">Pânico (sirene)</button>
          <button class="btn ghost" id="silence">Silenciar</button>
        </div>

        <div class="switch">
          <div>
            <div style="font-weight:900">Modo Noturno</div>
            <div class="small">Aumenta sensibilidade e habilita ações automáticas (simulado)</div>
          </div>
          <div class="toggle" id="night"><i></i></div>
        </div>
      </div>

      <!-- AÇÕES AUTOMÁTICAS -->
      <div class="card span-4">
        <div class="titleRow">
          <h3>⚙️ Automação</h3>
          <span class="pill">Regras</span>
        </div>
        <p class="small" style="margin-top:8px">
          Exemplo: se “presença” + sistema armado → sirene + notificação (no futuro).
        </p>

        <div style="margin-top:12px; display:grid; gap:10px">
          <div class="chip">✅ Armado + Presença → Sirene</div>
          <div class="chip">✅ Porta aberta → Evento no Log</div>
          <div class="chip">✅ Feixe/cerca → Alerta</div>
        </div>

        <div style="margin-top:12px; display:flex; gap:10px; flex-wrap:wrap">
          <button class="btn" id="simEvent">Simular invasão</button>
        </div>
      </div>

      <!-- SENSORES -->
      <div class="card span-12">
        <div class="titleRow">
          <h3>📟 Sensores</h3>
          <span class="pill">Tempo real (simulado)</span>
        </div>

        <div class="sensorGrid">
          <div class="sensor">
            <div class="sHead">
              <div class="chip">🚪 Porta / Portão</div>
              <span class="state ok" id="stDoor">Fechado</span>
            </div>
            <div class="sVal" id="vDoor">Sem alteração</div>
            <div class="small" style="margin-top:10px">Quando abrir, registra no log e pode acionar regra.</div>
          </div>

          <div class="sensor">
            <div class="sHead">
              <div class="chip">👣 Presença (PIR)</div>
              <span class="state ok" id="stPir">Normal</span>
            </div>
            <div class="sVal" id="vPir">Nenhuma presença</div>
            <div class="small" style="margin-top:10px">Se armado, pode disparar sirene.</div>
          </div>

          <div class="sensor">
            <div class="sHead">
              <div class="chip">📡 Feixe / Cerca</div>
              <span class="state ok" id="stFence">OK</span>
            </div>
            <div class="sVal" id="vFence">Sem violação</div>
            <div class="small" style="margin-top:10px">Ideal para perímetro e entrada.</div>
          </div>

          <div class="sensor">
            <div class="sHead">
              <div class="chip">🔋 Energia / Bateria</div>
              <span class="state warn" id="stPwr">Monitor</span>
            </div>
            <div class="sVal" id="vPwr">Rede normal</div>
            <div class="small" style="margin-top:10px">No futuro: alerta se cair energia.</div>
          </div>
        </div>
      </div>

      <!-- LOG -->
      <div class="card span-12">
        <div class="titleRow">
          <h3>🧾 Log de Eventos</h3>
          <span class="pill">Registrando</span>
        </div>

        <div class="log" id="log"></div>

        <div style="margin-top:12px; display:flex; gap:10px; flex-wrap:wrap">
          <button class="btn" id="clear">Limpar log</button>
          <a class="btn ghost" href="index.html">⬅ Voltar</a>
        </div>
      </div>

    </section>

    <div class="small" style="margin-top:18px; opacity:.85">
      Próximo bloco: Iluminação “top” com setores, cenas e automação.
    </div>

  </div>

<script>
  const st = {
    armed:false,
    siren:false,
    night:false,
    doorClosed:true,
    pir:false,
    fenceOk:true,
    powerOk:true,
    last:"—"
  };

  const pill = document.getElementById("pill");
  const mode = document.getElementById("mode");
  const sir  = document.getElementById("sirene");
  const last = document.getElementById("lastEvt");

  const stDoor = document.getElementById("stDoor");
  const vDoor  = document.getElementById("vDoor");

  const stPir = document.getElementById("stPir");
  const vPir  = document.getElementById("vPir");

  const stFence = document.getElementById("stFence");
  const vFence  = document.getElementById("vFence");

  const stPwr = document.getElementById("stPwr");
  const vPwr  = document.getElementById("vPwr");

  const logEl = document.getElementById("log");
  const nightToggle = document.getElementById("night");

  function nowStr(){
    const d = new Date();
    return d.toLocaleTimeString([], {hour:"2-digit", minute:"2-digit", second:"2-digit"});
  }
  function pushLog(msg){
    st.last = msg.replace(/<[^>]*>/g,"");
    last.textContent = st.last;
    const item = document.createElement("div");
    item.className = "logItem";
    item.innerHTML = `<div>${msg}</div><div class="time">${nowStr()}</div>`;
    logEl.prepend(item);
    if(logEl.children.length>40) logEl.removeChild(logEl.lastChild);
  }

  function setStateBadge(el, kind, text){
    el.className = "state " + kind;
    el.textContent = text;
  }

  function render(){
    pill.textContent = st.armed ? "ARMADO" : "DESARMADO";
    pill.style.borderColor = st.armed ? "rgba(46,229,157,.45)" : "rgba(255,255,255,.14)";
    pill.style.background  = st.armed ? "rgba(46,229,157,.10)" : "rgba(0,0,0,.22)";
    mode.textContent = st.armed ? "ARMADO" : "DESARMADO";
    sir.textContent  = st.siren ? "ON" : "OFF";

    nightToggle.classList.toggle("on", st.night);

    // Door
    setStateBadge(stDoor, st.doorClosed ? "ok" : (st.armed ? "bad" : "warn"), st.doorClosed ? "Fechado" : "Aberto");
    vDoor.textContent = st.doorClosed ? "Sem alteração" : "Porta/portão aberto";

    // PIR
    setStateBadge(stPir, st.pir ? (st.armed ? "bad" : "warn") : "ok", st.pir ? "Movimento" : "Normal");
    vPir.textContent = st.pir ? "Presença detectada" : "Nenhuma presença";

    // Fence
    setStateBadge(stFence, st.fenceOk ? "ok" : (st.armed ? "bad" : "warn"), st.fenceOk ? "OK" : "Violado");
    vFence.textContent = st.fenceOk ? "Sem violação" : "Perímetro violado";

    // Power
    setStateBadge(stPwr, st.powerOk ? "warn" : "bad", st.powerOk ? "Monitor" : "Falha");
    vPwr.textContent = st.powerOk ? "Rede normal" : "Energia caiu";
  }

  function arm(){
    st.armed = true;
    pushLog("✅ Sistema <b>ARMADO</b>.");
    render();
  }
  function disarm(){
    st.armed = false;
    st.siren = false;
    pushLog("🔓 Sistema <b>DESARMADO</b>.");
    render();
  }
  function panic(){
    st.siren = true;
    pushLog("🚨 <b>PÂNICO</b>: sirene ativada.");
    render();
  }
  function silence(){
    st.siren = false;
    pushLog("🔕 Sirene silenciada.");
    render();
  }

  document.getElementById("arm").onclick = arm;
  document.getElementById("disarm").onclick = disarm;
  document.getElementById("panic").onclick = panic;
  document.getElementById("silence").onclick = silence;

  nightToggle.onclick = ()=>{
    st.night = !st.night;
    pushLog(st.night ? "🌙 Modo noturno <b>ATIVADO</b>." : "☀️ Modo noturno <b>DESATIVADO</b>.");
    render();
  };

  document.getElementById("simEvent").onclick = ()=>{
    // Simula um evento de invasão
    st.pir = true;
    st.fenceOk = false;
    st.doorClosed = Math.random() > 0.5 ? false : st.doorClosed;
    pushLog("⚠️ Evento: <b>atividade suspeita</b> no perímetro.");
    if(st.armed){
      st.siren = true;
      pushLog("🚨 Regra: sistema armado → <b>sirene ON</b>.");
    }
    render();
    setTimeout(()=>{
      st.pir = false;
      st.fenceOk = true;
      st.doorClosed = true;
      render();
      pushLog("✅ Perímetro normalizado (simulação).");
    }, 2500);
  };

  document.getElementById("clear").onclick = ()=>{
    logEl.innerHTML = "";
    pushLog("Log limpo.");
  };

  // Simulações leves automáticas
  setInterval(()=>{
    // chance pequena de evento de porta
    if(Math.random() < 0.18){
      st.doorClosed = !st.doorClosed;
      pushLog(st.doorClosed ? "🚪 Porta fechou." : "🚪 Porta abriu.");
      if(!st.doorClosed && st.armed){
        st.siren = st.night ? true : st.siren;
        pushLog("⚠️ Armado: porta aberta registrada.");
        if(st.night){
          pushLog("🚨 Modo noturno: sirene ativada.");
        }
      }
      render();
    }
  }, 6000);

  // init
  pushLog("Painel de segurança iniciado (simulação).");
  render();
</script>
</body>
</html>
