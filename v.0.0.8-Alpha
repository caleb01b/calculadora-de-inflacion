<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Calculadora de Inflación — v0.0.8-Alpha</title>
  <link rel="preconnect" href="https://cdn.jsdelivr.net" crossorigin>
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.3/dist/chart.umd.min.js"></script>
  <style>
    :root{
      --bg: #0b0f1a;
      --panel: #111827ee;
      --panel-2: #0f172add;
      --card: #0b1220f2;
      --text: #e5e7eb;
      --muted:#9ca3af;
      --primary:#22d3ee;   /* Cyan - Escenario A */
      --secondary:#f97316; /* Naranja - Escenario B */
      --accent:#a78bfa;
      --ok:#10b981;
      --warn:#f59e0b;
      --err:#ef4444;
      --ring:#22d3ee55;
      --border:#1f2937;
      --shadow: 0 10px 25px rgba(0,0,0,.45);
      --radius: 16px;
      --radius-lg: 22px;
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, Segoe UI, Roboto, "Helvetica Neue", Arial, sans-serif;
      color:var(--text);
      background:
        radial-gradient(1200px 600px at 10% -10%, #0ea5e922, transparent 60%),
        radial-gradient(1200px 600px at 90% -10%, #7c3aed22, transparent 60%),
        linear-gradient(180deg, #0a0f1c 0%, #060912 100%);
      background-attachment: fixed;
    }
    .wrap{ max-width: 1200px; margin: 24px auto; padding: clamp(12px, 2vw, 24px); }

    /* Header */
    .header{
      display:flex; align-items:center; justify-content:space-between; gap:12px; padding:14px 18px;
      background: linear-gradient(180deg, var(--panel), var(--panel-2));
      border:1px solid var(--border); border-radius: var(--radius-lg);
      box-shadow: var(--shadow); backdrop-filter: blur(6px);
    }
    .brand{display:flex; align-items:center; gap:12px; font-weight:700; letter-spacing:.3px;}
    .brand .logo{width:36px; height:36px; border-radius:10px; background:linear-gradient(135deg, var(--primary), var(--accent)); box-shadow:0 0 0 4px #0006 inset}
    .version{font-size:12px; padding:4px 10px; border-radius: 999px; border:1px solid var(--border); background:#0a1220aa; color:#c7d2fe}

    .lang-control{ display:flex; align-items:center; gap:6px; }
    .lang-control label{ margin:0; font-size:12px; color:var(--muted); white-space:nowrap; }
    .lang-control select{ width:auto; min-width:118px; padding:6px 9px; border-radius:999px; font-size:12px; background:#0a1220aa; color:#e5e7eb; }
    @media (max-width: 640px){ .header{align-items:flex-start; flex-direction:column;} .d-flex{flex-wrap:wrap;} }

    /* FX Controls in Header */
    .fx-container { display: flex; flex-direction: column; justify-content: center; }
    .fx-row { display: flex; align-items: center; gap: 8px; margin-top: 2px; }
    .fx { font-size:13px; color:var(--muted); font-variant-numeric: tabular-nums; }
    .btn-icon {
      background: transparent; border: 1px solid var(--border); color: var(--primary);
      border-radius: 6px; width: 24px; height: 24px; display: flex; align-items: center; justify-content: center;
      cursor: pointer; transition: all .2s; padding: 0;
    }
    .btn-icon:hover { background: #1e293b; color: #fff; border-color: var(--muted); }
    .btn-icon:active { transform: scale(0.95); }
    .spin { animation: spin 1s linear infinite; pointer-events: none; opacity: 0.7; }
    @keyframes spin { 100% { transform: rotate(360deg); } }

    /* Layout */
    .grid{ display:grid; gap:18px; margin-top:18px; grid-template-columns: 1fr; }
    @media (min-width: 980px){ .grid{ grid-template-columns: 420px 1fr; align-items:start; } }

    /* Cards & Inputs */
    .card{
      background:linear-gradient(180deg, var(--card), #0a0f1edd);
      border:1px solid var(--border); border-radius: var(--radius);
      padding:16px; box-shadow: var(--shadow); position:relative;
    }
    h2{margin:0 0 10px; font-size:clamp(18px, 2.4vw, 22px)}
    h3{margin:0 0 8px; font-size:16px; color:var(--muted)}

    .row{display:grid; grid-template-columns: 1fr 90px; gap:10px; margin: 10px 0 6px;}
    .row-3{display:grid; grid-template-columns: 1fr 1fr 120px; gap:10px; margin: 10px 0 12px;}

    label{font-size:13px; color:#cbd5e1; display:block; margin-bottom:4px}
    input, select{
      width:100%; padding:10px 12px; border-radius:10px;
      border:1px solid var(--border); outline:none;
      background:#0b1220; color:var(--text); font-size:14px;
      transition: box-shadow .2s, border-color .2s;
    }
    input:focus, select:focus{ box-shadow: 0 0 0 3px var(--ring); border-color:#334155 }

    /* Buttons */
    .actions{display:flex; gap:8px; flex-wrap:wrap; margin-top:10px}
    .btn{
      border:1px solid #1f2a44; background:#0d2235; color:#dbeafe;
      padding:10px 14px; border-radius:10px; font-weight:600; cursor:pointer; font-size:13px;
      display:inline-flex; align-items:center; gap:6px; transition: filter .2s;
      user-select: none;
    }
    .btn:hover{ border-color:#26314d; filter:brightness(1.15) }
    .btn:active{ transform:translateY(1px); }
    .btn-primary{ background: linear-gradient(180deg, #0ea5e9, #0284c7); border-color:#0ea5e9; color:#001421 }
    .btn-sec{ background:#1e293b; color:#94a3b8 }
    .btn:disabled{ opacity:.5; cursor:not-allowed; filter:grayscale(1) }
    .btn-sm{ padding: 6px 10px; font-size:12px; }

    /* Escenarios */
    .sc-header{ display:flex; justify-content:space-between; align-items:center; cursor:pointer; user-select:none;}
    .sc-header:hover .caret{ color:var(--text); }
    .caret{ transition: transform .2s; color:var(--muted); }
    .collapsed .caret{ transform: rotate(-90deg); }
    .sc-body{ transition: height .3s ease; overflow:hidden; }
    .collapsed .sc-body{ display:none; }

    .border-b-sect{ border-left: 3px solid var(--secondary); padding-left: 12px; margin-top:12px; }
    .badge-ab{ background: var(--secondary); color:#000; font-size:11px; padding:2px 6px; border-radius:4px; font-weight:800; margin-left:6px; }

    /* Stats */
    .stats{ display:grid; gap:10px; grid-template-columns: 1fr; margin-top:6px }
    @media (min-width: 720px){ .stats{ grid-template-columns: repeat(3, 1fr) } }
    .stat{ padding:12px; border:1px dashed #1f2937; border-radius:12px; background:#0b1220aa; position:relative; }
    .stat b{ display:block; font-size:13px; color:#93c5fd; margin-bottom:4px }
    .stat-val{ font-size:18px; font-weight:700; letter-spacing:.5px; }
    .stat-sub{ font-size:12px; color:var(--muted); margin-top:2px; }
    .diff-badge{ position:absolute; top:8px; right:8px; font-size:10px; padding:2px 6px; border-radius:4px; background:#334155; color:#e2e8f0; }
    .diff-pos{ background:#064e3b; color:#6ee7b7; }
    .diff-neg{ background:#450a0a; color:#fca5a5; }

    /* Chart */
    .chart-card{ padding:12px; width:100%; min-height: 300px; }
    .chart-wrap{ width:100%; height:280px; position:relative; }

    /* Utilities */
    .help{font-size:11px; color:var(--muted); margin-top:4px}
    .sr-only{position:absolute; width:1px; height:1px; overflow:hidden; opacity:0}
    .d-flex{display:flex; align-items:center; gap:8px;}
    .justify-between{justify-content:space-between}

    /* Modal Cesta */
    dialog{
      background: #0f172a; color:var(--text); border:1px solid #334155; border-radius:16px;
      padding:20px; max-width:500px; width:90%; box-shadow: 0 20px 50px #0009;
    }
    dialog::backdrop{ background: #0008; backdrop-filter:blur(2px); }
    .basket-table{ width:100%; border-collapse:collapse; font-size:13px; margin:10px 0; }
    .basket-table th{ text-align:left; color:var(--muted); padding:8px; border-bottom:1px solid #334155; }
    .basket-table td{ padding:8px; border-bottom:1px solid #1e293b; }
    .basket-row input{ padding:6px; background:transparent; border:1px solid #334155; width:100%; color:white; border-radius:6px; }
    .basket-total{ margin-top:10px; padding-top:10px; border-top:1px solid #334155; display:flex; justify-content:space-between; font-weight:bold; }

    /* ===== PORTADA (Reporte) ===== */
    .report-cover{ display:none; margin-top: 18px; }
    .report-cover.show{ display:block; }
    .report-bg { background:#0b0f1a; border:1px solid #1f2937; border-radius:16px; padding:32px; position:relative; overflow:hidden; }

    .rc-header{ display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:24px; position:relative; z-index:2; }
    .rc-title{ font-size:32px; font-weight:800; margin:0; color:#fff; }
    .rc-sub{ color:#a78bfa; margin:0; font-size:14px; }
    .rc-pill{ background:#1e293b; color:#e2e8f0; padding:6px 12px; border-radius:99px; font-size:12px; border:1px solid #334155; }
    .rc-grid{ display:grid; grid-template-columns: repeat(3,1fr); gap:16px; margin-bottom:24px; position:relative; z-index:2; }
    .rc-card{ background:#111827; border:1px solid #1f2937; border-radius:12px; padding:16px; }
    .rc-lbl{ font-size:12px; color:#9ca3af; margin-bottom:4px; text-transform:uppercase; letter-spacing:0.5px; }
    .rc-val{ font-size:24px; font-weight:700; color:#fff; }
    .rc-val.cyan{ color:#22d3ee; text-shadow:0 0 10px #22d3ee44; }
    .rc-val.orange{ color:#f97316; text-shadow:0 0 10px #f9731644; }
    .rc-chart-box{ background:#0f172a; border-radius:12px; padding:16px; border:1px solid #1f2937; height:340px; margin-bottom:16px; position:relative; z-index:2; }

    /* Glow FX */
    .glow-blob{ position:absolute; width:400px; height:400px; border-radius:50%; filter:blur(80px); opacity:0.15; pointer-events:none; }
    .glow-1{ background:#22d3ee; top:-100px; left:-100px; }
    .glow-2{ background:#a78bfa; bottom:-100px; right:-100px; }

    /* Print styles */
    @media print {
      body * { visibility: hidden; }
      #reportCover, #reportCover * { visibility: visible; }
      #reportCover { position: absolute; left: 0; top: 0; width: 100%; margin:0; }
      .rc-toolbar, .wrap > header, .wrap > .grid { display: none !important; }
      .report-bg { background: #0b0f1a !important; -webkit-print-color-adjust: exact; print-color-adjust: exact; }
    }
  </style>
</head>
<body>

<div class="wrap">
  <header class="header" role="banner">
    <div class="brand">
      <div class="logo"></div>
      <div class="fx-container">
        <div style="line-height:1.2; font-weight:700"><span data-i18n="appTitle">Calculadora de Inflación</span></div>
        <div class="fx-row">
          <div class="fx" id="fxLabel">1 USD ≈ ...</div>
          <button class="btn-icon" id="btnRefreshFX" title="Actualizar Tasa (API)" aria-label="Actualizar tasa de cambio">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M23 4v6h-6"></path><path d="M1 20v-6h6"></path><path d="M3.51 9a9 9 0 0 1 14.85-3.36L23 10M1 14l4.64 4.36A9 9 0 0 0 20.49 15"></path></svg>
          </button>
        </div>
      </div>
    </div>
    <div class="d-flex">
      <div class="lang-control">
        <label for="langSelect" data-i18n="languageLabel">Idioma</label>
        <select id="langSelect" aria-label="Seleccionar idioma">
          <option value="es">Español</option>
          <option value="en">English</option>
          <option value="pt-BR">Português BR</option>
        </select>
      </div>
      <div class="version">v0.0.8-Alpha</div>
    </div>
  </header>

  <main class="grid" id="mainApp" role="main">

    <section class="card">
      <div class="d-flex justify-between">
        <h2 data-i18n="parameters">Parámetros</h2>
        <button class="btn btn-sm btn-sec" id="btnClear" title="Reiniciar campos" data-i18n="clear">Limpiar</button>
      </div>

      <div id="scenarioA">
        <h3 data-i18n="scenarioA">Escenario A (Principal)</h3>
        <div class="row">
          <div>
            <label for="p0_a" data-i18n="initialPrice">Precio Inicial</label>
            <input id="p0_a" inputmode="decimal" placeholder="0.00" class="live-input">
            <div class="help" id="eq_p0_a">Eq: —</div>
          </div>
          <div>
            <label for="c0_a" data-i18n="currency">Moneda</label>
            <select id="c0_a" class="live-input"><option value="BRL">BRL</option><option value="USD">USD</option></select>
          </div>
        </div>
        <div class="row">
          <div>
            <label for="p1_a" data-i18n="finalPrice">Precio Final</label>
            <input id="p1_a" inputmode="decimal" placeholder="0.00" class="live-input">
            <div class="help" id="eq_p1_a">Eq: —</div>
          </div>
          <div>
            <label for="c1_a" data-i18n="currency">Moneda</label>
            <select id="c1_a" class="live-input"><option value="BRL">BRL</option><option value="USD">USD</option></select>
          </div>
        </div>
      </div>

      <div class="collapsed" id="groupB" style="margin-top:18px">
        <div class="sc-header" id="toggleB">
          <div class="d-flex">
            <h3 style="margin:0; color:var(--secondary)" data-i18n="scenarioB">Escenario B (Comparativa)</h3>
            <span class="badge-ab">VS</span>
          </div>
          <span class="caret">▼</span>
        </div>
        <div class="sc-body border-b-sect">
          <button type="button" id="btnCopyAtoB" class="btn btn-sm btn-sec" style="margin-bottom:10px; width:100%" data-i18n="copyFromA">Copiar de A</button>
          <div class="row">
            <div>
              <label for="p0_b" data-i18n="initialPriceB">Precio Inicial (B)</label>
              <input id="p0_b" inputmode="decimal" placeholder="Opcional">
            </div>
            <div>
              <select id="c0_b"><option value="BRL">BRL</option><option value="USD">USD</option></select>
            </div>
          </div>
          <div class="row">
            <div>
              <label for="p1_b" data-i18n="finalPriceB">Precio Final (B)</label>
              <input id="p1_b" inputmode="decimal" placeholder="Opcional">
            </div>
            <div>
              <select id="c1_b"><option value="BRL">BRL</option><option value="USD">USD</option></select>
            </div>
          </div>
        </div>
      </div>

      <hr style="border:0; border-top:1px solid var(--border); margin:16px 0">

      <div class="row-3">
        <div>
          <label for="t_val" data-i18n="time">Tiempo</label>
          <input id="t_val" inputmode="numeric" placeholder="12">
        </div>
        <div>
          <label for="t_unit" data-i18n="unit">Unidad</label>
          <select id="t_unit">
            <option value="meses" selected data-i18n="months">Meses</option>
            <option value="anios" data-i18n="years">Años</option>
            <option value="dias" data-i18n="days">Días</option>
          </select>
        </div>
        <div style="margin-top:22px">
          <button id="btnCalc" class="btn btn-primary" style="width:100%; justify-content:center;" data-i18n="calculate">Calcular</button>
        </div>
      </div>
      <div class="help" style="text-align:center" data-i18n="shortcut">Atajo: Ctrl + Enter</div>

      <div class="actions" style="margin-top:16px; border-top:1px solid var(--border); padding-top:12px">
        <button id="btnOpenBasket" class="btn btn-sec btn-sm" data-i18n="basketBtn">🧺 Cesta Personalizada</button>
        <button id="btnShare" class="btn btn-sec btn-sm" data-i18n="shareBtn">🔗 Copiar Link</button>
        <button id="btnShowCover" class="btn btn-sec btn-sm" disabled data-i18n="coverBtn">📄 Ver Portada</button>
      <!-- Nuevos botones para guardar y abrir escenarios -->
      <button id="btnSaveScenario" class="btn btn-sec btn-sm" data-i18n="saveScenarioBtn">💾 Guardar Escenario</button>
      <button id="btnOpenScenarios" class="btn btn-sec btn-sm" data-i18n="scenariosBtn">📁 Escenarios</button>
      </div>
    </section>

    <section class="card">
      <div class="d-flex justify-between">
        <h2 data-i18n="results">Resultados</h2>
        <div class="d-flex" style="gap:6px; flex-wrap:wrap;">
          <button class="btn btn-sec btn-sm" id="btnCopyRes" data-i18n="copyText">Copiar Texto</button>
          <button class="btn btn-sec btn-sm" id="btnExportCSV" data-i18n="exportCSV">⬇ CSV</button>
          <button class="btn btn-sec btn-sm" id="btnExportJSON" data-i18n="exportJSON">⬇ JSON</button>
          <label style="display:flex; align-items:center; gap:4px; font-size:13px; cursor:pointer;">
            <input type="checkbox" id="toggleExplanation" style="margin:0"> <span data-i18n="explanationToggle">Explicación</span>
          </label>
        </div>
      </div>

      <div class="stats">
        <div class="stat">
          <b data-i18n="totalInflation">Inflación Total</b>
          <div id="resTotal" class="stat-val">—</div>
          <div id="resTotalB" class="stat-sub" style="color:var(--secondary)"></div>
          <div id="badgeTotal" class="diff-badge" hidden></div>
        </div>
        <div class="stat">
          <b data-i18n="monthlyAverage">Promedio Mensual</b> <div id="resAvg" class="stat-val">—</div>
          <div id="resAvgB" class="stat-sub" style="color:var(--secondary)"></div>
        </div>
        <div class="stat">
          <b data-i18n="annualized">Anualizada (CAGR)</b>
          <div id="resAnual" class="stat-val">—</div>
          <div id="resAnualB" class="stat-sub" style="color:var(--secondary)"></div>
          <div id="badgeAnual" class="diff-badge" hidden></div>
        </div>
      </div>

      <div style="margin-top:12px; display:grid; grid-template-columns:1fr 1fr; gap:10px">
        <div class="stat" style="padding:8px 12px">
          <b style="font-size:11px; color:var(--muted)" data-i18n="doublingTime">Tiempo de duplicación</b>
          <div id="resDouble" style="font-size:14px; color:#e2e8f0">—</div>
        </div>
        <div class="stat" style="padding:8px 12px">
          <b style="font-size:11px; color:var(--muted)" data-i18n="purchasingPower">Poder de compra remanente</b>
          <div id="resPower" style="font-size:14px; color:#e2e8f0">—</div>
        </div>
      </div>

      <!-- Contenedor para la explicación paso a paso -->
      <div id="explanation" class="card" style="padding:12px; margin-top:12px; background:#0b1220aa; border:1px dashed #334155; border-radius:12px; font-size:13px; line-height:1.4; display:none;"></div>

      <div class="card chart-card" style="margin-top:16px; background:#0b101b">
        <div class="chart-wrap">
          <canvas id="mainChart"></canvas>
        </div>
      </div>
    </section>
  </main>

  <dialog id="basketDialog">
    <h3 data-i18n="basketTitle">🧺 Cesta de Inflación Ponderada</h3>
    <p class="help" data-i18n="basketHelp">Define categorías y sus pesos. La suma debe ser 100%.</p>
    <table class="basket-table">
      <thead><tr><th data-i18n="category">Categoría</th><th width="70" data-i18n="weightPct">Peso %</th><th width="70" data-i18n="inflPct">Infl. %</th><th></th></tr></thead>
      <tbody id="basketBody"></tbody>
    </table>
    <button class="btn btn-sm btn-sec" id="btnAddRow" style="width:100%" data-i18n="addRow">+ Añadir Fila</button>

    <div class="basket-total">
      <span><span data-i18n="totalWeight">Total Peso</span>: <span id="basketWeightSum">0</span>%</span>
      <span><span data-i18n="weightedInflation">Inflación Pond</span>: <span id="basketRes" style="color:var(--primary)">—</span></span>
    </div>

    <div class="actions" style="justify-content:flex-end; margin-top:20px">
      <button class="btn btn-sec btn-sm" id="btnBasketImp" data-i18n="importJSON">Importar JSON</button>
      <button class="btn btn-sec btn-sm" id="btnBasketExp" data-i18n="exportJSONText">Exportar JSON</button>
      <button class="btn btn-primary btn-sm" id="btnCloseBasket" data-i18n="saveClose">Guardar y Cerrar</button>
    </div>
    <input type="file" id="fileInput" hidden accept=".json">
  </dialog>

  <!-- Dialogo para gestionar escenarios guardados -->
  <dialog id="scenariosDialog">
    <h3 data-i18n="savedScenariosTitle">📁 Escenarios Guardados</h3>
    <p class="help" data-i18n="savedScenariosHelp">Selecciona un escenario para cargar o eliminar.</p>
    <div id="scenariosList"></div>
    <div class="actions" style="justify-content:flex-end; margin-top: 12px">
      <button class="btn btn-primary btn-sm" id="btnCloseScenarios" data-i18n="close">Cerrar</button>
    </div>
  </dialog>

  <section id="reportCover" class="report-cover">
    <div class="rc-toolbar" style="display:flex; justify-content:flex-end; gap:10px; margin-bottom:10px">
      <button class="btn btn-sec" id="btnCloseCover" data-i18n="back">Volver</button>
      <button class="btn btn-primary" id="btnExportCoverPNG" data-i18n="downloadPNG">Descargar PNG</button>
      <button class="btn btn-sec" onclick="window.print()" data-i18n="printPDF">Imprimir / PDF</button>
    </div>

    <div class="report-bg" id="reportContent">
      <div class="glow-blob glow-1"></div>
      <div class="glow-blob glow-2"></div>

      <div class="rc-header">
        <div>
          <h1 class="rc-title" data-i18n="reportTitle">Reporte de Inflación</h1>
          <p class="rc-sub"><span data-i18n="generatedOn">Generado el</span> <span id="rcDate"></span> • v0.0.8-Alpha</p>
        </div>
        <div class="rc-pill" id="rcFX">USD/BRL: ...</div>
      </div>

      <div class="rc-grid">
        <div class="rc-card">
          <div class="rc-lbl" data-i18n="scenarioATotal">Escenario A: Total</div>
          <div class="rc-val cyan" id="rcTotalA">—%</div>
        </div>
        <div class="rc-card">
          <div class="rc-lbl" data-i18n="monthlyAverage">Promedio Mensual</div>
          <div class="rc-val" id="rcAvgA">—%</div>
        </div>
        <div class="rc-card">
          <div class="rc-lbl" data-i18n="annualizedShort">Anualizada</div>
          <div class="rc-val" id="rcAnualA">—%</div>
        </div>
      </div>

      <div id="rcSectionB" style="display:none">
        <h4 style="margin:0 0 10px; color:var(--secondary); border-bottom:1px solid #334155; padding-bottom:4px"><span data-i18n="scenarioBComparison">Comparativa Escenario B</span></h4>
        <div class="rc-grid">
          <div class="rc-card">
            <div class="rc-lbl" data-i18n="scenarioBTotal">Escenario B: Total</div>
            <div class="rc-val orange" id="rcTotalB">—%</div>
          </div>
          <div class="rc-card">
            <div class="rc-lbl" data-i18n="differenceBvsA">Diferencia (B vs A)</div>
            <div class="rc-val" id="rcDiff" style="font-size:20px">—</div>
          </div>
          <div class="rc-card">
            <div class="rc-lbl" data-i18n="annualizedB">Anualizada B</div>
            <div class="rc-val" id="rcAnualB">—%</div>
          </div>
        </div>
      </div>

      <div class="rc-chart-box">
        <div class="rc-lbl" style="margin-bottom:10px" data-i18n="accumulatedProjection">Proyección de Valor Acumulado</div>
        <div id="rcChartContainer" style="width:100%; height:90%;"></div>
      </div>

      <div style="font-size:11px; color:#64748b; text-align:center; position:relative; z-index:2;">
        <span data-i18n="disclaimer">Este documento es una simulación matemática. No constituye asesoramiento financiero.</span>
      </div>
    </div>
  </section>

  <canvas id="exportCanvas" width="1920" height="1080" style="display:none"></canvas>

</div>

<script>
document.addEventListener('DOMContentLoaded', () => {
  // ========= UTILS =========
  const $ = id => document.getElementById(id);
  const num = s => {
    if(!s) return 0;
    if(typeof s === 'number') return s;
    return parseFloat(s.toString().replace(',','.').replace(/[^0-9.-]/g, '')) || 0;
  };
  const fmtCurr = (n, c) => new Intl.NumberFormat(localeForLang(),{style:'currency', currency:c}).format(n);
  const fmtPct = n => (n*100).toFixed(2) + '%';
  const I18N = {
    es: {
      appTitle:'Calculadora de Inflación', languageLabel:'Idioma', parameters:'Parámetros', clear:'Limpiar', scenarioA:'Escenario A (Principal)', initialPrice:'Precio Inicial', finalPrice:'Precio Final', currency:'Moneda', scenarioB:'Escenario B (Comparativa)', copyFromA:'Copiar de A', initialPriceB:'Precio Inicial (B)', finalPriceB:'Precio Final (B)', time:'Tiempo', unit:'Unidad', months:'Meses', years:'Años', days:'Días', calculate:'Calcular', shortcut:'Atajo: Ctrl + Enter', basketBtn:'🧺 Cesta Personalizada', shareBtn:'🔗 Copiar Link', coverBtn:'📄 Ver Portada', saveScenarioBtn:'💾 Guardar Escenario', scenariosBtn:'📁 Escenarios', results:'Resultados', copyText:'Copiar Texto', exportCSV:'⬇ CSV', exportJSON:'⬇ JSON', explanationToggle:'Explicación', totalInflation:'Inflación Total', monthlyAverage:'Promedio Mensual', annualized:'Anualizada (CAGR)', annualizedShort:'Anualizada', doublingTime:'Tiempo de duplicación', purchasingPower:'Poder de compra remanente', basketTitle:'🧺 Cesta de Inflación Ponderada', basketHelp:'Define categorías y sus pesos. La suma debe ser 100%.', addRow:'+ Añadir Fila', importJSON:'Importar JSON', exportJSONText:'Exportar JSON', saveClose:'Guardar y Cerrar', savedScenariosTitle:'📁 Escenarios Guardados', savedScenariosHelp:'Selecciona un escenario para cargar o eliminar.', close:'Cerrar', back:'Volver', downloadPNG:'Descargar PNG', printPDF:'Imprimir / PDF', reportTitle:'Reporte de Inflación', generatedOn:'Generado el', scenarioATotal:'Escenario A: Total', scenarioBComparison:'Comparativa Escenario B', scenarioBTotal:'Escenario B: Total', differenceBvsA:'Diferencia (B vs A)', annualizedB:'Anualizada B', accumulatedProjection:'Proyección de Valor Acumulado', disclaimer:'Este documento es una simulación matemática. No constituye asesoramiento financiero.', category:'Categoría', weightPct:'Peso %', inflPct:'Infl. %', totalWeight:'Total Peso', weightedInflation:'Inflación Pond', noSavedScenarios:'No hay escenarios guardados.', load:'Cargar', delete:'Eliminar', newRow:'Nuevo', invalidValues:'⚠️ Por favor ingrese valores válidos.', noExportData:'No hay resultados para exportar', scenarioName:'Nombre del escenario:', scenarioSaved:'Escenario guardado.', copied:'Copiado!', linkCopied:'Enlace copiado!', invalidJSON:'JSON inválido', basketWeightWarning:'El peso no suma 100%. ¿Guardar igual?', fxError:'No se pudo conectar a la API de tasas. Usando la última tasa guardada o el valor local.', fxUpdating:'Actualizando tasa...', fxUpdated:'Tasa actualizada', fxStale:'Última tasa guardada', originalPower:'% del original', monthsLower:'meses', yearsMoreThan100:'> 100 años', live:'Live', sim:'Sim', eq:'Eq: —'
    },
    en: {
      appTitle:'Inflation Calculator', languageLabel:'Language', parameters:'Parameters', clear:'Clear', scenarioA:'Scenario A (Main)', initialPrice:'Initial Price', finalPrice:'Final Price', currency:'Currency', scenarioB:'Scenario B (Comparison)', copyFromA:'Copy from A', initialPriceB:'Initial Price (B)', finalPriceB:'Final Price (B)', time:'Time', unit:'Unit', months:'Months', years:'Years', days:'Days', calculate:'Calculate', shortcut:'Shortcut: Ctrl + Enter', basketBtn:'🧺 Custom Basket', shareBtn:'🔗 Copy Link', coverBtn:'📄 View Cover', saveScenarioBtn:'💾 Save Scenario', scenariosBtn:'📁 Scenarios', results:'Results', copyText:'Copy Text', exportCSV:'⬇ CSV', exportJSON:'⬇ JSON', explanationToggle:'Explanation', totalInflation:'Total Inflation', monthlyAverage:'Monthly Average', annualized:'Annualized (CAGR)', annualizedShort:'Annualized', doublingTime:'Doubling time', purchasingPower:'Remaining purchasing power', basketTitle:'🧺 Weighted Inflation Basket', basketHelp:'Define categories and weights. The total must be 100%.', addRow:'+ Add Row', importJSON:'Import JSON', exportJSONText:'Export JSON', saveClose:'Save and Close', savedScenariosTitle:'📁 Saved Scenarios', savedScenariosHelp:'Select a scenario to load or delete.', close:'Close', back:'Back', downloadPNG:'Download PNG', printPDF:'Print / PDF', reportTitle:'Inflation Report', generatedOn:'Generated on', scenarioATotal:'Scenario A: Total', scenarioBComparison:'Scenario B Comparison', scenarioBTotal:'Scenario B: Total', differenceBvsA:'Difference (B vs A)', annualizedB:'Annualized B', accumulatedProjection:'Accumulated Value Projection', disclaimer:'This document is a mathematical simulation. It is not financial advice.', category:'Category', weightPct:'Weight %', inflPct:'Infl. %', totalWeight:'Total Weight', weightedInflation:'Weighted Inflation', noSavedScenarios:'No saved scenarios.', load:'Load', delete:'Delete', newRow:'New', invalidValues:'⚠️ Please enter valid values.', noExportData:'No results to export', scenarioName:'Scenario name:', scenarioSaved:'Scenario saved.', copied:'Copied!', linkCopied:'Link copied!', invalidJSON:'Invalid JSON', basketWeightWarning:'The weight does not add up to 100%. Save anyway?', fxError:'Could not connect to the FX API. Using the last saved rate or local value.', fxUpdating:'Updating FX rate...', fxUpdated:'Rate updated', fxStale:'Last saved rate', originalPower:'% of the original', monthsLower:'months', yearsMoreThan100:'> 100 years', live:'Live', sim:'Sim', eq:'Eq: —'
    },
    'pt-BR': {
      appTitle:'Calculadora de Inflação', languageLabel:'Idioma', parameters:'Parâmetros', clear:'Limpar', scenarioA:'Cenário A (Principal)', initialPrice:'Preço Inicial', finalPrice:'Preço Final', currency:'Moeda', scenarioB:'Cenário B (Comparativo)', copyFromA:'Copiar de A', initialPriceB:'Preço Inicial (B)', finalPriceB:'Preço Final (B)', time:'Tempo', unit:'Unidade', months:'Meses', years:'Anos', days:'Dias', calculate:'Calcular', shortcut:'Atalho: Ctrl + Enter', basketBtn:'🧺 Cesta Personalizada', shareBtn:'🔗 Copiar Link', coverBtn:'📄 Ver Capa', saveScenarioBtn:'💾 Salvar Cenário', scenariosBtn:'📁 Cenários', results:'Resultados', copyText:'Copiar Texto', exportCSV:'⬇ CSV', exportJSON:'⬇ JSON', explanationToggle:'Explicação', totalInflation:'Inflação Total', monthlyAverage:'Média Mensal', annualized:'Anualizada (CAGR)', annualizedShort:'Anualizada', doublingTime:'Tempo de duplicação', purchasingPower:'Poder de compra restante', basketTitle:'🧺 Cesta de Inflação Ponderada', basketHelp:'Defina categorias e pesos. A soma deve ser 100%.', addRow:'+ Adicionar Linha', importJSON:'Importar JSON', exportJSONText:'Exportar JSON', saveClose:'Salvar e Fechar', savedScenariosTitle:'📁 Cenários Salvos', savedScenariosHelp:'Selecione um cenário para carregar ou excluir.', close:'Fechar', back:'Voltar', downloadPNG:'Baixar PNG', printPDF:'Imprimir / PDF', reportTitle:'Relatório de Inflação', generatedOn:'Gerado em', scenarioATotal:'Cenário A: Total', scenarioBComparison:'Comparativo Cenário B', scenarioBTotal:'Cenário B: Total', differenceBvsA:'Diferença (B vs A)', annualizedB:'Anualizada B', accumulatedProjection:'Projeção de Valor Acumulado', disclaimer:'Este documento é uma simulação matemática. Não constitui aconselhamento financeiro.', category:'Categoria', weightPct:'Peso %', inflPct:'Infl. %', totalWeight:'Peso Total', weightedInflation:'Inflação Pond.', noSavedScenarios:'Não há cenários salvos.', load:'Carregar', delete:'Excluir', newRow:'Novo', invalidValues:'⚠️ Insira valores válidos.', noExportData:'Não há resultados para exportar', scenarioName:'Nome do cenário:', scenarioSaved:'Cenário salvo.', copied:'Copiado!', linkCopied:'Link copiado!', invalidJSON:'JSON inválido', basketWeightWarning:'O peso não soma 100%. Salvar mesmo assim?', fxError:'Não foi possível conectar à API de câmbio. Usando a última taxa salva ou o valor local.', fxUpdating:'Atualizando taxa...', fxUpdated:'Taxa atualizada', fxStale:'Última taxa salva', originalPower:'% do original', monthsLower:'meses', yearsMoreThan100:'> 100 anos', live:'Live', sim:'Sim', eq:'Eq: —'
    }
  };
  let currentLang = localStorage.getItem('inflacion_lang') || 'es';
  const tr = key => (I18N[currentLang] && I18N[currentLang][key]) || I18N.es[key] || key;
  const localeForLang = () => currentLang === 'pt-BR' ? 'pt-BR' : currentLang === 'en' ? 'en-US' : 'es-ES';
  function applyTranslations(){
    document.documentElement.lang = currentLang;
    document.querySelectorAll('[data-i18n]').forEach(el => {
      const key = el.dataset.i18n;
      if(I18N[currentLang]?.[key]) el.textContent = I18N[currentLang][key];
    });
    const langSelect = $('langSelect');
    if(langSelect) langSelect.value = currentLang;
    updateFxDisplay();
    updateLiveEquivalents();
    if(STATE.lastResult) updateUI(STATE.lastResult.resA, STATE.lastResult.resB);
    updateExplanation();
  }

  let STATE = {
    // Valor inicial por defecto (backup). Se reemplaza por tasa en vivo al iniciar.
    fx: { USD_BRL: 5.50, date: null, isLive: false, source: 'Local' }, 
    basket: [
      { name: 'Alimentos', w: 30, inf: 5 },
      { name: 'Vivienda', w: 25, inf: 3 },
      { name: 'Transporte', w: 15, inf: 4 }
    ],
    lastResult: null
  };

  // ========= FX ENGINE (LIVE) =========
  const FX_CACHE_KEY = 'inflacion_fx_data';
  const FX_CACHE_MAX_AGE = 30 * 60 * 1000; // 30 min: útil para mostrar algo mientras llega la tasa en vivo.
  let fxAutoRefreshTimer = null;

  function fetchWithTimeout(url, ms = 8000){
    const controller = new AbortController();
    const timeoutId = setTimeout(() => controller.abort(), ms);
    const sep = url.includes('?') ? '&' : '?';
    return fetch(url + sep + '_=' + Date.now(), {
      cache: 'no-store',
      signal: controller.signal
    }).finally(() => clearTimeout(timeoutId));
  }

  async function readFxFromProviders(){
    const providers = [
      {
        name: 'open.er-api.com',
        url: 'https://open.er-api.com/v6/latest/USD',
        parse: data => data?.rates?.BRL
      },
      {
        name: 'AwesomeAPI',
        url: 'https://economia.awesomeapi.com.br/json/last/USD-BRL',
        parse: data => data?.USDBRL?.bid
      },
      {
        name: 'Frankfurter',
        url: 'https://api.frankfurter.app/latest?from=USD&to=BRL',
        parse: data => data?.rates?.BRL
      }
    ];

    const errors = [];
    for(const provider of providers){
      try{
        const res = await fetchWithTimeout(provider.url);
        if(!res.ok) throw new Error(`HTTP ${res.status}`);
        const data = await res.json();
        const rate = Number(provider.parse(data));
        if(!Number.isFinite(rate) || rate <= 0) throw new Error('tasa inválida');
        return { rate, source: provider.name };
      }catch(err){
        errors.push(`${provider.name}: ${err.message || err}`);
      }
    }
    throw new Error(errors.join(' | '));
  }

  async function fetchFx({ silent = false } = {}) {
    const btn = $('btnRefreshFX');
    const icon = btn?.querySelector('svg');
    const lbl = $('fxLabel');

    icon?.classList.add('spin');
    if(btn) btn.disabled = true;
    if(lbl) {
      lbl.style.opacity = 0.65;
      lbl.textContent = tr('fxUpdating');
    }

    try {
      const live = await readFxFromProviders();
      STATE.fx.USD_BRL = live.rate;
      STATE.fx.date = new Date();
      STATE.fx.isLive = true;
      STATE.fx.source = live.source;

      localStorage.setItem(FX_CACHE_KEY, JSON.stringify({
        rate: STATE.fx.USD_BRL,
        ts: STATE.fx.date.getTime(),
        source: STATE.fx.source
      }));

      updateFxDisplay();
      updateLiveEquivalents();
      // La tasa afecta los cálculos si se usan monedas mixtas; por eso recalculamos automáticamente.
      calc();
      return true;
    } catch (e) {
      console.warn('Error fetching FX:', e);
      STATE.fx.isLive = false;
      if(!silent) alert(tr('fxError'));
      updateFxDisplay();
      updateLiveEquivalents();
      return false;
    } finally {
      icon?.classList.remove('spin');
      if(btn) btn.disabled = false;
      if(lbl) lbl.style.opacity = 1;
    }
  }

  function updateFxDisplay(){
    const r = STATE.fx.USD_BRL;
    const mode = STATE.fx.isLive ? tr('live') : tr('fxStale');
    // Mostrar una etiqueta limpia: solo Live cuando la tasa viene de una API en vivo.
    $('fxLabel').textContent = `1 USD ≈ ${r.toFixed(4)} BRL (${mode})`;
    updateLiveEquivalents();
  }

  // Carga una tasa reciente de respaldo para que la app no arranque vacía.
  function loadSavedFx() {
    try {
      const saved = JSON.parse(localStorage.getItem(FX_CACHE_KEY));
      if(saved && Number(saved.rate) > 0) {
        STATE.fx.USD_BRL = Number(saved.rate);
        STATE.fx.date = saved.ts ? new Date(saved.ts) : null;
        STATE.fx.source = saved.source || 'Cache';
        STATE.fx.isLive = (Date.now() - Number(saved.ts || 0)) < FX_CACHE_MAX_AGE;
      }
    } catch(e){}
    updateFxDisplay();
  }

  function startFxAutoRefresh(){
    if(fxAutoRefreshTimer) clearInterval(fxAutoRefreshTimer);
    fxAutoRefreshTimer = setInterval(() => fetchFx({ silent: true }), FX_CACHE_MAX_AGE);
    document.addEventListener('visibilitychange', () => {
      if(!document.hidden) fetchFx({ silent: true });
    });
  }

  $('btnRefreshFX').addEventListener('click', () => fetchFx({ silent: false }));

  function updateLiveEquivalents(){
    const fx = STATE.fx.USD_BRL;
    const p0 = num($('p0_a').value);
    const p1 = num($('p1_a').value);
    const c0 = $('c0_a').value;
    const c1 = $('c1_a').value;

    if(p0 > 0) $('eq_p0_a').textContent = c0==='USD' ? `≈ ${fmtCurr(p0*fx, 'BRL')}` : `≈ ${fmtCurr(p0/fx, 'USD')}`;
    else $('eq_p0_a').textContent = tr('eq');

    if(p1 > 0) $('eq_p1_a').textContent = c1==='USD' ? `≈ ${fmtCurr(p1*fx, 'BRL')}` : `≈ ${fmtCurr(p1/fx, 'USD')}`;
    else $('eq_p1_a').textContent = tr('eq');
  }

  document.querySelectorAll('.live-input').forEach(el => {
    el.addEventListener('input', updateLiveEquivalents);
    el.addEventListener('change', updateLiveEquivalents);
  });


  // ========= CHART INIT =========
  let chartCtx = $('mainChart').getContext('2d');
  let mainChart = new Chart(chartCtx, {
    type: 'line',
    data: { labels: ['Inicio', ...Array(11).fill('.'), 'Fin'], datasets: [] },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      interaction: { mode: 'index', intersect: false },
      plugins: { legend: { labels: { color: '#94a3b8' } } },
      scales: {
        y: { ticks: { color: '#64748b', callback: v=>v.toFixed(1) }, grid: { color: '#1e293b' } },
        x: { ticks: { color: '#64748b' }, grid: { display:false } }
      }
    }
  });

  // ========= CORE LOGIC =========
  function calculateCAGR(p0, p1, t, unit, c0, c1){
    if(p0 <= 0 || p1 <= 0 || t <= 0) return null;

    const rate = STATE.fx.USD_BRL;
    const v0 = c0 === 'USD' ? p0 * rate : p0;
    const v1 = c1 === 'USD' ? p1 * rate : p1;
    const totalInf = (v1 / v0) - 1;

    let t_months = unit==='anios'? t*12 : unit==='dias'? t/30.44 : t;
    let t_years  = t_months / 12;

    if(t_months <= 0) return null;

    const cagr = Math.pow(v1/v0, 1/t_years) - 1;
    const monthly = Math.pow(v1/v0, 1/t_months) - 1;
    const doubleTime = Math.log(2) / Math.log(1 + monthly);

    const points = [];
    const steps = 12; 
    for(let i=0; i<=steps; i++){
      let frac = i/steps;
      points.push( v0 * Math.pow(1+totalInf, frac) );
    }

    return {
      total: totalInf,
      cagr: cagr,
      monthly: monthly,
      doubleMonths: doubleTime,
      powerRem: 1 / (1+totalInf),
      points: points
    };
  }

  function calc(){
    const t = num($('t_val').value);
    const u = $('t_unit').value;

    const resA = calculateCAGR(num($('p0_a').value), num($('p1_a').value), t, u, $('c0_a').value, $('c1_a').value);

    const p0b = $('p0_b').value;
    const hasB = p0b && t > 0;
    let resB = null;
    if(hasB){
      resB = calculateCAGR(num($('p0_b').value), num($('p1_b').value), t, u, $('c0_b').value, $('c1_b').value);
    }

    STATE.lastResult = { resA, resB, t, u };
    updateUI(resA, resB);
    saveState();
  }

  function updateUI(a, b){
    const chartBtn = $('btnShowCover');

    // Reset
    $('resTotal').innerText = '—'; $('resAnual').innerText = '—'; $('resAvg').innerText = '—';
    $('resTotalB').innerText = ''; $('resAnualB').innerText = ''; $('resAvgB').innerText = '';
    $('badgeTotal').hidden = true; $('badgeAnual').hidden = true;
    $('resDouble').innerText = '—'; $('resPower').innerText = '—';

    if(!a){
      chartBtn.disabled = true;
      if(mainChart){ mainChart.data.datasets = []; mainChart.update(); }
      // Ocultar explicación si no hay resultados
      updateExplanation();
      return;
    }

    chartBtn.disabled = false;
    $('resTotal').innerText = fmtPct(a.total);
    $('resAnual').innerText = fmtPct(a.cagr);
    $('resAvg').innerText   = fmtPct(a.monthly);
    $('resDouble').innerText = a.doubleMonths < 1200 ? a.doubleMonths.toFixed(1) + ' ' + tr('monthsLower') : tr('yearsMoreThan100');
    $('resPower').innerText  = (a.powerRem * 100).toFixed(1) + tr('originalPower');

    if(b){
      $('resTotalB').innerText = `(B: ${fmtPct(b.total)})`;
      $('resAnualB').innerText = `(B: ${fmtPct(b.cagr)})`;
      $('resAvgB').innerText   = `(B: ${fmtPct(b.monthly)})`;
      const diff = (b.total - a.total)*100;
      const el = $('badgeTotal');
      el.hidden = false;
      el.innerText = (diff>0?'+':'') + diff.toFixed(2) + ' pp';
      el.className = 'diff-badge ' + (diff>0 ? 'diff-pos' : 'diff-neg');
    }

    drawChart(a, b);
    // Actualizar explicación si corresponde
    updateExplanation();
  }

  function drawChart(a, b){
    if(!mainChart) return;
    const datasets = [{
      label: 'Escenario A (BRL)',
      data: a.points,
      borderColor: '#22d3ee',
      backgroundColor: '#22d3ee22',
      tension: 0.3, fill: true
    }];
    if(b){
      datasets.push({
        label: 'Escenario B',
        data: b.points,
        borderColor: '#f97316',
        borderDash: [5,5],
        tension: 0.3,
        pointStyle: 'rectRot'
      });
    }
    mainChart.data.datasets = datasets;
    mainChart.update();
  }

  // ========= DEEP LINK & STORAGE =========
  function saveState(){
    try {
      const params = new URLSearchParams();
      params.set('p0a', $('p0_a').value);
      params.set('p1a', $('p1_a').value);
      params.set('c0a', $('c0_a').value);
      params.set('c1a', $('c1_a').value);
      params.set('p0b', $('p0_b').value);
      params.set('p1b', $('p1_b').value);
      params.set('c0b', $('c0_b').value);
      params.set('c1b', $('c1_b').value);
      params.set('t', $('t_val').value);
      params.set('u', $('t_unit').value);

      const q = params.toString();
      if(window.history.replaceState) window.history.replaceState(null, '', '?' + q);
      localStorage.setItem('inflacion_v7_state', q);
    } catch(e) {
      console.warn("State save warning:", e);
    }
  }

  function loadState(){
    let q = window.location.search;
    if(!q || q.length < 3) q = localStorage.getItem('inflacion_v7_state'); 
    if(!q) return;

    const p = new URLSearchParams(q.replace('?',''));
    if(p.has('p0a')) $('p0_a').value = p.get('p0a');
    if(p.has('p1a')) $('p1_a').value = p.get('p1a');
    if(p.has('c0a')) $('c0_a').value = p.get('c0a');
    if(p.has('c1a')) $('c1_a').value = p.get('c1a');
    if(p.has('p0b')) $('p0_b').value = p.get('p0b');
    if(p.has('p1b')) $('p1_b').value = p.get('p1b');
    if(p.has('c0b')) $('c0_b').value = p.get('c0b');
    if(p.has('c1b')) $('c1_b').value = p.get('c1b');
    if(p.has('t')) $('t_val').value = p.get('t');
    if(p.has('u')) $('t_unit').value = p.get('u');

    if(p.get('p0b')) $('groupB').classList.remove('collapsed');
    updateLiveEquivalents();
    calc(); 
  }

  // ========= BASKET LOGIC =========
  const basketDialog = $('basketDialog');
  const basketBody = $('basketBody');

  function renderBasket(){
    basketBody.innerHTML = '';
    let totalW = 0;
    let wInf = 0;

    STATE.basket.forEach((item, idx) => {
      totalW += item.w;
      wInf += (item.w/100) * item.inf;

      const tr = document.createElement('tr');
      tr.className = 'basket-row';
      tr.innerHTML = `
        <td><input value="${item.name}" data-idx="${idx}" data-f="name"></td>
        <td><input type="number" value="${item.w}" data-idx="${idx}" data-f="w"></td>
        <td><input type="number" value="${item.inf}" data-idx="${idx}" data-f="inf"></td>
        <td style="text-align:right"><button class="btn btn-sec btn-sm" data-del="${idx}">×</button></td>
      `;
      basketBody.appendChild(tr);
    });

    $('basketWeightSum').innerText = totalW;
    $('basketWeightSum').style.color = totalW === 100 ? 'var(--ok)' : 'var(--err)';
    $('basketRes').innerText = totalW === 100 ? wInf.toFixed(2) + '%' : 'Error (≠100%)';
  }

  basketBody.addEventListener('change', (e) => {
    if(e.target.tagName === 'INPUT'){
      const idx = e.target.dataset.idx;
      const f = e.target.dataset.f;
      let val = e.target.value;
      if(f !== 'name') val = parseFloat(val) || 0;
      STATE.basket[idx][f] = val;
      renderBasket();
    }
  });
  basketBody.addEventListener('click', (e) => {
    if(e.target.dataset.del){
      STATE.basket.splice(e.target.dataset.del, 1);
      renderBasket();
    }
  });

  $('btnAddRow').addEventListener('click', () => {
    STATE.basket.push({name:tr('newRow'), w:0, inf:0});
    renderBasket();
  });
  $('btnOpenBasket').addEventListener('click', () => { renderBasket(); basketDialog.showModal(); });
  $('btnCloseBasket').addEventListener('click', () => {
    const sum = STATE.basket.reduce((a,b)=>a+b.w,0);
    if(sum !== 100 && !confirm(tr('basketWeightWarning'))) return;
    localStorage.setItem('inflacion_basket', JSON.stringify(STATE.basket));
    basketDialog.close();
  });

  $('btnBasketExp').addEventListener('click', () => {
    const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(STATE.basket));
    const anchor = document.createElement('a');
    anchor.setAttribute("href", dataStr);
    anchor.setAttribute("download", "cesta_inflacion.json");
    document.body.appendChild(anchor); anchor.click(); anchor.remove();
  });
  $('btnBasketImp').addEventListener('click', () => $('fileInput').click());
  $('fileInput').addEventListener('change', (e) => {
    const file = e.target.files[0];
    if(!file) return;
    const reader = new FileReader();
    reader.onload = (ev) => {
      try{ STATE.basket = JSON.parse(ev.target.result); renderBasket(); }
      catch(e){ alert(tr('invalidJSON')); }
    };
    reader.readAsText(file);
  });

  const savedBasket = localStorage.getItem('inflacion_basket');
  if(savedBasket) STATE.basket = JSON.parse(savedBasket);

  // ========= ESCENARIOS (Guardar/Cargar) =========
  // Devuelve una cadena con el estado actual de los inputs serializado como querystring
  function getCurrentStateString(){
    const params = new URLSearchParams();
    params.set('p0a', $('p0_a').value);
    params.set('p1a', $('p1_a').value);
    params.set('c0a', $('c0_a').value);
    params.set('c1a', $('c1_a').value);
    params.set('p0b', $('p0_b').value);
    params.set('p1b', $('p1_b').value);
    params.set('c0b', $('c0_b').value);
    params.set('c1b', $('c1_b').value);
    params.set('t', $('t_val').value);
    params.set('u', $('t_unit').value);
    return params.toString();
  }

  // Guarda el escenario actual en localStorage con un nombre introducido por el usuario
  function saveScenario(){
    const name = prompt(tr('scenarioName'));
    if(!name) return;
    const scenarios = JSON.parse(localStorage.getItem('inflacion_saved_scenarios') || '[]');
    const obj = {
      id: Date.now().toString(),
      name: name,
      state: getCurrentStateString(),
      date: new Date().toISOString()
    };
    scenarios.push(obj);
    localStorage.setItem('inflacion_saved_scenarios', JSON.stringify(scenarios));
    alert(tr('scenarioSaved'));
  }

  // Muestra el diálogo con la lista de escenarios guardados
  function openScenarios(){
    const listEl = $('scenariosList');
    const dialog = $('scenariosDialog');
    listEl.innerHTML = '';
    const scenarios = JSON.parse(localStorage.getItem('inflacion_saved_scenarios') || '[]');
    if(scenarios.length === 0){
      listEl.innerHTML = `<p class="help">${tr('noSavedScenarios')}</p>`;
    } else {
      scenarios.forEach(sc => {
        const div = document.createElement('div');
        div.style.display = 'flex';
        div.style.justifyContent = 'space-between';
        div.style.alignItems = 'center';
        div.style.borderBottom = '1px solid #1e293b';
        div.style.padding = '6px 0';
        div.innerHTML = `
          <div><strong>${sc.name}</strong><br><span style="font-size:11px;color:var(--muted)">${new Date(sc.date).toLocaleString(localeForLang())}</span></div>
          <div style="display:flex; gap:6px;">
            <button class="btn btn-primary btn-sm" data-load="${sc.id}">${tr('load')}</button>
            <button class="btn btn-sec btn-sm" data-del="${sc.id}">${tr('delete')}</button>
          </div>`;
        listEl.appendChild(div);
      });
    }
    dialog.showModal();
  }

  // Aplica un estado serializado a la interfaz y recalcula resultados
  function applyStateString(q){
    const p = new URLSearchParams(q);
    if(p.has('p0a')) $('p0_a').value = p.get('p0a');
    if(p.has('p1a')) $('p1_a').value = p.get('p1a');
    if(p.has('c0a')) $('c0_a').value = p.get('c0a');
    if(p.has('c1a')) $('c1_a').value = p.get('c1a');
    if(p.has('p0b')) $('p0_b').value = p.get('p0b');
    if(p.has('p1b')) $('p1_b').value = p.get('p1b');
    if(p.has('c0b')) $('c0_b').value = p.get('c0b');
    if(p.has('c1b')) $('c1_b').value = p.get('c1b');
    if(p.has('t')) $('t_val').value = p.get('t');
    if(p.has('u')) $('t_unit').value = p.get('u');
    // Expandir grupo B si hay datos
    if(p.get('p0b') || p.get('p1b')) $('groupB').classList.remove('collapsed');
    else $('groupB').classList.add('collapsed');
    updateLiveEquivalents();
    calc();
  }

  // Escuchadores para el diálogo de escenarios
  $('btnSaveScenario').addEventListener('click', saveScenario);
  $('btnOpenScenarios').addEventListener('click', openScenarios);
  $('btnCloseScenarios').addEventListener('click', () => $('scenariosDialog').close());
  $('scenariosList').addEventListener('click', (e) => {
    const loadId = e.target.dataset.load;
    const delId = e.target.dataset.del;
    if(loadId){
      const scenarios = JSON.parse(localStorage.getItem('inflacion_saved_scenarios') || '[]');
      const sc = scenarios.find(s => s.id === loadId);
      if(sc) {
        applyStateString(sc.state);
        $('scenariosDialog').close();
      }
    } else if(delId){
      let scenarios = JSON.parse(localStorage.getItem('inflacion_saved_scenarios') || '[]');
      scenarios = scenarios.filter(s => s.id !== delId);
      localStorage.setItem('inflacion_saved_scenarios', JSON.stringify(scenarios));
      // refrescar lista
      openScenarios();
    }
  });

  // ========= EXPORTAR DATOS =========
  function exportJSON(){
    if(!STATE.lastResult || !STATE.lastResult.resA) { alert(tr('noExportData')); return; }
    const inputs = {
      p0a: $('p0_a').value, c0a: $('c0_a').value, p1a: $('p1_a').value, c1a: $('c1_a').value,
      p0b: $('p0_b').value, c0b: $('c0_b').value, p1b: $('p1_b').value, c1b: $('c1_b').value,
      t: $('t_val').value, u: $('t_unit').value
    };
    const a = STATE.lastResult.resA;
    const b = STATE.lastResult.resB;
    const data = {
      inputs: inputs,
      results: {
        scenarioA: {
          total: a.total,
          cagr: a.cagr,
          monthly: a.monthly,
          doubleMonths: a.doubleMonths,
          powerRem: a.powerRem
        },
        scenarioB: b ? {
          total: b.total,
          cagr: b.cagr,
          monthly: b.monthly,
          doubleMonths: b.doubleMonths,
          powerRem: b.powerRem
        } : null
      },
      series: {
        scenarioA: a.points,
        scenarioB: b ? b.points : null
      }
    };
    const dataStr = "data:text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(data, null, 2));
    const anchor = document.createElement('a');
    anchor.setAttribute("href", dataStr);
    anchor.setAttribute("download", "inflacion_data.json");
    document.body.appendChild(anchor);
    anchor.click();
    anchor.remove();
  }

  function exportCSV(){
    if(!STATE.lastResult || !STATE.lastResult.resA) { alert(tr('noExportData')); return; }
    const a = STATE.lastResult.resA.points;
    const b = STATE.lastResult.resB ? STATE.lastResult.resB.points : null;
    let csv = '';
    if(b){
      csv += 'Indice,EscenarioA,EscenarioB\n';
    } else {
      csv += 'Indice,EscenarioA\n';
    }
    for(let i=0; i<a.length; i++){
      csv += `${i},${a[i].toFixed(4)}`;
      if(b) csv += `,${b[i].toFixed(4)}`;
      csv += '\n';
    }
    const dataStr = "data:text/csv;charset=utf-8," + encodeURIComponent(csv);
    const anchor = document.createElement('a');
    anchor.setAttribute("href", dataStr);
    anchor.setAttribute("download", "inflacion_data.csv");
    document.body.appendChild(anchor);
    anchor.click();
    anchor.remove();
  }

  $('btnExportCSV').addEventListener('click', exportCSV);
  $('btnExportJSON').addEventListener('click', exportJSON);

  // ========= EXPLICACIÓN PASO A PASO =========
  function updateExplanation(){
    const expDiv = $('explanation');
    if(!$('toggleExplanation').checked || !STATE.lastResult || !STATE.lastResult.resA){
      expDiv.style.display = 'none';
      return;
    }
    const p0 = num($('p0_a').value);
    const p1 = num($('p1_a').value);
    const c0 = $('c0_a').value;
    const c1 = $('c1_a').value;
    const t = num($('t_val').value);
    const u = $('t_unit').value;
    const rate = STATE.fx.USD_BRL;
    const v0 = c0 === 'USD' ? p0 * rate : p0;
    const v1 = c1 === 'USD' ? p1 * rate : p1;
    const total = (v1/v0) - 1;
    let t_months = u==='anios'? t*12 : u==='dias'? t/30.44 : t;
    let t_years = t_months / 12;
    const monthly = Math.pow(v1/v0, 1/t_months) - 1;
    const cagr = Math.pow(v1/v0, 1/t_years) - 1;
    const doubleTime = Math.log(2) / Math.log(1 + monthly);
    const powerRem = 1 / (1 + total);
    expDiv.innerHTML = `
    <div><b>Paso 1: Conversión a BRL</b><br>
      Precio inicial ajustado: ${fmtCurr(v0, 'BRL')}<br>
      Precio final ajustado: ${fmtCurr(v1, 'BRL')}
    </div>
    <div style="margin-top:6px"><b>Paso 2: Inflación total</b><br>
      Fórmula: (v₁ / v₀) − 1<br>
      Cálculo: (${v1.toFixed(2)} / ${v0.toFixed(2)}) − 1 = ${total.toFixed(4)} (${fmtPct(total)})
    </div>
    <div style="margin-top:6px"><b>Paso 3: Promedio mensual</b><br>
      Fórmula: (v₁/v₀)^(1/n) − 1, n = tiempo en meses (${t_months.toFixed(2)})<br>
      Cálculo: (${(v1/v0).toFixed(4)})^(1/${t_months.toFixed(2)}) − 1 = ${monthly.toFixed(4)} (${fmtPct(monthly)}/mes)
    </div>
    <div style="margin-top:6px"><b>Paso 4: Tasa anual (CAGR)</b><br>
      Fórmula: (v₁/v₀)^(1/años) − 1, años = ${t_years.toFixed(2)}<br>
      Cálculo: (${(v1/v0).toFixed(4)})^(1/${t_years.toFixed(2)}) − 1 = ${cagr.toFixed(4)} (${fmtPct(cagr)}/año)
    </div>
    <div style="margin-top:6px"><b>Paso 5: Tiempo de duplicación</b><br>
      Fórmula: ln(2) / ln(1 + promedio mensual)<br>
      Cálculo: ln(2) / ln(1 + ${monthly.toFixed(4)}) = ${doubleTime.toFixed(2)} meses
    </div>
    <div style="margin-top:6px"><b>Paso 6: Poder de compra remanente</b><br>
      1 / (1 + inflación total) = ${powerRem.toFixed(4)} (${(powerRem*100).toFixed(1)}% del original)
    </div>
    `;
    expDiv.style.display = 'block';
  }

  $('toggleExplanation').addEventListener('change', updateExplanation);

  // ========= EXPORT COVER =========
  $('btnShowCover').addEventListener('click', () => {
    updateCoverData();
    $('reportCover').classList.add('show');
    $('mainApp').style.display = 'none';
  });
  $('btnCloseCover').addEventListener('click', () => {
    $('reportCover').classList.remove('show');
    $('mainApp').style.display = 'grid';
  });

  function updateCoverData(){
    const res = STATE.lastResult;
    if(!res) return;

    $('rcDate').innerText = new Date().toLocaleDateString(localeForLang());
    $('rcFX').innerText = `1 USD ≈ ${STATE.fx.USD_BRL.toFixed(4)} BRL`;
    $('rcTotalA').innerText = fmtPct(res.resA.total);
    $('rcAvgA').innerText = fmtPct(res.resA.monthly);
    $('rcAnualA').innerText = fmtPct(res.resA.cagr);

    renderCoverChart(res.resA, res.resB);

    if(res.resB){
      $('rcSectionB').style.display = 'block';
      $('rcTotalB').innerText = fmtPct(res.resB.total);
      $('rcAnualB').innerText = fmtPct(res.resB.cagr);
      const diff = (res.resB.total - res.resA.total)*100;
      const dEl = $('rcDiff');
      dEl.innerText = (diff>0?'+':'') + diff.toFixed(2) + ' pp';
      dEl.style.color = diff > 0 ? '#4ade80' : '#f87171';
    } else {
      $('rcSectionB').style.display = 'none';
    }
  }

  function renderCoverChart(a, b){
    const W=800, H=300;
    let svg = `<svg viewBox="0 0 ${W} ${H}" xmlns="http://www.w3.org/2000/svg">
      <rect width="100%" height="100%" fill="#0f172a"/>
      <line x1="50" y1="250" x2="750" y2="250" stroke="#334155" stroke-width="2"/>
      <line x1="50" y1="50" x2="50" y2="250" stroke="#334155" stroke-width="2"/>`;

    const maxVal = Math.max(...a.points, ...(b?.points || [])) * 1.1;
    const minVal = Math.min(...a.points, ...(b?.points || [])) * 0.9;
    const getX = i => 50 + (i/(a.points.length-1))*700;
    const getY = v => 250 - ((v-minVal)/(maxVal-minVal))*200;

    let pathA = `M ${getX(0)} ${getY(a.points[0])}`;
    a.points.forEach((p,i) => pathA += ` L ${getX(i)} ${getY(p)}`);
    svg += `<path d="${pathA}" stroke="#22d3ee" stroke-width="4" fill="none"/>`;

    if(b){
      let pathB = `M ${getX(0)} ${getY(b.points[0])}`;
      b.points.forEach((p,i) => pathB += ` L ${getX(i)} ${getY(p)}`);
      svg += `<path d="${pathB}" stroke="#f97316" stroke-width="4" stroke-dasharray="8,4" fill="none"/>`;
    }
    svg += `</svg>`;
    const blob = new Blob([svg], {type: 'image/svg+xml'});
    $('rcChartContainer').innerHTML = `<img src="${URL.createObjectURL(blob)}" style="width:100%; height:100%; object-fit:contain;">`;
  }

  $('btnExportCoverPNG').addEventListener('click', () => {
    const cvs = $('exportCanvas');
    const ctx = cvs.getContext('2d');

    ctx.fillStyle = '#0b0f1a'; ctx.fillRect(0,0,1920,1080);
    ctx.fillStyle = '#ffffff'; ctx.font = 'bold 60px Inter, sans-serif';
    ctx.fillText('Reporte de Inflación', 80, 120);
    ctx.fillStyle = '#a78bfa'; ctx.font = '30px Inter, sans-serif';
    ctx.fillText(`Generado: ${new Date().toLocaleDateString(localeForLang())} • v0.0.8-Alpha`, 80, 170);

    const drawBox = (x,y, lbl, val, color) => {
      ctx.fillStyle = '#111827'; ctx.strokeStyle = '#1f2937'; ctx.lineWidth = 2;
      ctx.beginPath(); ctx.roundRect(x,y,500,160, 20); ctx.fill(); ctx.stroke();
      ctx.fillStyle = '#9ca3af'; ctx.font = '24px Inter, sans-serif';
      ctx.fillText(lbl, x+30, y+50);
      ctx.fillStyle = color || '#ffffff'; ctx.font = 'bold 60px Inter, sans-serif';
      ctx.fillText(val, x+30, y+120);
    };

    drawBox(80, 250, 'ESCENARIO A: TOTAL', $('rcTotalA').innerText, '#22d3ee');
    drawBox(620, 250, 'PROMEDIO MENSUAL', $('rcAvgA').innerText, '#fff');
    drawBox(1160, 250, 'ANUALIZADA (CAGR)', $('rcAnualA').innerText, '#fff');

    if(STATE.lastResult.resB){
      drawBox(80, 440, 'ESCENARIO B: TOTAL', $('rcTotalB').innerText, '#f97316');
      drawBox(620, 440, 'DIFERENCIA', $('rcDiff').innerText, $('rcDiff').style.color);
    }

    const img = $('rcChartContainer').querySelector('img');
    if(img) ctx.drawImage(img, 80, 650, 1760, 350);

    const link = document.createElement('a');
    link.download = 'Reporte_Inflacion.png';
    link.href = cvs.toDataURL('image/png');
    link.click();
  });

  // ========= GLOBAL EVENTS =========
  $('btnCalc').addEventListener('click', calc);
  $('btnClear').addEventListener('click', () => {
    ['p0_a','p1_a','p0_b','p1_b','t_val'].forEach(id=>$(id).value='');
    calc();
    updateLiveEquivalents();
  });
  $('toggleB').addEventListener('click', () => $('groupB').classList.toggle('collapsed'));
  $('btnCopyAtoB').addEventListener('click', () => {
    $('p0_b').value = $('p0_a').value;
    $('c0_b').value = $('c0_a').value;
    $('p1_b').value = $('p1_a').value;
    $('c1_b').value = $('c1_a').value;
  });
  document.addEventListener('keydown', e => {
    if((e.ctrlKey || e.metaKey) && e.key === 'Enter') calc();
  });
  $('btnCopyRes').addEventListener('click', () => {
    let t = `Inflación A: ${$('resTotal').innerText}
CAGR A: ${$('resAnual').innerText}`;
    if(STATE.lastResult?.resB) {
      t += `
Inflación B: ${$('resTotalB').innerText.replace('(B: ', '').replace(')', '')}
CAGR B: ${$('resAnualB').innerText.replace('(B: ', '').replace(')', '')}`;
    }
    t += `
Generado con Calculadora de Inflación v0.0.8-Alpha`;
    navigator.clipboard.writeText(t).then(()=>alert(tr('copied')));
  });
  $('btnShare').addEventListener('click', () => {
    saveState();
    navigator.clipboard.writeText(window.location.href).then(()=>alert(tr('linkCopied')));
  });

  $('langSelect').addEventListener('change', (e) => {
    currentLang = e.target.value;
    localStorage.setItem('inflacion_lang', currentLang);
    applyTranslations();
  });

  // Init
  loadSavedFx(); // Cargar cache FX mientras llega la tasa en vivo.
  loadState();   // Cargar datos inputs.
  applyTranslations(); // Aplicar idioma guardado o español por defecto.
  fetchFx({ silent: true }); // Actualizar automáticamente a la última tasa disponible.
  startFxAutoRefresh(); // Mantener la tasa viva cada 30 minutos y al volver a la pestaña.
});
</script>
</body>
</html>
