---
layout: page
title: Personal spending calculator
permalink: /spending
---

<h1 class="page-title">{{ page.title | escape }}</h1>


<html lang="hu">
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

    /* Page-scoped nav overrides (Materialize markup) */
    nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a,
    nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a:link,
    nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a:visited,
    nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a:hover,
    nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a:focus,
    nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a:active,
    nav.blue.accent-4 .nav-wrapper ul#nav-mobile.side-nav li > a {
      text-decoration: none !important;
      border-bottom: 0 !important;
      box-shadow: none !important;
    }

    nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li.active > a,
    nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a.active {
      text-decoration: none !important;
      border-bottom: 0 !important;
      box-shadow: none !important;
    }

    nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a::after {
      content: none !important;
      border: 0 !important;
      box-shadow: none !important;
    }

    nav.blue.accent-4 .nav-wrapper ul.right.hide-on-med-and-down > li > a .material-icons,
    nav.blue.accent-4 .nav-wrapper ul#nav-mobile.side-nav li > a .material-icons {
      text-decoration: none;
      display: inline-flex;
      vertical-align: middle;
    }

    /* --- NEW: minden szöveg fekete --- */
    .spending-table,
    .spending-table h2,
    .spending-table .table,
    .spending-table .table th,
    .spending-table .table td,
    .spending-table .badge,
    .form-label,
    .form-check-label {
      color: #000 !important;
      font-family: inherit !important;
      font-weight: normal !important;
    }
  
    /* === Calculator visual refresh === */
    .calculator-shell{background:linear-gradient(135deg,#eef6ff 0%,#f8fbff 100%);border:1px solid #d7e9ff;border-radius:22px;padding:24px;box-shadow:0 14px 36px rgba(13,110,253,.08)}
    .calculator-kicker{font-size:.75rem;font-weight:800;letter-spacing:.1em;text-transform:uppercase;color:#0d6efd;margin-bottom:6px}
    .calculator-shell .card{border:1px solid #e3eaf2;border-radius:16px;overflow:hidden;box-shadow:0 8px 22px rgba(15,23,42,.06)!important}
    .input-card{border-top:4px solid #0d6efd!important}
    #monthlyInput{background:#eaf4ff!important;border:2px solid #90c2ff!important;border-radius:10px!important;font-size:1.35rem!important;font-weight:800!important;padding:.75rem 1rem!important}
    #monthlyInput:focus{background:#fff7d6!important;border-color:#e0a800!important;box-shadow:0 0 0 3px rgba(224,168,0,.14)!important}
    .calculator-shell .form-check{background:#f8fafc;border:1px solid #e6ebf1;border-radius:10px;padding:.65rem .8rem .65rem 2.3rem;margin-bottom:.5rem!important}
    .calculator-shell .form-check-input{margin-left:-1.45rem}
    .spending-table thead th{background:#f1f5f9!important;color:#475569!important;font-size:.75rem;text-transform:uppercase;letter-spacing:.03em}
    .spending-table tbody tr:not(.category-removed) td:last-child .badge{background:#eaf4ff!important;color:#0756c9!important;border:1px solid #b9d9ff}
    .spending-table tfoot th{background:#eaf2fb!important;font-weight:800!important}
    .summary-card{background:linear-gradient(135deg,#0d6efd,#0756c9)!important;color:#fff!important;border:0!important}
    .summary-card h2,.summary-card li{color:#fff!important}
    .summary-card .text-muted{color:rgba(255,255,255,.84)!important}
    @media(max-width:768px){.calculator-shell{padding:14px}}

</style>
</head>
<body>
  <div class="container py-4">
    <div class="calculator-shell">
      <div class="calculator-kicker">Interactive calculator</div>
    <p class="text-muted mb-4">
      Enter your monthly spending. The calculator allocates it according to the weights of the Hungarian consumer basket.
      Selected items are removed and their weights are redistributed proportionally across the remaining categories. The calculator does not include rent, so you should subtract it from your total budget.
    </p>

    <div class="row g-4 align-items-start">
      <div class="col-md-6">
        <div class="card shadow-sm mb-4 summary-card">
          <div class="card-body">
            <h2 class="h6 mb-2">Summary</h2>
            <ul class="small text-muted mb-0">
              <li>The original weights are based on the KSH consumer basket, normalized to 100%.</li>
              <li>The weight of removed categories is redistributed proportionally across the remaining categories.</li>
            </ul>
          </div>
        </div>

        <div class="card shadow-sm input-card">
          <div class="card-body">
            <label for="monthlyInput" class="form-label">Average monthly spending</label>
            <div class="input-group mb-3">
              <input id="monthlyInput" type="number" class="form-control" min="0" step="100" value="0" />
            </div>

            <div class="form-check mb-2">
              <input class="form-check-input" type="checkbox" id="noSmoke">
              <label class="form-check-label" for="noSmoke">I do not smoke</label>
            </div>
            <div class="form-check mb-2">
              <input class="form-check-input" type="checkbox" id="noDrink">
              <label class="form-check-label" for="noDrink">I do not drink alcohol</label>
            </div>
            <div class="form-check mb-2">
              <input class="form-check-input" type="checkbox" id="noHouse">
              <label class="form-check-label" for="noHouse">I do not own a home</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="checkbox" id="noCar">
              <label class="form-check-label" for="noCar">I do not own a car</label>
            </div>

            <div id="removedAlert" class="alert alert-secondary mt-3 mb-0" role="alert">
              Total original weight removed: <strong><span id="removedPct">0.0</span>%</strong>
            </div>
          </div>
        </div>
      </div>

      <div class="col-md-6">
        <div class="card mt-md-0 mt-4 shadow-sm spending-table">
          <div class="card-body table-responsive">
            <h2 class="h6 mb-3">Categories and average monthly spending</h2>
            <table class="table table-sm table-striped align-middle">
              <thead>
                <tr>
                  <th>Category</th>
                  <th class="text-end">Weight (original)</th>
                  <th class="text-end">Weight (adjusted)</th>
                  <th class="text-end">Amount</th>
                </tr>
              </thead>
              <tbody id="tableBody"></tbody>
              <tfoot>
                <tr>
                  <th>Total</th>
                  <th class="text-end">100,0%</th>
                  <th class="text-end">100,0%</th>
                  <th class="text-end"><span id="totalCell" class="badge bg-light text-dark badge-fixed">0,00</span></th>
                </tr>
              </tfoot>
            </table>
          </div>
        </div>
        <p class="text-muted small mt-2 mb-0">Note: you can use any currency; the amounts in the table are calculated from the monthly total you enter.</p>
      </div>
    </div>
    </div>
  </div>

  <script>
    const BASE = [
      { key: "elelmiszer", label: "Food", weight: 0.2274 },
      { key: "alkohol", label: "Alkohol", weight: 0.0325 },
      { key: "dohanyaru", label: "Tobacco", weight: 0.0591 },
      { key: "ruhazat", label: "Clothing", weight: 0.0341 },
      { key: "rezsi_lakas", label: "Utilities and housing-related costs", weight: 0.1152 },
      { key: "lakber_karb", label: "Furnishings and home maintenance", weight: 0.0717 },
      { key: "egeszsegugy", label: "Healthcare", weight: 0.0545 },
      { key: "auto_kozlekedes", label: "Transport by own car", weight: 0.0872 },
      { key: "auto_vasarlas", label: "Car depreciation", weight: 0.0437 },
      { key: "egyeb_kozlekedes", label: "Other transport", weight: 0.0140 },
      { key: "tavkozles", label: "Telecommunications", weight: 0.0422 },
      { key: "oktatas", label: "Education", weight: 0.0222 },
      { key: "szabadido", label: "Leisure and hobbies", weight: 0.0734 },
      { key: "vendeglatas", label: "Restaurants and accommodation", weight: 0.0495 },
      { key: "egyeb_szolg", label: "Other services (e.g. insurance, bank fees)", weight: 0.0733 },
    ];

    const fmtPct = (v) => (v * 100).toFixed(1).replace('.', ',');
    const fmtAmt = (v) => new Intl.NumberFormat(undefined, { minimumFractionDigits: 2, maximumFractionDigits: 2 }).format(v);

    function redistribute(base, removedSet) {
      const keep = base.filter(c => !removedSet.has(c.key));
      const removed = base.filter(c => removedSet.has(c.key));
      const sumKeep = keep.reduce((s, c) => s + c.weight, 0);
      const sumRemoved = removed.reduce((s, c) => s + c.weight, 0);
      if (sumKeep <= 0) return base.map(c => ({ ...c, adjWeight: 0 }));
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
      if (document.getElementById('noCar').checked) ['auto_kozlekedes', 'auto_vasarlas'].forEach(item => removedSet.add(item));

      const adjusted = redistribute(BASE, removedSet);
      const raw = adjusted.map(r => r.adjWeight * monthly);
      const rounded = raw.map(v => Math.round(v * 100) / 100);
      const sumRounded = rounded.reduce((s, v) => s + v, 0);
      const diff = Math.round((monthly - sumRounded) * 100) / 100;
      let idxMax = -1, maxW = -Infinity;
      adjusted.forEach((r, i) => { if (r.adjWeight > maxW) { maxW = r.adjWeight; idxMax = i; } });
      if (idxMax >= 0) rounded[idxMax] = Math.round((rounded[idxMax] + diff) * 100) / 100;

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

      const removedPct = BASE.filter(c => removedSet.has(c.key)).reduce((s, c) => s + c.weight, 0);
      document.getElementById('removedPct').textContent = (removedPct * 100).toFixed(1).replace('.', ',');
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

