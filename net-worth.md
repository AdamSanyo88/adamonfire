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
    <p>This calculator shows you how wealthy your household was at the end of 2024 compared to all household in Hungary. The percentiles are created using the 2023 data collected by the Hungarian Central Bank (MNB) and modified to match the estimated wealth increase between 2023 and 2024. The calculation takes into account the value of your primary residence, so include that as well. All calculations are made in euro.</p>
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
    3160000,997801,751807,601008,522823,455581,412012,369040,339199,331937,311980,297613,283245,268878,254510,248353,239732,231112,222491,213871,205250,196630,188009,179389,170768,165021,160506,155990,151475,146959,142444,137928,133413,128897,124382,123150,119866,116582,113298,110014,106730,103446,100162,96878,93594,92116,89817,87519,85220,82921,80622,78323,76025,73726,71427,70606,68554,66501,64449,62396,60344,58291,56239,54186,52134,51313,49260,47208,45155,43103,41050,38998,36945,34893,32840,31609,29967,28325,26683,25041,23399,21757,20115,18473,16831,16215,14778,13341,11905,10468,9031,7594,6158,4721,3284,2627,1970,1314,657,0
  ];
  const LABELS = Array.from({length:100},(_,i)=>String(100-i));
  let chart;

  function initChart(){
    const canvas = document.getElementById("percentileChart");
    if(!canvas || !window.Chart) return;
    const ctx = canvas.getContext('2d');

    chart = new Chart(ctx,{
      type:"bar",
      data:{
        labels:LABELS,
        datasets:[{data:[...PCT].reverse(),backgroundColor:"#cfd8dc",borderWidth:0}]
      },
      options:{
        responsive:true,maintainAspectRatio:false,
        plugins:{
          legend:{display:false},
          tooltip:{callbacks:{
            title:it => it && it[0] ? ("Percentilis "+it[0].label) : "",
            label:it => "Küszöb €"+Number(it.raw).toLocaleString()
          }}
        },
        scales:{
          x:{grid:{display:false},ticks:{autoSkip:true,maxRotation:0}},
          y:{beginAtZero:true,ticks:{callback:v=>"€"+v.toLocaleString()}}
        }
      }
    });
  }

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
    tr.innerHTML=`<td>🏠</td><td>Real estate</td>
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
