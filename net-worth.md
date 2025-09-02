---
layout: page
title: Net worth calculator
permalink: /net-worth
---

<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Net Worth Calculator</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
  <style>
    body { background:#f8f9fa; color:#212529; }
    .card { margin-bottom: 20px; }
    .result { font-weight: 600; font-size: 1.1rem; white-space: pre-line; }
    .bg-orange { background-color: #ffe5b4; }
    .bg-green { background-color: #d4edda; }
    .bg-blue { background-color: #d1ecf1; }
    .bg-red { background-color: #f8d7da; }
    .bg-lightblue { background-color: #b3e5fc; }
    .chart-wrap { height: 360px; }
    .muted { opacity: .85; }
    /* Prevent underline on navigation icons/links */
    a, a:hover, a:focus, a:active { text-decoration: none !important; }
  </style>
</head>
<body>
  <div class="container py-4">
    <div class="d-flex justify-content-between align-items-center mb-4">
      <h1 class="h3 m-0">Net Worth & Percentile Calculator</h1>
    </div>

    <div class="row">
      <div class="col-lg-6">
        <div class="card bg-orange">
          <div class="card-body">
            <h4 class="h5">🏠 Real Estate</h4>
            <label for="propertyCount" class="form-label">Number of Properties</label>
            <input type="number" class="form-control" id="propertyCount" min="0" value="0" onchange="generatePropertyInputs()"/>
            <div id="propertyInputs"></div>
            <div class="mt-3"><strong>Total Real Estate:</strong> <span id="realEstateTotal">€0</span></div>
          </div>
        </div>

        <div class="card bg-green">
          <div class="card-body">
            <h4 class="h5">📊 Investments</h4>
            <label class="form-label">Private Pension</label>
            <input type="number" class="form-control" id="privatePension" value="0"/>
            <label class="form-label mt-2">Government Bonds</label>
            <input type="number" class="form-control" id="govBonds" value="0"/>
            <label class="form-label mt-2">Tax-efficient Investments</label>
            <input type="number" class="form-control" id="taxInvestments" value="0"/>
            <label class="form-label mt-2">Other Investments</label>
            <input type="number" class="form-control" id="otherInvestments" value="0"/>
            <div class="mt-3"><strong>Total Investments:</strong> <span id="investmentTotal">€0</span></div>
          </div>
        </div>

        <div class="card bg-blue">
          <div class="card-body">
            <h4 class="h5">💶 Other Assets</h4>
            <label class="form-label">Cars & Other Assets</label>
            <input type="number" class="form-control" id="otherAssets" value="0"/>
            <div class="mt-3"><strong>Total Other Assets:</strong> <span id="otherAssetsTotal">€0</span></div>
          </div>
        </div>

        <div class="card bg-red">
          <div class="card-body">
            <h4 class="h5">➖ Liabilities</h4>
            <label class="form-label">Other Liabilities</label>
            <input type="number" class="form-control" id="otherLiabilities" value="0"/>
            <div class="mt-3"><strong>Total Liabilities:</strong> <span id="liabilitiesTotal">€0</span></div>
          </div>
        </div>
      </div>

      <div class="col-lg-6">
        <div class="card bg-lightblue">
          <div class="card-body">
            <h4 class="h5">📈 Results</h4>
            <button class="btn btn-primary mb-3" id="calcBtn">Calculate Net Worth</button>
            <div id="result" class="result mb-3">Click “Calculate Net Worth” to update your percentile.</div>

            <div class="small muted mb-1">Percentile chart (100 → 1)</div>
            <div class="chart-wrap">
              <canvas id="percentileChart" aria-label="Percentile chart" role="img"></canvas>
            </div>
            <div class="small mt-2" id="percentileSummary">Your position will be highlighted in blue.</div>
          </div>
        </div>
      </div>
    </div>
  </div>

<script>
/**
 * Percentile thresholds:
 * index 0 => Top 1%, index 99 => Top 100% (bottom)
 * Values are the minimum net worth for that percentile.
 */
const PCT_THRESHOLDS = [
  3160000, 997801, 751807, 601008, 522823, 455581, 412012, 369040, 339199, 331937,
  311980, 297613, 283245, 268878, 254510, 248353, 239732, 231112, 222491, 213871,
  205250, 196630, 188009, 179389, 170768, 165021, 160506, 155990, 151475, 146959,
  142444, 137928, 133413, 128897, 124382, 123150, 119866, 116582, 113298, 110014,
  106730, 103446, 100162, 96878, 93594, 92116, 89817, 87519, 85220, 82921,
  80622, 78323, 76025, 73726, 71427, 70606, 68554, 66501, 64449, 62396,
  60344, 58291, 56239, 54186, 52134, 51313, 49260, 47208, 45155, 43103,
  41050, 38998, 36945, 34893, 32840, 31609, 29967, 28325, 26683, 25041,
  23399, 21757, 20115, 18473, 16831, 16215, 14778, 13341, 11905, 10468,
  9031, 7594, 6158, 4721, 3284, 2627, 1970, 1314, 657, 0
];

// Build labels 100 → 1 and base data once
const CHART_LABELS = Array.from({ length: 100 }, (_, i) => String(100 - i));
const CHART_DATA   = [...PCT_THRESHOLDS].reverse(); // align to labels (100→1)
let percentileChart = null;

function generatePropertyInputs() {
  const count = parseInt(document.getElementById("propertyCount").value) || 0;
  const container = document.getElementById("propertyInputs");
  container.innerHTML = "";
  for (let i = 0; i < count; i++) {
    const div = document.createElement("div");
    div.classList.add("mb-3");
    div.innerHTML = `
      <h5 class="h6 mt-3">Property ${i + 1}</h5>
      <label class="form-label">Property Value</label>
      <input type="number" class="form-control" id="propertyValue${i}" value="0"/>
      <label class="form-label mt-2">Remaining Mortgage</label>
      <input type="number" class="form-control" id="propertyMortgage${i}" value="0"/>
    `;
    container.appendChild(div);
  }
}

function calculateNetWorthAndPercentile() {
  const count = parseInt(document.getElementById("propertyCount").value) || 0;
  let realEstateTotal = 0;
  for (let i = 0; i < count; i++) {
    const v = parseFloat(document.getElementById(`propertyValue${i}`)?.value) || 0;
    const m = parseFloat(document.getElementById(`propertyMortgage${i}`)?.value) || 0;
    realEstateTotal += (v - m);
  }

  const investments =
    (parseFloat(document.getElementById("privatePension").value) || 0) +
    (parseFloat(document.getElementById("govBonds").value) || 0) +
    (parseFloat(document.getElementById("taxInvestments").value) || 0) +
    (parseFloat(document.getElementById("otherInvestments").value) || 0);

  const otherAssets = parseFloat(document.getElementById("otherAssets").value) || 0;
  const liabilities = parseFloat(document.getElementById("otherLiabilities").value) || 0;

  const netWorth = realEstateTotal + investments + otherAssets - liabilities;

  document.getElementById("realEstateTotal").innerText = `€${realEstateTotal.toLocaleString()}`;
  document.getElementById("investmentTotal").innerText = `€${investments.toLocaleString()}`;
  document.getElementById("otherAssetsTotal").innerText = `€${otherAssets.toLocaleString()}`;
  document.getElementById("liabilitiesTotal").innerText = `€${liabilities.toLocaleString()}`;

  // Find percentile: first threshold that netWorth >= threshold
  let percentile = 100;
  for (let i = 0; i < PCT_THRESHOLDS.length; i++) {
    if (netWorth >= PCT_THRESHOLDS[i]) { percentile = i + 1; break; }
  }

  return { netWorth, percentile };
}

// Build labels 100 → 1 and base data once (ensure these are defined above)
const CHART_LABELS = Array.from({ length: 100 }, (_, i) => String(100 - i));
const CHART_DATA   = [...PCT_THRESHOLDS].reverse(); // align to labels (100→1)

let percentileChart = null;

// Initialize chart ONCE so it is always visible
function initChart() {
  const canvas = document.getElementById("percentileChart");
  if (!canvas || !window.Chart) return;            // guard: DOM or lib not ready yet
  const ctx = canvas.getContext("2d");
  if (!ctx) return;                                // guard: canvas not ready

  // Safe re-init if something tried to create it before
  if (percentileChart) percentileChart.destroy();

  const baseColors = CHART_DATA.map(() => "#b9c2cf");

  percentileChart = new Chart(ctx, {
    type: "bar",
    data: {
      labels: CHART_LABELS,
      datasets: [{
        data: CHART_DATA,
        backgroundColor: baseColors,
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
            title: (items) => `Percentile: ${items[0].label}`,
            label:  (item)  => `Threshold: €${Number(item.raw).toLocaleString()}`
          }
        }
      },
      scales: {
        x: { grid: { display: false }, ticks: { maxRotation: 0, autoSkip: true } },
        y: { beginAtZero: true, ticks: { callback: (v) => `€${v.toLocaleString()}` } }
      }
    }
  });
}

// Update only the highlight after clicking the button
function highlightPercentile(percentile) {
  // If chart isn't ready (e.g., init was skipped), try to init now
  if (!percentileChart) initChart();
  if (!percentileChart) return; // still not ready → bail gracefully

  // Map percentile p to index (100 → 0, 1 → 99)
  const p = Math.max(1, Math.min(100, percentile));
  const highlightIndex = 100 - p;

  const colors = CHART_DATA.map(() => "#b9c2cf");
  colors[highlightIndex] = "#0d6efd";

  percentileChart.data.datasets[0].backgroundColor = colors;
  percentileChart.update();
}

// Wire up events
document.addEventListener("DOMContentLoaded", () => {
  // Ensure chart is visible immediately.
  if (window.Chart) initChart();
  else window.addEventListener("load", initChart); // fallback if Chart.js loads slowly

  document.getElementById("calcBtn").addEventListener("click", () => {
    const { netWorth, percentile } = calculateNetWorthAndPercentile();

    // "above X% of people": Top 1% ⇒ ~100%, Top 100% ⇒ 0%
    const above = Math.max(0, Math.min(100, 101 - percentile));

    document.getElementById("result").textContent =
      `Estimated Net Worth: €${netWorth.toLocaleString()}\nEstimated Wealth Percentile: Top ${percentile}%`;

    document.getElementById("percentileSummary").textContent =
      `You’re above approximately ${above}% of people (Top ${percentile}%).`;

    highlightPercentile(percentile);
  });
});

</body>
</html>
