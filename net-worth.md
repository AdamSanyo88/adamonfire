---
layout: page
title: Useful resources
permalink: /net-worth
---

<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Net Worth Calculator</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <style>
    body.dark-mode {
      background-color: #121212;
      color: #ffffff;
    }
    .card {
      margin-bottom: 20px;
    }
    .result {
      font-weight: bold;
      font-size: 1.2em;
    }
    .bg-orange { background-color: #ffe5b4; }
    .bg-green { background-color: #d4edda; }
    .bg-blue { background-color: #d1ecf1; }
    .bg-red { background-color: #f8d7da; }
    .bg-lightblue { background-color: #b3e5fc; }
    }
  </style>
</head>
<body class="bg-light">
  <div class="container py-4">
    <div class="d-flex justify-content-between align-items-center mb-4">
      <h1>Net Worth & Percentile Calculator</h1>
      <button class="btn btn-secondary" onclick="toggleDarkMode()">Toggle Dark Mode</button>
    </div>

    <div class="row">
      <div class="col-md-6">
        <div class="card bg-orange">
          <div class="card-body">
            <h4>🏠 Real Estate</h4>
            <label for="propertyCount" class="form-label">Number of Properties:</label>
            <input type="number" class="form-control" id="propertyCount" min="0" value="0" onchange="generatePropertyInputs()" />
            <div id="propertyInputs"></div>
            <div class="mt-3"><strong>Total Real Estate: </strong><span id="realEstateTotal">€0</span></div>
          </div>
        </div>

        <div class="card bg-green">
          <div class="card-body">
            <h4>📊 Investments</h4>
            <label class="form-label">Private Pension:</label>
            <input type="number" class="form-control" id="privatePension" value="0" />
            <label class="form-label">Government Bonds:</label>
            <input type="number" class="form-control" id="govBonds" value="0" />
            <label class="form-label">Tax-efficient Investments:</label>
            <input type="number" class="form-control" id="taxInvestments" value="0" />
            <label class="form-label">Other Investments:</label>
            <input type="number" class="form-control" id="otherInvestments" value="0" />
            <div class="mt-3"><strong>Total Investments: </strong><span id="investmentTotal">€0</span></div>
          </div>
        </div>

        <div class="card bg-blue">
          <div class="card-body">
            <h4>💶 Other Assets</h4>
            <label class="form-label">Cars and Other Assets:</label>
            <input type="number" class="form-control" id="otherAssets" value="0" />
            <div class="mt-3"><strong>Total Other Assets: </strong><span id="otherAssetsTotal">€0</span></div>
          </div>
        </div>

        <div class="card bg-red">
          <div class="card-body">
            <h4>➖ Liabilities</h4>
            <label class="form-label">Other Liabilities:</label>
            <input type="number" class="form-control" id="otherLiabilities" value="0" />
            <div class="mt-3"><strong>Total Liabilities: </strong><span id="liabilitiesTotal">€0</span></div>
          </div>
        </div>
      </div>

      <div class="col-md-6">
        <div class="card bg-lightblue">
          <div class="card-body">
            <h4>📈 Results</h4>
            <button class="btn btn-primary mb-3" onclick="calculateNetWorth()">Calculate Net Worth</button>
            <div id="result" class="result mb-3"></div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <script>
    const percentiles = [
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

    function toggleDarkMode() {
      document.body.classList.toggle('dark-mode');
      document.body.classList.toggle('bg-light');
      document.body.classList.toggle('bg-dark');
    }

    function generatePropertyInputs() {
      const count = parseInt(document.getElementById("propertyCount").value) || 0;
      const container = document.getElementById("propertyInputs");
      container.innerHTML = "";

      for (let i = 0; i < count; i++) {
        const div = document.createElement("div");
        div.classList.add("mb-3");
        div.innerHTML = `
          <h5>Property ${i + 1}</h5>
          <label class="form-label">Property Value:</label>
          <input type="number" class="form-control" id="propertyValue${i}" value="0" />
          <label class="form-label">Remaining Mortgage:</label>
          <input type="number" class="form-control" id="propertyMortgage${i}" value="0" />
        `;
        container.appendChild(div);
      }
    }

    function calculateNetWorth() {
      let netWorth = 0;
      const count = parseInt(document.getElementById("propertyCount").value) || 0;
      let realEstateTotal = 0;

      for (let i = 0; i < count; i++) {
        const value = parseFloat(document.getElementById(`propertyValue${i}`).value) || 0;
        const mortgage = parseFloat(document.getElementById(`propertyMortgage${i}`).value) || 0;
        realEstateTotal += (value - mortgage);
      }

      let investments =
        (parseFloat(document.getElementById("privatePension").value) || 0) +
        (parseFloat(document.getElementById("govBonds").value) || 0) +
        (parseFloat(document.getElementById("taxInvestments").value) || 0) +
        (parseFloat(document.getElementById("otherInvestments").value) || 0);

      let otherAssets = parseFloat(document.getElementById("otherAssets").value) || 0;
      let liabilities = parseFloat(document.getElementById("otherLiabilities").value) || 0;

      netWorth = realEstateTotal + investments + otherAssets - liabilities;

      let percentile = 100;
      for (let i = 0; i < percentiles.length; i++) {
        if (netWorth >= percentiles[i]) {
          percentile = i + 1;
          break;
        }
      }

      document.getElementById("realEstateTotal").innerText = `€${realEstateTotal.toLocaleString()}`;
      document.getElementById("investmentTotal").innerText = `€${investments.toLocaleString()}`;
      document.getElementById("otherAssetsTotal").innerText = `€${otherAssets.toLocaleString()}`;
      document.getElementById("liabilitiesTotal").innerText = `€${liabilities.toLocaleString()}`;

      document.getElementById("result").innerText =
        `Estimated Net Worth: €${netWorth.toLocaleString()}\nEstimated Wealth Percentile: ${percentile}th percentile`;

      drawChart(netWorth);
    }

    function drawChart(userNetWorth) {
      const ctx = document.getElementById("percentileChart").getContext("2d");
      if (window.percentileChart) window.percentileChart.destroy();
      const reversed = [...percentiles].reverse();
      const barColors = reversed.map(value => value < userNetWorth ? '#0d6efd' : '#ccc');
      window.percentileChart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: Array.from({ length: 100 }, (_, i) => `${100 - i}`),
          datasets: [{
            data: reversed,
            backgroundColor: barColors,
            borderWidth: 1
          }]
        },
        options: {
          responsive: true,
          plugins: {
            legend: { display: false },
            tooltip: { mode: 'index' }
          },
          scales: {
            x: { reverse: true, ticks: { autoSkip: true, maxRotation: 90, minRotation: 90 } },
            y: { beginAtZero: true }
          }
        }
      });
    }
  </script>
</body>
</html>

