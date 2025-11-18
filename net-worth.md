<script>
// ---------- Percentile arrays ----------
// EUR (számításokhoz)
const PCT_EUR = [
  3160000,997801,751807,601008,522823,455581,412012,369040,339199,331937,311980,297613,283245,268878,254510,248353,239732,231112,222491,213871,205250,196630,188009,179389,170768,165021,160506,155990,151475,146959,142444,137928,133413,128897,124382,123150,119866,116582,113298,110014,106730,103446,100162,96878,93594,92116,89817,87519,85220,82921,80622,78323,76025,73726,71427,70606,68554,66501,64449,62396,60344,58291,56239,54186,52134,51313,49260,47208,45155,43103,41050,38998,36945,34893,32840,31609,29967,28325,26683,25041,23399,21757,20115,18473,16831,16215,14778,13341,11905,10468,9031,7594,6158,4721,3284,2627,1970,1314,657,0
];

// HUF (grafikonhoz, 100M tickekkel)
const PCT_HUF = [
  1264000000,399120400,300722800,240403200,209129200,182232400,164804800,147616000,135679600,132774800,
  124792000,119045200,113298000,107551200,101804000,99341200,95892800,92444800,88996400,85548400,
  82100000,78652000,75203600,71755600,68307200,66008400,64202400,62396000,60590000,58783600,
  56977600,55171200,53365200,51558800,49752800,49260000,47946400,46632800,45319200,44005600,
  42692000,41378400,40064800,38751200,37437600,36846400,35926800,35007600,34088000,33168400,
  32248800,31329200,30410000,29490400,28570800,28242400,27421600,26600400,25779600,24958400,
  24137600,23316400,22495600,21674400,20853600,20525200,19704000,18883200,18062000,17241200,
  16420000,15599200,14778000,13957200,13136000,12643600,11986800,11330000,10673200,10016400,
  9359600,8702800,8046000,7389200,6732400,6486000,5911200,5336400,4762000,4187200,
  3612400,3037600,2463200,1888400,1313600,1050800,788000,525600,262800,0
];

// Labels: 100 → 1
const LABELS = Array.from({length:100},(_,i)=>String(100-i));

let chart;

// --- Biztonságos Chart.js betöltés (ha még nincs) ---
function ensureChartJs(ready){
  if (window.Chart) return ready();
  const s = document.createElement('script');
  s.src = 'https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js';
  s.async = true;
  s.onload = () => ready();
  s.onerror = () => console.error('Chart.js betöltése nem sikerült.');
  document.head.appendChild(s);
}

// --- Várunk, míg a canvas tényleg megjelenik és kap magasságot ---
function whenCanvasReady(cb){
  const el = document.getElementById('percentileChart');
  if (!el) { requestAnimationFrame(()=>whenCanvasReady(cb)); return; }
  // Ha zero-height, várjunk egy kört (Chrome néha késleltet)
  if ((el.offsetHeight||0) < 10 || (el.offsetWidth||0) < 10){
    requestAnimationFrame(()=>whenCanvasReady(cb));
    return;
  }
  cb(el);
}

// --- Grafikon inicializálása HUF tengellyel (100M lépcsők, 0..1500M) ---
function initChart(){
  if (chart) { chart.destroy(); chart = null; }
  const ctx = document.getElementById('percentileChart');
  chart = new Chart(ctx,{
    type:'bar',
    data:{
      labels: LABELS,
      datasets:[{
        data: [...PCT_HUF].reverse(), // bal: 100%, jobb: Top 1%
        backgroundColor: '#cfd8dc',
        borderWidth: 0
      }]
    },
    options:{
      responsive:true,
      maintainAspectRatio:false,
      plugins:{
        legend:{display:false},
        tooltip:{
          callbacks:{
            title: (items)=> `Percentile ${items[0].label}`,
            label: (item)=> `Threshold ${Number(item.raw).toLocaleString('hu-HU')} Ft`
          }
        }
      },
      scales:{
        x:{ grid:{display:false}, ticks:{autoSkip:true, maxRotation:0} },
        y:{
          beginAtZero:true, min:0, max:1_500_000_000,
          ticks:{
            stepSize:100_000_000,
            callback:(v)=> v===0 ? '' : `${Math.round(v/1_000_000)}M`
          },
          grid:{ drawTicks:false }
        }
      }
    }
  });
}

// --- Kiemelés (Top p%) ---
function highlight(p){
  if(!chart) return;
  const idx = 100 - p;
  const colors = new Array(100).fill('#cfd8dc');
  if (idx >= 0 && idx < colors.length) colors[idx] = '#0d6efd';
  chart.data.datasets[0].backgroundColor = colors;
  chart.update();
}

// --- Táblázat logika (EUR) változatlanul ---
function addProperty(){
  const anchor=document.getElementById('prop-anchor');
  const tr=document.createElement('tr'); tr.dataset.type='prop';
  tr.innerHTML=`<td>🏠</td><td>Real estate</td>
  <td><input type="number" data-field="value" value="0"></td>
  <td><input type="number" data-field="debt" value="0"></td>
  <td class="text-end mono" data-cell="net">€0</td>
  <td><button class="btn btn-sm btn-outline-danger" type="button">✖</button></td>`;
  anchor.parentNode.insertBefore(tr,anchor);
}

function compute(){
  let sumV=0,sumD=0,sumN=0;
  document.querySelectorAll('#rows tr[data-type]').forEach(r=>{
    const t=r.dataset.type;
    const v=+r.querySelector('[data-field="value"]')?.value||0;
    const d=+r.querySelector('[data-field="debt"]')?.value||0;
    let n=0;
    if(t==='prop') n=v-d;
    else if(t==='inv' || t==='asset') n=v;
    else if(t==='liab') n=-d;
    sumV+=v; sumD+=d; sumN+=n;
    r.querySelector('[data-cell="net"]').textContent='€'+n.toLocaleString();
  });
  document.getElementById('sum-value').textContent='€'+sumV.toLocaleString();
  document.getElementById('sum-debt').textContent='€'+sumD.toLocaleString();
  document.getElementById('sum-net').textContent='€'+sumN.toLocaleString();
  document.getElementById('nw-eur').textContent='€'+sumN.toLocaleString();

  // Percentilis EUR küszöbök alapján
  let pct=100; for(let i=0;i<PCT_EUR.length;i++){ if(sumN>=PCT_EUR[i]){ pct=i+1; break; } }
  document.getElementById('pct-chip').textContent='Top '+pct+'%';
  document.getElementById('pct-text').textContent=`Your estimated net worth is €${sumN.toLocaleString()}, which places you ${100-pct}% higher than others (Top ${pct}%).`;
  highlight(pct);
}

// --- Biztonságos indulás Chrome-hoz is ---
document.addEventListener('DOMContentLoaded', ()=>{
  ensureChartJs(()=> whenCanvasReady(()=>{
    initChart();               // csak akkor, ha Chart.js megvan és a canvas tényleg látható
    addProperty();
    compute();
  }));

  // élő frissítés
  document.body.addEventListener('input', e=>{ if(e.target.matches('input')) compute(); });
  document.getElementById('add-prop').addEventListener('click', ()=>{ addProperty(); });
  document.getElementById('rows').addEventListener('click', e=>{
    if(e.target.tagName==='BUTTON' && e.target.closest('tr')?.dataset.type==='prop'){
      e.target.closest('tr').remove(); compute();
    }
  });

  // Ha a konténer mérete később változik (pl. betűtípus/layoutról), frissítsük a chartot
  const wrap = document.querySelector('.chart-wrap');
  if (wrap && 'ResizeObserver' in window){
    new ResizeObserver(()=>{ if(chart) chart.resize(); }).observe(wrap);
  }
});
</script>
