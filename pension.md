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
.wrap {
  max-width: 1100px;
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
.grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
}
@media (min-width: 900px) {
  .grid { grid-template-columns: 3fr 2fr; }
}
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
.right .card { position: sticky; top: 16px; }
.btn {
  cursor: pointer;
  padding: 10px 14px;
  border-radius: 12px;
  border: 1px solid rgba(0,0,0,0.1);
  background: #fff;
  color: var(--text);
  font-weight: 600;
  transition: background 0.2s;
}
.btn:hover { background: #f0f0f0; }
.btn:active { transform: translateY(1px); }
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
#serviceYears {
  width: 280px !important;
  flex: 0 0 280px !important;
  margin: 0 6px;
  appearance: none;
  background: transparent;
  height: 18px;
  padding: 0;
}
#serviceYears::-webkit-slider-runnable-track {
  height: 4px;
  background: #ccc;
  border-radius: 999px;
}
#serviceYears::-moz-range-track {
  height: 4px;
  background: #ccc;
  border-radius: 999px;
}
#serviceYears::-webkit-slider-thumb {
  appearance: none;
  width: 16px;
  height: 16px;
  margin-top: -6px;
  border-radius: 50%;
  background: var(--accent);
  cursor: pointer;
}
#serviceYears::-moz-range-thumb {
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: var(--accent);
  cursor: pointer;
}
@media (max-width: 480px) {
  #serviceYears { width: 240px !important; flex-basis: 240px !important; }
}
</style>
</head>
<body>
  <div class="wrap">
    <h1>Nyugdíjkalkulátor</h1>
    <p class="lead">Add meg az éves nettó járulékköteles kereseteidet minden általad ledolgozott évben. Fontos, hogy a munkabérként és prémiumként kapott bejelentett és leadózott jövedelemmel számolj csak.</p>
	<p>A kalkulátor figyelembe veszi az adott év <em>valorizációs szorzóját</em>, a <em>szolgálati idő szorzóját</em>, a lépcsőzetes <em>degressziót</em>, és ezek alapján kiszámolja, mi a várható havi nyugdíjad. Egyéb tényezőkkel és kedvezményekkel nem számol a modell, csak a kereseti adatokkal.</p>
	<p>Az adatok tájékoztató jellegűek, pontosabb számításra a <a href="https://www.allamkincstar.gov.hu/nyugdij/sajat-jogu-ellatasok/az-oregsegi-nyugdij-osszegenek-szamitasa">Magyar Államkincstár nyugdíjkalkulátora</a> javasolt.</p>

    <div class="grid">
      <div class="left card">
        <div class="row" style="margin-bottom:12px">
          <label for="serviceYears"><strong>Szolgálati évek száma</strong></label>
          <input id="serviceYears" class="slider" type="range" min="10" max="50" step="1" value="15" />
          <strong id="serviceYearsLabel">15 év</strong>
        </div>

        <div style="max-height: 55vh; overflow:auto; border-radius: 12px; border: 1px solid rgba(255,255,255,.06)">
          <table>
            <thead>
              <tr>
                <th>Év</th>
                <th>Szorzó</th>
                <th>Éves kereset (Ft)</th>
                <th>Valorizált éves kereset</th>
                <th>Éves átlag nettó kereset (irányadó)</th>
              </tr>
            </thead>
            <tbody id="rows"></tbody>
          </table>
        </div>
        <div class="row" style="margin-top:12px">
          <button id="reset" class="btn" type="button">Összes mező törlése</button>
          <span id="serviceInfo" class="muted"></span>
        </div>
      </div>

      <div class="right">
        <div class="card">
          <div class="row" style="justify-content:space-between">
            <div>
              <div class="muted">[(összes × szorzó) ÷ szolgálati évek] × szolgálati szorzó ÷ 12, majd degresszió</div>
              <div class="result" id="result">— <small>havi várható nyugdíj</small></div>
            </div>
            <div>
          <hr style="border:none;border-top:1px solid rgba(255,255,255,.08); margin:14px 0" />
          <div class="muted" id="breakdown"></div>
        </div>
      </div>
    </div>
  </div>

<script>
const YEARS = [1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023,2024];
const YEAR_MULTS = [63.115,53.99,44.4,35.378,29.163,24.782,19.468,17.287,14.724,11.867,10.023,8.893,7.982,6.871,5.742,5.028,4.753,4.318,4.013,3.894,3.643,3.576,3.347,3.146,3.083,2.938,2.852,2.735,2.539,2.248,2.019,1.813,1.652,1.52,1.294,1.133,1.0];
const ANNUAL_NET = [85017, 97676, 119400, 150646, 184594, 215210, 271801, 308088, 365329, 457200, 542400, 601200, 669600, 778800, 931200, 1065600, 1125600, 1237200, 1330800, 1369200, 1464000, 1489200, 1591200, 1693200, 1728000, 1812000, 1868400, 1947600, 2100000, 2370000, 2632800, 2935200, 3220800, 3501600, 4115808, 4701132, 5325576];




function serviceMultiplier(years) {
  const y = Math.max(10, Math.min(50, years));
  return Math.min(1.00, 0.33 + (y - 10) * 0.02);
}

function progressiveDegression(monthly) {
  const a = 372000, b = 421000;
  let res = 0, parts = [];
  const p1 = Math.min(monthly, a);
  if (p1 > 0) { res += p1; parts.push(`0–${a}: ${formatFt(p1)} ×100%=${formatFt(p1)}`); }
  if (monthly > a) {
    const p2 = Math.min(monthly, b) - a;
    if (p2 > 0) { res += p2 * 0.9; parts.push(`${a+1}–${b}: ${formatFt(p2)} ×90%=${formatFt(p2*0.9)}`); }
  }
  if (monthly > b) {
    const p3 = monthly - b;
    res += p3 * 0.8;
    parts.push(`${b+1}+: ${formatFt(p3)} ×80%=${formatFt(p3*0.8)}`);
  }
  return { value: res, parts };
}

function formatFt(x){return new Intl.NumberFormat('hu-HU').format(Math.round(x))+' Ft';}
const rowsEl=document.getElementById('rows');
const serviceRange=document.getElementById('serviceYears');
const serviceLabel=document.getElementById('serviceYearsLabel');
const resultEl=document.getElementById('result');
const infoEl=document.getElementById('serviceInfo');
const breakdownEl=document.getElementById('breakdown');

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
  const tdGuide=document.createElement('td'); tdGuide.className='muted mono';
  tdGuide.textContent=formatFt(ANNUAL_NET[i]||0);
  tr.append(tdY,tdM,tdIn,tdVal,tdGuide);
  rowsEl.appendChild(tr);
  inputs.push({inp,tdVal});
});

function recalc(){
  let sumValorizalt=0;
  inputs.forEach(({inp,tdVal},i)=>{
    const raw=parseFloat(inp.value||'0');
    const valor=raw*YEAR_MULTS[i];
    tdVal.textContent=raw?formatFt(valor):'—';
    tdVal.className=raw?'':'muted';
    sumValorizalt+=valor;
  });
  const years=parseInt(serviceRange.value,10);
  const sMult=serviceMultiplier(years);
  const avgPerServiceYear=sumValorizalt/Math.max(1,years);
  const afterServiceMult=avgPerServiceYear*sMult;
  const grossMonthly=afterServiceMult/12;
  const prog=progressiveDegression(grossMonthly);
  resultEl.innerHTML=`${formatFt(prog.value)} <small>havi várható nyugdíj</small>`;
  serviceLabel.textContent=`${years} év`;
  infoEl.textContent=`Szolgálati szorzó: ${Math.round(sMult*100)}% | Éves valorizált összes: ${formatFt(sumValorizalt)} | / ${years} év = ${formatFt(avgPerServiceYear)}`;
  breakdownEl.innerHTML=`Összes valorizált kereset: <strong>${formatFt(sumValorizalt)}</strong><br/>Osztás szolgálati évekkel: ÷<strong>${years}</strong> = <strong>${formatFt(avgPerServiceYear)}</strong><br/>Szolgálati szorzó: ×<strong>${Math.round(sMult*100)}%</strong> → <strong>${formatFt(afterServiceMult)}</strong><br/>Havi osztás: ÷12 = <strong>${formatFt(grossMonthly)}</strong><br/>Degresszió:<br/>- ${prog.parts.join('<br/>- ')}<br/><strong>Eredmény: ${formatFt(prog.value)}</strong>`;
}
inputs.forEach(({inp})=>inp.addEventListener('input',recalc));
serviceRange.addEventListener('input',recalc);
document.getElementById('reset').addEventListener('click',()=>{inputs.forEach(({inp})=>inp.value='');recalc();});
recalc();
</script>
</body>
</html>
