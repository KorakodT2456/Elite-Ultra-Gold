# Elite-Ultra-Gold
<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>ELITE ULTRA GOLD FOOTBALL | Analytics Dashboard</title>
  <style>
    :root {
      --bg-main: #0B0E14;
      --bg-card: #151A23;
      --bg-input: #1C2331;
      --gold-primary: #D4AF37;
      --gold-hover: #F3E5AB;
      --gold-accent: #FFD700;
      --text-main: #E2E8F0;
      --text-muted: #94A3B8;
      --border-color: #2A3447;
      --green-accent: #10B981;
      --red-accent: #EF4444;
      --font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; font-family: var(--font-family); }
    body { background-color: var(--bg-main); color: var(--text-main); padding-bottom: 70px; -webkit-tap-highlight-color: transparent; }

    /* Top Navigation Header */
    header {
      background-color: var(--bg-card);
      border-bottom: 1px solid var(--border-color);
      padding: 12px 16px;
      position: sticky; top: 0; z-index: 1000;
      display: flex; justify-content: space-between; align-items: center;
    }
    .brand-title { color: var(--gold-primary); font-size: 1.05rem; font-weight: 800; letter-spacing: 0.5px; display: flex; align-items: center; gap: 6px; }
    .lang-switcher { display: flex; gap: 4px; background: var(--bg-input); padding: 2px; border-radius: 6px; border: 1px solid var(--border-color); }
    .lang-btn { background: none; border: none; color: var(--text-muted); padding: 4px 8px; font-size: 0.75rem; font-weight: 600; border-radius: 4px; cursor: pointer; }
    .lang-btn.active { background: var(--gold-primary); color: #000; }

    /* Navigation Bar (Mobile Tab Bar) */
    nav {
      position: fixed; bottom: 0; left: 0; right: 0;
      background: var(--bg-card); border-top: 1px solid var(--border-color);
      display: flex; overflow-x: auto; white-space: nowrap; z-index: 1000;
      -webkit-overflow-scrolling: touch;
    }
    nav::-webkit-scrollbar { display: none; }
    .nav-item {
      flex: 1; min-width: 72px; padding: 10px 4px; text-align: center;
      color: var(--text-muted); font-size: 0.68rem; text-decoration: none;
      display: flex; flex-direction: column; align-items: center; gap: 4px; cursor: pointer;
    }
    .nav-item.active { color: var(--gold-primary); font-weight: bold; border-top: 2px solid var(--gold-primary); }

    /* Container & Layout */
    .container { padding: 12px; max-width: 800px; margin: 0 auto; }
    .tab-content { display: none; }
    .tab-content.active { display: block; }

    /* Card Component */
    .card { background: var(--bg-card); border: 1px solid var(--border-color); border-radius: 10px; padding: 14px; margin-bottom: 12px; }
    .card-title { font-size: 0.9rem; font-weight: 700; color: var(--gold-primary); margin-bottom: 10px; display: flex; justify-content: space-between; align-items: center; }

    /* Badge & Tags */
    .badge { display: inline-block; padding: 2px 6px; border-radius: 4px; font-size: 0.65rem; font-weight: 700; text-transform: uppercase; }
    .badge-demo { background: rgba(239, 68, 68, 0.2); color: #FCA5A5; border: 1px solid var(--red-accent); }
    .badge-unverified { background: rgba(234, 179, 8, 0.2); color: #FDE047; border: 1px solid #EAB308; }
    .badge-gold { background: rgba(212, 175, 55, 0.2); color: var(--gold-primary); border: 1px solid var(--gold-primary); }

    /* Tables (Horizontal Scrollable) */
    .table-wrapper { overflow-x: auto; -webkit-overflow-scrolling: touch; margin-top: 8px; border-radius: 6px; border: 1px solid var(--border-color); }
    table { width: 100%; border-collapse: collapse; min-width: 500px; font-size: 0.78rem; text-align: left; }
    th { background: var(--bg-input); color: var(--gold-primary); padding: 8px 10px; font-weight: 600; border-bottom: 1px solid var(--border-color); white-space: nowrap; }
    td { padding: 8px 10px; border-bottom: 1px solid var(--border-color); white-space: nowrap; }
    tr:last-child td { border-bottom: none; }

    /* Form & Input Elements */
    .form-group { margin-bottom: 10px; }
    label { display: block; font-size: 0.75rem; color: var(--text-muted); margin-bottom: 4px; }
    input, select {
      width: 100%; padding: 8px 10px; background: var(--bg-input);
      border: 1px solid var(--border-color); border-radius: 6px;
      color: var(--text-main); font-size: 0.85rem; outline: none;
    }
    input:focus, select:focus { border-color: var(--gold-primary); }
    .btn {
      width: 100%; padding: 10px; background: var(--gold-primary); color: #000;
      font-weight: 700; border: none; border-radius: 6px; font-size: 0.85rem;
      cursor: pointer; transition: 0.2s;
    }
    .btn:active { background: var(--gold-hover); transform: scale(0.99); }

    /* Alert / Notice */
    .alert { padding: 10px; border-radius: 6px; font-size: 0.75rem; margin-bottom: 10px; background: rgba(239, 68, 68, 0.1); border: 1px solid var(--red-accent); color: #FCA5A5; display: none; }

    /* Visual Graph (SVG Container) */
    .graph-container { width: 100%; height: 120px; margin-top: 10px; }
    svg { width: 100%; height: 100%; overflow: visible; }

    /* Grid Layouts */
    .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
    .metric-box { background: var(--bg-input); padding: 8px; border-radius: 6px; text-align: center; border: 1px solid var(--border-color); }
    .metric-val { font-size: 1.1rem; font-weight: bold; color: var(--gold-primary); }
    .metric-lbl { font-size: 0.65rem; color: var(--text-muted); margin-top: 2px; }
  </style>
</head>
<body>

  <!-- Top Header -->
  <header>
    <div class="brand-title">
      <span>⚽</span> ELITE ULTRA GOLD
    </div>
    <div class="lang-switcher">
      <button class="lang-btn active" onclick="setLang('th')">TH</button>
      <button class="lang-btn" onclick="setLang('en')">EN</button>
    </div>
  </header>

  <div class="container">

    <!-- Alert Box -->
    <div id="alertBox" class="alert"></div>

    <!-- TAB 1: DASHBOARD -->
    <div id="tab-dashboard" class="tab-content active">
      <div class="card">
        <div class="card-title">
          <span data-th="แมตช์วิเคราะห์ล่าสุด" data-en="Featured Match">แมตช์วิเคราะห์ล่าสุด</span>
          <span class="badge badge-demo">DEMO DATA</span>
        </div>
        <div style="font-size:0.95rem; font-weight:bold; margin-bottom:4px;">Feyenoord vs Como</div>
        <div style="font-size:0.75rem; color:var(--text-muted);">
          <span data-th="ลีก" data-en="League">ลีก</span>: Club Friendly | 14/10/2026 - 19:25 (TH)
        </div>
        <div style="font-size:0.7rem; color:var(--text-muted); margin-top:2px;">
          <span data-th="อัปเดตล่าสุด" data-en="Last Updated">อัปเดตล่าสุด</span>: 14/10/2026 19:25 | Source: Public Market Proxy
        </div>
      </div>

      <div class="grid-2" style="margin-bottom: 12px;">
        <div class="metric-box">
          <div class="metric-val">3.00</div>
          <div class="metric-lbl" data-th="ค่า Total Expected Goals (λT)" data-en="Total Expected Goals (λT)">ค่า Total Expected Goals (λT)</div>
        </div>
        <div class="metric-box">
          <div class="metric-val" id="poissonProbDisplay">57.68%</div>
          <div class="metric-lbl" data-th="Model Prob (Poisson O2.5)" data-en="Model Prob (Poisson O2.5)">Model Prob (Poisson O2.5)</div>
        </div>
      </div>

      <div class="card">
        <div class="card-title" data-th="พารามิเตอร์เชิงลึก (xG & Shots)" data-en="Advanced Football Metrics">พารามิเตอร์เชิงลึก (xG & Shots)</div>
        <div class="grid-2">
          <div class="metric-box"><div class="metric-val">1.85</div><div class="metric-lbl">Home xG ($\lambda_H$)</div></div>
          <div class="metric-box"><div class="metric-val">1.15</div><div class="metric-lbl">Away xG ($\lambda_A$)</div></div>
          <div class="metric-box"><div class="metric-val">2.10</div><div class="metric-lbl">xGOT Combined</div></div>
          <div class="metric-box"><div class="metric-val">5</div><div class="metric-lbl">Big Chances Total</div></div>
        </div>
      </div>
    </div>

    <!-- TAB 2: O/U 2.5 -->
    <div id="tab-ou" class="tab-content">
      <div class="card">
        <div class="card-title">
          <span data-th="การเปรียบเทียบ Model vs Market (O/U)" data-en="Model vs Market (O/U)">การเปรียบเทียบ Model vs Market (O/U)</span>
          <span class="badge badge-unverified">UNVERIFIED REALTIME</span>
        </div>
        <div class="table-wrapper">
          <table>
            <thead>
              <tr>
                <th data-th="ประเภท" data-en="Type">ประเภท</th>
                <th data-th="ราคา Over" data-en="Over Line">ราคา Over</th>
                <th data-th="ความน่าจะเป็น" data-en="Implied Prob">ความน่าจะเป็น</th>
                <th data-th="Fair Price" data-en="Fair Price">Fair Price</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td><strong>Model Probability</strong></td>
                <td>Over 2.5</td>
                <td id="modelProbTable">57.68%</td>
                <td id="modelFairPrice">1.73</td>
              </tr>
              <tr>
                <td><strong>Market Implied</strong></td>
                <td>Over 3.0 (@1.80)</td>
                <td>55.55%</td>
                <td>1.80</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>

      <!-- Live Calculation Input Engine -->
      <div class="card">
        <div class="card-title" data-th="เครื่องมือคำนวณ Poisson O2.5" data-en="Poisson Calculator Engine">เครื่องมือคำนวณ Poisson O2.5</div>
        <div class="form-group">
          <label data-th="คาดการณ์ประตูทีมเหย้า (Home xG - λH)" data-en="Home Expected Goals (λH)">คาดการณ์ประตูทีมเหย้า (Home xG - λH)</label>
          <input type="number" id="inputXgH" step="0.01" value="1.85">
        </div>
        <div class="form-group">
          <label data-th="คาดการณ์ประตูทีมเยือน (Away xG - λA)" data-en="Away Expected Goals (λA)">คาดการณ์ประตูทีมเยือน (Away xG - λA)</label>
          <input type="number" id="inputXgA" step="0.01" value="1.15">
        </div>
        <button class="btn" onclick="calculateCustomPoisson()" data-th="คำนวณราคา Fair Price & Prob" data-en="Calculate Fair Price & Prob">คำนวณราคา Fair Price & Prob</button>
        <div id="customResult" style="margin-top:10px; font-size:0.8rem; line-height:1.4;"></div>
      </div>
    </div>

    <!-- TAB 3: ASIAN HANDICAP -->
    <div id="tab-ah" class="tab-content">
      <div class="card">
        <div class="card-title">
          <span data-th="โครงสร้างราคา Asian Handicap" data-en="Asian Handicap Structure">โครงสร้างราคา Asian Handicap</span>
          <span class="badge badge-demo">DEMO</span>
        </div>
        <div class="table-wrapper">
          <table>
            <thead>
              <tr>
                <th data-th="เวลา" data-en="Time">เวลา</th>
                <th data-th="แฮนดิแคป" data-en="Handicap">แฮนดิแคป</th>
                <th data-th="ทีมเหย้า" data-en="Home">ทีมเหย้า</th>
                <th data-th="ทีมเยือน" data-en="Away">ทีมเยือน</th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td>Opening</td>
                <td>-1.75</td>
                <td>2.05</td>
                <td>1.80</td>
              </tr>
              <tr>
                <td>Current</td>
                <td>-1.25</td>
                <td>1.80</td>
                <td>2.05</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div style="font-size:0.72rem; color:var(--text-muted); margin-top:8px;" data-th="การไหลของราคา AH สอดคล้องกับการปรับฐาน O/U จาก 3.5 ลงมา 3.0" data-en="AH movement aligns with O/U line adjustment from 3.5 to 3.0.">
          การไหลของราคา AH สอดคล้องกับการปรับฐาน O/U จาก 3.5 ลงมา 3.0
        </div>
      </div>
    </div>

    <!-- TAB 4: BROKER MOMENTUM -->
    <div id="tab-broker" class="tab-content">
      <div class="card">
        <div class="card-title">
          <span data-th="ทิศทางราคา Broker (Tier S / A)" data-en="Broker Price Momentum">ทิศทางราคา Broker (Tier S / A)</span>
          <span class="badge badge-unverified">PROXY DATA</span>
        </div>
        <div class="table-wrapper">
          <table>
            <thead>
              <tr>
                <th>Broker</th>
                <th>O/U Line</th>
                <th>Over Odds</th>
                <th>Direction</th>
              </tr>
            </thead>
            <tbody>
              <tr><td>Pinnacle</td><td>3.0</td><td>1.80</td><td style="color:var(--red-accent)">Line Drop (-0.5)</td></tr>
              <tr><td>SBOBET</td><td>3.0</td><td>1.82</td><td style="color:var(--red-accent)">Line Drop (-0.5)</td></tr>
              <tr><td>Bet365</td><td>3.0</td><td>1.80</td><td style="color:var(--green-accent)">Price Compress</td></tr>
              <tr><td>Macauslot</td><td>3.0</td><td>1.78</td><td style="color:var(--green-accent)">Price Compress</td></tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- TAB 5: BETFAIR EXCHANGE -->
    <div id="tab-betfair" class="tab-content">
      <div class="card">
        <div class="card-title">
          <span data-th="กราฟ Betfair Exchange Trend" data-en="Betfair Odds Trend Graph">กราฟ Betfair Exchange Trend</span>
          <span class="badge badge-demo">DEMO GRAPH</span>
        </div>
        <div class="graph-container">
          <svg viewBox="0 0 300 100" preserveAspectRatio="none">
            <!-- Grid lines -->
            <line x1="0" y1="20" x2="300" y2="20" stroke="#2A3447" stroke-dasharray="2"/>
            <line x1="0" y1="50" x2="300" y2="50" stroke="#2A3447" stroke-dasharray="2"/>
            <line x1="0" y1="80" x2="300" y2="80" stroke="#2A3447" stroke-dasharray="2"/>
            <!-- Odds Line 2.05 down to 1.80 -->
            <polyline fill="none" stroke="#D4AF37" stroke-width="3" points="0,20 100,25 200,60 300,75"/>
            <!-- Data Points -->
            <circle cx="0" cy="20" r="4" fill="#D4AF37"/>
            <circle cx="100" cy="25" r="4" fill="#D4AF37"/>
            <circle cx="200" cy="60" r="4" fill="#D4AF37"/>
            <circle cx="300" cy="75" r="4" fill="#D4AF37"/>
          </svg>
        </div>
        <div style="display:flex; justify-content:space-between; font-size:0.68rem; color:var(--text-muted); margin-top:4px;">
          <span>T-180 (2.05)</span>
          <span>T-120 (2.00)</span>
          <span>T-60 (1.88)</span>
          <span>Current (1.80)</span>
        </div>
        <div style="font-size:0.72rem; color:var(--text-muted); margin-top:8px;">
          <span data-th="ปริมาณซื้อขายจับคู่ (Matched Volume)" data-en="Matched Volume">ปริมาณซื้อขายจับคู่ (Matched Volume)</span>: <em>N/A (ต้องเชื่อมต่อ Betfair API)</em>
        </div>
      </div>
    </div>

    <!-- TAB 6: LIVE HT -->
    <div id="tab-live" class="tab-content">
      <div class="card">
        <div class="card-title">
          <span data-th="การวิเคราะห์สดครึ่งหลัง (Live Halftime)" data-en="Live Halftime Analytics">การวิเคราะห์สดครึ่งหลัง (Live Halftime)</span>
          <span class="badge badge-unverified">NOT CONNECTED</span>
        </div>
        <p style="font-size:0.8rem; color:var(--text-muted); line-height:1.4;">
          <span data-th="ระบบยึดหลักความน่าเชื่อถือ: ไม่มี API ดึงข้อมูลสดในขณะนี้ กรุณากรอกสถิติครึ่งแรกด้วยตนเองเพื่อประเมินความน่าจะเป็นครึ่งหลัง" data-en="Data Integrity Rule: Live API is not connected. Please enter HT stats manually.">
            ระบบยึดหลักความน่าเชื่อถือ: ไม่มี API ดึงข้อมูลสดในขณะนี้ กรุณากรอกสถิติครึ่งแรกด้วยตนเองเพื่อประเมินความน่าจะเป็นครึ่งหลัง
          </span>
        </p>
      </div>
    </div>

    <!-- TAB 7: RESULTS -->
    <div id="tab-results" class="tab-content">
      <div class="card">
        <div class="card-title" data-th="บันทึกผลการทำนายและผลจริง" data-en="Prediction & Outcome Journal">บันทึกผลการทำนายและผลจริง</div>
        <div class="form-group">
          <label data-th="คู่แข่งขัน" data-en="Match Title">คู่แข่งขัน</label>
          <input type="text" id="recMatch" placeholder="e.g. Feyenoord vs Como">
        </div>
        <div class="grid-2">
          <div class="form-group">
            <label data-th="Model Predict" data-en="Model Predict">Model Predict</label>
            <input type="text" id="recPred" placeholder="e.g. Over 2.5">
          </div>
          <div class="form-group">
            <label data-th="ผลจริง (Score)" data-en="Actual Score">ผลจริง (Score)</label>
            <input type="text" id="recActual" placeholder="e.g. 2-1">
          </div>
        </div>
        <button class="btn" onclick="addRecord()" data-th="บันทึกผล" data-en="Save Record">บันทึกผล</button>

        <div class="table-wrapper" style="margin-top:12px;">
          <table id="recordsTable">
            <thead>
              <tr>
                <th data-th="แมตช์" data-en="Match">แมตช์</th>
                <th data-th="ทำนาย" data-en="Pred">ทำนาย</th>
                <th data-th="ผลจริง" data-en="Score">ผลจริง</th>
                <th data-th="สถานะ" data-en="Status">สถานะ</th>
              </tr>
            </thead>
            <tbody>
              <!-- Dynamic Rows -->
            </tbody>
          </table>
        </div>
      </div>
    </div>

  </div>

  <!-- Bottom Nav Tabs -->
  <nav>
    <div class="nav-item active" onclick="switchTab('dashboard', this)">
      <span>📊</span><span data-th="แดชบอร์ด" data-en="Dashboard">แดชบอร์ด</span>
    </div>
    <div class="nav-item" onclick="switchTab('ou', this)">
      <span>🎯</span><span>O/U 2.5</span>
    </div>
    <div class="nav-item" onclick="switchTab('ah', this)">
      <span>⚖️</span><span>AH</span>
    </div>
    <div class="nav-item" onclick="switchTab('broker', this)">
      <span>🏢</span><span>Broker</span>
    </div>
    <div class="nav-item" onclick="switchTab('betfair', this)">
      <span>📈</span><span>Betfair</span>
    </div>
    <div class="nav-item" onclick="switchTab('live', this)">
      <span>⏱️</span><span>Live HT</span>
    </div>
    <div class="nav-item" onclick="switchTab('results', this)">
      <span>📝</span><span data-th="บันทึกผล" data-en="Results">บันทึกผล</span>
    </div>
  </nav>

  <script>
    // System Global Variables
    let currentLang = 'th';
    let records = [];

    // Mathematical Functions
    // Factorial Function
    function factorial(n) {
      if (n === 0 || n === 1) return 1;
      let res = 1;
      for (let i = 2; i <= n; i++) res *= i;
      return res;
    }

    // Poisson Probability Density Function P(k; lambda)
    function poissonPDF(k, lambda) {
      return (Math.pow(lambda, k) * Math.exp(-lambda)) / factorial(k);
    }

    // Calculate Over 2.5 Probability (1 - P(0) - P(1) - P(2))
    function calcPoissonOver25(lambdaTotal) {
      if (isNaN(lambdaTotal) || lambdaTotal <= 0) return 0;
      let p0 = poissonPDF(0, lambdaTotal);
      let p1 = poissonPDF(1, lambdaTotal);
      let p2 = poissonPDF(2, lambdaTotal);
      let pUnder25 = p0 + p1 + p2;
      let pOver25 = 1 - pUnder25;
      return pOver25 > 0 ? pOver25 : 0;
    }

    // Tab Navigation Logic
    function switchTab(tabId, el) {
      document.querySelectorAll('.tab-content').forEach(tab => tab.classList.remove('active'));
      document.querySelectorAll('.nav-item').forEach(nav => nav.classList.remove('active'));
      
      document.getElementById('tab-' + tabId).classList.add('active');
      el.classList.add('active');
      hideAlert();
    }

    // Alert Handling
    function showAlert(msg) {
      const box = document.getElementById('alertBox');
      box.innerText = msg;
      box.style.display = 'block';
    }

    function hideAlert() {
      const box = document.getElementById('alertBox');
      box.style.display = 'none';
    }

    // Language Toggle Engine
    function setLang(lang) {
      currentLang = lang;
      document.querySelectorAll('.lang-btn').forEach(btn => btn.classList.remove('active'));
      event.target.classList.add('active');

      document.querySelectorAll('[data-th]').forEach(el => {
        if (el.dataset[lang]) {
          el.innerText = el.dataset[lang];
        }
      });
    }

    // Custom Poisson Calculator Engine
    function calculateCustomPoisson() {
      hideAlert();
      const xgH = parseFloat(document.getElementById('inputXgH').value);
      const xgA = parseFloat(document.getElementById('inputXgA').value);

      if (isNaN(xgH) || isNaN(xgA) || xgH < 0 || xgA < 0) {
        showAlert(currentLang === 'th' ? 'กรุณากรอกค่า xG เป็นตัวเลขที่ถูกต้อง (มากกว่าหรือเท่ากับ 0)' : 'Please enter valid xG values (>= 0).');
        return;
      }

      const lambdaTotal = xgH + xgA;
      const probOver25 = calcPoissonOver25(lambdaTotal);
      const probPercent = (probOver25 * 100).toFixed(2);
      const fairPrice = probOver25 > 0 ? (1 / probOver25).toFixed(2) : 'N/A';

      const resDiv = document.getElementById('customResult');
      resDiv.innerHTML = `
        <div style="background:var(--bg-main); padding:8px; border-radius:6px; border:1px solid var(--border-color); margin-top:6px;">
          <strong>Total Expected Goals (λT):</strong> ${lambdaTotal.toFixed(2)}<br>
          <strong>Model Over 2.5 Prob:</strong> <span style="color:var(--gold-primary); font-weight:bold;">${probPercent}%</span><br>
          <strong>Fair Price (Odds):</strong> <span style="color:var(--green-accent); font-weight:bold;">${fairPrice}</span>
        </div>
      `;
    }

    // Journal / Results Engine
    function addRecord() {
      hideAlert();
      const match = document.getElementById('recMatch').value.trim();
      const pred = document.getElementById('recPred').value.trim();
      const actual = document.getElementById('recActual').value.trim();

      if (!match || !pred || !actual) {
        showAlert(currentLang === 'th' ? 'กรุณากรอกข้อมูลบันทึกผลให้ครบทุกช่อง' : 'Please fill in all record fields.');
        return;
      }

      // Determine Status
      const scores = actual.split('-').map(s => parseInt(s.trim()));
      let status = 'PENDING';
      if (scores.length === 2 && !isNaN(scores[0]) && !isNaN(scores[1])) {
        const totalGoals = scores[0] + scores[1];
        if (pred.toLowerCase().includes('over 2.5')) {
          status = totalGoals > 2.5 ? 'WIN' : 'LOSS';
        } else {
          status = 'SETTLED';
        }
      }

      records.push({ match, pred, actual, status });
      renderRecords();

      // Clear Inputs
      document.getElementById('recMatch').value = '';
      document.getElementById('recPred').value = '';
      document.getElementById('recActual').value = '';
    }

    function renderRecords() {
      const tbody = document.querySelector('#recordsTable tbody');
      tbody.innerHTML = '';
      records.forEach(r => {
        const tr = document.createElement('tr');
        let statusColor = 'var(--text-muted)';
        if (r.status === 'WIN') statusColor = 'var(--green-accent)';
        if (r.status === 'LOSS') statusColor = 'var(--red-accent)';

        tr.innerHTML = `
          <td>${r.match}</td>
          <td>${r.pred}</td>
          <td>${r.actual}</td>
          <td style="color:${statusColor}; font-weight:bold;">${r.status}</td>
        `;
        tbody.appendChild(tr);
      });
    }

    // Initial Default Poisson Sync on Load
    window.onload = function() {
      const defaultLambda = 3.00; // 1.85 + 1.15
      const prob = calcPoissonOver25(defaultLambda);
      const probPercent = (prob * 100).toFixed(2) + '%';
      const fair = (1 / prob).toFixed(2);

      document.getElementById('poissonProbDisplay').innerText = probPercent;
      document.getElementById('modelProbTable').innerText = probPercent;
      document.getElementById('modelFairPrice').innerText = fair;
    };
  </script>
</body>
</html>
