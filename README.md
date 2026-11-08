<html lang="zh">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>One Page Report (OPR) WebApp</title>
  <style>
    :root {
      --report-font: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    * { box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; }
    body { background-color: #0f172a; color: #f8fafc; margin: 0; padding: 0; min-height: 100vh; display: flex; flex-direction: column; }

    /* WebApp Top Navbar */
    .navbar {
      background-color: #1e293b; border-bottom: 1px solid #334155; padding: 10px 24px;
      display: flex; justify-content: space-between; align-items: center; position: sticky; top: 0; z-index: 100;
      flex-wrap: wrap; gap: 10px;
    }
    .navbar .brand { font-size: 1.15rem; font-weight: bold; color: #38bdf8; display: flex; align-items: center; gap: 8px; }
    .navbar .actions { display: flex; align-items: center; gap: 10px; flex-wrap: wrap; }

    .lang-select {
      background-color: #0f172a; color: #38bdf8; border: 1px solid #38bdf8;
      padding: 6px 10px; border-radius: 6px; font-weight: 600; font-size: 0.85rem; cursor: pointer;
    }

    /* Mobile View Mode Switcher (仅在手机端显示) */
    .mobile-view-tabs {
      display: none;
      width: 100%;
      background: #0f172a;
      padding: 6px;
      border-bottom: 1px solid #334155;
      gap: 8px;
    }
    .mobile-tab-btn {
      flex: 1;
      padding: 8px;
      border: 1px solid #334155;
      background: #1e293b;
      color: #94a3b8;
      border-radius: 6px;
      font-weight: bold;
      font-size: 0.85rem;
      cursor: pointer;
      text-align: center;
    }
    .mobile-tab-btn.active {
      background: #0284c7;
      color: #ffffff;
      border-color: #38bdf8;
    }

    /* WebApp Main Layout */
    .app-body { display: flex; flex: 1; overflow: hidden; height: calc(100vh - 57px); }

    /* Left Control Sidebar */
    .sidebar {
      width: 420px; background-color: #1e293b; border-right: 1px solid #334155; padding: 20px;
      overflow-y: auto; flex-shrink: 0; display: flex; flex-direction: column; gap: 16px;
    }
    .card { background-color: #0f172a; border: 1px solid #334155; border-radius: 8px; padding: 14px; }
    .card-title { font-size: 0.9rem; font-weight: bold; color: #38bdf8; margin-bottom: 12px; border-bottom: 1px solid #1e293b; padding-bottom: 6px; }
    
    .form-group { margin-bottom: 10px; }
    .form-group label { display: block; font-size: 0.8rem; font-weight: 600; color: #94a3b8; margin-bottom: 4px; }
    .form-group input, .form-group select, .form-group textarea {
      width: 100%; padding: 8px 10px; background-color: #1e293b; border: 1px solid #334155; border-radius: 6px;
      color: #f8fafc; font-size: 0.85rem; transition: 0.2s;
    }
    .form-group input:focus, .form-group select:focus, .form-group textarea:focus {
      outline: none; border-color: #38bdf8; box-shadow: 0 0 0 2px rgba(56, 189, 248, 0.2);
    }
    .form-group textarea { resize: vertical; height: 60px; }

    .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
    .grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 8px; }

    /* Button Styles */
    .btn {
      padding: 8px 14px; border-radius: 6px; font-size: 0.85rem; font-weight: 600; cursor: pointer;
      border: none; display: inline-flex; align-items: center; justify-content: center; gap: 6px; transition: 0.2s;
    }
    .btn-primary { background-color: #0284c7; color: white; }
    .btn-primary:hover { background-color: #0369a1; }
    .btn-success { background-color: #16a34a; color: white; }
    .btn-success:hover { background-color: #15803d; }
    .btn-secondary { background-color: #334155; color: #f8fafc; }
    .btn-secondary:hover { background-color: #475569; }
    .btn-danger { background-color: #dc2626; color: white; }
    .btn-danger:hover { background-color: #b91c1c; }

    /* Right Preview Canvas */
    .preview-area {
      flex: 1; background-color: #020617; padding: 20px; overflow-y: auto; overflow-x: auto;
      display: flex; justify-content: center; align-items: flex-start;
    }

    /* A4 Portrait Page Alignment (210mm x 297mm) */
    .a4-page {
      width: 210mm; height: 297mm; background-color: #ffffff; color: #000000;
      padding: 10mm 12mm; box-shadow: 0 20px 25px -5px rgba(0,0,0,0.5);
      display: flex; flex-direction: column; justify-content: space-between; position: relative; flex-shrink: 0;
      transform-origin: top center;
    }

    /* 应用动态字体到 A4 页面内部所有元素 */
    .a4-page, .a4-page * {
      font-family: var(--report-font) !important;
    }

    /* Header 区域 */
    .header-layout { 
      display: flex; justify-content: center; align-items: center; position: relative; 
      margin-bottom: 6px; min-height: 65px; 
    }
    .header-left { 
      width: 100%; text-align: center; display: flex; justify-content: center; align-items: center; 
      padding: 0 70px;
    }
    .title-school { 
      font-size: 17pt; font-weight: bold; color: #000000; margin: 0; line-height: 1.2; text-align: center; 
    }

    .logo-box { 
      position: absolute; right: 0; top: 50%; transform: translateY(-50%); 
      width: 65px; height: 65px; display: flex; align-items: center; justify-content: center; flex-shrink: 0; 
    }
    .logo-box img { max-width: 100%; max-height: 100%; object-fit: contain; }

    /* Yellow Banner */
    .banner-opr {
      background-color: #ffd700; color: #000; text-align: center; font-weight: 900;
      font-size: 11pt; padding: 2px 0; letter-spacing: 2px; border: 1.5px solid #000; margin-bottom: 6px;
    }

    /* Program Details Table */
    .info-table { width: 100%; border-collapse: collapse; border: 1.5px solid #000; margin-bottom: 6px; font-size: 8pt; font-weight: bold; }
    .info-table td { border: 1px solid #000; padding: 2px 5px; }
    .label-bg { background-color: #ffffff; color: #000; }
    .val-text { color: #000000; font-size: 8.5pt; font-weight: bold; }

    /* Dark Blue Header Containers */
    .box-container { border: 1.5px solid #000; margin-bottom: 6px; }
    .box-header { background-color: #1e3a8a; color: #ffffff; font-weight: bold; font-size: 8.5pt; padding: 2px 6px; border-bottom: 1px solid #000; letter-spacing: 0.5px; }
    .box-content { padding: 4px 6px; font-size: 7.5pt; line-height: 1.3; white-space: pre-line; color: #000; min-height: 36px; }

    /* 2x2 Photo Grid Layout */
    .photos-grid { display: grid; grid-template-columns: 1fr 1fr; grid-template-rows: 1fr 1fr; gap: 6px; flex: 1; min-height: 0; margin-bottom: 6px; }
    .photo-cell { border: 1.5px solid #000; background: #f8fafc; display: flex; flex-direction: column; align-items: center; justify-content: center; overflow: hidden; position: relative; }
    .photo-cell img { width: 100%; height: 100%; object-fit: cover; }
    .photo-ph { color: #94a3b8; font-size: 8pt; font-weight: bold; }
    
    /* Photo Caption Style */
    .photo-caption {
      position: absolute; bottom: 0; left: 0; right: 0;
      background: rgba(0, 0, 0, 0.65); color: #ffffff;
      font-size: 7pt; font-weight: 600; text-align: center;
      padding: 2px 4px; line-height: 1.1; word-break: break-word;
    }

    /* Reporter Footer Box */
    .pelapor-box { border: 1.5px solid #000; padding: 4px 6px; background-color: #fff; }
    .pelapor-title { font-size: 8pt; font-weight: bold; color: #000000; text-decoration: underline; margin-bottom: 1px; }
    .pelapor-name { font-size: 8pt; font-weight: bold; color: #000; }
    .pelapor-pos { font-size: 7.5pt; font-weight: normal; color: #333; }

   /* 📱 手机端/小屏响应式适配 CSS */
    @media screen and (max-width: 900px) {
      .navbar { padding: 8px 12px; }
      .navbar .brand { font-size: 1rem; }
      .navbar .actions { width: 100%; justify-content: space-between; gap: 6px; }
      .navbar .actions .btn, .navbar .actions select { flex: 1; font-size: 0.75rem; padding: 6px 4px; white-space: nowrap; }
      .mobile-view-tabs { display: flex; }
      .app-body { flex-direction: column; height: calc(100vh - 105px); position: relative; }
      .sidebar { width: 100%; height: 100%; border-right: none; padding: 12px; }
      .preview-area { width: 100%; height: 100%; padding: 10px; display: none; align-items: flex-start; justify-content: center; overflow-x: hidden; }
      .a4-page { max-width: 100%; height: auto; aspect-ratio: 210 / 297; transform: scale(0.95); transform-origin: top center; margin: 0 auto; }
      .grid-2, .grid-3 { grid-template-columns: 1fr; }
    }

    @media screen and (max-width: 480px) {
      .a4-page { transform: scale(1); margin: 0 auto; }
    }

    /* Print / PDF Export Styling */
    @media print {
      * { -webkit-print-color-adjust: exact !important; print-color-adjust: exact !important; }
      body { background: white; margin: 0; padding: 0; }
      .navbar, .sidebar, .mobile-view-tabs { display: none !important; }
      .app-body { height: auto; overflow: visible; display: block; }
      .preview-area { padding: 0; background: white; display: flex !important; }
      .a4-page { box-shadow: none; border: none; width: 210mm; height: 297mm; padding: 8mm 10mm; page-break-after: always; page-break-inside: avoid; overflow: hidden; transform: none !important; margin: 0 !important; }
      @page { size: A4 portrait; margin: 0; }
    }
  

    /* =========================================================
       MODERN UI REFRESH — OPR GENERATOR
       Application interface only; report content and JS retained.
       ========================================================= */
    :root {
      --ui-bg: #f3f6fb;
      --ui-surface: #ffffff;
      --ui-surface-soft: #f8fafc;
      --ui-primary: #2563eb;
      --ui-primary-dark: #1d4ed8;
      --ui-teal: #0f9f9a;
      --ui-text: #172033;
      --ui-muted: #64748b;
      --ui-border: #e2e8f0;
      --ui-shadow: 0 8px 24px rgba(15, 23, 42, .06);
    }

    html { scroll-behavior: smooth; }
    body {
      background: var(--ui-bg);
      color: var(--ui-text);
      font-family: Inter, "Segoe UI", Arial, sans-serif;
    }

    .navbar {
      min-height: 72px;
      padding: 12px 26px;
      background: rgba(255,255,255,.96);
      border-bottom: 1px solid var(--ui-border);
      box-shadow: 0 2px 12px rgba(15,23,42,.04);
      backdrop-filter: blur(12px);
    }
    .navbar .brand {
      color: #1e40af;
      font-size: 1.18rem;
      letter-spacing: -.35px;
      gap: 11px;
    }
    .navbar .brand > span:first-child {
      display: inline-flex;
      align-items: center;
      justify-content: center;
      width: 38px;
      height: 38px;
      border-radius: 12px;
      background: linear-gradient(135deg, #2563eb, #0f9f9a);
      color: white;
      font-size: 1.1rem;
    }
    .navbar .actions { gap: 9px; }
    .lang-select {
      min-height: 38px;
      padding: 8px 12px;
      background: #f8fafc;
      color: #334155;
      border: 1px solid var(--ui-border);
      border-radius: 10px;
      font-size: .84rem;
    }

    .btn {
      min-height: 38px;
      padding: 9px 14px;
      border-radius: 10px;
      font-size: .82rem;
      font-weight: 650;
      letter-spacing: .05px;
      box-shadow: none;
      transition: transform .18s ease, box-shadow .18s ease, background .18s ease;
    }
    .btn:hover {
      transform: translateY(-1px);
      box-shadow: 0 5px 12px rgba(15,23,42,.12);
    }
    .btn-primary { background: var(--ui-primary); }
    .btn-primary:hover { background: var(--ui-primary-dark); }
    .btn-success { background: #0f9f79; }
    .btn-success:hover { background: #0b8062; }
    .btn-secondary {
      background: #eef2ff;
      color: #3346a5;
      border: 1px solid #e0e7ff;
    }
    .btn-secondary:hover { background: #e0e7ff; }
    .btn-danger {
      background: #fff1f2;
      color: #be123c;
      border: 1px solid #ffe4e6;
    }
    .btn-danger:hover { background: #ffe4e6; }

    .app-body {
      height: calc(100vh - 72px);
      min-height: 500px;
      background: var(--ui-bg);
    }
    .sidebar {
      width: 390px;
      padding: 22px 20px 30px;
      gap: 15px;
      background: #f7f9fd;
      border-right: 1px solid var(--ui-border);
      scrollbar-width: thin;
      scrollbar-color: #cbd5e1 transparent;
    }
    .card {
      position: relative;
      padding: 17px;
      background: var(--ui-surface);
      border: 1px solid #e8edf5;
      border-radius: 15px;
      box-shadow: var(--ui-shadow);
      transition: border-color .2s ease, box-shadow .2s ease;
    }
    .card:hover {
      border-color: #cbdaf5;
      box-shadow: 0 10px 28px rgba(37,99,235,.08);
    }
    .card-title {
      display: flex;
      align-items: center;
      gap: 8px;
      margin: 0 0 15px;
      padding: 0 0 11px;
      color: #1e40af;
      border-bottom: 1px solid #edf1f7;
      font-size: .91rem;
      font-weight: 750;
      letter-spacing: -.15px;
    }
    .form-group { margin-bottom: 13px; }
    .form-group:last-child { margin-bottom: 0; }
    .form-group label {
      margin-bottom: 6px;
      color: #475569;
      font-size: .78rem;
      font-weight: 700;
      letter-spacing: .1px;
    }
    .form-group input,
    .form-group select,
    .form-group textarea {
      width: 100%;
      min-height: 39px;
      padding: 9px 11px;
      background: #fbfcfe;
      color: #1e293b;
      border: 1px solid #dbe3ef;
      border-radius: 9px;
      font-family: inherit;
      font-size: .84rem;
      transition: border-color .18s ease, box-shadow .18s ease, background .18s ease;
    }
    .form-group input:hover,
    .form-group select:hover,
    .form-group textarea:hover { border-color: #a9bddc; }
    .form-group input:focus,
    .form-group select:focus,
    .form-group textarea:focus {
      outline: none;
      background: #fff;
      border-color: #6094f5;
      box-shadow: 0 0 0 3px rgba(37,99,235,.12);
    }
    .form-group input[type="file"] {
      padding: 7px;
      background: #f8fafc;
      font-size: .76rem;
    }
    .form-group input[type="file"]::file-selector-button {
      margin-right: 9px;
      padding: 6px 10px;
      color: #1d4ed8;
      background: #eaf1ff;
      border: 0;
      border-radius: 7px;
      font-weight: 650;
      cursor: pointer;
    }
    .form-group textarea {
      min-height: 72px;
      line-height: 1.55;
      resize: vertical;
    }
    .grid-2 { gap: 12px; }
    .grid-3 { gap: 9px; }

    .preview-area {
      padding: 30px;
      background:
        radial-gradient(circle at 12% 10%, rgba(59,130,246,.07), transparent 25%),
        radial-gradient(circle at 90% 85%, rgba(20,184,166,.07), transparent 28%),
        #edf2f8;
      scrollbar-width: thin;
    }
    .a4-page {
      box-shadow: 0 18px 55px rgba(15,23,42,.16), 0 2px 8px rgba(15,23,42,.08);
      border: 1px solid #e5e7eb;
    }

    /* Refined report colour accents */
    .banner-opr {
      background: #e8f1ff;
      color: #1e40af;
      border: 1.5px solid #1e40af;
      border-radius: 3px;
      letter-spacing: 2.2px;
    }
    .label-bg {
      background: #eff6ff;
      color: #1e3a8a;
    }
    .box-container { border-color: #64748b; }
    .box-header {
      background: #1e3a8a;
      border-bottom-color: #1e3a8a;
    }
    .photo-cell {
      border-color: #94a3b8;
      background: #f1f5f9;
    }
    .pelapor-box { border-color: #64748b; }

    .mobile-view-tabs {
      background: #fff;
      border-bottom: 1px solid var(--ui-border);
      padding: 9px 12px;
      gap: 9px;
    }
    .mobile-tab-btn {
      padding: 10px;
      color: #64748b;
      background: #f1f5f9;
      border: 1px solid #e2e8f0;
      border-radius: 10px;
      font-size: .82rem;
    }
    .mobile-tab-btn.active {
      color: #fff;
      background: linear-gradient(135deg, #2563eb, #0f9f9a);
      border-color: transparent;
      box-shadow: 0 4px 10px rgba(37,99,235,.2);
    }

    #aiCategoryBadge {
      color: #0f766e !important;
      background: #ecfdf5 !important;
      border-color: #a7f3d0 !important;
      border-radius: 9px !important;
    }

    ::selection { background: #bfdbfe; color: #1e3a8a; }
    * { scrollbar-width: thin; scrollbar-color: #cbd5e1 transparent; }

    @media screen and (min-width: 901px) and (max-width: 1280px) {
      .sidebar { width: 340px; }
    }

    @media screen and (max-width: 900px) {
      .navbar {
        padding: 11px 14px;
        gap: 10px;
      }
      .navbar .actions {
        width: 100%;
        justify-content: stretch;
        gap: 7px;
      }
      .navbar .actions .btn,
      .navbar .actions select {
        flex: 1;
        min-width: 0;
        min-height: 36px;
        padding: 7px 5px;
        font-size: .72rem;
        border-radius: 8px;
      }
      .app-body {
        height: calc(100dvh - 112px);
        min-height: 0;
      }
      .sidebar {
        padding: 14px;
        gap: 12px;
      }
      .card { padding: 15px; border-radius: 13px; }
      .preview-area { padding: 14px 8px; }
      .grid-2 { grid-template-columns: 1fr 1fr; }
      .grid-3 { grid-template-columns: 1fr 1fr; }
    }
    @media screen and (max-width: 480px) {
      .navbar .brand { font-size: .95rem; }
      .navbar .brand > span:first-child {
        width: 33px; height: 33px; border-radius: 10px;
      }
      .grid-2 { grid-template-columns: 1fr; }
      .grid-3 { grid-template-columns: 1fr 1fr; }
      .a4-page { transform: scale(1); }
      .sidebar { padding: 11px; }
      .card { padding: 13px; }
    }

    @media print {
      .banner-opr {
        background: #e8f1ff !important;
        color: #1e40af !important;
        border: 1.5px solid #1e40af !important;
      }
      .label-bg { background: #eff6ff !important; }
      .box-header {
        background: #1e3a8a !important;
        color: #fff !important;
      }
    }

  </style>
</head>
<body>
  <!-- Top Navbar -->
  <header class="navbar">
    <div class="brand">📄 <span id="ui_appTitle">OPR 报告生成器</span></div>
    <div class="actions">
      <select id="langSelect" class="lang-select" onchange="switchLanguage(this.value)">
        <option value="zh" selected>中文</option>
        <option value="ms">Bahasa Melayu</option>
        <option value="en">English</option>
      </select>
      <button class="btn btn-secondary" onclick="saveDraft()"><span id="ui_btnSave">💾 保存</span></button>
      <button class="btn btn-secondary" onclick="loadDraft()"><span id="ui_btnLoad">📂 加载</span></button>
      <button class="btn btn-danger" onclick="resetForm()"><span id="ui_btnReset">🔄 重置</span></button>
      <button class="btn btn-success" onclick="window.print()"><span id="ui_btnPrint">🖨️ 导出 PDF</span></button>
    </div>
  </header>

  <!-- 📱 手机端快速切换标签栏 -->
  <div class="mobile-view-tabs">
    <div class="mobile-tab-btn active" id="tabEdit" onclick="switchMobileTab('edit')">✏️ 编辑内容</div>
    <div class="mobile-tab-btn" id="tabPreview" onclick="switchMobileTab('preview')">👁️ 预览报告</div>
  </div>

  <!-- Main WebApp Interface -->
  <div class="app-body">
    <!-- Left Sidebar -->
    <aside class="sidebar" id="sidebarArea">
      <!-- (Form inputs structured exactly as provided in the source) -->
      <div class="card">
        <div class="card-title" id="ui_secFont">🎨 字体风格设置</div>
        <div class="form-group">
          <label id="ui_lblFont">选择字体风格</label>
          <select id="fontSelect" onchange="updateFont(this.value)"></select>
        </div>
      </div>
      <div class="card">
        <div class="card-title" id="ui_secPreset">📌 学校活动预设</div>
        <div class="form-group">
          <label id="ui_lblPreset">选择活动预设</label>
          <select id="programSelect" onchange="autoFillProgram()"></select>
        </div>
      </div>
      <div class="card">
        <div class="card-title" id="ui_secSchool">🏫 学校与校徽设置</div>
        <div class="form-group">
          <label id="ui_lblLogo">上传学校 Logo / 校徽</label>
          <input type="file" id="logoInput" accept="image/*" onchange="loadLogo(event)">
        </div>
        <div class="form-group">
          <label id="ui_lblSchoolName">学校名称</label>
          <input type="text" id="inputSchoolName" value="SJKC PEI HUA" oninput="updateView()">
        </div>
      </div>
      <div class="card">
        <div class="card-title" id="ui_secDetails">📋 活动信息</div>
        <div class="form-group">
          <label id="ui_lblTitle">活动名称</label>
          <div style="display: flex; gap: 6px;">
            <input type="text" id="inputTitle" placeholder="输入活动名称（如：县级羽毛球比赛、参观科技馆一日游...）" oninput="updateView()" style="flex: 1;">
            <button type="button" class="btn" onclick="aiAutoSuggest()" id="ui_btnAiSuggest" style="background: linear-gradient(135deg, #6366f1, #0284c7); color: white; white-space: nowrap; font-size: 0.8rem; border: none; font-weight: bold; padding: 6px 12px; border-radius: 6px; cursor: pointer;">✨ AI 智能建议</button>
          </div>
          <div id="aiCategoryBadge" style="display:none; margin-top:6px; font-size:0.75rem; color:#38bdf8; background:#0f172a; padding:6px 10px; border-radius:6px; border:1px solid #334155; font-weight: 600;"></div>
        </div>
        <div class="grid-3">
          <div class="form-group"><label id="ui_lblDate">日期</label><input type="text" id="inputTarikh" value="12.02.2026" oninput="updateView()"></div>
          <div class="form-group"><label id="ui_lblTime">时间</label><input type="text" id="inputMasa" value="8:00 AM – 12:00 PM" oninput="updateView()"></div>
          <div class="form-group"><label id="ui_lblInvolvement">参与人员</label><input type="text" id="inputPenglibatan" oninput="updateView()"></div>
        </div>
      </div>
      <div class="card">
        <div class="card-title" id="ui_secObjImp">🎯 简介、目标与成效</div>
        <div class="form-group"><label id="ui_lblPengenalan">活动简介</label><textarea id="inputPengenalan" oninput="updateView()"></textarea></div>
        <div class="form-group"><label id="ui_lblObj">活动目标（至少3项）</label><textarea id="inputObjektif" style="height: 80px;" oninput="updateView()"></textarea></div>
        <div class="form-group"><label id="ui_lblImp">活动成效（至少3项）</label><textarea id="inputImpak" style="height: 80px;" oninput="updateView()"></textarea></div>
      </div>
      <div class="card">
        <div class="card-title" id="ui_secPhotos">🖼️ 活动照片与说明（4张）</div>
        <div class="grid-2">
          <div class="form-group"><label><span class="ui_lblPhotoWord">照片</span> 1</label><input type="file" id="photo1" accept="image/*" onchange="loadPhoto(event, 1)"><input type="text" id="caption1" class="caption-input" placeholder="照片 1 说明/标注" style="margin-top: 4px;" oninput="updateView()"></div>
          <div class="form-group"><label><span class="ui_lblPhotoWord">照片</span> 2</label><input type="file" id="photo2" accept="image/*" onchange="loadPhoto(event, 2)"><input type="text" id="caption2" class="caption-input" placeholder="照片 2 说明/标注" style="margin-top: 4px;" oninput="updateView()"></div>
          <div class="form-group"><label><span class="ui_lblPhotoWord">照片</span> 3</label><input type="file" id="photo3" accept="image/*" onchange="loadPhoto(event, 3)"><input type="text" id="caption3" class="caption-input" placeholder="照片 3 说明/标注" style="margin-top: 4px;" oninput="updateView()"></div>
          <div class="form-group"><label><span class="ui_lblPhotoWord">照片</span> 4</label><input type="file" id="photo4" accept="image/*" onchange="loadPhoto(event, 4)"><input type="text" id="caption4" class="caption-input" placeholder="照片 4 说明/标注" style="margin-top: 4px;" oninput="updateView()"></div>
        </div>
      </div>
      <div class="card">
        <div class="card-title" id="ui_secTeacher">✍️ 汇报教师信息</div>
        <div class="form-group"><label id="ui_lblTeacherName">教师姓名</label><input type="text" id="inputTeacherName" oninput="updateView()"></div>
        <div class="form-group"><label id="ui_lblPosition">教师职位</label><input type="text" id="inputPosition" oninput="updateView()"></div>
      </div>
    </aside>

    <!-- Right Canvas Preview -->
    <main class="preview-area" id="previewArea">
      <div class="a4-page">
        <div style="display: flex; flex-direction: column; flex: 1; min-height: 0;">
          <div class="header-layout">
            <div class="header-left"><div class="title-school" id="viewSchoolName">SJKC PEI HUA</div></div>
            <div class="logo-box" id="logoBox"><span style="font-size: 7pt; color: #888;">[ 校徽 LOGO ]</span></div>
          </div>
          <div class="banner-opr">ONE PAGE REPORT</div>
          <table class="info-table">
            <tr><td class="label-bg" style="width: 15%;" id="ui_tblProgram">PROGRAM :</td><td class="val-text" id="viewTitle" colspan="5">-</td></tr>
            <tr><td class="label-bg" style="width: 15%;" id="ui_tblDate">TARIKH :</td><td id="viewTarikh" style="width: 20%;">12.02.2026</td><td class="label-bg" style="width: 12%;" id="ui_tblTime">MASA :</td><td id="viewMasa" style="width: 23%;">8:00 AM – 12:00 PM</td><td class="label-bg" style="width: 18%;" id="ui_tblTarget">PENGLIBATAN:</td><td id="viewPenglibatan">-</td></tr>
          </table>
          <div class="box-container"><div class="box-header" id="ui_headerPengenalan">PENGENALAN</div><div class="box-content" id="viewPengenalan">-</div></div>
          <div class="box-container"><div class="box-header" id="ui_headerObjektif">OBJEKTIF</div><div class="box-content" id="viewObjektif">-</div></div>
          <div class="box-container"><div class="box-header" id="ui_headerImpak">IMPAK PROGRAM</div><div class="box-content" id="viewImpak">-</div></div>
          <div class="photos-grid">
            <div class="photo-cell" id="box1"><span class="photo-ph" id="ui_photoPh1">[ 照片 1 ]</span></div>
            <div class="photo-cell" id="box2"><span class="photo-ph" id="ui_photoPh2">[ 照片 2 ]</span></div>
            <div class="photo-cell" id="box3"><span class="photo-ph" id="ui_photoPh3">[ 照片 3 ]</span></div>
            <div class="photo-cell" id="box4"><span class="photo-ph" id="ui_photoPh4">[ 照片 4 ]</span></div>
          </div>
        </div>
        <div class="pelapor-box">
          <div class="pelapor-title" id="ui_headerPelapor">PELAPOR PROGRAM</div>
          <div class="pelapor-name" id="viewTeacherName">-</div>
          <div class="pelapor-pos" id="viewPosition">-</div>
        </div>
      </div>
    </main>
  </div>
  <script>
  let currentLang = 'zh';

  // 多语言 UI 界面词典
  const i18n = {
    zh: {
      appTitle: "OPR 报告生成器",
      btnSave: "💾 保存草稿",
      btnLoad: "📂 加载草稿",
      btnReset: "🔄 重置内容",
      btnPrint: "🖨️ 打印 / 导出 PDF",
      secFont: "🎨 字体风格设置",
      lblFont: "选择字体风格",
      secPreset: "📌 学校活动预设",
      lblPreset: "选择活动预设",
      secSchool: "🏫 学校与校徽设置",
      lblLogo: "上传学校 Logo / 校徽",
      lblSchoolName: "学校名称",
      secDetails: "📋 活动信息",
      lblTitle: "活动名称",
      lblDate: "日期",
      lblTime: "时间",
      lblInvolvement: "参与人员",
      secObjImp: "🎯 简介、目标与成效",
      lblPengenalan: "活动简介",
      lblObj: "活动目标（至少3项）",
      lblImp: "活动成效（至少3项）",
      secPhotos: "🖼️ 活动照片与说明（4张）",
      lblPhoto: "照片",
      captionPlaceholder: "说明/标注",
      photoPlaceholder: "照片",
      secTeacher: "✍️ 汇报教师信息",
      lblTeacherName: "教师姓名",
      lblPosition: "教师职位",
      headerPengenalan: "活动简介",
      headerObjektif: "活动目标",
      headerImpak: "活动成效",
      headerPelapor: "汇报教师",
      tblProgram: "活动名称 :",
      tblDate: "日期 :",
      tblTime: "时间 :",
      tblTarget: "参与人员 :",
      msgSaveSuccess: "✅ 草稿及图片已保存！",
      msgSaveError: "⚠️ 保存失败，图片可能过大超越了 LocalStorage 限制。建议使用较小图档。",
      msgLoadSuccess: "📂 草稿已成功加载！",
      msgLoadEmpty: "⚠️ 未找到已保存的草稿。",
      msgConfirmReset: "确定要重置所有内容与图片吗？"
    },
    ms: {
      appTitle: "OPR Generator WebApp",
      btnSave: "💾 Simpan Draf",
      btnLoad: "📂 Muat Draf",
      btnReset: "🔄 Semula",
      btnPrint: "🖨️ Cetak / PDF",
      secFont: "🎨 Tetapan Font & Gaya",
      lblFont: "Pilih Jenis Font",
      secPreset: "📌 Preset Program Sekolah",
      lblPreset: "Pilih Preset Aktiviti",
      secSchool: "🏫 Maklumat Sekolah & Logo",
      lblLogo: "Muat Naik Logo Sekolah",
      lblSchoolName: "Nama Sekolah",
      secDetails: "📋 Maklumat Program",
      lblTitle: "Nama Program / Aktiviti",
      lblDate: "Tarikh",
      lblTime: "Masa",
      lblInvolvement: "Penglibatan",
      secObjImp: "🎯 Pengenalan, Objektif & Impak",
      lblPengenalan: "Pengenalan Program",
      lblObj: "Objektif Program (Sekurang-kurangnya 3)",
      lblImp: "Impak Program (Sekurang-kurangnya 3)",
      secPhotos: "🖼️ Gambar & Keterangan (4 Keping)",
      lblPhoto: "Gambar",
      captionPlaceholder: "Keterangan / Label",
      photoPlaceholder: "Gambar",
      secTeacher: "✍️ Maklumat Pelapor Program",
      lblTeacherName: "Nama Guru Pelapor",
      lblPosition: "Jawatan Guru",
      headerPengenalan: "PENGENALAN",
      headerObjektif: "OBJEKTIF",
      headerImpak: "IMPAK PROGRAM",
      headerPelapor: "PELAPOR PROGRAM",
      tblProgram: "PROGRAM :",
      tblDate: "TARIKH :",
      tblTime: "MASA :",
      tblTarget: "PENGLIBATAN:",
      msgSaveSuccess: "✅ Draf berjaya disimpan!",
      msgSaveError: "⚠️ Gagal menyimpan, saiz imej mungkin terlalu besar.",
      msgLoadSuccess: "📂 Draf berjaya dimuatkan!",
      msgLoadEmpty: "⚠️ Tiada draf disimpan dijumpai.",
      msgConfirmReset: "Adakah anda pasti untuk semula semua kandungan?"
    },
    en: {
      appTitle: "OPR Generator WebApp",
      btnSave: "💾 Save Draft",
      btnLoad: "📂 Load Draft",
      btnReset: "🔄 Reset",
      btnPrint: "🖨️ Print / Export PDF",
      secFont: "🎨 Font & Style Settings",
      lblFont: "Select Font Family",
      secPreset: "📌 School Activity Presets",
      lblPreset: "Select Activity Preset",
      secSchool: "🏫 School Info & Logo",
      lblLogo: "Upload School Logo",
      lblSchoolName: "School Name",
      secDetails: "📋 Activity Details",
      lblTitle: "Program / Activity Title",
      lblDate: "Date",
      lblTime: "Time",
      lblInvolvement: "Target Audience",
      secObjImp: "🎯 Introduction, Objectives & Impact",
      lblPengenalan: "Program Introduction",
      lblObj: "Program Objectives (Min 3 points)",
      lblImp: "Program Impact (Min 3 points)",
      secPhotos: "🖼️ Activity Photos & Captions (4 Photos)",
      lblPhoto: "Photo",
      captionPlaceholder: "Caption / Label",
      photoPlaceholder: "Photo",
      secTeacher: "✍️ Reporting Teacher Details",
      lblTeacherName: "Reporting Teacher Name",
      lblPosition: "Teacher Designation",
      headerPengenalan: "INTRODUCTION",
      headerObjektif: "OBJECTIVES",
      headerImpak: "PROGRAM IMPACT",
      headerPelapor: "PROGRAM REPORTED BY",
      tblProgram: "PROGRAM :",
      tblDate: "DATE :",
      tblTime: "TIME :",
      tblTarget: "TARGET :",
      msgSaveSuccess: "✅ Draft saved successfully!",
      msgSaveError: "⚠️ Save failed. The images might be too large for storage.",
      msgLoadSuccess: "📂 Draft loaded successfully!",
      msgLoadEmpty: "⚠️ No saved draft found.",
      msgConfirmReset: "Are you sure you want to reset all contents and photos?"
    }
  };

  const fontOptions = [
    { name: "Segoe UI / Tahoma (默认)", value: "'Segoe UI', Tahoma, Geneva, Verdana, sans-serif" },
    { name: "Arial (经典无衬线)", value: "Arial, Helvetica, sans-serif" },
    { name: "Times New Roman (经典有衬线)", value: "'Times New Roman', Times, serif" },
    { name: "Calibri / Aptos", value: "Calibri, 'Aptos', sans-serif" },
    { name: "Georgia (优雅传统)", value: "Georgia, serif" },
    { name: "Verdana (清晰易读)", value: "Verdana, Geneva, sans-serif" },
    { name: "Microsoft YaHei (微软雅黑)", value: "'Microsoft YaHei', '黑体', sans-serif" },
    { name: "KaiTi (楷体)", value: "'KaiTi', '楷体', serif" }
  ];

  const presets = {
    hui_chun: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校级新春挥春比赛", ms: "PERTANDINGAN KALIGRAFI HUI CHUN PERINGKAT SEKOLAH", en: "SCHOOL HUI CHUN CALLIGRAPHY COMPETITION" },
      tarikh: "11.02.2026", masa: "8:00 AM – 10:30 AM",
      penglibatan: { zh: "全体师生", ms: "SEMUA WARGA SEKOLAH", en: "ALL SCHOOL MEMBERS" },
      pengenalan: {
        zh: "本校举办农历新年挥春比赛，旨在发扬中华传统书法艺术，营造浓厚的节日氛围，并培养学生对汉字书写美感的欣赏能力。",
        ms: "Pertandingan Kaligrafi Hui Chun dianjurkan sempena Sambutan Tahun Baru Cina bagi memelihara warisan seni kaligrafi dan menyemarakkan suasana perayaan.",
        en: "The Hui Chun competition is organized in celebration of Chinese New Year to preserve traditional calligraphy art and foster appreciation for Chinese characters."
      },
      objektif: {
        zh: "• 传承与弘扬中华优秀传统书法文化。\n• 提高学生的毛笔书写水平与艺术审美素养。\n• 增进校园内浓厚的春意与节日喜庆气氛。",
        ms: "• Memelihara dan memartabatkan warisan seni kaligrafi Cina.\n• Meningkatkan kemahiran penulisan kaligrafi dan estetika seni murid.\n• Menyemarakkan suasana kebudayaan dan perayaan di sekolah.",
        en: "• To preserve and promote Chinese traditional calligraphy art.\n• To improve students' brushwriting technique and aesthetic appreciation.\n• To enhance the festive New Year atmosphere across the campus."
      },
      impak: {
        zh: "• 学生们表现专注，挥毫写下充满祝福的春联。\n• 校园内展示了丰富的挥春作品，充满了喜庆气息。\n• 有效激发了学生对学习传统艺术与汉字的兴趣。",
        ms: "• Murid tekun menghasilkan karya kaligrafi bermakna dan indah.\n• Hasil karya murid dipamerkan sekali gus menceriakan suasana sekolah.\n• Memupuk minat murid terhadap seni penulisan dan kebudayaan.",
        en: "• Students engaged with focus, creating expressive spring couplets.\n• Student artworks were displayed, decorating the campus dynamically.\n• Successfully sparked student enthusiasm for traditional Chinese calligraphic arts."
      },
      teacher: { zh: "曾佳薇老师", ms: "PN. CHEN CHIA WEI", en: "MS. CHEN CHIA WEI" },
      position: { zh: "书法与中华文化负责老师", ms: "PENYELARAS KELAB KALIGRAFI", en: "CALLIGRAPHY CLUB COORDINATOR" }
    },
    ying_bi_zi: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校级硬笔字书写比赛", ms: "PERTANDINGAN TULISAN KEMAS PERINGKAT SEKOLAH", en: "SCHOOL HARD-PEN HANDWRITING COMPETITION" },
      tarikh: "18.04.2026", masa: "8:00 AM – 9:30 AM",
      penglibatan: { zh: "一至六年级学生", ms: "SEMUA MURID TAHUN 1 - 6", en: "ALL STANDARD 1 - 6 STUDENTS" },
      pengenalan: {
        zh: "硬笔字比赛旨在规范学生的书写姿势，培养良好的书写习惯，提高汉字硬笔书写规范与结构美感。",
        ms: "Pertandingan tulisan kemas dianjurkan bagi memupuk sikap penulisan tulisan tangan yang berdisiplin, kemas, dan menguasai struktur watak tulisan dengan betul.",
        en: "The hard-pen handwriting competition aims to regulate posture, foster neat writing habits, and improve structural precision in handwriting."
      },
      objektif: {
        zh: "• 培养学生良好的坐姿与执笔书写习惯。\n• 提高学生笔画笔顺规范及汉字结构间架美感。\n• 激发学生对写好规范字与硬笔书法的兴趣。",
        ms: "• Memupuk tabiat postur duduk dan pegangan pena yang betul.\n• Meningkatkan kemahiran struktur watak tulisan tangan dan susunan kemas.\n• Menyemarakkan minat murid untuk menghasilkan tulisan tangan yang indah.",
        en: "• To instill proper sitting posture and pen-holding techniques.\n• To enhance stroke order accuracy and character structural balance.\n• To inspire interest in beautiful and neat hard-pen handwriting."
      },
      impak: {
        zh: "• 学生作业与卷面书写整洁度得到显著提升。\n• 展出了多幅运笔流畅、端庄美观的硬笔字优秀作品。\n• 增强了学生书写规范汉字的自信心。",
        ms: "• Kejelasan dan kekemasan tulisan tangan murid meningkat dengan ketara.\n• Hasil karya tulisan tangan yang kemas dipamerkan untuk teladan murid lain.\n• Meningkatkan keyakinan murid dalam penulisan tulisan tangan.",
        en: "• Noticeable improvement in student handwriting neatness across classes.\n• Excellent handwriting pieces were exhibited to showcase proper penmanship.\n• Boosted students' confidence in standard character penmanship."
      },
      teacher: { zh: "曾佳薇老师", ms: "PN. CHEN CHIA WEI", en: "MS. CHEN CHIA WEI" },
      position: { zh: "华文科负责老师", ms: "PENYELARAS PANITIA BAHASA CINA", en: "CHINESE SUBJECT COORDINATOR" }
    },
    pertandingan_matematik: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校级数学笔试与奥数比赛", ms: "PERTANDINGAN BERTULIS MATEMATIK PERINGKAT SEKOLAH", en: "SCHOOL MATHEMATICS WRITTEN COMPETITION" },
      tarikh: "12.04.2026", masa: "8:00 AM – 9:30 AM",
      penglibatan: { zh: "一至六年级学生", ms: "SEMUA MURID TAHUN 1 - 6", en: "ALL STANDARD 1 - 6 STUDENTS" },
      pengenalan: {
        zh: "数学笔试比赛旨在激发学生对数学逻辑思维与计算技巧的兴趣，提高解决高阶思维（KBAT）难题的能力。",
        ms: "Pertandingan bertulis matematik dijalankan untuk menguji minda logik, kecekapan berhitung, dan pemikiran aras tinggi (KBAT) murid.",
        en: "The written mathematics competition aims to test logical thinking, calculation accuracy, and higher-order thinking skills (HOTS)."
      },
      objektif: {
        zh: "• 巩固学生的数学运算基础与逻辑推理能力。\n• 激发探索数学解题技巧的兴趣与自信心。\n• 遴选高分选手代表学校参加校际数学奥林匹克赛。",
        ms: "• Memantapkan asas pengiraan dan penaakulan logik murid.\n• Menyemai minat dan keyakinan dalam menyelesaikan soalan KBAT.\n• Memilih wakil sekolah ke pertandingan Olimpad Matematik.",
        en: "• To strengthen fundamental calculation and logical reasoning skills.\n• To ignite interest in solving HOTS mathematical problems.\n• To select qualified delegates for inter-school Math Olympiads."
      },
      impak: {
        zh: "• 选手的快速解题与逻辑思考能力得到了提升。\n• 营造了勤于思考、勇于挑战难题的学术风气。\n• 成功建立校级数学竞赛人才库。",
        ms: "• Kecekapan berhitung dan pemikiran kritikal murid meningkat.\n• Membudayakan amalan suka belajar matematik di sekolah.\n• Berjaya membina senarai bakat matematik sekolah.",
        en: "• Sharpend students' speed solving and critical logic.\n• Fostered a positive learning culture for mathematics.\n• Established a strong talent pool for math competitions."
      },
      teacher: { zh: "廖秀兰老师", ms: "PN. LIAW SIEW LAN", en: "MS. LIAW SIEW LAN" },
      position: { zh: "数学科小组主任", ms: "KETUA PANITIA MATEMATIK", en: "HEAD OF MATHEMATICS SUBJECT" }
    },
    pertandingan_sains: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校级科学知识与探究笔试比赛", ms: "PERTANDINGAN BERTULIS SAINS PERINGKAT SEKOLAH", en: "SCHOOL SCIENCE WRITTEN COMPETITION" },
      tarikh: "14.04.2026", masa: "8:00 AM – 9:30 AM",
      penglibatan: { zh: "三至六年级学生", ms: "MURID TAHUN 3 - 6", en: "STANDARD 3 - 6 STUDENTS" },
      pengenalan: {
        zh: "科学笔试比赛旨在考查学生对自然科学现象、实验设计、科学技能（KPS）与逻辑解释的掌握程度。",
        ms: "Pertandingan bertulis sains dianjurkan bagi menguji kefahaman konsep sains, kemahiran proses sains (KPS), dan sifat ingin tahu murid.",
        en: "The written science competition tests core scientific concepts, science process skills (SPS), and inquiry mindsets."
      },
      objektif: {
        zh: "• 检验学生对科学原理与实验变数的理解应用。\n• 培养探索自然奥秘与严谨推论的科学精神。\n• 选拔科学拔尖学生代表学校参加县级科学笔试。",
        ms: "• Menguji penguasaan kemahiran proses sains dan pembolehubah.\n• Memupuk sikap saintifik dan pemikiran berstruktur.\n• Memilih wakil sekolah ke Pertandingan Ujian Bertulis Sains MSSD.",
        en: "• To evaluate mastery in science process skills and variables.\n• To cultivate curiosity and systematic scientific thinking.\n• To select top candidates for the district science written test."
      },
      impak: {
        zh: "• 学生提高了将科学知识运用于日常生活常识的能力。\n• 激发了学生阅读科学科普书籍的热情。\n• 选拔出了备战校际科学竞赛的优秀代表。",
        ms: "• Penguasaan murid dalam soalan aplikasi sains harian meningkat.\n• Memupuk minat membaca buku-buku sains dan teknologi.\n• Wakil sekolah bersedia menghadapi cabaran peringkat daerah.",
        en: "• Improved students' ability to apply scientific knowledge practically.\n• Sparked active reading habits for scientific literature.\n• Prepared high-achieving representatives for district contests."
      },
      teacher: { zh: "张美芬老师", ms: "PN. CHEONG MEI FUN", en: "MS. CHEONG MEI FUN" },
      position: { zh: "科学科小组主任", ms: "KETUA PANITIA SAINS", en: "HEAD OF SCIENCE SUBJECT" }
    },
    pertandingan_stem: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校级 STEM 科技创新制作竞赛", ms: "CABARAN & PERTANDINGAN INOVASI STEM PERINGKAT SEKOLAH", en: "SCHOOL STEM INNOVATION & DESIGN CHALLENGE" },
      tarikh: "22.05.2026", masa: "8:00 AM – 12:30 PM",
      penglibatan: { zh: "四、五、六年级学生", ms: "MURID TAHUN 4, 5 & 6", en: "STANDARD 4, 5 & 6 STUDENTS" },
      pengenalan: {
        zh: "STEM竞赛整合科学、技术、工程与数学原理，通过动手组装桥梁模型、水火箭及机械装置，激发跨学科创新。",
        ms: "Pertandingan STEM mengintegrasikan sains, teknologi, kejuruteraan, dan matematik menerusi reka cipta model dan pemikiran reka bentuk.",
        en: "The STEM challenge integrates Science, Technology, Engineering, and Mathematics through hands-on model creation and engineering design."
      },
      objektif: {
        zh: "• 培养学生的工程设计思维、动手解决实际问题的能力。\n• 锻炼小组协同合作、模型构建与口头路演表达。\n• 遴选创新作品代表学校参加全州STEM嘉年华。",
        ms: "• Memupuk pemikiran reka bentuk dan kemahiran menyelesaikan masalah.\n• Membina semangat kerja berpasukan dan kemahiran pembentangan.\n• Memilih reka cipta terbaik ke Karnival STEM peringkat negeri.",
        en: "• To build engineering design thinking and hands-on problem-solving skills.\n• To foster group collaboration and prototype pitching skills.\n• To select top innovative projects for the state STEM Carnival."
      },
      impak: {
        zh: "• 展出了数十件结构独特且具有实用价值的STEM创新模型。\n• 显著提升了学生的团队合作与工程实践能力。\n• 营造了充满科创活力与探索精神的校园科技氛围。",
        ms: "• Pelbagai reka cipta reka bentuk inovatif berjaya dihasilkan.\n• Kemahiran kerja berpasukan dan pemikiran kreatif murid bertambah mantap.\n• Menyemarakkan budaya inovasi dan teknologi di sekolah.",
        en: "• Produced numerous ingenious STEM models and prototypes.\n• Enhanced teamwork dynamics and creative engineering confidence.\n• Cultivated a vibrant technological and innovative school culture."
      },
      teacher: { zh: "黄美玲老师", ms: "PN. WONG MEI LING", en: "MS. WONG MEI LING" },
      position: { zh: "STEM / RBT 协调老师", ms: "PENYELARAS STEM & RBT", en: "STEM & RBT COORDINATOR" }
    },
    mewarna_merdeka: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "国庆日爱国主题填色与海报比赛", ms: "PERTANDINGAN MEWARNA & MELUKIS POSTER HARI KEBANGSAAN", en: "NATIONAL DAY COLOURING & POSTER CONTEST" },
      tarikh: "28.08.2026", masa: "8:30 AM – 11:00 AM",
      penglibatan: { zh: "一至六年级全体学生", ms: "SEMUA MURID TAHUN 1 - 6", en: "ALL STANDARD 1 - 6 STUDENTS" },
      pengenalan: {
        zh: "结合国庆日庆典举行的填色与海报比赛，旨在通过艺术色彩表达对祖国的热爱，培养爱国主义情怀。",
        ms: "Pertandingan mewarna dan melukis poster Hari Kebangsaan dianjurkan bagi menyemarakkan semangat patriotisme menerusi ekspresi seni visual.",
        en: "Combines National Day celebrations with artistic expression to ignite national pride, patriotism, and visual creativity."
      },
      objektif: {
        zh: "• 激发学生对国家的归属感、自豪感与爱国情操。\n• 鼓励运用色彩技巧呈现独立日辉煌条纹与爱国主题。\n• 丰富爱国月（Bulan Kemerdekaan）校园系列活动。",
        ms: "• Memupuk semangat patriotisme dan cinta akan negara dalam kalangan murid.\n• Mengembangkan bakat kreativiti seni mewarna dan melukis poster.\n• Menyemarakkan sambutan Bulan Kemerdekaan peringkat sekolah.",
        en: "• To instill national pride, unity, and love for the country.\n• To enhance artistic color theory and poster illustration skills.\n• To enrich National Month patriotic celebrations across school."
      },
      impak: {
        zh: "• 校园充满了红黄蓝白辉煌条纹的爱国艺术色彩。\n• 深刻理解了国庆日独立历史背后的光荣含义。\n• 优秀绘画作品张贴于校园画廊展示爱国心声。",
        ms: "• Suasana sekolah dimeriahkan dengan karya seni bertemakan Jalur Gemilang.\n• Murid menghayati makna kemerdekaan menerusi ciptaan poster.\n• Hasil karya cemerlang dipamerkan di sudut patriotisme sekolah.",
        en: "• School walls vibrantly decorated with Jalur Gemilang artworks.\n• Deepened students' appreciation for independence history.\n• Top posters showcased at the school patriotic art gallery."
      },
      teacher: { zh: "李健伟老师", ms: "EN. LEE KIN WAI", en: "MR. LEE KIN WAI" },
      position: { zh: "爱国月活动委员会主席", ms: "PENYELARAS BULAN KEBANGSAAN", en: "NATIONAL MONTH COORDINATOR" }
    },
    kuiz_merdeka: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "爱国月建国历史知识问答比赛", ms: "PERTANDINGAN KUIZ SEJARAH & BULAN KEBANGSAAN", en: "PATRIOTIC MONTH & HISTORY KNOWLEDGE QUIZ" },
      tarikh: "15.09.2026", masa: "9:00 AM – 10:30 AM",
      penglibatan: { zh: "四、五、六年级学生", ms: "MURID TAHUN 4, 5 & 6", en: "STANDARD 4, 5 & 6 STUDENTS" },
      pengenalan: {
        zh: "爱国月历史问答比赛旨在考查学生对国家独立历史、国家原则（Rukun Negara）及国家领袖事迹的了解。",
        ms: "Pertandingan Kuiz Sejarah Kemerdekaan dianjurkan untuk menguji pengetahuan am murid mengenai sejarah pembentukan Malaysia.",
        en: "Tests students' general knowledge regarding Malaysian independence history, Rukun Negara, and national leaders."
      },
      objektif: {
        zh: "• 加深对马六甲王朝至国家独立建设史实的认识。\n• 强化恪守国家原则五大精神的爱国观念。\n• 培养敏捷的抢答思维与团队问答应变能力。",
        ms: "• Meningkatkan pengetahuan am murid mengenai sejarah pembentukan negara.\n• Memantapkan pemahaman dan penghayatan nilai Rukun Negara.\n• Memupuk semangat daya saing sihat dan pemikiran pantas.",
        en: "• To deepen knowledge of Malaysian history and national nation-building.\n• To reinforce core values of Rukun Negara among young citizens.\n• To foster healthy academic competition and rapid recall."
      },
      impak: {
        zh: "• 抢答环节气氛高涨，显著提高了历史科学习兴趣。\n• 强化了学生铭记历史、珍惜国家和平的爱国心。\n• 选出了爱国知识渊博的优胜小标兵。",
        ms: "• Suasana kuiz berlangsung rancak dan penuh dengan semangat kebangsaan.\n• Pengetahuan am dan pemahaman sejarah negara bertambah mantap.\n• Pemenang kuiz dipilih sebagai Duta Sejarah Sekolah.",
        en: "• Highly energetic competition boosted student enthusiasm for Malaysian history.\n• Strengthened student patriotic spirit and historical awareness.\n• Awarded top-performing history ambassadors."
      },
      teacher: { zh: "李健伟老师", ms: "EN. LEE KIN WAI", en: "MR. LEE KIN WAI" },
      position: { zh: "历史科小组主任", ms: "KETUA PANITIA SEJARAH", en: "HEAD OF HISTORY SUBJECT" }
    },
    mewarna_antidadah: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "反毒教育填色及画海报比赛", ms: "PERTANDINGAN MEWARNA & MELUKIS POSTER PENDIDIKAN PENCEGAHAN DADAH (PPDa)", en: "ANTI-DRUG AWARENESS COLOURING & POSTER COMPETITION" },
      tarikh: "08.07.2026", masa: "8:30 AM – 10:30 AM",
      penglibatan: { zh: "一至六年级学生", ms: "SEMUA MURID TAHUN 1 - 6", en: "ALL STANDARD 1 - 6 STUDENTS" },
      pengenalan: {
        zh: "配合反毒宣传周举办比赛，旨在通过艺术创作向学生宣导毒品的危害，建立健康无毒的校园生活态度。",
        ms: "Dianjurkan sempena Minggu PPDa bagi memberi kesedaran mengenai bahaya dadah menerusi karya seni melukis poster.",
        en: "Held in conjunction with Anti-Drug Week to raise awareness about drug dangers through creative poster painting."
      },
      objektif: {
        zh: "• 传达“远离毒品、关爱生命”的正确价值观。\n• 锻炼学生的色彩搭配、主题表现与绘画技能。\n• 建立坚固的防毒反毒意识屏障。",
        ms: "• Memupuk kesedaran menjauhi bahaya dadah dalam kalangan murid.\n• Mengembangkan kreativiti seni visual bertemakan PPDa.\n• Membina budaya sekolah sihat tanpa dadah.",
        en: "• To advocate healthy, drug-free lifestyles among youth.\n• To enhance artistic composition and color theory skills.\n• To reinforce strong anti-drug awareness school-wide."
      },
      impak: {
        zh: "• 展出了多幅主题突出、色彩鲜艳的反毒海报作品。\n• 提高了学生对毒品诱惑的辨识与防范能力。\n• 营造了健康的反毒宣传氛围。",
        ms: "• Karya seni bertemakan PPDa dipamerkan dengan kreatif.\n• Pemahaman murid terhadap kesan buruk dadah bertambah baik.\n• Mesej PPDa berjaya disampaikan secara berkesan.",
        en: "• Produced vivid poster artworks highlighting drug prevention.\n• Increased students' knowledge on staying away from harmful substances.\n• Successfully delivered strong educational health messages."
      },
      teacher: { zh: "廖秀兰老师", ms: "PN. LIAW SIEW LAN", en: "MS. LIAW SIEW LAN" },
      position: { zh: "美化与卫生委员会主任", ms: "PENYELARAS KEBERSIHAN & KECERIAAN", en: "CLEANLINESS & BEAUTIFICATION COORDINATOR" }
    },
    storytelling_bc: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校级华文讲故事比赛", ms: "PERTANDINGAN BERCERITA BAHASA CINA PERINGKAT SEKOLAH", en: "SCHOOL CHINESE STORYTELLING COMPETITION" },
      tarikh: "10.03.2026", masa: "8:00 AM – 11:30 AM",
      penglibatan: { zh: "一、二、三年级学生", ms: "MURID TAHUN 1, 2 & 3", en: "STANDARD 1, 2 & 3 STUDENTS" },
      pengenalan: {
        zh: "华文讲故事比赛专为低年级学生设计，旨在提高母语表达能力、语音语调精准度，并增强台风与自信心。",
        ms: "Pertandingan bercerita Bahasa Cina dianjurkan bagi membina keyakinan diri, kelancaran sebutan, serta kebolehan bercerita murid perintis.",
        en: "Designed for lower primary students to build mother-tongue articulation, voice intonation, and confident public stage performance."
      },
      objektif: {
        zh: "• 鼓励学生多用标准华语表达，丰富词汇量。\n• 培养丰富的生动肢体语言、面部表情与语调感染力。\n• 选拔优秀故事小选手代表学校参赛。",
        ms: "• Menggalakkan penggunaan Bahasa Cina yang sebutan betul dan jelas.\n• Memupuk gaya penyampaian, intonasi, dan nada cerita yang menarik.\n• Memilih wakil sekolah ke pertandingan bercerita peringkat daerah.",
        en: "• To encourage fluent, standard Chinese pronunciation and vocabulary.\n• To develop expressive body language, facial gestures, and voice tone.\n• To select outstanding storytellers for inter-school competitions."
      },
      impak: {
        zh: "• 小选手们表现生动活泼，展现了极强的语言天赋。\n• 极大地提升了低年级学生在公众面前发言的胆量。\n• 选拔出了代表学校参加区级讲故事比赛的人选。",
        ms: "• Peserta cilik menyampaikan cerita dengan penuh keyakinan dan ekspresif.\n• Meningkatkan keberanian murid bercakap di atas pentas.\n• Berjaya memilih wakil cemerlang ke pertandingan luar.",
        en: "• Young contestants delivered animated and expressive storytelling.\n• Significantly boosted stage courage for lower primary pupils.\n• Successfully picked qualified champions for district competitions."
      },
      teacher: { zh: "曾佳薇老师", ms: "PN. CHEN CHIA WEI", en: "MS. CHEN CHIA WEI" },
      position: { zh: "华文科小组主任", ms: "KETUA PANITIA BAHASA CINA", en: "HEAD OF CHINESE SUBJECT" }
    },
    storytelling_bm: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校级马来文讲故事比赛", ms: "PERTANDINGAN BERCERITA BAHASA MELAYU PERINGKAT SEKOLAH", en: "SCHOOL MALAY STORYTELLING COMPETITION" },
      tarikh: "12.03.2026", masa: "8:00 AM – 11:30 AM",
      penglibatan: { zh: "低年级与高年级学生", ms: "MURID TAHUN 1 - 6", en: "STANDARD 1 - 6 STUDENTS" },
      pengenalan: {
        zh: "马来文讲故事比赛旨在提升华小生的国语掌握能力、标准语音发音（Sebutan & Intonasi）与现场生动表达。",
        ms: "Pertandingan Bercerita Bahasa Melayu bertujuan untuk meningkatkan penguasaan Bahasa Kebangsaan, kelancaran sebutan, serta gaya persembahan murid SJKC.",
        en: "Aims to elevate Malay national language proficiency, correct pronunciation, and lively stage delivery for SJKC pupils."
      },
      objektif: {
        zh: "• 提高学生掌握国语的熟练度与词汇应用表达。\n• 培养优雅自信的语言台风与故事剧情感染力。\n• 选拔代表参加县级华小国语讲故事比赛（MSSD）。",
        ms: "• Meningkatkan tahap penguasaan Bahasa Melayu dalam kalangan murid SJKC.\n• Memupuk kemahiran persembahan bercerita yang menarik dan berkeyakinan.\n• Memilih wakil sekolah ke Pertandingan Bercerita Bahasa Melayu SJKC.",
        en: "• To enhance Malay language fluency, grammar, and active usage.\n• To build attractive storytelling tone and confident stage presence.\n• To select delegates for the district Malay Storytelling Competition."
      },
      impak: {
        zh: "• 非马来裔学生展现了标准流畅的马来文讲述能力。\n• 有效增进了全校学习国语的浓厚兴趣与氛围。\n• 选出了代表学校出征区级赛事的精英选手。",
        ms: "• Murid bukan Melayu menunjukkan penguasaan sebutan yang amat baik.\n• Menyemarakkan minat mempelajari Bahasa Kebangsaan di sekolah.\n• Senarai peserta terbaik ke peringkat daerah berjaya dipilih.",
        en: "• Non-Malay students demonstrated fluent standard Malay pronunciation.\n• Spurred enthusiastic national language learning across campus.\n• Selected elite candidates for the inter-school Bahasa Melayu contest."
      },
      teacher: { zh: "阿兹曼老师", ms: "EN. AZMAN BIN AHMAD", en: "MR. AZMAN BIN AHMAD" },
      position: { zh: "马来文科小组主任", ms: "KETUA PANITIA BAHASA MELAYU", en: "HEAD OF MALAY SUBJECT" }
    },
    storytelling_bi: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校级英文讲故事比赛", ms: "PERTANDINGAN BERCERITA BAHASA INGGERIS (ENGLISH STORYTELLING)", en: "SCHOOL ENGLISH STORYTELLING COMPETITION" },
      tarikh: "14.03.2026", masa: "8:00 AM – 11:30 AM",
      penglibatan: { zh: "一至六年级学生", ms: "SEMUA MURID TAHUN 1 - 6", en: "ALL STANDARD 1 - 6 STUDENTS" },
      pengenalan: {
        zh: "英文讲故事比赛旨在营造良好的英语学习环境，提高学生的英文口语发音、语调起伏与舞台故事演绎能力。",
        ms: "Pertandingan English Storytelling dianjurkan bagi membina keberanian bertutur dalam Bahasa Inggeris, sebutan tepat, dan gaya persembahan berkesan.",
        en: "Promotes an active English learning environment while boosting spoken fluency, correct pronunciation, and captivating theatrical flair."
      },
      objektif: {
        zh: "• 提升学生的英文听说能力与词汇应用技巧。\n• 鼓励克服使用英语表达的自卑与胆怯心理。\n• 遴选代表出战县级（MSSD）英文讲故事大赛。",
        ms: "• Meningkatkan penguasaan Bahasa Inggeris dari segi sebutan dan intonasi.\n• Membina keyakinan bertutur dalam bahasa kedua secara bebas.\n• Memilih wakil sekolah ke Pertandingan English Storytelling MSSD.",
        en: "• To improve English oral communication and vocabulary range.\n• To build confidence in speaking English openly without inhibition.\n• To select qualified delegates for the MSSD English Storytelling Contest."
      },
      impak: {
        zh: "• 参赛选手口齿伶俐，展现了地道的英文发音与语调。\n• 激发了全校学生乐于说英语、读英文绘本的兴趣。\n• 选出了极具潜力的英文讲故事学校代表。",
        ms: "• Peserta memperlihatkan sebutan Bahasa Inggeris yang fasih dan jelas.\n• Menyemarakkan budaya pembacaan buku cerita Bahasa Inggeris.\n• Wakil sekolah yang berbakat tinggi berjaya dikenal pasti.",
        en: "• Contestants delivered eloquent English with convincing expressions.\n• Inspired active English storybook reading habits school-wide.\n• Identified high-potential representatives for district matches."
      },
      teacher: { zh: "陈淑芬老师", ms: "PN. TAN SOOK FUN", en: "MS. TAN SOOK FUN" },
      position: { zh: "英文科小组主任", ms: "KETUA PANITIA BAHASA INGGERIS", en: "HEAD OF ENGLISH SUBJECT" }
    },
    speech_bc: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校级华文演讲比赛", ms: "PERTANDINGAN SYARAHAN BAHASA CINA PERINGKAT SEKOLAH", en: "SCHOOL CHINESE PUBLIC SPEAKING COMPETITION" },
      tarikh: "18.03.2026", masa: "8:00 AM – 11:30 AM",
      penglibatan: { zh: "四、五、六年级学生", ms: "MURID TAHUN 4, 5 & 6", en: "STANDARD 4, 5 & 6 STUDENTS" },
      pengenalan: {
        zh: "华文演讲比赛旨在训练学生的思辨能力、即席演说逻辑、声调爆发力与优雅稳重的说服力。",
        ms: "Pertandingan Syarahan Bahasa Cina dianjurkan untuk membina pemikiran kritis, artikulasi idea, serta gaya persembahan syarahan berhemah.",
        en: "Trains critical thinking, impromptu speech structure, vocal projection, and persuasive public speaking mannerisms."
      },
      objektif: {
        zh: "• 锻炼逻辑思考与稿件说服力，提高语言功底。\n• 培养大方得体、沉着冷静的演讲气场与表达技巧。\n• 选拔代表参加全州/全国华小华语演讲比赛。",
        ms: "• Memupuk pemikiran analitis dan susunan isi syarahan yang bernas.\n• Membina sahsiah dan kepimpinan bercakap di hadapan khalayak.\n• Memilih wakil ke Pertandingan Syarahan Bahasa Cina peringkat daerah.",
        en: "• To hone analytical reasoning and structured speech persuasion.\n• To build leadership posture and poise before large audiences.\n• To select delegates for inter-school Chinese Public Speaking contests."
      },
      impak: {
        zh: "• 选手演讲主题鲜明、吐字清晰、气势如虹。\n• 提升了高年级学生的思辨水准与文章赏析能力。\n• 遴选出了代表学校出征外赛的演讲尖子。",
        ms: "• Peserta memberikan persembahan syarahan yang amat meyakinkan.\n• Tahap pemikiran kritis dan kemahiran berbahasa murid meningkat.\n• Calon johan syarahan sekolah sedia bersaing di peringkat daerah.",
        en: "• Contestants delivered powerful speeches with compelling reasoning.\n• Elevated critical discourse and essay appreciation levels.\n• Selected top-tier public speakers for regional championships."
      },
      teacher: { zh: "曾佳薇老师", ms: "PN. CHEN CHIA WEI", en: "MS. CHEN CHIA WEI" },
      position: { zh: "演讲学会指导老师", ms: "PENYELARAS KELAB PIDATO & SYARAHAN", en: "SPEECH CLUB INSTRUCTOR" }
    },
    speech_bm: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校级马来文演讲比赛(Pidato)", ms: "PERTANDINGAN PIDATO BAHASA MELAYU PERINGKAT SEKOLAH", en: "SCHOOL MALAY PUBLIC SPEAKING (PIDATO) COMPETITION" },
      tarikh: "20.03.2026", masa: "8:00 AM – 11:30 AM",
      penglibatan: { zh: "四、五、六年级学生", ms: "MURID TAHUN 4, 5 & 6", en: "STANDARD 4, 5 & 6 STUDENTS" },
      pengenalan: {
        zh: "马来文演讲比赛旨在训练学生用精炼正式的国语阐述观点、展现演讲功底与即席回答问题（Spontan）能力。",
        ms: "Pertandingan Pidato Bahasa Melayu menguji kemahiran pengutaraan idea berstruktur, sebutan baku, serta jawapan soalan spontan.",
        en: "Tests formal Malay oratory skills, structured argument formulation, and quick impromptu (spontan) Q&A responses."
      },
      objektif: {
        zh: "• 提高学生运用高阶国语语法与成语（Peribahasa）表达的能力。\n• 训练面对随机提问时的即时临场应变思维。\n• 代表学校出战县级Pidato赛场。",
        ms: "• Meningkatkan penggunaan tatabahasa tinggi dan peribahasa Melayu.\n• Membina kemahiran berfikir secara spontan semasa soalan dikemukakan.\n• Memilih pemidato cemerlang ke Pertandingan Pidato MSSD.",
        en: "• To promote high-level Malay grammar and proverbial phrasing.\n• To train rapid spontaneous thinking during judge Q&A sessions.\n• To select outstanding orators for district Pidato championships."
      },
      impak: {
        zh: "• 参赛选手展现了熟练的国语公开发言水平与严密逻辑。\n• 大幅增强了华小生说标准国语的自信心。\n• 选出了备战区级比赛的优胜代表。",
        ms: "• Peserta mengutarakan hujah pidato dengan tersusun dan meyakinkan.\n• Tahap keyakinan bercakap Bahasa Melayu baku bertambah mantap.\n• Skuad pidato sekolah berjaya dibentuk dengan mantap.",
        en: "• Speakers presented structured arguments in flawless Malay.\n• Significantly boosted non-native speaker confidence in public oratorical skills.\n• Successfully selected top champions for regional rounds."
      },
      teacher: { zh: "阿兹曼老师", ms: "EN. AZMAN BIN AHMAD", en: "MR. AZMAN BIN AHMAD" },
      position: { zh: "国语演讲主教练", ms: "JURULATIH PIDATO BAHASA MELAYU", en: "MALAY PIDATO HEAD COACH" }
    },
    speech_bi: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校级英文演讲比赛(Public Speaking)", ms: "PERTANDINGAN PIDATO BAHASA INGGERIS (ENGLISH PUBLIC SPEAKING)", en: "SCHOOL ENGLISH PUBLIC SPEAKING COMPETITION" },
      tarikh: "22.03.2026", masa: "8:00 AM – 11:30 AM",
      penglibatan: { zh: "四、五、六年级学生", ms: "MURID TAHUN 4, 5 & 6", en: "STANDARD 4, 5 & 6 STUDENTS" },
      pengenalan: {
        zh: "英文演讲比赛旨在培养学生的英文辩证思维、演说气场、精准发音与即席演说（Impromptu Speech）才华。",
        ms: "Pertandingan Public Speaking Bahasa Inggeris dianjurkan untuk mengetengahkan kemahiran pidato persuasif dan ucapan spontan.",
        en: "Fosters persuasive English speechwriting, vocal dynamics, flawless pronunciation, and impromptu speech skills."
      },
      objektif: {
        zh: "• 培养用流畅英文撰写与公开发表逻辑演说稿的能力。\n• 训练在无准备情况下进行1分钟即席演讲的应变力。\n• 遴选代表出战全县（MSSD）英文演讲比赛。",
        ms: "• Memupuk kemahiran menulis dan menyampaikan ucapan Bahasa Inggeris.\n• Melatih minda menghasilkan ucapan spontan (impromptu) dalam masa singkat.\n• Memilih wakil ke Pertandingan English Public Speaking MSSD.",
        en: "• To craft and deliver logical, persuasive English speeches.\n• To train rapid spontaneous speech construction under strict timers.\n• To select representatives for district English Public Speaking meets."
      },
      impak: {
        zh: "• 选手在预备演讲与即席演讲环节中均表现超卓。\n• 展现了高水平的英文应用能力与国际化视野。\n• 成功确定了校级英文演讲代表人选。",
        ms: "• Peserta memperlihatkan ketetapan tatabahasa dan sebutan fasih.\n• Meningkatkan daya pemikiran global dan keberanian berucap.\n• Wakil sekolah bersedia dengan penuh yakin ke peringkat daerah.",
        en: "• Participants excelled in both prepared and impromptu categories.\n• Demonstrated sophisticated global perspectives and English mastery.\n• Confidently finalized school delegates for inter-school competitions."
      },
      teacher: { zh: "陈淑芬老师", ms: "PN. TAN SOOK FUN", en: "MS. TAN SOOK FUN" },
      position: { zh: "英文演讲指导老师", ms: "JURULATIH ENGLISH PUBLIC SPEAKING", en: "ENGLISH PUBLIC SPEAKING COACH" }
    },
    deklamasi_sajak: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校级诗歌朗诵比赛", ms: "PERTANDINGAN DEKLAMASI SAJAK & PUISI PERINGKAT SEKOLAH", en: "SCHOOL POETRY RECITATION COMPETITION" },
      tarikh: "05.04.2026", masa: "8:00 AM – 11:30 AM",
      penglibatan: { zh: "一至六年级学生", ms: "SEMUA MURID TAHUN 1 - 6", en: "ALL STANDARD 1 - 6 STUDENTS" },
      pengenalan: {
        zh: "诗歌朗诵比赛旨在通过韵律诵读与文学赏析，体验文学诗歌的意境之美，提高朗诵情感与艺术感染力。",
        ms: "Pertandingan Deklamasi Sajak dan Puisi dianjurkan bagi memupuk apresiasi terhadap keindahan seni sastera dan sajak.",
        en: "Celebrates literary appreciation and poetic rhythm through emotional vocal expression, cadence, and artistic recitation."
      },
      objektif: {
        zh: "• 感受诗歌词句优美意境，提高文学艺术鉴赏力。\n• 训练声调高低起伏、吐字清晰度与真情实感的表达。\n• 遴选优秀朗诵代表参加县级诗歌朗诵赛。",
        ms: "• menghayati keindahan sastera sajak dan irama puisi.\n• Memupuk kemahiran intonasi, sebutan, dan nada emosi yang tepat.\n• Memilih wakil sekolah ke Pertandingan Deklamasi Sajak MSSD.",
        en: "• To foster appreciation for literary beauty, rhythm, and poetry.\n• To practice emotional inflection, clear diction, and vocal harmony.\n• To select poem recitation representatives for district meets."
      },
      impak: {
        zh: "• 朗诵过程深情款款，充分展现了诗歌韵律之美。\n• 增强了学生对文学创作与经典朗诵的喜爱。\n• 顺利选拔出代表学校参赛的朗诵精英。",
        ms: "• Peserta mendeklamasikan sajak dengan nada emosi yang mengagumkan.\n• Minat terhadap karya sastera dan puisi bertambah ke arah positif.\n• Skuad deklamasi sajak sekolah berjaya dibentuk.",
        en: "• Contestants delivered moving recitations with rich artistic cadence.\n• Elevated student interest in literary arts and creative poetry.\n• Formed an expressive poetry team for inter-school contests."
      },
      teacher: { zh: "黄美玲老师", ms: "PN. WONG MEI LING", en: "MS. WONG MEI LING" },
      position: { zh: "诗歌朗诵负责老师", ms: "PENYELARAS DEKLAMASI SAJAK", en: "POETRY RECITATION COORDINATOR" }
    },
    perarakan_merdeka: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "国庆日校园爱心游行活动", ms: "PERARAKAN HARI KEBANGSAAN PERINGKAT SEKOLAH", en: "NATIONAL DAY CAMPUS PATRIOTIC PARADE" },
      tarikh: "28.08.2026", masa: "7:30 AM – 9:30 AM",
      penglibatan: { zh: "全体师生", ms: "SEMUA WARGA SEKOLAH", en: "ALL SCHOOL MEMBERS" },
      pengenalan: {
        zh: "全校师生挥舞辉煌条纹国旗在校园内及周边游行，高唱爱国歌曲，展现昂扬的国情自豪感与爱国心。",
        ms: "Perarakan ini melibatkan semua warga sekolah mengibarkan Jalur Gemilang dan menyanyikan lagu-lagu patriotik.",
        en: "Students and teachers march across campus waving national flags and singing patriotic songs to build national unity."
      },
      objektif: {
        zh: "• 欢庆国家独立，增强全校师生的国族认同与凝聚力。\n• 展现爱国情怀与团结一致的精神风貌。\n• 烘托国庆月喜庆热烈的节日气氛。",
        ms: "• Memupuk semangat cinta akan negara dan kesatuan warga sekolah.\n• Menampilkan semangat patriotisme menerusi perarakan bermakna.\n• Menyemarakkan sambutan Bulan Kemerdekaan.",
        en: "• To honor national independence and foster school unity.\n• To display collective pride and patriotic commitment.\n• To boost the celebratory National Month atmosphere."
      },
      impak: {
        zh: "• 游行队伍整齐划一，国旗飘扬，场面极为壮观与感人。\n• 极大地激发了全校学生的爱国主义崇高情操。\n• 留下深刻的爱国教育校园记忆。",
        ms: "• Perarakan berlangsung dengan teratur, meriah dan penuh warna-warni.\n• Semangat patriotisme dipupuk secara mendalam.\n• Kenangan perayaan yang indah dicipta bersama-sama.",
        en: "• Vibrant, disciplined parade waving flags across campus.\n• Deepened patriotism and collective civic pride.\n• Built unforgettable patriotic memories for students."
      },
      teacher: { zh: "阿兹曼老师", ms: "EN. AZMAN BIN AHMAD", en: "MR. AZMAN BIN AHMAD" },
      position: { zh: "游行总指挥", ms: "PENYELARAS PERARAKAN", en: "PARADE COMMANDER" }
    },
    kem_badan_beruniform: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "制服团体露营与技能训练营", ms: "PERKHEMAHAN BADAN BERUNIFORM PERINGKAT SEKOLAH", en: "UNIFORMED UNITS LEADERSHIP & CAMPING CAMP" },
      tarikh: "12.09.2026", masa: "2 天 1 夜 (2 Hari 1 Malam)",
      penglibatan: { zh: "四、五、六年级制服团体成员", ms: "MURID BADAN BERUNIFORM TAHUN 4, 5 & 6", en: "UNIFORMED UNIT MEMBERS (STD 4-6)" },
      pengenalan: {
        zh: "举办两天一夜制服团体营，进行搭建帐篷、急救包扎、步操演练及急变应变，培养纪律与生存技能。",
        ms: "Perkhemahan ini dijalankan bagi menguji kemahiran kawad kaki, ikatan, pertolongan cemas, dan berdikari murid.",
        en: "A 2D1N outdoor camping activity testing foot drills, knotting, first aid, and self-reliance skills."
      },
      objektif: {
        zh: "• 强化制服团体的纪律性、服从意识与独立生活能力。\n• 掌握野外生存、打结包扎及急救实用技能。\n• 培养团队领袖能力与同甘共苦的合作友谊。",
        ms: "• Memupuk disiplin, jati diri, dan sifat berdikari murid.\n• Menguasai kemahiran asas perkhemahan dan pertolongan cemas.\n• Membina kepimpinan dan kerjasama pasukan yang kukuh.",
        en: "• To build discipline, character, and independent life skills.\n• To master basic camping, knots, and emergency first aid.\n• To foster teamwork, leadership, and camaraderie."
      },
      impak: {
        zh: "• 营员克服困难，顺利完成了野外露营与各项考查挑战。\n• 显著提升了学生的独立性与纪律服从意识。\n• 制服团体的团队凝聚力得到了质的飞跃。",
        ms: "• Peserta berjaya menamatkan semua aktiviti dengan penuh ketahanan.\n• Tahap disiplin dan kebolehan berdikari murid meningkat kukuh.\n• Semangat koperatif badan beruniform bertambah mantap.",
        en: "• Campers successfully completed outdoor endurance challenges.\n• Markedly improved discipline and personal self-reliance.\n• High team unity achieved among all uniformed units."
      },
      teacher: { zh: "林嘉伟老师", ms: "PN. LIM CHIA WEI", en: "MS. LIM CHIA WEI" },
      position: { zh: "制服团体总协调", ms: "PENYELARAS BADAN BERUNIFORM", en: "UNIFORMED UNITS COORDINATOR" }
    },
    kem_motivasi: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "高年级“自我突破”学生生活营", ms: "KEM MOTIVASI & BINA INSAN MURID PERINGKAT SEKOLAH", en: "MOTIVATIONAL & LIFE SKILLS CAMP" },
      tarikh: "15.08.2026", masa: "8:00 AM – 5:00 PM",
      penglibatan: { zh: "高年级学生", ms: "MURID TAHUN 5 & 6", en: "STANDARD 5 & 6 STUDENTS" },
      pengenalan: {
        zh: "通过团队破冰、心理拓展、感恩教育及目标设立等环节，帮助学生克服自卑，建立积极进取的人生姿态。",
        ms: "Kem motivasi ini dianjurkan bagi membina jati diri, keyakinan diri, serta memberi dorongan positif kepada murid.",
        en: "Interactive day camp focused on self-discovery, resilience, goal setting, and team bonding."
      },
      objektif: {
        zh: "• 明确学习目标，激发内在驱动力与抗挫折心理弹性。\n• 培养感恩父母教诲、尊重师长与团结同侪的美德。\n• 掌握情绪管理与科学高效的时间规划技巧。",
        ms: "• Memupuk kesedaran diri, matlamat belajar, dan sikap positif.\n• Menyemai nilai kasih sayang, menghargai ibu bapa dan guru.\n• Meningkatkan kemahiran pengurusan masa dan emosi.",
        en: "• To define learning goals and foster personal resilience.\n• To cultivate gratitude towards parents and teachers.\n• To improve emotional regulation and time management."
      },
      impak: {
        zh: "• 感恩环节触动人心，有效提升了学生的心智成熟度。\n• 激发了学生冲刺学业目标的强烈斗志与自信。\n• 形成了互助友爱的班级学习共同体。",
        ms: "• Sesi refleksi menyentuh emosi dan mendewasakan fikiran murid.\n• Motivasi belajar murid meningkat secara mendalam.\n• Keharmonian dan ikatan ukhuwah sesama murid terjalin.",
        en: "• Heartfelt reflection sessions raised emotional maturity.\n• Sparked strong motivation toward academic performance.\n• Created a supportive learning atmosphere across grades."
      },
      teacher: { zh: "廖秀兰老师", ms: "PN. LIAW SIEW LAN", en: "MS. LIAW SIEW LAN" },
      position: { zh: "生活营总策划", ms: "PENYELARAS KEM MOTIVASI", en: "MOTIVATIONAL CAMP COORDINATOR" }
    },
    kem_kepimpinan: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "小领袖(巡察员、图书管理员)领袖营", ms: "KEM KEPIMPINAN PEMIMPIN MUDA SEKOLAH", en: "STUDENT LEADERSHIP TRAINING CAMP" },
      tarikh: "07.03.2026", masa: "8:00 AM – 4:00 PM",
      penglibatan: { zh: "巡察员、图书管理员及班长", ms: "PENGAWAS, PENGAWAS PUSAT SUMBER & KETUA KELAS", en: "PREFECTS, LIBRARIANS & CLASS MONITORS" },
      pengenalan: {
        zh: "专为学校学生领袖量身打造，旨在培训岗位履职技能、公信力沟通、危机处理及模范带头作用。",
        ms: "Dianjurkan khas untuk pemimpin muda bagi mengasah kemahiran pengurusan, pengucapan dan ketrampilan kepimpinan.",
        en: "Specially tailored for student leaders to refine team management, communication, and conflict resolution skills."
      },
      objektif: {
        zh: "• 明确领袖岗位职责，提高纪律执法与服务意识。\n• 锻炼人际沟通、团队指挥与公共说服能力。\n• 树立榜样标杆，协助教师维护良好的校园秩序。",
        ms: "• Memahami peranan dan tanggungjawab sebagai pemimpin sekolah.\n• Membina kemahiran komunikasi dan pengurusan tugasan.\n• Menjadi teladan yang berdisiplin tinggi kepada murid lain.",
        en: "• To clarify leadership responsibilities and service duties.\n• To build effective team communication and organization skills.\n• To establish strong role models for maintaining order."
      },
      impak: {
        zh: "• 小领袖们的执勤自信心与责任担当意识显著增强。\n• 学会了妥善处理同学间冲突纠纷的技巧。\n• 极大地提升了学生自治管理的成效。",
        ms: "• Keyakinan menjalankan tugas harian bertambah mantap.\n• Kemahiran menyelesaikan masalah dan konflik dipelajari.\n• Pengurusan kawalan murid di sekolah menjadi lebih teratur.",
        en: "• Markedly increased confidence and sense of responsibility.\n• Equipped leaders with effective peer conflict solving.\n• Streamlined student body governance and daily prefect duties."
      },
      teacher: { zh: "颜玉兰老师", ms: "PN. GAN YOK LAN", en: "MS. GAN YOK LAN" },
      position: { zh: "纪律与领袖营负责老师", ms: "PENYELARAS KEPIMPINAN & DISIPLIN", en: "DISCIPLINE & LEADERSHIP COORDINATOR" }
    },
    gotong_royong: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校园清洁大扫除 (Gotong-Royong MADANI)", ms: "PROGRAM GOTONG-ROYONG MADANI PERINGKAT SEKOLAH", en: "CAMPUS GOTONG-ROYONG CLEANLINESS DRIVE" },
      tarikh: "18.04.2026", masa: "8:00 AM – 11:30 AM",
      penglibatan: { zh: "全体师生及家协成员", ms: "SEMUA WARGA SEKOLAH & PIBG", en: "ALL SCHOOL MEMBERS & PTA" },
      pengenalan: {
        zh: "开展全校性大扫除活动，分工清理教室、美化校园绿化带、清除积水，共同打造整洁安全的环境。",
        ms: "Aktiviti gotong-royong ini diadakan untuk membersihkan kawasan sekolah, keceriaan dan pembersihan takungan air.",
        en: "Community cleaning drive organized to spruce up classrooms, grounds, and eliminate health hazards."
      },
      objektif: {
        zh: "• 清理校园死角，消除蚊虫滋生与卫生安全隐患。\n• 培养学生劳育精神、爱护校园环境与团结劳动常识。\n• 增进教师、学生及家长之间的协同配合感情。",
        ms: "• Membersihkan persekitaran sekolah daripada risiko penyakit.\n• Memupuk nilai murni kebersihan dan semangat bekerjasama.\n• Mengeratkan hubungan silaturahim warga sekolah dan PIBG.",
        en: "• To eliminate safety hazards and potential vector sites.\n• To cultivate civic pride, labor value, and environmental responsibility.\n• To build strong bonds among staff, students, and parents."
      },
      impak: {
        zh: "• 校园面貌焕然一新，卫生死角得到彻底整治。\n• 学生体验到了劳动的快乐与团队合作的重要性。\n• 筑牢了校园卫生与骨痛热症安全防线。",
        ms: "• Persekitaran sekolah bertambah bersih, ceria, dan selamat.\n• Semangat kerjasama antara murid dan warga sekolah terpancar.\n• Risiko pembiakan penyakit bawaan nyamuk dapat dicegah.",
        en: "• Transformed school grounds into a clean and safe area.\n• Fostered practical teamwork and labor respect among students.\n• Significantly reduced health risks across the campus."
      },
      teacher: { zh: "吴伟祥老师", ms: "EN. NG WEI SIANG", en: "MR. NG WEI SIANG" },
      position: { zh: "美化与卫生委员会主任", ms: "PENYELARAS KEBERSIHAN & KECERIAAN", en: "CLEANLINESS & BEAUTIFICATION COORDINATOR" }
    },
    duta_sahsiah: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "品德大使计划(Program Duta Sahsiah)", ms: "PROGRAM DUTA SAHSIAH PEMANGKIN KARAKTER PERINGKAT SEKOLAH", en: "PROGRAM DUTA SAHSIAH STUDENT CHARACTER LAUNCH" },
      tarikh: "09.03.2026", masa: "8:00 AM – 10:00 AM",
      penglibatan: { zh: "全体师生及皇家警察代表", ms: "SEMUA WARGA SEKOLAH & PEGAWAI PERHUBUNGAN POLIS (PPS)", en: "ALL SCHOOL MEMBERS & POLICE OFFICERS" },
      pengenalan: {
        zh: "配合教育部与警局合作推出，委任品德大使，宣导防范犯罪、遵守纪律与文明网络使用（Cybercrime）。",
        ms: "Program kerjasama KPM dan PDRM bagi melantik Duta Sahsiah serta menyebar mesej pencegahan jenayah dan salah laku.",
        en: "Joint ministry and police initiative appointing Student Ambassadors to champion good conduct, anti-crime, and cyber safety."
      },
      objektif: {
        zh: "• 树立卓越品德楷模，宣导正向遵纪守法观念。\n• 提高对青少年犯罪、网络霸凌及不良行为的防范意识。\n• 加强学校与皇家警察 (PDRM) 的校企安全防范机制。",
        ms: "• Membentuk karakter murid yang berdisiplin dan berintegriti.\n• Mengelakkan penglibatan murid dalam salah laku dan jenayah.\n• Memantapkan kerjasama sekolah bersama pihak PDRM.",
        en: "• To instill student integrity, ethics, and disciplined character.\n• To raise awareness against juvenile crime and cyberbullying.\n• To strengthen collaboration with the Royal Malaysia Police."
      },
      impak: {
        zh: "• 成功委任品德大使，发挥同侪正面引导与影响力。\n• 学生对警范防范与纪律底线有了更加清晰的认知。\n• 巩固了零犯罪、守纪律的良好校风。",
        ms: "• Pelantikan Duta Sahsiah memberi pengaruh positif kepada murid.\n• Kesedaran mematuhi peraturan dan undang-undang meningkat.\n• Persekitaran sekolah menjadi lebih aman dan berdisiplin.",
        en: "• Student Ambassadors now actively guide peers toward good ethics.\n• Increased understanding of school rules and law compliance.\n• Solidified a disciplined, secure, and respectful atmosphere."
      },
      teacher: { zh: "颜玉兰老师", ms: "PN. GAN YOK LAN", en: "MS. GAN YOK LAN" },
      position: { zh: "纪律与品德主任", ms: "PENYELARAS DISIPLIN & SAHSIAH", en: "DISCIPLINE & CHARACTER COORDINATOR" }
    },
    majlis_graduasi: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "第六十二届六年级毕业典礼", ms: "MAJLIS GRADUASI MURID TAHUN 6 PERINGKAT SEKOLAH", en: "YEAR 6 GRADUATION CEREMONY" },
      tarikh: "18.12.2026", masa: "8:00 AM – 12:00 PM",
      penglibatan: { zh: "六年级毕业生、全体师生及家长", ms: "MURID TAHUN 6, GURU & IBU BAPA", en: "YEAR 6 GRADUATES, TEACHERS & PARENTS" },
      pengenalan: {
        zh: "为六年级毕业生举办庄严隆重的毕业典礼，见证完成小学学业，表彰优秀表现，表达对母校与师长的感恩。",
        ms: "Majlis Graduasi dianjurkan bagi meraikan kejayaan murid Tahun 6 tamat persekolahan rendah dengan penuh kenangan.",
        en: "Celebrates Year 6 graduates completing primary education, recognizing achievements and expressing gratitude."
      },
      objektif: {
        zh: "• 见证并欢送六年级毕业生完成小学阶段六年的学习。\n• 表达尊师重道与感谢母校培育的感恩之情。\n• 激励毕业生在迈入中学阶段后继续奋发图强。",
        ms: "• Meraikan pencapaian murid Tahun 6 tamat sekolah rendah.\n• Memupuk rasa menghargai jasa guru dan sekolah.\n• Memberi motivasi murid melangkah ke alam sekolah menengah.",
        en: "• To mark the completion of 6-year primary school education.\n• To foster appreciation for teachers' guidance and education.\n• To motivate students as they transition to secondary education."
      },
      impak: {
        zh: "• 典礼温馨感人，毕业生怀着感恩之心领取毕业证书。\n• 家长深受感动，高度赞扬学校的育人成果。\n• 留下了美好而难忘的母校成长印记。",
        ms: "• Majlis berlangsung penuh emosi, syahdu dan membanggakan.\n• Penghargaan daripada ibu bapa terhadap dedikasi guru.\n• Kenangan indah berjaya dicipta untuk para graduan.",
        en: "• Moving graduation ceremony filled with pride and gratitude.\n• Parents expressed deep appreciation for school education quality.\n• Created cherished lifelong memories for graduating students."
      },
      teacher: { zh: "陈淑芬老师", ms: "PN. TAN SOOK FUN", en: "MS. TAN SOOK FUN" },
      position: { zh: "毕业典礼筹委会主席", ms: "PENYELARAS MAJLIS GRADUASI", en: "GRADUATION CEREMONY CHAIRPERSON" }
    },
    majlis_anugerah: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "毕业典礼暨颁奖典礼 (Majlis Anugerah Cemerlang)", ms: "MAJLIS ANUGERAH KECEMERLANGAN & APRESIASI MURID", en: "EXCELLENCE AWARDS & APPRECIATION DAY" },
      tarikh: "19.12.2026", masa: "8:00 AM – 12:30 PM",
      penglibatan: { zh: "全体获奖学生、师生及家协理事", ms: "SEMUA WARGA SEKOLAH, IBU BAPA & PIBG", en: "ALL SCHOOL MEMBERS, PARENTS & PTA" },
      pengenalan: {
        zh: "颁奖典礼旨在表彰在学术、体育、课外活动及品德表现杰出的学生，树立学习榜样，激励全校追求卓越。",
        ms: "Majlis ini dianjurkan bagi memberi penghargaan atas pencapaian cemerlang murid dalam akademik, kokurikulum dan sahsiah.",
        en: "Organized to honor student achievements in academics, co-curriculars, sports, and exemplary character."
      },
      objektif: {
        zh: "• 表彰学生在学术、体育及课外活动取得的辉煌成绩。\n• 树立身边榜样，激发全校学生争先创优的积极性。\n• 加强家校共育，见证学生的成长与飞跃。",
        ms: "• Memberi pengiktirafan pencapaian cemerlang akademik dan kokurikulum.\n• Memotivasi murid-murid lain untuk terus berusaha gigih.\n• Mengeratkan sokongan ibu bapa terhadap visi sekolah.",
        en: "• To reward outstanding performance in academic and co-curricular areas.\n• To inspire all students toward excellence and self-improvement.\n• To celebrate child growth together with parents and community."
      },
      impak: {
        zh: "• 获奖学生备受鼓舞，表达了继续努力进取的决心。\n• 现场掌声不断，营造了崇尚卓越与勤奋的良好校风。\n• 显著提升了家校合作与对学校教育的满意度。",
        ms: "• Murid teruja dan bersemangat menerima anugerah kejayaan.\n• Membudayakan keunggulan dan usaha gigih dalam kalangan murid.\n• Kerjasama sekolah dan ibu bapa terus diperkukuh.",
        en: "• Awardees felt highly recognized and motivated to achieve more.\n• Built a school culture celebrating hard work and excellence.\n• Strengthened parental trust and appreciation toward the school."
      },
      teacher: { zh: "黄美玲老师", ms: "PN. WONG MEI LING", en: "MS. WONG MEI LING" },
      position: { zh: "颁奖礼总协调", ms: "PENYELARAS MAJLIS ANUGERAH", en: "AWARDS DAY COORDINATOR" }
    },
    shu_fa: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校级书法比赛", ms: "PERTANDINGAN SENI KALIGRAFI PERINGKAT SEKOLAH", en: "SCHOOL CHINESE CALLIGRAPHY COMPETITION" },
      tarikh: "15.04.2026", masa: "8:00 AM – 10:30 AM",
      penglibatan: { zh: "三至六年级学生", ms: "MURID TAHUN 3 - 6", en: "STANDARD 3 - 6 STUDENTS" },
      pengenalan: {
        zh: "校级书法比赛旨在培养学生沉着冷静的心性，提升毛笔笔法、结构掌控与书法艺术修养，发掘优秀书法苗子。",
        ms: "Pertandingan Seni Kaligrafi dianjurkan bagi membina ketenangan minda, menguasai teknik garisan dan struktur tulisan, serta mencungkil bakat seni murid.",
        en: "The Chinese Calligraphy Competition aims to cultivate patience, refine brush control techniques, and discover outstanding calligraphic talents."
      },
      objektif: {
        zh: "• 培养学生细心、静心与持之以恒的书写品质。\n• 提升汉字结构美感与毛笔运笔技巧。\n• 选拔优秀书法选手代表学校参加校际与县级书法大赛。",
        ms: "• Memupuk sifat kesabaran, ketenangan, dan disiplin tinggi murid.\n• Meningkatkan kemahiran mengawal berus dan struktur watak tulisan.\n• Memilih wakil sekolah ke Pertandingan Kaligrafi peringkat daerah.",
        en: "• To instill mindfulness, focus, and perseverance in handwriting.\n• To master brush stroke control and character structural balance.\n• To select calligraphic representatives for district-level competitions."
      },
      impak: {
        zh: "• 参赛选手展现了极高的书法技巧与静心书写风貌。\n• 评选出了多幅笔法稳健、结构严谨的优秀书法作品。\n• 组建了代表学校备战外赛的书法代表队。",
        ms: "• Peserta menampakkan teknik penulisan yang mantap dan berdisiplin.\n• Karya tulisan cemerlang berjaya dipilih menerusi penilaian teliti.\n• Skuad kaligrafi sekolah berjaya dibentuk ke pertandingan luar.",
        en: "• Participants displayed steady brushwork and serene composure.\n• Selected multiple top-tier artworks demonstrating excellent structure.\n• Successfully formed the school calligraphy squad for future contests."
      },
      teacher: { zh: "曾佳薇老师", ms: "PN. CHEN CHIA WEI", en: "MS. CHEN CHIA WEI" },
      position: { zh: "华文科与书法学会指导老师", ms: "PENYELARAS KELAB KALIGRAFI", en: "CALLIGRAPHY CLUB INSTRUCTOR" }
    },
    tanglung_making: {
      school: { zh: "SJKC PEI HUA", ms: "SJKC PEI HUA", en: "SJKC PEI HUA" },
      title: { zh: "校级灯笼制作比赛", ms: "PERTANDINGAN MEMBUAT TANGLUNG PERINGKAT SEKOLAH", en: "SCHOOL LANTERN MAKING COMPETITION" },
      tarikh: "25.09.2026", masa: "8:00 AM – 10:30 AM",
      penglibatan: { zh: "一至六年级学生", ms: "SEMUA MURID TAHUN 1 - 6", en: "ALL STANDARD 1 - 6 STUDENTS" },
      pengenalan: {
        zh: "灯笼制作比赛旨在弘扬中秋传统文化，激发学生的创意与动手能力，鼓励使用环保材料创作独特艺术品。",
        ms: "Pertandingan membuat tanglung dianjurkan untuk memelihara warisan budaya Pesta Tanglung, di samping menggalakkan kreativiti menggunakan bahan kitar semula.",
        en: "The lantern making competition aims to promote Mid-Autumn traditions, inspiring creativity and hands-on skills while encouraging the use of recycled materials."
      },
      objektif: {
        zh: "• 传承与发扬中秋节传统文化美德。\n• 培养学生的环保意识与废物利用的创意构思。\n• 增强学生的动手实践能力与艺术审美感受。",
        ms: "• Memelihara warisan dan tradisi Pesta Tanglung.\n• Memupuk kesedaran alam sekitar dan kreativiti menggunakan bahan terbuang.\n• Meningkatkan kemahiran praktikal dan estetika seni murid.",
        en: "• To preserve and promote traditional Mid-Autumn festival culture.\n• To foster environmental awareness and creativity in upcycling.\n• To enhance students' practical crafting skills and artistic appreciation."
      },
      impak: {
        zh: "• 学生们提交了许多富有创意且精美的环保灯笼作品。\n• 加深了学生对中华传统节日的文化认同与认识。\n• 校园内充满了浓厚温暖的中秋节日气氛。",
        ms: "• Murid menghasilkan pelbagai tanglung kreatif yang cantik dan mesra alam.\n• Memperdalam kefahaman murid tentang perayaan tradisional Cina.\n• Menyemarakkan suasana perayaan Pesta Tanglung di sekolah.",
        en: "• Students submitted numerous creative and beautiful eco-friendly lanterns.\n• Deepened students' understanding of traditional Chinese festivals.\n• The campus was filled with a strong Mid-Autumn festive atmosphere."
      },
      teacher: { zh: "美术老师", ms: "GURU PENDIDIKAN SENI", en: "ART TEACHER" },
      position: { zh: "美术学会顾问老师", ms: "PENASIHAT KELAB SENI", en: "ART CLUB ADVISOR" }
    }
  };


  // 自动补齐活动预设：确保下拉菜单中的每项活动都能载入对应内容。
  const additionalPresetCatalog = {
  "tahun_baru_cina": [
    "校级农历新年庆典",
    "Sambutan Tahun Baharu Cina Peringkat Sekolah",
    "School Chinese New Year Celebration",
    "全体师生",
    "SEMUA WARGA SEKOLAH",
    "ALL SCHOOL MEMBERS"
  ],
  "yuan_xiao": [
    "元宵节（元宵猜灯谜）庆典",
    "Sambutan Chap Goh Mei dan Teka-teki Tanglung",
    "Chap Goh Mei Lantern Riddles Celebration",
    "全体师生",
    "SEMUA WARGA SEKOLAH",
    "ALL SCHOOL MEMBERS"
  ],
  "duan_wu": [
    "端午节（赛龙舟与裹粽文化）庆典",
    "Sambutan Pesta Duan Wu: Perahu Naga dan Pembuatan Bak Chang",
    "Dragon Boat Festival and Rice Dumpling Cultural Celebration",
    "全体师生",
    "SEMUA WARGA SEKOLAH",
    "ALL SCHOOL MEMBERS"
  ],
  "zhong_qiu": [
    "中秋节提灯晚会与赏月庆典",
    "Malam Tanglung dan Sambutan Pesta Kuih Bulan",
    "Mid-Autumn Lantern Night and Moon-Viewing Celebration",
    "全体师生与家长",
    "WARGA SEKOLAH DAN IBU BAPA",
    "SCHOOL MEMBERS AND PARENTS"
  ],
  "dong_zhi": [
    "冬至（搓汤圆）文化庆典",
    "Sambutan Kebudayaan Dongzhi: Membuat Tangyuan",
    "Dongzhi Cultural Celebration: Making Tangyuan",
    "全体师生",
    "SEMUA WARGA SEKOLAH",
    "ALL SCHOOL MEMBERS"
  ],
  "hari_raya": [
    "校级开斋节庆典",
    "Sambutan Hari Raya Aidilfitri Peringkat Sekolah",
    "School Hari Raya Aidilfitri Celebration",
    "全体师生",
    "SEMUA WARGA SEKOLAH",
    "ALL SCHOOL MEMBERS"
  ],
  "deepavali": [
    "校级屠妖节庆典",
    "Sambutan Deepavali Peringkat Sekolah",
    "School Deepavali Celebration",
    "全体师生",
    "SEMUA WARGA SEKOLAH",
    "ALL SCHOOL MEMBERS"
  ],
  "pesta_kaamatan_gawai": [
    "丰收节（Kaamatan & Gawai）庆典",
    "Sambutan Pesta Kaamatan dan Gawai",
    "Kaamatan and Gawai Harvest Festival Celebration",
    "全体师生",
    "SEMUA WARGA SEKOLAH",
    "ALL SCHOOL MEMBERS"
  ],
  "yi_mai_hui": [
    "校园爱心义卖会",
    "Jualan Amal Sekolah",
    "School Charity Sale",
    "全体师生与家长",
    "WARGA SEKOLAH DAN IBU BAPA",
    "SCHOOL MEMBERS AND PARENTS"
  ],
  "zhong_qiu_yuan_you_hui": [
    "中秋园游会",
    "Karnival Pesta Kuih Bulan",
    "Mid-Autumn Festival Carnival",
    "全体师生与家长",
    "WARGA SEKOLAH DAN IBU BAPA",
    "SCHOOL MEMBERS AND PARENTS"
  ],
  "er_tong_jie": [
    "校级儿童节庆典",
    "Sambutan Hari Kanak-kanak Peringkat Sekolah",
    "School Children's Day Celebration",
    "全体师生",
    "SEMUA WARGA SEKOLAH",
    "ALL SCHOOL MEMBERS"
  ],
  "mewarna_dengue": [
    "防黑斑蚊（骨痛热症）填色及画海报比赛",
    "Pertandingan Mewarna dan Melukis Poster Kesedaran Denggi",
    "Dengue Prevention Colouring and Poster Competition",
    "一至六年级学生",
    "MURID TAHUN 1 - 6",
    "STANDARD 1 - 6 STUDENTS"
  ],
  "mewarna_antibuli": [
    "“拒绝霸凌，关爱友谊”反霸凌填色比赛",
    "Pertandingan Mewarna Anti-Buli: Tolak Buli, Eratkan Persahabatan",
    "Anti-Bullying Colouring Contest: Say No to Bullying, Build Friendship",
    "一至六年级学生",
    "MURID TAHUN 1 - 6",
    "STANDARD 1 - 6 STUDENTS"
  ],
  "kuiz_merdeka_new": [
    "国庆日与国情历史知识问答比赛",
    "Pertandingan Kuiz Hari Kebangsaan dan Sejarah Negara",
    "National Day and Malaysian History Quiz",
    "四至六年级学生",
    "MURID TAHUN 4 - 6",
    "STANDARD 4 - 6 STUDENTS"
  ],
  "bertulis_sejarah": [
    "校级历史笔试比赛",
    "Pertandingan Ujian Bertulis Sejarah Peringkat Sekolah",
    "School History Written Competition",
    "四至六年级学生",
    "MURID TAHUN 4 - 6",
    "STANDARD 4 - 6 STUDENTS"
  ],
  "perarakan_merdeka": [
    "国庆日校园爱心游行活动",
    "Perarakan Patriotik Hari Kebangsaan",
    "National Day Patriotic Parade",
    "全体师生",
    "SEMUA WARGA SEKOLAH",
    "ALL SCHOOL MEMBERS"
  ],
  "kem_badan_beruniform": [
    "制服团体露营与技能训练营",
    "Perkhemahan dan Kemahiran Unit Beruniform",
    "Uniformed Bodies Camping and Skills Camp",
    "制服团体学生",
    "AHLI UNIT BERUNIFORM",
    "UNIFORMED BODIES MEMBERS"
  ],
  "kem_motivasi": [
    "高年级“自我突破”学生生活营",
    "Kem Jati Diri dan Motivasi Murid Tahap 2",
    "Upper Primary Self-Development Camp",
    "四至六年级学生",
    "MURID TAHUN 4 - 6",
    "STANDARD 4 - 6 STUDENTS"
  ],
  "kem_kepimpinan": [
    "小领袖（巡察员、图书管理员）领袖营",
    "Kem Kepimpinan Pengawas dan Pengawas Pusat Sumber",
    "Student Leaders Camp for Prefects and Librarians",
    "巡察员与图书管理员",
    "PENGAWAS DAN PENGAWAS PUSAT SUMBER",
    "PREFECTS AND LIBRARIANS"
  ],
  "gotong_royong": [
    "校园清洁大扫除（Gotong-Royong MADANI）",
    "Gotong-Royong MADANI: Pembersihan dan Keceriaan Sekolah",
    "Gotong-Royong MADANI School Clean-Up",
    "全体师生与家长",
    "WARGA SEKOLAH DAN IBU BAPA",
    "SCHOOL MEMBERS AND PARENTS"
  ],
  "duta_sahsiah": [
    "品德大使计划（Program Duta Sahsiah）",
    "Program Duta Sahsiah",
    "Character Ambassador Programme",
    "全体师生",
    "WARGA SEKOLAH",
    "SCHOOL COMMUNITY"
  ],
  "majlis_graduasi": [
    "第六十二届六年级毕业典礼",
    "Majlis Graduasi Tahun 6 Kali Ke-62",
    "62nd Year 6 Graduation Ceremony",
    "六年级学生、家长与教师",
    "MURID TAHUN 6, IBU BAPA DAN GURU",
    "YEAR 6 PUPILS, PARENTS AND TEACHERS"
  ],
  "majlis_anugerah": [
    "毕业典礼暨颁奖典礼（Majlis Anugerah Cemerlang）",
    "Majlis Graduasi dan Anugerah Cemerlang",
    "Graduation and Excellence Awards Ceremony",
    "全体师生与家长",
    "WARGA SEKOLAH DAN IBU BAPA",
    "SCHOOL MEMBERS AND PARENTS"
  ],
  "tilawah_quran": [
    "校级古兰经诵读比赛（Tilawah Al-Quran）",
    "Pertandingan Tilawah Al-Quran Peringkat Sekolah",
    "School Quran Recitation Competition",
    "穆斯林学生",
    "MURID ISLAM",
    "MUSLIM PUPILS"
  ],
  "hafazan_quran": [
    "校级古兰经背诵比赛（Hafazan Al-Quran）",
    "Pertandingan Hafazan Al-Quran Peringkat Sekolah",
    "School Quran Memorisation Competition",
    "穆斯林学生",
    "MURID ISLAM",
    "MUSLIM PUPILS"
  ],
  "nasyid": [
    "校级伊斯兰赞词歌曲比赛（Festival Nasyid）",
    "Festival Nasyid Peringkat Sekolah",
    "School Nasyid Competition",
    "穆斯林学生",
    "MURID ISLAM",
    "MUSLIM PUPILS"
  ],
  "seni_khat": [
    "校级爪夷文书法（Seni Khat）比赛",
    "Pertandingan Seni Khat Peringkat Sekolah",
    "School Jawi Calligraphy Competition",
    "穆斯林学生",
    "MURID ISLAM",
    "MUSLIM PUPILS"
  ],
  "pibks_gotong_royong_komuniti": [
    "PIBKS：家校社区协作清洁与美化校园日",
    "PIBKS: Hari Gotong-Royong dan Keceriaan Sekolah",
    "PIBKS: School Clean-Up and Beautification Day",
    "教师、家长与社区成员",
    "GURU, IBU BAPA DAN KOMUNITI",
    "TEACHERS, PARENTS AND COMMUNITY"
  ],
  "pibks_career_day": [
    "PIBKS：职业分享与生涯探索日",
    "PIBKS: Hari Perkongsian Kerjaya dan Eksplorasi Kerjaya",
    "PIBKS: Career Sharing and Exploration Day",
    "学生、家长与受邀嘉宾",
    "MURID, IBU BAPA DAN JEMPUTAN",
    "PUPILS, PARENTS AND GUESTS"
  ],
  "pibks_safety_talk": [
    "PIBKS：校园与道路安全醒觉讲座",
    "PIBKS: Ceramah Kesedaran Keselamatan Sekolah dan Jalan Raya",
    "PIBKS: School and Road Safety Awareness Talk",
    "学生、教师与家长",
    "MURID, GURU DAN IBU BAPA",
    "PUPILS, TEACHERS AND PARENTS"
  ],
  "pibks_health_screening": [
    "PIBKS：健康检查与健康生活推广日",
    "PIBKS: Hari Pemeriksaan Kesihatan dan Gaya Hidup Sihat",
    "PIBKS: Health Screening and Healthy Lifestyle Day",
    "学生、教师与家长",
    "MURID, GURU DAN IBU BAPA",
    "PUPILS, TEACHERS AND PARENTS"
  ],
  "pibks_digital_literacy": [
    "PIBKS：数码素养与网络安全工作坊",
    "PIBKS: Bengkel Literasi Digital dan Keselamatan Internet",
    "PIBKS: Digital Literacy and Internet Safety Workshop",
    "学生、教师与家长",
    "MURID, GURU DAN IBU BAPA",
    "PUPILS, TEACHERS AND PARENTS"
  ],
  "pibks_reading_buddy": [
    "PIBKS：亲子阅读与社区故事分享计划",
    "PIBKS: Program Bacaan Keluarga dan Perkongsian Cerita Komuniti",
    "PIBKS: Family Reading and Community Storytelling Programme",
    "学生、家长与社区志愿者",
    "MURID, IBU BAPA DAN SUKARELAWAN KOMUNITI",
    "PUPILS, PARENTS AND COMMUNITY VOLUNTEERS"
  ],
  "pibks_environment": [
    "PIBKS：绿色校园与环保回收行动",
    "PIBKS: Sekolah Lestari dan Kempen Kitar Semula",
    "PIBKS: Green School and Recycling Initiative",
    "全体师生、家长与社区",
    "WARGA SEKOLAH, IBU BAPA DAN KOMUNITI",
    "SCHOOL MEMBERS, PARENTS AND COMMUNITY"
  ],
  "pibks_parent_workshop": [
    "PIBKS：正向亲子沟通与家庭教育讲座",
    "PIBKS: Ceramah Komunikasi Positif Ibu Bapa dan Anak",
    "PIBKS: Positive Parent-Child Communication and Family Education Talk",
    "家长与教师",
    "IBU BAPA DAN GURU",
    "PARENTS AND TEACHERS"
  ],
  "pibks_learning_support": [
    "PIBKS：社区志愿者课业辅导与学习支援计划",
    "PIBKS: Program Bimbingan Akademik dan Sokongan Pembelajaran Sukarelawan Komuniti",
    "PIBKS: Community Volunteer Tutoring and Learning Support Programme",
    "学生与社区志愿者",
    "MURID DAN SUKARELAWAN KOMUNITI",
    "PUPILS AND COMMUNITY VOLUNTEERS"
  ],
  "pibks_sports_coaching": [
    "PIBKS：家长与社区运动教练体验营",
    "PIBKS: Kem Pengalaman Kejurulatihan Sukan Ibu Bapa dan Komuniti",
    "PIBKS: Parent and Community Sports Coaching Experience Camp",
    "学生、家长与社区教练",
    "MURID, IBU BAPA DAN JURULATIH KOMUNITI",
    "PUPILS, PARENTS AND COMMUNITY COACHES"
  ],
  "pibks_charity_drive": [
    "PIBKS：爱心物资捐献与社区关怀行动",
    "PIBKS: Kempen Sumbangan Barangan dan Prihatin Komuniti",
    "PIBKS: Charity Donation and Community Care Drive",
    "教师、家长与社区成员",
    "GURU, IBU BAPA DAN KOMUNITI",
    "TEACHERS, PARENTS AND COMMUNITY"
  ]
};

  Object.entries(additionalPresetCatalog).forEach(([key, item]) => {
    if (presets[key]) return; // 保留原有已精心编写的预设内容
    const [zhTitle, msTitle, enTitle, zhAudience, msAudience, enAudience] = item;
    const school = {
      zh: "SJKC PEI HUA",
      ms: "SJKC PEI HUA",
      en: "SJKC PEI HUA"
    };
    presets[key] = {
      school,
      title: { zh: zhTitle, ms: msTitle, en: enTitle },
      tarikh: "",
      masa: "",
      penglibatan: { zh: zhAudience, ms: msAudience, en: enAudience },
      pengenalan: {
        zh: `本活动旨在通过${zhTitle}，丰富学生的学习体验，促进全人发展，并营造积极、共融的校园文化。`,
        ms: `Program ini dianjurkan melalui ${msTitle} untuk memperkaya pengalaman pembelajaran murid, menyokong perkembangan holistik dan memupuk budaya sekolah yang positif serta inklusif.`,
        en: `This programme is organised to enrich pupils' learning experiences, support holistic development and foster a positive, inclusive school culture.`
      },
      objektif: {
        zh: `• 提高学生对活动主题及相关知识的认识。\n• 培养学生的沟通、合作与参与能力。\n• 营造积极、共融及富有意义的校园学习氛围。`,
        ms: `• Meningkatkan pengetahuan murid tentang tema dan kandungan program.\n• Memupuk kemahiran komunikasi, kerjasama dan penglibatan aktif.\n• Mewujudkan suasana pembelajaran sekolah yang positif dan inklusif.`,
        en: `• To increase pupils' understanding of the programme theme.\n• To develop communication, teamwork and active participation.\n• To create a positive and inclusive school learning environment.`
      },
      impak: {
        zh: `• 学生有机会积极参与活动并拓展相关知识。\n• 学生的沟通、合作及自信心得到培养。\n• 校园文化更加多元、积极与和谐。`,
        ms: `• Murid berpeluang melibatkan diri secara aktif dan menambah pengetahuan.\n• Kemahiran komunikasi, kerjasama dan keyakinan diri murid dapat dipupuk.\n• Budaya sekolah menjadi lebih positif, harmoni dan inklusif.`,
        en: `• Pupils actively participated and broadened their knowledge.\n• Pupils practised communication, teamwork and confidence.\n• A more positive, harmonious and inclusive school culture was fostered.`
      },
      teacher: { zh: "", ms: "", en: "" },
      position: { zh: "", ms: "", en: "" }
    };
  });

  let photoData = { logo: null, 1: null, 2: null, 3: null, 4: null };
  let captions = { 1: "", 2: "", 3: "", 4: "" };

  function switchMobileTab(tab) {
    const sidebar = document.getElementById('sidebarArea');
    const preview = document.getElementById('previewArea');
    const tabEdit = document.getElementById('tabEdit');
    const tabPreview = document.getElementById('tabPreview');
    if (tab === 'edit') {
      sidebar.style.display = 'flex';
      preview.style.display = 'none';
      tabEdit.classList.add('active');
      tabPreview.classList.remove('active');
    } else {
      sidebar.style.display = 'none';
      preview.style.display = 'flex';
      tabEdit.classList.remove('active');
      tabPreview.classList.add('active');
    }
  }

  // 监听窗口尺寸变化，自动适配桌面端与手机端布局
  window.addEventListener('resize', () => {
    const sidebar = document.getElementById('sidebarArea');
    const preview = document.getElementById('previewArea');
    
    if (window.innerWidth > 900) {
      // 桌面端宽屏：清除行内样式，依靠 CSS 恢复双栏左右并排布局
      sidebar.style.display = '';
      preview.style.display = '';
    } else {
      // 手机端窄屏：根据当前激活的底部/顶部标签，重新强制显示单栏
      const isPreviewActive = document.getElementById('tabPreview').classList.contains('active');
      if (isPreviewActive) {
        sidebar.style.display = 'none';
        preview.style.display = 'flex';
      } else {
        sidebar.style.display = 'flex';
        preview.style.display = 'none';
      }
    }
    adjustA4Scale();
  });

  window.addEventListener('DOMContentLoaded', () => {
    initFontDropdown();
    initPresetDropdown();
    switchLanguage('zh');
    setTimeout(adjustA4Scale, 100);
  });

  // 根据当前屏幕动态缩放 A4 预览页面 (仅限桌面端，修复笔电屏幕下部被裁切的问题)
  function adjustA4Scale() {
    if (window.innerWidth <= 900) {
       // 手机端尺寸依靠 CSS 自带的 scale，这里重置 JS 行内样式
       const a4 = document.querySelector('.a4-page');
       if(a4) {
           a4.style.transform = '';
           a4.style.marginBottom = '';
       }
       return;
    }
    
    const previewArea = document.getElementById('previewArea');
    const a4Page = document.querySelector('.a4-page');
    
    if (!previewArea || !a4Page) return;

    const a4Width = a4Page.offsetWidth; 
    const a4Height = a4Page.offsetHeight;

    // 预留预览区四周的 Padding (60px = 上下左右各 30px)
    const paddingX = 60;
    const paddingY = 60;

    const availableWidth = previewArea.clientWidth - paddingX;
    const availableHeight = previewArea.clientHeight - paddingY;

    // 计算横向和纵向的缩放比率
    const scaleX = availableWidth / a4Width;
    const scaleY = availableHeight / a4Height;
    
    // 取最小值以确保整个页面完整可见
    let scale = Math.min(scaleX, scaleY);
    
    // 限制最大缩放倍数为 1（不要放大得比原纸张更大）
    if (scale > 1) scale = 1;

    a4Page.style.transform = `scale(${scale})`;
    
    // 修正因缩放而产生的底部巨大空白
    const heightDiff = a4Height * (1 - scale);
    a4Page.style.marginBottom = `-${heightDiff}px`;
  }

  function initFontDropdown() {
    const fontSelect = document.getElementById('fontSelect');
    fontSelect.innerHTML = '';
    fontOptions.forEach(f => {
      const opt = document.createElement('option');
      opt.value = f.value;
      opt.textContent = f.name;
      fontSelect.appendChild(opt);
    });
  }

  function initPresetDropdown() {
    const programSelect = document.getElementById('programSelect');
    const dict = i18n[currentLang] || i18n['zh'];
    
    const categories = {
      festivals: {
        labels: { zh: "🎉 传统节日与文化庆典", ms: "🎉 SAMBUTAN PERAYAAN & KEBUDAYAAN", en: "🎉 FESTIVALS & CULTURAL CELEBRATIONS" },
        keys: [
          'hui_chun',
          'tahun_baru_cina',
          'yuan_xiao',
          'duan_wu',
          'zhong_qiu',
          'dong_zhi',
          'hari_raya',
          'deepavali',
          'pesta_kaamatan_gawai',
          'yi_mai_hui',
          'zhong_qiu_yuan_you_hui',
          'er_tong_jie'
        ]
      },
      sports: {
        labels: { zh: "🏆 体育与竞赛", ms: "🏆 SUKAN & KEJOHANAN", en: "🏆 SPORTS & ATHLETICS" },
        keys: [
          'yun_dong_hui', 'lan_qiu', 'yu_mao_qiu', 'ping_pong', 'pai_qiu', 'zu_qiu', 'gao_er_fu', 'ying_shi_lan_qiu', 'merentas_desa', 'lompat_tinggi', 'lompat_jauh', 'ti_cao', 'sai_pao', 'diu_qian_qiu',
          'hari_sukan_negara', 'san_ren_lan_qiu', 'shu_fa', 'ying_bi_zi', 'pertandingan_matematik', 'pertandingan_sains', 'pertandingan_stem', 'mewarna_merdeka', 'kuiz_merdeka',
          'mewarna_antidadah', 'mewarna_dengue', 'mewarna_antibuli', 'kuiz_merdeka_new', 'bertulis_sejarah'
        ]
      },
      speech: {
        labels: { zh: "🗣️ 语言与活动/仪式", ms: "🗣️ BAHASA, PEMBANGUNAN & MAJLIS", en: "🗣️ LANGUAGE, DEVELOPMENT & CEREMONY" },
        keys: [
          'storytelling_bc', 'storytelling_bm', 'storytelling_bi', 'speech_bc', 'speech_bm', 'speech_bi', 'deklamasi_sajak',
          'perarakan_merdeka', 'kem_badan_beruniform', 'kem_motivasi', 'kem_kepimpinan', 'gotong_royong', 'duta_sahsiah', 'majlis_graduasi', 'majlis_anugerah'
        ]
      },
      agama: {
        labels: { zh: "🌙 伊斯兰宗教活动 (Agama Islam)", ms: "🌙 AKTIVITI AGAMA ISLAM", en: "🌙 ISLAMIC RELIGIOUS EVENTS" },
        keys: ['tilawah_quran', 'hafazan_quran', 'nasyid', 'seni_khat']
      },
      pibks: {
        labels: { zh: "🤝 PIBKS 家校、社区与私人界合作活动", ms: "🤝 PROGRAM KERJASAMA PIBKS: SEKOLAH, IBU BAPA, KOMUNITI & SWASTA", en: "🤝 PIBKS: SCHOOL, PARENT, COMMUNITY & PRIVATE-SECTOR PARTNERSHIPS" },
        keys: [
          'pibks_gotong_royong_komuniti',
          'pibks_career_day',
          'pibks_safety_talk',
          'pibks_health_screening',
          'pibks_digital_literacy',
          'pibks_reading_buddy',
          'pibks_environment',
          'pibks_parent_workshop',
          'pibks_learning_support',
          'pibks_sports_coaching',
          'pibks_charity_drive'
        ]
      }
    };

    programSelect.innerHTML = `<option value="">-- ${dict.lblPreset || "选择活动预设"} --</option>`;

    for (let catKey in categories) {
      const category = categories[catKey];
      const group = document.createElement('optgroup');
      group.label = category.labels[currentLang] || category.labels['zh'];

      category.keys.forEach(key => {
        if (presets[key]) {
          const p = presets[key];
          const opt = document.createElement('option');
          opt.value = key;
          opt.textContent = p.title[currentLang] || p.title['zh'];
          group.appendChild(opt);
        }
      });

      programSelect.appendChild(group);
    }
  }

  function updateFont(fontFamily) { 
    document.documentElement.style.setProperty('--report-font', fontFamily); 
  }

  function switchLanguage(lang) {
    currentLang = lang;
    const dict = i18n[lang] || i18n['zh'];
    document.getElementById('ui_appTitle').innerText = dict.appTitle;
    document.getElementById('ui_btnSave').innerText = dict.btnSave;
    document.getElementById('ui_btnLoad').innerText = dict.btnLoad;
    document.getElementById('ui_btnReset').innerText = dict.btnReset;
    document.getElementById('ui_btnPrint').innerText = dict.btnPrint;

    document.getElementById('ui_secFont').innerText = dict.secFont;
    document.getElementById('ui_lblFont').innerText = dict.lblFont;
    document.getElementById('ui_secPreset').innerText = dict.secPreset;
    document.getElementById('ui_lblPreset').innerText = dict.lblPreset;
    document.getElementById('ui_secSchool').innerText = dict.secSchool;
    document.getElementById('ui_lblLogo').innerText = dict.lblLogo;
    document.getElementById('ui_lblSchoolName').innerText = dict.lblSchoolName;

    document.getElementById('ui_secDetails').innerText = dict.secDetails;
    document.getElementById('ui_lblTitle').innerText = dict.lblTitle;
    document.getElementById('ui_lblDate').innerText = dict.lblDate;
    document.getElementById('ui_lblTime').innerText = dict.lblTime;
    document.getElementById('ui_lblInvolvement').innerText = dict.lblInvolvement;

    document.getElementById('ui_secObjImp').innerText = dict.secObjImp;
    document.getElementById('ui_lblPengenalan').innerText = dict.lblPengenalan;
    document.getElementById('ui_lblObj').innerText = dict.lblObj;
    document.getElementById('ui_lblImp').innerText = dict.lblImp;

    document.getElementById('ui_secPhotos').innerText = dict.secPhotos;
    document.querySelectorAll('.ui_lblPhotoWord').forEach(el => el.innerText = dict.lblPhoto);

    for (let i = 1; i <= 4; i++) {
      const inputEl = document.getElementById(`caption${i}`);
      if (inputEl) {
        inputEl.placeholder = `${dict.lblPhoto} ${i} ${dict.captionPlaceholder}`;
      }
    }

    document.getElementById('ui_secTeacher').innerText = dict.secTeacher;
    document.getElementById('ui_lblTeacherName').innerText = dict.lblTeacherName;
    document.getElementById('ui_lblPosition').innerText = dict.lblPosition;

    document.getElementById('ui_headerPengenalan').innerText = dict.headerPengenalan;
    document.getElementById('ui_headerObjektif').innerText = dict.headerObjektif;
    document.getElementById('ui_headerImpak').innerText = dict.headerImpak;
    document.getElementById('ui_headerPelapor').innerText = dict.headerPelapor;

    document.getElementById('ui_tblProgram').innerText = dict.tblProgram;
    document.getElementById('ui_tblDate').innerText = dict.tblDate;
    document.getElementById('ui_tblTime').innerText = dict.tblTime;
    document.getElementById('ui_tblTarget').innerText = dict.tblTarget;

    const selectedPresetKey = document.getElementById('programSelect').value;
    initPresetDropdown();
    if (selectedPresetKey) {
      document.getElementById('programSelect').value = selectedPresetKey;
      autoFillProgram();
    }

    updateView();
  }

  function autoFillProgram() {
    const key = document.getElementById('programSelect').value;
    if (!key || !presets[key]) return;

    const p = presets[key];
    const lang = currentLang;

    document.getElementById('inputSchoolName').value = p.school[lang] || p.school['zh'];
    document.getElementById('inputTitle').value = p.title[lang] || p.title['zh'];
    document.getElementById('inputTarikh').value = p.tarikh;
    document.getElementById('inputMasa').value = p.masa;
    document.getElementById('inputPenglibatan').value = p.penglibatan[lang] || p.penglibatan['zh'];

    document.getElementById('inputPengenalan').value = p.pengenalan[lang] || p.pengenalan['zh'];
    document.getElementById('inputObjektif').value = p.objektif[lang] || p.objektif['zh'];
    document.getElementById('inputImpak').value = p.impak[lang] || p.impak['zh'];

    document.getElementById('inputTeacherName').value = p.teacher[lang] || p.teacher['zh'];
    document.getElementById('inputPosition').value = p.position[lang] || p.position['zh'];

    updateView();
  }

  function updateView() {
    document.getElementById('viewSchoolName').innerText = document.getElementById('inputSchoolName').value || '-';
    document.getElementById('viewTitle').innerText = document.getElementById('inputTitle').value || '-';
    document.getElementById('viewTarikh').innerText = document.getElementById('inputTarikh').value || '-';
    document.getElementById('viewMasa').innerText = document.getElementById('inputMasa').value || '-';
    document.getElementById('viewPenglibatan').innerText = document.getElementById('inputPenglibatan').value || '-';

    document.getElementById('viewPengenalan').innerText = document.getElementById('inputPengenalan').value || '-';
    document.getElementById('viewObjektif').innerText = document.getElementById('inputObjektif').value || '-';
    document.getElementById('viewImpak').innerText = document.getElementById('inputImpak').value || '-';

    document.getElementById('viewTeacherName').innerText = document.getElementById('inputTeacherName').value || '-';
    document.getElementById('viewPosition').innerText = document.getElementById('inputPosition').value || '-';

    for (let i = 1; i <= 4; i++) {
      captions[i] = document.getElementById(`caption${i}`).value || '';
      renderPhotoCell(i);
    }
  }

  function renderPhotoCell(index) {
    const box = document.getElementById(`box${index}`);
    const imgSrc = photoData[index];
    const capText = captions[index];
    const dict = i18n[currentLang] || i18n['zh'];

    if (imgSrc) {
      box.innerHTML = `<img src="${imgSrc}" alt="${dict.photoPlaceholder} ${index}">${capText ? `<div class="photo-caption">${capText}</div>` : ''}`;
    } else {
      box.innerHTML = `<span class="photo-ph">[ ${dict.photoPlaceholder} ${index} ]</span>${capText ? `<div class="photo-caption">${capText}</div>` : ''}`;
    }
  }

  function loadLogo(event) {
    const file = event.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = function(e) {
        photoData.logo = e.target.result;
        document.getElementById('logoBox').innerHTML = `<img src="${e.target.result}" alt="Logo">`;
      };
      reader.readAsDataURL(file);
    }
  }

  function loadPhoto(event, index) {
    const file = event.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = function(e) {
        photoData[index] = e.target.result;
        renderPhotoCell(index);
      };
      reader.readAsDataURL(file);
    }
  }

  function aiAutoSuggest() {
    const rawTitle = document.getElementById('inputTitle').value.trim();
    if (!rawTitle) {
      alert("请先在“活动名称”框中输入活动标题！");
      document.getElementById('inputTitle').focus();
      return;
    }
    for (let key in presets) {
      const p = presets[key];
      const pTitle = (p.title[currentLang] || p.title['zh']).toLowerCase();
      if (pTitle.includes(rawTitle.toLowerCase()) || rawTitle.toLowerCase().includes(pTitle)) {
        document.getElementById('programSelect').value = key;
        autoFillProgram();
        return;
      }
    }
    document.getElementById('inputPengenalan').value = `本校举办${rawTitle}，旨在丰富校园文化生活，提高学生的参与度与团队合作素养。`;
    document.getElementById('inputObjektif').value = `• 提升学生的综合实践能力与个人才艺。\n• 增强团队协助精神与集体荣誉感。\n• 营造积极健康向上之校园文化氛围。`;
    document.getElementById('inputImpak').value = `• 全校师生积极投入，活动反响热烈。\n• 有效拓宽了学生的技能展示渠道。\n• 圆满完成了活动预设的各项指标。`;
    updateView();
  }

  function saveDraft() {
    const dict = i18n[currentLang] || i18n['zh'];
    const draftData = {
      lang: currentLang,
      font: document.getElementById('fontSelect').value,
      presetKey: document.getElementById('programSelect').value,
      schoolName: document.getElementById('inputSchoolName').value,
      title: document.getElementById('inputTitle').value,
      tarikh: document.getElementById('inputTarikh').value,
      masa: document.getElementById('inputMasa').value,
      penglibatan: document.getElementById('inputPenglibatan').value,
      pengenalan: document.getElementById('inputPengenalan').value,
      objektif: document.getElementById('inputObjektif').value,
      impak: document.getElementById('inputImpak').value,
      teacherName: document.getElementById('inputTeacherName').value,
      position: document.getElementById('inputPosition').value,
      photoData: photoData,
      captions: {
        1: document.getElementById('caption1').value || '',
        2: document.getElementById('caption2').value || '',
        3: document.getElementById('caption3').value || '',
        4: document.getElementById('caption4').value || ''
      }
    };

    try {
      localStorage.setItem('opr_webapp_draft', JSON.stringify(draftData));
      alert(dict.msgSaveSuccess);
    } catch (e) {
      alert(dict.msgSaveError);
    }
  }

  function loadDraft() {
    const dict = i18n[currentLang] || i18n['zh'];
    const saved = localStorage.getItem('opr_webapp_draft');
    if (!saved) {
      alert(dict.msgLoadEmpty);
      return;
    }

    const data = JSON.parse(saved);
    if (data.lang) {
      document.getElementById('langSelect').value = data.lang;
      switchLanguage(data.lang);
    }

    if (data.font) {
      document.getElementById('fontSelect').value = data.font;
      updateFont(data.font);
    }

    document.getElementById('programSelect').value = data.presetKey || "";
    document.getElementById('inputSchoolName').value = data.schoolName || "";
    document.getElementById('inputTitle').value = data.title || "";
    document.getElementById('inputTarikh').value = data.tarikh || "";
    document.getElementById('inputMasa').value = data.masa || "";
    document.getElementById('inputPenglibatan').value = data.penglibatan || "";
    document.getElementById('inputPengenalan').value = data.pengenalan || "";
    document.getElementById('inputObjektif').value = data.objektif || "";
    document.getElementById('inputImpak').value = data.impak || "";
    document.getElementById('inputTeacherName').value = data.teacherName || "";
    document.getElementById('inputPosition').value = data.position || "";

    if (data.captions) {
      for (let i = 1; i <= 4; i++) {
        document.getElementById(`caption${i}`).value = data.captions[i] || "";
      }
    }

    if (data.photoData) {
      photoData = data.photoData;
      if (photoData.logo) {
        document.getElementById('logoBox').innerHTML = `<img src="${photoData.logo}" alt="Logo">`;
      }
    }

    updateView();
    alert(dict.msgLoadSuccess);
  }

  function resetForm() {
    const dict = i18n[currentLang] || i18n['zh'];
    if (!confirm(dict.msgConfirmReset)) return;

    document.getElementById('programSelect').value = "";
    document.getElementById('inputSchoolName').value = "SJKC PEI HUA";
    document.getElementById('inputTitle').value = "";
    document.getElementById('inputTarikh').value = "12.02.2026";
    document.getElementById('inputMasa').value = "8:00 AM – 12:00 PM";
    document.getElementById('inputPenglibatan').value = "";
    document.getElementById('inputPengenalan').value = "";
    document.getElementById('inputObjektif').value = "";
    document.getElementById('inputImpak').value = "";
    document.getElementById('inputTeacherName').value = "";
    document.getElementById('inputPosition').value = "";

    photoData = { logo: null, 1: null, 2: null, 3: null, 4: null };
    captions = { 1: "", 2: "", 3: "", 4: "" };
    document.getElementById('logoBox').innerHTML = `<span style="font-size: 7pt; color: #888;">[ 校徽 LOGO ]</span>`;
    for (let i = 1; i <= 4; i++) {
      document.getElementById(`photo${i}`).value = "";
      document.getElementById(`caption${i}`).value = "";
    }
    document.getElementById('logoInput').value = "";

    updateView();
  }
</script>
</body>
</html>
