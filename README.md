<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Analytics IntelX</title>
<style>
:root{
  --navy:#05164D; --navy-2:#0A2266; --amber:#F5A800;
  --text:#1D1D1F; --text2:#6E6E73; --text3:#A1A1A6;
  --bg:#FBFBFD; --surface:#FFFFFF; --fill:#F6F6F8; --fill-2:#F0F0F3; --border:#E9E9EE;
  --ok-bg:#E7F5EF; --ok-tx:#0F6E56; --ok-dot:#1D9E75;
  --warn-bg:#FBF0DA; --warn-tx:#633806; --warn-tx2:#854F0B; --warn-bd:#EDBE6A; --hero:#FEFBF3;
  --dng-bg:#FCEBEB; --dng-tx:#A32D2D;
  --r:12px; --r-sm:10px;
  --font:-apple-system,BlinkMacSystemFont,"SF Pro Text","Segoe UI",Roboto,system-ui,sans-serif;
}
*{box-sizing:border-box;margin:0;padding:0}
html,body{height:100%}
body{font-family:var(--font);background:var(--bg);color:var(--text);font-size:15px;line-height:1.6;-webkit-font-smoothing:antialiased;display:flex;flex-direction:column}
button{font-family:inherit;cursor:pointer;border:none;background:none;color:inherit}
input,textarea{font-family:inherit}
svg{display:block}
:focus-visible{outline:2px solid var(--navy);outline-offset:2px;border-radius:6px}
@media (prefers-reduced-motion:reduce){*{transition:none!important;animation:none!important}}

/* ---------- top bar ---------- */
.topbar{height:56px;flex-shrink:0;display:flex;align-items:center;gap:12px;padding:0 20px;border-bottom:1px solid var(--border);background:var(--surface)}
.brand{display:flex;align-items:center;gap:9px}
.brand .dot{width:9px;height:9px;border-radius:50%;background:var(--amber)}
.brand .nm{font-size:15px;font-weight:600;color:var(--navy);letter-spacing:-0.2px;line-height:1.1}
.brand .nby{font-size:10.5px;color:var(--text3);font-weight:400;margin-top:2px;letter-spacing:.01em}

/* key pill + modal */
.keypill{display:flex;align-items:center;gap:7px;font-size:12px;color:var(--text2);border:1px solid var(--border);border-radius:20px;padding:6px 12px;background:var(--surface);cursor:pointer;transition:border-color .15s}
.keypill:hover{border-color:var(--navy);color:var(--navy)}
.keypill svg{width:13px;height:13px}
.keypill .kdot{width:6px;height:6px;border-radius:50%;background:var(--text3)}
.keypill.ok .kdot{background:var(--ok-dot)}
.keypill.ok{color:var(--ok-tx);border-color:var(--ok-bg);background:var(--ok-bg)}
.backdrop{position:fixed;inset:0;background:rgba(5,22,77,.32);display:none;align-items:center;justify-content:center;z-index:50;padding:20px}
.backdrop.on{display:flex}
.modal{background:var(--surface);border:1px solid var(--border);border-radius:14px;width:100%;max-width:440px;padding:24px 24px 20px;box-shadow:0 20px 60px rgba(5,22,77,.18)}
.modal h3{font-size:17px;font-weight:600;color:var(--navy);letter-spacing:-0.2px;margin-bottom:6px}
.modal .msub{font-size:13px;color:var(--text2);margin-bottom:16px;line-height:1.6}
.modal input{width:100%;font-size:14px;padding:11px 13px;border:1px solid var(--border);border-radius:10px;color:var(--text);outline:none;transition:border-color .15s;font-family:var(--font)}
.modal input:focus{border-color:var(--navy)}
.modal .row{display:flex;gap:9px;justify-content:flex-end;margin-top:16px}
.modal .fine{font-size:11px;color:var(--text3);margin-top:12px;line-height:1.6}
.spacer{flex:1}
.pill{display:flex;align-items:center;gap:8px;font-size:12.5px;color:var(--text2);border:1px solid var(--border);border-radius:20px;padding:6px 12px;background:var(--surface)}
.pill .g{width:14px;height:14px}
.pill .live{width:6px;height:6px;border-radius:50%;background:var(--ok-dot)}
.avatar{width:30px;height:30px;border-radius:50%;background:var(--navy);color:#fff;display:flex;align-items:center;justify-content:center;font-size:11px;font-weight:600;letter-spacing:.3px}

/* ---------- layout ---------- */
.shell{flex:1;display:flex;min-height:0}
.rail{width:216px;flex-shrink:0;border-right:1px solid var(--border);background:var(--surface);padding:16px 12px;display:flex;flex-direction:column;gap:3px;overflow-y:auto}
.nav{display:flex;align-items:center;gap:11px;padding:10px 12px;border-radius:var(--r-sm);font-size:13.5px;color:var(--text2);transition:background .12s,color .12s}
.nav:hover{background:var(--fill)}
.nav.on{background:var(--navy);color:#fff}
.nav svg{width:18px;height:18px;flex-shrink:0}
.rail-lbl{font-size:11px;color:var(--text3);padding:14px 12px 6px;letter-spacing:.02em}
.ctx-card{margin-top:auto;padding:14px 12px;border-top:1px solid var(--border)}
.ctx-card .h{font-size:10.5px;color:var(--text3);margin-bottom:8px;letter-spacing:.04em;text-transform:uppercase;font-weight:500}
.ctx-card .row{font-size:12.5px;color:var(--text2);line-height:1.75}
.ctx-card .row b{font-weight:600;color:var(--navy)}

.main{flex:1;overflow-y:auto}
.view{display:none;max-width:900px;margin:0 auto;padding:34px 32px 80px}
.view.on{display:block}

/* ---------- shared ---------- */
.greet{font-size:24px;font-weight:600;color:var(--navy);letter-spacing:-0.4px;margin-bottom:4px}
.sub{font-size:14px;color:var(--text2)}

/* view header row */
.v-head{display:flex;align-items:flex-start;justify-content:space-between;gap:18px;margin-bottom:24px;flex-wrap:wrap}
.src-pill{display:flex;align-items:center;gap:11px;padding:9px 14px 9px 12px;border:1px solid var(--border);border-radius:12px;background:var(--surface)}
.src-pill .g{width:18px;height:18px;flex-shrink:0}
.src-pill .src-name{font-size:12.5px;font-weight:500;color:var(--navy);line-height:1.2}
.src-pill .src-sub{font-size:11px;color:var(--text3);line-height:1.2;margin-top:2px}
.src-pill .src-live{width:7px;height:7px;border-radius:50%;background:var(--ok-dot);flex-shrink:0;margin-left:2px}

/* first-run key setup card */
.key-setup{display:flex;align-items:center;justify-content:space-between;gap:16px;background:var(--hero);border:1px solid var(--warn-bd);border-radius:var(--r);padding:16px 20px;margin-bottom:16px}
.key-setup.hide{display:none}
.key-setup-h{font-size:14px;font-weight:600;color:var(--warn-tx);letter-spacing:-0.1px;margin-bottom:3px}
.key-setup-p{font-size:12.5px;color:var(--warn-tx);line-height:1.5;opacity:.85}
.key-setup .btn-primary{flex-shrink:0;padding:9px 18px;font-size:13px}
.ask.locked{opacity:.5;pointer-events:none}

/* landing preview card */
.landing{margin-top:26px;border:1px dashed var(--border);border-radius:var(--r);padding:20px 22px;background:var(--surface)}
.landing.hide{display:none}
.landing-h{font-size:12px;color:var(--text3);letter-spacing:.04em;text-transform:uppercase;font-weight:500;margin-bottom:14px}
.landing-cards{display:grid;grid-template-columns:repeat(3,1fr);gap:14px}
.lc{padding:6px 4px}
.lc svg{width:22px;height:22px;color:var(--navy);opacity:.6;margin-bottom:9px}
.lc-t{font-size:13px;font-weight:500;color:var(--navy);margin-bottom:3px}
.lc-s{font-size:12px;color:var(--text2);line-height:1.55}
.landing-foot{display:flex;align-items:center;gap:8px;margin-top:16px;padding-top:14px;border-top:1px solid var(--border);font-size:11.5px;color:var(--text3)}
.landing-foot .d{width:6px;height:6px;border-radius:50%;background:var(--ok-dot)}
@media(max-width:820px){.landing-cards{grid-template-columns:1fr}}
.eyebrow{font-size:12px;color:var(--text3);margin-bottom:9px;letter-spacing:.01em}
.card{background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:20px 22px}
.metric-lbl{font-size:12px;color:var(--text2);margin-bottom:4px}
.metric-val{font-size:22px;font-weight:600;color:var(--navy);letter-spacing:-0.3px}
.btn-primary{display:inline-flex;align-items:center;justify-content:center;gap:8px;background:var(--navy);color:#fff;font-size:14px;font-weight:500;padding:12px 20px;border-radius:var(--r-sm);transition:background .15s}
.btn-primary:hover{background:var(--navy-2)}
.btn-primary svg{width:16px;height:16px}
.btn-ghost{display:inline-flex;align-items:center;gap:7px;border:1px solid var(--border);color:var(--text2);font-size:13px;font-weight:500;padding:9px 15px;border-radius:var(--r-sm);transition:border-color .15s,color .15s}
.btn-ghost:hover{border-color:var(--navy);color:var(--navy)}
.tag{font-size:11px;border-radius:6px;padding:3px 9px;font-weight:500;display:inline-flex;align-items:center;gap:5px}
.tag.ok{background:var(--ok-bg);color:var(--ok-tx)}
.tag.warn{background:var(--warn-bg);color:var(--warn-tx)}
.tag.dng{background:var(--dng-bg);color:var(--dng-tx)}
.tag.amber{background:var(--warn-bg);color:var(--warn-tx2)}

/* ---------- analytics: ask ---------- */
.ask{display:flex;align-items:flex-end;gap:10px;border:1px solid var(--border);border-radius:14px;padding:12px 12px 12px 16px;background:var(--surface);transition:border-color .15s,box-shadow .15s;margin-top:22px}
.ask:focus-within{border-color:var(--navy);box-shadow:0 0 0 4px rgba(5,22,77,.05)}
.ask .spark{color:var(--amber);width:20px;height:20px;margin-bottom:8px;flex-shrink:0}
.ask textarea{flex:1;border:none;outline:none;resize:none;font-size:15px;line-height:1.5;color:var(--text);background:transparent;padding:6px 0;max-height:120px}
.ask .send{width:36px;height:36px;border-radius:10px;background:var(--navy);color:#fff;display:flex;align-items:center;justify-content:center;flex-shrink:0;transition:background .15s}
.ask .send:hover{background:var(--navy-2)}
.ask .send svg{width:17px;height:17px}
.suggest{display:flex;flex-wrap:wrap;gap:8px;margin-top:14px}
.sg{font-size:12.5px;color:var(--text2);border:1px solid var(--border);border-radius:20px;padding:7px 14px;background:var(--surface);transition:border-color .15s,color .15s}
.sg:hover{border-color:var(--navy);color:var(--navy)}

.adv-toggle{display:inline-flex;align-items:center;gap:7px;margin-top:16px;font-size:12.5px;color:var(--text2)}
.adv-toggle .chev{width:12px;height:12px;transition:transform .2s}
.adv-panel{display:none;margin-top:14px;border:1px solid var(--border);border-radius:var(--r);padding:18px 20px;background:var(--surface)}
.adv-panel.open{display:block}
.adv-grid{display:grid;grid-template-columns:1fr 1fr;gap:20px}
.field-lbl{font-size:12px;color:var(--text2);margin-bottom:8px}
.pills,.chips{display:flex;flex-wrap:wrap;gap:7px}
.dpill{font-size:12.5px;color:var(--text2);border:1px solid var(--border);border-radius:20px;padding:6px 13px;background:var(--surface);transition:all .12s}
.dpill.on{background:var(--amber);border-color:var(--amber);color:#fff;font-weight:500}
.dpill:not(.on):hover{border-color:var(--navy);color:var(--navy)}
.chip{font-size:12.5px;color:var(--text2);border:1px solid var(--border);border-radius:9px;padding:6px 12px;background:var(--surface);transition:all .12s}
.chip.on{background:var(--navy);border-color:var(--navy);color:#fff}
.chip:not(.on):hover{border-color:var(--navy);color:var(--navy)}
.adv-sep{height:1px;background:var(--border);margin:16px 0}

/* result */
.result{margin-top:24px;display:none}
.result.show{display:block;animation:rise .3s ease}
@keyframes rise{from{opacity:0;transform:translateY(6px)}to{opacity:1;transform:none}}
.thinking{display:none;align-items:center;gap:10px;margin-top:24px;color:var(--text2);font-size:13.5px}
.thinking.show{display:flex}
.dots{display:inline-flex;gap:4px}
.dots i{width:6px;height:6px;border-radius:50%;background:var(--amber);animation:blink 1.2s infinite both}
.dots i:nth-child(2){animation-delay:.2s}.dots i:nth-child(3){animation-delay:.4s}
@keyframes blink{0%,80%,100%{opacity:.25}40%{opacity:1}}
.answer .q{font-size:13px;color:var(--text2);margin-bottom:14px;display:flex;align-items:center;gap:9px}
.answer .q .u{width:24px;height:24px;border-radius:50%;background:var(--navy);color:#fff;display:flex;align-items:center;justify-content:center;font-size:9px;font-weight:600;flex-shrink:0}
.answer .insight{font-size:15.5px;line-height:1.65;margin-bottom:16px}
.answer .insight b{font-weight:600;color:var(--navy)}
.mrow{display:flex;gap:30px;margin-bottom:18px}
.mrow .v.dn{color:var(--dng-tx)}
.mrow .v.up{color:var(--ok-tx)}
.nextact{display:flex;align-items:center;gap:9px;background:var(--hero);border:1px solid var(--warn-bd);border-radius:var(--r-sm);padding:11px 14px;font-size:13px;color:var(--warn-tx)}
.nextact svg{width:16px;height:16px;color:var(--warn-tx2);flex-shrink:0}
.dq{display:flex;align-items:center;gap:8px;margin-top:14px;font-size:12px;color:var(--text2)}
.dq .d{width:7px;height:7px;border-radius:50%;background:var(--ok-dot)}
.warnband{display:flex;align-items:center;gap:9px;background:var(--warn-bg);border-radius:var(--r-sm) var(--r-sm) 0 0;padding:10px 16px;font-size:12.5px;color:var(--warn-tx);margin:-20px -22px 16px;border-bottom:1px solid var(--warn-bd)}
.warnband svg{width:15px;height:15px;color:var(--warn-tx2);flex-shrink:0}
.answer.sampled{border-color:var(--warn-bd)}
.actions{display:flex;gap:9px;flex-wrap:wrap;margin-top:16px}
.fine{font-size:11.5px;color:var(--text3);margin-top:12px;line-height:1.6}

/* ---------- competitive intelligence ---------- */
.ci-head{display:flex;justify-content:space-between;align-items:flex-start;gap:20px;margin-bottom:14px;flex-wrap:wrap}
.vs-switch{display:inline-flex;background:var(--fill);border-radius:11px;padding:3px;gap:2px}
.vs-btn{display:flex;flex-direction:column;align-items:flex-start;padding:7px 13px 6px;border-radius:8px;color:var(--text2);min-width:0}
.vs-btn .k{font-size:11px;font-weight:600;letter-spacing:.06em;line-height:1.1}
.vs-btn .l{font-size:10.5px;color:var(--text3);line-height:1.2;margin-top:1px}
.vs-btn.on{background:var(--surface);color:var(--navy);box-shadow:0 1px 2px rgba(5,22,77,.06)}
.vs-btn.on .l{color:var(--text2)}
.vs-banner{display:flex;align-items:center;gap:9px;font-size:12.5px;color:var(--text2);margin-bottom:14px;padding:9px 14px;border:1px solid var(--border);border-radius:10px;background:var(--surface)}
.vs-banner .tag{flex-shrink:0}
.ppl-empty{margin:22px 0 26px;padding:32px 24px;text-align:center;border:1px dashed var(--border);border-radius:var(--r);background:var(--surface)}
.ppl-empty svg{width:28px;height:28px;color:var(--text3);margin-bottom:10px}
.ppl-empty .ppl-h{font-size:14px;font-weight:500;color:var(--navy);margin-bottom:5px}
.ppl-empty .ppl-p{font-size:12.5px;color:var(--text2);line-height:1.6;max-width:440px;margin:0 auto}
.ci-hero{border:1px solid var(--warn-bd);border-radius:var(--r);padding:22px 24px;background:var(--hero);display:flex;justify-content:space-between;align-items:center;gap:20px;margin-bottom:14px}
.ci-hero .score{font-size:44px;font-weight:600;color:var(--navy);line-height:1;letter-spacing:-1px}
.ci-hero .score small{font-size:16px;color:var(--text3);font-weight:500}
.plats{display:grid;grid-template-columns:repeat(4,1fr);gap:10px;margin-bottom:22px}
.plat{background:var(--fill);border-radius:var(--r-sm);padding:14px 15px}
.plat .n{font-size:13px;color:var(--text2);margin-bottom:8px}
.plat .bar{height:5px;border-radius:3px;background:var(--fill-2);overflow:hidden;margin-bottom:8px}
.plat .bar i{display:block;height:100%;background:var(--navy);border-radius:3px}
.plat .v{font-size:18px;font-weight:600;color:var(--navy)}
.sect-h{font-size:16px;font-weight:600;color:var(--navy);letter-spacing:-0.2px;margin:26px 0 4px}
.sect-s{font-size:13px;color:var(--text2);margin-bottom:14px}
.spot-switch{display:inline-flex;background:var(--fill);border-radius:9px;padding:3px;gap:2px;margin-bottom:14px}
.spot-switch button{font-size:12.5px;color:var(--text2);padding:6px 13px;border-radius:7px}
.spot-switch button.on{background:var(--surface);color:var(--navy);font-weight:500}
.spot .meta{font-size:12px;color:var(--text3);margin:2px 0 14px}
.spot .big{display:flex;align-items:baseline;gap:9px;margin-bottom:16px}
.spot .big .n{font-size:28px;font-weight:600;color:var(--dng-tx)}
.spot .big .t{font-size:14px;color:var(--text2)}
.srcgrp{margin-bottom:11px}
.srcgrp .t{font-size:12px;color:var(--navy);font-weight:500;margin-bottom:2px}
.srcgrp .b{font-size:13px;color:var(--text2)}
.brandchips{display:flex;flex-wrap:wrap;gap:6px}
.bchip{font-size:12px;border:1px solid var(--border);border-radius:6px;padding:4px 9px;color:var(--text2)}
.signals{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:9px;margin-top:16px}
.srow{display:flex;align-items:center;gap:12px;border:1px solid var(--border);border-radius:var(--r-sm);padding:13px 14px;transition:border-color .15s}
.srow:hover{border-color:var(--navy)}
.srow svg{width:19px;height:19px;color:var(--navy);flex-shrink:0}
.srow .t{font-size:13px;color:var(--text)}
.srow .s{font-size:11.5px;color:var(--text3)}

@media(max-width:820px){
  .rail{width:64px;padding:14px 8px}
  .nav span,.rail-lbl,.ctx-card{display:none}
  .nav{justify-content:center}
  .adv-grid{grid-template-columns:1fr}
  .plats{grid-template-columns:repeat(2,1fr)}
  .view{padding:24px 18px 60px}
  .mrow{gap:20px}
}
</style>
</head>
<body>

<div class="topbar">
  <div class="brand"><span class="dot"></span><div><div class="nm">Analytics IntelX</div><div class="nby">V10 · By Ganeshbalan Pandy</div></div></div>
  <div class="spacer"></div>
  <div class="avatar">GP</div>
</div>

<div class="shell">
  <nav class="rail">
    <div class="nav on" id="nav-an" onclick="go('analytics')"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M4 20V10M10 20V4M16 20v-8M2 20h20"/></svg><span>Analytics</span></div>
    <div class="nav" id="nav-ci" onclick="go('ci')"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><circle cx="12" cy="12" r="9"/><circle cx="12" cy="12" r="4"/><path d="M12 3v3M12 18v3M3 12h3M18 12h3"/></svg><span>Competitive intelligence</span></div>
    <div class="ctx-card">
      <div class="h">Scoped to</div>
      <div class="row"><b>lufthansa.com</b><br>Inspiration / Search / Booking<br><span style="color:var(--text3)">EU market · booking KPIs</span></div>
    </div>
  </nav>

  <main class="main">

    <!-- ANALYTICS -->
    <section class="view on" id="view-analytics">
      <div class="v-head">
        <div>
          <div class="greet">Welcome back, Ganesh</div>
          <div class="sub">Ask anything about your GA4 data in natural language.</div>
        </div>
        <div class="src-pill" onclick="void 0" title="Connected data source">
          <svg class="g" viewBox="0 0 24 24"><path fill="#4285F4" d="M22.56 12.25c0-.78-.07-1.53-.2-2.25H12v4.26h5.92c-.26 1.37-1.04 2.53-2.21 3.31v2.77h3.57c2.08-1.92 3.28-4.74 3.28-8.09z"/><path fill="#34A853" d="M12 23c2.97 0 5.46-.98 7.28-2.66l-3.57-2.77c-.98.66-2.23 1.06-3.71 1.06-2.86 0-5.29-1.93-6.16-4.53H2.18v2.84C3.99 20.53 7.7 23 12 23z"/><path fill="#FBBC05" d="M5.84 14.09c-.22-.66-.35-1.36-.35-2.09s.13-1.43.35-2.09V7.07H2.18C1.43 8.55 1 10.22 1 12s.43 3.45 1.18 4.93l3.66-2.84z"/><path fill="#EA4335" d="M12 5.38c1.62 0 3.06.56 4.21 1.64l3.15-3.15C17.45 2.09 14.97 1 12 1 7.7 1 3.99 3.47 2.18 7.07l3.66 2.84c.87-2.6 3.3-4.53 6.16-4.53z"/></svg>
          <div>
            <div class="src-name">GA4 Demo Account</div>
            <div class="src-sub">Google Merchandise Store</div>
          </div>
          <span class="src-live"></span>
        </div>
      </div>

      <div class="key-setup" id="key-setup">
        <div class="key-setup-body">
          <div class="key-setup-h">One quick step to enable analysis</div>
          <div class="key-setup-p">Add your Anthropic API key to run natural-language questions against your GA4 data. Stays in your browser only.</div>
        </div>
        <button class="btn-primary" onclick="openKey()">Add key</button>
      </div>

      <div class="ask" id="ask-box">
        <svg class="spark" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2l1.9 5.6L19.5 9l-5.6 1.9L12 16.5l-1.9-5.6L4.5 9l5.6-1.4z"/></svg>
        <textarea id="q" rows="1" placeholder="Which booking pages lost the most traffic last month?" oninput="grow(this)"></textarea>
        <button class="send" onclick="ask()" aria-label="Ask"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 19V5M5 12l7-7 7 7"/></svg></button>
      </div>
      <div class="suggest">
        <button class="sg" onclick="pick(this)">Top landing pages for EU booking traffic</button>
        <button class="sg" onclick="pick(this)">Booking funnel drop-off by device, last 90 days</button>
        <button class="sg" onclick="pick(this)">Mobile vs desktop conversion this month</button>
      </div>

      <button class="adv-toggle" onclick="toggleAdv(this)">Advanced options <svg class="chev" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><polyline points="6 9 12 15 18 9"/></svg></button>
      <div class="adv-panel" id="adv">
        <div class="field-lbl">Date range</div>
        <div class="pills" id="dates">
          <button class="dpill" onclick="one(this,'dates')">7d</button>
          <button class="dpill on" onclick="one(this,'dates')">28d</button>
          <button class="dpill" onclick="one(this,'dates')">90d</button>
          <button class="dpill" onclick="one(this,'dates')">12m</button>
        </div>
        <div class="adv-sep"></div>
        <div class="adv-grid">
          <div>
            <div class="field-lbl">Dimensions</div>
            <div class="chips">
              <button class="chip on" onclick="tog(this)">page_path</button>
              <button class="chip on" onclick="tog(this)">device</button>
              <button class="chip" onclick="tog(this)">country</button>
              <button class="chip" onclick="tog(this)">source</button>
            </div>
          </div>
          <div>
            <div class="field-lbl">Metrics</div>
            <div class="chips">
              <button class="chip on" onclick="tog(this)">Sessions</button>
              <button class="chip on" onclick="tog(this)">Conversions</button>
              <button class="chip on" onclick="tog(this)">Revenue</button>
              <button class="chip" onclick="tog(this)">Bounce rate</button>
            </div>
          </div>
        </div>
      </div>

      <div class="landing" id="landing">
        <div class="landing-h">What you'll see</div>
        <div class="landing-cards">
          <div class="lc"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><path d="M3 12h4l3-9 4 18 3-9h4"/></svg><div class="lc-t">A clear insight</div><div class="lc-s">One sentence answer, with the key figure highlighted.</div></div>
          <div class="lc"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><rect x="3" y="12" width="4" height="9"/><rect x="10" y="7" width="4" height="14"/><rect x="17" y="4" width="4" height="17"/></svg><div class="lc-t">The metrics that matter</div><div class="lc-s">Sessions, conversion, and change, side by side.</div></div>
          <div class="lc"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><path d="M5 12h14"/><path d="M13 6l6 6-6 6"/></svg><div class="lc-t">A recommended next step</div><div class="lc-s">One action to take, based on what the data shows.</div></div>
        </div>
        <div class="landing-foot"><span class="d"></span>Every answer indicates unsampled or sampled data</div>
      </div>

      <div class="thinking" id="thinking"><span class="dots"><i></i><i></i><i></i></span> Reading your GA4 data...</div>

      <div class="result" id="result">
        <div class="card answer" id="answer"></div>
      </div>
    </section>

    <!-- COMPETITIVE INTELLIGENCE -->
    <section class="view" id="view-ci">
      <div class="ci-head">
        <div>
          <div class="greet" style="font-size:22px">Competitive intelligence</div>
          <div class="sub">lufthansa.com &nbsp;·&nbsp; powered by SEMrush</div>
        </div>
        <div class="vs-switch" role="tablist" aria-label="Value stream">
          <button class="vs-btn on" data-vs="ISB" onclick="setVS('ISB',this)"><span class="k">ISB</span><span class="l">Inspiration · Search · Booking</span></button>
          <button class="vs-btn" data-vs="TEX" onclick="setVS('TEX',this)"><span class="k">TEX</span><span class="l">Travel Experience</span></button>
          <button class="vs-btn" data-vs="PPL" onclick="setVS('PPL',this)"><span class="k">PPL</span><span class="l">Loyalty</span></button>
        </div>
      </div>

      <div class="vs-banner" id="vs-banner"></div>

      <div class="ci-hero">
        <div>
          <div style="display:flex;align-items:center;gap:9px;margin-bottom:8px"><span class="tag amber">Primary feature</span><span style="font-size:16px;font-weight:600;color:var(--navy)">AI visibility</span></div>
          <div id="hero-desc" style="font-size:13.5px;color:var(--text2);max-width:460px;line-height:1.6">How often AI assistants name and cite lufthansa.com across ChatGPT, Gemini, and AI Overviews.</div>
        </div>
        <div style="text-align:right;flex-shrink:0">
          <div class="score" id="hero-score">74<small>/100</small></div>
          <div id="hero-delta" style="font-size:11.5px;color:var(--dng-tx);margin-top:4px">&#9660; 9.3K mentions</div>
        </div>
      </div>

      <div class="eyebrow" style="margin-top:18px">Visibility by AI platform</div>
      <div class="plats" id="plats"></div>

      <div id="topics-block">
        <div class="sect-h">Find hot topics</div>
        <div class="sect-s" id="topics-sub">Prompts where competitors appear and Lufthansa does not.</div>
        <div class="spot-switch" id="spot-switch"></div>
        <div class="card spot" id="spot"></div>
      </div>

      <div id="ppl-empty" class="ppl-empty" style="display:none">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.6"><circle cx="12" cy="12" r="9"/><path d="M8 12h8M12 8v8" opacity=".4"/></svg>
        <div class="ppl-h">No prompt-level data yet for Loyalty</div>
        <div class="ppl-p">Loyalty prompts have thin AI-assistant coverage right now, so there's no faithful spotlight to show. When SEMrush picks up qualifying prompts, they'll appear here.</div>
      </div>

      <div class="sect-h">More competitive signals</div>
      <div class="signals" id="signals"></div>
    </section>

  </main>
</div>

<div class="backdrop" id="keymodal">
  <div class="modal" role="dialog" aria-labelledby="km-h">
    <h3 id="km-h">Anthropic API key</h3>
    <div class="msub">Paste your key to run live analysis. It stays in your browser and is never sent to any server other than Anthropic.</div>
    <input type="password" id="keyinput" placeholder="sk-ant-api03-..." autocomplete="off"/>
    <div class="row"><button class="btn-ghost" onclick="closeKey()">Cancel</button><button class="btn-primary" style="padding:10px 18px" onclick="saveKey()">Save key</button></div>
    <div class="fine">Get a key at console.anthropic.com. Clear it any time from this dialog.</div>
  </div>
</div>

<script>
var API_KEY = '';
function reflectKeyState(){
  var setup = document.getElementById('key-setup');
  var ask = document.getElementById('ask-box');
  if(API_KEY){ setup && setup.classList.add('hide'); ask && ask.classList.remove('locked'); }
  else       { setup && setup.classList.remove('hide'); ask && ask.classList.add('locked'); }
}
function openKey(){ document.getElementById('keyinput').value = API_KEY; document.getElementById('keymodal').classList.add('on'); setTimeout(function(){document.getElementById('keyinput').focus()},50); }
function closeKey(){ document.getElementById('keymodal').classList.remove('on'); }
function saveKey(){
  API_KEY = document.getElementById('keyinput').value.trim();
  reflectKeyState();
  closeKey();
}
document.getElementById('keymodal').addEventListener('click', function(e){ if(e.target === this) closeKey(); });
document.addEventListener('keydown', function(e){ if(e.key === 'Escape') closeKey(); });
function go(v){
  document.getElementById('view-analytics').classList.toggle('on', v==='analytics');
  document.getElementById('view-ci').classList.toggle('on', v==='ci');
  document.getElementById('nav-an').classList.toggle('on', v==='analytics');
  document.getElementById('nav-ci').classList.toggle('on', v==='ci');
  document.querySelector('.main').scrollTop = 0;
}
function grow(t){ t.style.height='auto'; t.style.height=Math.min(t.scrollHeight,120)+'px'; }
function pick(b){ var q=document.getElementById('q'); q.value=b.textContent; grow(q); ask(); }
function toggleAdv(b){
  var p=document.getElementById('adv'); p.classList.toggle('open');
  b.querySelector('.chev').style.transform = p.classList.contains('open')?'rotate(180deg)':'';
}
function one(b,id){ document.querySelectorAll('#'+id+' .dpill').forEach(function(x){x.classList.remove('on')}); b.classList.add('on'); }
function tog(b){ b.classList.toggle('on'); }

var INSIGHTS = {
  'Top landing pages for EU booking traffic':{
    insight:'Your top EU booking entry point is the fare-search page, driving <b>38%</b> of booking sessions. The homepage and Miles &amp; More landing follow.',
    metrics:[['Sessions','128.4K'],['Conversion','3.2%'],['vs prev','+4.1%','up']],
    action:'Next action: test a fare-search entry variant for DE and CH markets',
    sampled:false
  },
  'Booking funnel drop-off by device, last 90 days':{
    insight:'The largest drop-off is search to fare results at <b>61%</b>, concentrated on mobile. Desktop completes the funnel at nearly double the rate.',
    metrics:[['Mobile drop','61%','dn'],['Desktop drop','34%'],['Sessions','2.1M']],
    action:'Next action: review fare-results load time and layout on mobile',
    sampled:true
  },
  'Mobile vs desktop conversion this month':{
    insight:'Desktop converts at <b>4.0%</b> versus mobile at <b>2.1%</b>. Mobile carries 63% of sessions but only 41% of bookings.',
    metrics:[['Mobile CVR','2.1%'],['Desktop CVR','4.0%','up'],['Gap','1.9pt','dn']],
    action:'Next action: prioritise the mobile checkout for the next sprint',
    sampled:false
  }
};
function ask(){
  var q=document.getElementById('q').value.trim();
  if(!q) return;
  var res=document.getElementById('result'), think=document.getElementById('thinking'), land=document.getElementById('landing');
  res.classList.remove('show'); think.classList.add('show'); land && land.classList.add('hide');
  setTimeout(function(){
    think.classList.remove('show');
    render(q, INSIGHTS[q] || {
      insight:'Here is what your GA4 data shows for that question, with the key figures and a recommended next step.',
      metrics:[['Sessions','42.8K'],['Conversion','2.6%'],['Change','+1.8%','up']],
      action:'Next action: segment this by device to find where the shift is coming from',
      sampled:false
    });
    res.classList.add('show');
  }, 850);
}
function render(q,d){
  var a=document.getElementById('answer');
  a.classList.toggle('sampled', !!d.sampled);
  var m = d.metrics.map(function(x){
    return '<div><div class="metric-lbl">'+x[0]+'</div><div class="metric-val '+(x[2]||'')+'">'+x[1]+'</div></div>';
  }).join('');
  var band = d.sampled
    ? '<div class="warnband"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M10.3 3.9L1.8 18a2 2 0 001.7 3h17a2 2 0 001.7-3L13.7 3.9a2 2 0 00-3.4 0z"/><path d="M12 9v4M12 17h.01"/></svg> Based on 87% of data · sampled (query exceeded 10M events)</div>'
    : '';
  var footer = d.sampled
    ? '<div class="actions"><button class="btn-primary" style="padding:9px 15px">Request unsampled</button><button class="btn-ghost" onclick="return false">Narrow to 7 days</button></div><div class="fine">Unsampled needs a GA4 360 property, uses the shared daily token quota, and returns asynchronously.</div>'
    : '<div class="dq"><span class="d"></span> Unsampled · 100% of data</div>';
  a.innerHTML = band +
    '<div class="q"><span class="u">GP</span>'+escapeHtml(q)+'</div>'+
    '<div class="insight">'+d.insight+'</div>'+
    '<div class="mrow">'+m+'</div>'+
    '<div class="nextact"><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M5 12h14M13 6l6 6-6 6"/></svg>'+d.action+'</div>'+
    footer;
}
function escapeHtml(s){ return s.replace(/[&<>"]/g,function(c){return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[c];}); }

var VS = {
  ISB:{
    banner:['dng','Major gap','Lufthansa is absent on commercial booking and fare-search prompts. OTAs and low-cost carriers win these.'],
    score:'74', delta:'&#9660; 9.3K mentions', deltaCls:'var(--dng-tx)',
    heroDesc:'How often AI assistants name and cite lufthansa.com for inspiration, search, and booking prompts across ChatGPT, Gemini, and AI Overviews.',
    plats:[['Gemini',81],['AI Mode',77],['ChatGPT',71],['AI Overview',70]],
    spots:{
      a:{ title:'"economy class plane ticket"', meta:'US · AI Overview · 7 brands cited', n:'0', of:'of 16 sources cite lufthansa.com',
          groups:[['Cited brands','Spirit · Ryanair · Expedia · Frontier · Skyscanner · United · American']] },
      b:{ title:'"book flights in europe"', meta:'US · AI Overview · 24 May 2026 · 21 brands cited', n:'0', of:'of 9 sources cite lufthansa.com',
          groups:[['OTAs / aggregators','Skyscanner · Momondo · Expedia'],['Airlines','United · Virgin Atlantic · Alternative Airlines'],['Travel media','Rick Steves · Going · Dollar Flight Club']] }
    },
    signals:[
      ['diff','Gap analysis','14 pages KLM ranks, you don\'t'],
      ['search','Keyword gaps','228 shared, 61 missed'],
      ['page','New pages','Delta added 9 fare pages'],
      ['trend','Traffic trends','Share of search over time']
    ]
  },
  TEX:{
    banner:['ok','We lead','Lufthansa dominates baggage, check-in, and service prompts. Visibility 98-100 across the top prompts.'],
    score:'92', delta:'&#9650; leading category', deltaCls:'var(--ok-tx)',
    heroDesc:'Lufthansa owns the travel-experience conversation: baggage rules, check-in guidance, timetable and status prompts across every major AI assistant.',
    plats:[['Gemini',98],['AI Mode',95],['ChatGPT',89],['AI Overview',87]],
    spots:{
      a:{ title:'"lufthansa carry-on baggage"', meta:'DE · AI Overview · lufthansa.com is a top source', n:'188', of:'prompts cite /carry-on-baggage',
          groups:[['Top cited Lufthansa pages','/handgepaeck (238) · /carry-on-baggage (188) · /baggage-and-other-fees (144)']] },
      b:{ title:'"flight status lufthansa"', meta:'Global · AI assistants routinely cite /timetable-and-flight-status', n:'224', of:'prompts cite the timetable page',
          groups:[['Where Lufthansa is cited','home (737) · /timetable-and-flight-status (224) · /help-and-contact (84)']] }
    },
    signals:[
      ['diff','Gap analysis','You lead all 12 tracked pages'],
      ['search','Keyword gaps','Fully covered'],
      ['page','New pages','No competitor moves this week'],
      ['trend','Traffic trends','Stable 30-day trend']
    ]
  },
  PPL:{
    banner:['warn','Thin coverage','Loyalty prompts have limited AI-assistant visibility. There is no faithful prompt data to show for this stream yet.'],
    score:'--', delta:'no qualifying prompts', deltaCls:'var(--text3)',
    heroDesc:'AI-assistant coverage for Miles & More and loyalty prompts is currently thin. We do not simulate figures for this stream.',
    plats:[['Gemini',null],['AI Mode',null],['ChatGPT',null],['AI Overview',null]],
    spots:null,
    signals:[
      ['diff','Gap analysis','Awaiting qualifying prompts'],
      ['search','Keyword gaps','Not yet mapped'],
      ['page','New pages','No competitor loyalty pages tracked'],
      ['trend','Traffic trends','Insufficient data']
    ]
  }
};

var ICONS = {
  diff:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M7 7h10M7 12h10M7 17h6"/></svg>',
  search:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><circle cx="11" cy="11" r="7"/><path d="M21 21l-4-4"/></svg>',
  page:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><rect x="4" y="4" width="16" height="16" rx="2"/><path d="M8 9h8M8 13h5"/></svg>',
  trend:'<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.8"><path d="M3 17l6-6 4 4 8-8"/><path d="M17 7h4v4"/></svg>'
};

var currentVS = 'ISB', currentSpot = 'a';

function setVS(k, btn){
  currentVS = k;
  document.querySelectorAll('.vs-btn').forEach(function(x){x.classList.remove('on')});
  if(btn) btn.classList.add('on');
  var d = VS[k];

  // banner
  var b = d.banner;
  document.getElementById('vs-banner').innerHTML = '<span class="tag '+b[0]+'">'+b[1]+'</span><span>'+b[2]+'</span>';

  // hero
  document.getElementById('hero-desc').textContent = d.heroDesc;
  document.getElementById('hero-score').innerHTML = d.score === '--' ? '--<small style="font-size:16px;color:var(--text3)"></small>' : d.score+'<small>/100</small>';
  var del = document.getElementById('hero-delta');
  del.innerHTML = d.delta; del.style.color = d.deltaCls;

  // platforms
  var pl = document.getElementById('plats');
  pl.innerHTML = d.plats.map(function(p){
    if(p[1] === null){
      return '<div class="plat"><div class="n">'+p[0]+'</div><div class="bar"><i style="width:0"></i></div><div class="v" style="color:var(--text3);font-size:14px">no data</div></div>';
    }
    return '<div class="plat"><div class="n">'+p[0]+'</div><div class="bar"><i style="width:'+p[1]+'%"></i></div><div class="v">'+p[1]+'</div></div>';
  }).join('');

  // topics block or PPL empty
  var topics = document.getElementById('topics-block'), empty = document.getElementById('ppl-empty');
  if(d.spots){
    topics.style.display = ''; empty.style.display = 'none';
    document.getElementById('topics-sub').textContent = (k === 'TEX')
      ? 'Prompts where Lufthansa is a cited source in AI answers.'
      : 'Prompts where competitors appear and Lufthansa does not.';
    var keys = Object.keys(d.spots);
    var sw = document.getElementById('spot-switch');
    sw.innerHTML = keys.map(function(kk,i){ return '<button'+(i===0?' class="on"':'')+' onclick="spot(\''+kk+'\',this)">'+d.spots[kk].title+'</button>'; }).join('');
    currentSpot = keys[0]; spot(keys[0], null);
  } else {
    topics.style.display = 'none'; empty.style.display = 'block';
  }

  // signals
  document.getElementById('signals').innerHTML = d.signals.map(function(s){
    return '<div class="srow">'+ICONS[s[0]]+'<div><div class="t">'+s[1]+'</div><div class="s">'+s[2]+'</div></div></div>';
  }).join('');
}

function spot(k, btn){
  currentSpot = k;
  var d = VS[currentVS].spots[k];
  if(!d) return;
  if(btn){
    document.querySelectorAll('#spot-switch button').forEach(function(x){x.classList.remove('on')});
    btn.classList.add('on');
  }
  var groups = d.groups.map(function(g){return '<div class="srcgrp"><div class="t">'+g[0]+'</div><div class="b">'+g[1]+'</div></div>';}).join('');
  document.getElementById('spot').innerHTML =
    '<div style="display:flex;align-items:center;gap:8px;flex-wrap:wrap;margin-bottom:2px"><span class="tag '+(currentVS==='TEX'?'ok':'dng')+'">'+(currentVS==='TEX'?'Present':'Not mentioned')+'</span><span style="font-size:14px;font-weight:600;color:var(--navy)">'+d.title+'</span></div>'+
    '<div class="meta">'+d.meta+'</div>'+
    '<div class="big"><span class="n" style="color:'+(currentVS==='TEX'?'var(--ok-tx)':'var(--dng-tx)')+'">'+d.n+'</span><span class="t">'+d.of+'</span></div>'+
    '<div style="font-size:11.5px;color:var(--text3);margin-bottom:9px">'+(currentVS==='TEX'?'What the answer cites':'Who the answer cited instead')+'</div>'+
    groups;
}

setVS('ISB', document.querySelector('.vs-btn'));
reflectKeyState();
</script>
</body>
</html>
