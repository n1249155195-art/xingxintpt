<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>兴溪茶叶能碳管理平台</title>
    <script src="https://cdn.jsdelivr.net/npm/echarts@5.5.0/dist/echarts.min.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', 'Segoe UI', 'Roboto', 'Microsoft YaHei', sans-serif;
        }
        body {
            background: #f0f4f0;
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }
        /* 登录面板 */
        .login-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.75);
            backdrop-filter: blur(8px);
            z-index: 1000;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .login-card {
            background: white;
            border-radius: 40px;
            padding: 40px 36px;
            width: 420px;
            box-shadow: 0 20px 35px rgba(0,0,0,0.2);
            text-align: center;
            border: 1px solid #cfe3cf;
        }
        .login-card h2 {
            color: #2c6e3c;
            font-size: 28px;
            margin-bottom: 8px;
        }
        .login-card p { color: #5a7a5a; margin-bottom: 28px; }
        .input-group { margin-bottom: 24px; text-align: left; }
        .input-group label { display: block; color: #2a4b2a; margin-bottom: 8px; font-weight: 500; }
        .input-group input {
            width: 100%;
            background: #f9fff9;
            border: 1px solid #bdd8bd;
            border-radius: 32px;
            padding: 12px 20px;
            font-size: 16px;
            outline: none;
        }
        .login-btn {
            width: 100%;
            background: #2a7a3e;
            border: none;
            padding: 12px;
            border-radius: 40px;
            font-weight: bold;
            font-size: 16px;
            color: white;
            cursor: pointer;
            margin-top: 8px;
        }
        /* 主界面 */
        .main-app {
            display: none;
            width: 100%;
            max-width: 1400px;
            background: white;
            border-radius: 40px;
            padding: 24px 28px;
            box-shadow: 0 12px 30px rgba(0,0,0,0.08);
            border: 1px solid #dcecdc;
        }
        .nav-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            margin-bottom: 24px;
            border-bottom: 2px solid #e2f0e2;
            padding-bottom: 16px;
        }
        .logo-area h1 { color: #1e562e; font-size: 1.8rem; }
        .user-actions { display: flex; gap: 16px; align-items: center; }
        .live-time {
            background: #e8f3e5;
            padding: 6px 16px;
            border-radius: 40px;
            font-size: 14px;
            font-weight: 500;
            color: #1e562e;
            letter-spacing: 0.5px;
        }
        .btn-outline {
            background: transparent;
            border: 1px solid #479f5e;
            padding: 6px 18px;
            border-radius: 40px;
            color: #2a6e3a;
            cursor: pointer;
            transition: 0.2s;
        }
        .btn-outline:hover {
            background: #eef6ea;
        }
        .menu-tabs {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 28px;
            background: #f4faf4;
            padding: 12px 20px;
            border-radius: 60px;
        }
        .menu-item {
            padding: 8px 20px;
            border-radius: 40px;
            background: transparent;
            color: #2a5e3a;
            cursor: pointer;
            font-weight: 500;
            transition: 0.2s;
        }
        .menu-item.active {
            background: #2a7a3e;
            color: white;
            box-shadow: 0 2px 8px #bcddbb;
        }
        .dashboard-card {
            background: #ffffff;
            border-radius: 32px;
            padding: 24px;
            margin-bottom: 28px;
            box-shadow: 0 5px 18px rgba(0,0,0,0.05);
            border: 1px solid #e2eedf;
        }
        .stats-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 18px;
            margin-bottom: 28px;
        }
        .stat-item {
            background: #f8fef6;
            border-radius: 28px;
            padding: 16px 24px;
            flex: 1;
            min-width: 150px;
            text-align: center;
            border-left: 5px solid #3c9e5c;
        }
        .stat-label { font-size: 14px; color: #487a4e; }
        .stat-value { font-size: 28px; font-weight: 700; color: #1e562e; }
        .query-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            flex-wrap: wrap;
            gap: 12px;
        }
        select, .year-selector {
            background: #f0f7ef;
            border: 1px solid #bcd9b4;
            border-radius: 32px;
            padding: 8px 20px;
            font-size: 14px;
            color: #1e4a2a;
        }
        .chart-container { height: 320px; width: 100%; margin: 20px 0; }
        .insight-text {
            background: #f1f9ef;
            padding: 16px;
            border-radius: 24px;
            color: #2b5738;
            margin-top: 16px;
        }
        .modal {
            display: none;
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background: rgba(0,0,0,0.7);
            z-index: 2000;
            overflow: auto;
        }
        .modal-content {
            background: white;
            margin: 40px auto;
            width: 90%;
            max-width: 1300px;
            border-radius: 40px;
            padding: 24px;
        }
        .data-table { width: 100%; border-collapse: collapse; }
        .data-table th, .data-table td {
            border: 1px solid #caddca;
            padding: 8px;
            text-align: center;
        }
        .data-table input { width: 100px; padding: 6px; border-radius: 16px; border: 1px solid #b3cfaa; }
        .save-btn { background: #2a7a3e; padding: 10px 28px; border: none; border-radius: 32px; color: white; margin-top: 20px; cursor: pointer; margin-right: 16px; }
        .close-modal { float: right; font-size: 28px; cursor: pointer; }
        .year-tab { display: inline-block; margin-right: 20px; padding: 6px 18px; border-radius: 30px; background: #e2f0de; cursor: pointer; }
        .year-tab.active { background: #2a7a3e; color: white; }
        .report-container {
            background: #fefef7;
            border-radius: 28px;
            padding: 20px;
            line-height: 1.5;
        }
        .report-container h3 {
            color: #1e562e;
            margin-top: 20px;
            margin-bottom: 12px;
            border-left: 5px solid #3c9e5c;
            padding-left: 15px;
        }
        .report-container h4 {
            color: #2a6e3a;
            margin: 12px 0 8px;
        }
        .report-table {
            width: 100%;
            border-collapse: collapse;
            margin: 16px 0;
            background: white;
        }
        .report-table th, .report-table td {
            border: 1px solid #cde2cd;
            padding: 10px;
            text-align: center;
        }
        .report-table th {
            background: #eef5ea;
        }
        .flex-btns {
            display: flex;
            gap: 12px;
            align-items: center;
            flex-wrap: wrap;
        }
        .product-carbon-card {
            background: #f9fff7;
            border-radius: 28px;
            padding: 20px;
            margin-bottom: 24px;
            box-shadow: 0 2px 8px rgba(0,0,0,0.03);
            border: 1px solid #d8e8d4;
        }
        .product-title {
            font-size: 1.5rem;
            font-weight: 700;
            color: #2a6e3a;
            margin-bottom: 12px;
        }
        .carbon-value {
            font-size: 48px;
            font-weight: 800;
            color: #1e562e;
            line-height: 1;
        }
        .carbon-unit {
            font-size: 18px;
            font-weight: normal;
            color: #5a8f5a;
        }
        .stage-badge {
            display: inline-block;
            background: #eef3ea;
            border-radius: 24px;
            padding: 4px 12px;
            font-size: 13px;
            margin-right: 8px;
            margin-bottom: 8px;
        }
    </style>
</head>
<body>
<div id="loginContainer" class="login-container" style="display: flex;">
    <div class="login-card">
        <h2>🍃 兴溪茶叶能碳管理平台</h2>
        <p>智慧能源 | 碳迹可视 | 绿色制造</p>
        <div class="input-group"><label>账号</label><input type="text" id="loginAccount" placeholder="请输入账号"></div>
        <div class="input-group"><label>密码</label><input type="password" id="loginPassword" placeholder="请输入密码"></div>
        <button class="login-btn" id="doLoginBtn">登录系统</button>
    </div>
</div>

<div id="mainApp" class="main-app">
    <div class="nav-header">
        <div class="logo-area"><h1>🍃 兴溪茶叶能碳管理平台</h1></div>
        <div class="user-actions">
            <div class="live-time" id="liveTimeDisplay">--:--:--</div>
            <button id="adminBtn" class="btn-outline">⚙️ 管理后台</button>
            <button id="logoutBtn" class="btn-outline">🚪 退出</button>
        </div>
    </div>
    <div class="menu-tabs" id="menuTabs">
        <div class="menu-item" data-tab="energyQuery">⚡ 能耗查询</div>
        <div class="menu-item" data-tab="energyIntensity">📊 能源消费强度</div>
        <div class="menu-item" data-tab="energyAnalysis">📉 消费分析+策略</div>
        <div class="menu-item" data-tab="benchmark">🎯 能效对标</div>
        <div class="menu-item" data-tab="energyFlow">🌀 能流分析</div>
        <div class="menu-item" data-tab="carbonCheck">🌍 碳排放核查</div>
        <div class="menu-item" data-tab="carbonFootprint">👣 碳足迹核算</div>
        <div class="menu-item" data-tab="carbonSupport">📄 碳排放报告</div>
    </div>
    <div style="display: flex; justify-content: flex-end; margin-bottom: 12px;">
        <label style="margin-right: 8px;">📅 选择年份：</label>
        <select id="yearSelector" class="year-selector"><option value="2025">2025年</option><option value="2026">2026年</option></select>
    </div>
    <div id="tabContent" class="tab-content"></div>
</div>

<div id="adminModal" class="modal">
    <div class="modal-content">
        <span class="close-modal" id="closeModalBtn">×</span>
        <h2 style="color:#1e562e;">📅 双年度数据管理 (2025/2026)</h2>
        <div id="yearAdminTabs" style="margin-bottom: 20px;"><span class="year-tab active" data-year="2025">2025年数据</span><span class="year-tab" data-year="2026">2026年数据</span></div>
        <div style="overflow-x: auto;"><table class="data-table" id="dataEditTable"><thead><tr><th>月份</th><th>用电量(kWh)</th><th>绿电购入(kWh)</th><th>光伏自用(kWh)</th><th>产值(万元)</th><th>综合能耗(tce)</th><th>碳排放(tCO₂)</th></thead><tbody id="editTableBody"></tbody></table></div>
        <div class="flex-btns">
            <button id="saveDataBtn" class="save-btn">💾 保存所有年度数据</button>
            <button id="downloadHtmlAdminBtn" class="save-btn" style="background:#4c8b5c;">📥 下载HTML离线版</button>
        </div>
    </div>
</div>

<script>
    // ---------- 数据结构 ----------
    const months = ["1月","2月","3月","4月","5月","6月","7月","8月","9月","10月","11月","12月"];
    // 2025 原始数据 (用电量,绿电购入,光伏自用,产值)
    const default2025 = {
        electricity: [106850,93200,128700,138500,135600,112300,115800,120500,156200,152800,109600,94600],
        greenPurchase: [18500,16100,22300,24000,23500,19500,20100,20900,27100,26500,19000,16400],
        pvSelf: [12257,8881,17217,19908,6910,11032,22984,25499,20302,17412,13244,12146],
        outputValue: [1780,1550,2430,2650,2580,2050,2120,2210,3100,3020,1980,1580]
    };
    let default2026 = {
        electricity: [101000,88500,122265,131575,128820,106685,110010,114475,148390,145160,104120,89870],
        greenPurchase: [18500,16100,22300,24000,23500,19500,20100,20900,27100,26500,19000,16400],
        pvSelf: [12257,8881,17217,19908,6910,11032,22984,25499,20302,17412,13244,12146],
        outputValue: [1780,1550,2430,2650,2580,2050,2120,2210,3100,3020,1980,1580]
    }; 
    function calcEnergy(elec) { return elec * 1.229 / 10000; }
    function calcCarbon(elec, pv, green) { let net = elec - pv - green; if(net<0) net=0; return net * 0.4211 / 1000; }

    let dataset = {
        2025: [],
        2026: []
    };
    function buildYearData(year, elecArr, greenArr, pvArr, outArr) {
        let arr = [];
        for(let i=0;i<12;i++){
            let energy = calcEnergy(elecArr[i]);
            let carbon = calcCarbon(elecArr[i], pvArr[i], greenArr[i]);
            arr.push({
                month: months[i],
                electricity: elecArr[i],
                greenPurchase: greenArr[i],
                pvSelf: pvArr[i],
                outputValue: outArr[i],
                energyConsumption: parseFloat(energy.toFixed(2)),
                carbonEmission: parseFloat(carbon.toFixed(1))
            });
        }
        return arr;
    }
    dataset[2025] = buildYearData(2025, default2025.electricity, default2025.greenPurchase, default2025.pvSelf, default2025.outputValue);
    dataset[2026] = buildYearData(2026, default2026.electricity, default2026.greenPurchase, default2026.pvSelf, default2026.outputValue);

    function getYearSummary(year) {
        let data = dataset[year];
        let totalElec = data.reduce((s,d)=>s+d.electricity,0);
        let totalEnergy = data.reduce((s,d)=>s+d.energyConsumption,0);
        let totalOutput = data.reduce((s,d)=>s+d.outputValue,0);
        let totalCarbon = data.reduce((s,d)=>s+d.carbonEmission,0);
        let totalGreen = data.reduce((s,d)=>s+d.greenPurchase,0);
        let totalPv = data.reduce((s,d)=>s+d.pvSelf,0);
        return { totalElec, totalEnergy, totalOutput, totalCarbon, totalGreen, totalPv };
    }
    const fixedProductTon = 5571;
    const fixedWaterUse = 1610;

    let currentYear = "2025";
    let currentTab = "energyQuery";

    const STORAGE_KEY = 'xingxi_dual_year_data';
    function saveToLocal() {
        let toStore = {2025: dataset[2025], 2026: dataset[2026]};
        localStorage.setItem(STORAGE_KEY, JSON.stringify(toStore));
    }
    function loadFromLocal() {
        let stored = localStorage.getItem(STORAGE_KEY);
        if(stored) {
            let parsed = JSON.parse(stored);
            if(parsed[2025]) dataset[2025] = parsed[2025];
            if(parsed[2026]) dataset[2026] = parsed[2026];
        }
    }
    loadFromLocal();

    function renderCurrentTab() {
        const container = document.getElementById("tabContent");
        if(currentTab === "energyQuery") renderEnergyQuery(container);
        else if(currentTab === "energyIntensity") renderEnergyIntensity(container);
        else if(currentTab === "energyAnalysis") renderEnergyAnalysis(container);
        else if(currentTab === "benchmark") renderBenchmark(container);
        else if(currentTab === "energyFlow") renderEnergyFlow(container);
        else if(currentTab === "carbonCheck") renderCarbonCheck(container);
        else if(currentTab === "carbonFootprint") renderCarbonFootprint(container);
        else if(currentTab === "carbonSupport") renderCarbonReport(container);
    }

    function buildLineChart(domId, xData, seriesData, yName, unit) {
        let chart = echarts.init(document.getElementById(domId));
        chart.setOption({
            tooltip: { trigger: 'axis' },
            xAxis: { type: 'category', data: xData, axisLabel: { rotate: 30, color: '#2c5e3a' } },
            yAxis: { type: 'value', name: unit, nameTextStyle: { color: '#467a4e' }, axisLabel: { color: '#2a572e' } },
            series: [{ data: seriesData, type: 'line', smooth: true, lineStyle: { width: 3, color: '#3c9e5c' }, areaStyle: { opacity: 0.2, color: '#b5dfb0' }, symbol: 'circle' }],
            backgroundColor: 'transparent'
        });
        return chart;
    }

    // 能耗查询界面
    function renderEnergyQuery(container) {
        let yearData = dataset[currentYear];
        let summary = getYearSummary(currentYear);
        let monthsArr = yearData.map(d=>d.month);
        let elecArr = yearData.map(d=>d.electricity);
        let html = `<div class="dashboard-card"><div class="stats-grid">
            <div class="stat-item"><div class="stat-label">🌾 ${currentYear}总产值</div><div class="stat-value">${summary.totalOutput.toFixed(1)} 万元</div></div>
            <div class="stat-item"><div class="stat-label">🍃 精制茶产量</div><div class="stat-value">${fixedProductTon} 吨</div></div>
            <div class="stat-item"><div class="stat-label">💧 取水量</div><div class="stat-value">${fixedWaterUse} t</div></div>
            <div class="stat-item"><div class="stat-label">⚡ 全年用电量</div><div class="stat-value">${(summary.totalElec/10000).toFixed(2)} 万kWh</div></div>
            <div class="stat-item"><div class="stat-label">⛽ 综合能耗</div><div class="stat-value">${summary.totalEnergy.toFixed(2)} tce</div></div>
        </div>
        <div class="query-bar"><span>📈 月度用电量趋势 (${currentYear})</span><select id="monthQueryElec"><option value="">📅 查询单月数据</option>${monthsArr.map((m,i)=>`<option value="${i}">${m}</option>`).join('')}</select></div>
        <div id="elecChart" class="chart-container"></div>
        <div id="monthResult" class="insight-text">✨ 选择月份查看详细能耗数据</div></div>`;
        container.innerHTML = html;
        buildLineChart('elecChart', monthsArr, elecArr, '用电量', 'kWh');
        let sel = document.getElementById('monthQueryElec');
        sel.addEventListener('change', (e) => {
            let idx = e.target.value;
            if(idx !== "") {
                let d = yearData[parseInt(idx)];
                document.getElementById('monthResult').innerHTML = `📌 ${d.month} 用电量: ${d.electricity.toLocaleString()} kWh | 绿电购入: ${d.greenPurchase} kWh | 光伏自用: ${d.pvSelf} kWh | 产值: ${d.outputValue} 万元`;
            } else document.getElementById('monthResult').innerHTML = `✨ ${currentYear}年总用电量 ${(summary.totalElec/10000).toFixed(2)} 万kWh`;
        });
    }
    function renderEnergyIntensity(container) {
        let yearData = dataset[currentYear];
        let monthsArr = yearData.map(d=>d.month);
        let energyVals = yearData.map(d=>d.energyConsumption);
        container.innerHTML = `<div class="dashboard-card"><div class="query-bar"><span>🔥 综合能耗趋势 (tce) - ${currentYear}</span><select id="monthEnergySel"><option value="">查询月度</option>${monthsArr.map((m,i)=>`<option value="${i}">${m}</option>`).join('')}</select></div>
        <div id="energyChart" class="chart-container"></div><div id="energyResult" class="insight-text"></div></div>`;
        buildLineChart('energyChart', monthsArr, energyVals, '综合能耗', 'tce');
        let sel = document.getElementById('monthEnergySel');
        sel.addEventListener('change',(e)=>{
            let idx=e.target.value; if(idx!=""){let d=yearData[parseInt(idx)]; document.getElementById('energyResult').innerHTML=`${d.month} 综合能耗: ${d.energyConsumption} tce`;}
            else document.getElementById('energyResult').innerHTML=`${currentYear}年度综合能耗总量: ${getYearSummary(currentYear).totalEnergy.toFixed(2)} tce`;
        });
    }
    function renderEnergyAnalysis(container) {
        let yearData = dataset[currentYear];
        let monthsArr = yearData.map(d=>d.month);
        let intensity = yearData.map(d=> (d.energyConsumption*1000/d.outputValue).toFixed(2) );
        container.innerHTML = `<div class="dashboard-card"><div class="query-bar"><span>📉 万元产值综合能耗 (kgce/万元) - ${currentYear}</span><select id="intensitySel"><option value="">月度查询</option>${monthsArr.map((m,i)=>`<option value="${i}">${m}</option>`).join('')}</select></div>
        <div id="intensityChart" class="chart-container"></div><div id="intensityResult" class="insight-text"></div>
        <div class="insight-text"><strong>🍃 茶叶行业用能策略推荐</strong><br>✔ 杀青余热回收，降低滚筒能耗8~12%<br>✔ 揉捻工序集中驱动+变频，减少空载<br>✔ 光伏扩容+绿电交易，可再生电力占比30%+<br>✔ 烘干热泵替代电热，能效提升40%<br>✔ 能源定额考核，年降本5-8%</div></div>`;
        buildLineChart('intensityChart', monthsArr, intensity, '万元产值综合能耗', 'kgce/万元');
        let sel = document.getElementById('intensitySel');
        sel.addEventListener('change',(e)=>{
            if(e.target.value!=""){
                let idx=e.target.value; let d=yearData[idx];
                let val = (d.energyConsumption*1000/d.outputValue).toFixed(2);
                document.getElementById('intensityResult').innerHTML=`${d.month} 万元产值综合能耗: ${val} kgce/万元`;
            } else document.getElementById('intensityResult').innerHTML=`${currentYear}年度均值: ${(getYearSummary(currentYear).totalEnergy*1000/getYearSummary(currentYear).totalOutput).toFixed(2)} kgce/万元`;
        });
    }
    // 能效对标 - 图例放大、字体避免重叠
    function renderBenchmark(container) {
        let summary = getYearSummary(currentYear);
        let unitEnergy = (summary.totalEnergy * 1000 / fixedProductTon).toFixed(2);
        let advancedBench = 30;
        let gap = (unitEnergy - advancedBench).toFixed(2);
        container.innerHTML = `<div class="dashboard-card"><h3>🎯 能效对标 (单位产品综合能耗 - ${currentYear})</h3><div style="display:flex;gap:30px;flex-wrap:wrap;"><div style="background:#f1f9ef;padding:20px;border-radius:32px;flex:1"><div>🍃 兴溪茶业 ${currentYear}</div><div style="font-size:42px;font-weight:bold;">${unitEnergy}</div><div>kgce/t 成品茶</div></div><div style="background:#f1f9ef;padding:20px;border-radius:32px;flex:1"><div>🏆 行业先进水平</div><div style="font-size:42px;">${advancedBench}</div><div>kgce/t</div></div></div>
        <div class="insight-text">📊 差距: ${gap} kgce/t, 通过余热回收、绿电替代及设备升级缩小差距。</div><div id="benchGauge" style="height:280px;"></div></div>`;
        let gauge = echarts.init(document.getElementById("benchGauge"));
        gauge.setOption({ 
            series: [{ 
                type: 'gauge', 
                min:0, 
                max:60, 
                progress:{show:true,width:20}, 
                axisLine:{lineStyle:{width:20,color:[[0.5,'#48b07b'],[0.8,'#e6b422'],[1,'#e86848']]}}, 
                pointer:{show:true,length:'60%', width:12},
                axisTick: { show: true, splitNumber: 5, length: 8, lineStyle: { width: 1 } },
                splitLine: { show: true, length: 15, lineStyle: { width: 2 } },
                axisLabel: { fontSize: 14, distance: 8, color: '#1e562e' },
                title: { show: true, offsetCenter: [0, '20%'], fontSize: 16 },
                detail: { offsetCenter: [0, 25], fontSize: 22, color: '#1e562e' },
                data:[{value:parseFloat(unitEnergy), name:"单位产品能耗"}]
            }] 
        });
    }
    function renderEnergyFlow(container) {
        let summary = getYearSummary(currentYear);
        let totalElec = summary.totalElec / 10000;
        let pvTotal = dataset[currentYear].reduce((s,d)=>s+d.pvSelf,0)/10000;
        let greenTotal = dataset[currentYear].reduce((s,d)=>s+d.greenPurchase,0)/10000;
        let gridPower = totalElec - pvTotal;
        let nodes = [{name:"外购电网"},{name:"光伏发电"},{name:"绿证采购"},{name:"厂内配电"},{name:"杀青工序"},{name:"揉捻烘干"},{name:"辅助系统"},{name:"碳排放抵消"}];
        let links = [
            {source:"外购电网", target:"厂内配电", value:gridPower},
            {source:"光伏发电", target:"厂内配电", value:pvTotal},
            {source:"绿证采购", target:"碳排放抵消", value:greenTotal},
            {source:"厂内配电", target:"杀青工序", value:totalElec*0.45},
            {source:"厂内配电", target:"揉捻烘干", value:totalElec*0.4},
            {source:"厂内配电", target:"辅助系统", value:totalElec*0.15}
        ];
        container.innerHTML = `<div class="dashboard-card"><h3>🌀 ${currentYear} 能源流向图</h3><div id="sankeyChart" style="height:420px;"></div><div class="insight-text">光伏自用占比${((pvTotal/totalElec)*100).toFixed(1)}%，绿证抵消部分碳排放。</div></div>`;
        let chart = echarts.init(document.getElementById("sankeyChart"));
        chart.setOption({ tooltip: { trigger: 'item' }, series: { type: 'sankey', data: nodes, links: links, nodeWidth: 28, lineStyle: { color: 'gradient' } } });
    }
    function renderCarbonCheck(container) {
        let yearData = dataset[currentYear];
        let monthsArr = yearData.map(d=>d.month);
        let carbonData = yearData.map(d=>d.carbonEmission);
        container.innerHTML = `<div class="dashboard-card"><div class="query-bar"><span>🌱 月度碳排放趋势 (tCO₂) - ${currentYear}</span><select id="carbonMonth"><option value="">查询单月</option>${monthsArr.map((m,i)=>`<option value="${i}">${m}</option>`).join('')}</select></div>
        <div id="carbonChart" class="chart-container"></div><div id="carbonResult" class="insight-text"></div></div>`;
        buildLineChart('carbonChart', monthsArr, carbonData, '碳排放量', 'tCO₂');
        let sel = document.getElementById('carbonMonth');
        sel.addEventListener('change',(e)=>{
            if(e.target.value!=""){let d=yearData[parseInt(e.target.value)]; document.getElementById('carbonResult').innerHTML=`${d.month} 碳排放: ${d.carbonEmission} tCO₂`;}
            else document.getElementById('carbonResult').innerHTML=`${currentYear}碳排放总量: ${getYearSummary(currentYear).totalCarbon.toFixed(1)} tCO₂`;
        });
    }
    
    // 新产品碳足迹核算 (基于产品碳足迹自评报告)
    function renderCarbonFootprint(container) {
        // 依据报告数据：清香型铁观音（500g罐装）摇篮到大门 3.98 kgCO₂e/罐；袋泡乌龙茶（2g×25包）0.91 kgCO₂e/盒
        // 同时展示每公斤当量
        const tieguanyin_per_can = 3.98;   // kgCO₂e/500g罐
        const tieguanyin_per_kg = (3.98 / 0.5).toFixed(2); // 7.96
        const teaBag_per_box = 0.91;       // kgCO₂e/盒 (50g)
        const teaBag_per_kg = (0.91 / 0.05).toFixed(2); // 18.2
        
        const reportYear = "2025"; // 碳足迹报告基准年
        
        let html = `
        <div class="dashboard-card">
            <h3>👣 产品碳足迹核算 (基于GB/T 24067 & ISO 14067)</h3>
            <div class="insight-text" style="margin-bottom:20px;">
                📋 核算范围：从摇篮到大门（茶园种植 → 初制/精制加工 → 包装 → 厂内/运输至客户仓库）<br>
                📅 基准年度：${reportYear}年 | 数据来源：《产品碳足迹自评报告》(XX-CFP-2026-001)
            </div>
            <div class="stats-grid">
                <div class="product-carbon-card" style="flex:1;">
                    <div class="product-title">🍃 清香型铁观音 (500g罐装)</div>
                    <div class="carbon-value">${tieguanyin_per_can} <span class="carbon-unit">kgCO₂e/罐</span></div>
                    <div style="margin-top:8px; font-size:14px; color:#467a4e;">折合 ${tieguanyin_per_kg} kgCO₂e/kg 茶叶</div>
                    <div style="margin-top:16px;">
                        <span class="stage-badge">📦 包装材料 71%</span>
                        <span class="stage-badge">🌱 茶园种植 16%</span>
                        <span class="stage-badge">⚡ 初制+精制加工 10%</span>
                        <span class="stage-badge">🚛 运输 2%</span>
                        <span class="stage-badge">♻️ 废弃处置 1%</span>
                    </div>
                    <div class="insight-text" style="margin-top:16px; font-size:13px;">✅ 主要减排措施：马口铁罐薄壁化设计、光伏绿电占比30%、杀青余热回收</div>
                </div>
                <div class="product-carbon-card" style="flex:1;">
                    <div class="product-title">🍃 袋泡乌龙茶 (2g×25包盒装)</div>
                    <div class="carbon-value">${teaBag_per_box} <span class="carbon-unit">kgCO₂e/盒</span></div>
                    <div style="margin-top:8px; font-size:14px; color:#467a4e;">折合 ${teaBag_per_kg} kgCO₂e/kg 茶叶</div>
                    <div style="margin-top:16px;">
                        <span class="stage-badge">🌱 茶园种植 57%</span>
                        <span class="stage-badge">📦 包装材料 38%</span>
                        <span class="stage-badge">⚡ 加工能耗 4%</span>
                        <span class="stage-badge">🚛 运输 1%</span>
                    </div>
                    <div class="insight-text" style="margin-top:16px; font-size:13px;">✅ 主要减排措施：纸盒轻量化、生物降解内袋、优化供应链运输</div>
                </div>
            </div>
            <div class="insight-text">
                <strong>📊 碳足迹对比与行业水平</strong><br>
                • 清香型铁观音：7.96 kgCO₂e/kg 茶叶，优于行业平均（约9.5 kgCO₂e/kg），主要得益于清洁电力及包装轻量化。<br>
                • 袋泡乌龙茶：18.2 kgCO₂e/kg 茶叶（小包装材料占比高），但通过绿电及茶副产物资源化仍有优化空间。<br>
                • 2025年清洁电力（光伏+绿证）占比30.03%，减排190 tCO₂e；茶梗生物质颗粒替代天然气年减排42 tCO₂e。<br>
                🎯 2026年目标：扩大绿电采购至35%，推广全纸化包装，进一步降低碳足迹。
            </div>
        </div>`;
        container.innerHTML = html;
    }

    // 碳排放报告 (基于核查报告)
    function renderCarbonReport(container) {
        let summary = getYearSummary(currentYear);
        let totalElecMWh = summary.totalElec / 1000;
        let totalPvMWh = summary.totalPv / 1000;
        let totalGreenMWh = summary.totalGreen / 1000;
        let netGridMWh = totalElecMWh - totalPvMWh - totalGreenMWh;
        if(netGridMWh < 0) netGridMWh = 0;
        let emissionFactor = 0.4211;
        let calcEmission = netGridMWh * emissionFactor;
        let finalEmission = summary.totalCarbon; 
        let product = "精制茶（乌龙茶、铁观音等）";
        let html = `<div class="dashboard-card report-container">
            <h2 style="color:#1e562e; text-align:center;">📋 福建省安溪县兴溪茶业有限责任公司</h2>
            <h3 style="text-align:center;">${currentYear}年度温室气体排放核查报告（简版）</h3>
            <p><strong>报告主体：</strong>福建省安溪县兴溪茶业有限责任公司</p>
            <p><strong>核查年度：</strong>${currentYear}年1月1日至${currentYear}年12月31日</p>
            <p><strong>产品名称：</strong>${product}</p>
            <p><strong>核算依据：</strong>《工业企业温室气体排放核算和报告通则》（GB/T 32150-2015）</p>
            <h3>📌 排放总量结论</h3>
            <div style="background:#eef5ea; padding:16px; border-radius:24px; margin:12px 0;">
                <span style="font-size:32px; font-weight:bold; color:#2a7a3e;">${finalEmission.toFixed(2)} tCO₂e</span>
                <span style="margin-left:16px;">（净购入电力产生的排放）</span>
            </div>
            <h3>⚡ 活动水平数据</h3>
            <table class="report-table">
                  <tr><th>参数</th><th>数据</th><th>单位</th></tr>
                  <tr><td>总用电量（含光伏）</td><td>${totalElecMWh.toFixed(2)}</td><td>MWh</td></tr>
                  <tr><td>光伏自用电量</td><td>${totalPvMWh.toFixed(2)}</td><td>MWh</td></tr>
                  <tr><td>购入绿电（绿证）</td><td>${totalGreenMWh.toFixed(2)}</td><td>MWh</td></tr>
                  <tr><td><strong>净购入电网电量</strong></td><td><strong>${netGridMWh.toFixed(2)}</strong></td><td>MWh</td></tr>
                  <tr><td>精制茶产量</td><td>${fixedProductTon}</td><td>吨</td></tr>
                  <tr><td>总产值</td><td>${summary.totalOutput.toFixed(1)}</td><td>万元</td></tr>
             </table>
            <h3>🌱 排放因子及来源</h3>
            <table class="report-table">
                <tr><th>参数名称</th><th>数值</th><th>单位</th><th>来源</th></tr>
                <tr><td>福建省电力排放因子</td><td>0.4211</td><td>tCO₂/MWh</td><td>生态环境部公告(2025年第47号)</td></tr>
             </table>
            <h3>📊 排放量计算过程</h3>
            <table class="report-table">
                <tr><th>项目</th><th>活动数据(MWh)</th><th>排放因子(tCO₂/MWh)</th><th>碳排放(tCO₂)</th></tr>
                <tr><td>净购入电力排放</td><td>${netGridMWh.toFixed(2)}</td><td>${emissionFactor}</td><td>${calcEmission.toFixed(2)}</td></tr>
                <tr><td><strong>合计排放</strong></td><td colspan="2"></td><td><strong>${finalEmission.toFixed(2)}</strong></td></tr>
             </table>
            <div class="insight-text"><strong>🌿 清洁能源抵消贡献：</strong>光伏自用减排 ${(totalPvMWh * emissionFactor).toFixed(2)} tCO₂e，绿证交易减排 ${(totalGreenMWh * emissionFactor).toFixed(2)} tCO₂e，合计抵消 ${((totalPvMWh+totalGreenMWh)*emissionFactor).toFixed(2)} tCO₂e。</div>
            <p style="margin-top:16px;"><strong>核查结论：</strong>经核查，${currentYear}年度企业温室气体排放总量为 ${finalEmission.toFixed(2)} 吨二氧化碳当量，排放全部来源于净购入电力。数据真实可靠，支撑材料完整，符合第三方核查要求。</p>
        </div>`;
        container.innerHTML = html;
    }

    // 管理后台逻辑 + 密码验证
    let adminCurrentYear = "2025";
    function openAdminModalWithPassword() {
        let pwd = prompt("请输入管理后台密码：");
        if(pwd === "admin999") {
            openAdminModal();
        } else if(pwd !== null) {
            alert("密码错误，无法进入管理后台！");
        }
    }
    function openAdminModal() {
        document.getElementById('adminModal').style.display = 'block';
        renderAdminTable();
    }
    function renderAdminTable() {
        let yearData = dataset[adminCurrentYear];
        let tbody = document.getElementById('editTableBody');
        tbody.innerHTML = '';
        yearData.forEach((item, idx) => {
            let row = `<tr>
                <td>${item.month}</td>
                <td><input type="number" class="edit-elec" data-idx="${idx}" value="${item.electricity}"></td>
                <td><input type="number" class="edit-green" data-idx="${idx}" value="${item.greenPurchase}"></td>
                <td><input type="number" class="edit-pv" data-idx="${idx}" value="${item.pvSelf}"></td>
                <td><input type="number" class="edit-output" data-idx="${idx}" value="${item.outputValue}"></td>
                <td>${item.energyConsumption}</td>
                <td>${item.carbonEmission}</td>
            </tr>`;
            tbody.insertAdjacentHTML('beforeend', row);
        });
        document.querySelectorAll('#editTableBody input').forEach(inp => {
            inp.addEventListener('change', function() {
                let idx = this.getAttribute('data-idx');
                let elec = parseFloat(document.querySelector(`.edit-elec[data-idx="${idx}"]`).value);
                let green = parseFloat(document.querySelector(`.edit-green[data-idx="${idx}"]`).value);
                let pv = parseFloat(document.querySelector(`.edit-pv[data-idx="${idx}"]`).value);
                let output = parseFloat(document.querySelector(`.edit-output[data-idx="${idx}"]`).value);
                if(!isNaN(elec) && !isNaN(green) && !isNaN(pv) && !isNaN(output)) {
                    let newEnergy = calcEnergy(elec);
                    let newCarbon = calcCarbon(elec, pv, green);
                    dataset[adminCurrentYear][idx].electricity = elec;
                    dataset[adminCurrentYear][idx].greenPurchase = green;
                    dataset[adminCurrentYear][idx].pvSelf = pv;
                    dataset[adminCurrentYear][idx].outputValue = output;
                    dataset[adminCurrentYear][idx].energyConsumption = parseFloat(newEnergy.toFixed(2));
                    dataset[adminCurrentYear][idx].carbonEmission = parseFloat(newCarbon.toFixed(1));
                    let rowCells = this.closest('tr').cells;
                    rowCells[5].innerHTML = dataset[adminCurrentYear][idx].energyConsumption;
                    rowCells[6].innerHTML = dataset[adminCurrentYear][idx].carbonEmission;
                }
            });
        });
    }
    // 实时时间
    function updateLiveTime() {
        let now = new Date();
        let str = now.toLocaleString('zh-CN', { hour12: false });
        document.getElementById('liveTimeDisplay').innerText = `🕒 ${str}`;
    }
    // 下载HTML
    function downloadFullHTML() {
        let html = document.documentElement.outerHTML;
        let blob = new Blob([html], {type: 'text/html'});
        let a = document.createElement('a');
        a.href = URL.createObjectURL(blob);
        a.download = 'xingxi_energy_carbon_platform.html';
        a.click();
        URL.revokeObjectURL(a.href);
    }

    document.getElementById('yearSelector').addEventListener('change', (e) => {
        currentYear = e.target.value;
        renderCurrentTab();
    });
    function initMenu() {
        document.querySelectorAll('.menu-item').forEach(el=>{
            el.addEventListener('click',()=>{
                document.querySelectorAll('.menu-item').forEach(m=>m.classList.remove('active'));
                el.classList.add('active');
                currentTab = el.getAttribute('data-tab');
                renderCurrentTab();
            });
        });
    }
    // 登录
    document.getElementById('doLoginBtn').addEventListener('click',()=>{
        let acc = document.getElementById('loginAccount').value;
        let pwd = document.getElementById('loginPassword').value;
        if(acc === 'xxcy@ntpt' && pwd === 'xxcy1234') {
            document.getElementById('loginContainer').style.display = 'none';
            document.getElementById('mainApp').style.display = 'block';
            initMenu();
            currentTab = "energyQuery";
            document.querySelector('.menu-item[data-tab="energyQuery"]').classList.add('active');
            renderCurrentTab();
            updateLiveTime();
            setInterval(updateLiveTime, 1000);
        } else alert('账号或密码错误');
    });
    document.getElementById('logoutBtn').addEventListener('click',()=>{
        document.getElementById('loginContainer').style.display = 'flex';
        document.getElementById('mainApp').style.display = 'none';
    });
    document.getElementById('adminBtn').addEventListener('click', openAdminModalWithPassword);
    document.getElementById('closeModalBtn').addEventListener('click',()=>document.getElementById('adminModal').style.display='none');
    document.getElementById('saveDataBtn').addEventListener('click',()=>{
        saveToLocal();
        alert('所有年度数据已保存！');
        if(currentYear === adminCurrentYear) renderCurrentTab();
    });
    document.getElementById('downloadHtmlAdminBtn').addEventListener('click', downloadFullHTML);
    document.getElementById('yearAdminTabs').addEventListener('click', (e) => {
        if(e.target.classList.contains('year-tab')) {
            document.querySelectorAll('#yearAdminTabs .year-tab').forEach(t=>t.classList.remove('active'));
            e.target.classList.add('active');
            adminCurrentYear = e.target.getAttribute('data-year');
            renderAdminTable();
        }
    });
    window.onload = ()=>{ document.getElementById('loginContainer').style.display = 'flex'; };
</script>
</body>
</html>
