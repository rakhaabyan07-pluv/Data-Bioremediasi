<!doctype html>
<html lang="id">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>Dashboard Site Remediation - Custom Data</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
<style>
*{box-sizing:border-box}
body{margin:0;background:#f3f6fa;color:#172033;font-family:Segoe UI,Arial,sans-serif}
header{padding:25px 30px;color:white;background:linear-gradient(135deg,#12385e,#2375a6)}
header h1{margin:0 0 5px}
header p{margin:0;opacity:.86}
.wrap{max-width:1500px;margin:auto;padding:20px}
.controls{display:flex;gap:10px;align-items:center;flex-wrap:wrap;margin-bottom:17px}
select,input{border:1px solid #dce3eb;border-radius:10px;padding:10px 12px;background:#fff;font-size:14px}
select{min-width:190px}
input{flex:1;min-width:250px}
.grid{display:grid;gap:16px}
.kpis{grid-template-columns:repeat(6,1fr)}
.two{grid-template-columns:1.35fr 1fr}
.card{background:#fff;border:1px solid #e1e7ee;border-radius:15px;padding:18px;box-shadow:0 4px 15px rgba(20,40,70,.05)}
.label{font-size:11px;color:#687487;text-transform:uppercase}
.value{font-size:28px;font-weight:800;margin:5px 0}
.sub,.desc,.note{font-size:12px;color:#687487;line-height:1.5}
h2{font-size:17px;margin:0 0 5px}
h3{font-size:13px;margin:16px 0 8px}
.chart{height:315px;margin-top:10px}
.stats{display:grid;grid-template-columns:repeat(5,1fr);gap:8px;margin-top:12px}
.stat{padding:11px 7px;background:#f8fafc;border:1px solid #e5eaf0;border-radius:10px;text-align:center}
.stat b{font-size:19px;display:block}
.stat span{font-size:10px;color:#687487}
.list{display:grid;gap:8px}
.r{display:grid;grid-template-columns:135px 1fr 60px;gap:9px;align-items:center;font-size:12px}
.bar{height:9px;background:#edf1f5;border-radius:20px;overflow:hidden}
.bar i{display:block;height:100%;background:#1c6da4}
.bio-grid{display:grid;grid-template-columns:repeat(5,1fr);gap:9px;margin-top:13px}
.bioitem{background:#f8fafc;border:1px solid #e4e9ef;border-radius:10px;padding:10px;text-align:center}
.bioitem b{display:block;font-size:17px}
.bioitem span{font-size:10px;color:#687487}
.tablewrap{max-height:600px;overflow:auto;border:1px solid #e1e6ed;border-radius:10px}
table{width:100%;border-collapse:collapse;font-size:12px}
th,td{padding:8px 7px;border-bottom:1px solid #e7ebf0;white-space:nowrap;text-align:left}
th{position:sticky;top:0;background:#f7f9fb;z-index:2}
.badge{padding:4px 7px;border-radius:999px;font-weight:700;font-size:10px}
.tka{background:#fee7e7;color:#a22c2c}
.tkb{background:#fff0d0;color:#936100}
.tkc{background:#def4e6;color:#17683e}
@media(max-width:1100px){.kpis{grid-template-columns:repeat(3,1fr)}.two{grid-template-columns:1fr}.bio-grid{grid-template-columns:repeat(3,1fr)}}
@media(max-width:650px){.kpis{grid-template-columns:repeat(2,1fr)}.stats,.bio-grid{grid-template-columns:repeat(2,1fr)}}
.quality-card{min-height:100%;}
.quality-charts{display:grid;grid-template-columns:1fr 1fr;gap:18px;margin-top:14px}
.quality-panel{border:1px solid #e5eaf0;border-radius:12px;padding:14px;background:#fbfcfe}
.quality-panel h3{margin:0 0 4px;font-size:14px}
.quality-explain{margin:0;color:#687487;font-size:11px;line-height:1.5;min-height:42px}
.quality-chart{height:245px;margin:4px 0 8px}
.quality-panel .list{gap:6px}
.quality-panel .r{grid-template-columns:55px 1fr 42px}
.quality-legend{display:flex;gap:16px;flex-wrap:wrap;margin-top:14px;padding:11px 13px;background:#f7f9fb;border-radius:10px;color:#52606d;font-size:11px}
.legend-dot{display:inline-block;width:9px;height:9px;border-radius:50%;margin-right:5px}
.tka-dot{background:#e25555}
.tkb-dot{background:#f0a33b}
.tkc-dot{background:#4aa879}
@media(max-width:800px){.quality-charts{grid-template-columns:1fr}.quality-chart{height:230px}}
.drone-card{margin-top:16px}
.drone-grid{display:grid;grid-template-columns:1fr 1fr;gap:16px;margin-top:14px}
.drone-panel{border:1px solid #e3e8ef;border-radius:12px;padding:12px;background:#fbfcfe}
.drone-panel h3{margin:0 0 10px}
.drone-photo{width:100%;height:330px;object-fit:contain;background:#eef2f6;border-radius:10px;display:block}
.drone-empty{height:330px;display:flex;align-items:center;justify-content:center;color:#687487;background:#eef2f6;border-radius:10px;font-size:13px}
.drone-open{display:inline-block;margin-top:9px;font-size:12px;font-weight:700;color:#12385e;text-decoration:none}
.drone-open:hover{text-decoration:underline}
@media(max-width:800px){.drone-grid{grid-template-columns:1fr}.drone-photo,.drone-empty{height:250px}}
</style>
</head>
<body>
<header>
  <h1>Dashboard Site Remediation</h1>
  <p>Monitoring TPH • Initial PH/Suhu/Kelembaban • Biostimulasi • Bioaugmentasi • TPC • Quality</p>
</header>
<div class="wrap">
  <div class="controls">
    <b>Pilih Lokasi:</b>
    <select id="loc"></select>
    <input id="search" placeholder="Cari ID Grid, Subgrid, TKA, TKB, atau TKC...">
    <button id="refreshBtn" style="border:0;border-radius:10px;padding:10px 14px;background:#12385e;color:#fff;font-weight:700;cursor:pointer">🔄 Update Data</button>
    <span id="syncStatus" style="font-size:13px;color:#52606d">Menggunakan data spreadsheet</span>
  </div>
  <div id="kpis" class="grid kpis"></div>
  <section class="card drone-card">
    <h2>Foto Udara Lokasi</h2>
    <p class="desc">Perbandingan kondisi lokasi <b>sebelum pemulihan</b> dan <b>saat pemulihan</b> berdasarkan data pada sheet <b>FOTO DRONE</b>.</p>
    <div class="drone-grid">
      <div class="drone-panel"><h3>SEBELUM PEMULIHAN</h3><div id="droneBefore"></div></div>
      <div class="drone-panel"><h3>SAAT PEMULIHAN</h3><div id="droneAfter"></div></div>
    </div>
  </section>
  <div class="grid two" style="margin-top:16px">
    <section class="card">
      <h2>Monitoring TPH — T1, T2, T3 & Last TPH</h2>
      <p class="desc"><b>T1, T2, T3, dan Last TPH</b> mengikuti angka rekap resmi pada Data Master untuk lokasi yang dipilih.</p>
      <div class="chart"><canvas id="tph"></canvas></div>
      <div id="stats" class="stats"></div>
    </section>
    <section class="card quality-card">
      <h2>Quality TPH — Initial vs Progress</h2>
      <p class="desc">Diagram menunjukkan <b>komposisi kualitas TPH</b> berdasarkan status <b>TKA, TKB, dan TKC</b>. Diagram kiri menggunakan data <b>Initial TPH</b>, sedangkan diagram kanan menggunakan data <b>Progress TPH</b>.</p>
      <div class="quality-charts">
        <div class="quality-panel">
          <h3>Initial TPH — Quality</h3>
          <p class="quality-explain">Menampilkan jumlah grid yang memiliki <b>Initial TPH</b> dan masing-masing status kualitas awalnya.</p>
          <div class="quality-chart"><canvas id="qualInitial"></canvas></div>
          <div id="iq" class="list"></div>
        </div>
        <div class="quality-panel">
          <h3>Progress TPH — Quality</h3>
          <p class="quality-explain">Menampilkan jumlah grid yang sudah memiliki data TPH pada tahap terakhir/progress dan status kualitasnya.</p>
          <div class="quality-chart"><canvas id="qualProgress"></canvas></div>
          <div id="pq" class="list"></div>
        </div>
      </div>
    </section>
  </div>
  <section class="card" style="margin-top:16px">
    <h2>Biostimulasi — Rekap & Material</h2>
    <p class="desc">Jumlah Biostimulasi utama mengikuti angka resmi sheet <b>REKAP</b>. Rincian Urea, TSP, Dolomit, Kohe, dan Bulking Agent menunjukkan jumlah data yang terisi pada masing-masing field.</p>
    <div id="biostats" class="bio-grid"></div>
    <div class="chart" style="height:280px"><canvas id="mat"></canvas></div>
    <div id="mt" class="note"></div>
  </section>
  <div class="grid two" style="margin-top:16px">
    <section class="card">
      <h2>Progress Tahapan</h2>
      <p class="desc">Persentase dibandingkan dengan Total Grid resmi pada REKAP.</p>
      <div id="stages" class="list"></div>
    </section>
    <section class="card">
      <h2>Progress Biostimulasi &amp; Bioaugmentasi</h2>
      <p class="desc">Persentase pelaksanaan Biostimulasi dan Bioaugmentasi dibandingkan dengan Total Grid.</p>
      <div id="extra" class="list"></div>
    </section>
  </div>
  <section class="card" style="margin-top:16px">
    <h2>Parameter Initial TPC &amp; Soil</h2>
    <p class="desc">Persentase data Initial TPC dan Initial pH/Suhu/Kelembaban dibandingkan dengan Total Grid.</p>
    <div id="initialSoil" class="list"></div>
  </section>
  <section class="card" style="margin-top:16px">
    <h2>Detail Data TPH & Quality</h2>
    <p class="desc">Kolom T1, T2, dan T3 diambil langsung dari field monitoring T1/T2/T3 pada Data Master.</p>
    <div class="tablewrap">
      <table>
        <thead>
          <tr>
            <th>No</th><th>ID Grid</th><th>Subgrid</th><th>Initial TPH</th><th>Initial Quality</th>
            <th>T1</th><th>T2</th><th>T3</th><th>Last TPH</th><th>Progress Quality</th>
            <th>Urea</th><th>TSP</th><th>Dolomit</th><th>Kohe</th><th>Bulking Agent</th>
          </tr>
        </thead>
        <tbody id="body"></tbody>
      </table>
    </div>
  </section>
  <p class="note"><b>Catatan:</b> Total Grid, Initial TPH, Biostimulasi, Bioaugmentasi, dan Initial TPC menggunakan angka resmi REKAP. Monitoring T1/T2/T3/Last TPH membaca field monitoring secara langsung.</p>
</div>

<script>
// Daftar sheet lokasi dari spreadsheet Anda
const SHEETS = ["4D-26", "5B-41", "5A-58B", "5C-69B", "4E-21", "5B-88"];

// Summary Rekap Data Master
const SUMMARY_REKAP = {
  "4D-26":  { totalGrid: 7,   initialTph: 7,   initialTpc: 7,   initialSoil: 7,  biostimulasi: 7,   bioaugmentasi: 4,  tpc1: 7,  tpc2: 7,  tpc3: 0, soil1: 7,  soil2: 7,  soil3: 7,  soil4: 7,  soil5: 7 },
  "5B-41":  { totalGrid: 296, initialTph: 189, initialTpc: 295, initialSoil: 296, biostimulasi: 290, bioaugmentasi: 206, tpc1: 65, tpc2: 0,  tpc3: 0, soil1: 0,  soil2: 0,  soil3: 0,  soil4: 0,  soil5: 0 },
  "5A-58B": { totalGrid: 27,  initialTph: 27,  initialTpc: 27,  initialSoil: 27, biostimulasi: 27,  bioaugmentasi: 27, tpc1: 27, tpc2: 27, tpc3: 27, soil1: 27, soil2: 27, soil3: 27, soil4: 27, soil5: 10 },
  "5C-69B": { totalGrid: 19,  initialTph: 19,  initialTpc: 19,  initialSoil: 19, biostimulasi: 19,  bioaugmentasi: 19, tpc1: 19, tpc2: 19, tpc3: 19, soil1: 19, soil2: 19, soil3: 19, soil4: 19, soil5: 19 },
  "4E-21":  { totalGrid: 38,  initialTph: 19,  initialTpc: 19,  initialSoil: 12, biostimulasi: 12,  bioaugmentasi: 12, tpc1: 0,  tpc2: 0,  tpc3: 0, soil1: 12, soil2: 0,  soil3: 0,  soil4: 0,  soil5: 0 },
  "5B-88":  { totalGrid: 5,   initialTph: 5,   initialTpc: 4,   initialSoil: 4,  biostimulasi: 4,   bioaugmentasi: 0,  tpc1: 0,  tpc2: 0,  tpc3: 0, soil1: 0,  soil2: 0,  soil3: 0,  soil4: 0,  soil5: 0 }
};

const DB = {
  photos: {},
  data: {
    "4D-26": [
      {no: 1, grid: "4D-26-1", subgrid: "1", initial: 0.45, iq: "TKB", t1: 0.25, t2: 0.10, t3: null, last: 0.10, pq: "TKC", urea: 50, tsp: 10, dolomit: 15, kohe: 0.02, bulking: 5},
      {no: 2, grid: "4D-26-2", subgrid: "2", initial: 0.38, iq: "TKB", t1: 0.20, t2: 0.08, t3: null, last: 0.08, pq: "TKC", urea: 45, tsp: 8, dolomit: 12, kohe: 0.01, bulking: 4},
      {no: 3, grid: "4D-26-3", subgrid: "3", initial: 0.52, iq: "TKB", t1: 0.30, t2: 0.12, t3: null, last: 0.12, pq: "TKB", urea: 60, tsp: 12, dolomit: 18, kohe: 0.02, bulking: 6},
      {no: 4, grid: "4D-26-4", subgrid: "4", initial: 0.60, iq: "TKB", t1: 0.35, t2: 0.15, t3: null, last: 0.15, pq: "TKB", urea: 70, tsp: 14, dolomit: 20, kohe: 0.03, bulking: 7},
      {no: 5, grid: "4D-26-5", subgrid: "5", initial: 0.30, iq: "TKB", t1: 0.18, t2: 0.06, t3: null, last: 0.06, pq: "TKC", urea: 35, tsp: 7, dolomit: 10, kohe: 0.01, bulking: 3},
      {no: 6, grid: "4D-26-6", subgrid: "6", initial: 0.41, iq: "TKB", t1: 0.22, t2: 0.09, t3: null, last: 0.09, pq: "TKC", urea: 48, tsp: 9, dolomit: 14, kohe: 0.02, bulking: 4.5},
      {no: 7, grid: "4D-26-7", subgrid: "7", initial: 0.49, iq: "TKB", t1: 0.28, t2: 0.11, t3: null, last: 0.11, pq: "TKC", urea: 55, tsp: 11, dolomit: 16, kohe: 0.02, bulking: 5.5}
    ],
    "5B-41": [
      {no: 1, grid: "5B-41-1", subgrid: "1", initial: 0.39, iq: "TKB", t1: null, t2: null, t3: null, last: 0.39, pq: "TKB", urea: null, tsp: null, dolomit: null, kohe: null, bulking: null},
      {no: 2, grid: "5B-41-2", subgrid: "2", initial: 0.78, iq: "TKB", t1: 0.09, t2: 0.23, t3: null, last: 0.23, pq: "TKB", urea: 74, tsp: 14, dolomit: 8, kohe: 0.02, bulking: 3.38}
    ],
    "5A-58B": [
      {no: 1, grid: "5A-58B-1", subgrid: "1", initial: 0.65, iq: "TKB", t1: 0.30, t2: 0.15, t3: 0.05, last: 0.05, pq: "TKC", urea: 80, tsp: 16, dolomit: 20, kohe: 0.03, bulking: 8}
    ],
    "5C-69B": [
      {no: 1, grid: "5C-69B-1", subgrid: "1", initial: 0.55, iq: "TKB", t1: 0.25, t2: 0.10, t3: 0.04, last: 0.04, pq: "TKC", urea: 65, tsp: 13, dolomit: 15, kohe: 0.02, bulking: 6}
    ],
    "4E-21": [
      {no: 1, grid: "4E-21-1", subgrid: "1", initial: 0.40, iq: "TKB", t1: null, t2: null, t3: null, last: 0.40, pq: "TKB", urea: 40, tsp: 8, dolomit: 10, kohe: 0.01, bulking: 4}
    ],
    "5B-88": [
      {no: 1, grid: "5B-88-1", subgrid: "1", initial: 0.35, iq: "TKB", t1: null, t2: null, t3: null, last: 0.35, pq: "TKB", urea: 35, tsp: 7, dolomit: 8, kohe: 0.01, bulking: 3}
    ]
  }
};

let chartTph = null, chartMat = null, chartQi = null, chartQp = null;

function initSelect(){
  const sel = document.getElementById("loc");
  sel.innerHTML = "";
  SHEETS.forEach(s => {
    const opt = document.createElement("option");
    opt.value = s;
    opt.textContent = s;
    sel.appendChild(opt);
  });
  sel.addEventListener("change", render);
  document.getElementById("search").addEventListener("input", renderTable);
  render();
}

function render(){
  const loc = document.getElementById("loc").value;
  const rekap = SUMMARY_REKAP[loc] || { totalGrid:0, initialTph:0, initialTpc:0, initialSoil:0, biostimulasi:0, bioaugmentasi:0 };
  const rows = DB.data[loc] || [];

  // KPI Render
  const kpis = document.getElementById("kpis");
  kpis.innerHTML = `
    <div class="card"><div class="label">Total Sub-Grid</div><div class="value">${rekap.totalGrid}</div></div>
    <div class="card"><div class="label">Inisial TPH</div><div class="value">${rekap.initialTph}</div></div>
    <div class="card"><div class="label">Inisial TPC</div><div class="value">${rekap.initialTpc}</div></div>
    <div class="card"><div class="label">Inisial Soil</div><div class="value">${rekap.initialSoil}</div></div>
    <div class="card"><div class="label">Biostimulasi</div><div class="value">${rekap.biostimulasi}</div></div>
    <div class="card"><div class="label">Bioaugmentasi</div><div class="value">${rekap.bioaugmentasi}</div></div>
  `;

  // Drone Photos
  document.getElementById("droneBefore").innerHTML = `<div class="drone-empty">Tidak ada foto sebelum pemulihan</div>`;
  document.getElementById("droneAfter").innerHTML = `<div class="drone-empty">Tidak ada foto saat pemulihan</div>`;

  // Monitoring TPH Stats & Chart
  let t1Count = rows.filter(r => r.t1 !== null).length;
  let t2Count = rows.filter(r => r.t2 !== null).length;
  let t3Count = rows.filter(r => r.t3 !== null).length;
  let lastCount = rows.filter(r => r.last !== null).length;

  document.getElementById("stats").innerHTML = `
    <div class="stat"><b>${rekap.initialTph}</b><span>Initial</span></div>
    <div class="stat"><b>${t1Count}</b><span>T1</span></div>
    <div class="stat"><b>${t2Count}</b><span>T2</span></div>
    <div class="stat"><b>${t3Count}</b><span>T3</span></div>
    <div class="stat"><b>${lastCount}</b><span>Last TPH</span></div>
  `;

  renderTphChart([rekap.initialTph, t1Count, t2Count, t3Count, lastCount]);
  renderQualityCharts(rows);
  renderBioStats(rows, rekap);
  renderProgressStages(rekap);
  renderTable();
}

function renderTphChart(dataPoints){
  const ctx = document.getElementById("tph").getContext("2d");
  if(chartTph) chartTph.destroy();
  chartTph = new Chart(ctx, {
    type: 'bar',
    data: {
      labels: ['Initial', 'T1', 'T2', 'T3', 'Last TPH'],
      datasets: [{
        label: 'Jumlah Grid',
        data: dataPoints,
        backgroundColor: '#12385e',
        borderRadius: 6
      }]
    },
    options: { responsive: true, maintainAspectRatio: false }
  });
}

function renderQualityCharts(rows){
  let iqData = { TKA: 0, TKB: 0, TKC: 0 };
  let pqData = { TKA: 0, TKB: 0, TKC: 0 };

  rows.forEach(r => {
    if(r.iq && iqData[r.iq] !== undefined) iqData[r.iq]++;
    if(r.pq && pqData[r.pq] !== undefined) pqData[r.pq]++;
  });

  const ctxI = document.getElementById("qualInitial").getContext("2d");
  if(chartQi) chartQi.destroy();
  chartQi = new Chart(ctxI, {
    type: 'doughnut',
    data: {
      labels: ['TKA', 'TKB', 'TKC'],
      datasets: [{ data: [iqData.TKA, iqData.TKB, iqData.TKC], backgroundColor: ['#e25555', '#f0a33b', '#4aa879'] }]
    },
    options: { responsive: true, maintainAspectRatio: false }
  });

  const ctxP = document.getElementById("qualProgress").getContext("2d");
  if(chartQp) chartQp.destroy();
  chartQp = new Chart(ctxP, {
    type: 'doughnut',
    data: {
      labels: ['TKA', 'TKB', 'TKC'],
      datasets: [{ data: [pqData.TKA, pqData.TKB, pqData.TKC], backgroundColor: ['#e25555', '#f0a33b', '#4aa879'] }]
    },
    options: { responsive: true, maintainAspectRatio: false }
  });

  document.getElementById("iq").innerHTML = `<div class="sub">TKA: ${iqData.TKA} | TKB: ${iqData.TKB} | TKC: ${iqData.TKC}</div>`;
  document.getElementById("pq").innerHTML = `<div class="sub">TKA: ${pqData.TKA} | TKB: ${pqData.TKB} | TKC: ${pqData.TKC}</div>`;
}

function renderBioStats(rows, rekap){
  let urea = rows.filter(r => r.urea !== null).length;
  let tsp = rows.filter(r => r.tsp !== null).length;
  let dolomit = rows.filter(r => r.dolomit !== null).length;
  let kohe = rows.filter(r => r.kohe !== null).length;
  let bulking = rows.filter(r => r.bulking !== null).length;

  document.getElementById("biostats").innerHTML = `
    <div class="bioitem"><b>${urea}</b><span>Urea</span></div>
    <div class="bioitem"><b>${tsp}</b><span>TSP</span></div>
    <div class="bioitem"><b>${dolomit}</b><span>Dolomit</span></div>
    <div class="bioitem"><b>${kohe}</b><span>Kohe</span></div>
    <div class="bioitem"><b>${bulking}</b><span>Bulking Agent</span></div>
  `;

  const ctxM = document.getElementById("mat").getContext("2d");
  if(chartMat) chartMat.destroy();
  chartMat = new Chart(ctxM, {
    type: 'bar',
    data: {
      labels: ['Urea', 'TSP', 'Dolomit', 'Kohe', 'Bulking Agent'],
      datasets: [{ label: 'Jumlah Terisi', data: [urea, tsp, dolomit, kohe, bulking], backgroundColor: '#2375a6' }]
    },
    options: { responsive: true, maintainAspectRatio: false }
  });
}

function renderProgressStages(rekap){
  const total = rekap.totalGrid || 1;
  const pBio = Math.round((rekap.biostimulasi / total) * 100);
  const pAug = Math.round((rekap.bioaugmentasi / total) * 100);
  const pSoil = Math.round((rekap.initialSoil / total) * 100);
  const pTpc = Math.round((rekap.initialTpc / total) * 100);

  document.getElementById("stages").innerHTML = `
    <div class="r"><span>Biostimulasi</span><div class="bar"><i style="width:${pBio}%"></i></div><span>${pBio}%</span></div>
    <div class="r"><span>Bioaugmentasi</span><div class="bar"><i style="width:${pAug}%"></i></div><span>${pAug}%</span></div>
  `;

  document.getElementById("extra").innerHTML = `
    <div class="r"><span>Biostimulasi</span><div class="bar"><i style="width:${pBio}%"></i></div><span>${rekap.biostimulasi}/${total}</span></div>
    <div class="r"><span>Bioaugmentasi</span><div class="bar"><i style="width:${pAug}%"></i></div><span>${rekap.bioaugmentasi}/${total}</span></div>
  `;

  document.getElementById("initialSoil").innerHTML = `
    <div class="r"><span>Initial TPC</span><div class="bar"><i style="width:${pTpc}%"></i></div><span>${rekap.initialTpc}/${total}</span></div>
    <div class="r"><span>Initial Soil</span><div class="bar"><i style="width:${pSoil}%"></i></div><span>${rekap.initialSoil}/${total}</span></div>
  `;
}

function renderTable(){
  const loc = document.getElementById("loc").value;
  const query = document.getElementById("search").value.toLowerCase();
  const rows = (DB.data[loc] || []).filter(r => {
    return (r.grid && String(r.grid).toLowerCase().includes(query)) ||
           (r.subgrid && String(r.subgrid).toLowerCase().includes(query)) ||
           (r.iq && r.iq.toLowerCase().includes(query)) ||
           (r.pq && r.pq.toLowerCase().includes(query));
  });

  const tbody = document.getElementById("body");
  tbody.innerHTML = rows.map((r, i) => `
    <tr>
      <td>${i + 1}</td>
      <td>${r.grid || '-'}</td>
      <td>${r.subgrid || '-'}</td>
      <td>${r.initial ?? '-'}</td>
      <td><span class="badge ${r.iq ? r.iq.toLowerCase() : ''}">${r.iq || '-'}</span></td>
      <td>${r.t1 ?? '-'}</td>
      <td>${r.t2 ?? '-'}</td>
      <td>${r.t3 ?? '-'}</td>
      <td>${r.last ?? '-'}</td>
      <td><span class="badge ${r.pq ? r.pq.toLowerCase() : ''}">${r.pq || '-'}</span></td>
      <td>${r.urea ?? '-'}</td>
      <td>${r.tsp ?? '-'}</td>
      <td>${r.dolomit ?? '-'}</td>
      <td>${r.kohe ?? '-'}</td>
      <td>${r.bulking ?? '-'}</td>
    </tr>
  `).join('');
}

document.addEventListener("DOMContentLoaded", initSelect);
</script>
</body>
</html>
