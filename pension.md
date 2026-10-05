---
layout: page
title: Pension calculator
permalink: /pension
---

<h1 class="page-title">{{ page.title | escape }}</h1>
 
<html lang="hu">
<head>
  <meta charset="utf-8" />
<style>
:root {
  --bg: #f6f8fb;
  --card: #ffffff;
  --muted: #64748b;
  --accent: #1565c0;
  --accent-dark: #0d47a1;
  --accent-soft: #eaf3ff;
  --text: #172033;
  --border: #e2e8f0;
  --success: #0f766e;
}
* {
  box-sizing: border-box;
  font-family: Inter, system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
}
body {
  margin: 0;
  background: #fff;
  color: var(--text);
}

.wrap {
  width: 100%;
  max-width: 1180px;
  margin: 30px auto 56px;
  padding: 0 16px;
}

h1 { font-size: 28px; margin: 0 0 6px; }
p.lead { margin: 0 0 24px; color: var(--muted); }

/* Calculator highlight */
.calculator-shell {
  margin-top: 34px;
  background: linear-gradient(145deg, #f8fbff 0%, #eef5ff 100%);
  border: 1px solid #d8e7fb;
  border-radius: 24px;
  padding: 26px;
  box-shadow: 0 18px 50px rgba(22, 67, 120, 0.12);
}
.calculator-heading {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 20px;
  margin-bottom: 20px;
}
.calculator-heading h3 { margin: 0 0 6px; font-size: 26px; }
.calculator-heading p { margin: 0; color: var(--muted); }
.calculator-badge {
  display: inline-flex;
  align-items: center;
  white-space: nowrap;
  padding: 7px 12px;
  border-radius: 999px;
  background: #fff;
  border: 1px solid #cfe1f7;
  color: var(--accent-dark);
  font-size: 13px;
  font-weight: 700;
}

.summary-grid {
  display: grid;
  grid-template-columns: minmax(0, 1.15fr) minmax(300px, .85fr);
  gap: 18px;
  margin-bottom: 18px;
}
.card {
  background: var(--card);
  border-radius: 18px;
  padding: 22px;
  border: 1px solid var(--border);
  box-shadow: 0 8px 24px rgba(15, 23, 42, 0.06);
}

.input-card h5,
.income-card h5 { margin: 0 0 8px; }
.step-kicker {
  display: block;
  margin-bottom: 6px;
  color: var(--accent);
  font-size: 12px;
  font-weight: 800;
  letter-spacing: .08em;
  text-transform: uppercase;
}
.service-control {
  display: grid;
  grid-template-columns: 1fr auto;
  align-items: center;
  gap: 16px;
  margin-top: 20px;
}
.service-slider-wrap { min-width: 0; }
.service-range-labels {
  display: flex;
  justify-content: space-between;
  margin-top: 7px;
  color: #94a3b8;
  font-size: 12px;
}
#serviceYearsLabel {
  min-width: 92px;
  padding: 12px 14px;
  border-radius: 14px;
  background: var(--accent-soft);
  color: var(--accent-dark);
  text-align: center;
  font-size: 20px;
  font-weight: 800;
  white-space: nowrap;
}
#serviceYears {
  width: 100% !important;
  margin: 0;
  appearance: none;
  background: transparent;
  height: 22px;
  padding: 0;
}
#serviceYears::-webkit-slider-runnable-track { height: 6px; background: #d6e1ee; border-radius: 999px; }
#serviceYears::-moz-range-track { height: 6px; background: #d6e1ee; border-radius: 999px; }
#serviceYears::-webkit-slider-thumb {
  appearance: none;
  width: 22px;
  height: 22px;
  margin-top: -8px;
  border-radius: 50%;
  background: var(--accent);
  border: 3px solid #fff;
  box-shadow: 0 2px 8px rgba(21,101,192,.35);
  cursor: pointer;
}
#serviceYears::-moz-range-thumb {
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background: var(--accent);
  border: 3px solid #fff;
  box-shadow: 0 2px 8px rgba(21,101,192,.35);
  cursor: pointer;
}

.result-card {
  position: relative;
  overflow: hidden;
  color: #fff;
  border: 0;
  background: linear-gradient(135deg, var(--accent-dark), #1976d2 60%, #42a5f5);
  box-shadow: 0 16px 34px rgba(13, 71, 161, .24);
}
.result-card::after {
  content: '';
  position: absolute;
  width: 190px;
  height: 190px;
  border-radius: 50%;
  right: -65px;
  top: -90px;
  background: rgba(255,255,255,.10);
}
.result-label {
  position: relative;
  z-index: 1;
  margin-bottom: 8px;
  font-size: 13px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: .07em;
  opacity: .88;
}
.result {
  position: relative;
  z-index: 1;
  display: flex;
  flex-direction: column;
  gap: 4px;
  font-size: clamp(34px, 4vw, 48px);
  line-height: 1.05;
  font-weight: 850;
  letter-spacing: -.02em;
  font-variant-numeric: tabular-nums;
}
.result small {
  font-size: 14px;
  line-height: 1.4;
  font-weight: 600;
  color: rgba(255,255,255,.82);
}
.result-divider { border: 0; border-top: 1px solid rgba(255,255,255,.22); margin: 18px 0 14px; }
.result-card .muted { color: rgba(255,255,255,.82); }
.result-card #breakdown { line-height: 1.65; }
.result-card #breakdown strong { color: #fff; }

.income-card { padding: 0; overflow: hidden; }
.income-head { padding: 22px 22px 16px; }
.income-head p { margin: 4px 0 0; color: var(--muted); }
.quick-fill-panel {
  margin: 0 22px 18px;
  padding: 16px;
  border-radius: 14px;
  background: #f8fafc;
  border: 1px solid var(--border);
}
.inline-label {
  display: block;
  margin-bottom: 10px;
  font-weight: 700;
  color: #334155;
}
.btn-group { display: flex; flex-wrap: wrap; gap: 8px; }
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  padding: 10px 16px;
  min-width: 130px;
  border-radius: 10px;
  border: 1px solid #cbd5e1;
  background: #fff;
  color: #334155;
  font-weight: 700;
  font-size: 14px;
  white-space: nowrap;
  text-align: center;
  transition: .18s ease;
  box-shadow: none;
}
.btn:hover { background: var(--accent-soft); border-color: #9cc2ee; color: var(--accent-dark); transform: translateY(-1px); }
.btn.sm { padding: 8px 11px; font-size: 13px; min-width: auto; }
#reset { background: #fff; }

.table-wrap {
  max-height: 58vh;
  overflow: auto;
  border-top: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
  background: #fff;
}
table { width: 100%; border-collapse: collapse; }
th, td {
  padding: 10px 11px;
  border-bottom: 1px solid #eef2f7;
  text-align: left;
  vertical-align: middle;
}
thead th {
  position: sticky;
  top: 0;
  background: #f8fafc;
  z-index: 2;
  color: #475569;
  font-size: 12px;
  line-height: 1.35;
  font-weight: 800;
}
tbody tr:hover { background: #f8fbff; }

input[type="number"] {
  width: 100%;
  min-width: 120px;
  max-width: 170px;
  height: 40px;
  padding: 7px 10px;
  border-radius: 9px;
  border: 2px solid #90caf9;
  background: #eaf4ff;
  color: #0f172a;
  font-weight: 700;
  box-shadow: inset 0 1px 2px rgba(15,23,42,.03), 0 1px 3px rgba(21,101,192,.08);
  transition: background .18s ease, border-color .18s ease, box-shadow .18s ease;
}
input[type="number"]::placeholder {
  color: #6b8fb3;
  font-weight: 600;
}
input[type="number"]:hover {
  background: #dfefff;
  border-color: #64a4e8;
}
input[type="number"]:focus {
  outline: none;
  background: #fff8dc;
  border-color: #f2b134;
  box-shadow: 0 0 0 4px rgba(242,177,52,.18);
}
.muted { color: var(--muted); font-size: 13px; }
.mono { font-variant-numeric: tabular-nums; }
.table-footer {
  display: flex;
  align-items: center;
  gap: 14px;
  flex-wrap: wrap;
  padding: 16px 22px 20px;
}
#serviceInfo { line-height: 1.45; }

/* Materialize range tooltip suppression */
#serviceYears + .thumb,
#serviceYears ~ .thumb,
#serviceYears + .thumb .value,
#serviceYears ~ .thumb .value,
.thumb .value,
.range-label, .value, .value-indicator, .mdc-slider__value-indicator, .noUi-tooltip {
  display: none !important;
  visibility: hidden !important;
  opacity: 0 !important;
  pointer-events: none !important;
}

@media (max-width: 900px) {
  .summary-grid { grid-template-columns: 1fr; }
  .calculator-shell { padding: 18px; border-radius: 20px; }
  .calculator-heading { flex-direction: column; }
  .result-card { order: -1; }
}
@media (max-width: 560px) {
  .wrap { padding: 0 10px; }
  .calculator-shell { padding: 12px; margin-top: 24px; }
  .card { padding: 17px; }
  .service-control { grid-template-columns: 1fr; }
  #serviceYearsLabel { width: 100%; }
  .income-head { padding: 18px 16px 12px; }
  .quick-fill-panel { margin: 0 16px 16px; padding: 13px; }
  .table-footer { padding: 14px 16px 18px; }
  .btn-group .btn { flex: 1 1 calc(50% - 8px); }
}
</style>
</head>
<body>
  <div class="wrap">
    <br/>
	<h5>How to use the calculator</h5>
    <p>First enter your <strong>number of years of service</strong> (which determines the multiplier applied to your earnings). Then enter your <strong>annual gross earnings for every year you worked</strong>. Only include officially declared and taxed income received as salary or bonuses.</p>
	<p>The calculator takes into account the <em>valuation multiplier</em> for each year, the <em>service-time multiplier</em>, and the progressive <em>degression</em>, and uses these to estimate your expected monthly pension.</p>
	<p><strong>Important:</strong> The calculator does not automatically apply the annual contribution ceiling that existed before 2013, but the table shows the maximum annual gross income on which pension contributions were payable. Do not enter a gross annual amount above this limit for those years.</p>
	<p>Always enter full-year earnings. The calculator continuously recalculates average lifetime earnings, so if you have only worked for a few years, it assumes you will continue earning at a similar level in the future. For a more accurate estimate, enter data for every year available.</p>
	<br/>
	<h5>How the pension calculation works in practice</h5>
	<ol>
	<li>The total number of service years accumulated over your lifetime, rounded down to a whole number (partial years are disregarded).</li>
	<li>The annual net value of income earned since January 1, 1988 (first pension contributions are deducted, then income tax is deducted from the remaining amount).</li>
	<li>The net values for each year must be multiplied by the annual valuation factor. This converts historical earnings into their present-day equivalent. In practice, the key question is whether your income in a given year was below or above that year’s average net earnings, which determines its relative value.</li>
	<li>The resulting amounts are divided by the total service period measured in days. The result is then multiplied by 365 and divided by 12 to obtain the average monthly net lifetime earnings.</li>
	<li>If the resulting amount exceeds HUF 372,000 per month, degression must be applied (first at 90%, then at 80% above HUF 421,000). </li>
	<li>Finally, the resulting amount is multiplied by the factor corresponding to your years of service (53% after 20 years, 68% after 30 years, 80% after 40 years, etc.). The maximum service period is 50 years and the minimum is 15 years (although the calculator can also perform estimates for 10–15 years).</li>
	</ol>
	<p>The figures are for informational purposes only. For a more precise calculation, use the <a href="https://www.allamkincstar.gov.hu/nyugdij/sajat-jogu-ellatasok/oregsegi-nyugdij/onkiszolgalo-nyugdijkalkulator">Hungarian State Treasury pension calculator</a>.</p>

  <div class="calculator-shell">
    <div class="calculator-heading">
      <div>
        <span class="step-kicker">Interactive calculator</span>
        <h3>Estimate your expected monthly pension</h3>
        <p>Enter your years of service and annual earnings. The result updates automatically whenever you make a change.</p>
      </div>
      <span class="calculator-badge">2026 calculation</span>
    </div>

    <div class="summary-grid">
      <div class="card input-card">
        <span class="step-kicker">Step 1</span>
        <h5>Your years of service</h5>
        <p class="muted">Set the total number of service years to include in the calculation.</p>

        <div class="service-control">
          <div class="service-slider-wrap">
            <input id="serviceYears" type="range" min="10" max="50" step="1" value="40" aria-label="Number of service years" />
            <div class="service-range-labels"><span>10 years</span><span>50 years</span></div>
          </div>
          <strong id="serviceYearsLabel">40 years</strong>
        </div>
      </div>

      <div class="card result-card">
        <div class="result-label">Estimated monthly pension</div>
        <div class="result" id="result">— <small>estimated monthly pension</small></div>
        <hr class="result-divider" />
        <div class="muted" id="breakdown"></div>
      </div>
    </div>

    <div class="card income-card">
      <div class="income-head">
        <span class="step-kicker">Step 2</span>
        <h5>Annual income subject to contributions</h5>
        <p>Enter your annual gross earnings for each year, or fill the table quickly using a typical salary level.</p>
      </div>

      <div class="quick-fill-panel">
        <span class="inline-label">Quick fill relative to the average gross salary</span>
        <div class="btn-group" aria-label="Quick-fill buttons based on percentages of average gross earnings">
          <button class="btn sm" type="button" id="fill40">40% · ~minimum wage</button>
          <button class="btn sm" type="button" id="fill60">60% · bottom ~30%</button>
          <button class="btn sm" type="button" id="fill80">80% · median wage</button>
          <button class="btn sm" type="button" id="fill100">100% · average wage</button>
          <button class="btn sm" type="button" id="fill150">150% · top 15%</button>
          <button class="btn sm" type="button" id="fill275">275% · top 5%</button>
        </div>
      </div>

      <div class="table-wrap">
        <table>
          <thead>
            <tr>
              <th>Year</th>
              <th>Multiplier</th>
              <th>Annual gross earnings (HUF)</th>
              <th>Valuated annual earnings</th>
              <th>Average annual gross earnings (reference)</th>
              <th>Annual contribution ceiling</th>
              <th>After contributions</th>
              <th>Tax payable</th>
              <th>Annual net earnings</th>
            </tr>
          </thead>
          <tbody id="rows"></tbody>
        </table>
      </div>

      <div class="table-footer">
        <button id="reset" class="btn" type="button">Clear all fields</button>
        <span id="serviceInfo" class="muted"></span>
      </div>
    </div>
  </div>
</div>

<script>
/* ===== Adatok ===== */
const YEARS=[1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023,2024,2025];
const YEAR_MULTS=[68.795, 58.849, 48.396, 38.562, 31.788, 27.012, 21.220, 18.843, 16.049, 12.935, 10.925, 9.693, 8.700, 7.489, 6.259, 5.481, 5.181, 4.707, 4.374, 4.244, 3.971, 3.898, 3.648, 3.429, 3.360, 3.202, 3.109, 2.981, 2.768, 2.450, 2.201, 1.976, 1.801, 1.657, 1.410, 1.235, 1.090, 1];
const ANNUAL_NET=[107616, 126852, 161352, 215208, 267528, 326076, 399708, 466800, 562044, 687600, 813600, 926400, 1051200, 1243200, 1470000, 1646400, 1748400, 1899600, 2054400, 2220000, 2386800, 2397600, 2431200, 2557200, 2676000, 2772000, 2852400, 2972400, 3158400, 3564000, 3958800, 4413600, 4843200, 5265600, 6189192, 7069368, 8008380, 8460000];
const CONST_TAX_LIMIT=[,,, ,750000,915000,912500,912500,915000,1204500,1565850,1854200,2020320,2197300,2368850,3905500,5307000,6000600,6325450,6748850,7137000,7446000,7453300,7665000,7942200];
const SERVICE_TABLE={10:33,11:35,12:37,13:39,14:41,15:43,16:45,17:47,18:49,19:51,20:53,21:55,22:57,23:59,24:61,25:63,26:64,27:65,28:66,29:67,30:68,31:69,32:70,33:71,34:72,35:73,36:74,37:75.5,38:77,39:78.5,40:80,41:82,42:84,43:86,44:88,45:90,46:92,47:94,48:96,49:98,50:100};

/* ===== Járulék (évesített) ===== */
function contributionRatePct(year){
  if(year<=1990) return 10.0;
  if(year===1991) return 10.2;
  if(year===1992) return 11.0;
  if(year===1993) return 12.0;
  if(year>=1994 && year<=1997) return 11.5;
  if(year===1998) return 11.5;
  if(year===1999) return 12.5;
  if(year>=2000 && year<=2003) return 12.5;
  if(year===2004) return 13.5;
  if(year===2005) return 16.5;
  if(year===2006) return 17.1;
  if(year===2007) return 19.5;
  if(year===2008) return 19.5;
  if(year===2009) return 19.5;
  if(year===2010) return 18;
  if(year===2011) return 17.5;
  if(year>=2012) return 18.5;
  return 18.5;
}

/* ===== Éves SZJA sávok ===== */
const TAX_TABLES = {
  1988: [
    {th:0,      rate:0,    cap:48000},
    {th:48000,  rate:0.20, cap:70000},
    {th:70000,  rate:0.25, cap:90000},
    {th:90000,  rate:0.30, cap:120000},
    {th:120000, rate:0.35, cap:150000},
    {th:150000, rate:0.39, cap:180000},
    {th:180000, rate:0.44, cap:240000},
    {th:240000, rate:0.48, cap:360000},
    {th:360000, rate:0.52, cap:600000},
    {th:600000, rate:0.56, cap:800000},
    {th:800000, rate:0.60}
  ],
  1989: [
    {th:0,      rate:0,    cap:55000},
    {th:55000,  rate:0.17, cap:70000},
    {th:70000,  rate:0.23, cap:100000},
    {th:100000, rate:0.29, cap:150000},
    {th:150000, rate:0.35, cap:240000},
    {th:240000, rate:0.42, cap:360000},
    {th:360000, rate:0.49, cap:600000},
    {th:600000, rate:0.56}
  ],
  1990: [
    {th:0,      rate:0,    cap:55000},
    {th:55000,  rate:0.15, cap:90000},
    {th:90000,  rate:0.30, cap:300000},
    {th:300000, rate:0.40, cap:500000},
    {th:500000, rate:0.50}
  ],
  1991: [
    {th:0,      rate:0,    cap:55000},
    {th:55000,  rate:0.12, cap:90000},
    {th:90000,  rate:0.18, cap:120000},
    {th:120000, rate:0.30, cap:150000},
    {th:150000, rate:0.32, cap:300000},
    {th:300000, rate:0.40, cap:500000},
    {th:500000, rate:0.50}
  ],
  1992: [
    {th:0,      rate:0,    cap:100000},
    {th:100000, rate:0.25, cap:200000},
    {th:200000, rate:0.35, cap:500000},
    {th:500000, rate:0.40}
  ],
  1993: 'sameAs:1992',
  1994: [
    {th:0,      rate:0,    cap:110000},
    {th:110000, rate:0.20, cap:150000},
    {th:150000, rate:0.25, cap:220000},
    {th:220000, rate:0.35, cap:380000},
    {th:380000, rate:0.40, cap:550000},
    {th:550000, rate:0.44}
  ],
  1995: 'sameAs:1994',
  1996: [
    {th:0,      rate:0.20, cap:150000},
    {th:150000, rate:0.25, cap:220000},
    {th:220000, rate:0.35, cap:380000},
    {th:380000, rate:0.40, cap:500000},
    {th:500000, rate:0.44, cap:900000},
    {th:900000, rate:0.48}
  ],
  1997: [
    {th:0,      rate:0.20, cap:250000},
    {th:250000, rate:0.22, cap:300000},
    {th:300000, rate:0.31, cap:500000},
    {th:500000, rate:0.35, cap:700000},
    {th:700000, rate:0.39, cap:1100000},
    {th:1100000,rate:0.42}
  ],
  1998: 'sameAs:1997',
  1999: [
    {th:0,      rate:0.20, cap:400000},
    {th:400000, rate:0.30, cap:1000000},
    {th:1000000,rate:0.40}
  ],
  2000: 'sameAs:1999',
  2001: [
    {th:0,      rate:0.20, cap:480000},
    {th:480000, rate:0.30, cap:1050000},
    {th:1050000,rate:0.40}
  ],
  2002: [
    {th:0,      rate:0.20, cap:600000},
    {th:600000, rate:0.30, cap:1200000},
    {th:1200000,rate:0.40}
  ],
  2003: [
    {th:0,      rate:0.20, cap:650000},
    {th:650000, rate:0.30, cap:1350000},
    {th:1350000,rate:0.40}
  ],
  2004: [
    {th:0,      rate:0.18, cap:800000},
    {th:800000, rate:0.26, cap:1500000},
    {th:1500000,rate:0.38}
  ],
  2005: [
    {th:0,      rate:0.18, cap:1500000},
    {th:1500000,rate:0.38}
  ],
  2006: [
    {th:0,      rate:0.18, cap:1550000},
    {th:1550000,rate:0.36}
  ],
  2007: [
    {th:0,      rate:0.18, cap:1700000},
    {th:1700000,rate:0.36}
  ],
  2008: 'sameAs:2007',
  2009: [
    {th:0,      rate:0.18, cap:1900000},
    {th:1900000,rate:0.36}
  ],
  2010: [ // adóalap-kiegészítés: *1.27 a teljes alapra (ezt külön alkalmazzuk)
    {th:0,      rate:0.17, cap:5000000},
    {th:5000000,rate:0.32}
  ]
};

// "sameAs" feloldása
for (const y of Object.keys(TAX_TABLES)) {
  const v = TAX_TABLES[y];
  if (typeof v === 'string' && v.startsWith('sameAs:')) {
    const target = v.split(':')[1]|0;
    TAX_TABLES[y] = JSON.parse(JSON.stringify(TAX_TABLES[target]));
  }
}

/* ===== Adójóváírások 1988–2011 ===== */
function taxCreditAmount(year, rawIncome, contributionAmount){
  // 1988–1990: fix 12 000 Ft
  if (year>=1988 && year<=1990) return 12000;
  if (year===1991) return 3000;            // 1991: fix 3 000 Ft
  if (year===1993) return 2400;            // 1993: fix 2 400 Ft
  if (year===1994) return rawIncome * 0.10;
  if (year===1995) return contributionAmount * 0.25;
  if (year===1997) return Math.min(rawIncome * 0.20, 43200);
  if (year===1998) return Math.min(rawIncome * 0.20, 50400) + (contributionAmount * 0.25);
  if (year>=1999 && year<=2001) return Math.min(rawIncome * 0.10, 36000) + (contributionAmount * 0.25);
  if (year===2002) return Math.min(rawIncome * 0.18, 60000) + (contributionAmount * 0.25);
  if (year===2003) return Math.min(rawIncome * 0.18, 108000) + (contributionAmount * 0.25);
  if (year>=2004 && year<=2007) return Math.min(rawIncome * 0.18, 108000);
  if (year===2008 || year===2009) return Math.min(rawIncome * 0.18, 136080);
  if (year===2010) return Math.min(rawIncome * 0.17, 181200);
  if (year===2011) return Math.min(rawIncome * 0.16, 145200);
  // 1992, 1996, 2012+ : nincs
  return 0;
}

/* ===== Sávos adó összegző (bracket-by-bracket) ===== */
function progressiveTaxFromTable(year, taxBase){
  const table = TAX_TABLES[year];
  if (!table || !Number.isFinite(taxBase) || taxBase <= 0) return 0;
  let tax = 0;
  const brackets = [...table].sort((a,b)=>a.th - b.th);
  for (let i=0; i<brackets.length; i++){
    const {th=0, rate=0, cap=Infinity} = brackets[i];
    if (taxBase <= th) break;
    const upper = Math.min(taxBase, cap);
    const width = Math.max(0, upper - th);
    tax += width * rate;
    if (taxBase <= cap) break;
  }
  return Math.max(0, tax);
}

/* ===== Adó számítása egy évre – végleges logika =====
   1) bruttóból járuléklevonás -> baseAfterContribution (a hívó ezt adja át)
   2) 1988–1996: jóváírás az ADÓALAPBÓL
      1997–2011: jóváírás a KISZÁMOLT ADÓBÓL (min 0)
      2012+: nincs jóváírás
   3) év szerinti adóképlet (sávos/egykulcsos), ahol kell adóalap-kiegészítéssel
*/
function taxForYear(year, baseAfterContribution, rawIncomeForCredits, contributionAmount){
  if (!Number.isFinite(baseAfterContribution) || baseAfterContribution<=0) return 0;

  const credit = Math.max(0, taxCreditAmount(year, rawIncomeForCredits, contributionAmount));

  // 2016+ (15%) és 2013–2015 (16%) – nincs jóváírás
  if (year >= 2016) return baseAfterContribution * 0.15;
  if (year >= 2013 && year <= 2015) return baseAfterContribution * 0.16;

  // 2012 – nincs jóváírás, de részleges adóalap-kiegészítés
  if (year === 2012){
    const thr = 2424000;
    const extra = Math.max(0, baseAfterContribution - thr) * 0.27;
    const taxBase = baseAfterContribution + extra;
    return taxBase * 0.16;
  }

  // 1997–2011: jóváírás a FIZETENDŐ ADÓBÓL
  if (year >= 1997 && year <= 2011){
    let grossTax = 0;

    if (year === 2011){
      const taxBase = baseAfterContribution * 1.27; // teljes adóalap-kiegészítés
      grossTax = taxBase * 0.16;
    } else if (year === 2010){
      const taxBase = baseAfterContribution * 1.27;
      grossTax = progressiveTaxFromTable(2010, taxBase);
    } else {
      // 1997–2009: sávos
      const yr = Math.min(Math.max(1997, year), 2009);
      grossTax = progressiveTaxFromTable(yr, baseAfterContribution);
    }

    return Math.max(0, grossTax - credit);
  }

  // 1988–1996: jóváírás az ADÓALAPBÓL, majd sávos adó
  const baseAfterCredit = Math.max(0, baseAfterContribution - credit);
  const yr = Math.min(Math.max(1988, year), 1996);
  return progressiveTaxFromTable(yr, baseAfterCredit);
}

/* ===== Nyugdíj segédfüggvények ===== */
function serviceMultiplier(years){
  const y=Math.max(10, Math.min(50, years|0));
  const base=SERVICE_TABLE[40]??80;
  let pct=SERVICE_TABLE[y];
  if(typeof pct!=='number') pct=Math.min(100, base + Math.max(0,y-40)*2);
  if(y>40) pct=Math.min(100, base + (y-40)*2);
  return pct/100;
}
function formatPct(mult){const p=mult*100;return (p%1===0)?`${p.toFixed(0)}%`:`${p.toFixed(1)}%`;}
function progressiveDegression(monthly){
  const a=372000,b=421000;let res=0,parts=[];
  const p1=Math.min(monthly,a); if(p1>0){res+=p1;parts.push(`0–${a}: ${formatFt(p1)} ×100%=${formatFt(p1)}`);}
  if(monthly>a){const p2=Math.min(monthly,b)-a;if(p2>0){res+=p2*0.9;parts.push(`${a+1}–${b}: ${formatFt(p2)} ×90%=${formatFt(p2*0.9)}`);}}
  if(monthly>b){const p3=monthly-b;res+=p3*0.8;parts.push(`${b+1}+: ${formatFt(p3)} ×80%=${formatFt(p3*0.8)}`);}
  return {value:res, parts};
}
function formatFt(x){return new Intl.NumberFormat('hu-HU').format(Math.round(x))+' Ft';}

/* ===== DOM ===== */
const rowsEl=document.getElementById('rows');
const serviceRange=document.getElementById('serviceYears');
const serviceLabel=document.getElementById('serviceYearsLabel');
const resultEl=document.getElementById('result');
const infoEl=document.getElementById('serviceInfo');
const breakdownEl=document.getElementById('breakdown');

/* ===== Sorok ===== */
const inputs=[];
YEARS.forEach((y,i)=>{
  const tr=document.createElement('tr');

  const tdY=document.createElement('td'); tdY.textContent=y;
  const tdM=document.createElement('td'); tdM.textContent=YEAR_MULTS[i].toFixed(3);

  const tdIn=document.createElement('td');
  const inp=document.createElement('input');
  inp.type='number'; inp.min='0'; inp.step='1000'; inp.placeholder='0'; inp.inputMode='numeric';
  tdIn.appendChild(inp);

  const tdVal=document.createElement('td'); tdVal.className='muted'; tdVal.textContent='—';

  const tdGuide=document.createElement('td'); tdGuide.className='muted mono'; tdGuide.textContent=formatFt(ANNUAL_NET[i]||0);

  const tdTaxCap=document.createElement('td');
  const limit=CONST_TAX_LIMIT[i];
  tdTaxCap.className='muted mono'; tdTaxCap.textContent=limit?formatFt(limit):'—';

  const tdAfterContr=document.createElement('td'); tdAfterContr.className='muted mono'; tdAfterContr.textContent='—';
  const tdTax=document.createElement('td'); tdTax.className='muted mono'; tdTax.textContent='—';
  const tdFinal=document.createElement('td'); tdFinal.className='muted mono'; tdFinal.textContent='—';

  tr.append(tdY,tdM,tdIn,tdVal,tdGuide,tdTaxCap,tdAfterContr,tdTax,tdFinal);
  rowsEl.appendChild(tr);
  inputs.push({inp, tdVal, tdAfterContr, tdTax, tdFinal});
});

/* ===== Számítás ===== */
function recalc(){
  let sumValorizalt=0;
  let filledCount=0;

  inputs.forEach(({inp, tdVal, tdAfterContr, tdTax, tdFinal}, i)=>{
    const hasValue=String(inp.value??'').trim()!=='';  // felhasználói beírás
    const raw=parseFloat(inp.value||'0');

    if(hasValue) filledCount+=1;
    if(raw>0){
      const year=YEARS[i];

      // (1) Járulék
      const contr=contributionRatePct(year)/100;
      const contrAmt=raw*contr;
      const afterContr=Math.max(0, raw-contrAmt);
      tdAfterContr.textContent=formatFt(afterContr); tdAfterContr.className='';

      // (2+3) Adó – a taxForYear már kezeli a megfelelő jóváírás-szabályt
      const tax=taxForYear(year, afterContr, raw, contrAmt);

      const afterTax=Math.max(0, afterContr - tax);
      tdTax.textContent=formatFt(tax); tdTax.className=tax? '':'muted mono';

      tdFinal.textContent=formatFt(afterTax); tdFinal.className='';

      // Valorizáció a végeredményből
      const valor=afterTax*YEAR_MULTS[i];
      tdVal.textContent=formatFt(valor); tdVal.className='';
      if(isFinite(valor)) sumValorizalt+=valor;

    }else{
      tdAfterContr.textContent='—'; tdAfterContr.className='muted mono';
      tdTax.textContent='—'; tdTax.className='muted mono';
      tdFinal.textContent='—'; tdFinal.className='muted mono';
      tdVal.textContent='—'; tdVal.className='muted mono';
    }
  });

  // Nyugdíj fő kalkuláció
  const years=parseInt(serviceRange.value||'0',10)||0;
  const sMult=serviceMultiplier(years);
  const sMultPct=formatPct(sMult);

  const divisor=Math.max(1, filledCount);
  const avgPerEntered=sumValorizalt/divisor;
  const grossMonthlyBeforeDeg=avgPerEntered/12;

  const prog=progressiveDegression(grossMonthlyBeforeDeg);
  const monthlyAfterDegression=prog.value;
  const finalMonthly=monthlyAfterDegression*sMult;

  resultEl.innerHTML=`${formatFt(finalMonthly)} <small>estimated monthly pension</small>`;
  serviceLabel.textContent=`${years} years`;

  infoEl.textContent=
    `Service multiplier: ${sMultPct} | Total annual valuated earnings: ${formatFt(sumValorizalt)} | / ${divisor} years entered = ${formatFt(avgPerEntered)}`;

  breakdownEl.innerHTML=
    `Total valuated earnings (after contributions and tax): <strong>${formatFt(sumValorizalt)}</strong><br/>
     Average monthly lifetime earnings: <strong>${formatFt(grossMonthlyBeforeDeg)}</strong><br/>
     Degression:<br/>- ${prog.parts.join('<br/>- ')}<br/>
     Monthly amount after degression: <strong>${formatFt(monthlyAfterDegression)}</strong><br/>
     Service multiplier: ×<strong>${sMultPct}</strong> → <strong>${formatFt(finalMonthly)}</strong>`;
}

/* === QUICK FILL logic (new) === */
function quickFillByPct(p){
  inputs.forEach(({inp}, i)=>{
    const base = ANNUAL_NET[i] || 0; // irányadó: Éves bruttó átlagkereset oszlop
    inp.value = base ? Math.round(base * p) : '';
  });
  recalc();
}

/* Események + init */
inputs.forEach(({inp})=>inp.addEventListener('input', recalc));
document.getElementById('serviceYears').addEventListener('input', recalc);

/* Quick fill buttons */
document.getElementById('fill40').addEventListener('click', ()=>quickFillByPct(0.40));
document.getElementById('fill60').addEventListener('click', ()=>quickFillByPct(0.60));
document.getElementById('fill80').addEventListener('click', ()=>quickFillByPct(0.80));
document.getElementById('fill100').addEventListener('click', ()=>quickFillByPct(1.00));
document.getElementById('fill150').addEventListener('click', ()=>quickFillByPct(1.50));
document.getElementById('fill275').addEventListener('click', ()=>quickFillByPct(2.75));

recalc();

/* Reset */
document.getElementById('reset').addEventListener('click',(ev)=>{
  ev.preventDefault();
  inputs.forEach(({inp})=>{inp.value='';});
  recalc();
});
</script>
