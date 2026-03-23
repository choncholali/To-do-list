<!DOCTYPE html>
<html lang="bn">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Src To-Do List</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=Plus+Jakarta+Sans:wght@300;400;500;600&family=Noto+Sans+Bengali:wght@400;500;600&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#080b11; --bg2:#0d1018; --surface:#111520; --surface2:#171c28; --surface3:#1c2233;
  --border:#232a3c; --border2:#2e3750;
  --violet:#7c6af7; --vg:rgba(124,106,247,.16);
  --rose:#f05a7e;   --rg:rgba(240,90,126,.14);
  --cyan:#2dd4bf;   --cg:rgba(45,212,191,.14);
  --amber:#fbbf24;  --ag:rgba(251,191,36,.12);
  --lime:#a3e635;   --lg:rgba(163,230,53,.12);
  --sky:#38bdf8;    --sg:rgba(56,189,248,.14);
  --pink:#e879f9;   --pink-glow:rgba(232,121,249,.13);
  --text:#eef0fa; --text-dim:#8892aa; --text-muted:#50596e;
  --r:18px; --rs:11px; --rx:7px;
}
*{margin:0;padding:0;box-sizing:border-box;}
body{background:var(--bg);color:var(--text);font-family:'Plus Jakarta Sans','Noto Sans Bengali',sans-serif;min-height:100vh;overflow-x:hidden;}
body::after{content:'';position:fixed;inset:0;pointer-events:none;z-index:0;
  background:radial-gradient(ellipse 70% 50% at 10% 5%,rgba(124,106,247,.06) 0%,transparent 60%),
             radial-gradient(ellipse 50% 40% at 90% 95%,rgba(45,212,191,.05) 0%,transparent 60%),
             radial-gradient(ellipse 40% 30% at 60% 15%,rgba(240,90,126,.04) 0%,transparent 60%);}
.app{position:relative;z-index:1;max-width:940px;margin:0 auto;padding:0 18px 80px;}

/* ════ TOPBAR ════ */
.topbar{position:sticky;top:0;z-index:100;background:rgba(8,11,17,.88);backdrop-filter:blur(22px);border-bottom:1px solid var(--border);margin:0 -18px 28px;padding:13px 18px;}
.tb-inner{max-width:940px;margin:0 auto;display:flex;align-items:center;justify-content:space-between;gap:10px;flex-wrap:wrap;}
.brand{display:flex;align-items:center;gap:9px;}
.brand-dot{width:8px;height:8px;border-radius:50%;background:linear-gradient(135deg,var(--violet),var(--cyan));box-shadow:0 0 8px var(--violet);}
.brand-name{font-family:'Syne',sans-serif;font-size:12px;font-weight:700;letter-spacing:3px;text-transform:uppercase;color:var(--text-dim);}

/* Step pills */
.step-nav{display:flex;align-items:center;gap:3px;}
.sp{display:flex;align-items:center;gap:6px;padding:6px 14px;border-radius:50px;border:1.5px solid var(--border);font-family:'Syne',sans-serif;font-size:12px;font-weight:700;cursor:pointer;transition:all .25s;color:var(--text-muted);background:transparent;}
.sp:hover{border-color:var(--border2);color:var(--text-dim);}
.sp.active{border-color:var(--violet);color:var(--violet);background:var(--vg);}
.sp.done{border-color:var(--cyan);color:var(--cyan);background:var(--cg);}
.sn{width:18px;height:18px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:800;background:var(--border2);color:var(--text-dim);transition:all .25s;}
.sp.active .sn{background:var(--violet);color:white;}
.sp.done .sn{background:var(--cyan);color:#080b11;}
.sdiv{width:14px;height:1px;background:var(--border);}

/* Header right */
.tb-right{display:flex;align-items:center;gap:7px;}
.date-btn{display:flex;align-items:center;gap:6px;background:var(--surface2);border:1px solid var(--border);padding:6px 13px;border-radius:50px;font-family:'Syne',sans-serif;font-size:12px;font-weight:600;color:var(--sky);cursor:pointer;transition:all .2s;}
.date-btn:hover{border-color:var(--sky);}
.idx-chip{display:flex;align-items:center;gap:5px;background:var(--surface2);border:1px solid var(--border);padding:5px 12px;border-radius:50px;font-family:'Syne',sans-serif;font-size:12px;font-weight:700;cursor:pointer;transition:all .2s;}
.idx-chip:hover{border-color:var(--rose);}
.idx-chip em{color:var(--rose);font-style:normal;}
.idx-chip span{color:var(--text-muted);}

/* ════ CALENDAR POPOVER ════ */
.overlay{position:fixed;inset:0;background:rgba(0,0,0,.65);z-index:998;display:none;backdrop-filter:blur(5px);}
.overlay.open{display:block;}
.cal-pop{position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:var(--surface);border:1px solid var(--border2);border-radius:var(--r);padding:22px;z-index:999;width:300px;box-shadow:0 24px 60px rgba(0,0,0,.6);display:none;}
.cal-pop.open{display:block;animation:popIn .2s ease;}
@keyframes popIn{from{opacity:0;transform:translate(-50%,-53%)}to{opacity:1;transform:translate(-50%,-50%)}}
.cal-nav{display:flex;align-items:center;justify-content:space-between;margin-bottom:16px;}
.cal-nav-btn{width:30px;height:30px;border-radius:50%;border:1px solid var(--border2);background:transparent;color:var(--text-dim);cursor:pointer;font-size:14px;display:flex;align-items:center;justify-content:center;transition:all .2s;}
.cal-nav-btn:hover{border-color:var(--violet);color:var(--violet);}
.cal-month-yr{font-family:'Syne',sans-serif;font-size:14px;font-weight:700;color:var(--text);}
.cal-days-hdr{display:grid;grid-template-columns:repeat(7,1fr);gap:2px;margin-bottom:4px;}
.cal-dh{text-align:center;font-size:10px;font-weight:700;color:var(--text-muted);padding:4px 0;font-family:'Syne',sans-serif;}
.cal-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:3px;}
.cal-day{height:34px;border-radius:8px;display:flex;align-items:center;justify-content:center;font-size:12px;font-weight:600;cursor:pointer;transition:all .2s;color:var(--text-dim);background:transparent;}
.cal-day:hover{background:var(--vg);color:var(--violet);}
.cal-day.today{background:var(--vg);color:var(--violet);border:1px solid rgba(124,106,247,.35);}
.cal-day.selected{background:var(--violet);color:white;box-shadow:0 0 10px rgba(124,106,247,.4);}
.cal-day.other-month{color:var(--text-muted);opacity:.35;}
.cal-bottom{margin-top:14px;display:flex;gap:8px;}
.cal-ok{flex:1;padding:9px;border-radius:var(--rs);border:none;cursor:pointer;background:var(--violet);color:white;font-family:'Syne',sans-serif;font-size:12px;font-weight:800;letter-spacing:1px;text-transform:uppercase;transition:all .2s;}
.cal-ok:hover{background:#8b7bf9;}
.cal-cancel{flex:1;padding:9px;border-radius:var(--rs);border:1px solid var(--border2);cursor:pointer;background:transparent;color:var(--text-dim);font-family:'Syne',sans-serif;font-size:12px;font-weight:700;text-transform:uppercase;transition:all .2s;}
.cal-cancel:hover{border-color:var(--rose);color:var(--rose);}

/* IDX POPOVER */
.idx-pop{position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:var(--surface);border:1px solid var(--border2);border-radius:var(--r);padding:22px 24px;z-index:999;min-width:260px;box-shadow:0 24px 60px rgba(0,0,0,.6);display:none;}
.idx-pop.open{display:block;animation:popIn .2s ease;}
.pop-title{font-family:'Syne',sans-serif;font-size:11px;font-weight:700;letter-spacing:2.5px;text-transform:uppercase;color:var(--text-dim);margin-bottom:13px;}
.pop-inp{width:100%;padding:11px 14px;background:var(--surface2);border:1px solid var(--border2);border-radius:var(--rs);color:var(--text);font-size:18px;font-family:'Syne',sans-serif;font-weight:800;outline:none;margin-bottom:13px;text-align:center;}
.pop-inp:focus{border-color:var(--violet);}
.pop-btns{display:flex;gap:8px;}
.pop-ok{flex:1;padding:9px;border-radius:var(--rs);border:none;cursor:pointer;background:var(--violet);color:white;font-family:'Syne',sans-serif;font-size:12px;font-weight:800;letter-spacing:1px;text-transform:uppercase;}
.pop-cancel{flex:1;padding:9px;border-radius:var(--rs);border:1px solid var(--border2);cursor:pointer;background:transparent;color:var(--text-dim);font-family:'Syne',sans-serif;font-size:12px;font-weight:700;text-transform:uppercase;}

/* ════ PAGES ════ */
.page{display:none;}
.page.active{display:block;animation:pgIn .3s ease;}
@keyframes pgIn{from{opacity:0;transform:translateY(8px)}to{opacity:1;transform:translateY(0)}}
.pg-head{margin-bottom:22px;}
.pg-title{font-family:'Syne',sans-serif;font-size:20px;font-weight:800;background:linear-gradient(135deg,var(--text) 0%,var(--text-dim) 100%);-webkit-background-clip:text;-webkit-text-fill-color:transparent;line-height:1.2;margin-bottom:3px;}
.pg-sub{font-size:12px;color:var(--text-muted);}

/* ════ CARD ════ */
.card{background:var(--surface);border:1px solid var(--border);border-radius:var(--r);padding:18px 20px;position:relative;overflow:hidden;transition:border-color .2s;}
.card:hover{border-color:var(--border2);}
.card::before{content:'';position:absolute;top:0;left:0;right:0;height:2px;border-radius:var(--r) var(--r) 0 0;}
.cv::before{background:linear-gradient(90deg,var(--violet),var(--sky));}
.cs::before{background:linear-gradient(90deg,var(--sky),var(--cyan));}
.cr::before{background:linear-gradient(90deg,var(--rose),var(--amber));}
.ca::before{background:linear-gradient(90deg,var(--amber),var(--rose));}
.cl::before{background:linear-gradient(90deg,var(--lime),var(--cyan));}
.cc::before{background:linear-gradient(90deg,var(--cyan),var(--lime));}
.card-hd{display:flex;align-items:center;gap:10px;margin-bottom:14px;}
.cico{width:32px;height:32px;border-radius:9px;display:flex;align-items:center;justify-content:center;font-size:15px;flex-shrink:0;}
.iv{background:var(--vg);box-shadow:0 0 0 1px rgba(124,106,247,.2);}
.is{background:var(--sg);box-shadow:0 0 0 1px rgba(56,189,248,.2);}
.ir{background:var(--rg);box-shadow:0 0 0 1px rgba(240,90,126,.2);}
.ia{background:var(--ag);box-shadow:0 0 0 1px rgba(251,191,36,.2);}
.il{background:var(--lg);box-shadow:0 0 0 1px rgba(163,230,53,.2);}
.ic{background:var(--cg);box-shadow:0 0 0 1px rgba(45,212,191,.2);}
.ct{font-family:'Syne',sans-serif;font-size:12px;font-weight:700;text-transform:uppercase;letter-spacing:2px;color:var(--text-dim);}
.cd{font-size:11px;color:var(--text-muted);margin-top:1px;}

/* ════ PAGE 1 LAYOUT ════ */
.p1-wrap{display:grid;grid-template-columns:1fr 215px;gap:15px;}
.p1-main{display:flex;flex-direction:column;gap:13px;}
.p1-side{display:flex;flex-direction:column;gap:13px;}

/* Subjects */
.sub-grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(168px,1fr));gap:8px;}
.sub-item{background:var(--surface2);border:1px solid var(--border);border-radius:var(--rs);padding:11px 13px;cursor:pointer;transition:all .2s;}
.sub-item.on{border-color:rgba(124,106,247,.4);background:rgba(124,106,247,.06);}
.sub-name{font-size:13px;font-weight:600;margin-bottom:9px;}
.paper-row{display:flex;gap:7px;flex-wrap:wrap;}
.pck{display:flex;align-items:center;gap:4px;cursor:pointer;font-size:11px;color:var(--text-muted);user-select:none;}
.pck input{display:none;}
.pck .bx{width:16px;height:16px;border-radius:4px;border:1.5px solid var(--border2);display:flex;align-items:center;justify-content:center;transition:all .2s;font-size:8px;background:var(--bg2);flex-shrink:0;}
.pck input:checked+.bx{background:var(--violet);border-color:var(--violet);color:white;}

/* Class chips */
.chips{display:flex;flex-wrap:wrap;gap:7px;}
.chip{padding:6px 13px;border-radius:50px;border:1.5px solid var(--border2);font-size:12px;cursor:pointer;transition:all .2s;background:var(--surface2);color:var(--text-muted);user-select:none;}
.chip:hover{border-color:var(--sky);color:var(--sky);}
.chip.on{border-color:var(--sky);color:var(--sky);background:var(--sg);}

/* Exam rows — always /50 */
.ex-row{display:flex;align-items:center;gap:10px;padding:9px 13px;background:var(--surface2);border:1px solid var(--border);border-radius:var(--rs);margin-bottom:7px;transition:border-color .2s;}
.ex-row.on{border-color:rgba(240,90,126,.3);}
.ex-row label{display:flex;align-items:center;gap:7px;flex:1;cursor:pointer;font-size:13px;}
.ex-row input[type=checkbox]{display:none;}
.ebx{width:18px;height:18px;border-radius:5px;border:1.5px solid var(--border2);display:flex;align-items:center;justify-content:center;transition:all .2s;font-size:8px;flex-shrink:0;background:var(--bg2);}
.ebx.on{background:var(--rose);border-color:var(--rose);color:white;}
.ex-score{display:none;align-items:center;gap:5px;}
.ex-score.show{display:flex;}
.ex-score-inp{width:52px;padding:5px 7px;background:var(--bg);border:1px solid var(--border2);border-radius:var(--rx);color:var(--rose);font-size:13px;font-family:'Syne',sans-serif;font-weight:800;text-align:center;}
.ex-score-inp:focus{outline:none;border-color:var(--rose);}
.ex-denom{font-size:13px;color:var(--text-muted);font-family:'Syne',sans-serif;font-weight:700;}

/* Screen time */
.trow{display:flex;align-items:center;gap:9px;}
.tinp{width:62px;padding:8px 9px;background:var(--surface2);border:1px solid var(--border2);border-radius:var(--rs);color:var(--text);font-size:18px;font-family:'Syne',sans-serif;font-weight:800;text-align:center;}
.tinp:focus{outline:none;border-color:var(--amber);}
.tlbl{font-size:11px;color:var(--text-muted);}
.tsep{font-size:18px;color:var(--text-muted);}

/* Textarea */
.txa{width:100%;padding:10px 13px;background:var(--surface2);border:1px solid var(--border2);border-radius:var(--rs);color:var(--text);font-size:13px;font-family:'Plus Jakarta Sans','Noto Sans Bengali',sans-serif;resize:none;outline:none;transition:border-color .2s;line-height:1.6;}
.txa:focus{border-color:var(--violet);}
.txa.g-amber:focus{border-color:var(--amber);}
.txa.g-lime:focus{border-color:var(--lime);}

/* Prayer sidebar */
.pr-list{display:flex;flex-direction:column;gap:6px;}
.pri{display:flex;align-items:center;justify-content:space-between;padding:8px 11px;background:var(--surface2);border:1px solid var(--border);border-radius:var(--rs);cursor:pointer;transition:all .2s;user-select:none;}
.pri:hover{border-color:var(--lime);}
.pri.done{background:rgba(163,230,53,.06);border-color:rgba(163,230,53,.3);}
.pr-nm{font-size:13px;font-weight:600;}
.pr-tm{font-size:10px;color:var(--text-muted);}
.pr-ic{width:21px;height:21px;border-radius:50%;border:1.5px solid var(--border2);display:flex;align-items:center;justify-content:center;font-size:9px;transition:all .2s;}
.pri.done .pr-ic{background:var(--lime);border-color:var(--lime);color:#080b11;}
.bar-t{background:var(--surface3);border-radius:50px;height:8px;overflow:hidden;margin:7px 0;border:1px solid var(--border);}
.bar-f{height:100%;border-radius:50px;transition:width .6s cubic-bezier(.4,0,.2,1);}
.bf-prayer{background:linear-gradient(90deg,var(--lime),var(--cyan));}
.bf-pct{background:linear-gradient(90deg,var(--violet),var(--rose));}

/* ════ BUTTON ════ */
.btn-main{width:100%;padding:14px;background:linear-gradient(135deg,var(--violet) 0%,var(--rose) 100%);border:none;border-radius:var(--r);color:white;font-family:'Syne',sans-serif;font-size:13px;font-weight:800;letter-spacing:1.5px;text-transform:uppercase;cursor:pointer;transition:all .3s;box-shadow:0 4px 18px rgba(124,106,247,.28);}
.btn-main:hover{box-shadow:0 6px 26px rgba(124,106,247,.42);transform:translateY(-1px);}
.btn-main:active{transform:scale(.98);}
.btn-ghost{padding:10px 20px;background:var(--surface2);border:1px solid var(--border2);border-radius:var(--rs);color:var(--text-dim);font-family:'Syne',sans-serif;font-size:12px;font-weight:700;letter-spacing:1px;text-transform:uppercase;cursor:pointer;transition:all .2s;}
.btn-ghost:hover{border-color:var(--violet);color:var(--violet);}
.btn-cyan{background:linear-gradient(135deg,var(--cyan),var(--violet))!important;}
.nav-row{display:flex;align-items:center;justify-content:space-between;gap:10px;margin-top:16px;flex-wrap:wrap;}

/* ════ PAGE 2 ════ */
.p2-grid{display:grid;grid-template-columns:1fr 1fr;gap:13px;}
.p2-full{grid-column:1/-1;}
.rlbl{font-family:'Syne',sans-serif;font-size:10px;font-weight:700;letter-spacing:2.5px;text-transform:uppercase;color:var(--text-muted);margin-bottom:10px;}

/* Task lists */
.task-list{display:flex;flex-direction:column;gap:7px;}
.task-it{display:flex;align-items:center;gap:9px;padding:10px 13px;background:var(--surface2);border:1px solid var(--border);border-radius:var(--rs);cursor:pointer;transition:all .2s;user-select:none;}
.task-it:hover{border-color:var(--border2);}
.task-it.done{background:rgba(45,212,191,.06);border-color:rgba(45,212,191,.3);}
.task-it.done .tk-txt{text-decoration:line-through;color:var(--text-muted);}
.tk-chk{width:21px;height:21px;border-radius:6px;flex-shrink:0;border:2px solid var(--border2);display:flex;align-items:center;justify-content:center;font-size:10px;transition:all .2s;background:var(--bg2);}
.task-it.done .tk-chk{background:var(--cyan);border-color:var(--cyan);color:#080b11;}
.tk-txt{font-size:13px;font-weight:500;flex:1;line-height:1.3;}

/* Prayer on page 2 — compact row, no glow */
.p2-prayer-row{display:flex;flex-direction:column;gap:6px;}
.p2-pr{display:flex;align-items:center;justify-content:space-between;padding:7px 11px;background:var(--surface2);border:1px solid var(--border);border-radius:var(--rs);cursor:pointer;transition:border-color .2s;user-select:none;}
.p2-pr:hover{border-color:var(--border2);}
.p2-pr.done{border-color:rgba(163,230,53,.28);}
.p2-pr-nm{font-size:12px;font-weight:600;}
.p2-pr-tm{font-size:10px;color:var(--text-muted);}
.p2-pr-ic{width:19px;height:19px;border-radius:50%;border:1.5px solid var(--border2);display:flex;align-items:center;justify-content:center;font-size:9px;transition:all .2s;}
.p2-pr.done .p2-pr-ic{background:var(--lime);border-color:var(--lime);color:#080b11;}

/* Tags */
.tag{display:inline-flex;border-radius:50px;padding:3px 10px;font-size:11px;font-weight:600;margin:2px;}
.tv{background:var(--vg);color:var(--violet);}
.ts{background:var(--sg);color:var(--sky);}
.tr{background:var(--rg);color:var(--rose);}
.ta{background:var(--ag);color:var(--amber);}
.tl{background:var(--lg);color:var(--lime);}
.tc{background:var(--cg);color:var(--cyan);}

/* Screen big */
.scr-big{font-family:'Syne',sans-serif;font-size:30px;font-weight:800;background:linear-gradient(135deg,var(--amber),var(--rose));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}

/* Prayer dots p2 screenshot */
.pdots{display:flex;gap:6px;flex-wrap:wrap;}
.pdot{width:34px;height:34px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:800;font-family:'Syne',sans-serif;transition:all .2s;}
.pdot.on{background:var(--lime);color:#080b11;}
.pdot.off{background:var(--surface2);color:var(--text-muted);border:1px solid var(--border2);}

/* Page 2 screenshot card */
.shot-wrap{display:none;margin-top:14px;}
.shot-close-row{display:flex;justify-content:flex-end;margin-bottom:10px;}
.shot-card{background:linear-gradient(145deg,#0d1018 0%,#111520 50%,#0d1018 100%);border:1px solid var(--border2);border-radius:22px;padding:24px 26px;position:relative;overflow:hidden;box-shadow:0 8px 40px rgba(0,0,0,.45);}
.shot-card::before{content:'';position:absolute;top:-50px;right:-50px;width:180px;height:180px;border-radius:50%;background:radial-gradient(circle,rgba(124,106,247,.1),transparent 70%);}
.shot-card::after{content:'';position:absolute;bottom:-40px;left:-40px;width:150px;height:150px;border-radius:50%;background:radial-gradient(circle,rgba(45,212,191,.07),transparent 70%);}
.si{position:relative;z-index:1;}
.sh-hdr{display:flex;align-items:flex-start;justify-content:space-between;padding-bottom:16px;margin-bottom:16px;border-bottom:1px solid var(--border);}
.sh-brand{font-family:'Syne',sans-serif;font-size:10px;font-weight:700;letter-spacing:3px;text-transform:uppercase;color:var(--text-muted);}
.sh-date{font-family:'Syne',sans-serif;font-size:18px;font-weight:800;background:linear-gradient(135deg,var(--text),var(--sky));-webkit-background-clip:text;-webkit-text-fill-color:transparent;margin-top:2px;}
.sh-idx{font-family:'Syne',sans-serif;font-size:12px;font-weight:700;color:var(--rose);background:var(--rg);padding:4px 12px;border-radius:50px;border:1px solid rgba(240,90,126,.22);}
.sh-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:9px;margin-bottom:9px;}
.sh-mini{background:rgba(255,255,255,.025);border:1px solid var(--border);border-radius:11px;padding:10px 12px;}
.sh-mini.w2{grid-column:span 2;}
.sh-mini.w3{grid-column:span 3;}
.sml{font-family:'Syne',sans-serif;font-size:9px;font-weight:700;letter-spacing:2px;text-transform:uppercase;color:var(--text-muted);margin-bottom:6px;}
.sv{font-family:'Syne',sans-serif;font-size:22px;font-weight:800;}
.sv.pct{background:linear-gradient(135deg,var(--violet),var(--rose));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.sv.scr{background:linear-gradient(135deg,var(--amber),var(--rose));-webkit-background-clip:text;-webkit-text-fill-color:transparent;}
.sh-bar-t{background:var(--surface3);border-radius:50px;height:7px;overflow:hidden;margin:5px 0;border:1px solid var(--border);}
.sh-bar-f{height:100%;border-radius:50px;background:linear-gradient(90deg,var(--violet),var(--rose));}
.sh-pdots{display:flex;gap:5px;}
.sh-pdot{width:26px;height:26px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-size:9px;font-weight:800;font-family:'Syne',sans-serif;}
.sh-pdot.on{background:var(--lime);color:#080b11;}
.sh-pdot.off{background:var(--surface3);color:var(--text-muted);}
.sh-footer{text-align:center;padding-top:12px;border-top:1px solid var(--border);margin-top:3px;font-family:'Syne',sans-serif;font-size:10px;color:var(--text-muted);letter-spacing:2px;text-transform:uppercase;}

/* ════ PAGE 3 ════ */
/* The final screenshot card with glow effect */
.final-shot-card{
  background:linear-gradient(145deg,#0d1018 0%,#111520 50%,#0d1018 100%);
  border:1px solid var(--border2);
  border-radius:22px;padding:26px 28px;
  position:relative;overflow:hidden;
  box-shadow:0 8px 40px rgba(0,0,0,.45);
  margin-bottom:16px;
  transition:box-shadow .6s, border-color .6s;
}
.final-shot-card.glowing{
  border-color:rgba(124,106,247,.6);
  box-shadow:0 0 0 2px rgba(124,106,247,.2), 0 8px 50px rgba(124,106,247,.35), 0 0 80px rgba(45,212,191,.12);
  animation:cardGlow 2s ease;
}
@keyframes cardGlow{
  0%{box-shadow:0 8px 40px rgba(0,0,0,.45);}
  30%{box-shadow:0 0 0 3px rgba(124,106,247,.35), 0 8px 60px rgba(124,106,247,.5), 0 0 100px rgba(45,212,191,.2);}
  100%{box-shadow:0 0 0 2px rgba(124,106,247,.2), 0 8px 50px rgba(124,106,247,.35), 0 0 80px rgba(45,212,191,.12);}
}
.final-shot-card::before{content:'';position:absolute;top:-60px;right:-60px;width:220px;height:220px;border-radius:50%;background:radial-gradient(circle,rgba(124,106,247,.1),transparent 70%);}
.final-shot-card::after{content:'';position:absolute;bottom:-50px;left:-50px;width:180px;height:180px;border-radius:50%;background:radial-gradient(circle,rgba(45,212,191,.07),transparent 70%);}
/* Pct display big for page 3 */
.pct-big{font-family:'Syne',sans-serif;font-size:44px;font-weight:800;background:linear-gradient(135deg,var(--violet),var(--rose));-webkit-background-clip:text;-webkit-text-fill-color:transparent;line-height:1;}
.bar-fat-t{background:var(--surface3);border-radius:50px;height:13px;overflow:hidden;margin:10px 0 7px;border:1px solid var(--border);}
.bar-fat-f{height:100%;border-radius:50px;background:linear-gradient(90deg,var(--violet),var(--rose));transition:width .8s cubic-bezier(.4,0,.2,1);position:relative;}
.bar-fat-f::after{content:'';position:absolute;right:0;top:0;bottom:0;width:4px;background:white;opacity:.3;border-radius:0 50px 50px 0;}

/* Night update */
.night-box{background:var(--surface2);border:1px dashed rgba(124,106,247,.3);border-radius:var(--r);padding:18px 20px;}
.night-ttl{font-family:'Syne',sans-serif;font-size:11px;font-weight:700;letter-spacing:2px;text-transform:uppercase;color:var(--violet);margin-bottom:13px;display:flex;align-items:center;gap:7px;}
/* Exam score update rows */
.eu-row{display:flex;align-items:center;justify-content:space-between;padding:9px 13px;background:var(--surface);border:1px solid var(--border);border-radius:var(--rs);margin-bottom:7px;}
.eu-sub{font-size:13px;}
.eu-score{display:flex;align-items:center;gap:6px;}
.eu-inp{width:56px;padding:6px 8px;background:var(--surface2);border:1px solid var(--border2);border-radius:var(--rx);color:var(--rose);font-size:14px;font-family:'Syne',sans-serif;font-weight:800;text-align:center;}
.eu-inp:focus{outline:none;border-color:var(--rose);}
.eu-denom{font-size:13px;color:var(--text-muted);font-family:'Syne',sans-serif;font-weight:700;}
/* Manual pct in night update */
.nu-row{display:flex;gap:14px;margin-top:13px;flex-wrap:wrap;}
.nu-block{flex:1;min-width:140px;}
.nu-lbl{font-size:11px;color:var(--text-muted);margin-bottom:6px;}
.nu-inp{width:72px;padding:7px 10px;background:var(--surface2);border:1px solid var(--border2);border-radius:var(--rx);color:var(--text);font-size:15px;font-family:'Syne',sans-serif;font-weight:800;text-align:center;}
.nu-inp:focus{outline:none;border-color:var(--violet);}
.nu-sep{color:var(--text-muted);font-size:16px;}
.cmt-note{margin-top:8px;font-size:12px;color:var(--rose);display:none;}

/* Glow hint text */
.glow-hint{text-align:center;padding:8px 0 4px;font-size:12px;color:var(--text-muted);}

@media(max-width:640px){
  .p1-wrap{grid-template-columns:1fr;}
  .p2-grid{grid-template-columns:1fr;}
  .p2-full{grid-column:1;}
  .sh-grid{grid-template-columns:1fr 1fr;}
  .sh-mini.w2,.sh-mini.w3{grid-column:1/-1;}
  .sp span:not(.sn){display:none;}
  .sp{padding:6px 10px;}
}
</style>
</head>
<body>

<div class="overlay" id="ov" onclick="closePops()"></div>

<!-- CALENDAR POPOVER -->
<div class="cal-pop" id="calPop">
  <div class="cal-nav">
    <button class="cal-nav-btn" onclick="calPrev()">‹</button>
    <div class="cal-month-yr" id="calMonthYr"></div>
    <button class="cal-nav-btn" onclick="calNext()">›</button>
  </div>
  <div class="cal-days-hdr">
    <div class="cal-dh">Su</div><div class="cal-dh">Mo</div><div class="cal-dh">Tu</div>
    <div class="cal-dh">We</div><div class="cal-dh">Th</div><div class="cal-dh">Fr</div><div class="cal-dh">Sa</div>
  </div>
  <div class="cal-grid" id="calGrid"></div>
  <div class="cal-bottom">
    <button class="cal-ok" onclick="calSelect()">সিলেক্ট</button>
    <button class="cal-cancel" onclick="closePops()">বাতিল</button>
  </div>
</div>

<!-- IDX POPOVER -->
<div class="idx-pop" id="idxPop">
  <div class="pop-title">নম্বর পরিবর্তন</div>
  <input class="pop-inp" id="idxInp" type="number" min="1">
  <div class="pop-btns">
    <button class="pop-ok" onclick="saveIdx()">সেভ</button>
    <button class="pop-cancel" onclick="closePops()">বাতিল</button>
  </div>
</div>

<div class="app">

  <!-- ══ TOPBAR ══ -->
  <div class="topbar">
    <div class="tb-inner">
      <div class="brand">
        <div class="brand-dot"></div>
        <div class="brand-name">Src To-Do</div>
      </div>
      <div class="step-nav">
        <div class="sp active" id="sp1" onclick="gotoPage(1)"><div class="sn">1</div><span>সিলেক্ট</span></div>
        <div class="sdiv"></div>
        <div class="sp" id="sp2" onclick="gotoPage(2)"><div class="sn">2</div><span>রিভিউ</span></div>
        <div class="sdiv"></div>
        <div class="sp" id="sp3" onclick="gotoPage(3)"><div class="sn">3</div><span>ফাইনাল</span></div>
      </div>
      <div class="tb-right">
        <div class="date-btn" onclick="openCal()" id="dateBtnTxt">📅 লোড...</div>
        <div class="idx-chip" onclick="openIdx()"><span>#</span><em id="idxTxt">1</em></div>
      </div>
    </div>
  </div>

  <!-- ══════════════════════════════════════
       PAGE 1 — INPUT / SELECT
  ══════════════════════════════════════ -->
  <div class="page active" id="page1">
    <div class="pg-head">
      <div class="pg-title">আজকের পরিকল্পনা</div>
      <div class="pg-sub">সব তথ্য পূরণ করুন তারপর পরবর্তী ধাপে যান</div>
    </div>

    <div class="p1-wrap">
      <div class="p1-main">

        <!-- SELF STUDY -->
        <div class="card cv">
          <div class="card-hd"><div class="cico iv">📚</div><div><div class="ct">Self Study</div><div class="cd">সাবজেক্ট ও পত্র বেছে নিন</div></div></div>
          <div class="sub-grid" id="studyGrid"></div>
        </div>

        <!-- ONLINE CLASS -->
        <div class="card cs">
          <div class="card-hd"><div class="cico is">💻</div><div><div class="ct">Online Class</div><div class="cd">আজকের ক্লাস সিলেক্ট করুন</div></div></div>
          <div class="chips" id="classChips"></div>
        </div>

        <!-- PRE-ADMISSION -->
        <div class="card" style="background:var(--surface);border-color:var(--border);" id="preAdmCard">
          <div style="position:absolute;top:0;left:0;right:0;height:2px;border-radius:var(--r) var(--r) 0 0;background:linear-gradient(90deg,var(--pink),var(--violet));"></div>
          <div class="card-hd"><div class="cico" style="background:var(--pink-glow,rgba(232,121,249,.13));box-shadow:0 0 0 1px rgba(232,121,249,.2);">🎯</div><div><div class="ct">Pre-Admission</div><div class="cd">আজকের প্রি-অ্যাডমিশন ক্লাস / পড়া</div></div></div>
          <div class="chips" id="preAdmChips"></div>
        </div>

        <!-- EXAM — always /50 -->
        <div class="card cr">
          <div class="card-hd"><div class="cico ir">📝</div><div><div class="ct">Exam Tracker</div><div class="cd">পরীক্ষা সিলেক্ট করুন — সব পরীক্ষা ৫০ নম্বরের</div></div></div>
          <div id="examList"></div>
        </div>

        <!-- SCREEN TIME -->
        <div class="card ca">
          <div class="card-hd"><div class="cico ia">📱</div><div><div class="ct">Screen Time</div><div class="cd">আজকের স্ক্রিন ব্যবহার</div></div></div>
          <div class="trow">
            <span class="tlbl">ঘণ্টা</span>
            <input type="number" class="tinp" id="sH" min="0" max="24" value="0">
            <span class="tsep">:</span>
            <span class="tlbl">মিনিট</span>
            <input type="number" class="tinp" id="sM" min="0" max="59" value="0">
          </div>
        </div>

        <!-- REWARD -->
        <div class="card cl">
          <div class="card-hd"><div class="cico il">🏆</div><div><div class="ct">Self Reward</div><div class="cd">আজকের পুরস্কার</div></div></div>
          <textarea class="txa g-lime" id="rewardInp" rows="2" placeholder="যেমন: ১ ঘণ্টা গেম খেলা..."></textarea>
        </div>

        <!-- OTHERS -->
        <div class="card ca">
          <div class="card-hd"><div class="cico ia">📌</div><div><div class="ct">Others</div><div class="cd">অন্যান্য কাজ বা নোট</div></div></div>
          <textarea class="txa g-amber" id="othersInp" rows="3" placeholder="যেমন: ডাক্তারের অ্যাপয়েন্টমেন্ট, বইয়ের অর্ডার..."></textarea>
        </div>

        <button class="btn-main" onclick="goPage2()">পরবর্তী ধাপ — রিভিউ করুন →</button>
      </div>

      <!-- PRAYER SIDEBAR (page 1 only) -->
      <div class="p1-side">
        <div class="card cc" style="position:sticky;top:76px;">
          <div class="card-hd"><div class="cico ic">🕌</div><div><div class="ct">ইবাদত</div><div class="cd">৫ ওয়াক্ত নামাজ</div></div></div>
          <div class="pr-list" id="prayerList"></div>
          <div style="height:1px;background:var(--border);margin:12px 0;"></div>
          <div style="font-size:11px;color:var(--text-muted);text-align:center;margin-bottom:5px;"><span id="pCnt">0</span>/5 ওয়াক্ত আদায়</div>
          <div class="bar-t"><div class="bar-f bf-prayer" id="pBar" style="width:0%"></div></div>
        </div>
      </div>
    </div>
  </div>

  <!-- ══════════════════════════════════════
       PAGE 2 — REVIEW + COMPLETE
  ══════════════════════════════════════ -->
  <div class="page" id="page2">
    <div class="pg-head">
      <div class="pg-title">রিভিউ ও সম্পন্ন চিহ্নিত করুন</div>
      <div class="pg-sub">কাজ Done করুন এবং নামাজ আপডেট করুন • Screenshot নিন</div>
    </div>

    <div class="p2-grid">

      <!-- Self Study tasks -->
      <div class="card cv">
        <div class="card-hd"><div class="cico iv">📚</div><div><div class="ct">Self Study</div><div class="cd">পড়া শেষ হলে Done করুন</div></div></div>
        <div class="task-list" id="studyTaskList"></div>
      </div>

      <!-- Online Class tasks -->
      <div class="card cs">
        <div class="card-hd"><div class="cico is">💻</div><div><div class="ct">Online Class</div><div class="cd">ক্লাস শেষ হলে Done করুন</div></div></div>
        <div class="task-list" id="classTaskList"></div>
      </div>

      <!-- Pre-Admission tasks -->
      <div class="card" style="background:var(--surface);border-color:var(--border);" id="preAdmP2Card">
        <div style="position:absolute;top:0;left:0;right:0;height:2px;border-radius:var(--r) var(--r) 0 0;background:linear-gradient(90deg,var(--pink),var(--violet));"></div>
        <div class="card-hd"><div class="cico" style="background:rgba(232,121,249,.13);box-shadow:0 0 0 1px rgba(232,121,249,.2);">🎯</div><div><div class="ct">Pre-Admission</div><div class="cd">শেষ হলে Done করুন</div></div></div>
        <div class="task-list" id="preAdmTaskList"></div>
      </div>

      <!-- EXAM review -->
      <div class="card cr">
        <div class="card-hd"><div class="cico ir">📝</div><div><div class="ct">Exam</div></div></div>
        <div id="r2Exam"></div>
      </div>

      <!-- Prayer on page 2 — compact, no extra glow -->
      <div class="card cc">
        <div class="card-hd"><div class="cico ic">🕌</div><div><div class="ct">ইবাদত</div><div class="cd">নামাজ আদায় হলে টিক করুন</div></div></div>
        <div class="p2-prayer-row" id="p2PrayerList"></div>
        <div style="height:1px;background:var(--border);margin:10px 0;"></div>
        <div style="font-size:11px;color:var(--text-muted);text-align:center;margin-bottom:5px;"><span id="p2PCnt">0</span>/5 ওয়াক্ত</div>
        <div class="bar-t"><div class="bar-f bf-prayer" id="p2PBar" style="width:0%"></div></div>
      </div>

      <!-- Screen time -->
      <div class="card ca">
        <div class="card-hd"><div class="cico ia">📱</div><div><div class="ct">Screen Time</div></div></div>
        <div class="scr-big" id="r2Scr">0h 0m</div>
      </div>

      <!-- Reward -->
      <div class="card cl">
        <div class="card-hd"><div class="cico il">🏆</div><div><div class="ct">Reward</div></div></div>
        <div style="font-size:13px;color:var(--lime);line-height:1.5;" id="r2Reward">—</div>
      </div>

      <!-- Others -->
      <div class="card ca p2-full">
        <div class="card-hd"><div class="cico ia">📌</div><div><div class="ct">Others</div></div></div>
        <div style="font-size:13px;color:var(--amber);line-height:1.5;" id="r2Others">—</div>
      </div>

    </div>

    <!-- Screenshot for page 2 -->
    <div style="margin-top:14px;">
      <button class="btn-main btn-cyan" onclick="toggleShot2()">📸 Screenshot Mode</button>
    </div>
    <div class="shot-wrap" id="shot2Wrap">
      <div class="shot-close-row"><button class="btn-ghost" onclick="toggleShot2()">✕ বন্ধ</button></div>
      <div class="shot-card">
        <div class="si">
          <div class="sh-hdr">
            <div><div class="sh-brand">Src To-Do List</div><div class="sh-date" id="s2Date"></div></div>
            <div class="sh-idx" id="s2Idx"></div>
          </div>
          <div class="sh-grid">
            <div class="sh-mini w2"><div class="sml">📚 Self Study</div><div id="s2Study"></div></div>
            <div class="sh-mini"><div class="sml">💻 Online Class</div><div id="s2Class"></div></div>
            <div class="sh-mini w2"><div class="sml">🎯 Pre-Admission</div><div id="s2PreAdm"></div></div>
            <div class="sh-mini"><div class="sml">📝 Exam</div><div id="s2Exam"></div></div>
            <div class="sh-mini"><div class="sml">📱 Screen</div><div class="sv scr" id="s2Scr"></div></div>
            <div class="sh-mini"><div class="sml">🕌 ইবাদত</div><div class="sh-pdots" id="s2Prayer"></div></div>
            <div class="sh-mini"><div class="sml">🏆 Reward</div><div style="font-size:11px;color:var(--lime);line-height:1.4;" id="s2Reward"></div></div>
            <div class="sh-mini w3"><div class="sml">📌 Others</div><div style="font-size:11px;color:var(--amber);line-height:1.4;" id="s2Others"></div></div>
          </div>
          <div class="sh-footer">Src To-Do List · <span id="s2DateFt"></span></div>
        </div>
      </div>
      <div style="text-align:center;padding:9px 0;font-size:12px;color:var(--text-muted);">👆 এই কার্ডের স্ক্রিনশট নিন</div>
    </div>

    <div class="nav-row">
      <button class="btn-ghost" onclick="gotoPage(1)">← পেছনে</button>
      <button class="btn-main" style="width:auto;padding:12px 26px;" onclick="gotoPage(3)">ফাইনাল পেজে →</button>
    </div>
  </div>

  <!-- ══════════════════════════════════════
       PAGE 3 — FINAL RESULT + NIGHT UPDATE
  ══════════════════════════════════════ -->
  <div class="page" id="page3">
    <div class="pg-head">
      <div class="pg-title">ফাইনাল রিপোর্ট</div>
      <div class="pg-sub">রাতে আপডেট করুন — কার্ডে আলো জ্বলবে 🌟</div>
    </div>

    <!-- FINAL SCREENSHOT CARD (always visible, glows on update) -->
    <div class="final-shot-card" id="finalCard">
      <div class="si">
        <div class="sh-hdr">
          <div><div class="sh-brand">Src To-Do List</div><div class="sh-date" id="sfDate"></div></div>
          <div class="sh-idx" id="sfIdx"></div>
        </div>
        <div class="sh-grid">
          <div class="sh-mini w2"><div class="sml">📚 Self Study</div><div id="sfStudy"></div></div>
          <div class="sh-mini"><div class="sml">💻 Online Class</div><div id="sfClass"></div></div>
          <div class="sh-mini w2"><div class="sml">🎯 Pre-Admission</div><div id="sfPreAdm"></div></div>
          <div class="sh-mini"><div class="sml">📝 Exam & Marks</div><div id="sfExam"></div></div>
          <div class="sh-mini"><div class="sml">🕌 ইবাদত</div><div class="sh-pdots" id="sfPrayer"></div></div>
          <div class="sh-mini"><div class="sml">📱 Screen</div><div class="sv scr" id="sfScr"></div></div>
          <div class="sh-mini"><div class="sml">🏆 Reward</div><div style="font-size:11px;color:var(--lime);line-height:1.4;" id="sfReward"></div></div>
          <div class="sh-mini"><div class="sml">📌 Others</div><div style="font-size:11px;color:var(--amber);line-height:1.4;" id="sfOthers"></div></div>
          <div class="sh-mini w3">
            <div class="sml">📊 Completion Analysis</div>
            <div style="display:flex;align-items:center;gap:12px;">
              <div class="sv pct" id="sfPct">—</div>
              <div style="flex:1;"><div class="sh-bar-t"><div class="sh-bar-f" id="sfBar" style="width:0%"></div></div></div>
            </div>
            <div id="sfCmt" style="font-size:10px;color:var(--rose);margin-top:4px;display:none;"></div>
          </div>
        </div>
        <div class="sh-footer">Src To-Do List · <span id="sfDateFt"></span></div>
      </div>
    </div>
    <div class="glow-hint" id="glowHint">👆 রাতে আপডেট করলে এই কার্ডে আলো জ্বলবে ✨ — তারপর স্ক্রিনশট নিন</div>

    <!-- NIGHT UPDATE BOX -->
    <div class="night-box">
      <div class="night-ttl">🌙 রাতের আপডেট</div>

      <!-- Exam marks update -->
      <div id="nightExamRows"></div>

      <!-- Screen time + completion -->
      <div class="nu-row">
        <div class="nu-block">
          <div class="nu-lbl">📱 স্ক্রিন টাইম আপডেট</div>
          <div style="display:flex;align-items:center;gap:8px;">
            <input type="number" class="nu-inp" id="nSH" placeholder="ঘণ্টা" min="0" max="24">
            <span class="nu-sep">:</span>
            <input type="number" class="nu-inp" id="nSM" placeholder="মিনিট" min="0" max="59">
          </div>
        </div>
        <div class="nu-block">
          <div class="nu-lbl">📊 কমপ্লিশন % আপডেট</div>
          <div style="display:flex;align-items:center;gap:7px;">
            <input type="number" class="nu-inp" id="nPct" placeholder="%" min="0" max="100" oninput="liveNightPct(this.value)">
            <span style="font-size:12px;color:var(--text-muted);">%</span>
          </div>
          <div class="cmt-note" id="cmtNote"></div>
        </div>
      </div>

      <!-- Incomplete reason -->
      <div id="cmtArea" style="margin-top:12px;display:none;">
        <div style="font-size:11px;color:var(--rose);margin-bottom:5px;">⚠️ কেন ১০০% শেষ হয়নি?</div>
        <textarea class="txa" id="cmtInp" rows="2" placeholder="কারণ লিখুন..."></textarea>
      </div>

      <button class="btn-main" style="margin-top:14px;" id="nightSaveBtn" onclick="saveNight()">💾 সেভ করুন ও কার্ড আপডেট করুন</button>
    </div>

    <div style="margin-top:14px;"><button class="btn-ghost" onclick="gotoPage(2)">← রিভিউ পেজে যান</button></div>
  </div>

</div><!-- .app -->

<script>
/* ══ CONSTANTS ══ */
const SUBS=['বাংলা','ইংরেজি','ICT','ইতিহাস','সমাজবিজ্ঞান','ভূগোল','পৌরনীতি'];
const PRAYERS=[{n:'ফজর',t:'ভোর'},{n:'যোহর',t:'দুপুর'},{n:'আসর',t:'বিকাল'},{n:'মাগরিব',t:'সন্ধ্যা'},{n:'ইশা',t:'রাত'}];
const DAYS_BN=['রবিবার','সোমবার','মঙ্গলবার','বুধবার','বৃহস্পতিবার','শুক্রবার','শনিবার'];
const MONTHS_EN=['January','February','March','April','May','June','July','August','September','October','November','December'];
const MONTHS_BN=['জানুয়ারি','ফেব্রুয়ারি','মার্চ','এপ্রিল','মে','জুন','জুলাই','আগস্ট','সেপ্টেম্বর','অক্টোবর','নভেম্বর','ডিসেম্বর'];

/* ══ STATE ══ */
function defState(){
  return{idx:1,selDate:null,study:{},classes:[],preAdm:[],exams:{},sH:0,sM:0,reward:'',others:'',pct:null,cmt:'',prayers:[false,false,false,false,false],tasksDone:{},currentPage:1};
}
let S=(()=>{try{return JSON.parse(localStorage.getItem('srcTodo5'))||defState();}catch{return defState();}})();
function save(){localStorage.setItem('srcTodo5',JSON.stringify(S));}

/* ══ NEW DAY ══ */
(()=>{
  const today=new Date().toDateString();
  const last=localStorage.getItem('srcTodoDay5');
  if(last&&last!==today){const ns=defState();ns.idx=(S.idx||1)+1;S=ns;save();}
  localStorage.setItem('srcTodoDay5',today);
})();

/* ══ DATE ══ */
// selDate stored as {y,m,d} (month 0-indexed)
function getSelDate(){
  if(S.selDate) return new Date(S.selDate.y, S.selDate.m, S.selDate.d);
  return new Date();
}
function getDateStr(){
  const d=getSelDate();
  return DAYS_BN[d.getDay()]+', '+d.getDate()+' '+MONTHS_EN[d.getMonth()];
}
function refreshUI(){
  document.getElementById('dateBtnTxt').textContent='📅 '+getDateStr();
  document.getElementById('idxTxt').textContent=S.idx;
}

/* ══ CALENDAR ══ */
let calViewDate=new Date(); // what month we're viewing
let calPickedDate=getSelDate();

function openCal(){
  calViewDate=new Date(calPickedDate);
  renderCal();
  document.getElementById('calPop').classList.add('open');
  document.getElementById('ov').classList.add('open');
}
function renderCal(){
  document.getElementById('calMonthYr').textContent=MONTHS_EN[calViewDate.getMonth()]+' '+calViewDate.getFullYear();
  const grid=document.getElementById('calGrid');
  grid.innerHTML='';
  const y=calViewDate.getFullYear(), m=calViewDate.getMonth();
  const firstDay=new Date(y,m,1).getDay();
  const daysInMonth=new Date(y,m+1,0).getDate();
  const today=new Date();
  // blanks
  for(let i=0;i<firstDay;i++){
    const bl=document.createElement('div');bl.className='cal-day other-month';grid.appendChild(bl);
  }
  for(let d=1;d<=daysInMonth;d++){
    const el=document.createElement('div');el.className='cal-day';el.textContent=d;
    const isToday=today.getFullYear()===y&&today.getMonth()===m&&today.getDate()===d;
    const isPicked=calPickedDate.getFullYear()===y&&calPickedDate.getMonth()===m&&calPickedDate.getDate()===d;
    if(isPicked) el.classList.add('selected');
    else if(isToday) el.classList.add('today');
    el.onclick=()=>{
      calPickedDate=new Date(y,m,d);
      renderCal();
    };
    grid.appendChild(el);
  }
}
function calPrev(){calViewDate.setMonth(calViewDate.getMonth()-1);renderCal();}
function calNext(){calViewDate.setMonth(calViewDate.getMonth()+1);renderCal();}
function calSelect(){
  S.selDate={y:calPickedDate.getFullYear(),m:calPickedDate.getMonth(),d:calPickedDate.getDate()};
  save(); refreshUI(); closePops();
}

/* ══ IDX ══ */
function openIdx(){
  document.getElementById('idxInp').value=S.idx;
  document.getElementById('idxPop').classList.add('open');
  document.getElementById('ov').classList.add('open');
  setTimeout(()=>document.getElementById('idxInp').focus(),60);
}
function saveIdx(){
  const v=parseInt(document.getElementById('idxInp').value);
  if(v>0){S.idx=v;save();refreshUI();}
  closePops();
}
function closePops(){
  document.querySelectorAll('.cal-pop,.idx-pop').forEach(p=>p.classList.remove('open'));
  document.getElementById('ov').classList.remove('open');
}
document.addEventListener('keydown',e=>{if(e.key==='Escape')closePops();});
document.getElementById('idxInp').addEventListener('keydown',e=>{if(e.key==='Enter')saveIdx();});

/* ══ PAGE NAV ══ */
function gotoPage(n){
  if(n>=2) collectP1();
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.getElementById('page'+n).classList.add('active');
  ['sp1','sp2','sp3'].forEach((id,i)=>{
    const el=document.getElementById(id);
    el.classList.remove('active','done');
    if(i+1===n) el.classList.add('active');
    else if(i+1<n) el.classList.add('done');
  });
  S.currentPage=n; save();
  window.scrollTo({top:0,behavior:'smooth'});
  if(n===2) renderP2();
  if(n===3) renderP3();
}
function goPage2(){collectP1();renderP2();gotoPage(2);}

/* ══ PAGE 1 BUILD ══ */
function buildStudy(){
  const g=document.getElementById('studyGrid'); g.innerHTML='';
  SUBS.forEach(sub=>{
    const d=document.createElement('div');
    d.className='sub-item'+(S.study[sub]&&S.study[sub].length?' on':'');
    d.innerHTML=`<div class="sub-name">${sub}</div>
      <div class="paper-row">
        <label class="pck"><input type="checkbox" data-s="${sub}" data-p="1st" onchange="onStudy()"><span class="bx">✓</span><span>১ম পত্র</span></label>
        <label class="pck"><input type="checkbox" data-s="${sub}" data-p="2nd" onchange="onStudy()"><span class="bx">✓</span><span>২য় পত্র</span></label>
      </div>`;
    g.appendChild(d);
  });
  Object.entries(S.study).forEach(([sub,ps])=>ps.forEach(p=>{
    const cb=g.querySelector(`[data-s="${sub}"][data-p="${p}"]`);
    if(cb) cb.checked=true;
  }));
}

function buildChips(){
  const w=document.getElementById('classChips'); w.innerHTML='';
  SUBS.forEach(sub=>{
    const c=document.createElement('div');
    c.className='chip'+(S.classes.includes(sub)?' on':'');
    c.textContent=sub;
    c.onclick=()=>{S.classes=S.classes.includes(sub)?S.classes.filter(x=>x!==sub):[...S.classes,sub];save();buildChips();};
    w.appendChild(c);
  });
}

const PRE_ADM_SUBS=['বাংলা','English','GK'];
function buildPreAdm(){
  const w=document.getElementById('preAdmChips'); w.innerHTML='';
  PRE_ADM_SUBS.forEach(sub=>{
    const c=document.createElement('div');
    c.className='chip'+(( S.preAdm||[]).includes(sub)?' on':'');
    c.style.cssText=c.classList.contains('on')?'border-color:#e879f9;color:#e879f9;background:rgba(232,121,249,.1);':'';
    c.textContent=sub;
    c.onclick=()=>{
      if(!S.preAdm) S.preAdm=[];
      S.preAdm=S.preAdm.includes(sub)?S.preAdm.filter(x=>x!==sub):[...S.preAdm,sub];
      save(); buildPreAdm();
    };
    // hover pink style for selected
    if((S.preAdm||[]).includes(sub)){
      c.style.borderColor='#e879f9'; c.style.color='#e879f9'; c.style.background='rgba(232,121,249,.1)';
    }
    w.appendChild(c);
  });
}

function buildExam(){
  const el=document.getElementById('examList'); el.innerHTML='';
  SUBS.forEach(sub=>{
    const ck=S.exams.hasOwnProperty(sub);
    const ob=S.exams[sub]||''; // obtained marks
    const row=document.createElement('div');
    row.className='ex-row'+(ck?' on':'');
    row.innerHTML=`
      <label>
        <input type="checkbox" ${ck?'checked':''} onchange="toggleExam(this,'${sub}')">
        <div class="ebx ${ck?'on':''}">✓</div>
        <span style="margin-left:6px">${sub}</span>
      </label>
      <div class="ex-score ${ck?'show':''}">
        <input type="number" class="ex-score-inp" id="ex_${sub}" min="0" max="50" value="${ob}" placeholder="—" oninput="S.exams['${sub}']=this.value;save();">
        <span class="ex-denom">/ 50</span>
      </div>`;
    el.appendChild(row);
  });
}

function toggleExam(cb,sub){
  const row=cb.closest('.ex-row');
  const ebx=row.querySelector('.ebx');
  const score=row.querySelector('.ex-score');
  if(cb.checked){ebx.classList.add('on');score.classList.add('show');row.classList.add('on');S.exams[sub]='';}
  else{ebx.classList.remove('on');score.classList.remove('show');row.classList.remove('on');delete S.exams[sub];}
  save();
}

function buildPrayer(){
  const l=document.getElementById('prayerList'); l.innerHTML='';
  PRAYERS.forEach((p,i)=>{
    const item=document.createElement('div');
    item.className='pri'+(S.prayers[i]?' done':'');
    item.innerHTML=`<div><div class="pr-nm">${p.n}</div><div class="pr-tm">${p.t}</div></div><div class="pr-ic">${S.prayers[i]?'✓':''}</div>`;
    item.onclick=()=>{S.prayers[i]=!S.prayers[i];save();buildPrayer();buildP2Prayer();};
    l.appendChild(item);
  });
  updatePrayerBar('pCnt','pBar');
}

function updatePrayerBar(cntId,barId){
  const done=S.prayers.filter(Boolean).length;
  const el=document.getElementById(cntId);const be=document.getElementById(barId);
  if(el)el.textContent=done;if(be)be.style.width=(done/5*100)+'%';
}

function buildP2Prayer(){
  const l=document.getElementById('p2PrayerList');
  if(!l)return; l.innerHTML='';
  PRAYERS.forEach((p,i)=>{
    const item=document.createElement('div');
    item.className='p2-pr'+(S.prayers[i]?' done':'');
    item.innerHTML=`<div style="display:flex;align-items:center;gap:8px;"><div class="p2-pr-nm">${p.n}</div><div class="p2-pr-tm">${p.t}</div></div><div class="p2-pr-ic">${S.prayers[i]?'✓':''}</div>`;
    item.onclick=()=>{S.prayers[i]=!S.prayers[i];save();buildPrayer();buildP2Prayer();};
    l.appendChild(item);
  });
  updatePrayerBar('p2PCnt','p2PBar');
}

function onStudy(){
  S.study={};
  document.querySelectorAll('#studyGrid input:checked').forEach(cb=>{
    if(!S.study[cb.dataset.s])S.study[cb.dataset.s]=[];
    S.study[cb.dataset.s].push(cb.dataset.p);
  });
  document.querySelectorAll('.sub-item').forEach(item=>{
    const s=item.querySelector('[data-s]')?.dataset.s;
    if(s) item.classList.toggle('on',!!(S.study[s]&&S.study[s].length));
  });
  save();
}

function collectP1(){
  S.sH=parseInt(document.getElementById('sH').value)||0;
  S.sM=parseInt(document.getElementById('sM').value)||0;
  S.reward=document.getElementById('rewardInp').value.trim();
  S.others=document.getElementById('othersInp').value.trim();
  S.cmt=document.getElementById('cmtInp')?.value.trim()||S.cmt;
  save();
}

function restoreP1(){
  document.getElementById('sH').value=S.sH||0;
  document.getElementById('sM').value=S.sM||0;
  document.getElementById('rewardInp').value=S.reward||'';
  document.getElementById('othersInp').value=S.others||'';
}

/* ══ PAGE 2 RENDER ══ */
function tkKey(type,sub,paper){return type+'|'+sub+(paper?'|'+paper:'');}

function renderP2(){
  // Self Study tasks
  const sl=document.getElementById('studyTaskList');
  const studyTasks=[];
  Object.entries(S.study).forEach(([sub,ps])=>ps.forEach(p=>studyTasks.push({key:tkKey('study',sub,p),label:sub+' '+(p==='1st'?'১ম পত্র':'২য় পত্র')})));
  sl.innerHTML=studyTasks.length===0?'<div style="font-size:12px;color:var(--text-muted);font-style:italic;">কোনো সাবজেক্ট সিলেক্ট হয়নি</div>'
    :studyTasks.map(t=>`<div class="task-it ${S.tasksDone[t.key]?'done':''}" onclick="toggleTask('${t.key}')"><div class="tk-chk">${S.tasksDone[t.key]?'✓':''}</div><div class="tk-txt">${t.label}</div></div>`).join('');

  // Class tasks
  const cl=document.getElementById('classTaskList');
  const classTasks=S.classes.map(c=>({key:tkKey('class',c),label:c}));
  cl.innerHTML=classTasks.length===0?'<div style="font-size:12px;color:var(--text-muted);font-style:italic;">কোনো ক্লাস সিলেক্ট হয়নি</div>'
    :classTasks.map(t=>`<div class="task-it ${S.tasksDone[t.key]?'done':''}" onclick="toggleTask('${t.key}')"><div class="tk-chk">${S.tasksDone[t.key]?'✓':''}</div><div class="tk-txt">${t.label}</div></div>`).join('');

  // Pre-Admission tasks
  const pl=document.getElementById('preAdmTaskList');
  const preAdmTasks=(S.preAdm||[]).map(s=>({key:tkKey('preAdm',s),label:s}));
  pl.innerHTML=preAdmTasks.length===0?'<div style="font-size:12px;color:var(--text-muted);font-style:italic;">কোনো সাবজেক্ট সিলেক্ট হয়নি</div>'
    :preAdmTasks.map(t=>`<div class="task-it ${S.tasksDone[t.key]?'done':''}" onclick="toggleTask('${t.key}')"><div class="tk-chk">${S.tasksDone[t.key]?'✓':''}</div><div class="tk-txt">${t.label}</div></div>`).join('');

  // Exam
  const ee=Object.entries(S.exams);
  document.getElementById('r2Exam').innerHTML=ee.length===0?'<span style="font-size:12px;color:var(--text-muted);font-style:italic;">কোনো পরীক্ষা নেই</span>'
    :ee.map(([s,m])=>`<div style="display:flex;justify-content:space-between;padding:4px 0;font-size:13px;border-bottom:1px solid var(--border)"><span>${s}</span><span style="color:var(--rose);font-family:'Syne',sans-serif;font-weight:700;">${m||'—'} / 50</span></div>`).join('');

  document.getElementById('r2Scr').textContent=S.sH+'h '+S.sM+'m';
  document.getElementById('r2Reward').textContent=S.reward||'—';
  document.getElementById('r2Others').textContent=S.others||'—';

  buildP2Prayer();
}

function toggleTask(key){
  S.tasksDone[key]=!S.tasksDone[key]; save(); renderP2();
}

/* ══ PAGE 2 SCREENSHOT ══ */
function toggleShot2(){
  const w=document.getElementById('shot2Wrap');
  const open=w.style.display==='none'||w.style.display==='';
  w.style.display=open?'block':'none';
  if(open) fillShot2();
}
function fillShot2(){
  const ds=getDateStr();
  document.getElementById('s2Date').textContent=ds;
  document.getElementById('s2Idx').textContent='To-Do #'+S.idx;
  document.getElementById('s2DateFt').textContent=ds;

  const se=Object.entries(S.study);
  document.getElementById('s2Study').innerHTML=se.length===0?'<span style="color:var(--text-muted);font-size:10px;">—</span>'
    :se.map(([s,ps])=>ps.map(p=>`<span class="tag tv">${s} ${p==='1st'?'১ম':'২য়'}</span>`).join('')).join('');

  document.getElementById('s2Class').innerHTML=S.classes.length===0?'<span style="color:var(--text-muted);font-size:10px;">—</span>'
    :S.classes.map(c=>`<span class="tag ts">${c}</span>`).join('');

  document.getElementById('s2PreAdm').innerHTML=(S.preAdm||[]).length===0?'<span style="color:var(--text-muted);font-size:10px;">—</span>'
    :(S.preAdm||[]).map(s=>`<span class="tag" style="background:rgba(232,121,249,.13);color:#e879f9;">${s}</span>`).join('');

  const ee=Object.entries(S.exams);
  document.getElementById('s2Exam').innerHTML=ee.length===0?'<span style="color:var(--text-muted);font-size:10px;">—</span>'
    :ee.map(([s,m])=>`<span class="tag tr">${s}${m?' '+m+'/50':''}</span>`).join('');

  document.getElementById('s2Scr').textContent=S.sH+'h '+S.sM+'m';
  document.getElementById('s2Reward').textContent=S.reward||'—';
  document.getElementById('s2Others').textContent=S.others||'—';
  document.getElementById('s2Prayer').innerHTML=PRAYERS.map((p,i)=>`<div class="sh-pdot ${S.prayers[i]?'on':'off'}">${p.n.charAt(0)}</div>`).join('');
}

/* ══ PAGE 3 RENDER ══ */
function renderP3(){
  const ds=getDateStr();
  document.getElementById('sfDate').textContent=ds;
  document.getElementById('sfDateFt').textContent=ds;
  document.getElementById('sfIdx').textContent='To-Do #'+S.idx;

  const se=Object.entries(S.study);
  document.getElementById('sfStudy').innerHTML=se.length===0?'<span style="color:var(--text-muted);font-size:10px;">—</span>'
    :se.map(([s,ps])=>ps.map(p=>`<span class="tag tv">${s} ${p==='1st'?'১ম':'২য়'}</span>`).join('')).join('');

  document.getElementById('sfClass').innerHTML=S.classes.length===0?'<span style="color:var(--text-muted);font-size:10px;">—</span>'
    :S.classes.map(c=>`<span class="tag ts">${c}</span>`).join('');

  document.getElementById('sfPreAdm').innerHTML=(S.preAdm||[]).length===0?'<span style="color:var(--text-muted);font-size:10px;">—</span>'
    :(S.preAdm||[]).map(s=>`<span class="tag" style="background:rgba(232,121,249,.13);color:#e879f9;">${s}</span>`).join('');

  const ee=Object.entries(S.exams);
  document.getElementById('sfExam').innerHTML=ee.length===0?'<span style="color:var(--text-muted);font-size:10px;">—</span>'
    :ee.map(([s,m])=>`<div style="display:flex;justify-content:space-between;font-size:11px;padding:2px 0;border-bottom:1px solid var(--border)"><span style="color:var(--text-dim)">${s}</span><span style="color:var(--rose);font-weight:700;">${m||'—'} / 50</span></div>`).join('');

  document.getElementById('sfScr').textContent=S.sH+'h '+S.sM+'m';
  document.getElementById('sfReward').textContent=S.reward||'—';
  document.getElementById('sfOthers').textContent=S.others||'—';
  document.getElementById('sfPrayer').innerHTML=PRAYERS.map((p,i)=>`<div class="sh-pdot ${S.prayers[i]?'on':'off'}">${p.n.charAt(0)}</div>`).join('');

  // Completion — only show if set
  const pct=S.pct;
  const sfPctEl=document.getElementById('sfPct');
  const sfBarEl=document.getElementById('sfBar');
  if(pct!==null&&pct!==undefined){
    sfPctEl.textContent=pct+'%'; sfBarEl.style.width=pct+'%';
    const cmt=document.getElementById('sfCmt');
    if(pct<100&&S.cmt){cmt.style.display='block';cmt.textContent='⚠️ '+S.cmt;}else cmt.style.display='none';
  } else {
    sfPctEl.textContent='—'; sfBarEl.style.width='0%';
  }

  // Night exam rows
  const nr=document.getElementById('nightExamRows');
  if(ee.length>0){
    nr.innerHTML='<div style="font-size:10px;color:var(--text-muted);margin-bottom:8px;letter-spacing:1px;text-transform:uppercase;font-family:\'Syne\',sans-serif;font-weight:700;">পরীক্ষার মার্কস আপডেট (/ ৫০)</div>'
      +ee.map(([s,m])=>`<div class="eu-row"><span class="eu-sub">${s}</span><div class="eu-score"><input type="number" class="eu-inp" value="${m||''}" min="0" max="50" placeholder="—" oninput="updateExamMark('${s}',this.value)"><span class="eu-denom">/ 50</span></div></div>`).join('');
  } else {
    nr.innerHTML='<div style="font-size:12px;color:var(--text-muted);">কোনো পরীক্ষা নেই</div>';
  }

  document.getElementById('nSH').value=S.sH;
  document.getElementById('nSM').value=S.sM;
  if(S.pct!==null&&S.pct!==undefined) document.getElementById('nPct').value=S.pct;
}

function updateExamMark(sub,val){
  S.exams[sub]=val; save();
  // live update final card
  const ee=Object.entries(S.exams);
  document.getElementById('sfExam').innerHTML=ee.length===0?'<span style="color:var(--text-muted);font-size:10px;">—</span>'
    :ee.map(([s,m])=>`<div style="display:flex;justify-content:space-between;font-size:11px;padding:2px 0;border-bottom:1px solid var(--border)"><span style="color:var(--text-dim)">${s}</span><span style="color:var(--rose);font-weight:700;">${m||'—'} / 50</span></div>`).join('');
  // also update p1 exam input if on page 1
  const el=document.getElementById('ex_'+sub);
  if(el) el.value=val;
}

function liveNightPct(v){
  const p=Math.max(0,Math.min(100,parseInt(v)||0));
  document.getElementById('cmtArea').style.display=p<100?'block':'none';
}

function saveNight(){
  S.sH=parseInt(document.getElementById('nSH').value)||0;
  S.sM=parseInt(document.getElementById('nSM').value)||0;
  S.pct=parseInt(document.getElementById('nPct').value)||0;
  S.cmt=document.getElementById('cmtInp')?.value.trim()||'';
  document.getElementById('sH').value=S.sH;
  document.getElementById('sM').value=S.sM;
  save();
  renderP3();
  // GLOW EFFECT
  const card=document.getElementById('finalCard');
  card.classList.remove('glowing');
  void card.offsetWidth; // reflow
  card.classList.add('glowing');
  const btn=document.getElementById('nightSaveBtn');
  btn.textContent='✅ সেভ হয়েছে! কার্ড আপডেট ✨';
  setTimeout(()=>btn.textContent='💾 সেভ করুন ও কার্ড আপডেট করুন',3000);
}

/* ══ INIT ══ */
refreshUI();
buildStudy();
buildChips();
buildPreAdm();
buildExam();
buildPrayer();
restoreP1();
const cp=S.currentPage||1;
if(cp>1){gotoPage(cp);}
else{document.getElementById('sp1').classList.add('active');}
</script>
</body>
</html>
