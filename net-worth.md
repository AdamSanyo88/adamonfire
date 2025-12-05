<script>
(function(){
  // === Percentilis küszöbök Ft-ban (1..100; 1 = leggazdagabb, 100 = legszegényebb) ===
  const PCT_DATA = {
    "2014": [
      1071200, 312800, 223200, 182300, 159300, 138000, 124000, 114200, 104600, 100300,
      94100, 88400, 83000, 78000, 73200, 68800, 66100, 63600, 61100, 58800,
      56500, 54400, 52300, 50300, 48400, 46500, 44900, 43400, 42000, 40500,
      39200, 37900, 36600, 35300, 34200, 33000, 32100, 31200, 30300, 29400,
      28600, 27800, 27000, 26200, 25500, 24800, 24100, 23400, 22800, 22100,
      21500, 21000, 20400, 19800, 19300, 18800, 18100, 17600, 17000, 16400,
      15900, 15400, 14900, 14400, 14000, 13500, 12900, 12300, 11800, 11200,
      10700, 10200, 9800, 9300, 8900, 8500, 7900, 7300, 6800, 6300,
      5800, 5400, 5000, 4700, 4300, 4000, 3000, 2300, 1700, 1300,
      1000, 800, 600, 400, 300, 300, 200, 100, 100, 0
    ],
    "2017": [
      1618000, 478300, 343000, 260000, 209800, 183500, 160800, 146100, 139100, 124500,
      118200, 112300, 106600, 101200, 96100, 91300, 87600, 84200, 80900, 77700,
      74600, 71700, 68800, 66100, 63500, 61000, 59300, 57600, 56000, 54400,
      52800, 51300, 49900, 48500, 47100, 45800, 44600, 43400, 42300, 41200,
      40200, 39100, 38100, 37100, 36200, 35300, 34300, 33300, 32400, 31400,
      30600, 29700, 28900, 28100, 27300, 26500, 25700, 24900, 24100, 23300,
      22600, 21900, 21200, 20500, 19900, 19300, 18400, 17700, 16900, 16200,
      15500, 14900, 14200, 13600, 13100, 12500, 11500, 10600, 9800, 9000,
      8300, 7600, 7000, 6500, 6000, 5500, 4600, 3900, 3300, 2800,
      2300, 2000, 1700, 1400, 1200, 1000, 800, 500, 300, 0
    ],
    "2020": [
      1918100, 774900, 555700, 421200, 339900, 297300, 260600, 236600, 225300, 201500,
      193600, 185400, 177500, 169900, 162700, 155800, 149600, 143700, 138000, 132500,
      127300, 122200, 117400, 112700, 108300, 104000, 100800, 97600, 94600, 91600,
      88800, 86000, 83300, 80700, 78200, 75800, 73600, 71600, 69600, 67600,
      65700, 63900, 62100, 60300, 58600, 57000, 55500, 54000, 52600, 51200,
      49800, 48500, 47200, 45900, 44700, 43500, 42200, 40800, 39600, 38400,
      37200, 36000, 34900, 33800, 32800, 31800, 30300, 28900, 27600, 26400,
      25200, 24100, 23000, 21900, 20900, 20000, 18700, 17500, 16400, 15300,
      14300, 13400, 12500, 11700, 11000, 10300, 8700, 7400, 6300, 5300,
      4500, 3800, 3300, 2800, 2400, 2000, 1500, 1000, 500, 0
    ],
    "2023": [
      2958100, 1195100, 857000, 649600, 524100, 458400, 401800, 364900, 347500, 310700,
      298600, 285900, 273700, 262100, 250900, 240200, 230700, 221600, 212800, 204400,
      197800, 190000, 182500, 175200, 168300, 161600, 156600, 151700, 147000, 142400,
      140200, 135900, 131600, 127500, 123500, 119700, 116300, 113100, 109900, 106800,
      104500, 101500, 98700, 95900, 93200, 90600, 88200, 85800, 83600, 81300,
      78600, 76500, 74500, 72500, 70500, 68700, 66500, 64500, 62500, 60500,
      59200, 57400, 55600, 53900, 52200, 50600, 48300, 46100, 44000, 42100,
      41100, 39300, 37500, 35800, 34200, 32700, 30500, 28600, 26700, 25000,
      21500, 20100, 18800, 17600, 16400, 15400, 13100, 11100, 9400, 8000,
      6800, 5800, 4900, 4200, 3500, 3000, 2300, 1500, 700, 0
    ],
    "2025": [
      4437100, 1732900, 1242600, 941900, 760000, 664700, 582700, 529100, 503900, 463000,
      444900, 426000, 407800, 390500, 373800, 357900, 343700, 330100, 317100, 304500,
      294700, 283100, 271900, 261100, 250800, 240800, 233300, 226000, 219000, 212200,
      210300, 203800, 197400, 191300, 185300, 179500, 174500, 169600, 164800, 160200,
      156700, 152300, 148000, 143900, 139800, 135900, 132300, 128800, 125300, 122000,
      117900, 114800, 111700, 108700, 105800, 103000, 99800, 96700, 93700, 90800,
      88800, 86100, 83400, 80800, 78300, 75900, 72500, 69200, 66100, 63100,
      61700, 58900, 56300, 53700, 51300, 45700, 42800, 40000, 37400, 35000,
      30100, 28100, 26300, 24600, 23000, 21500, 18300, 15500, 13200, 11200,
      9500, 8100, 6900, 5800, 4900, 4200, 3200, 2000, 900, 0
    ]
  };

  const LABELS = Array.from({length:100},(_,i)=>String(100-i)); // 100 -> 1
  const CURRENCY = "Ft";
  const LOCALE = "hu-HU";

  function fmtFt(n){
    return `${CURRENCY} ${Number(n||0).toLocaleString(LOCALE)}`;
  }

  // Alapértelmezett év: 2025
  let currentYear = "2025";
  let PCT_THRESHOLDS = PCT_DATA[currentYear].slice();

  let chart;

  function getChartBars(){
    // PCT_THRESHOLDS: 1..100 (gazdag->szegény), grafikonon: 1..100 (szegény->gazdag)
    return [...PCT_THRESHOLDS].reverse();
  }

  function initChart(){
    const canvas = document.getElementById("percentileChart");
    if(!canvas || !window.Chart || !PCT_THRESHOLDS.length) return;
    const ctx = canvas.getContext('2d');

    chart = new Chart(ctx,{
      type:"bar",
      data:{
        labels: LABELS,
        datasets:[{
          data: getChartBars(),
          backgroundColor: new Array(100).fill("#cfd8dc"),
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
              title: items => items && items[0] ? ("Percentilis "+items[0].label) : "",
              label:  item  => "Medián vagyon " + fmtFt(item.raw)
            }
          }
        },
        scales:{
          x:{grid:{display:false},ticks:{autoSkip:true,maxRotation:0}},
          y: {
  beginAtZero: true,
  max: 5000000,
  suggestedMax: 5000000,
  ticks: {
    stepSize: 250000,
    callback: (value) => {
      const n = Number(value);
      if (isNaN(n)) return "";
      if (n === 0) return "0";
      return (n / 1_000_000).toFixed(1).replace(".0","") + "M";
    }
  }
}
      }
    });
  }

  function updateChartScale(year){
    if(!PCT_DATA[year] || !PCT_DATA[year].length || !chart) return;
    currentYear = year;
    PCT_THRESHOLDS = PCT_DATA[year].slice();

    chart.data.datasets[0].data = getChartBars();

    // Y-skála minden évre fix: 0–5M, 250k lépés
    chart.options.scales.y.max = 5000000;
    chart.options.scales.y.suggestedMax = 5000000;
    chart.options.scales.y.ticks.stepSize = 250000;

    chart.data.datasets[0].backgroundColor = new Array(100).fill("#cfd8dc");
    chart.update();

    compute(); // új skálához újraszámoljuk a percentilist
  }

  function highlight(p){
    if(!chart) return;
    const idx = 100 - p; // 1-es percentilis a jobb szél
    const colors = new Array(100).fill("#cfd8dc");
    if (idx>=0 && idx<colors.length) colors[idx] = "#0d6efd";
    chart.data.datasets[0].backgroundColor = colors;
    chart.update();
  }

  function addProperty(){
    const anchor=document.getElementById("prop-anchor");
    const tr=document.createElement("tr"); tr.dataset.type="prop";
    tr.innerHTML=`<td>🏠</td><td>Ingatlan</td>
      <td><input type="number" data-field="value" value="0"></td>
      <td><input type="number" data-field="debt" value="0"></td>
      <td class="text-end mono" data-cell="net">Ft 0</td>
      <td><button class="btn btn-sm btn-outline-danger" type="button">✖</button></td>`;
    anchor.parentNode.insertBefore(tr,anchor);
  }

  function compute(){
    let sumV=0,sumD=0,sumN=0;
    document.querySelectorAll("#rows tr[data-type]").forEach(r=>{
      const t=r.dataset.type;
      const v=+r.querySelector('[data-field="value"]')?.value||0;
      const d=+r.querySelector('[data-field="debt"]')?.value||0;
      let n=0;
      if(t==="prop") n=v-d;
      else if(t==="inv"||t==="asset") n=v;
      else if(t==="liab") n=-d;
      sumV+=v; sumD+=d; sumN+=n;
      r.querySelector('[data-cell="net"]').textContent = fmtFt(n);
    });

    document.getElementById("sum-value").textContent = fmtFt(sumV);
    document.getElementById("sum-debt").textContent  = fmtFt(sumD);
    document.getElementById("sum-net").textContent   = fmtFt(sumN);
    document.getElementById("nw-ft").textContent     = fmtFt(sumN);

    if (!PCT_THRESHOLDS.length){
      document.getElementById("pct-chip").textContent = "Percentilis: –";
      document.getElementById("pct-text").textContent = "Hiányzó percentilis adatok.";
      return;
    }

    // Percentilis (1..100) az aktuális év skáláján
    let pct=100;
    for(let i=0;i<PCT_THRESHOLDS.length;i++){
      if(sumN>=PCT_THRESHOLDS[i]){ pct=i+1; break; }
    }

    document.getElementById("pct-chip").textContent = "Top "+pct+"% ("+currentYear+")";
    document.getElementById("pct-text").textContent =
      `A nettó becsült vagyonod ${fmtFt(sumN)}, amivel a háztartások ${100-pct}%-ánál vagyonosabb vagy (Top ${pct}%, ${currentYear}-es skála).`;

    highlight(pct);
  }

  function initAll(){
    addProperty();
    initChart();
    compute();
  }

  if (document.readyState === "complete" || document.readyState === "interactive") {
    initAll();
  } else {
    document.addEventListener("DOMContentLoaded", initAll);
  }

  // Események
  document.body.addEventListener("input",e=>{ if(e.target.matches("input")) compute(); });
  document.addEventListener("click",e=>{
    if(e.target.id==="add-prop"){ addProperty(); }
    if(e.target.tagName==="BUTTON" && e.target.closest("tr")?.dataset.type==="prop"){
      e.target.closest("tr").remove(); compute();
    }
  });

  document.addEventListener("change", e => {
    if (e.target && e.target.id === "scale-year") {
      updateChartScale(e.target.value);
    }
  });
})();
</script>
