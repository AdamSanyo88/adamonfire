---
layout: page
title: Household expenses calculator 
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
  </style>
</head>
<body>
  <div class="container py-4">
    <p class="text-muted mb-4">
      Enter your average monthly spending total. The calculator distributes your spending according to the weights of the Hungarian consumer basket.
      If selected items are removed, their weights are proportionally redistributed among the remaining categories. The calculator does not include rent, so it is recommended to subtract that from your total budget.
    </p>

    <div class="row g-4 align-items-start">
      <div class="col-md-6">
        <div class="card shadow-sm mb-4">
          <div class="card-body">
            <h2 class="h6 mb-2">Summary</h2>
            <ul class="small text-muted mb-0">
            <li>The original weights come from the Hungarian Central Statistical Office (KSH) consumer basket, normalized to 100%.</li>
			<li>The weights of the removed categories are proportionally redistributed among the remaining categories.</li>
            </ul>
          </div>
        </div>

        <div class="card shadow-sm">
          <div class="card-body">
            <label for="monthlyInput" class="form-label">Monthly average spending</label>
            <div class="input-group mb-3">
              <input id="monthlyInput" type="number" class="form-control" min="0" step="100" value="0" />
            </div>

            <div class="form-check mb-2">
              <input class="form-check-input" type="checkbox" id="noSmoke">
              <label class="form-check-label" for="noSmoke">I don't smoke</label>
            </div>
            <div class="form-check mb-2">
              <input class="form-check-input" type="checkbox" id="noDrink">
              <label class="form-check-label" for="noDrink">I don't drink alcohol</label>
            </div>
            <div class="form-check mb-2">
              <input class="form-check-input" type="checkbox" id="noHouse">
              <label class="form-check-label" for="noHouse">I don't live in my own flat/house</label>
            </div>
            <div class="form-check">
              <input class="form-check-input" type="checkbox" id="noCar">
              <label class="form-check-label" for="noCar">I don't have a car</label>
            </div>

            <div id="removedAlert" class="alert alert-secondary mt-3 mb-0" role="alert">
              Eltávolított eredeti súly összesen: <strong><span id="removedPct">0.0</span>%</strong>
            </div>
          </div>
        </div>
      </div>

      <div class="col-md-6">
        <div class="card mt-md-0 mt-4 shadow-sm spending-table">
          <div class="card-body table-responsive">
            <h2 class="h6 mb-3">Categories and monthly average spending</h2>
            <table class="table table-sm table-striped align-middle">
              <thead>
                <tr>
                  <th>Kategória</th>
                  <th class="text-end">Original weight</th>
                  <th class="text-end">New weight</th>
                  <th class="text-end">Amount</th>
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
        <p class="text-muted small mt-2 mb-0">Note: The currency here is not relevant; the amounts in the table are calculated from the specified monthly budget.</p>
      </div>
    </div>
  </div>

  <script>
    const BASE = [
      { key: "elelmiszer", label: "Groceries", weight: 0.2274 },
      { key: "alkohol", label: "Alcohol", weight: 0.0325 },
      { key: "dohanyaru", label: "Tobacco", weight: 0.0591 },
      { key: "ruhazat", label: "Clothing", weight: 0.0341 },
      { key: "rezsi_lakas", label: "Utilities and housing-related services", weight: 0.1152 },
      { key: "lakber_karb", label: "Furniture and maintenance", weight: 0.0717 },
      { key: "egeszsegugy", label: "Healthcare", weight: 0.0545 },
      { key: "auto_kozlekedes", label: "Transportation - car", weight: 0.0872 },
      { key: "auto_vasarlas", label: "Car depreciation", weight: 0.0437 },
      { key: "egyeb_kozlekedes", label: "Transportation - other", weight: 0.0140 },
      { key: "tavkozles", label: "Mobile & Internet", weight: 0.0422 },
      { key: "oktatas", label: "Education", weight: 0.0222 },
      { key: "szabadido", label: "Hobbies", weight: 0.0734 },
      { key: "vendeglatas", label: "Eating out", weight: 0.0495 },
      { key: "egyeb_szolg", label: "All other services (e.g. insurance, banking)", weight: 0.0733 },
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

