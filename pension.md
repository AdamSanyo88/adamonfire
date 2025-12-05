---
layout: page
title: Nyugdíjkalkulátor
permalink: /pension
---

<h1 class="page-title">{{ page.title | escape }}</h1>
 
<html lang="hu">
<head>
  <meta charset="utf-8" />
<style>
:root {
  --bg: #ffffff;
  --card: #ffffff;
  --muted: #555555;
  --accent: #007acc;
  --text: #111111;
}
* {
  box-sizing: border-box;
  font-family: Inter, system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
}
body {
  margin: 0;
  background: var(--bg);
  color: var(--text);
}

/* --- Teljes szélességű tartalom --- */
.wrap {
  width: 100%;
  max-width: 100%;
  margin: 32px auto;
  padding: 0 16px;
}

h1 {
  font-size: 28px;
  margin: 0 0 6px;
}
p.lead {
  margin: 0 0 24px;
  color: var(--muted);
}

.card {
  background: var(--card);
  border-radius: 16px;
  padding: 20px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
}

/* --- Egyoszlopos elrendezés minden kijelzőn --- */
.grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
}
@media (min-width: 900px) {
  .grid { grid-template-columns: 1fr; }
}
.right .card { position: static; top: auto; }

/* --- Táblázat teljes szélességben --- */
table {
  width: 100%;
  border-collapse: collapse;
}
th, td {
  padding: 8px 10px;
  border-bottom: 1px solid rgba(0,0,0,0.1);
  text-align: left;
}
thead th {
  position: sticky;
  top: 0;
  background: #f5f5f5;
  z-index: 2;
}
tbody tr:hover { background: rgba(0,0,0,0.03); }

/* --- Űrlapmezők --- */
input[type="number"] {
  width: 120px;
  padding: 6px 8px;
  border-radius: 8px;
  border: 1px solid rgba(0,0,0,0.2);
  background: #fff;
  color: var(--text);
}
.muted { color: var(--muted); font-size: 13px; }
.pill {
  display: inline-block;
  padding: 6px 10px;
  border-radius: 999px;
  background: rgba(0,122,204,0.1);
  border: 1px solid rgba(0,122,204,0.3);
  color: var(--accent);
  font-weight: 600;
}
.result {
  font-size: 28px;
  font-weight: 800;
  letter-spacing: .3px;
}
.result small {
  font-size: 14px;
  font-weight: 600;
  color: var(--muted);
}
.footer { margin-top: 10px; font-size: 12px; color: var(--muted); }
.btn {
  display: inline-block;
  cursor: pointer;
  padding: 10px 18px;
  min-width: 130px;
  border-radius: 12px;
  border: 1px solid rgba(0,0,0,0.1);
  background: #fff;
  color: var(--text);
  font-weight: 600;
  font-size: 14px;
  white-space: nowrap;
  text-align: center;
  transition: background 0.2s, transform 0.1s;
}
.btn:hover { background: #f0f0f0; }

.mono { font-variant-numeric: tabular-nums; }

/* --- SLIDER compact layout --- */
.left .row {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-wrap: nowrap;
  margin: 6px 0;
  padding: 0;
  min-height: 0;
}
label[for="serviceYears"] { white-space: nowrap; }
#serviceYearsLabel {
  min-width: 56px;
  text-align: right;
  font-weight: 600;
  white-space: nowrap;
}
/* Slider alap */
#serviceYears {
  width: 280px !important;
  flex: 0 0 280px !important;
  margin: 0 6px;
  appearance: none;
  background: transparent;
  height: 18px;
  padding: 0;
  position: relative;
}
/* Track */
#serviceYears::-webkit-slider-runnable-track { height: 4px; background: #ccc; border-radius: 999px; }
#serviceYears::-moz-range-track { height: 4px; background: #ccc; border-radius: 999px; }
/* Thumb */
#serviceYears::-webkit-slider-thumb,
#serviceYears::-moz-range-thumb {
  appearance: none;
  width: 16px; height: 16px; margin-top: -6px;
  border-radius: 50%; background: var(--accent); cursor: pointer; position: relative; z-index: 2;
}
/* Tooltipok tiltása */
#serviceYears::-webkit-slider-thumb::before,
#serviceYears::-webkit-slider-thumb::after,
#serviceYears::-moz-range-thumb::before,
#serviceYears::-moz-range-thumb::after,
#serviceYears::before,
#serviceYears::after { display: none !important; content: none !important; }
#serviceYears::-ms-tooltip { display: none !important; }

/* Mobilon se nőjön meg */
@media (max-width: 480px) {
  #serviceYears { width: 240px !important; flex-basis: 240px !important; }
}

/* Külső lib-féle buborékok tiltása */
#serviceYears + .thumb,
#serviceYears ~ .thumb,
#serviceYears + .thumb .value,
#serviceYears ~ .thumb .value,
.left .row .thumb,
.left .row .thumb .value { display: none !important; visibility: hidden !important; opacity: 0 !important; pointer-events: none !important; }
.range-label, .value, .value-indicator, .mdc-slider__value-indicator, .noUi-tooltip { display: none !important; }

/* Kártyák teljes szélesség használata */
.left .card, .right .card { width: 100%; height: 100%; }

/* Táblázat biztosan kitöltse a sort */
table { width: 100%; }

/* Számmezők se lógjanak túl kis kijelzőn */
input[type="number"] {
  max-width: 160px;
  width: 100%;
}
</style>
</head>
<body>
  <div class="wrap">
    <br/>
		<h5>How to use the calculator?</h5>
<p>First, enter the <strong>number of qualifying years</strong> (which determines the multiplier applied to your earnings). Then provide your <strong>annual gross earnings for each year you worked</strong>. Only include officially declared and taxed income received as wages or bonuses.</p>
<p>The calculator takes into account the given year’s <em>valorization multiplier</em>, the <em>qualifying years multiplier</em>, and the tiered <em>degression</em>, and based on these values, it calculates your expected monthly pension.</p>
<p><strong>Important:</strong> The calculator does not account for the annual contribution cap prior to 2013, but the table shows the maximum annual gross income on which pension contributions had to be paid. Do not enter a higher annual gross amount than this for those years.</p>
<p>Always enter full-year earnings. The calculator continuously recalculates the average lifetime earnings, so if you have worked only a few years, it assumes that you will continue to earn at a similar level in the future. Therefore, it is important to enter all yearly data for a more accurate calculation.</p>
<br/>
<h5>The actual steps of pension calculations</h5>
<ol>
<li>The total years of service earned during your lifetime, rounded down to whole years (partial years are lost).</li>
<li>The net annual value of earnings received since January 1, 1988 (first pension contributions are deducted, then the income tax).</li>
<li>The net values must be multiplied each year by the annual valorization multiplier. This shows how much previous earnings are worth in today's terms. In practice, the key factor is whether the given year's income was below or above that year's net average wage (this determines its relative value).</li>
<li>The resulting amounts must be divided by the total qualying years (calculated in days). This amount must then be multiplied by 365 and divided by 12 to obtain the monthly net average lifetime earnings.</li>
<li>If the resulting amount exceeds 372,000 HUF per month, it must be degressed (first at 90%, then above 421,000 HUF at 80%).</li>
<li>Finally, the resulting amount must be multiplied by the multiplier corresponding to the qualifying years (after 20 years this is 53%, at 30 years 68%, at 40 years 80%, etc.). The maximum is 50 years, the minimum is 15 years (but the calculator can also calculate for the 10–15 year range).</li>
</ol>
<p>The information is for guidance only. For more accurate calculations, use the <a href="https://www.allamkincstar.gov.hu/nyugdij/sajat-jogu-ellatasok/oregsegi-nyugdij/onkiszolgalo-nyugdijkalkulator">Hungarian State Treasury's pension calculator</a>.</p>


  <div class="grid">
    <div class="left card">
      <div class="row" style="margin-bottom:12px">
        <label for="serviceYears"><h5>1. Qualifying years</h5></label>
        <input id="serviceYears" type="range" min="10" max="50" step="1" value="40" />
        <strong id="serviceYearsLabel">40 years</strong>
      </div>

<h5>2. Annual gross income</h5>

 <!-- === QUICK FILL (new) === -->
      <div class="row" style="margin-bottom:12px; align-items:flex-start;">
        <div class="btn-group" aria-label="Quick fill with wage levels relative to the gross average wage">
          <span class="inline-label">Quick fill with wage levels relative to the gross average wage:</span>
          <button class="btn sm" type="button" id="fill40">40% (~minimum wage)</button>
          <button class="btn sm" type="button" id="fill60">60% (bottom ~30%)</button>
          <button class="btn sm" type="button" id="fill80">80% (median wage)</button> 
          <button class="btn sm" type="button" id="fill100">100% (average wage)</button>
          <button class="btn sm" type="button" id="fill150">150% (top 15%)</button>
          <button class="btn sm" type="button" id="fill275">275% (top 5%)</button>
        </div>
      </div>


      <div style="max-height:55vh; overflow:auto; border-radius:12px; border:1px solid rgba(0,0,0,.06)">
        <table>
          <thead>
            <tr>
              <th>Year</th>
              <th>Multiplier</th>
              <th>Annual gross income (Ft)</th>
              <th>Indexed gross income</th>
              <th>Yearly gross income (indicative)</th>
              <th>Pension cap on gross yearly income</th>
              <th>Insurance deductions</th>
              <th>Taxes</th>
              <th>Yearly net income</th>
            </tr>
          </thead>
          <tbody id="rows"></tbody>
        </table>
      </div>

      <div class="row" style="margin-top:12px">
        <button id="reset" class="btn" type="button">Clear all fields</button>
        <span id="serviceInfo" class="muted"></span>
      </div>
    </div>

    <div class="right">
      <div class="card">
        <div class="result" id="result">— <small>expected monthly pension</small></div>
        <hr style="border:none;border-top:1px solid rgba(0,0,0,.08); margin:14px 0" />
        <div class="muted" id="breakdown"></div>
      </div>
    </div>
  </div>
</div>

<script>
/* ===== Adatok ===== */
const YEARS=[1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023,2024];
const YEAR_MULTS=[63.115,53.99,44.4,35.378,29.163,24.782,19.468,17.287,14.724,11.867,10.023,8.893,7.982,6.871,5.742,5.028,4.753,4.318,4.013,3.894,3.643,3.576,3.347,3.146,3.083,2.938,2.852,2.735,2.539,2.248,2.019,1.813,1.652,1.52,1.294,1.133,1.0];
const ANNUAL_NET=[107616, 126852, 161352, 215208, 267528, 326076, 399708, 466800, 562044, 687600, 813600, 926400, 1051200, 1243200, 1470000, 1646400, 1748400, 1899600, 2054400, 2220000, 2386800, 2397600, 2431200, 2557200, 2676000, 2772000, 2852400, 2972400, 3158400, 3564000, 3958800, 4413600, 4843200, 5265600, 6189192, 7069368, 8008380];
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

  resultEl.innerHTML=`${formatFt(finalMonthly)} <small>havi várható nyugdíj</small>`;
  serviceLabel.textContent=`${years} év`;

  infoEl.textContent=
    `Qualifying years multiplier: ${sMultPct} | Annual indexed total: ${formatFt(sumValorizalt)} | / ${divisor} years entered = ${formatFt(avgPerEntered)}`;

  breakdownEl.innerHTML=
    `Total indexed income (after tax and insurance): <strong>${formatFt(sumValorizalt)}</strong><br/>
     Monthly average lifetime income: <strong>${formatFt(grossMonthlyBeforeDeg)}</strong><br/>
     Degression:<br/>- ${prog.parts.join('<br/>- ')}<br/>
     After degression monthly: <strong>${formatFt(monthlyAfterDegression)}</strong><br/>
     Qualifying years multiplier: ×<strong>${sMultPct}</strong> → <strong>${formatFt(finalMonthly)}</strong>`;
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