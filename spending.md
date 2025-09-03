---
layout: page
title: Spending calculator
permalink: /spending
---

<h1 class="page-title">{{ page.title | escape }}</h1>

<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Monthly Spending Redistributor</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <style>
    body { background: #f8fafc; }
    .category-removed { opacity: 0.5; }
    .badge-fixed { font-variant-numeric: tabular-nums; }
    .table thead th { white-space: nowrap; }
	
	/* your existing styles */
  body { background: #f8fafc; }
  .category-removed { opacity: 0.5; }
  .badge-fixed { font-variant-numeric: tabular-nums; }
  .table thead th { white-space: nowrap; }

  /* Page-scoped nav overrides (Materialize markup) */
  nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a,
  nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a:link,
  nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a:visited,
  nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a:hover,
  nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a:focus,
  nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a:active,
  nav.blue.accent-4 .nav-wrapper ul#nav-mobile.side-nav li > a {
    text-decoration: none !important;
    border-bottom: 0 !important;   /* handles themes that fake underline as border */
    box-shadow: none !important;    /* handles themes using bottom shadow */
  }

  /* Active item variants */
  nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li.active > a,
  nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a.active {
    text-decoration: none !important;
    border-bottom: 0 !important;
    box-shadow: none !important;
  }

  /* Kill any pseudo-element underline decorations */
  nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a::after {
    content: none !important;
    border: 0 !important;
    box-shadow: none !important;
  }

  /* Keep icons themselves clean */
  nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a .material-icons,
  nav.blue.accent-4 .nav-wrapper ul#nav-mobile.side-nav li > a .material-icons {
    text-decoration: none;
    display: inline-flex;
    vertical-align: middle;
  }
	
  </style>
</head>
<body>
  <div class="container py-4">
    <p class="text-muted mb-4">
      Add meg a havi költésedet. A kalkulátor a magyar fogyasztói kosár súlyai szerint osztja szét.
      A kijelölt tételek eltávolításra kerülnek, majd a súlyuk arányosan újraelosztódik a maradék kategóriák között. A kalkulátor lakbért nem számol, ezt érdemes kivonni a teljes költségvetésből.
    </p>

    <div class="row g-4">
      <div class="col-md-6">
        <div class="card shadow-sm">
          <div class="card-body">
            <label for="monthlyInput" class="form-label">Havi átlagos költés</label>
            <div class="input-group mb-3">
              <input id="monthlyInput" type="number" class="form-control" min="0" step="100" value="0" />
            </div>

            <div class="form-check mb-2">
              <input class="form-check-input" type="checkbox" id="noSmoke">
              <label class="form-check-label" for="noSmoke">Nem dohányzom</label>
            </div>
            <div class="form-check mb-2">
              <input class="form-check-input" type="checkbox" id="noDrink">
              <label class="form-check-label" for="noDrink">Nem iszom alkoholt</label>
            </div>
            <div class="form-check mb-2">
              <input class="form-check-input" type="checkbox" id="noHouse">
              <label class="form-check-label" for="noHouse">Nincs saját házam</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="checkbox" id="noCar">
              <label class="form-check-label" for="noCar">Nincs autóm</label>
            </div>

            <div id="removedAlert" class="alert alert-secondary mt-3 mb-0" role="alert">
              Eltávolított eredeti súly összesen: <strong><span id="removedPct">0.0</span>%</strong>
            </div>
          </div>
        </div>
      </div>

      <div class="col-md-6">
        <div class="card shadow-sm">
          <div class="card-body">
            <h2 class="h6 mb-2">Összefoglaló</h2>
            <ul class="small text-muted mb-0">
              <li>Az eredeti súlyok a KSH fogyasztói kosárból származnak normalizálva 100%-ra.</li>
              <li>Az eltávolított kategóriák súlya arányosan kerül szétosztásra a megmaradt kategóriák között.</li>
            </ul>
          </div>
        </div>
      </div>
    </div>

    <div class="card mt-4 shadow-sm">
      <div class="card-body table-responsive">
        <table class="table table-sm table-striped align-middle">
          <thead>
            <tr>
              <th>Kategória</th>
              <th class="text-end">Súly (eredeti)</th>
              <th class="text-end">Súly (új)</th>
              <th class="text-end">Összeg</th>
            </tr>
          </thead>
          <tbody id="tableBody"></tbody>
          <tfoot>
            <tr>
              <th>Összesen</th>
              <th class="text-end">100,0%</th>
              <th class="text-end">100,0%</th>
              <th class="text-end"><span id="totalCell" class="badge bg-light text-dark badge-fixed">0,00</span></th>
            </tr>
          </tfoot>
        </table>
      </div>
    </div>

    <p class="text-muted small mt-2 mb-0">Megjegyzés: a pénznem tetszőleges; a táblázatban szereplő összegek a megadott havi összegből számolódnak.</p>
  </div>

  <script>
    // Hungarian CPI basket weights (normalized to 1.0)
    const BASE = [
      { key: "elelmiszer", label: "Élelmiszer", weight: 0.2274 },
      { key: "alkohol", label: "Alkohol", weight: 0.0325 },
      { key: "dohanyaru", label: "Dohányárú", weight: 0.0591 },
      { key: "ruhazat", label: "Ruházat", weight: 0.0341 },
      { key: "rezsi_lakas", label: "Rezsi és lakáshoz kötött költségek", weight: 0.1152 },
      { key: "lakber_karb", label: "Lakberendezés és lakás karbantartása", weight: 0.0717 },
      { key: "egeszsegugy", label: "Egészségügy", weight: 0.0545 },
      { key: "auto_kozlekedes", label: "Közlekedés saját autóval", weight: 0.0872 },
      { key: "auto_vasarlas", label: "Saját autó értékvesztése", weight: 0.0437 },
      { key: "egyeb_kozlekedes", label: "Egyéb közlekedés", weight: 0.0140 },
      { key: "tavkozles", label: "Távközlés", weight: 0.0422 },
      { key: "oktatas", label: "Oktatás", weight: 0.0222 },
      { key: "szabadido", label: "Szabadidő és hobbi", weight: 0.0734 },
      { key: "vendeglatas", label: "Vendéglátás", weight: 0.0495 },
      { key: "egyeb_szolg", label: "Egyéb szolgáltatás (pl. biztosítások, banki díjak)", weight: 0.0733 },
    ];

    const fmtPct = (v) => (v * 100).toFixed(1).replace('.', ',');
    const fmtAmt = (v) => new Intl.NumberFormat(undefined, { minimumFractionDigits: 2, maximumFractionDigits: 2 }).format(v);

    function redistribute(base, removedSet) {
      const keep = base.filter(c => !removedSet.has(c.key));
      const removed = base.filter(c => removedSet.has(c.key));
      const sumKeep = keep.reduce((s, c) => s + c.weight, 0);
      const sumRemoved = removed.reduce((s, c) => s + c.weight, 0);

      if (sumKeep <= 0) {
        return base.map(c => ({ ...c, adjWeight: 0 }));
      }
      const adjusted = base.map(c => {
        if (removedSet.has(c.key)) return { ...c, adjWeight: 0 };
        const share = c.weight / sumKeep;
        const adjWeight = share * (sumKeep + sumRemoved);
        return { ...c, adjWeight };
      });
      const totalAdj = adjusted.reduce((s, c) => s + c.adjWeight, 0);
      return adjusted.map(c => ({ ...c, adjWeight: c.adjWeight / totalAdj }));
    }

    function update() {
      const monthly = Math.max(0, Number(document.getElementById('monthlyInput').value) || 0);
      const removedSet = new Set();
      if (document.getElementById('noSmoke').checked) removedSet.add('dohanyaru');
      if (document.getElementById('noDrink').checked) removedSet.add('alkohol');
      if (document.getElementById('noHouse').checked) removedSet.add('lakber_karb');
      if (document.getElementById('noCar').checked) {
        ['auto_kozlekedes', 'auto_vasarlas'].forEach(item => removedSet.add(item));
      } // <-- missing brace added here ✅

      const adjusted = redistribute(BASE, removedSet);

      // Compute rounded amounts and correct to sum exactly to monthly
      const raw = adjusted.map(r => r.adjWeight * monthly);
      const rounded = raw.map(v => Math.round(v * 100) / 100);
      const sumRounded = rounded.reduce((s, v) => s + v, 0);
      const diff = Math.round((monthly - sumRounded) * 100) / 100;

      // apply diff to the largest kept category
      let idxMax = -1, maxW = -Infinity;
      adjusted.forEach((r, i) => { if (r.adjWeight > maxW) { maxW = r.adjWeight; idxMax = i; } });
      if (idxMax >= 0) rounded[idxMax] = Math.round((rounded[idxMax] + diff) * 100) / 100;

      // Fill table
      const tbody = document.getElementById('tableBody');
      tbody.innerHTML = '';
      adjusted.forEach((r, i) => {
        const tr = document.createElement('tr');
        if (r.adjWeight === 0) tr.classList.add('category-removed');
        tr.innerHTML = `
          <td>${r.label}</td>
          <td class="text-end">${fmtPct(BASE[i].weight)}%</td>
          <td class="text-end">${fmtPct(r.adjWeight)}%</td>
          <td class="text-end"><span class="badge bg-light text-dark badge-fixed">${fmtAmt(rounded[i])}</span></td>
        `;
        tbody.appendChild(tr);
      });

      // Removed total percent
      const removedPct = BASE.filter(c => removedSet.has(c.key)).reduce((s, c) => s + c.weight, 0);
      document.getElementById('removedPct').textContent = (removedPct * 100).toFixed(1).replace('.', ',');

      // Total cell
      document.getElementById('totalCell').textContent = fmtAmt(monthly);
    }

    document.addEventListener('DOMContentLoaded', () => {
      ['monthlyInput', 'noSmoke', 'noDrink', 'noHouse', 'noCar'].forEach(id => {
        const el = document.getElementById(id);
        el.addEventListener(el.type === 'number' ? 'input' : 'change', update);
      });
      update();
    });
  </script>
</body>
</html>