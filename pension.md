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
  white-space: nowrap;     /* nem törik a sor */
  text-align: center;
  transition: background 0.2s, transform 0.1s;
}
.btn:hover {
  background: #f0f0f0;
}


.mono { font-variant-numeric: tabular-nums; }

/* --- SLIDER compact layout --- */

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

label[for="serviceYears"] {
  white-space: nowrap;
}

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

/* Thumb */
#serviceYears::-webkit-slider-thumb {
  appearance: none;
  width: 16px;
  height: 16px;
  margin-top: -6px;
  border-radius: 50%;
  background: var(--accent);
  cursor: pointer;
  position: relative;
  z-index: 2;
}
#serviceYears::-moz-range-thumb {
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: var(--accent);
  cursor: pointer;
  position: relative;
  z-index: 2;
}

/* --- Tooltip / value indicator eltüntetése --- */
#serviceYears::-webkit-slider-thumb::before,
#serviceYears::-webkit-slider-thumb::after,
#serviceYears::-moz-range-thumb::before,
#serviceYears::-moz-range-thumb::after,
#serviceYears::before,
#serviceYears::after {
  display: none !important;
  content: none !important;
}

/* Egyes böngészők (pl. Edge, Chromium) belső tooltipjét kikapcsolja */
#serviceYears::-ms-tooltip {
  display: none !important;
}

/* Mobilon se nőjön meg */
@media (max-width: 480px) {
  #serviceYears {
    width: 240px !important;
    flex-basis: 240px !important;
  }
}

/* Könyvtár által beszúrt érték-buborék elrejtése */
#serviceYears + .thumb,
#serviceYears ~ .thumb,
#serviceYears + .thumb .value,
#serviceYears ~ .thumb .value,
.left .row .thumb,
.left .row .thumb .value {
  display: none !important;
  visibility: hidden !important;
  opacity: 0 !important;
  pointer-events: none !important;
}

/* Ha a lib más neveket is használ, ezeket is tiltsd le (opcionális) */
.range-label,
.value,
.value-indicator,
.mdc-slider__value-indicator,
.noUi-tooltip {
  display: none !important;
}

/* A bekezdésekkel egyező “tartalomszélesség” – kicsin nem túl széles, nagyban tágas */
.wrap {
  /* 720px és 1280px között skálázódik, 92vw-t nem lépi túl */
  max-width: clamp(720px, 92vw, 1280px);
}

/* Alap rács: kicsin 1 oszlop marad */
.grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 20px; /* kicsit nagyobb lélegzet */
  align-items: start;
}

/* Nagyobb képernyőn két egyforma oszlop (a régi 3fr/2fr helyett) */
@media (min-width: 900px) {
  .grid {
    grid-template-columns: 2fr 1fr;
  }
  /* A jobb oldali kártya ragadjon, de csak nagyban */
  .right .card {
    position: sticky;
    top: 16px;
  }
}

/* A kártyák húzódjanak ki a rendelkezésre álló oszlopszélességre */
.left .card,
.right .card {
  width: 100%;
  height: 100%;
}

/* A táblázat továbbra is kitölti a kártyát */
table {
  width: 100%;
}

/* A számmezők ne lógjanak túl kis kijelzőn sem */
input[type="number"] {
  max-width: 160px;  /* kicsit tágasabb, de nem végtelen */
  width: 100%;
}
</style>
</head>
<body>
  <div class="wrap">
    <br/>
    <p>Add meg az éves nettó járulékköteles kereseteidet minden általad ledolgozott évben. Fontos, hogy a munkabérként és prémiumként kapott bejelentett és leadózott jövedelemmel számolj csak.</p>
	<p>A kalkulátor figyelembe veszi az adott év <em>valorizációs szorzóját</em>, a <em>szolgálati idő szorzóját</em>, a lépcsőzetes <em>degressziót</em>, és ezek alapján kiszámolja, mi a várható havi nyugdíjad. Egyéb tényezőkkel és kedvezményekkel nem számol a modell, csak a kereseti adatokkal.</p>
	<p>A kalkulátor nem számol továbbá a 2013 előtti éves járulékplafonnal, de a táblázat megmutatja, hogy milyen éves bruttó bér volt a maximum, amelyre nyugdíjjárulékot kellett fizetni.</p>
	<p>Mindig egész éves keresetet adj meg, mert törtévvel nem tud számolni a kalkulátor.</p>
	<h5>Nyugdíjszámítás menete a gyakorlatban</h5>
	<ol>
	<li>Az egész élet során szerzett szolgálati évek egész számban, lefelé kerekítve (a törtév elvész).</li>
	<li>Az 1988 január 1-je óta szerzett jövedelmek nettósított éves értéke (adók és járulékok levonása után).</li>
	<li>A nettó értékeket minden évben fel kell szorozni az éves valorizációs szorzóval. Ez megmutatja, hogy mai áron számolva az akkori kereset mennyit ért. Gyakorlatban a fő szempont az, hogy az adott éves jövedelem az adott év nettó átlagkeresete alatt vagy felett volt-e (ez határozza meg a relatív értékét).</li>
	<li>A kapott összegeket el kell osztani a teljes szolgálati idővel (napokban számolva). Ezt az összeget utána fel kell szorozni 365-tel, majd osztani 12-vel, így megkapva a nettó életpálya átlagkeresetet. </li>
	<li>Ha az így kapott összeg meghaladja a 372 ezer Ft-ot havonta, akkor degresszálni kell (először 90%-kal, majd 421 ezer Ft felett 80%-kal). </li>
	<li>Végül az így kapott összeget meg kell szorozni a szolgálati évekre eső szorzóval (20 év után ez 53%, 30 évnél 68%, 40 évnél 80%, stb.). A maximális szolgálati idő 50 év, a minimális 15 év (de a kalkulátor 10-15 év közötti időszakra is tud számítást végezni).</li>
	</ol>
	<p>Az adatok tájékoztató jellegűek, pontosabb számításra a <a href="https://www.allamkincstar.gov.hu/nyugdij/sajat-jogu-ellatasok/oregsegi-nyugdij/onkiszolgalo-nyugdijkalkulator">Magyar Államkincstár nyugdíjkalkulátora</a> javasolt.</p>

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
                <th>Éves nettó kereset (Ft)</th>
                <th>Valorizált éves kereset</th>
                <th>Éves átlag nettó kereset (irányadó)</th>
				<th>Éves bruttó kereset járulékplafonja</th>
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

// --- ÚJ ADATLISTA: ADÓALAP-HATÁROK ---
const CONST_TAX_LIMIT = [
  , , , , 750000, 915000, 912500, 912500, 915000, 1204500,
  1565850, 1854200, 2020320, 2197300, 2368850, 3905500, 5307000,
  6000600, 6325450, 6748850, 7137000, 7446000, 7453300, 7665000, 7942200
];

const SERVICE_TABLE = {
  10: 33, 11: 35, 12: 37, 13: 39, 14: 41,
  15: 43, 16: 45, 17: 47, 18: 49, 19: 51,
  20: 53, 21: 55, 22: 57, 23: 59, 24: 61,
  25: 63, 26: 64, 27: 65, 28: 66, 29: 67,
  30: 68, 31: 69, 32: 70, 33: 71, 34: 72,
  35: 73, 36: 74, 37: 75.5, 38: 77, 39: 78.5,
  40: 80, 41: 82, 42: 84, 43: 86, 44: 88,
  45: 90, 46: 92, 47: 94, 48: 96, 49: 98, 50: 100
};

// ------- Segédfüggvények -------
function serviceMultiplier(years) {
  const y = Math.max(10, Math.min(50, years|0));
  let pct = SERVICE_TABLE[y];
  if (typeof pct !== 'number') {
    const baseAt40 = SERVICE_TABLE[40] ?? 80;
    const extra = Math.max(0, y - 40) * 2;
    pct = Math.min(100, baseAt40 + extra);
  }
  if (y > 40) {
    const baseAt40 = SERVICE_TABLE[40] ?? 80;
    pct = Math.min(100, baseAt40 + (y - 40) * 2);
  }
  return pct / 100;
}

function formatPct(mult) {
  const pct = mult * 100;
  return (pct % 1 === 0) ? `${pct.toFixed(0)}%` : `${pct.toFixed(1)}%`;
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

// ------- DOM elemek -------
const rowsEl = document.getElementById('rows');
const serviceRange = document.getElementById('serviceYears');
const serviceLabel = document.getElementById('serviceYearsLabel');
const resultEl = document.getElementById('result');
const infoEl = document.getElementById('serviceInfo');
const breakdownEl = document.getElementById('breakdown');

// Védőkorlát: ha kritikus elemek hiányoznak, jelezzük
if (!rowsEl || !serviceRange || !serviceLabel || !resultEl || !infoEl || !breakdownEl) {
  console.error('Hiányzó DOM elemek: ellenőrizd az #rows, #serviceYears, #serviceYearsLabel, #result, #serviceInfo, #breakdown elemeket.');
}

// ------- Sorok és inputok felépítése -------
const inputs = [];
YEARS.forEach((y, i) => {
  const tr = document.createElement('tr');

  const tdY = document.createElement('td'); tdY.textContent = y;
  const tdM = document.createElement('td'); tdM.textContent = YEAR_MULTS[i].toFixed(3);

  const tdIn = document.createElement('td');
  const inp = document.createElement('input');
  inp.type = 'number'; inp.min = '0'; inp.step = '1000'; inp.placeholder = '0'; inp.inputMode = 'numeric';
  tdIn.appendChild(inp);

  const tdVal = document.createElement('td'); tdVal.className = 'muted'; tdVal.textContent = '—';

  const tdGuide = document.createElement('td'); tdGuide.className = 'muted mono';
  tdGuide.textContent = formatFt(ANNUAL_NET[i] || 0);

  const tdTax = document.createElement('td');
  const limit = CONST_TAX_LIMIT[i];
  tdTax.className = 'muted mono';
  tdTax.textContent = limit ? formatFt(limit) : '—';

  tr.append(tdY, tdM, tdIn, tdVal, tdGuide, tdTax);
  rowsEl.appendChild(tr);
  inputs.push({ inp, tdVal });
});

// ------- Számítás -------
function recalc(){
  let sumValorizalt = 0;
  let filledCount = 0; // kitöltött mezők darabszáma (a "0" is számít kitöltöttnek)

  inputs.forEach(({inp, tdVal}, i) => {
    const hasValue = String(inp.value ?? '').trim() !== '';
    const raw = parseFloat(inp.value || '0');
    const valor = raw * YEAR_MULTS[i];

    // 0 esetén "—" jelenik meg (megegyezik a korábbi viselkedéssel)
    tdVal.textContent = raw ? formatFt(valor) : '—';
    tdVal.className = raw ? '' : 'muted';

    if (isFinite(valor)) sumValorizalt += valor;
    if (hasValue) filledCount += 1; // a 0 is számít kitöltöttnek
  });

  const years = parseInt(serviceRange.value || '0', 10) || 0;
  const sMult = serviceMultiplier(years);
  const sMultPct = formatPct(sMult);

  // Osztás a kitöltött mezők számával (min. 1)
  const divisor = Math.max(1, filledCount);
  const avgPerEntered = sumValorizalt / divisor;

  // Havi bruttó (szorzó nélkül)
  const grossMonthlyBeforeDeg = avgPerEntered / 12;

  // Degresszió
  const prog = progressiveDegression(grossMonthlyBeforeDeg);
  const monthlyAfterDegression = prog.value;

  // Szolgálati szorzó a degresszió után
  const finalMonthly = monthlyAfterDegression * sMult;

  // Kiírások
  resultEl.innerHTML = `${formatFt(finalMonthly)} <small>havi várható nyugdíj</small>`;
  serviceLabel.textContent = `${years} év`;

  infoEl.textContent =
    `Szolgálati szorzó: ${sMultPct} | Éves valorizált összes: ${formatFt(sumValorizalt)} | / ${divisor} megadott év = ${formatFt(avgPerEntered)}`;

  breakdownEl.innerHTML =
    `Összes valorizált kereset: <strong>${formatFt(sumValorizalt)}</strong><br/>
     Osztás megadott évek számával: <strong>${divisor}</strong> = <strong>${formatFt(avgPerEntered)}</strong><br/>
     Havi életpálya átlagkereset = <strong>${formatFt(grossMonthlyBeforeDeg)}</strong><br/>
     Degresszió:<br/>
     - ${prog.parts.join('<br/>- ')}<br/>
     Degresszió utáni havi: <strong>${formatFt(monthlyAfterDegression)}</strong><br/>
     Szolgálati szorzó alkalmazása: ×<strong>${sMultPct}</strong> → <strong>${formatFt(finalMonthly)}</strong>`;
}

// ------- Események és inicializálás -------
// (ez hiányzott, ezért nem frissültek a számok gépeléskor)
inputs.forEach(({inp}) => inp.addEventListener('input', recalc));
if (serviceRange) serviceRange.addEventListener('input', recalc);

// Első kalkuláció
recalc();
</script>
