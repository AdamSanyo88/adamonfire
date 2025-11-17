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
  max-width: 100%;      /* NE korlátozzuk a szélességet */
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
  grid-template-columns: 1fr; /* mindig 1 oszlop */
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
    <p>Add meg az <strong>éves bruttó kereseteidet minden általad ledolgozott évben</strong>. Fontos, hogy a munkabérként és prémiumként kapott bejelentett és leadózott jövedelemmel számolj csak.</p>
	<p>A kalkulátor figyelembe veszi az adott év <em>valorizációs szorzóját</em>, a <em>szolgálati idő szorzóját</em>, a lépcsőzetes <em>degressziót</em>, és ezek alapján kiszámolja, mi a várható havi nyugdíjad. Egyéb tényezőkkel és kedvezményekkel nem számol a modell, csak a kereseti adatokkal.</p>
	<p>A kalkulátor nem számol továbbá a 2013 előtti éves járulékplafonnal, de a táblázat megmutatja, hogy milyen éves bruttó bér volt a maximum, amelyre nyugdíjjárulékot kellett fizetni. Ennél nagyobb bruttó éves összeget ne adj meg az adott sorban.</p>
	<p>Mindig egész éves keresetet adj meg, mert törtévvel nem tud számolni a kalkulátor.</p>
	<br/>
	<h5>Nyugdíjszámítás menete a gyakorlatban</h5>
	<ol>
	<li>Az egész élet során szerzett szolgálati évek egész számban, lefelé kerekítve (a törtév elvész).</li>
	<li>Az 1988 január 1-je óta szerzett jövedelmek nettósított éves értéke (először levonjuk a nyugdíjjárulékot, majd ebből az összegből az adót).</li>
	<li>A nettó értékeket minden évben fel kell szorozni az éves valorizációs szorzóval. Ez megmutatja, hogy mai áron számolva az akkori kereset mennyit ért. Gyakorlatban a fő szempont az, hogy az adott éves jövedelem az adott év nettó átlagkeresete alatt vagy felett volt-e (ez határozza meg a relatív értékét).</li>
	<li>A kapott összegeket el kell osztani a teljes szolgálati idővel (napokban számolva). Ezt az összeget utána fel kell szorozni 365-tel, majd osztani 12-vel, így megkapva a havi nettó életpálya átlagkeresetet.</li>
	<li>Ha az így kapott összeg meghaladja a 372 ezer Ft-ot havonta, akkor degresszálni kell (először 90%-kal, majd 421 ezer Ft felett 80%-kal). </li>
	<li>Végül az így kapott összeget meg kell szorozni a szolgálati évekre eső szorzóval (20 év után ez 53%, 30 évnél 68%, 40 évnél 80%, stb.). A maximális szolgálati idő 50 év, a minimális 15 év (de a kalkulátor 10-15 év közötti időszakra is tud számítást végezni).</li>
	</ol>
	<p>Az adatok tájékoztató jellegűek, pontosabb számításra a <a href="https://www.allamkincstar.gov.hu/nyugdij/sajat-jogu-ellatasok/oregsegi-nyugdij/onkiszolgalo-nyugdijkalkulator">Magyar Államkincstár nyugdíjkalkulátora</a> javasolt.</p>

  <div class="grid">
    <div class="left card">
      <div class="row" style="margin-bottom:12px">
        <label for="serviceYears"><strong>Szolgálati évek száma</strong></label>
        <input id="serviceYears" type="range" min="10" max="50" step="1" value="15" />
        <strong id="serviceYearsLabel">15 év</strong>
      </div>

      <div style="max-height:55vh; overflow:auto; border-radius:12px; border:1px solid rgba(0,0,0,.06)">
        <table>
          <thead>
            <tr>
              <th>Év</th>
              <th>Szorzó</th>
              <th>Éves bruttó kereset (Ft)</th>
              <th>Valorizált éves kereset</th>
              <th>Éves átlag nettó keresetek (irányadó)</th>
              <th>Éves bruttó kereset járulékplafonja</th>
              <th>Járulék levonás után</th>
              <th>Fizetendő adó</th>
              <th>Éves nettó kereset</th>
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
        <div class="result" id="result">— <small>havi várható nyugdíj</small></div>
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
const ANNUAL_NET=[85017,97676,119400,150646,184594,215210,271801,308088,365329,457200,542400,601200,669600,778800,931200,1065600,1125600,1237200,1330800,1369200,1464000,1489200,1591200,1693200,1728000,1812000,1868400,1947600,2100000,2370000,2632800,2935200,3220800,3501600,4115808,4701132,5325576];
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
    {th:0,      base:0,     rate:0,   cap:48000},
    {th:48000,  base:0,     rate:0.20,cap:70000},
    {th:70000,  base:4400,  rate:0.25,cap:90000},
    {th:90000,  base:9400,  rate:0.30,cap:120000},
    {th:120000, base:18400, rate:0.35,cap:150000},
    {th:150000, base:28900, rate:0.39,cap:180000},
    {th:180000, base:40600, rate:0.44,cap:240000},
    {th:240000, base:67000, rate:0.48,cap:360000},
    {th:360000, base:124600,rate:0.52,cap:600000},
    {th:600000, base:249400,rate:0.56,cap:800000},
    {th:800000, base:361400,rate:0.60}
  ],
  1989: [
    {th:0,      base:0,     rate:0,   cap:55000},
    {th:55000,  base:0,     rate:0.17,cap:70000},
    {th:70000,  base:2550,  rate:0.23,cap:100000},
    {th:100000, base:9450,  rate:0.29,cap:150000},
    {th:150000, base:23950, rate:0.35,cap:240000},
    {th:240000, base:55450, rate:0.42,cap:360000},
    {th:360000, base:105850,rate:0.49,cap:600000},
    {th:600000, base:223450,rate:0.56}
  ],
  1990: [
    {th:0,      base:0,     rate:0,   cap:55000},
    {th:55000,  base:0,     rate:0.15,cap:90000},
    {th:90000,  base:5250,  rate:0.30,cap:300000},
    {th:300000, base:68250, rate:0.40,cap:500000},
    {th:500000, base:148250,rate:0.50}
  ],
  1991: [
    {th:0,      base:0,     rate:0,   cap:55000},
    {th:55000,  base:0,     rate:0.12,cap:90000},
    {th:90000,  base:4200,  rate:0.18,cap:120000},
    {th:120000, base:9600,  rate:0.30,cap:150000},
    {th:150000, base:18600, rate:0.32,cap:300000},
    {th:300000, base:66600, rate:0.40,cap:500000},
    {th:500000, base:146600,rate:0.50}
  ],
  1992: [
    {th:0,      base:0,     rate:0,   cap:100000},
    {th:100000, base:0,     rate:0.25,cap:200000},
    {th:200000, base:25000, rate:0.35,cap:500000},
    {th:500000, base:130000,rate:0.40}
  ],
  1993: 'sameAs:1992',
  1994: [
    {th:0,      base:0,     rate:0,   cap:110000},
    {th:110000, base:0,     rate:0.20,cap:150000},
    {th:150000, base:8000,  rate:0.25,cap:220000},
    {th:220000, base:25500, rate:0.35,cap:380000},
    {th:380000, base:81500, rate:0.40,cap:550000},
    {th:550000, base:149500,rate:0.44}
  ],
  1995: 'sameAs:1994',
  1996: [
    {th:0,      base:0,     rate:0.20,cap:150000},
    {th:150000, base:30000, rate:0.25,cap:220000},
    {th:220000, base:47500, rate:0.35,cap:380000},
    {th:380000, base:103500,rate:0.40,cap:500000},
    {th:500000, base:171500,rate:0.44,cap:900000},
    {th:900000, base:325500,rate:0.48}
  ],
  1997: [
    {th:0,      base:0,     rate:0.20,cap:250000},
    {th:250000, base:50000, rate:0.22,cap:300000},
    {th:300000, base:61000, rate:0.31,cap:500000},
    {th:500000, base:123000,rate:0.35,cap:700000},
    {th:700000, base:193000,rate:0.39,cap:1100000},
    {th:1100000,base:349000,rate:0.42}
  ],
  1998: 'sameAs:1997',
  1999: [
    {th:0,      base:0,     rate:0.20,cap:400000},
    {th:400000, base:80000, rate:0.30,cap:1000000},
    {th:1000000,base:260000,rate:0.40}
  ],
  2000: 'sameAs:1999',
  2001: [
    {th:0,      base:0,     rate:0.20,cap:480000},
    {th:480000, base:96000, rate:0.30,cap:1050000},
    {th:1050000,base:267000,rate:0.40}
  ],
  2002: [
    {th:0,      base:0,     rate:0.20,cap:600000},
    {th:600000, base:120000,rate:0.30,cap:1200000},
    {th:1200000,base:300000,rate:0.40}
  ],
  2003: [
    {th:0,      base:0,     rate:0.20,cap:650000},
    {th:650000, base:130000,rate:0.30,cap:1350000},
    {th:1350000,base:340000,rate:0.40}
  ],
  2004: [
    {th:0,      base:0,     rate:0.18,cap:800000},
    {th:800000, base:144000,rate:0.26,cap:1500000},
    {th:1500000,base:326000,rate:0.38}
  ],
  2005: [
    {th:0,      base:0,     rate:0.18,cap:1500000},
    {th:1500000,base:270000,rate:0.38}
  ],
  2006: [
    {th:0,      base:0,     rate:0.18,cap:1550000},
    {th:1550000,base:279000,rate:0.36}
  ],
  2007: [
    {th:0,      base:0,     rate:0.18,cap:1700000},
    {th:1700000,base:306000,rate:0.36}
  ],
  2008: 'sameAs:2007',
  2009: [
    {th:0,      base:0,     rate:0.18,cap:1900000},
    {th:1900000,base:342000,rate:0.36}
  ],
  2010: [ // adóalap-kiegészítés: *1.27 a teljes alapra
    {th:0,      base:0,     rate:0.17,cap:5000000},
    {th:5000000,base:850000,rate:0.32}
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

/* ===== Adójóváírások 1988–2011 =====
   A jóváírást a számított adóból vonjuk le (min 0-ig).
   Megjegyzés: ahol a szöveg járulék(ok) 25%-át említi, a kalkulátorban
   elérhető "levont járulék" összeg 25%-ával közelítünk. */
function taxCreditAmount(year, rawIncome, contributionAmount){
  let c = 0;

  // 1988–1990: keresetből 12 000 Ft (itt adócsökkentésként kezeljük)
  if (year>=1988 && year<=1990){ c = 12000; return c; }

  // 1991: fix 3 000 Ft
  if (year===1991){ return 3000; }

  // 1993: fix 2 400 Ft
  if (year===1993){ return 2400; }

  // 1994: kereset 10%-a
  if (year===1994){ return rawIncome * 0.10; }

  // 1995: levont egészségbizt.+nyugdíjjárulék 25%-a (közelítés: teljes levont járulék 25%-a)
  if (year===1995){ return contributionAmount * 0.25; }

  // 1997: min(kereset 20%-a, 43 200)
  if (year===1997){ return Math.min(rawIncome * 0.20, 43200); }

  // 1998: min(kereset 20%-a, 50 400) + (nyugdíjjárulék + magánnyugdíj tagdíj) 25%
  if (year===1998){ return Math.min(rawIncome * 0.20, 50400) + (contributionAmount * 0.25); }

  // 1999–2001: min(kereset 10%-a, 36 000) + nyugdíjjárulék(+magánnyugdíj) 25%
  if (year>=1999 && year<=2001){ return Math.min(rawIncome * 0.10, 36000) + (contributionAmount * 0.25); }

  // 2002: min(kereset 18%-a, 60 000) + nyugdíjjárulék(+magánnyugdíj) 25%
  if (year===2002){ return Math.min(rawIncome * 0.18, 60000) + (contributionAmount * 0.25); }

  // 2003: min(kereset 18%-a, 108 000) + nyugdíjjárulék(+magánnyugdíj) 25%
  if (year===2003){ return Math.min(rawIncome * 0.18, 108000) + (contributionAmount * 0.25); }

  // 2004–2007: min(kereset 18%-a, 108 000)
  if (year>=2004 && year<=2007){ return Math.min(rawIncome * 0.18, 108000); }

  // 2008–2009: min(kereset 18%-a, 136 080)
  if (year===2008 || year===2009){ return Math.min(rawIncome * 0.18, 136080); }

  // 2010: min(kereset 17%-a, 181 200)
  if (year===2010){ return Math.min(rawIncome * 0.17, 181200); }

  // 2011: min(kereset 16%-a, 145 200)
  if (year===2011){ return Math.min(rawIncome * 0.16, 145200); }

  // 1992, 1996, 2012+ stb.: nincs
  return 0;
}

/* ===== Adó számítása egy évre (járulék UTÁNI alapra) ===== */
function taxForYear(year, baseAfterContribution, rawIncomeForCredits, contributionAmount){
  if (!Number.isFinite(baseAfterContribution) || baseAfterContribution<=0) return 0;

  // 2016–: 15%
  if (year>=2016) {
    const grossTax = baseAfterContribution*0.15;
    return Math.max(0, grossTax - taxCreditAmount(year, rawIncomeForCredits, contributionAmount));
  }

  // 2013–2015: 16%
  if (year>=2013 && year<=2015) {
    const grossTax = baseAfterContribution*0.16;
    return Math.max(0, grossTax - taxCreditAmount(year, rawIncomeForCredits, contributionAmount));
  }

  // 2012: 16%, adóalap-kiegészítés csak 2 424 000 feletti részre
  if (year===2012){
    const thr=2424000;
    const extra=Math.max(0, baseAfterContribution - thr)*0.27;
    const taxBase=baseAfterContribution + extra;
    const grossTax = taxBase*0.16;
    return Math.max(0, grossTax - taxCreditAmount(year, rawIncomeForCredits, contributionAmount));
  }

  // 2011: 16%, teljes adóalap +27%
  if (year===2011){
    const grossTax = (baseAfterContribution*1.27)*0.16;
    return Math.max(0, grossTax - taxCreditAmount(year, rawIncomeForCredits, contributionAmount));
  }

  // 2010: sávos + teljes 27% adóalap-kiegészítés
  if (year===2010){
    const taxBase = baseAfterContribution*1.27;
    const grossTax = applyBracketTable(2010, taxBase);
    return Math.max(0, grossTax - taxCreditAmount(year, rawIncomeForCredits, contributionAmount));
  }

  // 1988–2009: sávos
  const yr = Math.min(Math.max(1988, year), 2009);
  const grossTax = applyBracketTable(yr, baseAfterContribution);
  return Math.max(0, grossTax - taxCreditAmount(year, rawIncomeForCredits, contributionAmount));
}

/* Sávos adó: adott év tábláján lépegetünk */
function applyBracketTable(year, taxBase){
  const table = TAX_TABLES[year];
  let tax = 0;
  for (let i=0;i<table.length;i++){
    const {th, base, rate, cap} = table[i];
    if (taxBase>=th && (cap===undefined || taxBase<=cap)){
      tax = base + Math.max(0, taxBase - th)*rate;
      break;
    }
    if (cap!==undefined && taxBase>cap) continue;
  }
  if (tax===0){
    const first = table[0];
    if (taxBase<=first.cap){ tax = first.base + (taxBase-first.th)*first.rate; }
  }
  return Math.max(0, tax);
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

      // 1) Járulék
      const contr=contributionRatePct(year)/100;
      const contrAmt=raw*contr;
      const afterContr=Math.max(0, raw-contrAmt);
      tdAfterContr.textContent=formatFt(afterContr); tdAfterContr.className='';

      // 2) Adó – sávok szerint + ADÓJÓVÁÍRÁS LEVONÁSA
      const tax=taxForYear(year, afterContr, raw, contrAmt);
      const afterTax=Math.max(0, afterContr - tax);
      tdTax.textContent=formatFt(tax); tdTax.className=tax? '':'muted mono';

      tdFinal.textContent=formatFt(afterTax); tdFinal.className='';

      // 3) Valorizáció a végeredményből
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
    `Szolgálati szorzó: ${sMultPct} | Éves valorizált összes: ${formatFt(sumValorizalt)} | / ${divisor} megadott év = ${formatFt(avgPerEntered)}`;

  breakdownEl.innerHTML=
    `Összes valorizált kereset (járulék+adó után): <strong>${formatFt(sumValorizalt)}</strong><br/>
     Havi életpálya átlagkereset: <strong>${formatFt(grossMonthlyBeforeDeg)}</strong><br/>
     Degresszió:<br/>- ${prog.parts.join('<br/>- ')}<br/>
     Degresszió utáni havi: <strong>${formatFt(monthlyAfterDegression)}</strong><br/>
     Szolgálati szorzó: ×<strong>${sMultPct}</strong> → <strong>${formatFt(finalMonthly)}</strong>`;
}

/* Események + init */
inputs.forEach(({inp})=>inp.addEventListener('input', recalc));
document.getElementById('serviceYears').addEventListener('input', recalc);
recalc();

/* Reset */
document.getElementById('reset').addEventListener('click',(ev)=>{
  ev.preventDefault();
  inputs.forEach(({inp})=>{inp.value='';});
  recalc();
});
</script>

