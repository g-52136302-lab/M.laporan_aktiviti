<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>CHR OPR Prompt Generator · 全预设多语言版（完整）</title>
<style>
:root{--bg:#f3f6fb;--ink:#172033;--muted:#64748b;--brand:#3157d5;--line:#e2e8f0;--card:#fff}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--ink);font:15px/1.5 Inter,"Segoe UI",Arial,sans-serif}
.top{background:linear-gradient(120deg,#172554,#3157d5 60%,#7c3aed);color:#fff;padding:26px max(20px,calc((100% - 1200px)/2)) 24px}
.top h1{margin:0;font-size:clamp(22px,4vw,34px)}
.top p{margin:7px 0 0;color:#dbeafe}
.top .lang-bar{display:flex;gap:8px;flex-wrap:wrap;margin-top:14px}
.lang-btn{border:1px solid rgba(255,255,255,.55);background:rgba(255,255,255,.12);color:#fff;border-radius:99px;padding:6px 14px;font-weight:700;font-size:13px;cursor:pointer}
.lang-btn.active{background:#fff;color:#1e3a8a;border-color:#fff}
.wrap{max-width:1200px;margin:22px auto;padding:0 16px;display:grid;grid-template-columns:minmax(0,1.1fr) minmax(300px,.9fr);gap:18px}
.panel,.preview{background:var(--card);border:1px solid var(--line);border-radius:16px;box-shadow:0 8px 28px #0f172a0a}
.panel{padding:20px}
.step{display:flex;gap:10px;align-items:center;margin:4px 0 16px;font-size:18px;font-weight:750}
.num{display:grid;place-items:center;width:28px;height:28px;border-radius:9px;background:#e9efff;color:#3157d5;font-size:14px;flex-shrink:0}
.grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:12px}
.field{margin-bottom:12px}
.field.full{grid-column:1/-1}
label{display:block;font-size:13px;font-weight:700;margin-bottom:5px;color:#334155}
input,select,textarea{width:100%;border:1px solid #cbd5e1;border-radius:9px;padding:10px 11px;background:#fff;color:#172033;font:inherit;outline:none}
input:focus,select:focus,textarea:focus{border-color:#5576ed;box-shadow:0 0 0 3px #3157d51a}
textarea{min-height:88px;resize:vertical}
.hint{font-size:12px;color:var(--muted);margin-top:5px}
.section{border-top:1px solid var(--line);padding-top:17px;margin-top:17px}
.check{display:flex;gap:8px;align-items:center;font-size:13px;margin:6px 0}
.check input{width:auto}
.preset-box{background:linear-gradient(135deg,#eff6ff,#e0e7ff);border:1.5px solid #93c5fd;padding:14px;border-radius:12px;margin-bottom:14px}
.preset-box label{color:#1e40af;font-size:14px;font-weight:800;display:flex;align-items:center;gap:6px;flex-wrap:wrap}
.preset-box select{background:#fff;font-weight:600;color:#1e3a8a;border-color:#60a5fa}
.preset-count{font-size:12px;color:#1e40af;font-weight:700;margin-left:auto}
.preview{padding:18px;position:sticky;top:12px;height:max-content}
.preview-head{display:flex;justify-content:space-between;gap:8px;align-items:center}
.preview h3{margin:0}
.pill{font-size:11px;padding:4px 9px;border-radius:99px;background:#eef2ff;color:#334fc7;font-weight:700}
.sample{margin-top:14px;aspect-ratio:210/297;border:1px solid #d7deea;border-radius:10px;padding:16px;background:#fff;display:flex;flex-direction:column;gap:9px;overflow:hidden}
.sample-top{height:9px;border-radius:6px;background:var(--c1)}
.sample-title{font-size:19px;font-weight:850;color:var(--c1);line-height:1.15}
.sample-sub{height:7px;width:60%;background:var(--c2);border-radius:8px}
.sample-band{background:var(--c1);color:#fff;padding:7px 9px;font-size:10px;font-weight:800;letter-spacing:1px;border-radius:5px}
.sample-grid{display:grid;grid-template-columns:1fr 1fr;gap:7px}
.sample-box{border:1px solid var(--c2);border-radius:6px;padding:8px;min-height:52px}
.sample-box b{display:block;color:var(--c1);font-size:9px;margin-bottom:5px}
.sample-box span{display:block;height:4px;background:#dce3ed;border-radius:5px;margin:4px 0}
.sample-photos{display:grid;grid-template-columns:repeat(3,1fr);gap:6px;flex:1;min-height:65px}
.sample-photos i{background:linear-gradient(135deg,var(--c2),#f1f5f9);border-radius:5px;border:1px solid var(--c1)}
.sample-sign{display:flex;justify-content:space-between;border-top:1px solid #cbd5e1;padding-top:9px;font-size:8px;color:#64748b}
.actions{display:flex;gap:9px;flex-wrap:wrap;margin-top:16px}
.btn{border:0;border-radius:9px;padding:11px 15px;font-weight:750;cursor:pointer}
.primary{background:#3157d5;color:#fff}
.secondary{background:#e9efff;color:#2746b8}
.ghost{background:#f1f5f9;color:#334155}
.btn:hover{filter:brightness(.97)}
.result{margin-top:18px;display:none}
.result textarea{min-height:250px;font:12px/1.55 ui-monospace,monospace;background:#f8fafc}
.note{background:#f8fafc;border:1px dashed #cbd5e1;padding:10px 12px;border-radius:99px;color:#64748b;font-size:12px}
.footer{text-align:center;color:#64748b;font-size:12px;padding:22px}
.mobile-tabs{display:none;position:sticky;top:0;z-index:50;background:#0f172a;padding:8px;gap:8px;border-bottom:1px solid #334155}
.mobile-tab-btn{flex:1;padding:10px;border:1px solid #334155;background:#1e293b;color:#94a3b8;border-radius:8px;font-weight:800;font-size:14px;cursor:pointer;text-align:center}
.mobile-tab-btn.active{background:#3157d5;color:#fff;border-color:#60a5fa}
@media(max-width:850px){
  .wrap{grid-template-columns:1fr}.preview{position:static;height:auto}.grid{grid-template-columns:1fr}.field.full{grid-column:auto}
  .top{padding:22px 18px}.mobile-tabs{display:flex}.desktop-only{display:none !important}
  .panel,.preview{border-radius:14px}.preview{padding:14px}.sample{aspect-ratio:auto;min-height:420px}
}
</style>
</head>
<body>
<header class="top">
  <h1 id="ui_appTitle">✦ OPR Prompt Generator · 全预设多语言版</h1>
  <p id="ui_appSub">Penjana prompt One Page Report untuk guru · SJK(C) Chung Hua Rantau</p>
  <div class="lang-bar">
    <button class="lang-btn active" data-lang="zh" onclick="switchLanguage('zh')">中文</button>
    <button class="lang-btn" data-lang="ms" onclick="switchLanguage('ms')">Bahasa Melayu</button>
    <button class="lang-btn" data-lang="en" onclick="switchLanguage('en')">English</button>
  </div>
</header>

<div class="mobile-tabs">
  <div class="mobile-tab-btn active" id="tabEdit" onclick="switchMobileTab('edit')">✏️ <span id="ui_tabEdit">编辑表单</span></div>
  <div class="mobile-tab-btn" id="tabPreview" onclick="switchMobileTab('preview')">👁️ <span id="ui_tabPreview">预览样式</span></div>
</div>

<main class="wrap">
<section class="panel" id="panelArea">

<div class="step"><span class="num">01</span><span id="ui_step1">Pilih reka bentuk & warna</span></div>
<div class="grid">
  <div class="field">
    <label id="ui_lblCategory">类别 / Kategori Design</label>
    <select id="category">
      <option value="all">Semua kategori</option>
      <option value="creative">Kreatif & Eksperimental</option>
      <option value="formal">Formal & Korporat</option>
      <option value="islamic">Islamik & Geometri</option>
      <option value="nature">Alam & Pendidikan</option>
      <option value="modern">Moden & Minimalis</option>
    </select>
  </div>
  <div class="field">
    <label id="ui_lblDesign">Reka bentuk OPR</label>
    <select id="design"></select>
    <div class="hint" id="ui_hintDesign">150 pilihan reka bentuk dengan susun atur berbeza.</div>
  </div>
  <div class="field">
    <label id="ui_lblPalette">Palet warna</label>
    <select id="palette"></select>
    <div class="hint" id="ui_hintPalette">100 kombinasi warna boleh dipilih.</div>
  </div>
  <div class="field">
    <label id="ui_lblPhotos">Bilangan foto aktiviti</label>
    <select id="photos"><option>0</option><option>1</option><option>2</option><option>3</option><option selected>4</option><option>5</option></select>
  </div>
  <div class="field">
    <label id="ui_lblOrientation">Saiz & orientasi</label>
    <select id="orientation"></select>
  </div>
  <div class="field">
    <label id="ui_lblPhotoMode">Jenis foto</label>
    <select id="photoMode"></select>
  </div>
</div>

<div class="section">
  <div class="step"><span class="num">02</span><span id="ui_step2">Butiran program</span></div>
  <div class="field full preset-box">
    <label>📌 <span id="ui_lblPreset">快速套用全页学校活动预设</span> <span class="preset-count" id="presetCount"></span></label>
    <select id="presetSelect" onchange="autoFillPreset()"></select>
    <div class="hint" style="color:#1e3a8a;font-size:12.5px;margin-top:6px;font-weight:600;" id="ui_presetHint">✨ 选择活动后，全页所有项目均将一键全自动填满！</div>
  </div>
  <div class="grid">
    <div class="field"><label id="ui_lblSchool">Nama sekolah / organisasi</label><input id="school" value="SJK(C) Chung Hua Rantau"></div>
    <div class="field full" style="background: linear-gradient(135deg, #eff6ff, #dbeafe); border: 2px solid #3b82f6; padding: 14px; border-radius: 12px; margin-bottom: 12px;">
  <label id="ui_lblProgram" style="color: #1e40af; font-size: 15px; font-weight: 800; display: flex; align-items: center; gap: 6px;">
    ✏️ 老师输入活动题目 (Nama Program) 
  </label>
  <div style="display: flex; gap: 8px; margin-top: 6px; flex-wrap: wrap;">
    <input id="program" placeholder="输入任何活动名称（例如：五年级科学探究比赛 / Kem Motivasi / 反毒讲座）" style="flex: 1; min-width: 240px; font-size: 15px; font-weight: 600; border-color: #3b82f6;">
    <button type="button" class="btn ai-btn" id="btnAIFill" onclick="aiAutoFillFromTitle()" style="background: linear-gradient(135deg, #2563eb, #7c3aed); color: #fff; font-size: 14px; padding: 10px 18px; border-radius: 9px; font-weight: 800; cursor: pointer; border: none; box-shadow: 0 4px 12px rgba(37,99,235,0.3);">
      ✨ AI 智能推导填满所有 OPR 内容
    </button>
  </div>
  <div class="hint" style="color: #1e40af; font-size: 12.5px; margin-top: 6px; font-weight: 600;" id="ui_aiHint">
    💡 提示：老师只需在此输入活动题目，点击右侧 AI 按钮，系统将根据题目全自动填满下方所有 18 项 OPR Report 事项！
  </div>
</div>
    <div class="field"><label id="ui_lblDate">Tarikh & hari</label><input id="date"></div>
    <div class="field"><label id="ui_lblVenue">Tempat</label><input id="venue"></div>
    <div class="field"><label id="ui_lblStart">Masa mula</label><input id="start"></div>
    <div class="field"><label id="ui_lblEnd">Masa tamat</label><input id="end"></div>
    <div class="field"><label id="ui_lblOrganizer">Penganjur / unit pelaksana</label><input id="organizer"></div>
    <div class="field"><label id="ui_lblTarget">Kumpulan sasaran</label><input id="target"></div>
    <div class="field"><label id="ui_lblAttendance">Kehadiran sebenar</label><input id="attendance"></div>
    <div class="field"><label id="ui_lblLanguage">Bahasa kandungan OPR</label>
      <select id="language" onchange="onLanguageChange()">
        <option value="中文（简体）">中文（简体）</option>
        <option value="Bahasa Melayu" selected>Bahasa Melayu</option>
        <option value="English">English</option>
        <option value="Bahasa Melayu + 中文">Bahasa Melayu + 中文</option>
        <option value="Bahasa Melayu + English">Bahasa Melayu + English</option>
      </select>
    </div>
    <div class="field full"><label id="ui_lblLogo">Logo sekolah</label>
      <select id="logo"></select>
    </div>
  </div>
</div>

<div class="section">
  <div class="step"><span class="num">03</span><span id="ui_step3">Isi laporan yang bermakna</span></div>
  <div class="grid">
    <div class="field full"><label id="ui_lblObjectives">Objektif program</label><textarea id="objectives"></textarea></div>
    <div class="field full"><label id="ui_lblImplementation">Pelaksanaan & pengisian aktiviti (Pengenalan)</label><textarea id="implementation"></textarea></div>
    <div class="field full"><label id="ui_lblImpact">Impak / hasil program</label><textarea id="impact"></textarea></div>
    <div class="field full"><label id="ui_lblEvidence">Bukti & data sokongan (Evidence & Data)</label><textarea id="evidence"></textarea></div>
    <div class="field"><label id="ui_lblStrengths">Kekuatan program (Strengths)</label><textarea id="strengths"></textarea></div>
    <div class="field"><label id="ui_lblChallenges">Kekangan / cabaran (Challenges)</label><textarea id="challenges"></textarea></div>
    <div class="field full"><label id="ui_lblSuggestions">Cadangan penambahbaikan (Suggestions)</label><textarea id="suggestions"></textarea></div>
    <div class="field"><label id="ui_lblOfficer">Pegawai / pihak bertanggungjawab (pilihan)</label><input id="officer"></div>
    <div class="field"><label id="ui_lblDuration">Tempoh tindakan (pilihan)</label><input id="duration"></div>
  </div>
</div>

<div class="section">
  <div class="step"><span class="num">04</span><span id="ui_step4">Pengesahan & sentuhan akhir</span></div>
  <div class="grid">
    <div class="field"><label id="ui_lblPreparedName">Disediakan oleh — Nama</label><input id="preparedName"></div>
    <div class="field"><label id="ui_lblPreparedRole">Jawatan</label><input id="preparedRole"></div>
    <div class="field"><label id="ui_lblPreparedDate">Tarikh</label><input id="preparedDate"></div>
    <div class="field"><label id="ui_lblVerifiedName">Disahkan oleh — Nama</label><input id="verifiedName"></div>
    <div class="field"><label id="ui_lblVerifiedRole">Jawatan</label><input id="verifiedRole"></div>
    <div class="field"><label id="ui_lblVerifiedDate">Tarikh</label><input id="verifiedDate"></div>
  </div>
  <label class="check"><input type="checkbox" id="addReviewed"> <span id="ui_lblAddReviewed">Tambah blok “Disemak oleh”</span></label>
  <div id="reviewFields" class="grid" style="display:none">
    <div class="field"><label id="ui_lblReviewName">Nama penyemak</label><input id="reviewName"></div>
    <div class="field"><label id="ui_lblReviewRole">Jawatan</label><input id="reviewRole"></div>
    <div class="field"><label id="ui_lblReviewDate">Tarikh</label><input id="reviewDate"></div>
  </div>
  <label class="check"><input type="checkbox" id="addComment"> <span id="ui_lblAddComment">Sediakan ruang ulasan</span></label>
  <label class="check"><input type="checkbox" id="addStamp"> <span id="ui_lblAddStamp">Sediakan ruang cop rasmi kosong</span></label>
  <div class="field"><label id="ui_lblFormat">Keperluan format sekolah (pilihan)</label><textarea id="format"></textarea></div>
  <div class="field"><label id="ui_lblExtra">Arahan tambahan (pilihan)</label><textarea id="extra"></textarea></div>
  <div class="note" id="ui_note">Generator ini membina prompt untuk ChatGPT atau Gemini.</div>
  <div class="actions">
    <button class="btn primary" id="generate">✦ <span id="ui_btnGenerate">Generate prompt penuh</span></button>
    <button class="btn ghost" id="reset">↺ <span id="ui_btnReset">Kosongkan borang</span></button>
  </div>
  <div class="result" id="result">
    <div class="step" id="ui_resultTitle">Prompt anda sudah lengkap</div>
    <textarea id="output" readonly></textarea>
    <div class="actions">
      <button class="btn primary" id="copy"><span id="ui_btnCopy">Salin prompt</span></button>
      <button class="btn secondary" id="download"><span id="ui_btnDownload">Muat turun .txt</span></button>
      <button class="btn ghost" id="chatgpt">ChatGPT ↗</button>
      <button class="btn ghost" id="gemini">Gemini ↗</button>
    </div>
  </div>
</div>
</section>

<aside class="preview" id="previewArea">
  <div class="preview-head">
    <h3 id="ui_previewTitle">Pratonton gaya design</h3>
    <span class="pill" id="designCount">150 design</span>
  </div>
  <div class="sample" id="sample">
    <div class="sample-top"></div>
    <div class="sample-title" id="sampleTitle">ONE PAGE REPORT</div>
    <div class="sample-sub"></div>
    <div class="sample-band" id="ui_sampleBand">PROGRAM OVERVIEW</div>
    <div class="sample-grid">
      <div class="sample-box"><b id="ui_sbObj">OBJEKTIF</b><span></span><span></span><span></span></div>
      <div class="sample-box"><b id="ui_sbImpl">PELAKSANAAN</b><span></span><span></span><span></span></div>
      <div class="sample-box"><b id="ui_sbImpact">IMPAK</b><span></span><span></span></div>
      <div class="sample-box"><b id="ui_sbEvidence">BUKTI & DATA</b><span></span><span></span></div>
    </div>
    <div class="sample-band" id="ui_sampleBand2">DOKUMENTASI AKTIVITI</div>
    <div class="sample-photos"><i></i><i></i><i></i><i></i></div>
    <div class="sample-sign"><span id="ui_sign1">DISEDIAKAN OLEH</span><span id="ui_sign2">DISAHKAN OLEH</span></div>
  </div>
  <div class="hint" id="designDescription">Pilih reka bentuk untuk melihat gaya visual ringkas.</div>
  <div class="section">
    <b id="ui_flowTitle">Aliran penggunaan</b>
    <p class="hint" id="ui_flowText"></p>
  </div>
</aside>
</main>
<footer class="footer" id="ui_footer"></footer>

<script>
/* ===== 1. 设计 & 调色板 ===== */
const names=['Ribbon Orbit','Editorial Layers','Dynamic Mosaic','Geometric Flow','Diagonal Energy','Photo Storyline','Modern Cards','Bold Typography','Timeline Journey','Asymmetric Balance','Minimal Grid','Color Blocks','Circular Focus','Paper Cut','Magazine Spread','Campus Chronicle','Creative Frames','Infographic Path','Spotlight Header','Modular Report'];
const catLabels={
  creative:{zh:'创意与实验性',ms:'Kreatif & Eksperimental',en:'Creative & Experimental'},
  formal:{zh:'正式与企业风',ms:'Formal & Korporat',en:'Formal & Corporate'},
  islamic:{zh:'伊斯兰与几何',ms:'Islamik & Geometri',en:'Islamic & Geometric'},
  nature:{zh:'自然与教育',ms:'Alam & Pendidikan',en:'Nature & Education'},
  modern:{zh:'现代与极简',ms:'Moden & Minimalis',en:'Modern & Minimalist'}
};
const designs=Array.from({length:150},(_,i)=>{let cat=i<55?'creative':i<100?'formal':i<120?'islamic':i<135?'nature':'modern';return {id:i+1,name:names[i%names.length]+(i>=20?' '+String(Math.floor(i/20)+1):''),cat};});
const palettes=[['Royal Blue','#1e3a8a','#bfdbfe'],['Emerald Fresh','#166534','#bbf7d0'],['Sunset Orange','#c2410c','#fed7aa'],['Purple Gold','#5b3a78','#e9d5ff'],['Ocean Teal','#0f766e','#99f6e4'],['Rose Blush','#9f1239','#fecdd3'],['Charcoal Silver','#334155','#cbd5e1'],['Navy Gold','#172554','#facc15'],['Sky Minimal','#0369a1','#bae6fd'],['Earth Natural','#78350f','#fde68a']];
const allPalettes=Array.from({length:100},(_,i)=>{let b=palettes[i%10];return {name:`${String(i+1).padStart(3,'0')} · ${b[0]} ${i>=10?'Var '+(Math.floor(i/10)+1):''}`,a:b[1],b:b[2]};});

/* ===== 2. 完整预设数据库（右侧文件全部活动） ===== */
const presets = {
  hui_chun: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级新春挥春比赛",ms:"PERTANDINGAN KALIGRAFI HUI CHUN PERINGKAT SEKOLAH",en:"SCHOOL HUI CHUN CALLIGRAPHY COMPETITION"},
    tarikh:"11.02.2026",masa:"8:00 AM – 10:30 AM",
    venue:{zh:"晏斗中华小学礼堂",ms:"Dewan SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Hall"},
    organizer:{zh:"华文科组与课外活动处",ms:"Panitia Bahasa Cina & Unit Kokurikulum",en:"Chinese Language Panel & Co-Curricular Unit"},
    target:{zh:"全体师生",ms:"SEMUA WARGA SEKOLAH",en:"ALL SCHOOL MEMBERS"},
    attendance:{zh:"180 人",ms:"180 orang murid dan guru",en:"180 students & teachers"},
    pengenalan:{zh:"本校举办农历新年挥春比赛，旨在发扬中华传统书法艺术，营造浓厚的节日氛围，并培养学生对汉字书写美感的欣赏能力。",ms:"Pertandingan Kaligrafi Hui Chun dianjurkan sempena Sambutan Tahun Baru Cina bagi memelihara warisan seni kaligrafi dan menyemarakkan suasana perayaan.",en:"The Hui Chun competition is organized in celebration of Chinese New Year to preserve traditional calligraphy art."},
    objektif:{zh:"传承与弘扬中华优秀传统书法文化。\n提高学生的毛笔书写水平与艺术审美素养。\n增进校园内浓厚的春意与节日喜庆气氛。",ms:"Memelihara dan memartabatkan warisan seni kaligrafi Cina.\nMeningkatkan kemahiran penulisan kaligrafi dan estetika seni murid.\nMenyemarakkan suasana kebudayaan dan perayaan di sekolah.",en:"To preserve and promote Chinese traditional calligraphy art.\nTo improve students' brushwriting technique and aesthetic appreciation.\nTo enhance the festive New Year atmosphere."},
    impak:{zh:"学生们表现专注，挥毫写下充满祝福的春联。\n校园内展示了丰富的挥春作品，充满了喜庆气息。\n有效激发了学生对学习传统艺术与汉字的兴趣。",ms:"Murid tekun menghasilkan karya kaligrafi bermakna dan indah.\nHasil karya murid dipamerkan sekali gus menceriakan suasana sekolah.\nMemupuk minat murid terhadap seni penulisan dan kebudayaan.",en:"Students engaged with focus, creating expressive spring couplets.\nStudent artworks were displayed, decorating the campus dynamically.\nSuccessfully sparked student enthusiasm for traditional arts."},
    evidence:{zh:"签到表格、竞赛作品展览照片、得奖名单及评委评分表",ms:"Borang kehadiran, foto pameran karya kaligrafi, senarai pemenang & borang pemarkahan hakim",en:"Attendance records, calligraphy exhibition photos, winner list & judge mark sheets"},
    strengths:{zh:"师生参与度极高，各年级配合良好，现场比赛秩序井然。",ms:"Penglibatan warga sekolah amat menggalakkan dan pengurusan masa pertandingan yang teratur.",en:"High participation rate across classes with smooth venue management."},
    challenges:{zh:"比赛场地空间有限，低年级墨汁清洗设施需增加。",ms:"Ruang dewan yang terhad bagi murid tahap 1 dan kemudahan mencuci berus perlu ditingkatkan.",en:"Limited hall space for lower primary pupils and brush cleaning facilities need improvement."},
    suggestions:{zh:"建议下届比赛分场次进行，并准备更多专业挥春纸张与清洗桌垫。",ms:"Dicadangkan agar pertandingan akan datang dijalankan mengikut sesi serta menambah kertas kaligrafi berkualiti.",en:"Suggest holding future events in split sessions and providing more calligraphy paper pads."},
    officer:{zh:"曾佳薇老师 (Penyelaras Program)",ms:"Pn. Chen Chia Wei",en:"Ms. Chen Chia Wei"},
    duration:{zh:"1 天 (1 Hari)",ms:"1 Hari (Hari Kejohanan)",en:"1 Day"},
    teacher:{zh:"曾佳薇老师",ms:"PN. CHEN CHIA WEI",en:"MS. CHEN CHIA WEI"},
    position:{zh:"书法与中华文化负责老师",ms:"PENYELARAS KELAB KALIGRAFI",en:"CALLIGRAPHY CLUB COORDINATOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  shu_fa: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级书法比赛",ms:"PERTANDINGAN SENI KALIGRAFI PERINGKAT SEKOLAH",en:"SCHOOL CHINESE CALLIGRAPHY COMPETITION"},
    tarikh:"15.04.2026",masa:"8:00 AM – 10:30 AM",
    venue:{zh:"晏斗中华小学礼堂",ms:"Dewan Sekolah SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau School Hall"},
    organizer:{zh:"华文科组",ms:"Panitia Bahasa Cina",en:"Chinese Language Panel"},
    target:{zh:"三至六年级学生",ms:"MURID TAHUN 3 - 6",en:"STANDARD 3 - 6 STUDENTS"},
    attendance:{zh:"95 人",ms:"95 orang murid",en:"95 pupils"},
    pengenalan:{zh:"校级书法比赛旨在培养学生沉着冷静的心性，提升毛笔笔法、结构掌控与书法艺术修养，发掘优秀书法苗子。",ms:"Pertandingan Seni Kaligrafi dianjurkan bagi membina ketenangan minda, menguasai teknik garisan dan struktur tulisan.",en:"The Chinese Calligraphy Competition aims to cultivate patience, refine brush control techniques, and discover outstanding calligraphic talents."},
    objektif:{zh:"培养学生细心、静心与持之以恒的书写品质。\n提升汉字结构美感与毛笔运笔技巧。\n选拔优秀书法选手代表学校参加校际与县级书法大赛。",ms:"Memupuk sifat kesabaran, ketenangan, dan disiplin tinggi murid.\nMeningkatkan kemahiran mengawal berus dan struktur watak tulisan.\nMemilih wakil sekolah ke Pertandingan Kaligrafi peringkat daerah.",en:"To instill mindfulness, focus, and perseverance in handwriting.\nTo master brush stroke control and character structural balance.\nTo select calligraphic representatives for district-level competitions."},
    impak:{zh:"参赛选手展现了极高的书法技巧与静心书写风貌。\n评选出了多幅笔法稳健、结构严谨的优秀书法作品。\n组建了代表学校备战外赛的书法代表队。",ms:"Peserta menampakkan teknik penulisan yang mantap dan berdisiplin.\nKarya tulisan cemerlang berjaya dipilih menerusi penilaian teliti.\nSkuad kaligrafi sekolah berjaya dibentuk ke pertandingan luar.",en:"Participants displayed steady brushwork and serene composure.\nSelected multiple top-tier artworks demonstrating excellent structure.\nSuccessfully formed the school calligraphy squad."},
    evidence:{zh:"参赛名单、作品评分表、获奖作品集及颁奖照片",ms:"Senarai nama peserta, borang pemarkahan hakim, koleksi karya pemenang & foto penyampaian hadiah",en:"Participant list, judging score sheets, winning artworks collection & prize photos"},
    strengths:{zh:"高年级学生基础稳固，作品整体水平令人满意。",ms:"Murid tahap 2 mempunyai asas kemahiran berus yang kukuh dan berdisiplin.",en:"Upper primary pupils showed strong foundational brush control skills."},
    challenges:{zh:"部分初学者在笔画结构安排上仍有待加强。",ms:"Sesetengah murid baru memerlukan bimbingan tambahan dari segi struktur watak.",en:"Some beginners require additional guidance on character structural balance."},
    suggestions:{zh:"建议设立每周书法课外兴趣小组进行有针对性的强化训练。",ms:"Mengadakan kelas bimbingan kaligrafi mingguan bagi persediaan pertandingan daerah.",en:"Establish weekly dedicated calligraphy coaching for district competition prep."},
    officer:{zh:"曾佳薇老师",ms:"Pn. Chen Chia Wei",en:"Ms. Chen Chia Wei"},
    duration:{zh:"1 天 (1 Hari)",ms:"1 Hari",en:"1 Day"},
    teacher:{zh:"曾佳薇老师",ms:"PN. CHEN CHIA WEI",en:"MS. CHEN CHIA WEI"},
    position:{zh:"华文科与书法学会指导老师",ms:"PENYELARAS KELAB KALIGRAFI",en:"CALLIGRAPHY CLUB INSTRUCTOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  yun_dong_hui: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级年度运动会",ms:"KEJOHANAN SUKAN TAHUNAN PERINGKAT SEKOLAH",en:"ANNUAL SCHOOL SPORTS DAY"},
    tarikh:"16.07.2026",masa:"7:30 AM – 1:00 PM",
    venue:{zh:"晏斗中华小学大草场",ms:"Padang SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Sports Field"},
    organizer:{zh:"体育组与家长教师协会 (PIBG)",ms:"Unit Sukan & PIBG Sekolah",en:"Sports Unit & School PTA"},
    target:{zh:"全体师生、家协理事及家长",ms:"SEMUA WARGA SEKOLAH, AJK PIBG & IBU BAPA",en:"ALL SCHOOL MEMBERS, PTA & PARENTS"},
    attendance:{zh:"350 人",ms:"350 orang peserta dan jemputan",en:"350 participants & guests"},
    pengenalan:{zh:"本校举办年度运动会，旨在推行健康生活方式，培养学生的体育精神与团队合作能力，并选拔优秀运动苗子。",ms:"Kejohanan Sukan Tahunan dianjurkan bagi memupuk gaya hidup sihat, menyemai semangat kesukanan dan kerja berpasukan.",en:"The Annual Sports Day is organized to promote a healthy lifestyle, foster sportsmanship and teamwork, and discover talented young athletes."},
    objektif:{zh:"鼓励学生积极参与体育锻炼，增强身体素质与团队凝聚力。\n发掘有潜质的运动人才，为校队选拔储备优秀运动员。\n加强家校合作与社区互动，建立和谐融洽的校园氛围。",ms:"Menggalakkan penglibatan aktif murid dalam sukan bagi meningkatkan tahap kecergasan fizikal.\nMencungkil dan memupuk bakat atlet baharu untuk diserapkan ke dalam pasukan sekolah.\nMengeratkan hubungan silaturahim antara pihak sekolah, ibu bapa, dan AJK PIBG.",en:"To encourage active sports participation and improve overall physical fitness.\nTo identify and nurture potential athletic talents for school sports teams.\nTo strengthen home-school partnership and community engagement."},
    impak:{zh:"全校学生踊跃参与各项田径与竞技比赛，展现出高昂的体育竞技精神。\n成功挖掘了多名优秀的田径与接力赛运动员。\n赛事吸引了众多家长到场加油支持，显著提升了家校凝聚力。",ms:"Murid menyertai pelbagai acara balapan dan padang dengan semangat juang yang tinggi.\nBerjaya mengenal pasti beberapa atlet berpotensi dalam acara olahraga dan lari berperingkat.\nKehadiran dan sokongan padu ibu bapa merapatkan hubungan strategik dengan pihak sekolah.",en:"Students actively participated in track and field events with great sportsmanship.\nSuccessfully identified promising athletes for track and field competitions.\nHigh attendance and enthusiastic support from parents strengthened school-community bonds."},
    evidence:{zh:"项目得分表、总冠军积分榜、现场照片记录、家协出资赞助记录",ms:"Papan markah kedudukan rumah sukan, rekod keputusan acara, foto kejohanan & rekod tajaan PIBG",en:"House points tally, track results sheets, sports day photos & PTA sponsorship log"},
    strengths:{zh:"家协大力支持，筹委会准备充分，参赛选手士气高昂。",ms:"Sokongan padu daripada AJK PIBG dan semangat kesukanan murid yang amat tinggi.",en:"Strong PTA backing, well-organized logistics, and high athletic spirit."},
    challenges:{zh:"午间天气炎热，需加设防晒帐篷与及时补充水分。",ms:"Cuaca agak panas menjelang tengah hari dan memerlukan penambahan khemah peneduh.",en:"Hot afternoon weather required extra shaded tents and hydration stations."},
    suggestions:{zh:"建议明年提早比赛开幕时间，并增设更多亲子趣味竞技项目。",ms:"Cadangan awal acara dijalankan lebih awal serta menambah acara sukaneka ibu bapa.",en:"Start morning events earlier next year and include more parent-child fun games."},
    officer:{zh:"张志明老师 (Setiausaha Sukan)",ms:"En. Chong Chee Ming",en:"Mr. Chong Chee Ming"},
    duration:{zh:"1 天 (1 Hari)",ms:"1 Hari",en:"1 Day"},
    teacher:{zh:"张志明老师",ms:"EN. CHONG CHEE MING",en:"MR. CHONG CHEE MING"},
    position:{zh:"体育主任 / 运动会协调员",ms:"SETIAUSAHA SUKAN / PENYELARAS HARI SUKAN",en:"SPORTS SECRETARY / SPORTS DAY COORDINATOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  ying_bi_zi: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级硬笔字书写比赛",ms:"PERTANDINGAN TULISAN KEMAS / PENULISAN HARD-PEN PERINGKAT SEKOLAH",en:"SCHOOL HARD-PEN HANDWRITING COMPETITION"},
    tarikh:"18.04.2026",masa:"8:00 AM – 9:30 AM",
    venue:{zh:"晏斗中华小学课室",ms:"Bilik Darjah SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Classrooms"},
    organizer:{zh:"华文科组",ms:"Panitia Bahasa Cina",en:"Chinese Language Panel"},
    target:{zh:"一至六年级学生",ms:"SEMUA MURID TAHUN 1 - 6",en:"ALL STANDARD 1 - 6 STUDENTS"},
    attendance:{zh:"160 人",ms:"160 orang murid",en:"160 pupils"},
    pengenalan:{zh:"硬笔字比赛旨在规范学生的书写姿势，培养良好的书写习惯，提高汉字硬笔书写规范与结构美感。",ms:"Pertandingan tulisan kemas dianjurkan bagi memupuk sikap penulisan tulisan tangan yang berdisiplin, kemas, dan menguasai struktur watak tulisan dengan betul.",en:"The hard-pen handwriting competition aims to regulate posture, foster neat writing habits, and improve structural precision in handwriting."},
    objektif:{zh:"培养学生良好的坐姿与执笔书写习惯。\n提高学生笔画笔顺规范及汉字结构间架美感。\n激发学生对写好规范字与硬笔书法的兴趣。",ms:"Memupuk tabiat postur duduk dan pegangan pena yang betul.\nMeningkatkan kemahiran struktur watak tulisan tangan dan susunan kemas.\nMenyemarakkan minat murid untuk menghasilkan tulisan tangan yang indah.",en:"To instill proper sitting posture and pen-holding techniques.\nTo enhance stroke order accuracy and character structural balance.\nTo inspire interest in beautiful and neat hard-pen handwriting."},
    impak:{zh:"学生作业与卷面书写整洁度得到显著提升。\n展出了多幅运笔流畅、端庄美观的硬笔字优秀作品。\n增强了学生书写规范汉字的自信心。",ms:"Kejelasan dan kekemasan tulisan tangan murid meningkat dengan ketara.\nHasil karya tulisan tangan yang kemas dipamerkan untuk teladan murid lain.\nMeningkatkan keyakinan murid dalam penulisan tulisan tangan.",en:"Noticeable improvement in student handwriting neatness across classes.\nExcellent handwriting pieces were exhibited to showcase proper penmanship.\nBoosted students' confidence in standard character penmanship."},
    evidence:{zh:"比赛作品原稿、评分标准表、获奖名单",ms:"Karya asal peserta, borang pemarkahan, senarai pemenang",en:"Original entries, scoring rubrics, winners list"},
    strengths:{zh:"学生书写态度认真，整体卷面整洁度良好。",ms:"Murid menunjukkan sikap serius dan kekemasan tulisan yang baik.",en:"Students showed serious attitude and neat handwriting overall."},
    challenges:{zh:"部分学生握笔姿势需持续纠正。",ms:"Sebahagian murid masih perlu dibimbing dari segi pegangan pena.",en:"Some students still need guidance on pen grip."},
    suggestions:{zh:"建议将硬笔字练习纳入日常课堂书写要求。",ms:"Cadangan agar latihan tulisan kemas dijadikan amalan harian dalam kelas.",en:"Integrate neat handwriting practice into daily classroom routines."},
    officer:{zh:"曾佳薇老师",ms:"Pn. Chen Chia Wei",en:"Ms. Chen Chia Wei"},
    duration:{zh:"1 天",ms:"1 Hari",en:"1 Day"},
    teacher:{zh:"曾佳薇老师",ms:"PN. CHEN CHIA WEI",en:"MS. CHEN CHIA WEI"},
    position:{zh:"华文科负责老师",ms:"PENYELARAS PANITIA BAHASA CINA",en:"CHINESE SUBJECT COORDINATOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  tahun_baru_cina: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级农历新年庆典",ms:"SAMBUTAN TAHUN BARU CINA PERINGKAT SEKOLAH",en:"SCHOOL CHINESE NEW YEAR CELEBRATION"},
    tarikh:"12.02.2026",masa:"8:00 AM – 12:00 PM",
    venue:{zh:"晏斗中华小学礼堂",ms:"Dewan SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Hall"},
    organizer:{zh:"文化活动组 & 家协",ms:"Unit Kebudayaan & PIBG",en:"Cultural Unit & PTA"},
    target:{zh:"全体师生及家协理事",ms:"SEMUA WARGA SEKOLAH & PIBG",en:"ALL SCHOOL MEMBERS & PTA"},
    attendance:{zh:"200 人",ms:"200 orang",en:"200 people"},
    pengenalan:{zh:"学校每年举办农历新年庆典，旨在庆祝多元文化，弘扬传统文化美德，并促进各族师生之间的团结与相互理解。",ms:"Sambutan Tahun Baru Cina dianjurkan secara tahunan bagi meraikan kepelbagaian budaya di sekolah serta memupuk semangat perpaduan dan persefahaman dalam kalangan murid pelbagai kaum.",en:"The Chinese New Year celebration is organized annually to honor cultural diversity, uphold traditional values, and foster unity and mutual understanding among multi-racial students and teachers."},
    objektif:{zh:"培养校园内各族师生之间的团结与包容精神。\n加深学生对中华传统文化、艺术与风俗习惯的认识。\n通过节庆活动增进教师、学生及家长之间的感情交流。",ms:"Memupuk semangat perpaduan dan toleransi antarkaum dalam kalangan warga sekolah.\nMendedahkan murid kepada kebudayaan, tradisi, dan kesenian masyarakat Cina.\nMengeratkan hubungan silaturahim antara guru, murid, dan ibu bapa menerusi aktiviti kebudayaan.",en:"To foster spirit of unity and cultural tolerance among the school community.\nTo expose students to Chinese traditional culture, arts, and customs.\nTo strengthen bond between teachers, students, and parents through festive events."},
    impak:{zh:"学生能够亲身感受并尊重不同种族的文化传统。\n校园内展现出和谐温馨的融洽氛围。\n各族学生积极参与舞狮与文化表演，提升自信心。",ms:"Murid dapat memahami dan menghormati kebudayaan kaum lain dengan lebih dekat.\nSuasana keharmonian dan semangat Muhibbah terpancar dengan jelas di peringkat sekolah.\nPenglibatan aktif murid pelbagai kaum dalam tarian singa dan persembahan seni.",en:"Students gain a deeper understanding and appreciation for diverse ethnic traditions.\nFosters a united, vibrant, and harmonious atmosphere across the school.\nActive participation of multi-racial students in lion dances and cultural performances."},
    evidence:{zh:"庆典照片、节目表、出席记录",ms:"Foto sambutan, atur cara majlis, rekod kehadiran",en:"Celebration photos, programme schedule, attendance records"},
    strengths:{zh:"师生与家协通力合作，节目丰富精彩。",ms:"Kerjasama erat warga sekolah dan PIBG, persembahan menarik.",en:"Strong collaboration among staff and PTA with engaging performances."},
    challenges:{zh:"礼堂座位有限，部分家长需站立观赏。",ms:"Ruang tempat duduk dewan terhad untuk ibu bapa.",en:"Limited seating in the hall for parents."},
    suggestions:{zh:"建议增设户外直播区，让更多家长参与。",ms:"Cadangan menambah kawasan tontonan luar dewan.",en:"Add an outdoor viewing area for more parents."},
    officer:{zh:"黄美玲老师",ms:"Pn. Wong Mei Ling",en:"Ms. Wong Mei Ling"},
    duration:{zh:"半天",ms:"Setengah Hari",en:"Half Day"},
    teacher:{zh:"黄美玲老师",ms:"PN. WONG MEI LING",en:"MS. WONG MEI LING"},
    position:{zh:"文化活动协调员",ms:"PENYELARAS KEBUDAYAAN SEKOLAH",en:"SCHOOL CULTURAL COORDINATOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  yuan_xiao: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"元宵节（元宵猜灯谜）庆典",ms:"SAMBUTAN PERAYAAN CHAP GOH MEI PERINGKAT SEKOLAH",en:"SCHOOL CHAP GOH MEI (LANTERN FESTIVAL) CELEBRATION"},
    tarikh:"03.03.2026",masa:"8:00 AM – 10:30 AM",
    venue:{zh:"晏斗中华小学礼堂",ms:"Dewan SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Hall"},
    organizer:{zh:"华文科组",ms:"Panitia Bahasa Cina",en:"Chinese Language Panel"},
    target:{zh:"全体师生",ms:"SEMUA WARGA SEKOLAH",en:"ALL SCHOOL MEMBERS"},
    attendance:{zh:"180 人",ms:"180 orang",en:"180 people"},
    pengenalan:{zh:"元宵节庆典作为农历新年的圆满收官活动，旨在让学生体验猜灯谜、品尝汤圆等传统民俗，增进文化认同与节日喜悦。",ms:"Sambutan Chap Goh Mei dianjurkan bagi menandakan penutup tirai Sambutan Tahun Baru Cina serta memupuk kefahaman kebudayaan dan keharmonisan dalam kalangan murid.",en:"The Chap Goh Mei celebration marks the joyful grand finale of the Chinese New Year, fostering cultural appreciation through lantern riddles and tradition."},
    objektif:{zh:"向学生介绍元宵节的文化寓意与民间传统习俗。\n通过猜灯谜活动锻炼学生的思维灵敏度与语文能力。\n增进各年级学生之间的互动与校园和谐氛围。",ms:"Memperkenalkan tradisi dan keunikan perayaan Chap Goh Mei kepada warga sekolah.\nMemupuk semangat integrasi dan persefahaman antarkaum menerusi aktiviti seni kebudayaan.\nMengeratkan silaturahim sesama murid menerusi aktiviti teka teki tanglung.",en:"To introduce the rich heritage and significance of Chap Goh Mei.\nTo enhance critical thinking and language skills via lantern riddle solving.\nTo strengthen friendship and harmony among all students."},
    impak:{zh:"学生对元宵节传统习俗有了更深刻的理解。\n猜灯谜环节气氛热烈，思维敏捷度得到有效锻炼。\n全校师生在融洽喜庆的气氛中感受中华文化的魅力。",ms:"Murid memahami keunikan tradisi dan warisan budaya perayaan Chap Goh Mei secara mendalam.\nSemangat keharmonian dan toleransi antarkaum dapat dipupuk dengan berkesan.\nSuasana perayaan dimeriahkan dengan aktiviti teka teki dan persembahan seni kebudayaan.",en:"Students developed a deep understanding of Chap Goh Mei traditions.\nLively engagement in riddle-solving boosted language interest.\nCreated a harmonious cultural atmosphere across the school campus."},
    evidence:{zh:"灯谜题目、活动照片、学生参与记录",ms:"Soalan teka teki, foto aktiviti, rekod penyertaan murid",en:"Riddle questions, activity photos, student participation records"},
    strengths:{zh:"灯谜内容生动有趣，学生参与度高。",ms:"Soalan teka teki menarik dan penyertaan murid tinggi.",en:"Engaging riddles with high student participation."},
    challenges:{zh:"部分低年级学生对谜面理解有困难。",ms:"Sebahagian murid tahap 1 sukar memahami teka teki.",en:"Some lower primary pupils had difficulty understanding riddles."},
    suggestions:{zh:"建议为低年级增设图画版灯谜。",ms:"Cadangan menyediakan teka teki bergambar untuk tahap 1.",en:"Add picture-based riddles for lower primary."},
    officer:{zh:"曾佳薇老师",ms:"Pn. Chen Chia Wei",en:"Ms. Chen Chia Wei"},
    duration:{zh:"半天",ms:"Setengah Hari",en:"Half Day"},
    teacher:{zh:"曾佳薇老师",ms:"PN. CHEN CHIA WEI",en:"MS. CHEN CHIA WEI"},
    position:{zh:"文化学会负责老师",ms:"PENYELARAS KELAB KEBUDAYAAN",en:"CULTURAL CLUB TEACHER-IN-CHARGE"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  duan_wu: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"端午节（赛龙舟与裹粽文化）庆典",ms:"SAMBUTAN FESTIVAL BOT NAGA & DUMPLING (PESTA DUAN WU)",en:"DRAGON BOAT & DUMPLING FESTIVAL (DUAN WU) CELEBRATION"},
    tarikh:"19.06.2026",masa:"8:00 AM – 11:30 AM",
    venue:{zh:"晏斗中华小学礼堂",ms:"Dewan SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Hall"},
    organizer:{zh:"历史科小组 & 文化学会",ms:"Panitia Sejarah & Kelab Kebudayaan",en:"History Panel & Cultural Club"},
    target:{zh:"全体师生",ms:"SEMUA WARGA SEKOLAH",en:"ALL SCHOOL MEMBERS"},
    attendance:{zh:"180 人",ms:"180 orang",en:"180 people"},
    pengenalan:{zh:"端午节庆典旨在让学生了解屈原的爱国主义精神，学习包粽子的传统技艺，体验中华传统节日的深厚文化底蕴。",ms:"Sambutan Pesta Bot Naga (Duan Wu) diadakan bagi mengenali sejarah tradisi pembuatan zongzi (bakchang) serta nilai patriotisme di sebalik perayaan tradisional ini.",en:"The Duan Wu Festival event introduces the historical significance of patriotism, dragon boat races, and traditional rice dumpling wrapping."},
    objektif:{zh:"加深学生对端午节历史由来与屈原爱国精神的认识。\n让学生亲自体验传统包粽子技艺，传承非物质文化遗产。\n培养学生团结互助与敬老怀古的美德。",ms:"Mendedahkan murid kepada sejarah dan asal usul penganjuran Pesta Bot Naga.\nMemperkenalkan tradisi seni pembuatan zongzi tradisional kepada generasi muda.\nMemupuk nilai murni seperti patriotisme dan keprihatinan masyarakat melalui kisah sejarah.",en:"To educate students on history of Duan Wu Festival and patriotic values.\nTo preserve traditional culinary art of dumpling wrapping for younger generation.\nTo build team cooperation and historical appreciation."},
    impak:{zh:"学生掌握了基本的包粽子技巧，提高了动手实践能力。\n提高了对传统文物的保护意识与爱国情怀。\n增进师生在包粽子示范与文化讲座中的互动。",ms:"Warga sekolah dapat mempelajari warisan kebudayaan dan nilai sejarah perayaan secara langsung.\nMengeratkan hubungan silaturahim menerusi sesi demonstrasi pembungkusan zongzi.\nMeningkatkan pengetahuan am murid mengenai warisan budaya berbilang kaum.",en:"Students mastered basic dumpling wrapping techniques hands-on.\nInstilled patriotic spirit and deep appreciation for historical heritage.\nEnhanced teacher-student bonding through interactive cultural workshops."},
    evidence:{zh:"活动照片、包粽子示范记录、学生作品",ms:"Foto aktiviti, rekod demonstrasi, hasil karya murid",en:"Activity photos, demonstration records, student works"},
    strengths:{zh:"学生动手体验积极，文化讲座内容充实。",ms:"Penglibatan murid aktif dan ceramah budaya padat.",en:"Active hands-on participation and informative cultural talk."},
    challenges:{zh:"包粽子材料准备耗时，场地需分批进行。",ms:"Penyediaan bahan mengambil masa, perlu dijalankan mengikut sesi.",en:"Material preparation takes time; sessions need to be staggered."},
    suggestions:{zh:"建议明年增设龙舟模型制作比赛。",ms:"Cadangan menambah pertandingan model bot naga tahun depan.",en:"Add a dragon boat model competition next year."},
    officer:{zh:"李健伟老师",ms:"En. Lee Kin Wai",en:"Mr. Lee Kin Wai"},
    duration:{zh:"半天",ms:"Setengah Hari",en:"Half Day"},
    teacher:{zh:"李健伟老师",ms:"EN. LEE KIN WAI",en:"MR. LEE KIN WAI"},
    position:{zh:"历史科小组主任",ms:"PENYELARAS PANITIA SEJARAH",en:"HEAD OF HISTORY SUBJECT"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  zhong_qiu: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"中秋节提灯晚会与赏月庆典",ms:"SAMBUTAN PESTA TANGLUNG & KUIH BULAN (MID-AUTUMN FESTIVAL)",en:"MID-AUTUMN & LANTERN NIGHT FESTIVAL CELEBRATION"},
    tarikh:"25.09.2026",masa:"6:30 PM – 9:30 PM",
    venue:{zh:"晏斗中华小学校园",ms:"Kawasan SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Campus"},
    organizer:{zh:"家协 & 文化活动组",ms:"PIBG & Unit Kebudayaan",en:"PTA & Cultural Unit"},
    target:{zh:"全体师生、家协及社区居民",ms:"SEMUA WARGA SEKOLAH & PIBG",en:"ALL SCHOOL MEMBERS & PTA"},
    attendance:{zh:"250 人",ms:"250 orang",en:"250 people"},
    pengenalan:{zh:"中秋提灯晚会旨在弘扬团圆文化，增进家庭与社区的情感纽带，通过提灯游行、品尝月饼与文化表演享受节日喜悦。",ms:"Sambutan Pesta Tanglung dianjurkan bagi meraikan keharmonian, penyatuan keluarga, serta menyemarakkan warisan kebudayaan masyarakat Cina bersama komuniti sekolah.",en:"The Mid-Autumn Festival brings together families, school staff, and community members to celebrate togetherness, mooncakes, and vibrant lantern parades."},
    objektif:{zh:"加深学生对中秋节团圆文化与传说故事的了解。\n促进家校合作，建立和谐紧密的社区纽带。\n鼓励学生发挥创意制作提灯，传承传统手艺。",ms:"Memupuk persefahaman dan menghargai kebudayaan perayaan Pesta Tanglung.\nMengeratkan hubungan muhibbah dalam kalangan warga sekolah, murid, dan ibu bapa.\nMenyemarakkan suasana perayaan menerusi acara perarakan tanglung dan persembahan seni.",en:"To foster appreciation for family unity and cultural heritage.\nTo strengthen home-school partnership and community goodwill.\nTo ignite creativity through homemade lantern showcases."},
    impak:{zh:"提灯游行吸引各族家长与学生踊跃参与，气氛温馨热闹。\n增强了学校与家长的沟通与凝聚力。\n学生感受到了传统文化的魅力与家庭团聚的温暖。",ms:"Warga sekolah menikmati kegembiraan perarakan tanglung dan rasa kebersamaan yang erat.\nMurid mengenali tradisi merasai kuih bulan dan asal usul sejarah perayaan.\nHubungan kerjasama antara pihak sekolah dan PIBG menjadi semakin kukuh.",en:"Overwhelming participation from diverse families created a joyful event.\nSolidified trust and communication between PTA and school management.\nStudents experienced warmth of cultural unity and creative expression."},
    evidence:{zh:"提灯游行照片、月饼品尝记录、活动流程表",ms:"Foto perarakan tanglung, rekod kuih bulan, atur cara",en:"Lantern parade photos, mooncake tasting records, programme schedule"},
    strengths:{zh:"夜间活动气氛温馨，家长参与度高。",ms:"Suasana malam harmoni, penyertaan ibu bapa tinggi.",en:"Warm evening atmosphere with high parent participation."},
    challenges:{zh:"夜间照明与安全需加强。",ms:"Pencahayaan dan keselamatan malam perlu diperkukuh.",en:"Night lighting and safety need strengthening."},
    suggestions:{zh:"建议增设亲子灯笼制作比赛。",ms:"Cadangan menambah pertandingan membuat tanglung keluarga.",en:"Add a parent-child lantern making contest."},
    officer:{zh:"陈淑芬老师",ms:"Pn. Tan Sook Fun",en:"Ms. Tan Sook Fun"},
    duration:{zh:"1 晚",ms:"1 Malam",en:"1 Evening"},
    teacher:{zh:"陈淑芬老师",ms:"PN. TAN SOOK FUN",en:"MS. TAN SOOK FUN"},
    position:{zh:"节庆活动协调员",ms:"PENYELARAS SAMBUTAN PERAYAAN",en:"FESTIVAL CELEBRATION COORDINATOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  dong_zhi: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"冬至（搓汤圆）文化庆典",ms:"SAMBUTAN PERAYAAN DONGZHI (PESTA KUIH TANGYUAN)",en:"DONGZHI (TANGYUAN) FESTIVAL CELEBRATION"},
    tarikh:"21.12.2026",masa:"8:00 AM – 10:30 AM",
    venue:{zh:"晏斗中华小学食堂",ms:"Kantin SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Canteen"},
    organizer:{zh:"手工与烹饪学会",ms:"Kelab Rekacipta & Kulinari",en:"Handicraft & Culinary Club"},
    target:{zh:"全体师生",ms:"SEMUA WARGA SEKOLAH",en:"ALL SCHOOL MEMBERS"},
    attendance:{zh:"180 人",ms:"180 orang",en:"180 people"},
    pengenalan:{zh:"冬至庆典旨在向学生传授“冬至大如年”的文化寓意，通过亲手搓汤圆，感受团圆、感恩与长一岁的喜悦。",ms:"Sambutan Perayaan Dongzhi dianjurkan bagi memperkenalkan tradisi pembuatan kuih Tangyuan yang melambangkan penyatuan, kesyukuran, dan keharmonian keluarga.",en:"The Dongzhi Festival celebration highlights the cultural symbolism of unity, harmony, and gratitude through hands-on glutinous rice ball (tangyuan) making."},
    objektif:{zh:"普及冬至节气的由来与中华传统食育文化。\n培养学生感恩图报与团结同心的美德。\n锻炼学生的动手能力，体验搓汤圆的乐趣。",ms:"Memperkenalkan asal usul dan makna di sebalik perayaan Dongzhi.\nMemupuk nilai murni penyatuan, keharmonian, dan rasa bersyukur dalam kalangan murid.\nMemberi peluang kepada murid mencuba amali penyediaan kuih Tangyuan secara tradisional.",en:"To educate students on winter solstice significance and culinary culture.\nTo instil core values of togetherness, gratitude, and respect.\nTo develop practical culinary creativity through tangyuan crafting."},
    impak:{zh:"学生成功学会了制作五彩汤圆，享受劳动成果。\n深刻理解冬至背后的团圆与感恩价值观。\n校园充满了浓厚的节日人情味与和谐欢笑声。",ms:"Murid menguasai kemahiran asas membuat kuih Tangyuan pelbagai warna secara kreatif.\nSemangat kekeluargaan dan persahabatan erat dipupuk dalam kalangan warga sekolah.\nMemelihara warisan seni kuliner tradisional dalam kalangan generasi muda.",en:"Students successfully crafted colorful glutinous rice balls eagerly.\nStrengthened bonds of friendship and family values among peers.\nPreserved traditional culinary art for the younger generation."},
    evidence:{zh:"搓汤圆照片、食材清单、活动记录",ms:"Foto aktiviti, senarai bahan, rekod aktiviti",en:"Tangyuan making photos, ingredient list, activity records"},
    strengths:{zh:"学生动手兴趣浓厚，活动温馨有意义。",ms:"Minat murid tinggi, aktiviti harmoni dan bermakna.",en:"High student interest and a warm, meaningful activity."},
    challenges:{zh:"食材与卫生管理需更严谨。",ms:"Pengurusan bahan dan kebersihan perlu lebih ketat.",en:"Ingredient and hygiene management need to be stricter."},
    suggestions:{zh:"建议加入冬至文化故事分享环节。",ms:"Cadangan menambah sesi perkongsian cerita Dongzhi.",en:"Add a Dongzhi cultural story sharing session."},
    officer:{zh:"黄慧玲老师",ms:"Pn. Ng Wei Ling",en:"Ms. Ng Wei Ling"},
    duration:{zh:"半天",ms:"Setengah Hari",en:"Half Day"},
    teacher:{zh:"黄慧玲老师",ms:"PN. NG WEI LING",en:"MS. NG WEI LING"},
    position:{zh:"手工与烹饪学会导师",ms:"PENYELARAS KELAB REKACIPTA & KULINER",en:"HANDICRAFT & CULINARY CLUB TEACHER"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  hari_raya: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级开斋节庆典",ms:"SAMBUTAN HARI RAYA AIDILFITRI PERINGKAT SEKOLAH",en:"SCHOOL HARI RAYA AIDILFITRI CELEBRATION"},
    tarikh:"18.04.2026",masa:"8:00 AM – 12:00 PM",
    venue:{zh:"晏斗中华小学礼堂",ms:"Dewan SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Hall"},
    organizer:{zh:"伊斯兰宗教组 & 家协",ms:"Panitia Agama Islam & PIBG",en:"Islamic Religious Panel & PTA"},
    target:{zh:"全体师生及家协理事",ms:"SEMUA WARGA SEKOLAH & PIBG",en:"ALL SCHOOL MEMBERS & PTA"},
    attendance:{zh:"200 人",ms:"200 orang",en:"200 people"},
    pengenalan:{zh:"学校举办开斋节庆典，旨在促进多元种族间的和谐共处，让学生体验马来同胞的传统节庆习俗与文化。",ms:"Sambutan Hari Raya Aidilfitri dianjurkan bagi memupuk semangat muhibbah, integrasi kaum, serta meraikan kepelbagaian perayaan agama dan budaya di sekolah.",en:"The Hari Raya Aidilfitri celebration promotes inter-ethnic harmony and exposes students to Malay cultural traditions and festive customs."},
    objektif:{zh:"弘扬各族互敬互爱的团结精神。\n加深学生对开斋节传统文化、美德与美食的了解。\n增进师生与家长的感情交流，营造温馨校园。",ms:"Memupuk nilai saling menghormati dan perpaduan dalam kalangan warga sekolah.\nMendedahkan murid kepada amalan, tradisi, dan makanan tradisional Hari Raya.\nMengeratkan hubungan silaturahim antara guru, murid, dan ibu bapa.",en:"To foster mutual respect and unity across diverse ethnic groups.\nTo introduce festive Malay cuisine, customs, and moral values.\nTo strengthen friendship bonds among students, teachers, and parents."},
    impak:{zh:"全校师生身穿各族传统服装参与，展现和谐融合氛围。\n学生学会包粽子（Ketupat）与品尝传统美食。\n有效促进各族学生之间的友谊与沟通。",ms:"Warga sekolah berpakaian kebudayaan sekali gus menyemarakkan atmosfera Muhibbah.\nMurid mempelajari seni menganyam ketupat dan tradisi kunjung-mengunjung.\nMengukuhkan integrasi dan toleransi antara murid pelbagai kaum.",en:"School community donned festive attires, showcasing multi-racial harmony.\nStudents experienced ketupat weaving and festive delicacies.\nSolidified goodwill and mutual respect among students."},
    evidence:{zh:"庆典照片、传统服装展示记录、美食准备清单",ms:"Foto sambutan, rekod pakaian tradisional, senarai makanan",en:"Celebration photos, traditional attire records, food preparation list"},
    strengths:{zh:"师生热情参与，文化展示丰富。",ms:"Penyertaan hangat, pameran budaya pelbagai.",en:"Enthusiastic participation and rich cultural displays."},
    challenges:{zh:"美食准备与分发需更有序。",ms:"Penyediaan dan pengagihan makanan perlu lebih teratur.",en:"Food preparation and distribution need better organization."},
    suggestions:{zh:"建议增设 Ketupat 编织比赛。",ms:"Cadangan menambah pertandingan anyaman ketupat.",en:"Add a ketupat weaving competition."},
    officer:{zh:"阿兹曼老师",ms:"En. Azman bin Ahmad",en:"Mr. Azman bin Ahmad"},
    duration:{zh:"半天",ms:"Setengah Hari",en:"Half Day"},
    teacher:{zh:"阿兹曼老师",ms:"EN. AZMAN BIN AHMAD",en:"MR. AZMAN BIN AHMAD"},
    position:{zh:"伊斯兰宗教与文化协调员",ms:"PENYELARAS SAMBUTAN RAYA",en:"RAYA CELEBRATION COORDINATOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  deepavali: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级屠妖节庆典",ms:"SAMBUTAN HARI DEEPAVALI PERINGKAT SEKOLAH",en:"SCHOOL DEEPAVALI CELEBRATION"},
    tarikh:"04.11.2026",masa:"8:00 AM – 11:30 AM",
    venue:{zh:"晏斗中华小学礼堂",ms:"Dewan SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Hall"},
    organizer:{zh:"文化活动组",ms:"Unit Kebudayaan",en:"Cultural Unit"},
    target:{zh:"全体师生",ms:"SEMUA WARGA SEKOLAH",en:"ALL SCHOOL MEMBERS"},
    attendance:{zh:"180 人",ms:"180 orang",en:"180 people"},
    pengenalan:{zh:"屠妖节庆典旨在让学生认识印裔同胞光明战胜黑暗的意义，体验精美的Kolam彩米图与传统文化。",ms:"Sambutan Hari Deepavali dianjurkan untuk meraikan 'Pesta Cahaya', memupuk kefahaman dan apresiasi seni kebudayaan masyarakat India.",en:"The Deepavali celebration honors the 'Festival of Lights', promoting understanding and appreciation for Indian culture and artistic traditions."},
    objektif:{zh:"向学生普及屠妖节的历史渊源与文化内涵。\n亲自参与Kolam彩米图创作，培养艺术审美与团队合作。\n强化校园内的跨文化理解与和谐共处。",ms:"Memperkenalkan makna dan perayaan Hari Deepavali kepada murid.\nMenggalakkan murid menyertai reka cipta hiasan Kolam secara kreatif.\nMemupuk perpaduan dan persefahaman antara kaum di sekolah.",en:"To educate students on the significance of the Festival of Lights.\nTo encourage creative team participation in traditional Kolam art.\nTo reinforce intercultural unity and harmony within the school."},
    impak:{zh:"学生共同完成了鲜艳夺目的Kolam彩米创作。\n加深了各族学生对印裔同胞风俗习惯的尊重。\n打造了多元、包容且充满喜庆氛围的校园生活。",ms:"Murid berjaya menghasilkan hiasan Kolam beras yang berwarna-warni.\nMenambah pengetahuan dan penghormatan terhadap amalan kaum India.\nSuasana perayaan dimeriahkan dengan semangat keharmonian.",en:"Students crafted vibrant and intricate Kolam art together.\nEnhanced knowledge and respect for Indian traditions.\nFostered an inclusive, festive, and harmonious learning environment."},
    evidence:{zh:"Kolam作品照片、庆典记录、学生参与名单",ms:"Foto karya Kolam, rekod sambutan, senarai penyertaan",en:"Kolam artwork photos, celebration records, participant list"},
    strengths:{zh:"Kolam创作活动吸引学生积极参与。",ms:"Aktiviti Kolam menarik penyertaan aktif murid.",en:"Kolam activity attracted active student participation."},
    challenges:{zh:"彩米材料准备需更充足。",ms:"Bahan beras berwarna perlu disediakan lebih banyak.",en:"Coloured rice materials need to be more sufficient."},
    suggestions:{zh:"建议增设印裔传统舞蹈表演。",ms:"Cadangan menambah persembahan tarian tradisional India.",en:"Add traditional Indian dance performances."},
    officer:{zh:"卡拉老师",ms:"Pn. Kalaivani",en:"Ms. Kalaivani"},
    duration:{zh:"半天",ms:"Setengah Hari",en:"Half Day"},
    teacher:{zh:"卡拉老师",ms:"PN. KALAIVANI",en:"MS. KALAIVANI"},
    position:{zh:"印裔学生文化代表老师",ms:"PENYELARAS KEBUDAYAAN DEEPAVALI",en:"DEEPAVALI CULTURAL COORDINATOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  pesta_kaamatan_gawai: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"丰收节（Kaamatan & Gawai）庆典",ms:"SAMBUTAN PESTA KAAMATAN & HARI GAWAI",en:"PESTA KAAMATAN & HARI GAWAI CELEBRATION"},
    tarikh:"28.05.2026",masa:"8:00 AM – 11:30 AM",
    venue:{zh:"晏斗中华小学礼堂",ms:"Dewan SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Hall"},
    organizer:{zh:"跨文化教育组",ms:"Unit Integrasi Nasional",en:"National Integration Unit"},
    target:{zh:"全体师生",ms:"SEMUA WARGA SEKOLAH",en:"ALL SCHOOL MEMBERS"},
    attendance:{zh:"180 人",ms:"180 orang",en:"180 people"},
    pengenalan:{zh:"丰收节庆典旨在向学生介绍沙巴与砂拉越原住民的丰收民俗，促进东西马各族文化的融通与爱国情怀。",ms:"Sambutan Pesta Kaamatan dan Hari Gawai diadakan bagi memperkenalkan warisan dan kebudayaan unik masyarakat Sabah dan Sarawak.",en:"Celebrates Sabah and Sarawak harvest traditions, broadening cultural knowledge and strengthening unity between East and West Malaysia."},
    objektif:{zh:"丰富学生对东马沙巴和砂拉越原住民传统节日的认识。\n学习感恩粮食与自然的伟大赐予。\n展示原住民音乐、传统舞蹈与服饰之美。",ms:"Mendedahkan murid kepada warisan kebudayaan Sabah dan Sarawak.\nMemupuk nilai murni bersyukur dan menghargai rezeki hasil bumi.\nMempersembahkan tarian, muzik dan pakaian tradisional borneo.",en:"To introduce East Malaysian heritage and native celebrations.\nTo instil gratitude for agricultural harvests and natural resources.\nTo showcase indigenous dance, music, and traditional attire."},
    impak:{zh:"学生对东马的丰收节习俗与英雄舞蹈有了直观理解。\n拓宽了地理与国家多元文化的认知视野。\n增强了全校一心的“国民团结”认同感。",ms:"Murid memahami tarian Sumazau dan Ngajat serta keunikan pakaian tradisi.\nMeluaskan horizon pengetahuan geososiokultur murid mengenai Malaysia.\nMenyejajarkan semangat 'Malaysia Madani' dan integrasi nasional.",en:"Students appreciated Sumazau and Ngajat dances and attires.\nBroadened cultural horizons regarding East Malaysian traditions.\nReinforced national integration and Malaysian unity spirit."},
    evidence:{zh:"庆典照片、传统服饰展示、文化表演记录",ms:"Foto sambutan, pameran pakaian tradisi, rekod persembahan",en:"Celebration photos, traditional attire display, performance records"},
    strengths:{zh:"文化展示新颖，学生兴趣浓厚。",ms:"Pameran budaya menarik minat murid.",en:"Novel cultural displays with strong student interest."},
    challenges:{zh:"相关文化资料与道具需提前准备。",ms:"Bahan dan prop budaya perlu disediakan awal.",en:"Cultural materials and props need early preparation."},
    suggestions:{zh:"建议邀请东马文化团体到校交流。",ms:"Cadangan menjemput kumpulan budaya Borneo ke sekolah.",en:"Invite East Malaysian cultural groups for exchange."},
    officer:{zh:"陈淑芬老师",ms:"Pn. Tan Sook Fun",en:"Ms. Tan Sook Fun"},
    duration:{zh:"半天",ms:"Setengah Hari",en:"Half Day"},
    teacher:{zh:"陈淑芬老师",ms:"PN. TAN SOOK FUN",en:"MS. TAN SOOK FUN"},
    position:{zh:"跨文化教育协调员",ms:"PENYELARAS INTEGRASI NASIONAL",en:"NATIONAL INTEGRATION COORDINATOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  lan_qiu: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级篮球锦标赛",ms:"KEJOHANAN BOLA KERANJANG PERINGKAT SEKOLAH",en:"SCHOOL BASKETBALL TOURNAMENT"},
    tarikh:"14.05.2026",masa:"8:00 AM – 1:00 PM",
    venue:{zh:"晏斗中华小学篮球场",ms:"Gelanggang Bola Keranjang SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Basketball Court"},
    organizer:{zh:"体育组",ms:"Unit Sukan",en:"Sports Unit"},
    target:{zh:"四、五、六年级学生",ms:"MURID TAHUN 4, 5 & 6",en:"STANDARD 4, 5 & 6 STUDENTS"},
    attendance:{zh:"80 人",ms:"80 orang murid",en:"80 pupils"},
    pengenalan:{zh:"本校举办篮球锦标赛，旨在推行健康生活方式，提升学生篮球竞技水平，培养团队合作与体育精神。",ms:"Kejohanan bola keranjang dianjurkan bagi memupuk gaya hidup sihat, meningkatkan kemahiran sukan bola keranjang, serta menyemai semangat kesukanan dan kerja berpasukan.",en:"The school basketball tournament is organized to promote a healthy lifestyle, enhance students' basketball skills, and foster teamwork and sportsmanship."},
    objektif:{zh:"挖掘与发掘学生在篮球运动方面的潜能与天赋。\n培养团队合作协作能力、纪律性与战术执行力。\n促进不同年级学生之间的友谊与体育竞技交流。",ms:"Mencungkil bakat dan potensi murid dalam permainan bola keranjang.\nMemupuk semangat kerjasama berpasukan, disiplin, dan strategi permainan.\nMengeratkan silaturahim dan integrasi antaramurid pelbagai kelas.",en:"To identify and nurture students' basketball talents.\nTo foster teamwork, discipline, and strategic game execution.\nTo strengthen friendship and sportsmanship among students."},
    impak:{zh:"学生积极参与篮球运动，提升身体素质与反应敏捷度。\n选拔出优秀的学校篮球代表队成员参加县级(MSSD)比赛。\n增强学生的集体荣誉感与团队凝聚力。",ms:"Penglibatan aktif murid meningkatkan ketahanan fizikal dan kelajuan kendiri.\nBerjaya memilih wakil pemain sekolah yang berkualiti ke kejohanan peringkat MSSD.\nMeningkatkan semangat kekitaan dan daya juang pasukan.",en:"Active participation improved students' physical fitness and agility.\nSuccessfully selected qualified players to represent the school at MSSD level.\nBoosted team cohesion and school pride."},
    evidence:{zh:"比赛得分表、队伍名单、现场照片",ms:"Papan markah, senarai pasukan, foto kejohanan",en:"Score sheets, team lists, event photos"},
    strengths:{zh:"比赛组织有序，学生竞技水平高。",ms:"Penganjuran teratur, tahap permainan murid tinggi.",en:"Well-organized tournament with high skill levels."},
    challenges:{zh:"场地与裁判人手有限。",ms:"Gelanggang dan pengadil terhad.",en:"Limited courts and referees."},
    suggestions:{zh:"建议增加女子组比赛。",ms:"Cadangan menambah kategori perempuan.",en:"Add a girls' category."},
    officer:{zh:"张志明老师",ms:"En. Chong Chee Ming",en:"Mr. Chong Chee Ming"},
    duration:{zh:"1 天",ms:"1 Hari",en:"1 Day"},
    teacher:{zh:"张志明老师",ms:"EN. CHONG CHEE MING",en:"MR. CHONG CHEE MING"},
    position:{zh:"体育主任",ms:"SETIAUSAHA SUKAN",en:"SPORTS SECRETARY"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  yu_mao_qiu: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级羽毛球单打与双打锦标赛",ms:"KEJOHANAN BADMINTON PERINGKAT SEKOLAH",en:"SCHOOL BADMINTON CHAMPIONSHIP"},
    tarikh:"16.05.2026",masa:"8:00 AM – 1:00 PM",
    venue:{zh:"晏斗中华小学礼堂",ms:"Dewan SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Hall"},
    organizer:{zh:"羽毛球学会",ms:"Kelab Badminton",en:"Badminton Club"},
    target:{zh:"三至六年级学生",ms:"MURID TAHUN 3 - 6",en:"STANDARD 3 - 6 STUDENTS"},
    attendance:{zh:"60 人",ms:"60 orang murid",en:"60 pupils"},
    pengenalan:{zh:"羽毛球锦标赛旨在提供专业竞技平台，提高学生的眼手协调能力、体能耐力与单双打对战战术。",ms:"Kejohanan badminton dianjurkan bagi memberikan platform pertandingan rasmi, meningkatkan koordinasi mata-tangan serta ketahanan fizikal murid.",en:"The badminton championship aims to provide an official competition platform, enhancing students' hand-eye coordination and physical endurance."},
    objektif:{zh:"选拔优秀羽毛球苗子代表学校出战县级比赛。\n锻炼学生的反应速度、体能爆发力与敏捷度。\n培养胜不骄、败不馁的良好体育道德精神。",ms:"Memilih wakil sekolah berkualiti ke Kejohanan Badminton MSSD.\nMeningkatkan kelajuan tindak balas, ketahanan dan kelincahan murid.\nMemupuk sikap kesukanan yang tinggi dan berdisiplin.",en:"To select top badminton talents for district-level MSSD tournaments.\nTo boost reaction speed, physical endurance, and agility.\nTo instill good sportsmanship and resilience."},
    impak:{zh:"参赛选手展现了高超的球技与顽强的拼搏精神。\n成功遴选出男女单双打学校代表队名单。\n推动了全校羽毛球运动普及与体育风气。",ms:"Peserta menampilkan mutu permainan yang tinggi dan semangat pantang menyerah.\nBerjaya membentuk skuad perseorangan dan beregu sekolah.\nMenghidupkan budaya sukan badminton di peringkat sekolah.",en:"Participants displayed high skill levels and perseverance.\nSuccessfully formed the school singles and doubles badminton squad.\nPromoted an active badminton sporting culture school-wide."},
    evidence:{zh:"比赛成绩表、参赛名单、现场照片",ms:"Keputusan perlawanan, senarai peserta, foto",en:"Match results, participant list, photos"},
    strengths:{zh:"选手技术水平较高，比赛精彩。",ms:"Tahap kemahiran peserta tinggi, perlawanan menarik.",en:"High skill level and exciting matches."},
    challenges:{zh:"场地数量有限需分时段进行。",ms:"Bilangan gelanggang terhad, perlu mengikut sesi.",en:"Limited courts require staggered sessions."},
    suggestions:{zh:"建议增设混合双打项目。",ms:"Cadangan menambah acara beregu campuran.",en:"Add mixed doubles category."},
    officer:{zh:"林嘉伟老师",ms:"Pn. Lim Chia Wei",en:"Ms. Lim Chia Wei"},
    duration:{zh:"1 天",ms:"1 Hari",en:"1 Day"},
    teacher:{zh:"林嘉伟老师",ms:"PN. LIM CHIA WEI",en:"MS. LIM CHIA WEI"},
    position:{zh:"羽毛球学会负责老师",ms:"PENYELARAS KELAB BADMINTON",en:"BADMINTON CLUB COORDINATOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  ping_pong: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级乒乓锦标赛",ms:"KEJOHANAN PING PONG PERINGKAT SEKOLAH",en:"SCHOOL TABLE TENNIS CHAMPIONSHIP"},
    tarikh:"15.06.2026",masa:"8:00 AM – 1:00 PM",
    venue:{zh:"晏斗中华小学乒乓室",ms:"Bilik Ping Pong SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Table Tennis Room"},
    organizer:{zh:"乒乓学会",ms:"Kelab Ping Pong",en:"Table Tennis Club"},
    target:{zh:"三至六年级学生",ms:"MURID TAHUN 3 - 6",en:"STANDARD 3 - 6 STUDENTS"},
    attendance:{zh:"50 人",ms:"50 orang murid",en:"50 pupils"},
    pengenalan:{zh:"乒乓锦标赛旨在锻炼学生的动态视力、发球旋转战术、快速反应力与专注沉着的心理素质。",ms:"Kejohanan ping pong dianjurkan bagi membina kejituan kawalan bola, refleksi pantas, serta pemikiran strategik dalam sukan raket.",en:"The table tennis championship builds rapid reflexes, spin mastery, tactical precision, and mental focus under pressure."},
    objektif:{zh:"普及乒乓球运动，发掘具潜质的校队选手。\n提高眼手协调灵敏度与短距爆发反应。\n培养顽强拼搏与独立临场应变能力。",ms:"Mencungkil bakat baharu untuk menyertai skuad ping pong sekolah.\nMeningkatkan koordinasi mata-tangan dan daya refleks murid.\nMemupuk sifat berdisiplin dan fokus tinggi semasa perlawanan.",en:"To discover promising table tennis players for the school squad.\nTo sharpen hand-eye agility and quick tactical decision-making.\nTo build mental resilience and self-reliance during matches."},
    impak:{zh:"选拔出了男女组单打与双打校队预备人选。\n提高了选手的战术推挡与扣杀实战能力。\n激发了学生利用课余时间积极练球的风气。",ms:"Berjaya memilih wakil sekolah ke pertandingan peringkat MSSD.\nMutu perlawanan bertambah mantap dengan kawalan teknik yang berkesan.\nMemupuk minat berterusan terhadap sukan ping pong.",en:"Selected elite players for the MSSD district championship.\nUpgraded offensive and defensive technical play style.\nFostered active table tennis practice habits among students."},
    evidence:{zh:"比赛成绩、参赛名单、训练记录",ms:"Keputusan, senarai peserta, rekod latihan",en:"Results, participant list, training records"},
    strengths:{zh:"学生专注力高，技术提升明显。",ms:"Fokus murid tinggi, kemahiran meningkat.",en:"High focus and noticeable skill improvement."},
    challenges:{zh:"球台数量有限。",ms:"Bilangan meja terhad.",en:"Limited tables."},
    suggestions:{zh:"建议增加校际友谊赛。",ms:"Cadangan menambah perlawanan persahabatan.",en:"Add inter-school friendly matches."},
    officer:{zh:"李健伟老师",ms:"En. Lee Kin Wai",en:"Mr. Lee Kin Wai"},
    duration:{zh:"1 天",ms:"1 Hari",en:"1 Day"},
    teacher:{zh:"李健伟老师",ms:"EN. LEE KIN WAI",en:"MR. LEE KIN WAI"},
    position:{zh:"乒乓学会导师",ms:"PENYELARAS KELAB PING PONG",en:"TABLE TENNIS CLUB ADVISOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  pai_qiu: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级排球锦标赛",ms:"KEJOHANAN BOLA TAMPAR PERINGKAT SEKOLAH",en:"SCHOOL VOLLEYBALL TOURNAMENT"},
    tarikh:"20.05.2026",masa:"8:00 AM – 12:30 PM",
    venue:{zh:"晏斗中华小学排球場",ms:"Gelanggang Bola Tampar SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Volleyball Court"},
    organizer:{zh:"体育组",ms:"Unit Sukan",en:"Sports Unit"},
    target:{zh:"四、五、六年级学生",ms:"MURID TAHUN 4, 5 & 6",en:"STANDARD 4, 5 & 6 STUDENTS"},
    attendance:{zh:"70 人",ms:"70 orang murid",en:"70 pupils"},
    pengenalan:{zh:"举办排球比赛旨在培养学生的高空发球、传球、扣杀战术与高度默契的团队配合能力。",ms:"Kejohanan bola tampar bertujuan untuk memupuk kemahiran servis, menghadang, dan kemahiran berpasukan yang mantap dalam kalangan murid.",en:"The volleyball tournament aims to develop serving, blocking, and strong team coordination skills among upper primary students."},
    objektif:{zh:"强化学生的排球基本功与团队传接球战术默契。\n培养学生在紧要关头的心理承受力与合作意识。\n选拔排球小将加入校队训练营。",ms:"Menguasai kemahiran asas servis, hantaran, dan rejaman bola tampar.\nMemupuk ketahanan mental dan kerjasama pasukan semasa perlawanan.\nMemilih bakat baharu untuk menyertai skuad bola tampar sekolah.",en:"To master fundamental volleyball techniques: serve, pass, and spike.\nTo build mental toughness and team cooperation under pressure.\nTo recruit promising talents for the school volleyball squad."},
    impak:{zh:"各队展示了极佳的默契配合与精彩的扣杀防守。\n提高了学生的空间判断力与团队信任度。\n顺利选拔出优秀代表队参加区级赛事。",ms:"Pasukan menunjukkan persefahaman amat baik di atas gelanggang.\nMeningkatkan persepsi ruang dan nilai kepercayaan antara rakan sepasukan.\nBerjaya memilih calon wakil sekolah ke pertandingan peringkat daerah.",en:"Teams demonstrated exceptional court synergy and defensive plays.\nEnhanced spatial awareness and peer trust during matches.\nSuccessfully selected school representatives for district competitions."},
    evidence:{zh:"比赛记录、队伍名单、现场照片",ms:"Rekod perlawanan, senarai pasukan, foto",en:"Match records, team lists, photos"},
    strengths:{zh:"团队合作精神突出。",ms:"Semangat kerjasama tinggi.",en:"Outstanding teamwork."},
    challenges:{zh:"天气与场地影响赛程。",ms:"Cuaca dan gelanggang mempengaruhi jadual.",en:"Weather and venue affect schedule."},
    suggestions:{zh:"建议增设友谊赛与训练营。",ms:"Cadangan menambah perlawanan persahabatan dan kem latihan.",en:"Add friendly matches and training camps."},
    officer:{zh:"陈金伟老师",ms:"En. Chong Kin Wai",en:"Mr. Chong Kin Wai"},
    duration:{zh:"1 天",ms:"1 Hari",en:"1 Day"},
    teacher:{zh:"陈金伟老师",ms:"EN. CHONG KIN WAI",en:"MR. CHONG KIN WAI"},
    position:{zh:"排球校队教练",ms:"JURULATIH BOLA TAMPAR SEKOLAH",en:"SCHOOL VOLLEYBALL COACH"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  zu_qiu: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级五人制/十一人制足球赛",ms:"KEJOHANAN BOLA SEPAK PERINGKAT SEKOLAH",en:"SCHOOL FOOTBALL / FUTSAL CHAMPIONSHIP"},
    tarikh:"22.05.2026",masa:"8:00 AM – 1:00 PM",
    venue:{zh:"晏斗中华小学草场",ms:"Padang SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Field"},
    organizer:{zh:"足球学会",ms:"Kelab Bola Sepak",en:"Football Club"},
    target:{zh:"四、五、六年级学生",ms:"MURID TAHUN 4, 5 & 6",en:"STANDARD 4, 5 & 6 STUDENTS"},
    attendance:{zh:"80 人",ms:"80 orang murid",en:"80 pupils"},
    pengenalan:{zh:"足球赛旨在提高学生的运球控球、射门技术、体能耐力以及球场上的大局观与战术执行能力。",ms:"Kejohanan bola sepak dianjurkan bagi membina kecergasan fizikal, kemahiran mengawal bola, serta semangat kesukanan melalui permainan bola sepak.",en:"The football championship enhances physical endurance, dribbling skills, and strategic teamwork through pitch competition."},
    objektif:{zh:"普及足球运动，提升学生的下肢力量与有氧耐力。\n培养遵守裁判判罚的纪律性与团队战术执行力。\n为校级足球队遴选具有潜力的守门员与攻防球员。",ms:"Meningkatkan tahap kecergasan, kelajuan dan kawalan bola murid.\nMemupuk sikap berdisiplin serta mematuhi peraturan dan keputusan pengadil.\nMemilih pemain berpotensi untuk diserapkan ke dalam pasukan sekolah.",en:"To improve physical fitness, stamina, and ball control capabilities.\nTo instil discipline, rule compliance, and tactical execution.\nTo scout potential players for the school football team."},
    impak:{zh:"赛事精彩纷呈，学生展现了极高昂的拼搏斗志。\n建立了纪律严密的校级足球代表队初选名单。\n有效培养了学生的团队合作精神与顽强意志。",ms:"Perlawanan berlangsung rancak dengan semangat juang yang tinggi.\nBerjaya membentuk senarai awal pemain bakat sukan bola sepak sekolah.\nMemupuk semangat kerjasama dan ketahanan mental murid.",en:"High-energy matches showcased intense fighting spirit and skill.\nFormed a disciplined shortlist for the school football team.\nStrengthened student perseverance and collective sportsmanship."},
    evidence:{zh:"比赛成绩、球员名单、现场照片",ms:"Keputusan, senarai pemain, foto",en:"Results, player lists, photos"},
    strengths:{zh:"学生斗志高昂，比赛气氛热烈。",ms:"Semangat juang tinggi, suasana meriah.",en:"High fighting spirit and vibrant atmosphere."},
    challenges:{zh:"场地受天气影响。",ms:"Gelanggang terjejas oleh cuaca.",en:"Field affected by weather."},
    suggestions:{zh:"建议增加校内联赛制度。",ms:"Cadangan menambah liga dalaman sekolah.",en:"Add an internal school league."},
    officer:{zh:"法伊扎老师",ms:"En. Ahmad Faizal",en:"Mr. Ahmad Faizal"},
    duration:{zh:"1 天",ms:"1 Hari",en:"1 Day"},
    teacher:{zh:"法伊扎老师",ms:"EN. AHMAD FAIZAL",en:"MR. AHMAD FAIZAL"},
    position:{zh:"足球学会负责老师",ms:"PENYELARAS KELAB BOLA SEPAK",en:"FOOTBALL CLUB COORDINATOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  gao_er_fu: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校园高尔夫球体验与挥杆技能赛",ms:"KEJOHANAN & CABARAN SUKAN GOLF PERINGKAT SEKOLAH",en:"SCHOOL GOLF SKILLS & PUTTING CHALLENGE"},
    tarikh:"25.05.2026",masa:"8:00 AM – 11:30 AM",
    venue:{zh:"晏斗中华小学草场",ms:"Padang SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Field"},
    organizer:{zh:"特色体育项目组",ms:"Unit Sukan Khas",en:"Special Sports Unit"},
    target:{zh:"高年级感兴趣学生",ms:"MURID TAHUN 4, 5 & 6",en:"UPPER PRIMARY STUDENTS"},
    attendance:{zh:"30 人",ms:"30 orang murid",en:"30 pupils"},
    pengenalan:{zh:"高尔夫球技能赛旨在向学生推广高尔夫这项绅士运动，训练专注力、挥杆掌控力、自我约束与优雅的体育礼仪。",ms:"Pertandingan golf merangsang fokus mental, kawalan hayunan bola, serta etika sukan yang tinggi dalam kalangan murid.",en:"The golf event promotes mental focus, swing control, self-discipline, and gentlemanly sports etiquette among students."},
    objektif:{zh:"普及高尔夫球基本推杆、挥杆动作与比赛规则。\n培养学生极高的专注力、情绪管理与沉着冷静的心理素质。\n提供多元化体育运动选择，挖掘特色体育人才。",ms:"Memperkenalkan asas hayunan, pukulan, dan peraturan permainan golf.\nMemupuk fokus mental yang tinggi, kawalan emosi, dan ketenangan.\nMempelbagaikan pilihan sukan untuk bakat murid.",en:"To introduce basic golf posture, swing techniques, and match rules.\nTo build intense concentration, emotional control, and patience.\nTo diversify sports opportunities and identify specialized athletes."},
    impak:{zh:"学生掌握了基础推杆技巧，提高了专注与耐心。\n开拓了学生的体育视野，培养优雅的比赛礼仪。\n发现多名具备较强力量控制力的高尔夫球潜能学生。",ms:"Murid menguasai asas pukulan golf dengan kejituan dan fokus tinggi.\nMembuka minda murid terhadap kepelbagaian sukan berimpak tinggi.\nMengenal pasti beberapa murid yang mempunyai potensi kejuaraan golf.",en:"Students gained precision, control, and heightened focus.\nBroadened athletic horizons and promoted proper sporting decorum.\nIdentified promising talents with excellent swing potential."},
    evidence:{zh:"体验照片、技能评分表、学生名单",ms:"Foto aktiviti, borang penilaian, senarai murid",en:"Activity photos, skill assessment forms, student list"},
    strengths:{zh:"活动新颖，学生专注力高。",ms:"Aktiviti baharu, fokus murid tinggi.",en:"Novel activity with high student focus."},
    challenges:{zh:"器材数量有限。",ms:"Peralatan terhad.",en:"Limited equipment."},
    suggestions:{zh:"建议增加更多推杆练习器材。",ms:"Cadangan menambah peralatan latihan putting.",en:"Add more putting practice equipment."},
    officer:{zh:"吴伟祥老师",ms:"En. Ng Wei Siang",en:"Mr. Ng Wei Siang"},
    duration:{zh:"半天",ms:"Setengah Hari",en:"Half Day"},
    teacher:{zh:"吴伟祥老师",ms:"EN. NG WEI SIANG",en:"MR. NG WEI SIANG"},
    position:{zh:"特色体育项目组长",ms:"PENYELARAS SUKAN KHAS",en:"SPECIAL SPORTS COORDINATOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  ying_shi_lan_qiu: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级英式篮球（Netball）锦标赛",ms:"KEJOHANAN BOLA JARING PERINGKAT SEKOLAH",en:"SCHOOL NETBALL CHAMPIONSHIP"},
    tarikh:"28.05.2026",masa:"8:00 AM – 12:00 PM",
    venue:{zh:"晏斗中华小学篮球场",ms:"Gelanggang SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Court"},
    organizer:{zh:"英式篮球学会",ms:"Kelab Bola Jaring",en:"Netball Club"},
    target:{zh:"四、五、六年级女学生",ms:"MURID PEREMPUAN TAHUN 4, 5 & 6",en:"FEMALE STUDENTS (STD 4-6)"},
    attendance:{zh:"50 人",ms:"50 orang murid",en:"50 pupils"},
    pengenalan:{zh:"英式篮球锦标赛专为女学生设计，旨在提高控球定位传球、无球跑位战术、投篮精准度与团队无缝配合。",ms:"Kejohanan bola jaring diadakan khas untuk murid perempuan bagi meningkatkan kemahiran hantaran, pergerakan bebas, dan ketepatan jaringan.",en:"The netball championship offers female students an opportunity to hone ball passing accuracy, court positioning, and teamwork."},
    objektif:{zh:"选拔优秀女运动员组建学校英式篮球队。\n提高女学生的敏捷度、弹跳力与快速定位传球能力。\n培养严密的团队防守与进攻战术协同。",ms:"Memilih bakat atlet perempuan untuk pasukan bola jaring sekolah.\nMeningkatkan kelincahan, daya lompatan, dan ketepatan hantaran murid.\nMemupuk persefahaman taktik serangan dan pertahanan.",en:"To recruit promising female athletes for the school netball team.\nTo enhance agility, leaping power, and rapid passing precision.\nTo cultivate seamless team defense and offensive coordination."},
    impak:{zh:"参赛女学生展现了极佳的体能与无球跑位战术素养。\n成功组建了备战区级 MSSD 比赛的英式篮球代表队。\n极大增强了女学生对体育竞技的兴趣与自信心。",ms:"Murid perempuan menunjukkan kelincahan dan koordinasi padu.\nBerjaya membentuk skuad bola jaring sekolah ke MSSD.\nMeningkatkan minat dan keyakinan murid perempuan dalam sukan.",en:"Players displayed exceptional agility and court strategy.\nSuccessfully formed the school netball squad for district MSSD.\nBoosted confidence and sports participation among female students."},
    evidence:{zh:"比赛记录、球员名单、现场照片",ms:"Rekod perlawanan, senarai pemain, foto",en:"Match records, player lists, photos"},
    strengths:{zh:"女学生参与积极，团队配合良好。",ms:"Penyertaan murid perempuan aktif, kerjasama baik.",en:"Active female participation and good teamwork."},
    challenges:{zh:"训练时间有限。",ms:"Masa latihan terhad.",en:"Limited training time."},
    suggestions:{zh:"建议增加定期训练与友谊赛。",ms:"Cadangan menambah latihan berkala dan perlawanan persahabatan.",en:"Add regular training and friendly matches."},
    officer:{zh:"颜玉兰老师",ms:"Pn. Gan Yok Lan",en:"Ms. Gan Yok Lan"},
    duration:{zh:"1 天",ms:"1 Hari",en:"1 Day"},
    teacher:{zh:"颜玉兰老师",ms:"PN. GAN YOK LAN",en:"MS. GAN YOK LAN"},
    position:{zh:"英式篮球负责老师",ms:"PENYELARAS KELAB BOLA JARING",en:"NETBALL CLUB COORDINATOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  merentas_desa: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级越野赛跑比赛",ms:"KEJOHANAN MERENTAS DESA PERINGKAT SEKOLAH",en:"SCHOOL CROSS COUNTRY CHAMPIONSHIP"},
    tarikh:"20.01.2026",masa:"7:30 AM – 10:30 AM",
    venue:{zh:"晏斗中华小学校园及周边",ms:"Kawasan SJKC Chung Hua Rantau & persekitaran",en:"SJKC Chung Hua Rantau Campus & Surroundings"},
    organizer:{zh:"体育组",ms:"Unit Sukan",en:"Sports Unit"},
    target:{zh:"三至六年级全体学生",ms:"SEMUA MURID TAHUN 3 - 6",en:"ALL STANDARD 3 - 6 STUDENTS"},
    attendance:{zh:"200 人",ms:"200 orang murid",en:"200 pupils"},
    pengenalan:{zh:"越野赛跑比赛是年度体能大检阅，旨在培养学生强健的体魄、心肺有氧耐力与长跑意志力。",ms:"Kejohanan merentas desa merupakan aktiviti tahunan bagi membina ketahanan fizikal, kardiovaskular, serta semangat juang tinggi dalam kalangan murid.",en:"The cross country championship tests endurance, cardiorespiratory fitness, and physical determination across outdoor terrains."},
    objektif:{zh:"提高学生的心肺功能、有氧体能耐力与身体素质。\n培养勇于克服长途疲劳、永不放弃的顽强拼搏精神。\n遴选长跑健将代表学校参加县级越野赛跑。",ms:"Meningkatkan tahap kecergasan fizikal dan kardiovaskular murid.\nMemupuk ketahanan mental, kecekalan, dan pantang menyerah.\nMemilih pelari jarak jauh sekolah ke Kejohanan Merentas Desa MSSD.",en:"To boost physical stamina, cardiovascular health, and endurance.\nTo build mental grit, resilience, and perseverance against fatigue.\nTo select distance runners for the MSSD cross country championship."},
    impak:{zh:"全体参赛学生安全顺利完成规定路线跑程。\n增强了学生的健康运动意识与意志品质。\n选拔出了一支精干的校级长跑越野代表队。",ms:"Murid Berjaya menamatkan senarai laluan larian dengan selamat.\nKesedaran penjagaan kesihatan dan ketahanan stamina meningkat.\nSkuad pelari merentas desa sekolah berjaya dibentuk.",en:"All runners safely completed the designated outdoor running routes.\nEnhanced long-distance running fitness and mental toughness.\nFormed a strong cross country representative team."},
    evidence:{zh:"赛程路线图、成绩记录、现场照片",ms:"Peta laluan, rekod keputusan, foto",en:"Route map, results records, photos"},
    strengths:{zh:"学生坚持完成全程，意志力强。",ms:"Murid berjaya menamatkan larian, semangat tinggi.",en:"Students completed the full route with strong determination."},
    challenges:{zh:"路线安全与补水需加强。",ms:"Keselamatan laluan dan air minuman perlu diperkukuh.",en:"Route safety and hydration need strengthening."},
    suggestions:{zh:"建议增加沿途医疗支援。",ms:"Cadangan menambah bantuan perubatan di laluan.",en:"Add medical support along the route."},
    officer:{zh:"张志明老师",ms:"En. Chong Chee Ming",en:"Mr. Chong Chee Ming"},
    duration:{zh:"半天",ms:"Setengah Hari",en:"Half Day"},
    teacher:{zh:"张志明老师",ms:"EN. CHONG CHEE MING",en:"MR. CHONG CHEE MING"},
    position:{zh:"越野赛跑主教练",ms:"PENYELARAS MERENTAS DESA",en:"CROSS COUNTRY COORDINATOR"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  lompat_tinggi: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级田径跳高比赛",ms:"PERTANDINGAN LOMPAT TINGGI PERINGKAT SEKOLAH",en:"SCHOOL HIGH JUMP COMPETITION"},
    tarikh:"15.07.2026",masa:"8:00 AM – 11:00 AM",
    venue:{zh:"晏斗中华小学草场",ms:"Padang SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Field"},
    organizer:{zh:"田径队",ms:"Pasukan Olahraga",en:"Athletics Team"},
    target:{zh:"四、五、六年级学生",ms:"MURID TAHUN 4, 5 & 6",en:"STANDARD 4, 5 & 6 STUDENTS"},
    attendance:{zh:"40 人",ms:"40 orang murid",en:"40 pupils"},
    pengenalan:{zh:"跳高比赛旨在训练学生的助跑节奏、起跳弹跳力、空中过杆姿势与落地安全技巧。",ms:"Pertandingan lompat tinggi diadakan bagi menguji daya lompatan, teknik larian landas, dan gaya pelepasan di atas palang.",en:"The high jump competition tests take-off vertical leap, approach run timing, and bar clearance techniques."},
    objektif:{zh:"掌握正确的助跑、起跳与过杆姿势规范。\n提高下肢肌肉弹跳力与身体空间协调能力。\n选拔优秀的跳高苗子代表学校出赛。",ms:"Menguasai teknik larian landas dan daya lonjakan yang betul.\nMeningkatkan kuasa otot kaki dan koordinasi badan di udara.\nMemilih wakil sekolah bagi acara padang lompat tinggi.",en:"To master run-up rhythm and bar clearance postures accurately.\nTo build leg explosive power and spatial body coordination.\nTo scout high jump athletes for inter-school athletics."},
    impak:{zh:"选手们屡创新高，刷新了校园跳高纪录。\n提升了高年级学生的下肢爆发力与动作技巧。\n选拔出优秀代表加入校级田径训练营。",ms:"Peserta memperlihatkan teknik lonjakan yang meyakinkan.\nDaya pecutan dan kekuatan otot kaki murid meningkat.\nBerjaya memilih wakil ke latihan berfokus MSSD.",en:"Athletes displayed high vertical leaps and sound posture.\nImproved explosive leg strength and jump timing skills.\nRecruited top performers into the intensive track & field squad."},
    evidence:{zh:"成绩记录、参赛名单、现场照片",ms:"Rekod keputusan, senarai peserta, foto",en:"Results records, participant list, photos"},
    strengths:{zh:"选手动作规范，成绩优异。",ms:"Teknik peserta baik, keputusan memberangsangkan.",en:"Good technique and commendable results."},
    challenges:{zh:"器材与安全设施需完善。",ms:"Peralatan dan kemudahan keselamatan perlu ditambah baik.",en:"Equipment and safety facilities need improvement."},
    suggestions:{zh:"建议增加专项训练时段。",ms:"Cadangan menambah sesi latihan khusus.",en:"Add dedicated training sessions."},
    officer:{zh:"李志明老师",ms:"En. Lee Chee Ming",en:"Mr. Lee Chee Ming"},
    duration:{zh:"半天",ms:"Setengah Hari",en:"Half Day"},
    teacher:{zh:"李志明老师",ms:"EN. LEE CHEE MING",en:"MR. LEE CHEE MING"},
    position:{zh:"跳高专项教练",ms:"JURULATIH LOMPAT TINGGI",en:"HIGH JUMP COACH"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG SIEW LING",en:"MS. WONG SIEW LING"},
    verifiedRole:{zh:"晏斗中华小学校长",ms:"Guru Besar SJK(C) Chung Hua Rantau",en:"Headmistress SJK(C) Chung Hua Rantau"}
  },
  lompat_jauh: {
    school:{zh:"晏斗中华小学 SJK (C) CHUNG HUA RANTAU",ms:"SJK (C) CHUNG HUA RANTAU",en:"SJK (C) CHUNG HUA RANTAU"},
    title:{zh:"校级田径跳远比赛",ms:"PERTANDINGAN LOMPAT JAUH PERINGKAT SEKOLAH",en:"SCHOOL LONG JUMP COMPETITION"},
    tarikh:"16.07.2026",masa:"8:00 AM – 11:00 AM",
    venue:{zh:"晏斗中华小学草场",ms:"Padang SJKC Chung Hua Rantau",en:"SJKC Chung Hua Rantau Field"},
    organizer:{zh:"田径队",ms:"Pasukan Olahraga",en:"Athletics Team"},
    target:{zh:"四、五、六年级学生",ms:"MURID TAHUN 4, 5 & 6",en:"STANDARD 4, 5 & 6 STUDENTS"},
    attendance:{zh:"40 人",ms:"40 orang murid",en:"40 pupils"},
    pengenalan:{zh:"跳远比赛旨在锻炼学生的快速助跑、精准踏板起跳、空中展体与沙坑落地技巧，考验爆发力与协调性。",ms:"Pertandingan lompat jauh dianjurkan bagi menguji kelajuan larian landas, ketepatan pijakan papan, serta jarak lompatan ke dalam kawasan pasir.",en:"The long jump competition challenges horizontal leap distance through explosive sprinting speed and precise take-off timing."},
    objektif:{zh:"熟练掌握助跑加速、踏板不踩线与双脚安全落地的动作。\n增强冲刺速度向水平跳远距离转化的能力。\n挑选合格选手参加县级田径锦标赛跳远项目。",ms:"Menguasai kemahiran memecut, mendarat, dan ketepatan lonjakan.\nMeningkatkan gabungan kelajuan larian dan daya lonjakan.\nMemilih peserta cemerlang bagi acara lompat jauh sekolah.",en:"To refine sprint approach, board precision, and safe landing.\nTo convert sprinting momentum into maximum horizontal distance.\nTo select long jump field athletes for district track meets."},
    impak:{zh:"参赛选手跳出了优异的远度成绩。\n增强了学生的速度爆发与下肢力量。\n成功组建了跳远项目的校队集训名单。",ms:"Peserta berjaya mencatatkan jarak lompatan yang mengagumkan.\nMeningkatkan ketangkasan fizikal dan kelajuan larian.\nSkuad acara padang lompat jauh sekolah berjaya dibentuk.",en:"Participants recorded impressive horizontal distance jumps.\nEnhanced sprint acceleration and leg explosive power.\nFormed a solid long jump lineup for upcoming athletics meets."},
    evidence:{zh:"成绩记录、参赛名单、现场照片",ms:"Rekod keputusan, senarai peserta, foto",en:"Results records, participant list, photos"},
    strengths:{zh:"学生爆发力强，成绩理想。",ms:"Daya lonjakan murid tinggi, keputusan baik.",en:"Strong explosive power and good results."},
    challenges:{zh:"沙坑维护需加强。",ms:"Penyelenggaraan pasir perlu diperkukuh.",en:"Sand pit maintenance needs strengthening."},
    suggestions:{zh:"建议增加起跳板训练。",ms:"Cadangan menambah latihan papan lonjakan.",en:"Add take-off board training."},
    officer:{zh:"陈金伟老师",ms:"En. Chong Kin Wai",en:"Mr. Chong Kin Wai"},
    duration:{zh:"半天",ms:"Setengah Hari",en:"Half Day"},
    teacher:{zh:"陈金伟老师",ms:"EN. CHONG KIN WAI",en:"MR. CHONG KIN WAI"},
    position:{zh:"跳远专项教练",ms:"JURULATIH LOMPAT JAUH",en:"LONG JUMP COACH"},
    verifiedName:{zh:"黄秀玲校长",ms:"PN. WONG
