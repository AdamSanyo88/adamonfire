---
layout: page
title: Személyes infláció kalkulátor
permalink: /inflation
---

<h1 class="page-title">{{ page.title | escape }}</h1>

<html lang="hu">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Személyes infláció kalkulátor</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <style>
    body { background: #f8fafc; }
    .badge-fixed { font-variant-numeric: tabular-nums; }
    .table thead th { white-space: nowrap; }
    .muted { color:#6c757d; }
  </style>
</head>
<body>
<div class="container py-4">

  <p class="muted">
    Add meg, mennyit költesz havonta az alábbi kategóriákban.  
    A kalkulátor a <strong>KSH 10 éves átlagos inflációs rátáit</strong> használja fixen,  
    és ezek alapján kiszámítja a <strong>személyes inflációdat</strong>.
  </p>

  <div class="row g-4 align-items-start">
    <!-- BAL OSZLOP: Kategóriák és költés -->
    <div class="col-lg-7">
      <div class="card shadow-sm">
        <div class="card-body">
          <h2 class="h6 mb-3">Havi átlagos költés kategóriánként</h2>
          <div class="table-responsive">
            <table class="table table-sm table-striped align-middle">
              <thead>
                <tr>
                  <th>Kategória</th>
                  <th class="text-end">Költés</th>
                  <th class="text-end">Relatív súly</th>
                  <th class="text-end">10 év átlagos infláció</th>
                </tr>
              </thead>
              <tbody id="spendBody"></tbody>
              <tfoot>
                <tr>
                  <th>Összes költés</th>
                  <th class="text-end"><span id="totalSpend" class="badge bg-light text-dark badge-fixed">0,00</span></th>
                  <th class="text-end"><span class="badge bg-light text-dark badge-fixed">100,0%</span></th>
                  <th></th>
                </tr>
              </tfoot>
            </table>
          </div>
          <p class="small muted mb-0"></p>
        </div>
      </div>
    </div>

    <!-- JOBB OSZLOP: Eredmény -->
    <div class="col-lg-5">
      <div class="card shadow-sm">
        <div class="card-body">
          <h2 class="h6 mb-3">Eredmény</h2>

          <div class="d-flex justify-content-between">
            <div><strong>Személyes infláció</strong> (súlyozott átlag):</div>
            <div><span id="personalInfl" class="badge bg-primary badge-fixed fs-5">0,0%</span></div>
          </div>

          <hr>
          <p class="small muted mb-0">
            Az inflációs százalékok a 2015–2024 közötti időszak KSH által közölt <strong>átlagos éves inflációját</strong> mutatják kategóriánként.
          </p>
        </div>
      </div>
    </div>
  </div>
</div>

<script>
  // --- 15 kategória a megadott 10 éves átlagos inflációval (decimális formában)
  const CATS = [
    { key: "elelmiszer", label: "Élelmiszer", rate: 0.082 },
    { key: "alkohol", label: "Alkohol", rate: 0.055 },
	{ key: "dohany", label: "Dohányáru", rate: 0.098 },
    { key: "ruhazat", label: "Ruházat", rate: 0.023 },
    { key: "lakasszolg", label: "Rezsi és lakáshoz kötött költségek", rate: 0.043 },
    { key: "lakber", label: "Lakberendezés és lakás karbantartása", rate: 0.047 },
    { key: "egeszsegugy", label: "Egészségügy", rate: 0.057 },
    { key: "auto", label: "Közlekedés saját autóval", rate: 0.058 },
	{ key: "auto_ertek", label: "Autó értékvesztése", rate: 0.027 },
	{ key: "egyeb_kozlekedes", label: "Egyéb közlekedés", rate: 0.074 },
    { key: "tavkozles", label: "Távközlés", rate: 0.008 },
	{ key: "oktatas", label: "Oktatás", rate: 0.041 },
    { key: "szabadido", label: "Szabadidő és hobbi", rate: 0.044 },
    { key: "vendeglatas", label: "Vendéglátás és szálláshely", rate: 0.090 },
    { key: "egyeb", label: "Egyéb termékek és szolgáltatások (pl. biztosítások, banki díjak)", rate: 0.051 },
  ];

  const fmtPct = (v) => (v * 100).toFixed(1).replace('.', ',') + '%';
  const fmtAmt = (v) => new Intl.NumberFormat('hu-HU', { minimumFractionDigits: 2, maximumFractionDigits: 2 }).format(v);

  // --- táblázat felépítése
  const spendBody = document.getElementById('spendBody');
  CATS.forEach(c => {
    const tr = document.createElement('tr');
    tr.innerHTML = `
      <td>${c.label}</td>
      <td class="text-end">
        <input type="number" class="form-control form-control-sm text-end" id="amt_${c.key}" min="0" step="100" value="0">
      </td>
      <td class="text-end"><span id="w_${c.key}" class="badge bg-light text-dark badge-fixed">0,0%</span></td>
      <td class="text-end text-muted">${fmtPct(c.rate)}</td>
    `;
    spendBody.appendChild(tr);
  });

  function readSpends() {
    const out = {};
    CATS.forEach(c => {
      const v = Number(document.getElementById('amt_' + c.key).value) || 0;
      out[c.key] = Math.max(0, v);
    });
    return out;
  }

  function recompute() {
    const spends = readSpends();
    const total = Object.values(spends).reduce((s, v) => s + v, 0);

    // relatív súlyok cellánként
    CATS.forEach(c => {
      const w = total > 0 ? spends[c.key] / total : 0;
      document.getElementById('w_' + c.key).textContent = fmtPct(w);
    });

    document.getElementById('totalSpend').textContent = fmtAmt(total);

    // személyes infláció = súlyozott átlag
    let personal = 0, sumW = 0;
    CATS.forEach(c => {
      const w = total > 0 ? spends[c.key] / total : 0;
      sumW += w;
      personal += w * c.rate;
    });

    document.getElementById('personalInfl').textContent = (personal * 100).toFixed(1).replace('.', ',') + '%';
  }

  // eseményfigyelés
  document.addEventListener('DOMContentLoaded', () => {
    CATS.forEach(c => {
      document.getElementById('amt_' + c.key).addEventListener('input', recompute);
    });
    recompute();
  });
</script>
</body>
</html>
