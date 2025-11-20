---
layout: page
title: Net worth calculator
permalink: /net-worth
---

<h1 class="page-title">{{ page.title | escape }}</h1>

<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
<script defer src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>

<style>
body{background:#f8f9fa;color:#212529;font-family:system-ui,-apple-system,Segoe UI,Roboto,Inter,Arial}
.container-lg{max-width:1400px}
input[type=number]{width:100%;padding:6px 8px;border:1px solid #ced4da;border-radius:6px}
.table th,.table td{vertical-align:middle}
.table input[disabled]{background:#eee}
.chart-wrap{height:420px}
.result strong{font-size:1.25rem}
.mono{font-variant-numeric: tabular-nums}
</style>

<div class="container py-4">
  <div class="d-flex justify-content-between align-items-center mb-4">
    <p>This calculator shows you how wealthy your household was at the end of 2025 compared to all household in Hungary. The percentiles are created using the 2023 data collected by the Hungarian Central Bank (MNB) and modified to match the estimated wealth increase between 2023 and 2025. The calculation takes into account the value of your primary residence, so include that as well. All calculations are made in euro.</p>
  </div>

  <table class="table table-bordered bg-white" id="nw-table">
    <thead class="table-light">
      <tr>
        <th style="width:15%">Categories</th>
        <th>Items</th>
        <th class="text-end" style="width:15%">Market value (€)</th>
        <th class="text-end" style="width:15%">Mortgage (€)</th>
        <th class="text-end" style="width:15%">Net value</th>
        <th style="width:4%"></th>
      </tr>
    </thead>
    <tbody id="rows">
      <tr class="table-secondary"><td colspan="6">🏠 Real estate</td></tr>
      <tr id="prop-anchor"></tr>
      <tr><td colspan="6"><button class="btn btn-outline-secondary" id="add-prop" type="button">➕ Add property</button></td></tr>

      <tr class="table-secondary"><td colspan="6">📊 Investments</td></tr>
      <tr data-type="inv"><td>📦</td><td>Private pension</td>
        <td><input type="number" data-field="value" value="0"></td><td><input type="number" data-field="debt" value="0" disabled></td>
        <td class="text-end mono" data-cell="net">€0</td><td></td></tr>
      <tr data-type="inv"><td>🏛️</td><td>Government bonds</td>
        <td><input type="number" data-field="value" value="0"></td><td><input type="number" data-field="debt" value="0" disabled></td>
        <td class="text-end mono" data-cell="net">€0</td><td></td></tr>
      <tr data-type="inv"><td>🧾</td><td>Tax-efficient investments</td>
        <td><input type="number" data-field="value" value="0"></td><td><input type="number" data-field="debt" value="0" disabled></td>
        <td class="text-end mono" data-cell="net">€0</td><td></td></tr>
      <tr data-type="inv"><td>📈</td><td>Other investments</td>
        <td><input type="number" data-field="value" value="0"></td><td><input type="number" data-field="debt" value="0" disabled></td>
        <td class="text-end mono" data-cell="net">€0</td><td></td></tr>

      <tr class="table-secondary"><td colspan="6">💶 Other assets</td></tr>
      <tr data-type="asset"><td>🚗</td><td>Cars and other assets</td>
        <td><input type="number" data-field="value" value="0"></td><td><input type="number" data-field="debt" value="0" disabled></td>
        <td class="text-end mono" data-cell="net">€0</td><td></td></tr>

      <tr class="table-secondary"><td colspan="6">➖ Liabilities</td></tr>
      <tr data-type="liab"><td>💳</td><td>Other liabilities</td>
        <td><input type="number" data-field="value" value="0" disabled></td>
        <td><input type="number" data-field="debt" value="0"></td>
        <td class="text-end mono" data-cell="net">€0</td><td></td></tr>

      <tr class="fw-bold table-light">
        <td colspan="2">Total</td>
        <td class="text-end" id="sum-value">€0</td>
        <td class="text-end" id="sum-debt">€0</td>
        <td class="text-end" id="sum-net">€0</td>
        <td></td>
      </tr>
    </tbody>
  </table>

  <div class="result mb-3">
    <strong>Net worth:</strong> <span id="nw-eur">€0</span>
    <span class="badge text-bg-primary" id="pct-chip">Percentile: –</span>
  </div>

  <div class="chart-wrap"><canvas id="percentileChart"></canvas></div>
  <div id="pct-text" class="mt-2 small text-muted">Your totals are automatically updated when entering new values.</div>
</div>

<script>
(function(){
  const PCT = [
3645300, 1196700, 903800, 729100, 635000, 558500, 503400, 448800, 415400, 406100, 383700, 366100, 348400, 333400, 315600, 308000, 299700, 288900, 278100, 269500, 258600, 247800, 238800, 227800, 216900, 211200, 205400, 199700, 193900, 188100, 182300, 176500, 170800, 165000, 159200, 157600, 153400, 149200, 145000, 140800, 136600, 132400, 128200, 124000, 119800, 117900, 115000, 112000, 109100, 106100, 103200, 100300, 97300, 94400, 91400, 90400, 87700, 85100, 82500, 79900, 77200, 74600, 72000, 69400, 66700, 65700, 63100, 60400, 57800, 55200, 52500, 49900, 47300, 44700, 42000, 40500, 38400, 36300, 34200, 32100, 30000, 27800, 25700, 23600, 21500, 20800, 18900, 17100, 15200, 13400, 11600, 9700, 7900, 6000, 4200, 3400, 2500, 1700, 800, 0
  ];
  const LABELS = Array.from({length:100},(_,i)=>String(100-i));
  let chart;

  function initChart(){
    const canvas = document.getElementById("percentileChart");
    if(!canvas || !window.Chart) return;
    const ctx = canvas.getContext('2d');

    chart = new Chart(ctx, {
  type: "bar",
  data: {
    labels: LABELS,
    datasets: [{
      data: [...PCT].reverse(),
      backgroundColor: "#cfd8dc",
      borderWidth: 0
    }]
  },
  options: {
    responsive: true,
    maintainAspectRatio: false,
    plugins: {
      legend: { display: false },
      tooltip: {
        callbacks: {
          title: it => it && it[0] ? ("Percentile " + it[0].label) : "",
          label: it => "Median wealth €" + Number(it.raw).toLocaleString()
        }
      }
    },
    scales: {
      x: {
        grid: { display: false },
        ticks: { autoSkip: true, maxRotation: 0 }
      },
      y: {
        beginAtZero: true,
        max: 4000000, // 👈 Added to cap Y-axis at €4 million
        ticks: {
          callback: v => "€" + v.toLocaleString()
        }
      }
    }
  }
});

  function highlight(p){
    if(!chart) return;
    const idx = 100 - p;
    const colors = new Array(100).fill("#cfd8dc");
    if (idx>=0 && idx<colors.length) colors[idx]="#0d6efd";
    chart.data.datasets[0].backgroundColor = colors;
    chart.update();
  }

  function addProperty(){
    const anchor=document.getElementById("prop-anchor");
    const tr=document.createElement("tr"); tr.dataset.type="prop";
    tr.innerHTML=`<td>🏠</td><td>Property</td>
      <td><input type="number" data-field="value" value="0"></td>
      <td><input type="number" data-field="debt" value="0"></td>
      <td class="text-end mono" data-cell="net">€0</td>
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
      r.querySelector('[data-cell="net"]').textContent="€"+n.toLocaleString();
    });
    document.getElementById("sum-value").textContent="€"+sumV.toLocaleString();
    document.getElementById("sum-debt").textContent="€"+sumD.toLocaleString();
    document.getElementById("sum-net").textContent="€"+sumN.toLocaleString();
    document.getElementById("nw-eur").textContent="€"+sumN.toLocaleString();

    let pct=100; for(let i=0;i<PCT.length;i++){ if(sumN>=PCT[i]){ pct=i+1; break; } }
    document.getElementById("pct-chip").textContent="Top "+pct+"%";
    document.getElementById("pct-text").textContent=`Your estimated net worth is €${sumN.toLocaleString()}, which places you ${100-pct}% higher than others (Top ${pct}%).`;
    highlight(pct);
  }

  // Biztosan csak akkor futunk, amikor minden betöltött
  if (document.readyState === "complete" || document.readyState === "interactive") {
    initChart(); addProperty(); compute();
  } else {
    document.addEventListener("DOMContentLoaded", ()=>{ initChart(); addProperty(); compute(); });
  }

  document.body.addEventListener("input",e=>{ if(e.target.matches("input")) compute(); });
  document.addEventListener("click",e=>{
    if(e.target.id==="add-prop"){ addProperty(); }
    if(e.target.tagName==="BUTTON" && e.target.closest("tr")?.dataset.type==="prop"){
      e.target.closest("tr").remove(); compute();
    }
  });
})();
</script>
