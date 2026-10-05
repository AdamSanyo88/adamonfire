---
layout: page
title: FIRE blog
permalink: /blog
---

<style>
.blog-header { margin: 55px 0 40px; }
.blog-header .eyebrow { font-size: 12px; font-weight: 700; letter-spacing: 2px; text-transform: uppercase; color: #2962ff; margin-bottom: 8px; }
.blog-header h1 { margin: 0; font-size: 48px; font-weight: 700; letter-spacing: -1.5px; }
.blog-header p { margin-top: 12px; max-width: 650px; font-size: 18px; line-height: 1.6; color: #666; }
.blog-section-title { display: flex; align-items: center; gap: 18px; margin: 45px 0 28px; }
.blog-section-title h4 { margin: 0; font-weight: 600; white-space: nowrap; }
.blog-section-title:after { content: ""; flex: 1; height: 1px; background: #e0e0e0; }
.blog-grid { display: flex; flex-wrap: wrap; }
.blog-grid > .col { display: flex; margin-bottom: 24px; }
.blog-grid > .col > a { display: flex; width: 100%; }
.blog-card { width: 100%; height: 100%; display: flex; flex-direction: column; border-radius: 12px; overflow: hidden; background: #4f83d1; color: white; border: 1px solid rgba(255,255,255,.15); box-shadow: 0 3px 12px rgba(0,0,0,.07); transition: transform .22s ease, box-shadow .22s ease, background .22s ease; }
.blog-card:hover { transform: translateY(-5px); box-shadow: 0 12px 30px rgba(13,71,161,.20); background: #4779bf; }
.blog-card .card-content { flex: 1; }
.blog-meta { display: flex; gap: 8px; margin-bottom: 15px; align-items: center; }
.blog-category { display: inline-block; background: rgba(255,255,255,.16); color: white; border: 1px solid rgba(255,255,255,.28); border-radius: 20px; padding: 4px 10px; font-size: 11px; font-weight: 700; letter-spacing: .4px; text-transform: uppercase; }
.blog-date { font-size: 12px; color: rgba(255,255,255,.72); }
.blog-card .card-title { font-size: 23px; line-height: 1.25; margin-bottom: 14px !important; }
.blog-card .card-content p { font-size: 15px; line-height: 1.65; color: rgba(255,255,255,.90); }
.blog-card .card-action { border-top: 1px solid rgba(255,255,255,.20); color: white; font-weight: 600; text-transform: none; letter-spacing: 0; }
.arrow { display: inline-block; transition: transform .2s ease; }
.blog-card:hover .arrow, .blog-featured:hover .arrow { transform: translateX(5px); }
.blog-featured { position: relative; border-radius: 16px; overflow: hidden; background: linear-gradient(135deg,#0d47a1 0%,#2962ff 100%); color: white; padding: 42px 45px; margin-bottom: 45px; box-shadow: 0 15px 40px rgba(41,98,255,.18); transition: transform .22s ease, box-shadow .22s ease; }
.blog-featured:hover { transform: translateY(-3px); box-shadow: 0 20px 45px rgba(41,98,255,.24); }
.blog-featured .featured-label { font-size: 11px; letter-spacing: 2px; text-transform: uppercase; font-weight: 700; opacity: .8; }
.blog-featured h2 { font-size: 35px; font-weight: 700; line-height: 1.15; margin: 12px 0 16px; max-width: 700px; }
.blog-featured p { font-size: 17px; line-height: 1.6; max-width: 700px; opacity: .9; }
.blog-featured .featured-link { display: inline-block; margin-top: 16px; color: white; font-weight: 700; font-size: 15px; }
.blog-featured .featured-hitbox { position: absolute; inset: 0; z-index: 10; display: block; }
.blog-featured > *:not(.featured-hitbox) { position: relative; z-index: 2; }
.blog-featured:after { content: "FIRE"; position: absolute; right: -10px; bottom: -45px; font-size: 150px; font-weight: 900; letter-spacing: -10px; opacity: .055; pointer-events: none; }
@media only screen and (max-width: 600px) { .blog-header { margin: 35px 0 30px; } .blog-header h1 { font-size: 38px; } .blog-header p { font-size: 16px; } .blog-featured { padding: 30px 25px; } .blog-featured h2 { font-size: 28px; } .blog-featured:after { font-size: 100px; } .blog-section-title h4 { font-size: 1.7rem; } }
</style>

<div class="blog-header">
  <div class="eyebrow">Adam on FIRE</div>
  <h1>{{ page.title | escape }}</h1>
  <p>What is my day-to-day life like after reaching FIRE? This is where I share my personal thoughts and experiences.</p>
</div>

<div class="blog-featured">
  <a class="featured-hitbox" href="/blog-15" aria-label="Brain Bar and the bond markets"></a>
  <div class="featured-label">Latest post · Investing · 2026</div>
  <h2>Brain Bar and the bond markets</h2>
  <p>A Brain Bar talk, US bond yields above 5%, and a new portfolio visualization. Thoughts on what a persistently high interest-rate environment could mean for equity and bond markets.</p>
  <div class="featured-link">Read more <span class="arrow">→</span></div>
</div>

<div class="section">
  <div class="blog-section-title"><h4>All previous posts</h4></div> 
  <div class="row blog-grid">
    <div class="col s12 m6">
      <a href="/blog-14" style="color: inherit;">
        <div class="card hoverable blog-card">
          <div class="card-content">
            <div class="blog-meta"><span class="blog-category">Life</span><span class="blog-date">2026</span></div>
            <span class="card-title"><strong>Losing direction and summer depression</strong></span>
            <p>The first months of a three-day workweek did not automatically bring the sense of freedom I had expected. Thoughts on motivation, post-FIRE goals, and my experiences volunteering at Bátor Tábor.</p>
          </div>
          <div class="card-action">Read more <span class="arrow">→</span></div>
        </div>
      </a>
    </div>
    <div class="col s12 m6">
      <a href="/blog-13" style="color: inherit;">
        <div class="card hoverable blog-card">
          <div class="card-content">
            <div class="blog-meta"><span class="blog-category">Travel</span><span class="blog-date">2026</span></div>
            <span class="card-title"><strong>Lessons from my first road trip</strong></span>
            <p>1,700 kilometres in eight days across Austria and Slovenia. What did I learn about travelling by car, prices, and how different a road-trip holiday feels?</p>
          </div>
          <div class="card-action">Read more <span class="arrow">→</span></div>
        </div>
      </a>
    </div>
    <div class="col s12 m6">
      <a href="/blog-12" style="color: inherit;">
        <div class="card hoverable blog-card">
          <div class="card-content">
            <div class="blog-meta"><span class="blog-category">FIRE</span><span class="blog-date">2026</span></div>
            <span class="card-title"><strong>Bátor Tábor, FIRE talks, and the three-day workweek</strong></span>
            <p>FIRE talks, volunteering at Bátor Tábor, and an important decision: from June, I would work three days a week, leaving more time for my own projects and allowing a more gradual transition to full FIRE.</p>
          </div>
          <div class="card-action">Read more <span class="arrow">→</span></div>
        </div>
      </a>
    </div>
    <div class="col s12 m6">
      <a href="/blog-11" style="color: inherit;">
        <div class="card hoverable blog-card">
          <div class="card-content">
            <div class="blog-meta"><span class="blog-category">Politics</span><span class="blog-date">2026</span></div>
            <span class="card-title"><strong>The end of the Orbán system</strong></span>
            <p>141 seats, a two-thirds victory for Tisza, and the end of an era. A personal look back at the election, the forint’s reaction, and behind the scenes of an election-integrity simulation.</p>
          </div>
          <div class="card-action">Read more <span class="arrow">→</span></div>
        </div>
      </a>
    </div>
    <div class="col s12 m6">
      <a href="/blog-10" style="color: inherit;">
        <div class="card hoverable blog-card">
          <div class="card-content">
            <div class="blog-meta"><span class="blog-category">Politics</span><span class="blog-date">2026</span></div>
            <span class="card-title"><strong>Election season has begun</strong></span>
            <p>Only 24 days remain until the Hungarian election. Expectations for the final stretch of the campaign, political talks, and how the result could shape the coming years.</p>
          </div>
          <div class="card-action">Read more <span class="arrow">→</span></div>
        </div>
      </a>
    </div>
    <div class="col s12 m6">
      <a href="/blog-9" style="color: inherit;">
        <div class="card hoverable blog-card">
          <div class="card-content">
            <div class="blog-meta"><span class="blog-category">TBSZ</span><span class="blog-date">2026</span></div>
            <span class="card-title"><strong>Lessons from my first TBSZ transfer</strong></span>
            <p>After six years of investing, I had to transfer a maturing TBSZ account for the first time. Transaction fees, several days of reinvesting, and a few lessons for the next transfer.</p>
          </div>
          <div class="card-action">Read more <span class="arrow">→</span></div>
        </div>
      </a>
    </div>
    <div class="col s12 m6">
      <a href="/blog-8" style="color: inherit;">
        <div class="card hoverable blog-card">
          <div class="card-content">
            <div class="blog-meta"><span class="blog-category">FIRE</span><span class="blog-date">2026</span></div>
            <span class="card-title"><strong>Looking back at 2025 and ahead to 2026</strong></span>
            <p>By the end of 2025, I had reached my FIRE target of roughly €600,000 in net worth. A look back at the portfolio and what comes next in 2026.</p>
          </div>
          <div class="card-action">Read more <span class="arrow">→</span></div>
        </div>
      </a>
    </div>
    <div class="col s12 m6">
      <a href="/blog-7" style="color: inherit;">
        <div class="card hoverable blog-card">
          <div class="card-content">
            <div class="blog-meta"><span class="blog-category">Travel</span><span class="blog-date">2025</span></div>
            <span class="card-title"><strong>Japan is the best holiday destination</strong></span>
            <p>Many people still think of Japan as an expensive destination, yet the weak yen and relatively low prices can make a trip surprisingly affordable.</p>
          </div>
          <div class="card-action">Read more <span class="arrow">→</span></div>
        </div>
      </a>
    </div>
    <div class="col s12 m6">
      <a href="/blog-6" style="color: inherit;">
        <div class="card hoverable blog-card">
          <div class="card-content">
            <div class="blog-meta"><span class="blog-category">FIRE</span><span class="blog-date">2025</span></div>
            <span class="card-title"><strong>I got the green light for FIRE</strong></span>
            <p>How can you actually assess whether a portfolio is ready for FIRE, and how do you know that everything is truly in place financially?</p>
          </div>
          <div class="card-action">Read more <span class="arrow">→</span></div>
        </div>
      </a>
    </div>
    <div class="col s12 m6">
      <a href="/blog-5" style="color: inherit;">
        <div class="card hoverable blog-card">
          <div class="card-content">
            <div class="blog-meta"><span class="blog-category">Data</span><span class="blog-date">2025</span></div>
            <span class="card-title"><strong>New visualizations</strong></span>
            <p>Transparency is one of the most important parts of my FIRE journey: I use detailed data and visualizations to show how my portfolio and net worth change over time.</p>
          </div>
          <div class="card-action">Read more <span class="arrow">→</span></div>
        </div>
      </a>
    </div>
    <div class="col s12 m6">
      <a href="/blog-4" style="color: inherit;">
        <div class="card hoverable blog-card">
          <div class="card-content">
            <div class="blog-meta"><span class="blog-category">Life</span><span class="blog-date">2025</span></div>
            <span class="card-title"><strong>I started playing basketball</strong></span>
            <p>After a longer illness, I was looking for a new hobby and ended up choosing basketball. A more personal post about life beyond FIRE.</p>
          </div>
          <div class="card-action">Read more <span class="arrow">→</span></div>
        </div>
      </a>
    </div>
    <div class="col s12 m6">
      <a href="/blog-3" style="color: inherit;">
        <div class="card hoverable blog-card">
          <div class="card-content">
            <div class="blog-meta"><span class="blog-category">Investing</span><span class="blog-date">2025</span></div>
            <span class="card-title"><strong>I built my portfolio tracker</strong></span>
            <p>Market turbulence showed me that I needed a better system for tracking my portfolio. That is how my own portfolio tracker was born.</p>
          </div>
          <div class="card-action">Read more <span class="arrow">→</span></div>
        </div>
      </a>
    </div>
    <div class="col s12 m6">
      <a href="/blog-2" style="color: inherit;">
        <div class="card hoverable blog-card">
          <div class="card-content">
            <div class="blog-meta"><span class="blog-category">Investing</span><span class="blog-date">2025</span></div>
            <span class="card-title"><strong>Trump made me panic too</strong></span>
            <p>The sudden market drop tested me too. What did I do with my portfolio, how did I react, and what did I learn from my own behaviour?</p>
          </div>
          <div class="card-action">Read more <span class="arrow">→</span></div>
        </div>
      </a>
    </div>
    <div class="col s12 m6">
      <a href="/blog-1" style="color: inherit;">
        <div class="card hoverable blog-card">
          <div class="card-content">
            <div class="blog-meta"><span class="blog-category">FIRE</span><span class="blog-date">2025</span></div>
            <span class="card-title"><strong>550 days to go</strong></span>
            <p>According to my calculations, I could reach my FIRE target by mid-2026. But what does that mean when equity markets are trading at historic highs?</p>
          </div>
          <div class="card-action">Read more <span class="arrow">→</span></div>
        </div>
      </a>
    </div>
  </div>
</div>
