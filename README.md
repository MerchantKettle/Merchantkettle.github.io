<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8"/>
<meta name="viewport" content="width=device-width,initial-scale=1.0"/>
<title>The Merchant — Game Intelligence Hub</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;600;700;900&family=Crimson+Pro:ital,wght@0,300;0,400;0,600;1,300;1,400&display=swap" rel="stylesheet">
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
:root{
  --bg:#060509;--bg2:#0a090e;--surface:#0f0d16;--surface2:#151220;
  --border:rgba(180,150,100,0.12);--border2:rgba(180,150,100,0.22);
  --gold:#c9a84c;--gold-light:#e8c87a;--gold-dim:rgba(201,168,76,0.15);
  --text:#e2ddd4;--muted:#7a7265;
  --mcgg:#f5a623;--mlbb:#e84545;--hsr:#7b9cef;
  --win:#4caf7a;--loss:#e84545;--pending:#8a8070;
}
html{scroll-behavior:smooth}
body{background:var(--bg);color:var(--text);font-family:'Crimson Pro',Georgia,serif;font-size:16px;line-height:1.65;overflow-x:hidden}
#stars-canvas{position:fixed;inset:0;z-index:0;pointer-events:none;opacity:.45}
body::before{content:'';position:fixed;inset:0;background-image:url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='1'/%3E%3C/svg%3E");opacity:.028;pointer-events:none;z-index:1}
.wrap{position:relative;z-index:2}

/* HEADER */
header{position:sticky;top:0;z-index:200;background:rgba(6,5,9,.93);backdrop-filter:blur(18px);border-bottom:1px solid var(--border2);padding:0 5vw;display:flex;align-items:center;justify-content:space-between;height:62px}
.logo{font-family:'Cinzel',serif;font-weight:900;font-size:1.2rem;letter-spacing:.12em;color:var(--gold);text-transform:uppercase}
.logo small{font-family:'Crimson Pro',serif;font-size:.68rem;font-weight:300;letter-spacing:.25em;color:var(--muted);display:block;margin-top:-2px;text-transform:uppercase}
nav{display:flex;gap:4px}
nav a{font-family:'Cinzel',serif;font-size:.64rem;font-weight:600;letter-spacing:.12em;text-transform:uppercase;color:var(--muted);text-decoration:none;padding:6px 13px;border-radius:4px;transition:color .2s,background .2s}
nav a:hover{color:var(--gold);background:var(--gold-dim)}

/* STATUS BAR */
.status-bar{background:var(--bg2);border-bottom:1px solid var(--border);padding:7px 5vw;display:flex;align-items:center;gap:12px;flex-wrap:wrap}
.live-dot{width:7px;height:7px;border-radius:50%;background:#4caf7a;box-shadow:0 0 8px #4caf7a;flex-shrink:0;animation:pulse 1.6s ease-in-out infinite}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:.2}}
.status-item{font-size:.75rem;color:var(--muted);display:flex;align-items:center;gap:6px}
.status-item strong{color:var(--gold-light)}
.status-divider{color:var(--border2);font-size:.7rem}
.refresh-btn{font-family:'Cinzel',serif;font-size:.58rem;font-weight:700;letter-spacing:.1em;text-transform:uppercase;background:var(--gold-dim);border:1px solid rgba(201,168,76,.3);color:var(--gold);padding:3px 11px;border-radius:2px;cursor:pointer;margin-left:auto;transition:background .2s}
.refresh-btn:hover{background:rgba(201,168,76,.25)}
.spin{animation:spin .6s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}

/* TICKER */
.ticker-wrap{background:var(--surface);border-top:1px solid var(--border);border-bottom:1px solid var(--border);overflow:hidden;white-space:nowrap;padding:9px 0}
.ticker-inner{display:inline-block;animation:ticker 60s linear infinite}
.ticker-inner:hover{animation-play-state:paused}
.ticker-item{display:inline-block;padding:0 36px;font-family:'Cinzel',serif;font-size:.68rem;letter-spacing:.07em;color:var(--muted)}
.t-game{margin-right:8px;font-weight:700}
.t-mcgg{color:var(--mcgg)}.t-mlbb{color:var(--mlbb)}.t-hsr{color:var(--hsr)}
.ticker-sep{color:var(--gold);margin:0 18px;opacity:.35}
@keyframes ticker{0%{transform:translateX(0)}100%{transform:translateX(-50%)}}

/* HERO */
.hero{padding:80px 5vw 50px;text-align:center;position:relative}
.hero::after{content:'';position:absolute;bottom:0;left:50%;transform:translateX(-50%);width:60%;height:1px;background:linear-gradient(to right,transparent,var(--gold),transparent);opacity:.3}
.hero-eyebrow{font-family:'Cinzel',serif;font-size:.62rem;letter-spacing:.3em;text-transform:uppercase;color:var(--muted);margin-bottom:20px}
.hero h1{font-family:'Cinzel',serif;font-weight:900;font-size:clamp(2rem,6vw,4.5rem);line-height:1.05;color:#fff;text-shadow:0 0 60px rgba(201,168,76,.22);margin-bottom:6px}
.hero h1 span{color:var(--gold)}
.hero-sub{font-size:1.05rem;font-style:italic;color:var(--muted);margin:12px auto 32px;max-width:420px;font-weight:300}
.hero-games{display:flex;justify-content:center;gap:12px;flex-wrap:wrap}
.hero-pill{font-family:'Cinzel',serif;font-size:.64rem;font-weight:700;letter-spacing:.1em;text-transform:uppercase;padding:7px 18px;border-radius:2px;border:1px solid}
.pill-mcgg{color:var(--mcgg);border-color:var(--mcgg);background:rgba(245,166,35,.07)}
.pill-mlbb{color:var(--mlbb);border-color:var(--mlbb);background:rgba(232,69,69,.07)}
.pill-hsr{color:var(--hsr);border-color:var(--hsr);background:rgba(123,156,239,.07)}

/* SECTION */
.section{padding:52px 5vw;position:relative}
.section+.section{padding-top:0}
.section-divider{border:none;height:1px;margin:0 5vw;background:linear-gradient(to right,transparent,var(--border2),transparent)}
.sec-header{display:flex;align-items:center;gap:14px;margin-bottom:26px}
.sec-badge{font-family:'Cinzel',serif;font-size:.58rem;font-weight:700;letter-spacing:.15em;text-transform:uppercase;padding:3px 10px;border-radius:2px}
.badge-mcgg{background:var(--mcgg);color:#000}.badge-mlbb{background:var(--mlbb);color:#fff}.badge-hsr{background:var(--hsr);color:#fff}
.sec-header h2{font-family:'Cinzel',serif;font-weight:700;font-size:1.12rem;color:#fff;letter-spacing:.04em}
.sec-patch{font-size:.8rem;color:var(--muted);margin-left:auto;font-style:italic}

/* FILTER */
.filter-bar{display:flex;gap:6px;flex-wrap:wrap;margin-bottom:20px}
.fbtn{font-family:'Cinzel',serif;font-size:.59rem;font-weight:600;letter-spacing:.1em;text-transform:uppercase;background:transparent;border:1px solid var(--border2);color:var(--muted);padding:5px 13px;border-radius:2px;cursor:pointer;transition:all .2s}
.fbtn.on,.fbtn:hover{color:var(--gold);border-color:var(--gold);background:var(--gold-dim)}

/* CARDS */
.cards{display:grid;grid-template-columns:repeat(auto-fill,minmax(285px,1fr));gap:13px}
.card{background:var(--surface);border:1px solid var(--border);border-radius:3px;padding:17px 19px;position:relative;overflow:hidden;transition:transform .22s,box-shadow .22s;animation:fadeUp .4s ease both}
.card:nth-child(1){animation-delay:.04s}.card:nth-child(2){animation-delay:.08s}.card:nth-child(3){animation-delay:.12s}
.card:nth-child(4){animation-delay:.16s}.card:nth-child(5){animation-delay:.20s}.card:nth-child(6){animation-delay:.24s}
@keyframes fadeUp{from{opacity:0;transform:translateY(14px)}to{opacity:1;transform:none}}
.card:hover{transform:translateY(-2px);box-shadow:0 10px 40px rgba(0,0,0,.5)}
.card::before{content:'';position:absolute;left:0;top:0;bottom:0;width:3px;border-radius:3px 0 0 3px}
.card-mcgg::before{background:var(--mcgg);box-shadow:0 0 10px rgba(245,166,35,.45)}
.card-mlbb::before{background:var(--mlbb);box-shadow:0 0 10px rgba(232,69,69,.45)}
.card-hsr::before{background:var(--hsr);box-shadow:0 0 10px rgba(123,156,239,.45)}
.card-top{display:flex;align-items:flex-start;justify-content:space-between;margin-bottom:8px}
.ctag{font-family:'Cinzel',serif;font-size:.57rem;font-weight:700;letter-spacing:.1em;text-transform:uppercase;padding:2px 7px;border-radius:2px}
.tag-update{background:rgba(123,156,239,.15);color:#a0baf0}
.tag-new{background:rgba(76,175,122,.15);color:#7dcca0}
.tag-tourney{background:rgba(201,168,76,.15);color:var(--gold)}
.tag-season{background:rgba(232,69,69,.15);color:#f09090}
.tag-collab{background:rgba(160,100,220,.15);color:#c4a0e8}
.tag-patch{background:rgba(80,180,200,.15);color:#88d4e0}
.cdate{font-size:.73rem;color:var(--muted);font-style:italic}
.card h3{font-family:'Cinzel',serif;font-weight:600;font-size:.88rem;color:#fff;margin-bottom:6px;line-height:1.35;letter-spacing:.02em}
.card p{font-size:.82rem;color:#857e70;line-height:1.52}
.card-footer{display:flex;align-items:center;justify-content:space-between;margin-top:13px;padding-top:11px;border-top:1px solid var(--border)}
.csource{font-size:.68rem;color:var(--muted);font-style:italic}
.clink{font-family:'Cinzel',serif;font-size:.6rem;font-weight:700;letter-spacing:.08em;text-decoration:none;opacity:.6;transition:opacity .2s;display:flex;align-items:center;gap:3px}
.clink:hover{opacity:1}
.card-mcgg .clink{color:var(--mcgg)}.card-mlbb .clink{color:var(--mlbb)}.card-hsr .clink{color:var(--hsr)}

/* NEWS FEED CARDS */
.news-card{background:var(--surface);border:1px solid var(--border);border-radius:3px;padding:14px 17px;position:relative;overflow:hidden;transition:transform .2s}
.news-card:hover{transform:translateY(-2px)}
.news-card::before{content:'';position:absolute;left:0;top:0;bottom:0;width:2px}
.news-card-mcgg::before{background:var(--mcgg)}
.news-card-mlbb::before{background:var(--mlbb)}
.news-card-hsr::before{background:var(--hsr)}
.news-card h4{font-family:'Cinzel',serif;font-size:.82rem;font-weight:600;color:#d8d0c0;margin-bottom:5px;line-height:1.3;letter-spacing:.015em}
.news-card p{font-size:.76rem;color:#6a6358;line-height:1.45;margin-bottom:8px}
.news-card-footer{display:flex;align-items:center;justify-content:space-between}
.news-source{font-size:.66rem;color:var(--muted);font-style:italic}
.news-date{font-size:.66rem;color:var(--muted)}
.news-loading{text-align:center;padding:28px;color:var(--muted);font-style:italic;font-size:.85rem;grid-column:1/-1}
.news-error{text-align:center;padding:20px;color:var(--loss);font-size:.8rem;grid-column:1/-1;font-style:italic}

/* HSR TIMERS */
.timer-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(265px,1fr));gap:13px;margin-bottom:22px}
.timer-card{background:var(--surface);border:1px solid var(--border);border-radius:3px;padding:16px 18px;position:relative;overflow:hidden}
.timer-card::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;background:linear-gradient(to right,var(--hsr),rgba(123,156,239,.15))}
.timer-label-top{font-family:'Cinzel',serif;font-size:.64rem;font-weight:700;letter-spacing:.12em;text-transform:uppercase;color:var(--hsr);margin-bottom:3px}
.timer-name{font-size:.88rem;color:#fff;margin-bottom:10px;font-style:italic}
.timer-display{display:flex;gap:9px;align-items:flex-end}
.tblock{text-align:center}
.tnum{font-family:'Cinzel',serif;font-size:1.55rem;font-weight:700;color:#fff;line-height:1;display:block}
.tlabel{font-size:.58rem;letter-spacing:.12em;text-transform:uppercase;color:var(--muted)}
.tsep{font-family:'Cinzel',serif;font-size:1.35rem;color:var(--gold);opacity:.45;margin-bottom:4px;align-self:flex-end}
.timer-status{font-size:.73rem;margin-top:8px;font-style:italic}
.t-active{color:var(--win)}.t-upcoming{color:var(--hsr)}.t-ended{color:var(--muted)}

/* BRACKET */
.bracket-box{background:var(--surface);border:1px solid var(--border);border-radius:3px;padding:22px;margin-bottom:18px}
.bracket-heading{font-family:'Cinzel',serif;font-size:.68rem;font-weight:700;letter-spacing:.15em;text-transform:uppercase;color:var(--gold);margin-bottom:16px;display:flex;align-items:center;gap:10px}
.bracket-heading::after{content:'';flex:1;height:1px;background:linear-gradient(to right,var(--border2),transparent)}
.bracket-note{font-size:.76rem;color:var(--muted);font-style:italic;margin-top:-10px;margin-bottom:16px}
.standings-table{width:100%;border-collapse:collapse;font-size:.8rem}
.standings-table th{font-family:'Cinzel',serif;font-size:.56rem;font-weight:700;letter-spacing:.14em;text-transform:uppercase;color:var(--muted);text-align:left;padding:7px 9px;border-bottom:1px solid var(--border)}
.standings-table td{padding:8px 9px;border-bottom:1px solid var(--border);vertical-align:middle}
.standings-table tr:last-child td{border-bottom:none}
.standings-table tr:hover td{background:rgba(201,168,76,.03)}
.rank-num{font-family:'Cinzel',serif;font-weight:700;color:var(--gold);font-size:.82rem}
.team-name{color:#fff;font-weight:600}
.team-tag{font-size:.7rem;color:var(--muted);margin-left:3px}
.rec-w{color:var(--win)}.rec-l{color:var(--loss)}
.qual-tag{font-family:'Cinzel',serif;font-size:.53rem;font-weight:700;letter-spacing:.1em;text-transform:uppercase;padding:2px 6px;border-radius:2px}
.qual-ub{background:rgba(201,168,76,.14);color:var(--gold)}
.qual-pi{background:rgba(123,156,239,.14);color:var(--hsr)}

/* MATCH GRID */
.bracket-rounds{display:grid;grid-template-columns:repeat(auto-fit,minmax(210px,1fr));gap:13px;margin-top:16px}
.bracket-round h4{font-family:'Cinzel',serif;font-size:.6rem;font-weight:700;letter-spacing:.14em;text-transform:uppercase;color:var(--muted);margin-bottom:9px;padding-bottom:5px;border-bottom:1px solid var(--border)}
.match-card{background:var(--bg2);border:1px solid var(--border);border-radius:2px;margin-bottom:7px;overflow:hidden;transition:border-color .2s}
.match-card.updated{border-color:rgba(201,168,76,.4);animation:flashBorder .8s ease}
@keyframes flashBorder{0%,100%{border-color:var(--border)}50%{border-color:var(--gold)}}
.match-date{font-size:.63rem;padding:4px 9px;background:rgba(201,168,76,.05);border-bottom:1px solid var(--border);color:var(--muted);font-style:italic}
.match-team{display:flex;align-items:center;justify-content:space-between;padding:6px 9px}
.match-team+.match-team{border-top:1px solid var(--border)}
.mt-name{font-size:.8rem;color:#c0b8a8}
.mt-score{font-family:'Cinzel',serif;font-weight:700;font-size:.88rem}
.score-win{color:var(--win)}.score-loss{color:var(--loss)}.score-tbd{color:var(--pending)}
.match-team.is-winner{background:rgba(76,175,122,.05)}
.match-format{font-size:.59rem;padding:3px 9px;color:var(--muted);border-top:1px solid var(--border);text-align:center;font-style:italic}
.match-live{background:rgba(232,69,69,.08);border-color:rgba(232,69,69,.3)}
.live-badge{font-size:.6rem;color:var(--loss);font-family:'Cinzel',serif;font-weight:700;letter-spacing:.1em;animation:pulse 1s infinite}

/* TIMELINE */
.timeline{border-left:2px solid var(--border2);margin-left:10px;padding-left:24px}
.tl-item{position:relative;padding-bottom:24px}
.tl-item:last-child{padding-bottom:0}
.tl-dot{position:absolute;left:-31px;top:5px;width:10px;height:10px;border-radius:50%;border:2px solid var(--bg)}
.dot-mcgg{background:var(--mcgg);box-shadow:0 0 6px var(--mcgg)}.dot-mlbb{background:var(--mlbb);box-shadow:0 0 6px var(--mlbb)}.dot-hsr{background:var(--hsr);box-shadow:0 0 6px var(--hsr)}
.tl-meta{display:flex;align-items:center;gap:7px;margin-bottom:3px}
.tl-date{font-family:'Cinzel',serif;font-size:.63rem;font-weight:700;color:var(--muted)}
.tl-item h4{font-family:'Cinzel',serif;font-size:.86rem;font-weight:600;color:#fff;margin-bottom:2px}
.tl-item p{font-size:.76rem;color:#5e5850}

/* FOOTER */
footer{margin-top:56px;padding:24px 5vw;border-top:1px solid var(--border);display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:12px}
.footer-logo{font-family:'Cinzel',serif;font-weight:700;font-size:.88rem;color:var(--gold);letter-spacing:.1em}
footer p{font-size:.72rem;color:var(--muted);font-style:italic}
.footer-links{display:flex;gap:16px}
.footer-links a{font-family:'Cinzel',serif;font-size:.6rem;font-weight:600;letter-spacing:.08em;color:var(--muted);text-decoration:none;text-transform:uppercase}
.footer-links a:hover{color:var(--gold)}

/* LIVE NEWS SECTION LABEL */
.auto-label{font-family:'Cinzel',serif;font-size:.6rem;letter-spacing:.18em;text-transform:uppercase;color:var(--muted);margin-bottom:11px;display:flex;align-items:center;gap:8px}
.auto-label::before{content:'◈';color:var(--gold);opacity:.6}

@media(max-width:640px){nav{display:none}.bracket-rounds{grid-template-columns:1fr}.timer-grid{grid-template-columns:1fr}}
</style>
</head>
<body>
<canvas id="stars-canvas"></canvas>
<div class="wrap">

<!-- ─── HEADER ─── -->
<header>
  <div class="logo">The Merchant<small>Game Intelligence Hub</small></div>
  <nav>
    <a href="#mcgg">MCGG</a>
    <a href="#mlbb">MLBB</a>
    <a href="#hsr">HSR</a>
    <a href="#calendar">Calendar</a>
  </nav>
</header>

<!-- ─── STATUS BAR ─── -->
<div class="status-bar">
  <span class="live-dot"></span>
  <span class="status-item">Auto-refreshes every <strong>15 min</strong></span>
  <span class="status-divider">·</span>
  <span class="status-item">Last updated: <strong id="last-updated">Loading…</strong></span>
  <span class="status-divider">·</span>
  <span class="status-item" id="bracket-status">Bracket: <strong>Fetching…</strong></span>
  <span class="status-divider">·</span>
  <span class="status-item" id="news-status">News feeds: <strong>Fetching…</strong></span>
  <button class="refresh-btn" onclick="refreshAll()" id="refresh-btn">↻ Refresh Now</button>
</div>

<!-- ─── TICKER ─── -->
<div class="ticker-wrap">
  <div class="ticker-inner" id="ticker-inner">
    <span class="ticker-item"><span class="t-mcgg t-game">MCGG</span>Season 6 Dawnlight · Commander Kalea · Anniversary Update</span>
    <span class="ticker-sep">◆</span>
    <span class="ticker-item"><span class="t-mlbb t-game">MLBB</span>Season 40 · Patch 2.1.70 · New Hero: Marcel</span>
    <span class="ticker-sep">◆</span>
    <span class="ticker-item"><span class="t-hsr t-game">HSR</span>v4.2 Phase 2 · Evanescia banner · v4.3 launches June 1</span>
    <span class="ticker-sep">◆</span>
    <span class="ticker-item"><span class="t-mlbb t-game">MPL PH S17</span>Playoffs LIVE · May 27–31 · FilOil Centre, San Juan City</span>
    <span class="ticker-sep">◆</span>
    <span class="ticker-item"><span class="t-hsr t-game">HSR</span>"The Lethe Below the Living" · Mortenax Blade · Starward Mode</span>
    <span class="ticker-sep">◆</span>
    <span class="ticker-item"><span class="t-mcgg t-game">MCGG</span>Layla Blue Specter skin returns · Free for limited time</span>
    <span class="ticker-sep">◆</span>
    <span class="ticker-item"><span class="t-mlbb t-game">MLBB</span>Adv. Server Patch 2.1.84 · Argus · Hirara · Masha buffed</span>
    <span class="ticker-sep">◆</span>
    <span class="ticker-item"><span class="t-hsr t-game">HSR</span>Fate/Stay Night collab still active · Free Gilgamesh with v4.3</span>
    <!-- mirror for seamless loop -->
    <span class="ticker-item"><span class="t-mcgg t-game">MCGG</span>Season 6 Dawnlight · Commander Kalea · Anniversary Update</span>
    <span class="ticker-sep">◆</span>
    <span class="ticker-item"><span class="t-mlbb t-game">MLBB</span>Season 40 · Patch 2.1.70 · New Hero: Marcel</span>
    <span class="ticker-sep">◆</span>
    <span class="ticker-item"><span class="t-hsr t-game">HSR</span>v4.2 Phase 2 · Evanescia banner · v4.3 launches June 1</span>
    <span class="ticker-sep">◆</span>
    <span class="ticker-item"><span class="t-mlbb t-game">MPL PH S17</span>Playoffs LIVE · May 27–31 · FilOil Centre, San Juan City</span>
    <span class="ticker-sep">◆</span>
    <span class="ticker-item"><span class="t-hsr t-game">HSR</span>"The Lethe Below the Living" · Mortenax Blade · Starward Mode</span>
    <span class="ticker-sep">◆</span>
    <span class="ticker-item"><span class="t-mcgg t-game">MCGG</span>Layla Blue Specter skin returns · Free for limited time</span>
    <span class="ticker-sep">◆</span>
    <span class="ticker-item"><span class="t-mlbb t-game">MLBB</span>Adv. Server Patch 2.1.84 · Argus · Hirara · Masha buffed</span>
    <span class="ticker-sep">◆</span>
    <span class="ticker-item"><span class="t-hsr t-game">HSR</span>Fate/Stay Night collab still active · Free Gilgamesh with v4.3</span>
  </div>
</div>

<!--
