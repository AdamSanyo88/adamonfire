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
    4192100, 1376200, 1039400, 838500, 730300, 642200, 578900, 516100, 477800, 467000, 441300, 421000, 400700, 383400, 362900, 354200, 344600, 332200, 319800, 309900, 297400, 284900, 274600, 262000, 249400, 242900, 236300, 229600, 223000, 216300, 209700, 203000, 196400, 189700, 183100, 181300, 176400, 171600, 166800, 161900, 157100, 152300, 147400, 142600, 137800, 135600, 132200, 128800, 125400, 122100, 118700, 115300, 111900, 108500, 105100, 103900, 100900, 97900, 94900, 91800, 88800, 85800, 82800, 79800, 76700, 75500, 72500, 69500, 66500, 63400, 60400, 57400, 54400, 51400, 48300, 46500, 44100, 41700, 39300, 36900, 34400, 32000, 29600, 27200, 24800, 23900, 21800, 19600, 17500, 15400, 13300, 11200, 9100, 7000, 4800, 3900, 2900, 1900, 1000,0
  ];
  const LABELS = Array.from({length: PCT.length}, (_, i) => String(PCT.length - i));
  let chart;

  function initChart(){
    const canvas = document.getElementById("percentileChart");
    if (!canvas || !window.Chart) return;
    const ctx = canvas.getContext("2d");

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
            max: 4500000,
            ticks: { callback: v => "€" + v.toLocaleString() }
          }
        }
      }
    });
  } // ✅ properly close initChart

  function highlight(p){
    if (!chart) return;
    const bars = chart.data.datasets[0].data.length;
    const idx = (PCT.length) - p; // dataset is reversed vs percentile
    const colors = Array.from({length: bars}, () => "#cfd8dc");
    if (idx >= 0 && idx < colors.length) colors[idx] = "#0d6efd";
    chart.data.datasets[0].backgroundColor = colors;
    chart.update();
  }

  function addProperty(){
    const anchor = document.getElementById("prop-anchor");
    const tr = document.createElement("tr");
    tr.dataset.type = "prop";
    tr.innerHTML = `<td>🏠</td><td>Property</td>
      <td><input type="number" data-field="value" value="0" inputmode="decimal"></td>
      <td><input type="number" data-field="debt" value="0" inputmode="decimal"></td>
      <td class="text-end mono" data-cell="net">€0</td>
      <td><button class="btn btn-sm btn-outline-danger" type="button">✖</button></td>`;
    (anchor?.parentNode || document.querySelector("#rows")).insertBefore(tr, anchor || null);
  }

  function compute(){
    let sumV = 0, sumD = 0, sumN = 0;
    document.querySelectorAll("#rows tr[data-type]").forEach(r=>{
      const t = r.dataset.type;
      const v = +r.querySelector('[data-field="value"]')?.value || 0;
      const d = +r.querySelector('[data-field="debt"]')?.value || 0;
      let n = 0;
      if (t === "prop") n = v - d;
      else if (t === "inv" || t === "asset") n = v;
      else if (t === "liab") n = -d;

      sumV += v; sumD += d; sumN += n;
      const netCell = r.querySelector('[data-cell="net"]');
      if (netCell) netCell.textContent = "€" + n.toLocaleString();
    });

    document.getElementById("sum-value").textContent = "€" + sumV.toLocaleString();
    document.getElementById("sum-debt").textContent  = "€" + sumD.toLocaleString();
    document.getElementById("sum-net").textContent   = "€" + sumN.toLocaleString();
    document.getElementById("nw-eur").textContent    = "€" + sumN.toLocaleString();

    // Percentile: find the first threshold not greater than sumN
    let pct = PCT.length;
    for (let i = 0; i < PCT.length; i++) {
      if (sumN >= PCT[i]) { pct = i + 1; break; }
    }
    const higherThan = Math.max(0, PCT.length - pct);
    document.getElementById("pct-chip").textContent = "Top " + pct + "%";
    document.getElementById("pct-text").textContent =
      `Your estimated net worth is €${sumN.toLocaleString()}, which places you ${higherThan}% higher than others (Top ${pct}%).`;

    highlight(pct);
  }

  // Run when ready
  const start = () => { initChart(); addProperty(); compute(); };
  if (document.readyState === "complete" || document.readyState === "interactive") {
    start();
  } else {
    document.addEventListener("DOMContentLoaded", start);
  }

  // Recompute on input; support add/remove property
  document.body.addEventListener("input", e => { if (e.target.matches("input[type=number]")) compute(); });
  document.addEventListener("click", e => {
    if (e.target.id === "add-prop") { addProperty(); }
    if (e.target.closest("tr")?.dataset.type === "prop" && e.target.matches("button")) {
      e.target.closest("tr").remove(); compute();
    }
  });
})();
</script>

