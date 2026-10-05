---
layout: page
title: Net worth calculator
permalink: /net-worth
---

<h1 class="page-title">{{ page.title | escape }}</h1>

<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
<script defer src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>

<style>
body{
  background:#f8f9fa;
  color:#212529;
  font-family:system-ui,-apple-system,Segoe UI,Roboto,Inter,Arial;
}

.container-lg{
  max-width:1400px;
}

input[type=number]{
  width:100%;
  padding:6px 8px;
  border:1px solid #ced4da;
  border-radius:6px;
}

.table th,
.table td{
  vertical-align:middle;
}

.table input[disabled]{
  background:#eee;
}

.chart-wrap{
  height:420px;
}

.result strong{
  font-size:1.25rem;
}

.mono{
  font-variant-numeric:tabular-nums;
}


/* === Calculator visual refresh === */

.calculator-shell{
  background:linear-gradient(135deg,#eef6ff 0%,#f8fbff 100%);
  border:1px solid #d7e9ff;
  border-radius:22px;
  padding:24px;
  box-shadow:0 14px 36px rgba(13,110,253,.08);
}

.calculator-kicker{
  font-size:.75rem;
  font-weight:800;
  letter-spacing:.1em;
  text-transform:uppercase;
  color:#0d6efd;
  margin-bottom:6px;
}


/* === Net worth table === */

#nw-table{
  border-radius:16px;
  overflow:hidden;
  box-shadow:0 8px 22px rgba(15,23,42,.06);
  border-color:#e3eaf2;
}

#nw-table thead th{
  background:#f1f5f9!important;
  color:#475569;
  font-size:.78rem;
  text-transform:uppercase;
  letter-spacing:.03em;
}

#nw-table .table-secondary td{
  background:#eaf2fb!important;
  color:#23415f;
  font-weight:800;
  border-top:8px solid #fff;
}


/* Positive / asset inputs */

#nw-table input[data-field=value]:not(:disabled){
  background:#eaf4ff!important;
  border:2px solid #90c2ff!important;
  font-weight:700;
}


/* Debt inputs */

#nw-table input[data-field=debt]:not(:disabled){
  background:#fff1f1!important;
  border:2px solid #f0a2a2!important;
  font-weight:700;
}


/* Active input */

#nw-table input:not(:disabled):focus{
  background:#fff7d6!important;
  border-color:#e0a800!important;
  box-shadow:0 0 0 3px rgba(224,168,0,.14)!important;
  outline:0;
}


/* Disabled inputs */

#nw-table input:disabled{
  background:#f1f3f5!important;
  border-color:#e2e5e8!important;
  color:#a0a6ad;
}


/* === Main result card === */

.net-result-card{
  display:flex;
  align-items:center;
  justify-content:space-between;
  gap:32px;

  background:linear-gradient(135deg,#0d6efd,#0756c9);
  color:#fff;

  border-radius:18px;
  padding:24px 28px;
  margin:22px 0;

  box-shadow:0 12px 28px rgba(13,110,253,.2);
}


/* Left side: net worth result */

.net-result-card > div{
  flex:1 1 auto;
  min-width:0;
}

.net-result-card .result-label{
  font-size:.78rem;
  font-weight:800;
  letter-spacing:.08em;
  text-transform:uppercase;
  opacity:.82;
  margin-bottom:5px;
}

.net-result-card #nw-ft{
  display:block;
  font-size:clamp(1.8rem,3.2vw,2.4rem);
  font-weight:800;
  line-height:1.15;
  white-space:nowrap;
}


/* Right side: percentile */

.net-result-card #pct-chip{
  flex:0 0 auto;

  display:inline-flex;
  align-items:center;
  justify-content:center;

  min-width:130px;

  font-size:1rem;
  font-weight:800;
  line-height:1.2;
  white-space:nowrap;

  padding:.75rem 1.1rem;

  background:#fff!important;
  color:#0d6efd!important;

  border-radius:999px;

  box-shadow:0 4px 12px rgba(0,0,0,.12);
}


/* === Chart === */

.chart-card{
  background:#fff;
  border:1px solid #e3eaf2;
  border-radius:16px;
  padding:18px;
  box-shadow:0 8px 22px rgba(15,23,42,.06);
}


/* === Mobile === */

@media(max-width:768px){

  .calculator-shell{
    padding:14px;
  }

  .net-result-card{
    flex-direction:column;
    align-items:flex-start;
    gap:16px;
    padding:20px;
  }

  .net-result-card #nw-ft{
    font-size:1.7rem;
    white-space:normal;
  }

  .net-result-card #pct-chip{
    align-self:flex-start;
    min-width:auto;
  }

}
</style>

<div class="container py-4">
<div class="calculator-shell">
  <div class="calculator-kicker">Interactive calculator</div>
  <div class="d-flex justify-content-between align-items-center mb-4">
     <p>This calculator shows how your net worth compares with Hungarian households based on data from the Hungarian National Bank (MNB). The percentiles (dividing the population into 100 equal groups) are based on MNB data from 2014, 2017, 2020 and 2023, adjusted for estimated wealth growth between 2023 and 2025. The calculation includes the value of your primary residence, so make sure to include it. Calculations are in Hungarian forints, assuming an EUR/HUF exchange rate of 400. The chart below lets you compare your wealth with wealth levels from several years; naturally, the 2025 scale best reflects current wealth levels.</p>
  </div>

  <table class="table table-bordered bg-white" id="nw-table">
    <thead class="table-light">
      <tr>
        <th style="width:15%">Categories</th>
        <th>Assets</th>
        <th class="text-end" style="width:15%">Market value (HUF)</th>
        <th class="text-end" style="width:15%">Debt (HUF)</th>
        <th class="text-end" style="width:15%">Net value</th>
        <th style="width:4%"></th>
      </tr>
    </thead>
    <tbody id="rows">
      <tr class="table-secondary"><td colspan="6">🏠 Real estate</td></tr>
      <tr id="prop-anchor"></tr>
      <tr><td colspan="6"><button class="btn btn-outline-secondary" id="add-prop" type="button">➕ Add property</button></td></tr>

      <tr class="table-secondary"><td colspan="6">📊 Investments</td></tr>
      <tr data-type="inv"><td>📦</td><td>Pension fund</td>
        <td><input type="number" data-field="value" value="0"></td>
        <td><input type="number" data-field="debt" value="0" disabled></td>
        <td class="text-end mono" data-cell="net">Ft 0</td>
        <td></td>
      </tr>
      <tr data-type="inv"><td>🏛️</td><td>Government bonds</td>
        <td><input type="number" data-field="value" value="0"></td>
        <td><input type="number" data-field="debt" value="0" disabled></td>
        <td class="text-end mono" data-cell="net">Ft 0</td>
        <td></td>
      </tr>
      <tr data-type="inv"><td>🧾</td><td>Long-term investment account (TBSZ)</td>
        <td><input type="number" data-field="value" value="0"></td>
        <td><input type="number" data-field="debt" value="0" disabled></td>
        <td class="text-end mono" data-cell="net">Ft 0</td>
        <td></td>
      </tr>
      <tr data-type="inv"><td>📈</td><td>Other investments</td>
        <td><input type="number" data-field="value" value="0"></td>
        <td><input type="number" data-field="debt" value="0" disabled></td>
        <td class="text-end mono" data-cell="net">Ft 0</td>
        <td></td>
      </tr>

      <tr class="table-secondary"><td colspan="6">💶 Other assets</td></tr>
      <tr data-type="asset"><td>🚗</td><td>Car and other assets</td>
        <td><input type="number" data-field="value" value="0"></td>
        <td><input type="number" data-field="debt" value="0" disabled></td>
        <td class="text-end mono" data-cell="net">Ft 0</td>
        <td></td>
      </tr>

      <tr class="table-secondary"><td colspan="6">➖ Debts</td></tr>
      <tr data-type="liab"><td>💳</td><td>Other debts</td>
        <td><input type="number" data-field="value" value="0" disabled></td>
        <td><input type="number" data-field="debt" value="0"></td>
        <td class="text-end mono" data-cell="net">Ft 0</td>
        <td></td>
      </tr>

      <tr class="fw-bold table-light">
        <td colspan="2">Total</td>
        <td class="text-end" id="sum-value">Ft 0</td>
        <td class="text-end" id="sum-debt">Ft 0</td>
        <td class="text-end" id="sum-net">Ft 0</td>
        <td></td>
      </tr>
    </tbody>
  </table>

  <div class="net-result-card">
    <div><div class="result-label">Estimated net worth</div><span id="nw-ft">Ft 0</span></div>
    <span class="badge" id="pct-chip">Percentile: –</span>
  </div>

  <!-- Skála év választó -->
  <div class="d-flex align-items-center gap-2 mb-1">
    <label for="scale-year" class="form-label mb-0 small text-muted">
      Scale year for the chart:
    </label>
    <select id="scale-year" class="form-select form-select-sm" style="width:auto">
      <option value="2014">2014</option>
      <option value="2017">2017</option>
      <option value="2020">2020</option>
      <option value="2023">2023</option>
      <option value="2025" selected>2025</option>
    </select>
  </div>

  <div class="chart-card"><div class="chart-wrap"><canvas id="percentileChart"></canvas></div>
  <div id="pct-text" class="mt-2 small text-muted">Values update automatically when you enter new data.</div></div>
</div>
</div>

<script>
(function(){
  // === Percentilis küszöbök Ft-ban (1..100; 1 = leggazdagabb, 100 = legszegényebb) ===
  const PCT_DATA = {
    "2014": [
      428491000, 125133000, 89270000, 72937000, 63705000, 55200000, 49602000, 45677000, 41848000, 40127000,
      37656700, 35362300, 33207700, 31184300, 29284300, 27500000, 26445400, 25431300, 24456100, 23518200,
      22616400, 21749100, 20915000, 20113000, 19341700, 18600000, 17972900, 17367000, 16781500, 16215800,
      15669100, 15140800, 14630400, 14137100, 13660500, 13200000, 12825700, 12462000, 12108600, 11765200,
      11431500, 11107400, 10792400, 10486300, 10188900, 9900000, 9628900, 9365300, 9108800, 8859400,
      8616800, 8380900, 8151400, 7928200, 7711100, 7500000, 7257600, 7023100, 6796100, 6576500,
      6364000, 6158300, 5959300, 5766700, 5580300, 5400000, 5155900, 4922800, 4700200, 4487700,
      4284900, 4091100, 3906200, 3729600, 3561000, 3400000, 3153100, 2924200, 2711900, 2515000,
      2332400, 2163000, 2006000, 1860300, 1725300, 1600000, 1212600, 919000, 696400, 527800,
      400000, 303100, 229700, 174100, 132000, 100000, 70000, 40000, 20000, 0
    ],
    "2017": [
      647184000, 191334000, 137200000, 104003000, 83914000, 73398000, 64335000, 58422000, 55636000, 49800000,
      47286800, 44900400, 42634500, 40482900, 38439900, 36500000, 35059200, 33675400, 32346100, 31069300,
      29842900, 28664900, 27533500, 26446600, 25402700, 24400000, 23708100, 23035700, 22382500, 21747700,
      21131000, 20531800, 19949500, 19383800, 18834100, 18300000, 17829000, 17370200, 16923200, 16487600,
      16063300, 15649900, 15247200, 14854800, 14472500, 14100000, 13703400, 13317900, 12943300, 12579200,
      12225400, 11881500, 11547300, 11222500, 10906800, 10600000, 10266500, 9943600, 9630800, 9327800,
      9034400, 8750200, 8474900, 8208300, 7950100, 7700000, 7374600, 7063000, 6764500, 6478600,
      6204800, 5942600, 5691500, 5451000, 5220600, 5000000, 4605900, 4242900, 3908500, 3600400,
      3316600, 3055200, 2814400, 2592600, 2388200, 2200000, 1855200, 1564400, 1319200, 1112400,
      938100, 791100, 667100, 562500, 474300, 400000, 300000, 200000, 100000, 0
    ],
    "2020": [
      767230000, 309962000, 222265000, 168485000, 135942000, 118905000, 104223000, 94645000, 90130000, 80591000,
      77453000, 74152900, 70993500, 67968600, 65072600, 62300000, 59834100, 57465800, 55191200, 53006600,
      50908500, 48893500, 46958200, 45099600, 43314400, 41600000, 40302200, 39044800, 37826700, 36646500,
      35503200, 34395600, 33322500, 32282900, 31275700, 30300000, 29450400, 28624700, 27822100, 27042000,
      26283800, 25546900, 24830600, 24134400, 23457700, 22800000, 22192000, 21600200, 21024200, 20463500,
      19917800, 19386700, 18869700, 18366500, 17876700, 17400000, 16860700, 16338000, 15831600, 15340900,
      14865400, 14404600, 13958100, 13525500, 13106200, 12700000, 12126400, 11578700, 11055800, 10556500,
      10079700, 9624400, 9189800, 8774700, 8378400, 8000000, 7482700, 6998900, 6546300, 6123000,
      5727100, 5356800, 5010400, 4686500, 4383400, 4100000, 3481900, 2957000, 2511200, 2132600,
      1811100, 1538000, 1306200, 1109200, 942000, 800000, 600000, 390000, 180000, 0
    ],
    "2023": [
      1183230000, 478028000, 342780000, 259840000, 209652000, 183377000, 160735000, 145963000, 139001000, 124290000, 119449000, 114360000, 109487000, 104822000, 100356000, 96080000, 92277000, 88624000, 85117000, 81747000, 79121000, 75990000, 72982000, 70093000, 67319000, 64654000, 62637000, 60683000, 58790000, 56955000, 56092000, 54342000, 52647000, 51004000, 49413000, 47872000, 46529000, 45225000, 43957000, 42724000, 41785000, 40614000, 39475000, 38368000, 37292000, 36247000, 35280000, 34339000, 33424000, 32532000, 31439000, 30601000, 29785000, 28991000, 28218000, 27465000, 26614000, 25789000, 24989000, 24215000, 23690000, 22956000, 22244000, 21555000, 20886000, 20239000, 19325000, 18452000, 17619000, 16823000, 16456000, 15712000, 15003000, 14325000, 13678000, 13060000, 12216000, 11426000, 10687000, 9996000, 8591000, 8035000, 7516000, 7030000, 6575000, 6150000, 5223000, 4436000, 3767000, 3199000, 2717000, 2307000, 1959000, 1664000, 1413000, 1200000, 900000, 585000, 270000, 0
    ],
    "2025": [
      1774845000, 693140000, 497031000, 376768000, 303995000, 265897000, 233066000, 211646000, 201551000, 185191000, 177979000, 170396000, 163136000, 156185000, 149530000, 143159000, 137493000, 132050000, 126824000, 121804000, 117891000, 113224000, 108743000, 104439000, 100305000, 96335000, 93329000, 90417000, 87597000, 84864000, 84138000, 81514000, 78970000, 76507000, 74120000, 71807000, 69794000, 67837000, 65935000, 64086000, 62678000, 60921000, 59213000, 57552000, 55939000, 54370000, 52920000, 51509000, 50136000, 48798000, 47159000, 45902000, 44677000, 43486000, 42326000, 41198000, 39921000, 38683000, 37484000, 36322000, 35535000, 34433000, 33366000, 32332000, 31330000, 30359000, 28987000, 27678000, 26428000, 25235000, 24684000, 23569000, 22504000, 21488000, 20517000, 18285000, 17102000, 15997000, 14962000, 13995000, 12027000, 11249000, 10522000, 9842000, 9205000, 8610000, 7312000, 6210000, 5274000, 4478000, 3803000, 3230000, 2743000, 2329000, 1978000, 1680000, 1260000, 819000, 378000, 0
    ]
  };

  const LABELS = Array.from({length:100},(_,i)=>String(100-i)); // 100 -> 1
  const CURRENCY = "Ft";
  const LOCALE = "hu-HU";

  function fmtFt(n){
    return `${CURRENCY} ${Number(n||0).toLocaleString(LOCALE)}`;
  }

  // Alapértelmezett év: 2025
  let currentYear = "2025";
  let PCT_THRESHOLDS = PCT_DATA[currentYear].slice();

  let chart;

  function getChartBars(){
    // PCT_THRESHOLDS: 1..100 (gazdag->szegény), grafikonon: 1..100 (szegény->gazdag)
    return [...PCT_THRESHOLDS].reverse();
  }

  function initChart(){
    const canvas = document.getElementById("percentileChart");
    if(!canvas || !window.Chart || !PCT_THRESHOLDS.length) return;
    const ctx = canvas.getContext('2d');

    chart = new Chart(ctx,{
      type:"bar",
      data:{
        labels: LABELS,
        datasets:[{
          data: getChartBars(),
          backgroundColor: new Array(100).fill("#cfd8dc"),
          borderWidth: 0
        }]
      },
      options:{
        responsive:true,
        maintainAspectRatio:false,
        plugins:{
          legend:{display:false},
          tooltip:{
            callbacks:{
              title: items => items && items[0] ? ("Percentile "+items[0].label) : "",
              label:  item  => "Median net worth " + fmtFt(item.raw)
            }
          }
        },
        scales:{
          x:{grid:{display:false},ticks:{autoSkip:true,maxRotation:0}},
          y:{
            beginAtZero:true,
            suggestedMax: 1800000000,   // fix: 1,8 Mrd
            max: 1800000000,
            ticks:{
              stepSize: 100000000,      // fix: 100M lépték
              callback: (v) => (v/1e6)+"M"
            }
          }
        }
      }
    });
  }

  function updateChartScale(year){
    if(!PCT_DATA[year] || !PCT_DATA[year].length || !chart) return;
    currentYear = year;
    PCT_THRESHOLDS = PCT_DATA[year].slice();

    chart.data.datasets[0].data = getChartBars();

    // Y-skála fix marad: 0–1.8Mrd, 100M lépés
    chart.options.scales.y.max = 1800000000;
    chart.options.scales.y.suggestedMax = 1800000000;
    chart.options.scales.y.ticks.stepSize = 100000000;

    chart.data.datasets[0].backgroundColor = new Array(100).fill("#cfd8dc");
    chart.update();

    compute(); // új skálához újraszámoljuk a percentilist
  }

  function highlight(p){
    if(!chart) return;
    const idx = 100 - p; // 1-es percentilis a jobb szél
    const colors = new Array(100).fill("#cfd8dc");
    if (idx>=0 && idx<colors.length) colors[idx] = "#0d6efd";
    chart.data.datasets[0].backgroundColor = colors;
    chart.update();
  }

  function addProperty(){
    const anchor=document.getElementById("prop-anchor");
    const tr=document.createElement("tr"); tr.dataset.type="prop";
    tr.innerHTML=`<td>🏠</td><td>Property</td>
      <td><input type="number" data-field="value" value="0"></td>
      <td><input type="number" data-field="debt" value="0"></td>
      <td class="text-end mono" data-cell="net">Ft 0</td>
      <td><button class="btn btn-sm btn-outline-danger" type="button">✖</button></td>`;
    anchor.parentNode.insertBefore(tr,anchor);
  }

  function compute(){
    let sumV=0,sumD=0,sumN=0;
    document.querySelectorAll("#rows tr[data-type]").forEach(r=>{
      const t=r.dataset.type;
      const v=+r.querySelector('[data-field="value"]')?.value||0;
      const d=+r.querySelector('[data-field="debt"]')?.value||0;
      let n=0;
      if(t==="prop") n=v-d;
      else if(t==="inv"||t==="asset") n=v;
      else if(t==="liab") n=-d;
      sumV+=v; sumD+=d; sumN+=n;
      r.querySelector('[data-cell="net"]').textContent = fmtFt(n);
    });

    document.getElementById("sum-value").textContent = fmtFt(sumV);
    document.getElementById("sum-debt").textContent  = fmtFt(sumD);
    document.getElementById("sum-net").textContent   = fmtFt(sumN);
    document.getElementById("nw-ft").textContent     = fmtFt(sumN);

    if (!PCT_THRESHOLDS.length){
      document.getElementById("pct-chip").textContent = "Percentile: –";
      document.getElementById("pct-text").textContent = "Percentile data unavailable.";
      return;
    }

    // Percentilis (1..100) az aktuális év skáláján
    let pct=100;
    for(let i=0;i<PCT_THRESHOLDS.length;i++){
      if(sumN>=PCT_THRESHOLDS[i]){ pct=i+1; break; }
    }

    document.getElementById("pct-chip").textContent = "Top "+pct+"% ("+currentYear+")";
    document.getElementById("pct-text").textContent =
      `Your estimated net worth is ${fmtFt(sumN)}, making you wealthier than ${100-pct}% of households (Top ${pct}%, ${currentYear} scale).`;

    highlight(pct);
  }

  function initAll(){
    addProperty();
    initChart();
    compute();
  }

  if (document.readyState === "complete" || document.readyState === "interactive") {
    initAll();
  } else {
    document.addEventListener("DOMContentLoaded", initAll);
  }

  // Események
  document.body.addEventListener("input",e=>{ if(e.target.matches("input")) compute(); });
  document.addEventListener("click",e=>{
    if(e.target.id==="add-prop"){ addProperty(); }
    if(e.target.tagName==="BUTTON" && e.target.closest("tr")?.dataset.type==="prop"){
      e.target.closest("tr").remove(); compute();
    }
  });

  document.addEventListener("change", e => {
    if (e.target && e.target.id === "scale-year") {
      updateChartScale(e.target.value);
    }
  });
})();
</script>

