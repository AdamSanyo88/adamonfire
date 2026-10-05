---
layout: page
title: Useful resources
permalink: /resources
---

<style>
/* ==========================================
   ADAM ON FIRE - RESOURCES PAGE
   ========================================== */
.resources-page {
  margin-top: 8px;
}

.resources-header {
  margin: 55px 0 40px;
}

.resources-header .eyebrow {
  margin-bottom: 8px;
  color: #2962ff;
  font-size: 12px;
  font-weight: 700;
  letter-spacing: 2px;
  text-transform: uppercase;
}

.resources-header h1 {
  margin: 0;
  font-size: 48px;
  font-weight: 700;
  letter-spacing: -1.5px;
}

.resources-header p {
  max-width: 650px;
  margin-top: 12px;
  color: #666;
  font-size: 18px;
  line-height: 1.6;
}

.resources-page .section > h4 {
  display: flex;
  align-items: center;
  gap: 16px;
  margin: 20px 0 26px !important;
  color: #0d47a1;
  font-weight: 700;
}

.resources-page .section > h4::after {
  content: "";
  flex: 1;
  height: 1px;
  background: #d6e2f7;
}

.resources-page .card {
  overflow: hidden;
  border-radius: 14px;
  border: 1px solid rgba(255,255,255,.22);
  background: linear-gradient(135deg, #2f6fd0 0%, #4e86dd 100%);
  color: #fff;
  box-shadow: 0 8px 22px rgba(13,71,161,.14);
  transition: transform .22s ease, box-shadow .22s ease, filter .22s ease;
}

.resources-page .card:hover {
  transform: translateY(-4px);
  box-shadow: 0 14px 32px rgba(13,71,161,.22);
  filter: brightness(.98);
}

.resources-page .card-content {
  color: #fff;
}

.resources-page .card-title,
.resources-page .card-title strong,
.resources-page .card-content p {
  color: #fff !important;
}

.resources-page .card-title {
  line-height: 1.3 !important;
}

.resources-page .card-content p {
  color: rgba(255,255,255,.90) !important;
  line-height: 1.65;
}

.resources-page .card-action {
  border-top: 1px solid rgba(255,255,255,.20) !important;
  background: rgba(8,45,104,.14);
  color: #fff !important;
  font-weight: 700;
}

.resources-page .card-action a,
.resources-page > .section .card-action a {
  color: #fff !important;
  text-transform: none !important;
  font-weight: 700;
  letter-spacing: 0;
}

.resources-page .card-image {
  background: #fff;
}

.resources-page .card-image img,
.resources-page .card-image iframe {
  display: block;
}

@media only screen and (max-width: 600px) {
  .resources-header {
    margin: 35px 0 30px;
  }

  .resources-header h1 {
    font-size: 38px;
  }

  .resources-header p {
    font-size: 16px;
  }

  .resources-page .section > h4 {
    font-size: 1.65rem;
  }
}
</style>

<div class="resources-header">
  <div class="eyebrow">Adam on FIRE</div>
  <h1>{{ page.title | escape }}</h1>
  <p>Useful links, calculators, presentations, and videos that can help you plan your own FIRE journey and manage your finances more consciously.</p>
</div>

<div class="container resources-page">


  <!-- TBSZ AND PORTFOLIO -->
  <div class="section">
    <h4 style="margin-bottom: 25px;">TBSZ and portfolio management</h4>

    <div class="row">

      <div class="col s12 m6">
        <a href="tbsz" style="color: inherit;">
          <div class="card hoverable" style="height: 100%;">
            <div class="card-content">
              <span class="card-title">
                <strong>Long-Term Investment Account (TBSZ)</strong>
              </span>
              <p>
                Tax optimization in Hungary – information about the Long-Term
                Investment Account (TBSZ).
              </p>
            </div>
            <div class="card-action">
              Learn more →
            </div>
          </div>
        </a>
      </div>


      <div class="col s12 m6">
        <a href="https://docs.google.com/spreadsheets/d/1bqick4Vy13FZMrMZ44g9YiPqF8-Wobd7CH5pAhfl2Bc/copy"
           target="_blank"
           style="color: inherit;">
          <div class="card hoverable" style="height: 100%;">
            <div class="card-content">
              <span class="card-title">
                <strong>Tracking portfolio performance</strong>
              </span>
              <p>
                A simple, semi-automated Google Sheets spreadsheet
                for tracking your investment portfolio.
              </p>
            </div>
            <div class="card-action">
              Open spreadsheet →
            </div>
          </div>
        </a>
      </div>

    </div>
  </div>


  <!-- CALCULATORS -->
  <div class="section">
    <h4 style="margin-bottom: 25px;">Calculators</h4>

    <div class="row">

      <div class="col s12 m6">
        <a href="net-worth" style="color: inherit;">
          <div class="card hoverable">
            <div class="card-content">
              <span class="card-title">
                <strong>Net worth calculator</strong>
              </span>
              <p>
                How wealthy were you in Hungary in 2025?
                Compare your net worth.
              </p>
            </div>
            <div class="card-action">
              Open calculator →
            </div>
          </div>
        </a>
      </div>


      <div class="col s12 m6">
        <a href="spending" style="color: inherit;">
          <div class="card hoverable">
            <div class="card-content">
              <span class="card-title">
                <strong>Personal spending calculator</strong>
              </span>
              <p>
                See how your spending compares with that of an average Hungarian
                household.
              </p>
            </div>
            <div class="card-action">
              Open calculator →
            </div>
          </div>
        </a>
      </div>


      <div class="col s12 m6">
        <a href="pension" style="color: inherit;">
          <div class="card hoverable">
            <div class="card-content">
              <span class="card-title">
                <strong>State pension calculator</strong>
              </span>
              <p>
                Estimate how much your state pension will be
                under the current rules.
              </p>
            </div>
            <div class="card-action">
              Open calculator →
            </div>
          </div>
        </a>
      </div>


      <div class="col s12 m6">
        <a href="inflation" style="color: inherit;">
          <div class="card hoverable">
            <div class="card-content">
              <span class="card-title">
                <strong>Personal inflation calculator</strong>
              </span>
              <p>
                Calculate your personal inflation rate based on the composition
                of your consumer basket.
              </p>
            </div>
            <div class="card-action">
              Open calculator →
            </div>
          </div>
        </a>
      </div>

    </div>
  </div>


  <!-- PRESENTATIONS AND VIDEOS -->
  <div class="section">
    <h4 style="margin-bottom: 25px;">Presentations and videos</h4>

    <div class="row">

      <!-- Presentation -->
      <div class="col s12 m6">
        <div class="card hoverable">

          <div class="card-image">
            <a href="https://www.slideshare.net/slideshow/hogyan-epits-vagyont-tapasztalatok-egy-15-eves-fire-ut-vegen/276076087"
               target="_blank">
              <img src="images/presentation-1.png"
                   alt="How to build wealth?">
            </a>
          </div>

          <div class="card-content">
            <span class="card-title">
              <strong>How to build wealth?</strong>
            </span>

            <p>
              Lessons from the end of a 15-year FIRE journey.
              Presentation – February 2025
            </p>
          </div>

          <div class="card-action">
            <a href="https://www.slideshare.net/slideshow/hogyan-epits-vagyont-tapasztalatok-egy-15-eves-fire-ut-vegen/276076087"
               target="_blank">
              Open presentation →
            </a>
          </div>

        </div>
      </div>

 <div class="col s12 m6">
        <div class="card hoverable">

          <div class="card-image">
            <a href="https://www.slideshare.net/slideshow/hogyan-gondolkozz-hosszu-tavban-makrogazdasagi-szempontok-az-elmult-70-evben/287004907"
               target="_blank">
              <img src="images/presentation-2.png"
                   alt="How to think long term?">
            </a>
          </div>

          <div class="card-content">
            <span class="card-title">
              <strong>How to think long term?</strong>
            </span>

            <p>
              Macroeconomic perspectives based on 70 years of stock-market events
              Presentation – March 2026
            </p>
          </div>

          <div class="card-action">
            <a href="https://www.slideshare.net/slideshow/hogyan-gondolkozz-hosszu-tavban-makrogazdasagi-szempontok-az-elmult-70-evben/287004907"
               target="_blank">
              Open presentation →
            </a>
          </div>

        </div>
      </div>


      <!-- YouTube video 1 -->
      <div class="col s12 m6">
        <div class="card hoverable">

          <div class="card-image">
            <div style="
              position: relative;
              padding-bottom: 56.25%;
              height: 0;
              overflow: hidden;
            ">
              <iframe
                src="https://www.youtube.com/embed/i6TT_x7nPZ4"
                title="10 misconceptions about the FIRE movement"
                style="
                  position: absolute;
                  top: 0;
                  left: 0;
                  width: 100%;
                  height: 100%;
                  border: 0;
                "
                allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                allowfullscreen>
              </iframe>
            </div>
          </div>

          <div class="card-content">
            <span class="card-title">
              <strong>10 misconceptions about the FIRE movement</strong>
            </span>

            <p>
              The most common misunderstandings and misconceptions
              about the FIRE movement – September 2025.
            </p>
          </div>

        </div>
      </div>


      <!-- YouTube video 2 -->
      <div class="col s12 m6">
        <div class="card hoverable">

          <div class="card-image">
            <div style="
              position: relative;
              padding-bottom: 56.25%;
              height: 0;
              overflow: hidden;
            ">
              <iframe
                src="https://www.youtube.com/embed/M3R2zgmog5U"
                title="What happened in 2025, and what can we expect in 2026?"
                style="
                  position: absolute;
                  top: 0;
                  left: 0;
                  width: 100%;
                  height: 100%;
                  border: 0;
                "
                allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share"
                allowfullscreen>
              </iframe>
            </div>
          </div>

          <div class="card-content">
            <span class="card-title">
              <strong>What happened in 2025, and what can we expect in 2026?</strong>
            </span>

            <p>
              A review of the year and an outlook for the year ahead –
              January 2026.
            </p>
          </div>

        </div>
      </div>

    </div>
  </div>

</div>


