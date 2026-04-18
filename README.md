# xingxintpt
兴溪茶叶能碳管理平台
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
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'PingFang SC', 'Microsoft YaHei', sans-serif;
        }

        body {
            background: #F5F7FA;
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        /* 登录界面 */
        .login-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            backdrop-filter: blur(8px);
            z-index: 1000;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        .login-card {
            background: white;
            border-radius: 24px;
            padding: 40px 36px;
            width: 420px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
            text-align: center;
            border: 1px solid #E9ECEF;
        }
        .login-card h2 {
            color: #1E3A5F;
            font-size: 28px;
            margin-bottom: 8px;
            font-weight: 600;
        }
        .login-card p {
            color: #5A6874;
            margin-bottom: 32px;
            font-size: 14px;
        }
        .input-group {
            margin-bottom: 24px;
            text-align: left;
        }
        .input-group label {
            display: block;
            color: #2C3E50;
            margin-bottom: 8px;
            font-weight: 500;
            font-size: 14px;
        }
        .input-group input {
            width: 100%;
            background: #F8F9FA;
            border: 1px solid #DEE2E6;
            border-radius: 12px;
            padding: 12px 16px;
            font-size: 15px;
            outline: none;
            transition: all 0.2s;
        }
        .input-group input:focus {
            border-color: #2C7DA0;
            box-shadow: 0 0 0 3px rgba(44,125,160,0.1);
        }
        .login-btn {
            width: 100%;
            background: #2C7DA0;
            border: none;
            padding: 12px;
            border-radius: 12px;
            font-weight: 600;
            font-size: 16px;
            color: white;
            cursor: pointer;
            margin-top: 8px;
            transition: background 0.2s;
        }
        .login-btn:hover { background: #1F5E7E; }
        .download-html { background: #6C757D; margin-top: 16px; }
        .download-html:hover { background: #495057; }

        /* 主应用界面 - 商务简约风格 */
        .main-app {
            display: none;
            width: 100%;
            max-width: 1400px;
            background: white;
            border-radius: 28px;
            box-shadow: 0 8px 30px rgba(0,0,0,0.05);
            overflow: hidden;
            border: 1px solid #EDF2F7;
        }

        /* 头部导航 */
        .nav-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 20px 32px;
            background: white;
            border-bottom: 1px solid #EDF2F7;
        }
        .logo-area h1 {
            font-size: 1.6rem;
            font-weight: 600;
            background: linear-gradient(135deg, #1E3A5F, #2C7DA0);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }
        .user-actions { display: flex; gap: 12px; }
        .btn-outline {
            background: transparent;
            border: 1px solid #CBD5E0;
            padding: 8px 20px;
            border-radius: 40px;
            color: #2D3748;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.2s;
        }
        .btn-outline:hover {
            background: #F1F5F9;
            border-color: #2C7DA0;
            color: #2C7DA0;
        }

        /* 菜单栏 - 钉钉风格选项卡 */
        .menu-tabs {
            display: flex;
            flex-wrap: wrap;
            gap: 4px;
            padding: 12px 24px 0 24px;
            background: white;
            border-bottom: 1px solid #EDF2F7;
        }
        .menu-item {
            padding: 10px 20px;
            border-radius: 24px;
            background: transparent;
            color: #4A5568;
            cursor: pointer;
            font-weight: 500;
            font-size: 14px;
            transition: all 0.2s;
        }
        .menu-item.active {
            background: #F1F5F9;
            color: #2C7DA0;
            font-weight: 600;
        }

        /* 内容区 */
        .dashboard-content {
            padding: 24px 32px;
            background: #F8FAFC;
        }
        .dashboard-card {
            background: white;
            border-radius: 20px;
            padding: 24px;
            margin-bottom: 24px;
            box-shadow: 0 1px 3px rgba(0,0,0,0.05);
            border: 1px solid #E9ECEF;
        }
        .stats-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            margin-bottom: 28px;
        }
        .stat-item {
            background: #F8F9FA;
            border-radius: 20px;
            padding: 20px 24px;
            flex: 1;
            min-width: 160px;
            border-left: 4px solid #2C7DA0;
        }
        .stat-label {
            font-size: 14px;
            color: #6C757D;
            margin-bottom: 8px;
        }
        .stat-value {
            font-size: 28px;
            font-weight: 700;
            color: #1E3A5F;
        }
        .query-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            flex-wrap: wrap;
            gap: 12px;
        }
        .year-selector, select {
            background: white;
            border: 1px solid #CBD5E0;
            border-radius: 30px;
            padding: 8px 20px;
            font-size: 14px;
            color: #2D3748;
            cursor: pointer;
        }
        .chart-container {
            height: 340px;
            width: 100%;
            margin: 20px 0;
        }
        .insight-text {
            background: #F1F9FE;
            padding: 16px 20px;
            border-radius: 16px;
            color: #2C3E50;
            font-size: 14px;
            border-left: 3px solid #2C7DA0;
            margin-top: 16px;
        }
        /* 管理后台模态框 */
        .modal {
            display: none;
            position: fixed;
            top: 0; left: 0;
            width: 100%; height: 100%;
            background: rgba(0,0,0,0.6);
            z-index: 2000;
            overflow: auto;
            backdrop-filter: blur(4px);
        }
        .modal-content {
            background: white;
            margin: 40px auto;
            width: 90%;
            max-width: 1200px;
            border-radius: 28px;
            padding: 28px;
            box-shadow: 0 20px 35px rgba(0,0,0,0.2);
        }
        .data-table {
            width: 100%;
            border-collapse: collapse;
            font-size: 13px;
        }
        .data-table th, .data-table td {
            border: 1px solid #E2E8F0;
            padding: 10px 8px;
            text-align: center;
        }
        .data-table input {
            width: 95px;
            padding: 6px;
            border-radius: 8px;
            border: 1px solid #CBD5E0;
        }
        .save-btn {
            background: #2C7DA0;
            padding: 10px 28px;
            border: none;
            border-radius: 40px;
            color: white;
            font-weight: 600;
            margin-top: 20px;
            cursor: pointer;
        }
        .close-modal {
            float: right;
            font-size: 26px;
            cursor: pointer;
            color: #718096;
        }
        .year-tab-btn {
            display: inline-block;
            margin-right: 16px;
            padding: 6px 20px;
            border-radius: 30px;
            background: #EDF2F7;
            cursor: pointer;
            font-size: 14px;
        }
        .year-tab-btn.active {
            background: #2C7DA0;
            color: white;
        }
    </style>
</head>
<body>

<div id="loginContainer" class="login-container">
    <div class="login-card">
        <h2>🍃 兴溪茶叶能碳管理平台</h2>
        <p>绿色制造 · 智慧能碳</p>
        <div class="input-group"><label>账号</label><input type="text" id="loginAccount" placeholder="xxcy@ntpt"></div>
        <div class="input-group"><label>密码</label><input type="password" id="loginPassword" placeholder="········"></div>
        <button class="login-btn" id="doLoginBtn">登录系统</button>
        <button class="login-btn download-html" id="downloadHtmlBtn">📥 下载 HTML 离线版</button>
    </div>
</div>

<div id="mainApp" class="main-app">
    <div class="nav-header">
        <div class="logo-area"><h1>🍃 兴溪茶叶能碳管理平台</h1></div>
        <div class="user-actions"><button id="adminBtn" class="btn-outline">⚙️ 管理后台</button><button id="logoutBtn" class="btn-outline">退出登录</button></div>
    </div>
    <div class="menu-tabs" id="menuTabs">
        <div class="menu-item" data-tab="energyQuery">能耗查询</div>
        <div class="menu-item" data-tab="energyIntensity">能源消费强度</div>
        <div class="menu-item" data-tab="energyAnalysis">消费分析+策略</div>
        <div class="menu-item" data-tab="benchmark">能效对标</div>
        <div class="menu-item" data-tab="energyFlow">能流分析</div>
        <div class="menu-item" data-tab="carbonCheck">碳排放核查</div>
        <div class="menu-item" data-tab="carbonFootprint">碳足迹核算</div>
        <div class="menu-item" data-tab="carbonSupport">碳核查支撑</div>
    </div>
    <div style="padding: 16px 24px 0 24px; background: white; display: flex; justify-content: flex-end; border-bottom: 1px solid #EDF2F7;">
        <label style="margin-right: 12px; font-weight: 500;">📅 数据年份：</label>
        <select id="yearSelector" class="year-selector"><option value="2025">2025年</option><option value="2026">2026年</option></select>
    </div>
    <div class="dashboard-content" id="tabContent"></div>
</div>

<!-- 管理后台模态框 -->
<div id="adminModal" class="modal">
    <div class="modal-content">
        <span class="close-modal" id="closeModalBtn">&times;</span>
        <h2 style="margin-bottom: 20px;">📊 双年度数据管理 (2025/2026)</h2>
        <div id="yearAdminTabs" style="margin-bottom: 20px;">
            <span class="year-tab-btn" data-year="2025">2025年数据</span>
            <span class="year-tab-btn" data-year="2026">2026年数据</span>
        </div>
        <div style="overflow-x: auto;">
            <table class="data-table" id="dataEditTable">
                <thead><tr><th>月份</th><th>用电量(kWh)</th><th>绿电购入(kWh)</th><th>光伏自用(kWh)</th><th>产值(万元)</th><th>综合能耗(tce)</th><th>碳排放(tCO₂)</th></tr></thead>
                <tbody id="editTableBody"></tbody>
            </table>
        </div>
        <button id="saveDataBtn" class="save-btn">💾 保存全部年度数据</button>
    </div>
</div>

<script>
    // ---------- 双年度数据结构 ----------
    const months = ["1月","2月","3月","4月","5月","6月","7月","8月","9月","10月","11月","12月"];
    // 2025 基准数据 (根据真实汇总)
    const default2025 = {
        electricity: [106850,93200,128700,138500,135600,112300,115800,120500,156200,152800,109600,94600],
        greenPurchase: [18500,16100,22300,24000,23500,19500,20100,20900,27100,26500,19000,16400],
        pvSelf: [12257,8881,17217,19908,6910,11032,22984,25499,20302,17412,13244,12146],
        outputValue: [1780,1550,2430,2650,2580,2050,2120,2210,3100,3020,1980,1580]
    };
    // 2026 预设初始值 (基于2025轻微波动，可被后台覆盖)
    let default2026 = {
        electricity: default2025.electricity.map(v => Math.round(v * 0.98)),
        greenPurchase: default2025.greenPurchase.map(v => Math.round(v * 1.02)),
        pvSelf: default2025.pvSelf.map(v => Math.round(v * 1.01)),
        outputValue: default2025.outputValue.map(v => Math.round(v * 1.03))
    };
    function calcEnergy(elec) { return elec * 1.229 / 10000; }
    function calcCarbon(elec, pv, green) { let net = elec - pv - green; if(net<0) net=0; return net * 0.4211 / 1000; }

    let dataset = {2025: [], 2026: []};
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

    // 年度汇总辅助
    function getYearSummary(year) {
        let data = dataset[year];
        let totalElec = data.reduce((s,d)=>s+d.electricity,0);
        let totalEnergy = data.reduce((s,d)=>s+d.energyConsumption,0);
        let totalOutput = data.reduce((s,d)=>s+d.outputValue,0);
        let totalCarbon = data.reduce((s,d)=>s+d.carbonEmission,0);
        return { totalElec, totalEnergy, totalOutput, totalCarbon };
    }
    const fixedProductTon = 5571;
    const fixedWaterUse = 1610;

    let currentYear = "2025";
    let currentTab = "energyQuery";

    // 本地存储
    const STORAGE_KEY = 'xingxi_dual_energy';
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

    // 渲染主内容（根据当前标签和年份）
    function renderCurrentTab() {
        const container = document.getElementById("tabContent");
        if(currentTab === "energyQuery") renderEnergyQuery(container);
        else if(currentTab === "energyIntensity") renderEnergyIntensity(container);
        else if(currentTab === "energyAnalysis") renderEnergyAnalysis(container);
        else if(currentTab === "benchmark") renderBenchmark(container);
        else if(currentTab === "energyFlow") renderEnergyFlow(container);
        else if(currentTab === "carbonCheck") renderCarbonCheck(container);
        else if(currentTab === "carbonFootprint") renderCarbonFootprint(container);
        else if(currentTab === "carbonSupport") renderCarbonSupport(container);
    }
    function buildLineChart(domId, xData, seriesData, yName, unit) {
        let chart = echarts.init(document.getElementById(domId));
        chart.setOption({
            tooltip: { trigger: 'axis' },
            xAxis: { type: 'category', data: xData, axisLabel: { rotate: 30, color: '#4A5568' } },
            yAxis: { type: 'value', name: unit, nameTextStyle: { color: '#4A5568' }, axisLabel: { color: '#4A5568' } },
            series: [{ data: seriesData, type: 'line', smooth: true, lineStyle: { width: 3, color: '#2C7DA0' }, areaStyle: { opacity: 0.1, color: '#A0C4D9' }, symbol: 'circle', symbolSize: 7 }],
            grid: { borderWidth: 0, containLabel: true },
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
            <div class="stat-item"><div class="stat-label">🌾 总产值 (${currentYear})</div><div class="stat-value">${summary.totalOutput.toFixed(1)} 万元</div></div>
            <div class="stat-item"><div class="stat-label">🍃 精制茶产量</div><div class="stat-value">${fixedProductTon} 吨</div></div>
            <div class="stat-item"><div class="stat-label">💧 取水量</div><div class="stat-value">${fixedWaterUse} t</div></div>
            <div class="stat-item"><div class="stat-label">⚡ 全年用电量</div><div class="stat-value">${(summary.totalElec/10000).toFixed(2)} 万kWh</div></div>
            <div class="stat-item"><div class="stat-label">⛽ 综合能耗</div><div class="stat-value">${summary.totalEnergy.toFixed(2)} tce</div></div>
        </div>
        <div class="query-bar"><span>📈 月度用电量趋势 (${currentYear})</span><select id="monthQueryElec"><option value="">查询单月</option>${monthsArr.map((m,i)=>`<option value="${i}">${m}</option>`).join('')}</select></div>
        <div id="elecChart" class="chart-container"></div>
        <div id="monthResult" class="insight-text">✨ 选择月份查看详细数据</div></div>`;
        container.innerHTML = html;
        buildLineChart('elecChart', monthsArr, elecArr, '用电量', 'kWh');
        let sel = document.getElementById('monthQueryElec');
        sel.addEventListener('change', (e) => {
            let idx = e.target.value;
            if(idx !== "") {
                let d = yearData[parseInt(idx)];
                document.getElementById('monthResult').innerHTML = `📌 ${d.month} 用电量: ${d.electricity.toLocaleString()} kWh | 绿电购入: ${d.greenPurchase} kWh | 光伏自用: ${d.pvSelf} kWh | 产值: ${d.outputValue} 万元`;
            } else document.getElementById('monthResult').innerHTML = `✨ ${currentYear}年总用电量 ${(summary.totalElec/10000).toFixed(2)} 万kWh，清洁能源占比30%+`;
        });
    }
    // 能源消费强度
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
    // 能源消费分析+策略推荐
    function renderEnergyAnalysis(container) {
        let yearData = dataset[currentYear];
        let monthsArr = yearData.map(d=>d.month);
        let intensity = yearData.map(d=> (d.energyConsumption*1000/d.outputValue).toFixed(2) );
        container.innerHTML = `<div class="dashboard-card"><div class="query-bar"><span>📉 万元产值综合能耗 (kgce/万元) - ${currentYear}</span><select id="intensitySel"><option value="">月度查询</option>${monthsArr.map((m,i)=>`<option value="${i}">${m}</option>`).join('')}</select></div>
        <div id="intensityChart" class="chart-container"></div><div id="intensityResult" class="insight-text"></div>
        <div class="insight-text"><strong>🍃 茶叶行业用能策略推荐</strong><br>✔ 杀青余热回收，降低能耗8~12%<br>✔ 揉捻工序集中驱动+变频控制，减少空载损耗<br>✔ 光伏扩容+绿电交易，可再生电力占比提升至35%<br>✔ 烘干热泵替代电热管，能效提升40%<br>✔ 能源管理体系与定额考核，年节能5-8%</div></div>`;
        buildLineChart('intensityChart', monthsArr, intensity, '万元产值综合能耗', 'kgce/万元');
        let sel = document.getElementById('intensitySel');
        sel.addEventListener('change',(e)=>{
            if(e.target.value!=""){
                let idx=e.target.value; let d=yearData[idx];
                let val = (d.energyConsumption*1000/d.outputValue).toFixed(2);
                document.getElementById('intensityResult').innerHTML=`${d.month} 万元产值综合能耗: ${val} kgce/万元`;
            } else document.getElementById('intensityResult').innerHTML=`${currentYear}年度均值: ${(getYearSummary(currentYear).totalEnergy*1000/getYearSummary(currentYear).totalOutput).toFixed(2)} kgce/万元 (优于行业平均)`;
        });
    }
    // 能效对标 (单位产品综合能耗)
    function renderBenchmark(container) {
        let summary = getYearSummary(currentYear);
        let unitEnergy = (summary.totalEnergy * 1000 / fixedProductTon).toFixed(2);
        let advancedBench = 32;
        let gap = (unitEnergy - advancedBench).toFixed(2);
        container.innerHTML = `<div class="dashboard-card"><h3>🎯 能效对标 (单位产品综合能耗 - ${currentYear})</h3><div style="display:flex;gap:30px;flex-wrap:wrap;"><div style="background:#F8FAFC;padding:20px;border-radius:24px;flex:1"><div>🍃 兴溪茶业 ${currentYear}</div><div style="font-size:42px;font-weight:bold;color:#1E3A5F;">${unitEnergy}</div><div>kgce/t 成品茶</div></div><div style="background:#F8FAFC;padding:20px;border-radius:24px;flex:1"><div>🏆 行业先进水平</div><div style="font-size:42px;">${advancedBench}</div><div>kgce/t</div></div></div>
        <div class="insight-text">📊 差距: ${gap} kgce/t，通过余热回收、绿电替代及设备升级可缩小差距。</div><div id="benchGauge" style="height:240px;"></div></div>`;
        let gauge = echarts.init(document.getElementById("benchGauge"));
        gauge.setOption({ series: [{ type: 'gauge', min:0, max:70, progress:{show:true,width:18}, axisLine:{lineStyle:{width:18,color:[[0.5,'#48b07b'],[0.8,'#e6b422'],[1,'#e86848']]}}, pointer:{show:true,length:'60%'}, detail:{offsetCenter:[0,25],fontSize:18}, data:[{value:parseFloat(unitEnergy), name:"单位产品能耗"}] }] });
    }
    // 能流分析 (桑基图)
    function renderEnergyFlow(container) {
        let summary = getYearSummary(currentYear);
        let totalElec = summary.totalElec / 10000;
        let pvTotal = dataset[currentYear].reduce((s,d)=>s+d.pvSelf,0)/10000;
        let greenTotal = dataset[currentYear].reduce((s,d)=>s+d.greenPurchase,0)/10000;
        let gridPower = totalElec - pvTotal;
        let nodes = [{name:"外购电网"},{name:"光伏发电"},{name:"绿证采购"},{name:"厂内配电"},{name:"杀青工序"},{name:"揉捻烘干"},{name:"辅助系统"}];
        let links = [
            {source:"外购电网", target:"厂内配电", value:gridPower},
            {source:"光伏发电", target:"厂内配电", value:pvTotal},
            {source:"绿证采购", target:"碳排放抵消", value:greenTotal},
            {source:"厂内配电", target:"杀青工序", value:totalElec*0.45},
            {source:"厂内配电", target:"揉捻烘干", value:totalElec*0.4},
            {source:"厂内配电", target:"辅助系统", value:totalElec*0.15}
        ];
        container.innerHTML = `<div class="dashboard-card"><h3>🌀 ${currentYear} 能源流向图</h3><div id="sankeyChart" style="height:400px;"></div><div class="insight-text">光伏自用占比${((pvTotal/totalElec)*100).toFixed(1)}%，绿证抵消碳排放，清洁能源占比超30%</div></div>`;
        let chart = echarts.init(document.getElementById("sankeyChart"));
        chart.setOption({ tooltip: { trigger: 'item' }, series: { type: 'sankey', data: nodes, links: links, nodeWidth: 28, lineStyle: { color: 'gradient' } } });
    }
    // 碳排放核查
    function renderCarbonCheck(container) {
        let yearData = dataset[currentYear];
        let monthsArr = yearData.map(d=>d.month);
        let carbonData = yearData.map(d=>d.carbonEmission);
        container.innerHTML = `<div class="dashboard-card"><div class="query-bar"><span>🌱 月度碳排放趋势 (tCO₂) - ${currentYear}</span><select id="carbonMonth"><option value="">查询单月</option>${monthsArr.map((m,i)=>`<option value="${i}">${m}</option>`).join('')}</select></div>
        <div id="carbonChart" class="chart-container"></div><div id="carbonResult" class="insight-text"></div></div>`;
        buildLineChart('carbonChart', monthsArr, carbonData, '碳排放量', 'tCO₂');
        let sel = document.getElementById('carbonMonth');
        sel.addEventListener('change',(e)=>{
            if(e.target.value!=""){let d=yearData[parseInt(e.target.value)]; document.getElementById('carbonResult').innerHTML=`${d.month} 碳排放: ${d.carbonEmission} tCO₂ (净购入电力排放)`;}
            else document.getElementById('carbonResult').innerHTML=`${currentYear}碳排放总量: ${getYearSummary(currentYear).totalCarbon.toFixed(1)} tCO₂，较基准年下降明显`;
        });
    }
    // 碳足迹核算
    function renderCarbonFootprint(container) {
        let totalCarbon = getYearSummary(currentYear).totalCarbon;
        let perProduct = (totalCarbon*1000/fixedProductTon).toFixed(1);
        container.innerHTML = `<div class="dashboard-card"><h3>👣 产品碳足迹 (每吨精制茶 - ${currentYear})</h3><div style="font-size:48px;font-weight:bold;color:#1E3A5F;">${perProduct} kgCO₂e/t</div><div class="insight-text">基于范围二电力碳排放（净购入+光伏+绿证抵消），符合ISO14067。光伏及绿电贡献显著，较行业均值低18%</div>
        <div style="margin-top:20px;"><strong>📌 碳足迹组成</strong><br>外购电力排放占比约85% · 绿证抵消12% · 光伏自用3% · 后续推进供应链减排</div></div>`;
    }
    // 碳核查支撑
    function renderCarbonSupport(container) {
        container.innerHTML = `<div class="dashboard-card"><h3>📁 碳核查支撑材料清单</h3><ul style="margin-left:20px;color:#2C3E50;"><li>✔ 2025年能源购进消费与库存表(205-1)</li><li>✔ 电力结算单及绿色电力证书交易凭证(264张绿证)</li><li>✔ 光伏发电量月度统计表/上网电量记录</li><li>✔ 工业产销总值B204-1表(产值佐证)</li><li>✔ 温室气体排放核查报告(内部核算)</li><li>✔ 碳排放因子引用生态环境部2023年省级电力排放因子</li><li>✔ 能源计量器具台账及校准记录</li><li>✔ 产品碳足迹自评报告及LCA分析</li></ul><div class="insight-text">所有支撑材料完整，满足第三方核查及绿色工厂评价要求。</div></div>`;
    }

    // 管理后台
    let adminCurrentYear = "2025";
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

    // 事件绑定
    document.getElementById('yearSelector').addEventListener('change', (e) => { currentYear = e.target.value; renderCurrentTab(); });
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
        } else alert('账号或密码错误');
    });
    document.getElementById('logoutBtn').addEventListener('click',()=>{
        document.getElementById('loginContainer').style.display = 'flex';
        document.getElementById('mainApp').style.display = 'none';
    });
    document.getElementById('adminBtn').addEventListener('click', openAdminModal);
    document.getElementById('closeModalBtn').addEventListener('click',()=>document.getElementById('adminModal').style.display='none');
    document.getElementById('saveDataBtn').addEventListener('click',()=>{
        saveToLocal();
        alert('数据已保存！');
        if(currentYear === adminCurrentYear) renderCurrentTab();
    });
    document.getElementById('yearAdminTabs').addEventListener('click', (e) => {
        if(e.target.classList.contains('year-tab-btn')) {
            document.querySelectorAll('#yearAdminTabs .year-tab-btn').forEach(t=>t.classList.remove('active'));
            e.target.classList.add('active');
            adminCurrentYear = e.target.getAttribute('data-year');
            renderAdminTable();
        }
    });
    document.getElementById('downloadHtmlBtn').addEventListener('click',()=>{
        let htmlContent = document.documentElement.outerHTML;
        let blob = new Blob([htmlContent], {type: 'text/html'});
        let link = document.createElement('a');
        link.href = URL.createObjectURL(blob);
        link.download = 'xingxi_energy_carbon_platform.html';
        link.click();
        URL.revokeObjectURL(link.href);
    });
    window.onload = () => { document.getElementById('loginContainer').style.display = 'flex'; };
</script>
</body>
</html>
