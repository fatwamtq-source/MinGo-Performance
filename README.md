# MinGo-Performance
website performance admin Fandiego Travel
<!DOCTYPE html>
<!-- v3: periode dropdown + menu mitra -->
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MinGO Performance Hub — Closing Dashboard</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.0/chart.umd.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
:root{
  --ink:#0B1024; --ink-2:#141A33; --bg:#F3F5F9; --card:#FFFFFF; --line:#E7EAF1;
  --blue:#3B5BFD; --blue-soft:#E4E9FF;
  --green:#3FB27F; --green-soft:#E2F4EC;
  --amber:#F5A623; --amber-soft:#FDF1DA;
  --rose:#E1503C; --rose-soft:#FBE5E1;
  --text:#101528; --muted:#7A8399;
  --radius:18px; --shadow:0 1px 2px rgba(16,21,40,.05),0 8px 24px rgba(16,21,40,.06);
}
*{margin:0;padding:0;box-sizing:border-box}
body{font-family:"Segoe UI",system-ui,-apple-system,Roboto,Arial,sans-serif;background:var(--bg);color:var(--text);font-size:15px}
button{font-family:inherit;cursor:pointer}
input,select{font-family:inherit;font-size:.95rem;padding:10px 13px;border:1.5px solid var(--line);border-radius:12px;background:#fff;width:100%}
input:focus,select:focus{outline:none;border-color:var(--blue);box-shadow:0 0 0 3px rgba(59,91,253,.15)}
label{display:block;font-size:.75rem;font-weight:700;color:var(--muted);margin-bottom:6px;letter-spacing:.02em}

.app{display:flex;min-height:100vh}
.sidebar{width:250px;background:var(--ink);color:#C9D1E4;position:fixed;top:0;bottom:0;left:0;padding:26px 16px;z-index:40}
.brand{display:flex;align-items:center;gap:12px;padding:0 10px 28px}
.brand .m{width:44px;height:44px;border-radius:13px;background:var(--blue);color:#fff;display:flex;align-items:center;justify-content:center;font-weight:800;font-size:1.25rem}
.brand h1{font-size:1.15rem;color:#fff;letter-spacing:.01em}
.brand small{display:block;font-size:.62rem;letter-spacing:.22em;color:#7C87A5;margin-top:2px}
.nav button{display:flex;align-items:center;gap:12px;width:100%;background:none;border:0;border-left:3px solid transparent;color:#AAB4CC;padding:13px 14px;border-radius:12px;font-size:.95rem;text-align:left;margin-bottom:6px}
.nav button:hover{background:rgba(255,255,255,.06);color:#fff}
.nav button.active{background:#1B2340;border-left-color:var(--blue);color:#fff;font-weight:600}
.side-foot{position:absolute;bottom:20px;left:26px;right:16px;font-size:.66rem;color:#5D688A;line-height:1.5}

.main{margin-left:250px;flex:1;min-width:0;padding:30px 36px 70px;max-width:1180px}
.crumb{font-size:.72rem;font-weight:700;letter-spacing:.18em;color:#9AA3B8;text-transform:uppercase}
.topbar{display:flex;justify-content:space-between;align-items:center;gap:14px;flex-wrap:wrap;margin:6px 0 8px}
.topbar h2{font-size:2rem;font-weight:800;letter-spacing:-.01em}
.top-actions{display:flex;gap:12px;align-items:center;flex-wrap:wrap}
.month-sel{display:flex;align-items:center;gap:8px;background:#fff;border:1.5px solid var(--line);border-radius:13px;padding:9px 14px;font-weight:600;font-size:.92rem}
.month-sel select{border:0;padding:0 4px;font-weight:600;box-shadow:none;width:auto}
.month-sel select:focus{box-shadow:none}
.btn{border:0;border-radius:13px;padding:12px 20px;font-size:.93rem;font-weight:700;display:inline-flex;align-items:center;gap:9px}
.btn-blue{background:var(--blue);color:#fff}
.btn-blue:hover{background:#2E4AE0}
.btn-ghost{background:#fff;border:1.5px solid var(--line);color:var(--text)}
.btn-sm{padding:7px 12px;font-size:.8rem;border-radius:9px}
.btn-danger{background:var(--rose-soft);color:var(--rose)}
hr.sep{border:0;border-top:1.5px solid var(--line);margin:14px 0 26px}

.sec-head{display:flex;justify-content:space-between;align-items:flex-end;gap:12px;flex-wrap:wrap;margin-bottom:16px}
.sec-head h3{font-size:1.35rem;font-weight:800}
.sec-head p{color:var(--muted);font-size:.9rem;margin-top:3px}
.chip{background:#fff;border:1.5px solid var(--line);border-radius:12px;padding:8px 14px;font-size:.82rem;font-weight:600;color:var(--muted);display:inline-flex;gap:8px;align-items:center}

.kpi-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(230px,1fr));gap:18px;margin-bottom:22px}
.card{background:var(--card);border:1px solid var(--line);border-radius:var(--radius);box-shadow:var(--shadow);padding:24px 26px}
.kpi .ic{width:46px;height:46px;border-radius:13px;display:flex;align-items:center;justify-content:center;font-size:1.25rem;margin-bottom:14px}
.kpi .t{font-weight:700;font-size:.95rem;color:#3A4157;display:flex;align-items:center;gap:11px;margin-bottom:10px}
.kpi .t .ic{margin:0;width:40px;height:40px;font-size:1.05rem}
.kpi .v{font-size:1.9rem;font-weight:800;letter-spacing:-.01em}
.kpi .s{color:var(--muted);font-size:.85rem;margin-top:6px}
.ic-blue{background:var(--blue-soft);color:var(--blue)}
.ic-green{background:var(--green-soft);color:var(--green)}
.ic-amber{background:var(--amber-soft);color:var(--amber)}
.ic-rose{background:var(--rose-soft);color:var(--rose)}

.compare{background:var(--ink);color:#fff;border:0;margin-bottom:22px}
.compare .eyebrow{font-size:.68rem;font-weight:800;letter-spacing:.2em;color:#7C87A5}
.compare .head{display:flex;justify-content:space-between;align-items:center;gap:10px;flex-wrap:wrap;margin:6px 0 18px}
.compare h3{font-size:1.2rem}
.compare .badge{background:#232C4E;border-radius:11px;padding:8px 14px;font-size:.85rem;font-weight:600;color:#C9D1E4}
.cmp-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(250px,1fr));gap:14px}
.cmp{background:#1A2140;border-radius:14px;padding:16px 18px;display:flex;align-items:center;gap:14px}
.cmp .ic{width:42px;height:42px;border-radius:12px;background:#242E56;display:flex;align-items:center;justify-content:center;font-size:1.1rem}
.cmp .l{font-size:.83rem;color:#AAB4CC}
.cmp .n{font-size:1.35rem;font-weight:800;margin-top:2px}
.cmp .pct{margin-left:auto;border-radius:9px;padding:5px 10px;font-size:.82rem;font-weight:800}
.pct.good{background:rgba(63,178,127,.2);color:#5FD8A4}
.pct.bad{background:rgba(225,80,60,.25);color:#FF9A8B}
.pct.flat{background:#242E56;color:#AAB4CC}

.chart-card{margin-bottom:22px}
.chart-box{position:relative;height:340px;margin-top:14px}
.pills{display:flex;gap:8px;flex-wrap:wrap;margin-top:14px}
.pill{border:1.5px solid #E2E8F2;background:#fff;border-radius:999px;padding:7px 15px;font-size:.82rem;font-weight:600;color:#5B6478;cursor:pointer;transition:all .15s ease;font-family:inherit}
.pill:hover{border-color:#3B5BFD;color:#3B5BFD}
.pill.active{background:#3B5BFD;border-color:#3B5BFD;color:#fff;box-shadow:0 4px 12px rgba(59,91,253,.25)}
.legend-note{color:var(--muted);font-size:.85rem}

.snap-row{display:flex;align-items:center;gap:16px;padding:15px 4px;border-bottom:1.5px solid #F0F2F7}
.snap-row:last-child{border-bottom:0}
.rank{width:28px;height:28px;border-radius:9px;background:#F0F2F7;color:#6A7590;display:flex;align-items:center;justify-content:center;font-weight:800;font-size:.85rem;flex:0 0 28px}
.rank.r1{background:var(--amber-soft);color:var(--amber)}
.ava{width:48px;height:48px;border-radius:50%;display:flex;align-items:center;justify-content:center;font-weight:800;font-size:1.05rem;flex:0 0 48px}
.snap-row .who b{font-size:1.02rem}
.snap-row .who span{display:block;color:var(--muted);font-size:.82rem;margin-top:2px}
.snap-row .big{margin-left:auto;font-size:1.5rem;font-weight:800}
.snap-row .growth{font-size:.8rem;font-weight:700;margin-left:14px;min-width:64px;text-align:right}
.g-up{color:var(--green)} .g-down{color:var(--rose)} .g-flat{color:var(--muted)}

.tbl-wrap{overflow-x:auto}
table{width:100%;border-collapse:collapse;font-size:.9rem}
thead th{text-align:left;padding:11px 13px;color:var(--muted);font-size:.72rem;text-transform:uppercase;letter-spacing:.07em;border-bottom:2px solid var(--line);white-space:nowrap}
tbody td{padding:11px 13px;border-bottom:1.5px solid #F0F2F7;white-space:nowrap}
td.num,th.num{text-align:right}
tbody tr:hover{background:#F8FAFD}
.empty{color:var(--muted);text-align:center;padding:26px 8px;font-size:.9rem}

.modal-bg{position:fixed;inset:0;background:rgba(11,16,36,.55);display:none;align-items:center;justify-content:center;z-index:60;padding:18px}
.modal-bg.show{display:flex}
.modal{background:#fff;border-radius:20px;max-width:520px;width:100%;max-height:92vh;overflow-y:auto;padding:28px}
.modal h3{font-size:1.3rem;margin-bottom:4px}
.modal .sub{color:var(--muted);font-size:.87rem;margin-bottom:18px}
.form-grid{display:grid;grid-template-columns:1fr 1fr;gap:14px}
.form-grid .full{grid-column:1/-1}
.modal .foot{display:flex;justify-content:flex-end;gap:10px;margin-top:20px}
.hint{font-size:.78rem;color:var(--muted);margin-top:6px}

#toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);background:var(--ink);color:#fff;padding:12px 22px;border-radius:999px;font-size:.88rem;box-shadow:var(--shadow);opacity:0;pointer-events:none;transition:opacity .3s;z-index:99}
#toast.show{opacity:1}
.view{display:none}.view.active{display:block}

#menuBtn{display:none;position:fixed;top:14px;left:14px;z-index:50;background:var(--ink);color:#fff;border:0;border-radius:10px;padding:9px 13px;font-size:1rem}
@media(max-width:920px){
  #menuBtn{display:block}
  .sidebar{transform:translateX(-100%);transition:transform .25s}
  .sidebar.open{transform:translateX(0)}
  .main{margin-left:0;padding:66px 16px 60px}
  .topbar h2{font-size:1.5rem}
  .form-grid{grid-template-columns:1fr}
}
@media print{ .sidebar,#menuBtn,.top-actions,.modal-bg,#toast{display:none!important} .main{margin:0} }
.leads-form-grid{display:grid;grid-template-columns:repeat(5,minmax(120px,1fr));gap:10px;margin-bottom:14px}
@media(max-width:800px){.leads-form-grid{grid-template-columns:1fr 1fr}.leads-form-grid>:first-child{grid-column:1/-1}}
@media(max-width:480px){.leads-form-grid{grid-template-columns:1fr}.leads-form-grid>:first-child{grid-column:auto}}
</style>
</head>
<body>

<button id="menuBtn" onclick="document.querySelector('.sidebar').classList.toggle('open')">☰</button>
<div class="app">
<aside class="sidebar">
  <div class="brand">
    <div class="m">M</div>
    <div><h1>MinGO</h1><small>PERFORMANCE HUB</small></div>
  </div>
  <nav class="nav" id="nav">
    <button data-view="dash" class="active">▦&nbsp; Dashboard</button>
    <button data-view="sales">👥&nbsp; Sales performance</button>
    <button data-view="mitra">🤝&nbsp; Mitra</button>
    <button data-view="leads">◎&nbsp; Leads</button>
    <button data-view="data">🗂&nbsp; Data</button>
  </nav>
  <div class="side-foot">Fandiego Travel · Monitoring closing tim MinGo</div>
</aside>

<main class="main">
  <div class="crumb">OVERVIEW / CLOSING</div>
  <div class="topbar">
    <h2 id="pageTitle">Closing dashboard</h2>
    <div class="top-actions">
      <span class="month-sel">⏱ <select id="periodSel">
        <option value="week">Mingguan</option>
        <option value="month" selected>Bulanan</option>
        <option value="year">Tahunan</option>
      </select></span>
      <span class="month-sel">🗓 <select id="monthView"></select></span>
      <button class="btn btn-blue" onclick="openModal()">＋ Tambah data</button>
    </div>
  </div>
  <hr class="sep">

  <!-- ================= DASHBOARD ================= -->
  <section class="view active" id="view-dash">
    <div class="sec-head">
      <div><h3 id="perfTitle">Monthly performance</h3><p>Pantau closing tim MinGo secara ringkas.</p></div>
      <span class="chip">⛃ <span id="periodCount">0 periode tercatat</span></span>
    </div>

    <div class="kpi-grid" id="kpiGrid"></div>

    <div class="card chart-card" id="weeklyCard">
      <div class="sec-head" style="margin-bottom:14px">
        <div><h3 style="font-size:1.15rem">Ringkasan mingguan</h3><p>Total closing, ads spent, dan poin mitra per minggu (estimasi bulan dibagi 4)</p></div>
        <button class="btn btn-blue" onclick="exportWeeklyPDF()">⬇ Unduh laporan mingguan (PDF)</button>
      </div>
      <div class="kpi-grid" id="weeklyKpi"></div>
      <table id="weeklyTable"></table>
    </div>


    <div class="card compare">
      <div class="eyebrow">PERIOD COMPARISON</div>
      <div class="head"><h3>Perubahan dari periode sebelumnya</h3><span class="badge" id="cmpBadge">—</span></div>
      <div class="cmp-grid" id="cmpGrid"></div>
    </div>

    <div class="card chart-card">
      <div class="sec-head" style="margin-bottom:0">
        <div><h3 style="font-size:1.15rem">Pertumbuhan akumulasi closing</h3><p>Total closing berjalan (kumulatif) dari bulan ke bulan setiap MinGo</p></div>
      </div>
      <div class="chart-box"><canvas id="chCumul"></canvas></div>
    </div>

    <div class="card chart-card">
      <div class="sec-head" style="margin-bottom:0">
        <div><h3 style="font-size:1.15rem">Detail closing per MinGo</h3><p>Pilih satu bulan, tombol <b>Semua</b> (grafik garis seluruh bulan), atau klik titik di grafik akumulasi di atas</p></div>
      </div>
      <div class="pills" id="dashPills"></div>
      <div class="chart-box"><canvas id="chMonthly"></canvas></div>
      <div class="sec-head" style="margin:20px 0 0">
        <div><h3 style="font-size:1.05rem">Rekap closing per bulan</h3><p>Tren closing tiap MinGo dari bulan ke bulan (bukan akumulasi)</p></div>
      </div>
      <div class="chart-box"><canvas id="chRecap"></canvas></div>
    </div>

    <div class="card chart-card">
      <div class="sec-head" style="margin-bottom:0">
        <div><h3 style="font-size:1.15rem">Ads spent per bulan &amp; akumulasi</h3><p>Biaya iklan bulanan (batang) dan total berjalan (garis)</p></div>
      </div>
      <div class="chart-box"><canvas id="chAds"></canvas></div>
    </div>

    <div class="card">
      <div class="sec-head" style="margin-bottom:4px">
        <div><h3 style="font-size:1.15rem">Team snapshot</h3><p id="snapSub">Sales dengan closing terbanyak</p></div>
        <select id="snapMode" style="width:auto;font-weight:600;padding:9px 12px"></select>
      </div>
      <div id="teamSnap"></div>
    </div>
  </section>

  <!-- ================= SALES PERFORMANCE ================= -->
  <section class="view" id="view-sales">
    <div class="sec-head">
      <div><h3>Sales performance</h3><p>Detail pertumbuhan akumulasi dan perolehan bulanan setiap MinGo.</p></div>
    </div>
    <div class="card chart-card">
      <div class="sec-head" style="margin-bottom:0"><div><h3 style="font-size:1.15rem">Akumulasi closing</h3><p>Total berjalan per MinGo</p></div></div>
      <div class="chart-box" style="height:380px"><canvas id="chCumul2"></canvas></div>
    </div>
    <div class="card tbl-wrap" style="margin-bottom:22px">
      <div class="sec-head" style="margin-bottom:6px"><div><h3 style="font-size:1.1rem">Closing per bulan</h3></div></div>
      <table id="monthlyTable"></table>
    </div>
    <div class="card tbl-wrap">
      <div class="sec-head" style="margin-bottom:6px"><div><h3 style="font-size:1.1rem">Akumulasi per bulan</h3><p>Angka kumulatif (total berjalan)</p></div></div>
      <table id="cumulTable"></table>
    </div>
  </section>

  <!-- ================= MITRA ================= -->
  <section class="view" id="view-mitra">
    <div class="sec-head">
      <div><h3>Performa Mitra</h3><p>Closing Mitra Fitri, Mitra Ashilah, Mitra Nay, dan Mitra Depok. Data tersimpan otomatis dan tersinkron di semua perangkat.</p></div>
    </div>

    <div class="kpi-grid" id="mitraKpi"></div>

    <div class="card tbl-wrap" style="margin-bottom:22px">
      <div class="sec-head" style="margin-bottom:6px"><div><h3 style="font-size:1.1rem">Input closing mitra</h3><p>Pilih bulan & mitra yang sama untuk memperbarui data.</p></div></div>
      <div style="display:flex;gap:10px;flex-wrap:wrap;margin-bottom:14px">
        <input type="month" id="mitraMonth" style="width:auto">
        <select id="mitraName" style="width:auto"></select>
        <input type="number" id="mitraClosing" min="0" placeholder="Jumlah closing" style="width:auto;flex:1;min-width:150px">
        <button class="btn btn-blue btn-sm" onclick="saveMitra()">Simpan</button>
      </div>
      <table id="mitraTable"></table>
    </div>

    <div class="card chart-card">
      <div class="sec-head" style="margin-bottom:0">
        <div><h3 style="font-size:1.15rem">Detail closing per mitra</h3><p>Pilih satu bulan — atau klik titik di grafik akumulasi di bawah</p></div>
      </div>
      <div class="pills" id="mitraPills"></div>
      <div class="chart-box"><canvas id="chMitra"></canvas></div>
    </div>

    <div class="card chart-card">
      <div class="sec-head" style="margin-bottom:0">
        <div><h3 style="font-size:1.15rem">Akumulasi closing mitra</h3><p>Total berjalan per mitra</p></div>
      </div>
      <div class="chart-box"><canvas id="chMitraCumul"></canvas></div>
    </div>
  </section>

  <!-- ================= LEADS ================= -->
  <section class="view" id="view-leads">
    <div class="sec-head">
      <div><h3>Leads MinGo</h3><p>Catat jumlah leads Cold, Warm, dan Hot setiap MinGo per bulan.</p></div>
    </div>
    <div class="kpi-grid" id="leadsKpi"></div>
    <div class="card tbl-wrap" style="margin-bottom:22px">
      <div class="sec-head" style="margin-bottom:6px"><div><h3 style="font-size:1.1rem">Input leads</h3><p>Bulan dan MinGo yang sama akan memperbarui data sebelumnya.</p></div></div>
      <div class="leads-form-grid">
        <input type="month" id="leadsMonth">
        <select id="leadsAdmin"></select>
        <input type="number" id="leadsCold" min="0" step="1" placeholder="Cold">
        <input type="number" id="leadsWarm" min="0" step="1" placeholder="Warm">
        <input type="number" id="leadsHot" min="0" step="1" placeholder="Hot">
      </div>
      <button class="btn btn-blue btn-sm" onclick="saveLeads()">Simpan leads</button>
      <table id="leadsTable" style="margin-top:16px"></table>
    </div>
    <div class="card chart-card">
      <div class="sec-head" style="margin-bottom:0"><div><h3 style="font-size:1.15rem">Tren leads bulanan</h3><p>Perbandingan Cold, Warm, dan Hot dari bulan ke bulan.</p></div></div>
      <div class="chart-box"><canvas id="chLeads"></canvas></div>
    </div>
  </section>

  <!-- ================= DATA ================= -->
  <section class="view" id="view-data">
    <div class="sec-head">
      <div><h3>Data tercatat</h3><p>Klik untuk edit atau hapus. Data tersimpan otomatis dan tersinkron di semua perangkat.</p></div>
      <span style="display:flex;gap:8px;flex-wrap:wrap">
        <button class="btn btn-ghost btn-sm" onclick="seedData(true)">↺ Muat ulang data awal</button>
        <button class="btn btn-danger btn-sm" onclick="clearAll()">Hapus semua</button>
      </span>
    </div>

    <div class="card" style="margin-bottom:22px">
      <div class="sec-head" style="margin-bottom:10px">
        <div><h3 style="font-size:1.1rem">Backup database</h3><p>Simpan seluruh data (closing, ads spent, mitra, target) ke file, atau pulihkan dari file backup.</p></div>
      </div>
      <div style="display:flex;gap:8px;flex-wrap:wrap;align-items:center">
        <button class="btn btn-blue btn-sm" onclick="backupJSON()">💾 Unduh backup (.json)</button>
        <button class="btn btn-ghost btn-sm" onclick="backupCSV()">📄 Unduh CSV</button>
        <button class="btn btn-ghost btn-sm" onclick="document.getElementById('restoreFile').click()">⬆️ Pulihkan dari file</button>
        <button class="btn btn-ghost btn-sm" onclick="restoreAuto()">↩️ Pulihkan cadangan otomatis</button>
        <input type="file" id="restoreFile" accept="application/json,.json" style="display:none" onchange="restoreJSON(this)">
      </div>
      <p class="empty" id="backupInfo" style="text-align:left;margin-top:12px"></p>
    </div>
    <div class="card tbl-wrap"><table id="rawTable"></table></div>

    <div class="card tbl-wrap" style="margin-top:22px">
      <div class="sec-head" style="margin-bottom:6px"><div><h3 style="font-size:1.1rem">Ads spent per bulan</h3><p>Biaya iklan dalam Rupiah. Pilih bulan yang sama untuk memperbarui nilai.</p></div></div>
      <div style="display:flex;gap:10px;flex-wrap:wrap;margin-bottom:14px">
        <input type="month" id="adsMonth" style="width:auto">
        <input type="number" id="adsAmount" min="0" placeholder="Jumlah (Rp)" style="width:auto;flex:1;min-width:170px">
        <button class="btn btn-blue btn-sm" onclick="saveAds()">Simpan ads spent</button>
      </div>
      <table id="adsTable"></table>
    </div>
  </section>
</main>
</div>

<!-- Modal Tambah data -->
<div class="modal-bg" id="modal">
  <div class="modal">
    <h3 id="modalTitle">Tambah data</h3>
    <div class="sub">Input jumlah closing per MinGo untuk satu bulan. Jika bulan & MinGo sama, data lama akan diperbarui.</div>
    <input type="hidden" id="fId">
    <div class="form-grid">
      <div><label>Bulan</label><input type="month" id="fMonth"></div>
      <div><label>MinGo</label><select id="fAdmin"></select></div>
      <div><label>Jumlah closing</label><input type="number" id="fClosing" min="0" placeholder="0"></div>
      <div><label>Target closing tim / bulan</label><input type="number" id="fTarget" min="0"></div>
    </div>
    <div class="hint">Target dipakai untuk menghitung target achievement seluruh tim.</div>
    <div class="foot">
      <button class="btn btn-ghost" onclick="closeModal()">Batal</button>
      <button class="btn btn-blue" onclick="saveEntry()">Simpan</button>
    </div>
  </div>
</div>
<div id="toast"></div>

<script>
/* ===== MinGO Performance Hub — closing only, growth & akumulasi ===== */
const Store=(()=>{let mem={},usable=true;
  try{const k='__t__';localStorage.setItem(k,'1');localStorage.removeItem(k);}catch(e){usable=false;}
  return{usable,get:k=>{try{return usable?localStorage.getItem(k):(mem[k]??null);}catch(e){return mem[k]??null;}},
    set:(k,v)=>{try{if(usable){localStorage.setItem(k,v);return;}}catch(e){}mem[k]=v;}};})();
const KEY='mingo_performance_hub_v2';
const ADMINS=['MinGo Yeni','MinGo Handa','MinGo Bian','MinGo Farisa','MinGo Fira'];
const COLORS={'MinGo Yeni':'#3B5BFD','MinGo Handa':'#3FB27F','MinGo Bian':'#F5A623','MinGo Farisa':'#9B59D0','MinGo Fira':'#E1503C'};
const MITRAS=['Mitra Fitri','Mitra Ashilah','Mitra Nay','Mitra Depok'];
const MCOLORS={'Mitra Fitri':'#3B5BFD','Mitra Ashilah':'#3FB27F','Mitra Nay':'#F5A623','Mitra Depok':'#9B59D0'};
const PER_LABEL={week:'minggu',month:'bulan',year:'tahun'};
const MON=['Jan','Feb','Mar','Apr','Mei','Juni','Juli','Agustus','Sep','Okt','Nov','Des'];
const MON_S=['Jan','Feb','Mar','Apr','Mei','Jun','Jul','Agu','Sep','Okt','Nov','Des'];

let DB={entries:[],target:60,ads:{},mitra:[],leads:[]}; // entries/mitra: {id, month:'2026-08', admin/mit, closing}; ads: {'2026-08': 2500000}
const fmtRp=n=>'Rp '+Math.round(n||0).toLocaleString('id-ID');
const fmtRpShort=v=>v>=1e9?'Rp '+(v/1e9).toLocaleString('id-ID',{maximumFractionDigits:1})+' M':v>=1e6?'Rp '+(v/1e6).toLocaleString('id-ID',{maximumFractionDigits:1})+' jt':v>=1e3?'Rp '+Math.round(v/1e3)+' rb':'Rp '+v;
let charts={};
const $=id=>document.getElementById(id);
const uid=()=>Date.now().toString(36)+Math.random().toString(36).slice(2,6);
const short=a=>a.replace('MinGo ','');
const monLabel=m=>{const[y,mm]=m.split('-');return MON[Number(mm)-1]+' '+y;};
const monShort=m=>{const[y,mm]=m.split('-');return MON_S[Number(mm)-1];};
function toast(t){const e=$('toast');e.textContent=t;e.classList.add('show');clearTimeout(e._h);e._h=setTimeout(()=>e.classList.remove('show'),2500);}
const BKEY='mingo_backups_v1';
const baks=()=>{try{return JSON.parse(Store.get(BKEY)||'[]');}catch(e){return [];}};
function snapshotRaw(raw){try{const l=baks();l.push({at:new Date().toISOString(),raw});while(l.length>5)l.shift();Store.set(BKEY,JSON.stringify(l));}catch(e){}}
const save=()=>{const prev=Store.get(KEY);if(prev)snapshotRaw(prev);Store.set(KEY,JSON.stringify(DB));renderBackupInfo();window.parent.postMessage({type:'mingo-save',data:DB},location.origin);};
function load(){const r=Store.get(KEY);if(r){try{DB=Object.assign(DB,JSON.parse(r));}catch(e){}}if(!DB.entries)DB.entries=[];if(!DB.target)DB.target=60;if(!DB.ads)DB.ads={};if(!DB.mitra)DB.mitra=[];if(!DB.leads)DB.leads=[];}

const months=()=>[...new Set(DB.entries.map(e=>e.month))].sort();
const sum=(arr,f)=>arr.reduce((t,e)=>t+(e[f]||0),0);
const amc=(a,m)=>sum(DB.entries.filter(e=>e.month===m&&e.admin===a),'closing'); // admin-month closing
const teamMonth=m=>sum(DB.entries.filter(e=>e.month===m),'closing');
function cumulSeries(a){const ms=months();let t=0;return ms.map(m=>t+=amc(a,m));}
function scopeEntries(){const v=$('monthView').value;return v==='all'?DB.entries:DB.entries.filter(e=>e.month===v);}

/* ---------- Agregasi periode: minggu / bulan / tahun ---------- */
const periodMode=()=>{const el=$('periodSel');return el?el.value:'month';};
function scopeMonths(){
  const v=$('monthView').value;
  const ms=[...new Set([...months(),...Object.keys(DB.ads),...DB.mitra.map(e=>e.month),...DB.leads.map(e=>e.month)])].sort();
  return v==='all'?ms:ms.filter(m=>m===v);
}
// buckets: [{key,label,short,months:[...],div}] — div membagi nilai bulan (mode minggu: bulan dibagi 4 sebagai estimasi)
function buckets(){
  const mode=periodMode();const ms=scopeMonths();
  if(mode==='year'){
    const ys=[...new Set(ms.map(m=>m.slice(0,4)))].sort();
    return ys.map(y=>({key:y,label:'Tahun '+y,short:String(y),months:ms.filter(m=>m.startsWith(y)),div:1}));
  }
  if(mode==='week'){
    const out=[];
    ms.forEach(m=>{for(let w=1;w<=4;w++)out.push({key:m+'-W'+w,label:'Minggu '+w+' '+monLabel(m),short:monShort(m)+' Mg'+w,months:[m],div:4});});
    return out;
  }
  return ms.map(m=>({key:m,label:monLabel(m),short:monShort(m),months:[m],div:1}));
}
const bVal=(a,b)=>b.months.reduce((t,m)=>t+amc(a,m),0)/b.div;
const bTeam=b=>b.months.reduce((t,m)=>t+teamMonth(m),0)/b.div;
const bAds=b=>b.months.reduce((t,m)=>t+(DB.ads[m]||0),0)/b.div;
const bTarget=b=>DB.target*b.months.length/b.div;

/* ---------- Render ---------- */
function fillMonthView(){
  const sel=$('monthView');const cur=sel.value;
  sel.innerHTML='<option value="all">Monthly view — semua</option>'+months().map(m=>`<option value="${m}">${monLabel(m)}</option>`).join('');
  if([...sel.options].some(o=>o.value===cur))sel.value=cur;
}
const kpiHTML=(ic,cls,title,val,sub)=>`<div class="card kpi"><div class="t"><span class="ic ${cls}">${ic}</span>${title}</div><div class="v">${val}</div><div class="s">${sub}</div></div>`;
function renderKPI(){
  const es=scopeEntries();const msAll=months();const bs=buckets();const per=PER_LABEL[periodMode()];
  const closing=sum(es,'closing');
  const avg=bs.length?bs.reduce((t,b)=>t+bTeam(b),0)/bs.length:null;
  const target=bs.reduce((t,b)=>t+bTarget(b),0);
  const ach=target>0?closing/target*100:null;
  // growth total tim periode terakhir vs sebelumnya
  let gTxt='—',gSub='Butuh ≥ 2 periode';
  if(bs.length>=2){
    const p=bTeam(bs[bs.length-2]),c=bTeam(bs[bs.length-1]);
    if(p>0){const d=(c-p)/p*100;gTxt=(d>0?'+':'')+d.toFixed(1)+'%';gSub=bs[bs.length-2].short+' → '+bs[bs.length-1].short+' ('+(Math.round(p*10)/10)+' → '+(Math.round(c*10)/10)+')';}
  }
  $('kpiGrid').innerHTML=
    kpiHTML('📈','ic-blue','Total closing',closing.toLocaleString('id-ID'),(bs.length||0)+' '+per+' terhitung')+
    kpiHTML('📊','ic-green','Rata-rata per '+per,avg!==null?avg.toLocaleString('id-ID',{maximumFractionDigits:1}):'—','Closing per '+per)+
    kpiHTML('🚀','ic-amber','Growth '+per+' terakhir',gTxt,gSub)+
    kpiHTML('🎯','ic-rose','Target achievement',ach!==null?ach.toLocaleString('id-ID',{maximumFractionDigits:1})+'%':'—','Target '+target.toLocaleString('id-ID')+' closing')+
    kpiHTML('💸','ic-amber','Ads spent',fmtRp(bs.reduce((t,b)=>t+bAds(b),0)),(bs.length||0)+' '+per+' terhitung')+
    kpiHTML('🧾','ic-blue','Akumulasi ads spent',fmtRp(Object.values(DB.ads).reduce((t,v)=>t+v,0)),'Total seluruh periode');
  $('periodCount').textContent=msAll.length+' bulan tercatat';
  const v=$('monthView').value;
  $('perfTitle').textContent=(periodMode()==='year'?'Yearly performance':periodMode()==='week'?'Weekly performance (estimasi)':'Monthly performance')+(v==='all'?'':' — '+monLabel(v));
}
function pctPill(d,invert){
  if(d===null||isNaN(d))return '<span class="pct flat">—</span>';
  const good=invert?d<0:d>0;
  const cls=Math.abs(d)<0.5?'flat':good?'good':'bad';
  return `<span class="pct ${cls}">${d>0?'+':''}${d.toFixed(1)}%</span>`;
}
function renderCompare(){
  const bs=buckets();const g=$('cmpGrid');const per=PER_LABEL[periodMode()];
  if(bs.length<2){$('cmpBadge').textContent='Butuh ≥ 2 periode';g.innerHTML='<div class="cmp"><div class="l">Belum cukup data untuk perbandingan. Input minimal 2 '+per+'.</div></div>';return;}
  const a=bs[bs.length-2],b=bs[bs.length-1];
  $('cmpBadge').textContent=a.short+' → '+b.short;
  const pT=bTeam(a),cT=bTeam(b);const d=Math.round((cT-pT)*10)/10;const p=pT>0?(cT-pT)/pT*100:null;
  // top MinGo periode terakhir & kenaikan terbesar
  const pm=ADMINS.map(x=>({x,c:bVal(x,b),d:bVal(x,b)-bVal(x,a)}));
  const top=[...pm].sort((u,v)=>v.c-u.c)[0];
  const rise=[...pm].sort((u,v)=>v.d-u.d)[0];
  g.innerHTML=
    `<div class="cmp"><span class="ic">📈</span><div><div class="l">Closing vs ${per} sebelumnya</div><div class="n">${d>0?'+':''}${d}</div></div>${pctPill(p,false)}</div>`+
    `<div class="cmp"><span class="ic">🏆</span><div><div class="l">Top MinGo ${b.short}</div><div class="n">${short(top.x)} · ${Math.round(top.c*10)/10}</div></div></div>`+
    `<div class="cmp"><span class="ic">🚀</span><div><div class="l">Kenaikan terbesar</div><div class="n">${short(rise.x)} ${rise.d>0?'+':''}${Math.round(rise.d*10)/10}</div></div></div>`;
}
function destroy(id){if(charts[id]){charts[id].destroy();delete charts[id];}}
function mk(id,cfg){destroy(id);const el=$(id);if(el)charts[id]=new Chart(el.getContext('2d'),cfg);}
const baseLine=(datasets,labels,yTitle)=>({type:'line',
  data:{labels,datasets},
  options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
    plugins:{legend:{position:'bottom',labels:{usePointStyle:true,pointStyle:'circle',padding:18,font:{weight:'600'}}},
      tooltip:{callbacks:{label:c=>' '+c.dataset.label+': '+(Math.round(c.parsed.y*10)/10)+' closing'}}},
    scales:{y:{beginAtZero:true,ticks:{precision:0},grid:{color:'#EEF1F6'},title:{display:true,text:yTitle}},x:{grid:{display:false}}}}});
const ds=(a,data,dash)=>({label:short(a),data,borderColor:COLORS[a],backgroundColor:COLORS[a],borderWidth:3,tension:.4,
  pointRadius:5,pointHoverRadius:8,pointBackgroundColor:'#fff',pointBorderWidth:3,fill:false,borderDash:dash||[]});
function activeAdmins(){return ADMINS.filter(a=>DB.entries.some(e=>e.admin===a));}
function cumulCfg(){
  const bs=buckets();const per=PER_LABEL[periodMode()];
  const sets=activeAdmins().map(a=>{let t=0;return ds(a,bs.map(b=>t+=bVal(a,b)));});
  const cfg=baseLine(sets,bs.map(b=>b.label),'Akumulasi closing (per '+per+')');
  cfg.options.onClick=(e,els)=>{if(els.length){const b=bs[els[0].index];if(b)setChartMonth(b.months[b.months.length-1]);}};
  return cfg;
}
function recapCfg(){
  const bs=buckets();const per=PER_LABEL[periodMode()];
  const sets=activeAdmins().map(a=>ds(a,bs.map(b=>bVal(a,b))));
  const tot=bs.map(b=>activeAdmins().reduce((t,a)=>t+bVal(a,b),0));
  sets.push({label:'Total tim',data:tot,borderColor:'#0F172A',backgroundColor:'#0F172A',borderWidth:3,tension:.4,
    pointRadius:5,pointHoverRadius:8,pointBackgroundColor:'#fff',pointBorderWidth:3,fill:false,borderDash:[6,5]});
  const cfg=baseLine(sets,bs.map(b=>b.label),'Closing per '+per);
  cfg.options.onClick=(e,els)=>{if(els.length){const b=bs[els[0].index];if(b)setChartMonth(b.months[b.months.length-1]);}};
  return cfg;
}
/* ---------- Grafik satu bulan (klik pill) ---------- */
let chartMonth=null,mitraChartMonth=null;
function pickMonth(cur){
  if(cur==='ALL')return 'ALL';
  const ms=scopeMonths();if(!ms.length)return null;
  if(cur&&ms.includes(cur))return cur;
  const v=$('monthView').value;
  if(v!=='all'&&ms.includes(v))return v;
  return ms[ms.length-1];
}
function pillRow(list,cur,fn,withAll){
  if(!list.length)return '<span class="empty">Belum ada data bulan.</span>';
  let h=list.map(m=>`<button class="pill${m===cur?' active':''}" onclick="${fn}('${m}')">${monShort(m)}</button>`).join('');
  if(withAll&&list.length>1)h+=`<button class="pill${cur==='ALL'?' active':''}" onclick="${fn}('ALL')">Semua</button>`;
  return h;
}
function setChartMonth(m){chartMonth=m;const ms=scopeMonths();$('dashPills').innerHTML=pillRow(ms,chartMonth,'setChartMonth',true);mk('chMonthly',monthlyBarCfg());}
function setMitraChartMonth(m){mitraChartMonth=m;const ms=scopeMonths();$('mitraPills').innerHTML=pillRow(ms,mitraChartMonth,'setMitraChartMonth');mk('chMitra',mitraBarCfg());}
const barOpts=(title)=>({responsive:true,maintainAspectRatio:false,
  plugins:{legend:{display:false},tooltip:{callbacks:{label:c=>' '+c.parsed.y+' closing'}}},
  scales:{y:{beginAtZero:true,ticks:{precision:0},grid:{color:'#EEF1F6'},title:{display:true,text:title}},x:{grid:{display:false}}}});
function monthlyBarCfg(){
  const m=pickMonth(chartMonth);
  if(!m)return {type:'bar',data:{labels:[],datasets:[]},options:barOpts('Closing')};
  if(m==='ALL'){ // grafik semua bulan: closing tiap MinGo dari bulan pertama sampai terakhir
    const ms=months();
    const sets=activeAdmins().map(a=>ds(a,ms.map(x=>amc(a,x))));
    const cfg=baseLine(sets,ms.map(monShort),'Closing per bulan — semua periode');
    return cfg;
  }
  const acts=activeAdmins();
  return {type:'bar',data:{labels:acts.map(short),datasets:[{label:'Closing '+monLabel(m),data:acts.map(a=>amc(a,m)),
    backgroundColor:acts.map(a=>COLORS[a]+'D9'),borderColor:acts.map(a=>COLORS[a]),borderWidth:2,borderRadius:10,maxBarThickness:64}]},
    options:barOpts('Closing '+monLabel(m))};
}
function mitraBarCfg(){
  const m=pickMonth(mitraChartMonth);
  if(!m)return {type:'bar',data:{labels:[],datasets:[]},options:barOpts('Closing')};
  const names=activeMitras().length?activeMitras():MITRAS;
  return {type:'bar',data:{labels:names.map(n=>n.replace('Mitra ','')),datasets:[{label:'Closing '+monLabel(m),data:names.map(n=>mVal(n,m)),
    backgroundColor:names.map(n=>MCOLORS[n]+'D9'),borderColor:names.map(n=>MCOLORS[n]),borderWidth:2,borderRadius:10,maxBarThickness:64}]},
    options:barOpts('Closing '+monLabel(m))};
}
function adsCfg(){
  const bs=buckets();const per=PER_LABEL[periodMode()];
  const vals=bs.map(bAds);let t=0;const cum=vals.map(v=>t+=v);
  return {type:'bar',
    data:{labels:bs.map(b=>b.label),datasets:[
      {type:'bar',label:'Ads spent per '+per,data:vals,backgroundColor:'#F5A623D9',borderColor:'#F5A623',borderWidth:2,borderRadius:10,maxBarThickness:64,yAxisID:'y'},
      {type:'line',label:'Akumulasi ads spent',data:cum,borderColor:'#3B5BFD',backgroundColor:'#3B5BFD',borderWidth:3,tension:.4,pointRadius:5,pointHoverRadius:8,pointBackgroundColor:'#fff',pointBorderWidth:3,yAxisID:'y1'}
    ]},
    options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
      plugins:{legend:{position:'bottom',labels:{usePointStyle:true,pointStyle:'circle',padding:18,font:{weight:'600'}}},
        tooltip:{callbacks:{label:c=>' '+c.dataset.label+': '+fmtRp(c.parsed.y)}}},
      scales:{y:{beginAtZero:true,grid:{color:'#EEF1F6'},ticks:{callback:fmtRpShort},title:{display:true,text:'Per '+per}},
        y1:{beginAtZero:true,position:'right',grid:{display:false},ticks:{callback:fmtRpShort},title:{display:true,text:'Akumulasi'}},
        x:{grid:{display:false}}}}};
}
function renderCharts(){
  chartMonth=pickMonth(chartMonth);
  $('dashPills').innerHTML=pillRow(scopeMonths(),chartMonth,'setChartMonth',true);
  mk('chCumul',cumulCfg());mk('chCumul2',cumulCfg());mk('chMonthly',monthlyBarCfg());mk('chRecap',recapCfg());mk('chAds',adsCfg());
}
function fillSnapMode(){
  const sel=$('snapMode');const cur=sel.value;const ms=months();
  let opts='<option value="all">Total semua bulan</option>';
  for(let i=1;i<ms.length;i++)opts+=`<option value="cmp:${ms[i-1]}|${ms[i]}">${monShort(ms[i-1])} → ${monShort(ms[i])}</option>`;
  sel.innerHTML=opts;
  if([...sel.options].some(o=>o.value===cur))sel.value=cur;
}
function renderSnap(){
  const el=$('teamSnap');const ms=months();const mode=$('snapMode').value||'all';
  if(!ms.length){el.innerHTML='<div class="empty">Belum ada data. Klik <b>＋ Tambah data</b>.</div>';return;}
  if(mode.startsWith('cmp:')){
    const [pm,cm]=mode.slice(4).split('|');
    $('snapSub').textContent='Perbandingan closing '+monLabel(pm)+' → '+monLabel(cm);
    const per=ADMINS.map(a=>{
      const p=amc(a,pm),c=amc(a,cm);const d=c-p;
      const g=p>0?d/p*100:(c>0?100:null);
      return{a,p,c,d,g};
    }).filter(x=>x.p>0||x.c>0).sort((x,y)=>y.c-x.c||y.d-x.d);
    if(!per.length){el.innerHTML='<div class="empty">Belum ada data pada dua bulan tersebut.</div>';return;}
    el.innerHTML=per.map((x,i)=>{
      const pill=x.g===null?'<span class="growth g-flat">—</span>':`<span class="growth ${x.d>0?'g-up':x.d<0?'g-down':'g-flat'}">${x.d>0?'↑ +':x.d<0?'↓ ':'→ '}${Math.abs(x.d)} (${x.g>0?'+':''}${x.g.toFixed(0)}%)</span>`;
      return `<div class="snap-row"><span class="rank ${i===0?'r1':''}">${i+1}</span>
        <span class="ava" style="background:${COLORS[x.a]}22;color:${COLORS[x.a]}">${short(x.a)[0]}</span>
        <div class="who"><b>${short(x.a)}</b><span>${monShort(pm)}: ${x.p} → ${monShort(cm)}: ${x.c} closing</span></div>
        <span class="big">${x.c}</span>${pill}</div>`;}).join('');
    return;
  }
  $('snapSub').textContent='Sales dengan closing terbanyak — total seluruh periode';
  const per=ADMINS.map(a=>{
    const total=sum(DB.entries.filter(e=>e.admin===a),'closing');
    let g=null,last=null;
    if(ms.length>=1)last=amc(a,ms[ms.length-1]);
    if(ms.length>=2){const p=amc(a,ms[ms.length-2]);if(p>0)g=(last-p)/p*100;else if(last>0)g=100;}
    return{a,total,last,g};
  }).filter(x=>x.total>0).sort((x,y)=>y.total-x.total);
  if(!per.length){el.innerHTML='<div class="empty">Belum ada data. Klik <b>＋ Tambah data</b>.</div>';return;}
  el.innerHTML=per.map((x,i)=>{
    const gTxt=x.g===null?'':(`<span class="growth ${x.g>1?'g-up':x.g<-1?'g-down':'g-flat'}">${x.g>0?'↑':x.g<0?'↓':'→'} ${Math.abs(x.g).toFixed(0)}%</span>`);
    return `<div class="snap-row"><span class="rank ${i===0?'r1':''}">${i+1}</span>
      <span class="ava" style="background:${COLORS[x.a]}22;color:${COLORS[x.a]}">${short(x.a)[0]}</span>
      <div class="who"><b>${short(x.a)}</b><span>Bulan terakhir: ${x.last??'—'} closing</span></div>
      <span class="big">${x.total}</span>${gTxt}</div>`;}).join('');
}
function tableFrom(valFn,elId,withGrowth){
  const ms=months();const el=$(elId);
  if(!ms.length){el.innerHTML='<tr><td class="empty">Belum ada data.</td></tr>';return;}
  let h='<thead><tr><th>MinGo</th>'+ms.map(m=>`<th class="num">${monShort(m)}</th>`).join('')+(withGrowth?'<th class="num">Total</th><th class="num">Growth terakhir</th>':'')+'</tr></thead><tbody>';
  activeAdmins().forEach(a=>{
    const vals=ms.map((m,i)=>valFn(a,m,i));
    h+=`<tr><td><b style="color:${COLORS[a]}">●</b> <b>${short(a)}</b></td>`+vals.map(v=>`<td class="num">${v}</td>`).join('');
    if(withGrowth){
      const raw=ms.map(m=>amc(a,m));const tot=raw.reduce((x,y)=>x+y,0);
      let g='—';
      if(ms.length>=2){const p=raw[raw.length-2],c=raw[raw.length-1];
        if(p>0){const d=(c-p)/p*100;g=(d>0?'↑ +':d<0?'↓ ':'→ ')+d.toFixed(0)+'%';}else if(c>0)g='↑ baru';}
      h+=`<td class="num"><b>${tot}</b></td><td class="num">${g}</td>`;
    }
    h+='</tr>';
  });
  // baris total tim
  h+=`<tr style="background:#F8FAFD"><td><b>TIM</b></td>`+ms.map((m,i)=>{
    const v = elId==='cumulTable' ? ms.slice(0,i+1).reduce((t,x)=>t+teamMonth(x),0) : teamMonth(m);
    return `<td class="num"><b>${v}</b></td>`;}).join('')+(withGrowth?`<td class="num"><b>${sum(DB.entries,'closing')}</b></td><td></td>`:'')+'</tr>';
  el.innerHTML=h+'</tbody>';
}
function renderTables(){
  tableFrom((a,m)=>amc(a,m),'monthlyTable',true);
  tableFrom((a,m,i)=>cumulSeries(a)[i],'cumulTable',false);
}
function renderRaw(){
  const el=$('rawTable');const mode=periodMode();
  if(mode!=='month'){
    const bs=buckets();const per=PER_LABEL[mode];
    if(!bs.length){el.innerHTML='<tr><td class="empty">Belum ada data tercatat.</td></tr>';return;}
    const acts=activeAdmins();
    let h=`<thead><tr><th>Periode (${per})</th>`+acts.map(a=>`<th class="num">${short(a)}</th>`).join('')+'<th class="num">Total</th></tr></thead><tbody>';
    [...bs].reverse().forEach(b=>{
      h+=`<tr><td>${b.label}</td>`+acts.map(a=>`<td class="num">${Math.round(bVal(a,b)*10)/10}</td>`).join('')+`<td class="num"><b>${Math.round(bTeam(b)*10)/10}</b></td></tr>`;
    });
    h+=`<tr style="background:#F8FAFD"><td><b>TOTAL</b></td>`+acts.map(a=>`<td class="num"><b>${Math.round(bs.reduce((t,b)=>t+bVal(a,b),0)*10)/10}</b></td>`).join('')+`<td class="num"><b>${Math.round(bs.reduce((t,b)=>t+bTeam(b),0)*10)/10}</b></td></tr>`;
    el.innerHTML=h+'</tbody>';return;
  }
  const es=[...DB.entries].sort((a,b)=>b.month.localeCompare(a.month)||a.admin.localeCompare(b.admin));
  if(!es.length){el.innerHTML='<tr><td class="empty">Belum ada data tercatat.</td></tr>';return;}
  let h='<thead><tr><th>Bulan</th><th>MinGo</th><th class="num">Closing</th><th></th></tr></thead><tbody>';
  es.forEach(e=>{h+=`<tr><td>${monLabel(e.month)}</td><td><b>${short(e.admin)}</b></td><td class="num">${e.closing}</td>
    <td style="text-align:right"><button class="btn btn-ghost btn-sm" onclick="editEntry('${e.id}')">✏️</button> <button class="btn btn-danger btn-sm" onclick="delEntry('${e.id}')">🗑️</button></td></tr>`;});
  el.innerHTML=h+'</tbody>';
}
function renderAds(){
  const el=$('adsTable');const mode=periodMode();const per=PER_LABEL[mode];
  const bs=buckets();
  if(!Object.keys(DB.ads).length||!bs.length){el.innerHTML='<tr><td class="empty">Belum ada data ads spent. Isi lewat form di atas.</td></tr>';return;}
  let run=0;const cum=bs.map(b=>run+=bAds(b));
  let h=`<thead><tr><th>Periode (${per})</th><th class="num">Ads spent</th><th class="num">Akumulasi</th>${mode==='month'?'<th></th>':''}</tr></thead><tbody>`;
  for(let i=bs.length-1;i>=0;i--){
    const b=bs[i];
    h+=`<tr><td>${b.label}</td><td class="num">${fmtRp(bAds(b))}</td><td class="num">${fmtRp(cum[i])}</td>`;
    if(mode==='month')h+=`<td style="text-align:right"><button class="btn btn-ghost btn-sm" onclick="editAds('${b.key}')">✏️</button> <button class="btn btn-danger btn-sm" onclick="delAds('${b.key}')">🗑️</button></td>`;
    h+='</tr>';
  }
  h+=`<tr style="background:#F8FAFD"><td><b>TOTAL</b></td><td class="num"><b>${fmtRp(run)}</b></td><td></td>${mode==='month'?'<td></td>':''}</tr>`;
  el.innerHTML=h+'</tbody>';
}
/* ---------- Mitra ---------- */
const mVal=(n,m)=>sum(DB.mitra.filter(e=>e.month===m&&e.mitra===n),'closing');
const bMitra=(n,b)=>b.months.reduce((t,m)=>t+mVal(n,m),0)/b.div;
function activeMitras(){return MITRAS.filter(n=>DB.mitra.some(e=>e.mitra===n));}
function fillMitraName(){const s=$('mitraName');if(s&&!s.options.length)s.innerHTML=MITRAS.map(n=>`<option>${n}</option>`).join('');}
function renderMitraKPI(){
  const bs=buckets();const per=PER_LABEL[periodMode()];
  const tot=bs.reduce((t,b)=>t+MITRAS.reduce((s,n)=>s+bMitra(n,b),0),0);
  const avg=bs.length?tot/bs.length:0;
  const rank=MITRAS.map(n=>({n,v:bs.reduce((t,b)=>t+bMitra(n,b),0)})).sort((a,b)=>b.v-a.v);
  let g='—',gs='Butuh ≥ 2 periode';
  if(bs.length>=2){
    const p=MITRAS.reduce((s,n)=>s+bMitra(n,bs[bs.length-2]),0),c=MITRAS.reduce((s,n)=>s+bMitra(n,bs[bs.length-1]),0);
    if(p>0){const d=(c-p)/p*100;g=(d>0?'+':'')+d.toFixed(1)+'%';gs=bs[bs.length-2].short+' → '+bs[bs.length-1].short;}
  }
  $('mitraKpi').innerHTML=
    kpiHTML('🤝','ic-blue','Total closing mitra',Math.round(tot*10)/10,(bs.length||0)+' '+per+' terhitung')+
    kpiHTML('📊','ic-green','Rata-rata per '+per,Math.round(avg*10)/10,'Seluruh mitra')+
    kpiHTML('🏆','ic-amber','Mitra terbaik',rank[0]&&rank[0].v>0?rank[0].n.replace('Mitra ','')+' · '+(Math.round(rank[0].v*10)/10):'—','Periode terpilih')+
    kpiHTML('🚀','ic-rose','Growth '+per+' terakhir',g,gs);
}
function renderMitraTable(){
  const el=$('mitraTable');const bs=buckets();const mode=periodMode();const per=PER_LABEL[mode];
  if(!bs.length||!DB.mitra.length){el.innerHTML='<tr><td class="empty">Belum ada data mitra. Isi lewat form di atas.</td></tr>';return;}
  let h=`<thead><tr><th>Periode (${per})</th>`+MITRAS.map(n=>`<th class="num">${n.replace('Mitra ','')}</th>`).join('')+`<th class="num">Total</th>${mode==='month'?'<th></th>':''}</tr></thead><tbody>`;
  [...bs].reverse().forEach(b=>{
    const tot=MITRAS.reduce((t,n)=>t+bMitra(n,b),0);
    h+=`<tr><td>${b.label}</td>`+MITRAS.map(n=>`<td class="num">${Math.round(bMitra(n,b)*10)/10}</td>`).join('')+`<td class="num"><b>${Math.round(tot*10)/10}</b></td>`;
    if(mode==='month')h+=`<td style="text-align:right"><button class="btn btn-danger btn-sm" onclick="delMitraMonth('${b.key}')">🗑️</button></td>`;
    h+='</tr>';
  });
  h+=`<tr style="background:#F8FAFD"><td><b>TOTAL</b></td>`+MITRAS.map(n=>`<td class="num"><b>${Math.round(bs.reduce((t,b)=>t+bMitra(n,b),0)*10)/10}</b></td>`).join('')+`<td class="num"><b>${Math.round(bs.reduce((t,b)=>t+MITRAS.reduce((s,n)=>s+bMitra(n,b),0),0)*10)/10}</b></td>${mode==='month'?'<td></td>':''}</tr>`;
  el.innerHTML=h+'</tbody>';
}
const mds=(n,data)=>({label:n.replace('Mitra ',''),data,borderColor:MCOLORS[n],backgroundColor:MCOLORS[n],borderWidth:3,tension:.4,pointRadius:5,pointHoverRadius:8,pointBackgroundColor:'#fff',pointBorderWidth:3,fill:false});
function mitraCfg(cumulative){
  const bs=buckets();const per=PER_LABEL[periodMode()];
  const sets=(activeMitras().length?activeMitras():MITRAS).map(n=>{
    let t=0;return mds(n,bs.map(b=>Math.round((cumulative?(t+=bMitra(n,b)):bMitra(n,b))*10)/10));
  });
  const cfg=baseLine(sets,bs.map(b=>b.label),(cumulative?'Akumulasi closing':'Closing')+' (per '+per+')');
  if(cumulative)cfg.options.onClick=(e,els)=>{if(els.length){const b=bs[els[0].index];if(b)setMitraChartMonth(b.months[b.months.length-1]);}};
  return cfg;
}
function renderMitra(){
  fillMitraName();renderMitraKPI();renderMitraTable();
  mitraChartMonth=pickMonth(mitraChartMonth);
  $('mitraPills').innerHTML=pillRow(scopeMonths(),mitraChartMonth,'setMitraChartMonth');
  mk('chMitra',mitraBarCfg());mk('chMitraCumul',mitraCfg(true));
}
function saveMitra(){
  const m=$('mitraMonth').value,n=$('mitraName').value,c=Number($('mitraClosing').value)||0;
  if(!m){toast('Pilih bulan dulu');return;}
  if(c<0){toast('Angka tidak boleh negatif');return;}
  const dup=DB.mitra.find(x=>x.month===m&&x.mitra===n);
  if(dup)dup.closing=c;else DB.mitra.push({id:uid(),month:m,mitra:n,closing:c});
  save();$('mitraClosing').value='';renderAll();toast('✅ Data mitra tersimpan');
}
function delMitraMonth(m){if(!confirm('Hapus data mitra '+monLabel(m)+'?'))return;DB.mitra=DB.mitra.filter(x=>x.month!==m);save();renderAll();toast('Data mitra dihapus');}
/* ---------- Leads ---------- */
function fillLeadsAdmin(){const s=$('leadsAdmin');if(s&&!s.options.length)s.innerHTML=ADMINS.map(a=>`<option>${a}</option>`).join('');}
function leadVal(type,m){return DB.leads.filter(e=>e.month===m).reduce((t,e)=>t+(Number(e[type])||0),0);}
function renderLeads(){
  fillLeadsAdmin();const bs=buckets();const scoped=DB.leads.filter(e=>scopeMonths().includes(e.month));
  const totals=['cold','warm','hot'].map(k=>scoped.reduce((t,e)=>t+(Number(e[k])||0),0));const all=totals.reduce((a,b)=>a+b,0);
  $('leadsKpi').innerHTML=kpiHTML('❄','ic-blue','Cold',totals[0].toLocaleString('id-ID'),'Leads periode terpilih')+kpiHTML('◐','ic-amber','Warm',totals[1].toLocaleString('id-ID'),'Leads periode terpilih')+kpiHTML('🔥','ic-rose','Hot',totals[2].toLocaleString('id-ID'),'Leads periode terpilih')+kpiHTML('◎','ic-green','Total leads',all.toLocaleString('id-ID'),'Semua kategori');
  const rows=[...scoped].sort((a,b)=>b.month.localeCompare(a.month)||a.admin.localeCompare(b.admin));
  $('leadsTable').innerHTML=rows.length?'<thead><tr><th>Bulan</th><th>MinGo</th><th class="num">Cold</th><th class="num">Warm</th><th class="num">Hot</th><th class="num">Total</th><th></th></tr></thead><tbody>'+rows.map(e=>`<tr><td>${monLabel(e.month)}</td><td><b>${short(e.admin)}</b></td><td class="num">${e.cold}</td><td class="num">${e.warm}</td><td class="num">${e.hot}</td><td class="num"><b>${e.cold+e.warm+e.hot}</b></td><td style="text-align:right"><button class="btn btn-ghost btn-sm" onclick="editLeads('${e.id}')">✏️</button> <button class="btn btn-danger btn-sm" onclick="delLeads('${e.id}')">🗑️</button></td></tr>`).join('')+'</tbody>':'<tr><td class="empty">Belum ada data leads.</td></tr>';
  const labels=bs.map(b=>b.label);const colors={cold:'#3B5BFD',warm:'#F5A623',hot:'#E1503C'};
  const sets=['cold','warm','hot'].map(k=>({label:k[0].toUpperCase()+k.slice(1),data:bs.map(b=>Math.round(b.months.reduce((t,m)=>t+leadVal(k,m),0)/b.div*10)/10),borderColor:colors[k],backgroundColor:colors[k],borderWidth:3,tension:.4,pointRadius:5,pointHoverRadius:8,pointBackgroundColor:'#fff',pointBorderWidth:3,fill:false}));
  mk('chLeads',baseLine(sets,labels,'Jumlah leads'));
}
function saveLeads(){const month=$('leadsMonth').value,admin=$('leadsAdmin').value;const vals=['Cold','Warm','Hot'].map(k=>Number($('leads'+k).value));if(!month){toast('Pilih bulan dulu');return;}if(vals.some(v=>!Number.isInteger(v)||v<0)){toast('Isi semua leads dengan angka bulat 0 atau lebih');return;}const dup=DB.leads.find(e=>e.month===month&&e.admin===admin);const row={id:dup?dup.id:uid(),month,admin,cold:vals[0],warm:vals[1],hot:vals[2]};if(dup)Object.assign(dup,row);else DB.leads.push(row);save();['Cold','Warm','Hot'].forEach(k=>$('leads'+k).value='');renderAll();toast('✅ Leads tersimpan');}
function editLeads(id){const e=DB.leads.find(x=>x.id===id);if(!e)return;$('leadsMonth').value=e.month;$('leadsAdmin').value=e.admin;$('leadsCold').value=e.cold;$('leadsWarm').value=e.warm;$('leadsHot').value=e.hot;$('leadsCold').focus();}
function delLeads(id){if(!confirm('Hapus data leads ini?'))return;DB.leads=DB.leads.filter(e=>e.id!==id);save();renderAll();toast('Data leads dihapus');}
/* ---------- Ringkasan mingguan (selalu per minggu, tidak ikut dropdown) ---------- */
function weekBuckets(){
  const out=[];
  scopeMonths().forEach(m=>{for(let w=1;w<=4;w++)out.push({key:m+'-W'+w,label:'Minggu '+w+' · '+monLabel(m),months:[m],div:4});});
  return out;
}
function weekRows(){
  return weekBuckets().map(b=>({
    label:b.label,
    closing:Math.round(bTeam(b)*10)/10,
    ads:bAds(b),
    mitra:Math.round(MITRAS.reduce((s,n)=>s+bMitra(n,b),0)*10)/10
  }));
}
function renderWeekly(){
  const rows=weekRows();const el=$('weeklyTable');
  const tC=rows.reduce((t,r)=>t+r.closing,0),tA=rows.reduce((t,r)=>t+r.ads,0),tM=rows.reduce((t,r)=>t+r.mitra,0);
  const last=rows[rows.length-1];
  $('weeklyKpi').innerHTML=
    kpiHTML('📈','ic-blue','Closing minggu terakhir',last?Math.round(last.closing*10)/10:'—',last?last.label:'Belum ada data')+
    kpiHTML('💸','ic-amber','Ads spent minggu terakhir',last?fmtRp(last.ads):'—','Estimasi mingguan')+
    kpiHTML('🤝','ic-green','Poin mitra minggu terakhir',last?Math.round(last.mitra*10)/10:'—','Seluruh mitra')+
    kpiHTML('🧾','ic-rose','Total '+rows.length+' minggu',Math.round(tC*10)/10+' closing',fmtRp(tA)+' · '+Math.round(tM*10)/10+' poin mitra');
  if(!rows.length){el.innerHTML='<tr><td class="empty">Belum ada data.</td></tr>';return;}
  let h='<thead><tr><th>Minggu</th><th class="num">Total closing</th><th class="num">Ads spent</th><th class="num">Poin mitra</th></tr></thead><tbody>';
  [...rows].reverse().forEach(r=>{h+=`<tr><td>${r.label}</td><td class="num">${r.closing}</td><td class="num">${fmtRp(r.ads)}</td><td class="num">${r.mitra}</td></tr>`;});
  h+=`<tr style="background:#F8FAFD"><td><b>TOTAL</b></td><td class="num"><b>${Math.round(tC*10)/10}</b></td><td class="num"><b>${fmtRp(tA)}</b></td><td class="num"><b>${Math.round(tM*10)/10}</b></td></tr>`;
  el.innerHTML=h+'</tbody>';
}
function exportWeeklyPDF(){
  const J=window.jspdf&&window.jspdf.jsPDF;
  if(!J){toast('Gagal memuat pembuat PDF');return;}
  const rows=weekRows();
  if(!rows.length){toast('Belum ada data untuk dilaporkan');return;}
  const d=new J({unit:'pt',format:'a4'});const W=d.internal.pageSize.getWidth();
  let y=56;
  d.setFontSize(17).setFont(undefined,'bold').text('Laporan Mingguan · MinGO Performance Hub',40,y);
  y+=18;d.setFontSize(10).setFont(undefined,'normal').setTextColor(110)
   .text('Fandiego Travel · dibuat '+new Date().toLocaleString('id-ID'),40,y);
  d.setTextColor(20);y+=26;
  const cx=[40,250,340,470];
  const head=()=>{d.setFillColor(240,243,250).rect(36,y-13,W-72,20,'F');
    d.setFontSize(10).setFont(undefined,'bold');
    d.text('Minggu',cx[0],y);d.text('Closing',cx[1],y);d.text('Ads spent',cx[2],y);d.text('Poin mitra',cx[3],y);
    y+=18;d.setFont(undefined,'normal');};
  head();
  rows.forEach(r=>{
    if(y>780){d.addPage();y=56;head();}
    d.setFontSize(10);
    d.text(String(r.label),cx[0],y);
    d.text(String(r.closing),cx[1],y);
    d.text(fmtRp(r.ads),cx[2],y);
    d.text(String(r.mitra),cx[3],y);
    y+=16;
  });
  const tC=rows.reduce((t,r)=>t+r.closing,0),tA=rows.reduce((t,r)=>t+r.ads,0),tM=rows.reduce((t,r)=>t+r.mitra,0);
  y+=6;d.setDrawColor(200).line(36,y-10,W-36,y-10);
  d.setFont(undefined,'bold');
  d.text('TOTAL',cx[0],y+4);d.text(String(Math.round(tC*10)/10),cx[1],y+4);
  d.text(fmtRp(tA),cx[2],y+4);d.text(String(Math.round(tM*10)/10),cx[3],y+4);
  d.save('Laporan_Mingguan_MinGO_'+new Date().toISOString().slice(0,10)+'.pdf');
  toast('📄 Laporan mingguan diunduh');
}
function renderAll(){fillMonthView();fillSnapMode();renderKPI();renderCompare();renderCharts();renderSnap();renderTables();renderRaw();renderAds();renderMitra();renderLeads();renderWeekly();renderBackupInfo();}


/* ---------- CRUD ---------- */
function openModal(id){
  $('fAdmin').innerHTML=ADMINS.map(a=>`<option>${a}</option>`).join('');
  $('fTarget').value=DB.target;
  if(id){const e=DB.entries.find(x=>x.id===id);if(e){$('modalTitle').textContent='Edit data';$('fId').value=e.id;$('fMonth').value=e.month;$('fAdmin').value=e.admin;$('fClosing').value=e.closing;}}
  else{$('modalTitle').textContent='Tambah data';$('fId').value='';const d=new Date();$('fMonth').value=d.getFullYear()+'-'+String(d.getMonth()+1).padStart(2,'0');$('fClosing').value='';}
  $('modal').classList.add('show');
}
function closeModal(){$('modal').classList.remove('show');}
function editEntry(id){openModal(id);}
function saveEntry(){
  const id=$('fId').value,month=$('fMonth').value,admin=$('fAdmin').value;
  const closing=Number($('fClosing').value)||0;
  if(!month){toast('Pilih bulan dulu');return;}
  if(closing<0){toast('Angka tidak boleh negatif');return;}
  DB.target=Number($('fTarget').value)||DB.target;
  if(id){const i=DB.entries.findIndex(x=>x.id===id);if(i>-1)DB.entries[i]={id,month,admin,closing};}
  else{
    const dup=DB.entries.find(x=>x.month===month&&x.admin===admin);
    if(dup)dup.closing=closing;
    else DB.entries.push({id:uid(),month,admin,closing});
  }
  save();closeModal();renderAll();toast('✅ Data tersimpan');
}
function delEntry(id){if(!confirm('Hapus data ini?'))return;DB.entries=DB.entries.filter(x=>x.id!==id);save();renderAll();toast('Data dihapus');}
function saveAds(){
  const m=$('adsMonth').value,a=Number($('adsAmount').value)||0;
  if(!m){toast('Pilih bulan dulu');return;}
  if(a<0){toast('Angka tidak boleh negatif');return;}
  DB.ads[m]=a;save();$('adsAmount').value='';renderAll();toast('✅ Ads spent tersimpan');
}
function editAds(m){$('adsMonth').value=m;$('adsAmount').value=DB.ads[m]||0;$('adsAmount').focus();toast('✏️ Ubah nilai '+monLabel(m)+' lalu klik Simpan ads spent');}
function delAds(m){if(!confirm('Hapus ads spent '+monLabel(m)+'?'))return;delete DB.ads[m];save();renderAll();toast('Ads spent dihapus');}
function clearAll(){if(!confirm('Hapus SEMUA data? Tidak bisa dibatalkan.'))return;DB.entries=[];DB.ads={};DB.mitra=[];DB.leads=[];save();renderAll();toast('Semua data dihapus');}
function dl(name,content,type){
  const a=document.createElement('a');
  a.href=URL.createObjectURL(new Blob([content],{type}));
  a.download=name;document.body.appendChild(a);a.click();a.remove();
}
function stamp(){const d=new Date();const p=n=>String(n).padStart(2,'0');return d.getFullYear()+'-'+p(d.getMonth()+1)+'-'+p(d.getDate())+'_'+p(d.getHours())+p(d.getMinutes());}
function backupJSON(){
  const payload={app:'MinGO Performance Hub',version:1,exportedAt:new Date().toISOString(),data:DB};
  dl('MinGO_Hub_Backup_'+stamp()+'.json',JSON.stringify(payload,null,2),'application/json');
  Store.set('mingo_last_backup',new Date().toISOString());renderBackupInfo();
  toast('💾 Backup database diunduh');
}
function backupCSV(){
  const rows=[['tipe','bulan','nama','nilai']];
  DB.entries.slice().sort((a,b)=>a.month.localeCompare(b.month)).forEach(e=>rows.push(['closing',e.month,e.admin,e.closing]));
  (DB.mitra||[]).slice().sort((a,b)=>a.month.localeCompare(b.month)).forEach(e=>rows.push(['mitra',e.month,e.mitra,e.closing]));
  (DB.leads||[]).slice().sort((a,b)=>a.month.localeCompare(b.month)).forEach(e=>rows.push(['leads',e.month,e.admin,'Cold: '+e.cold+' | Warm: '+e.warm+' | Hot: '+e.hot]));
  Object.keys(DB.ads||{}).sort().forEach(m=>rows.push(['ads',m,'Ads spent',DB.ads[m]]));
  rows.push(['target','','Target bulanan',DB.target]);
  dl('MinGO_Hub_Data_'+stamp()+'.csv','\uFEFF'+rows.map(r=>r.join(';')).join('\n'),'text/csv');
  toast('📄 CSV diunduh');
}
function applyDB(obj){
  const d=obj&&obj.data?obj.data:obj;
  if(!d||typeof d!=='object'||!Array.isArray(d.entries))throw new Error('format');
  const prev=Store.get(KEY);if(prev)snapshotRaw(prev);
  DB={entries:d.entries||[],target:d.target||60,ads:d.ads||{},mitra:d.mitra||[],leads:d.leads||[]};
  Store.set(KEY,JSON.stringify(DB));renderAll();
}
function restoreJSON(input){
  const f=input.files&&input.files[0];if(!f)return;
  const r=new FileReader();
  r.onload=()=>{
    try{
      const obj=JSON.parse(r.result);
      if(!confirm('Pulihkan database dari file ini? Data saat ini akan diganti.')){input.value='';return;}
      applyDB(obj);toast('✅ Database dipulihkan');
    }catch(e){toast('⚠️ File backup tidak valid');}
    input.value='';
  };
  r.readAsText(f);
}
function restoreAuto(){
  const l=baks();
  if(!l.length){toast('Belum ada cadangan otomatis');return;}
  const last=l[l.length-1];
  if(!confirm('Kembalikan ke cadangan otomatis '+new Date(last.at).toLocaleString('id-ID')+'?'))return;
  try{applyDB(JSON.parse(last.raw));const nl=baks();nl.pop();Store.set(BKEY,JSON.stringify(nl));toast('↩️ Data dikembalikan');}
  catch(e){toast('⚠️ Cadangan rusak');}
}
function renderBackupInfo(){
  const el=$('backupInfo');if(!el)return;
  const l=baks();const lb=Store.get('mingo_last_backup');
  const n=DB.entries.length+(DB.mitra?DB.mitra.length:0)+Object.keys(DB.ads||{}).length+(DB.leads?DB.leads.length:0);
  el.textContent=n+' baris data · '+l.length+' cadangan otomatis'+
    (l.length?' (terakhir '+new Date(l[l.length-1].at).toLocaleString('id-ID')+')':'')+
    (lb?' · unduhan backup terakhir '+new Date(lb).toLocaleString('id-ID'):' · belum pernah diunduh');
}

/* ---------- Data awal (dari tabel Manager) ---------- */
function seedData(confirmFirst){
  if(confirmFirst&&!confirm('Muat ulang data awal Mei–Agustus? Nilai bulan tsb akan dikembalikan ke data awal.'))return;
  const y=2026;
  const base={'MinGo Yeni':[16,5,34,33],'MinGo Handa':[15,14,9,3],'MinGo Bian':[20,2,8,5],'MinGo Farisa':[7,7,8,13],'MinGo Fira':[0,2,5,5]};
  const ms=[y+'-05',y+'-06',y+'-07',y+'-08'];
  ms.forEach((m,i)=>{ADMINS.forEach(a=>{
    const dup=DB.entries.find(x=>x.month===m&&x.admin===a);
    if(dup)dup.closing=base[a][i];
    else DB.entries.push({id:uid(),month:m,admin:a,closing:base[a][i]});
  });});
  if(!DB.target)DB.target=60;
  /* Ads spent awal (CONTOH — silakan edit lewat menu Data) */
  const adsBase={};
  ms.forEach((m,i)=>{adsBase[m]=[2000000,2500000,3000000,3500000][i];});
  if(!Object.keys(DB.ads).length)DB.ads=adsBase;
  /* Closing mitra awal (CONTOH — silakan edit lewat menu Mitra) */
  const mBase={'Mitra Fitri':[6,8,10,12],'Mitra Ashilah':[4,6,7,9],'Mitra Nay':[3,5,6,8],'Mitra Depok':[2,4,5,7]};
  if(!DB.mitra.length)ms.forEach((m,i)=>MITRAS.forEach(n=>DB.mitra.push({id:uid(),month:m,mitra:n,closing:mBase[n][i]})));
  save();renderAll();if(confirmFirst)toast('✅ Data awal dimuat');
}

/* ---------- Nav & init ---------- */
const TITLES={dash:'Closing dashboard',sales:'Sales performance',mitra:'Mitra',leads:'Leads',data:'Data'};
document.querySelectorAll('#nav button').forEach(b=>b.addEventListener('click',()=>{
  document.querySelectorAll('#nav button').forEach(x=>x.classList.toggle('active',x===b));
  document.querySelectorAll('.view').forEach(v=>v.classList.toggle('active',v.id==='view-'+b.dataset.view));
  $('pageTitle').textContent=TITLES[b.dataset.view];
  document.querySelector('.sidebar').classList.remove('open');
  renderAll();
}));
$('monthView').addEventListener('change',renderAll);
$('periodSel').addEventListener('change',renderAll);
$('snapMode').addEventListener('change',renderSnap);
(function init(){
  load();renderAll();
  window.addEventListener('message',function(e){
    if(e.origin!==location.origin||!e.data||e.data.type!=='mingo-load')return;
    if(e.data.data&&typeof e.data.data==='object'){
      DB=Object.assign({entries:[],target:60,ads:{},mitra:[],leads:[]},e.data.data);
      if(!DB.entries.length&&!DB.mitra.length&&!Object.keys(DB.ads).length){seedData(false);}else{Store.set(KEY,JSON.stringify(DB));renderAll();}
      toast('☁️ Data online tersinkron');
    }
  });
  window.parent.postMessage({type:'mingo-ready',localData:DB},location.origin);
})();
</script>
</body>
</html>
