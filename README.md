<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>服装店销售管理系统</title>
    <style>
        *{margin:0;padding:0;box-sizing:border-box}
        body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI','PingFang SC','Microsoft YaHei',sans-serif;background:linear-gradient(135deg,#667eea 0%,#764ba2 100%);min-height:100vh;padding:20px}
        .container{max-width:900px;margin:0 auto}
        h1{text-align:center;color:white;margin-bottom:20px;font-size:28px;text-shadow:0 2px 4px rgba(0,0,0,0.2)}
        .card{background:white;border-radius:16px;padding:20px;margin-bottom:20px;box-shadow:0 10px 30px rgba(0,0,0,0.2)}
        .login-container{max-width:400px;margin:80px auto;text-align:center}
        .role-selector{display:grid;grid-template-columns:1fr;gap:20px;margin-top:30px}
        .role-card{background:white;padding:30px;border-radius:16px;cursor:pointer;transition:all 0.3s;border:3px solid transparent}
        .role-card.staff{border-color:#4caf50}
        .role-card.staff:hover{transform:translateY(-5px);box-shadow:0 10px 30px rgba(76,175,80,0.3)}
        .role-card.boss{border-color:#ff9800}
        .role-card.boss:hover{transform:translateY(-5px);box-shadow:0 10px 30px rgba(255,152,0,0.3)}
        .role-icon{font-size:48px;margin-bottom:10px}
        .role-title{font-size:20px;font-weight:bold;margin-bottom:5px}
        .role-desc{color:#666;font-size:14px;line-height:1.6}
        .no-password-badge{display:inline-block;background:#4caf50;color:white;padding:4px 12px;border-radius:12px;font-size:12px;margin-top:10px}
        .password-section{margin-top:20px;display:none}
        .password-input{width:100%;padding:15px;border:2px solid #ddd;border-radius:8px;font-size:18px;text-align:center;letter-spacing:8px;margin-bottom:15px}
        .btn{width:100%;padding:15px;border:none;border-radius:8px;font-size:16px;font-weight:bold;cursor:pointer;transition:all 0.3s}
        .btn-primary{background:linear-gradient(135deg,#667eea 0%,#764ba2 100%);color:white}
        .btn-secondary{background:#f0f0f0;color:#333;margin-top:10px}
        .back-link{display:inline-block;margin-top:15px;color:#666;text-decoration:none;font-size:14px}
        .header{display:flex;justify-content:space-between;align-items:center;margin-bottom:20px;flex-wrap:wrap;gap:10px}
        .mode-indicator{display:flex;align-items:center;gap:10px;color:white}
        .mode-badge{padding:6px 16px;border-radius:20px;font-size:14px;font-weight:bold}
        .mode-badge.staff{background:#4caf50}
        .mode-badge.boss{background:#ff9800}
        .logout-btn{background:rgba(255,255,255,0.2);color:white;border:1px solid rgba(255,255,255,0.3);padding:8px 16px;border-radius:8px;cursor:pointer;font-size:14px}
        .stats-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(150px,1fr));gap:15px;margin-bottom:20px}
        .stat-box{background:linear-gradient(135deg,#667eea 0%,#764ba2 100%);color:white;padding:20px;border-radius:12px;text-align:center}
        .stat-box.hidden{background:#e0e0e0;position:relative;overflow:hidden}
        .stat-box.hidden::after{content:"🔒";position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);font-size:24px;opacity:0.5}
        .stat-box.hidden .stat-value{filter:blur(4px);user-select:none}
        .stat-label{font-size:12px;opacity:0.9;margin-bottom:5px}
        .stat-value{font-size:24px;font-weight:bold}
        .form-group{margin-bottom:15px}
        label{display:block;margin-bottom:5px;color:#333;font-weight:500}
        input,select{width:100%;padding:12px;border:2px solid #e0e0e0;border-radius:8px;font-size:16px}
        input:focus,select:focus{outline:none;border-color:#667eea}
        .form-row{display:grid;grid-template-columns:1fr 1fr;gap:15px}
        button[type="submit"]{width:100%;padding:15px;background:linear-gradient(135deg,#667eea 0%,#764ba2 100%);color:white;border:none;border-radius:8px;font-size:16px;font-weight:bold;cursor:pointer}
        .records-header{display:grid;grid-template-columns:2fr 1fr 1fr 1fr 1fr 1fr 100px;gap:10px;padding:12px 15px;background:#f8f9fa;border-radius:8px;margin-bottom:10px;font-weight:600;color:#666;font-size:13px;text-align:center}
        .records-header.staff-view{grid-template-columns:2fr 1fr 1fr 1fr 80px}
        .records-header span:first-child{text-align:left}
        .record-row{display:grid;grid-template-columns:2fr 1fr 1fr 1fr 1fr 1fr 100px;gap:10px;padding:15px;border-bottom:1px solid #f0f0f0;align-items:center;font-size:14px}
        .record-row.staff-view{grid-template-columns:2fr 1fr 1fr 1fr 80px}
        .record-row:hover{background:#fafafa;border-radius:8px}
        .record-product{display:flex;flex-direction:column;gap:4px}
        .record-name{font-weight:600;color:#333;font-size:15px}
        .record-category{display:inline-block;padding:2px 8px;background:#e3f2fd;color:#1976d2;border-radius:4px;font-size:11px;width:fit-content}
        .record-date{color:#999;font-size:12px}
        .record-cell{text-align:center;color:#555}
        .record-price{font-weight:500;color:#333}
        .record-profit{font-weight:bold;font-size:15px}
        .profit-positive{color:#4caf50}
        .profit-negative{color:#f44336}
        .record-actions{text-align:center;display:flex;gap:5px;justify-content:center}
        .action-btn{padding:6px 10px;border:none;border-radius:6px;cursor:pointer;font-size:12px}
        .edit-btn{background:#e3f2fd;color:#1976d2}
        .delete-btn{background:#ffebee;color:#c62828}
        .empty-state{text-align:center;padding:60px 20px;color:#999}
        .empty-state-icon{font-size:48px;margin-bottom:10px}
        .date-filter{display:flex;gap:10px;margin-bottom:15px;flex-wrap:wrap}
        .date-filter button{flex:1;min-width:80px;padding:8px;font-size:14px;background:#f0f0f0;color:#333;border:none;border-radius:8px;cursor:pointer}
        .date-filter button.active{background:linear-gradient(135deg,#667eea 0%,#764ba2 100%);color:white}
        .export-btn{background:#52c41a;margin-top:10px;width:100%;padding:12px;border:none;border-radius:8px;color:white;font-size:16px;cursor:pointer}
        .clear-btn{background:#ff4d4f;margin-top:10px;width:100%;padding:12px;border:none;border-radius:8px;color:white;font-size:16px;cursor:pointer}
        .summary-row{display:grid;grid-template-columns:2fr 1fr 1fr 1fr 1fr 1fr 100px;gap:10px;padding:15px;background:#f5f5f5;border-radius:8px;margin-top:10px;font-weight:bold;color:#333;border-top:2px solid #e0e0e0}
        .summary-row.staff-view{grid-template-columns:2fr 1fr 1fr 1fr 80px}
        .summary-label{text-align:left;color:#666}
        .modal-overlay{position:fixed;top:0;left:0;right:0;bottom:0;background:rgba(0,0,0,0.5);display:none;justify-content:center;align-items:center;z-index:1000;padding:20px}
        .modal-overlay.active{display:flex}
        .modal-content{background:white;padding:25px;border-radius:16px;width:100%;max-width:400px}
        .modal-title{font-size:20px;font-weight:bold;margin-bottom:20px;color:#333}
        .modal-buttons{display:flex;gap:10px;margin-top:20px}
        .modal-buttons button{flex:1;padding:12px;border:none;border-radius:8px;font-size:16px;cursor:pointer}
        .btn-save{background:linear-gradient(135deg,#667eea 0%,#764ba2 100%);color:white}
        .btn-cancel{background:#f0f0f0;color:#333}
        .current-value{background:#f5f5f5;padding:12px;border-radius:8px;margin-bottom:15px;font-size:14px;color:#666}
        .info-banner{background:#e3f2fd;border-left:4px solid #2196f3;padding:12px;margin-bottom:20px;border-radius:4px;color:#1976d2;font-size:14px}
        @media(max-width:768px){.form-row{grid-template-columns:1fr}.stats-grid{grid-template-columns:1fr 1fr}.records-header,.record-row,.summary-row{grid-template-columns:1fr}.records-header.staff-view,.record-row.staff-view,.summary-row.staff-view{grid-template-columns:1fr}.records-header{display:none}.record-row{gap:8px;padding:15px;margin-bottom:10px;background:#fafafa;border-radius:12px;border:1px solid #e0e0e0}.record-row>div{display:flex;justify-content:space-between;align-items:center;padding:4px 0}.record-row>div::before{content:attr(data-label);font-weight:600;color:#666;font-size:12px}.record-product{flex-direction:row;justify-content:space-between;align-items:center;border-bottom:1px solid #e0e0e0;padding-bottom:10px;margin-bottom:5px}.record-product::before{display:none}.record-actions{justify-content:flex-end;margin-top:10px;padding-top:10px;border-top:1px dashed #ddd}.record-actions::before{display:none}.summary-row{grid-template-columns:1fr 1fr}.summary-row>div:nth-child(3),.summary-row>div:nth-child(4),.summary-row>div:nth-child(5),.summary-row>div:nth-child(6){display:none}}
    </style>
</head>
<body>
    <div id="login-select" class="container">
        <div class="login-container">
            <h1 style="margin-bottom:10px">👗 销售管理系统</h1>
            <p style="color:rgba(255,255,255,0.8);margin-bottom:30px">请选择身份进入</p>
            <div class="role-selector">
                <div class="role-card staff" onclick="enterStaff()">
                    <div class="role-icon">👤</div>
                    <div class="role-title">店员入口</div>
                    <div class="role-desc">快速录入销售记录<br>查看当日营收统计</div>
                    <span class="no-password-badge">✓ 免密码直接进入</span>
                </div>
                <div class="role-card boss" onclick="showBossLogin()">
                    <div class="role-icon">👑</div>
                    <div class="role-title">老板入口</div>
                    <div class="role-desc">查看成本利润数据<br>修改成本、导出报表</div>
                </div>
            </div>
        </div>
    </div>

    <div id="login-boss" class="container" style="display:none">
        <div class="login-container">
            <h1 style="margin-bottom:10px">👑 老板验证</h1>
            <p style="color:rgba(255,255,255,0.8);margin-bottom:30px">请输入管理密码（默认：8888）</p>
            <div class="password-section" style="display:block">
                <input type="password" id="boss-password" class="password-input" placeholder="••••" maxlength="6">
                <button class="btn btn-primary" onclick="enterBoss()">进入系统</button>
                <a href="#" class="back-link" onclick="backToSelect()">← 返回选择</a>
            </div>
        </div>
    </div>

    <div id="app-page" style="display:none">
        <div class="container">
            <div class="header">
                <h1>👗 销售管理系统</h1>
                <div class="mode-indicator">
                    <span id="mode-badge" class="mode-badge"></span>
                    <button class="logout-btn" onclick="logout()">退出</button>
                </div>
            </div>
            <div id="info-banner" class="info-banner"></div>
            <div class="card">
                <div class="date-filter">
                    <button onclick="filterDate('today')" class="active" id="btn-today">今日</button>
                    <button onclick="filterDate('week')" id="btn-week">本周</button>
                    <button onclick="filterDate('month')" id="btn-month">本月</button>
                    <button onclick="filterDate('all')" id="btn-all">全部</button>
                </div>
                <div class="stats-grid" id="stats-grid"></div>
            </div>
            <div class="card" id="input-card">
                <h2 style="margin-bottom:15px;color:#333">📝 记一笔</h2>
                <form id="sale-form">
                    <div class="form-row">
                        <div class="form-group">
                            <label>商品名称 *</label>
                            <input type="text" id="product-name" placeholder="例如：碎花连衣裙" required>
                        </div>
                        <div class="form-group">
                            <label>分类</label>
                            <select id="category">
                                <option value="上衣">上衣</option>
                                <option value="裤子">裤子</option>
                                <option value="裙子">裙子</option>
                                <option value="外套">外套</option>
                                <option value="配饰">配饰</option>
                                <option value="其他">其他</option>
                            </select>
                        </div>
                    </div>
                    <div class="form-row">
                        <div class="form-group">
                            <label>售价（元）*</label>
                            <input type="number" id="sale-price" placeholder="199" min="0" step="0.01" required>
                        </div>
                        <div class="form-group" id="cost-group">
                            <label>成本（元）<span id="cost-hint" style="color:#999;font-size:12px"></span></label>
                            <input type="number" id="cost-price" placeholder="0" min="0" step="0.01">
                        </div>
                    </div>
                    <div class="form-row">
                        <div class="form-group">
                            <label>数量 *</label>
                            <input type="number" id="quantity" placeholder="1" min="1" value="1" required>
                        </div>
                        <div class="form-group">
                            <label>日期</label>
                            <input type="date" id="sale-date" required>
                        </div>
                    </div>
                    <button type="submit" id="submit-btn">➕ 添加记录</button>
                </form>
            </div>
            <div class="card">
                <h2 style="margin-bottom:15px;color:#333">📋 销售记录</h2>
                <div class="records-header" id="records-header"></div>
                <div id="records-list">
                    <div class="empty-state">
                        <div class="empty-state-icon">📭</div>
                        <div>暂无记录</div>
                    </div>
                </div>
                <button class="export-btn" onclick="exportData()" id="export-btn">📥 导出Excel</button>
                <button class="clear-btn" onclick="clearAll()" id="clear-btn">🗑️ 清空所有数据</button>
            </div>
        </div>
    </div>

    <div class="modal-overlay" id="edit-modal">
        <div class="modal-content">
            <div class="modal-title">✏️ 修改成本</div>
            <div class="current-value" id="current-info"></div>
            <div class="form-group">
                <label>新成本（元）</label>
                <input type="number" id="edit-cost" min="0" step="0.01" placeholder="输入新成本">
            </div>
            <div class="modal-buttons">
                <button class="btn-save" onclick="saveEdit()">保存</button>
                <button class="btn-cancel" onclick="closeModal()">取消</button>
            </div>
        </div>
    </div>

    <script>
        const CONFIG={bossPassword:'8888',defaultCost:0};
        let currentRole='staff',records=JSON.parse(localStorage.getItem('clothingSales'))||[],currentFilter='today',editingId=null;

        // URL参数检测 - 页面加载时立即执行
        (function checkUrlParams(){
            const urlParams=new URLSearchParams(window.location.search);
            const mode=urlParams.get('mode');
            console.log('URL参数:',window.location.search,'mode=',mode);
            
            if(mode==='staff'){
                currentRole='staff';
                document.getElementById('login-select').style.display='none';
                document.getElementById('app-page').style.display='block';
                initApp();
            }else if(mode==='boss'){
                document.getElementById('login-select').style.display='none';
                document.getElementById('login-boss').style.display='block';
                setTimeout(()=>document.getElementById('boss-password').focus(),100);
            }
        })();

        function enterStaff(){currentRole='staff';showApp()}
        function showBossLogin(){document.getElementById('login-select').style.display='none';document.getElementById('login-boss').style.display='block';setTimeout(()=>document.getElementById('boss-password').focus(),100)}
        function backToSelect(){document.getElementById('login-boss').style.display='none';document.getElementById('login-select').style.display='block';document.getElementById('boss-password').value=''}
        function enterBoss(){
            if(document.getElementById('boss-password').value===CONFIG.bossPassword){currentRole='boss';showApp()}
            else{alert('密码错误！默认密码：8888');document.getElementById('boss-password').value='';document.getElementById('boss-password').focus()}
        }
        function showApp(){document.getElementById('login-select').style.display='none';document.getElementById('login-boss').style.display='none';document.getElementById('app-page').style.display='block';initApp()}
        function logout(){if(confirm('确定要退出吗？'))window.location.href=window.location.pathname}

        function initApp(){
            const badge=document.getElementById('mode-badge'),banner=document.getElementById('info-banner'),costHint=document.getElementById('cost-hint'),costInput=document.getElementById('cost-price'),clearBtn=document.getElementById('clear-btn');
            if(currentRole==='staff'){
                badge.className='mode-badge staff';badge.innerHTML='👤 店员录入';
                banner.innerHTML='💡 店员模式：请录入商品信息和售价，成本可留空（默认0），老板后续可修改';
                costHint.textContent='（选填）';costInput.placeholder='留空则默认0';costInput.removeAttribute('required');clearBtn.style.display='none';
            }else{
                badge.className='mode-badge boss';badge.innerHTML='👑 老板管理';
                banner.innerHTML='👑 老板模式：查看完整数据，点击"修改"可调整成本，查看真实利润';
                costHint.textContent='';costInput.placeholder='80';costInput.setAttribute('required','required');clearBtn.style.display='block';
            }
            document.getElementById('sale-date').valueAsDate=new Date();
            updateStats();updateTable();
            document.getElementById('sale-form').addEventListener('submit',handleSubmit);
        }

        function handleSubmit(e){
            e.preventDefault();
            const name=document.getElementById('product-name').value,category=document.getElementById('category').value,salePrice=parseFloat(document.getElementById('sale-price').value),costPrice=parseFloat(document.getElementById('cost-price').value)||CONFIG.defaultCost,quantity=parseInt(document.getElementById('quantity').value),date=document.getElementById('sale-date').value;
            if(currentRole==='staff'&&!document.getElementById('cost-price').value&&!confirm('您没有输入成本，系统将记录成本为0，确定继续吗？\n（提示：老板后续可在数据中补充成本）'))return;
            records.unshift({id:Date.now(),name,category,salePrice,costPrice,quantity,date,timestamp:new Date().getTime(),createdBy:currentRole,modifiedBy:null,modifiedAt:null});
            saveData();updateStats();updateTable();
            document.getElementById('product-name').value='';document.getElementById('sale-price').value='';document.getElementById('cost-price').value='';document.getElementById('quantity').value='1';document.getElementById('product-name').focus();
            showToast('✅ 记录添加成功！');
        }

        function saveData(){localStorage.setItem('clothingSales',JSON.stringify(records))}

        function updateStats(){
            const filtered=getFilteredRecords();
            let revenue=0,cost=0;
            filtered.forEach(r=>{revenue+=r.salePrice*r.quantity;cost+=r.costPrice*r.quantity});
            const profit=revenue-cost,rate=revenue>0?((profit/revenue)*100).toFixed(1):0;
            const grid=document.getElementById('stats-grid');
            if(currentRole==='staff'){
                grid.innerHTML=`<div class="stat-box"><div class="stat-label">今日营收</div><div class="stat-value">¥${revenue.toFixed(2)}</div></div><div class="stat-box"><div class="stat-label">销售件数</div><div class="stat-value">${filtered.reduce((sum,r)=>sum+r.quantity,0)}件</div></div><div class="stat-box hidden"><div class="stat-label">成本</div><div class="stat-value">****</div></div><div class="stat-box hidden"><div class="stat-label">利润</div><div class="stat-value">🔒</div></div>`;
            }else{
                grid.innerHTML=`<div class="stat-box"><div class="stat-label">营业收入</div><div class="stat-value">¥${revenue.toFixed(2)}</div></div><div class="stat-box"><div class="stat-label">总成本</div><div class="stat-value">¥${cost.toFixed(2)}</div></div><div class="stat-box"><div class="stat-label">净利润</div><div class="stat-value" style="color:${profit>=0?'#fff':'#ffebee'}">¥${profit.toFixed(2)}</div></div><div class="stat-box"><div class="stat-label">利润率</div><div class="stat-value">${rate}%</div></div>`;
            }
        }

        function updateTable(){
            const filtered=getFilteredRecords(),header=document.getElementById('records-header'),list=document.getElementById('records-list');
            if(currentRole==='staff'){header.className='records-header staff-view';header.innerHTML='<span>商品信息</span><span>售价</span><span>数量</span><span>营收</span><span>操作</span>';}
            else{header.className='records-header';header.innerHTML='<span>商品信息</span><span>售价</span><span>成本</span><span>数量</span><span>营收</span><span>利润</span><span>操作</span>';}
            if(filtered.length===0){list.innerHTML='<div class="empty-state"><div class="empty-state-icon">📭</div><div>该时间段暂无记录</div></div>';return;}
            let html='';
            filtered.forEach(r=>{
                const itemRevenue=r.salePrice*r.quantity,itemCost=r.costPrice*r.quantity,itemProfit=itemRevenue-itemCost,isProfit=itemProfit>=0;
                if(currentRole==='staff'){
                    html+=`<div class="record-row staff-view"><div class="record-product" data-label="商品"><div><div class="record-name">${r.name}</div><span class="record-category">${r.category}</span></div><div class="record-date">${r.date}</div></div><div class="record-cell record-price" data-label="售价">¥${r.salePrice.toFixed(2)}</div><div class="record-cell" data-label="数量">${r.quantity}</div><div class="record-cell record-price" data-label="营收">¥${itemRevenue.toFixed(2)}</div><div class="record-actions" data-label="操作"><button class="delete-btn action-btn" onclick="deleteRecord(${r.id})">删除</button></div></div>`;
                }else{
                    html+=`<div class="record-row"><div class="record-product" data-label="商品"><div><div class="record-name">${r.name}</div><span class="record-category">${r.category}</span>${r.modifiedBy?'<span style="color:#ff9800;font-size:11px;">(已修改)</span>':''}</div><div class="record-date">${r.date}</div></div><div class="record-cell record-price" data-label="售价">¥${r.salePrice.toFixed(2)}</div><div class="record-cell" data-label="成本" style="color:${r.costPrice===0?'#999':'#333'}">¥${r.costPrice.toFixed(2)}${r.costPrice===0?'<span style="font-size:10px;color:#999;">(待补)</span>':''}</div><div class="record-cell" data-label="数量">${r.quantity}</div><div class="record-cell record-price" data-label="营收">¥${itemRevenue.toFixed(2)}</div><div class="record-cell record-profit ${isProfit?'profit-positive':'profit-negative'}" data-label="利润">${isProfit?'+':''}¥${itemProfit.toFixed(2)}</div><div class="record-actions" data-label="操作"><button class="edit-btn action-btn" onclick="openEdit(${r.id})">修改</button><button class="delete-btn action-btn" onclick="deleteRecord(${r.id})">删除</button></div></div>`;
                }
            });
            let totalRevenue=0,totalCost=0;
            filtered.forEach(r=>{totalRevenue+=r.salePrice*r.quantity;totalCost+=r.costPrice*r.quantity});
            const totalProfit=totalRevenue-totalCost;
            if(currentRole==='staff'){html+=`<div class="summary-row staff-view"><div class="summary-label">合计 (${filtered.length}笔)</div><div>-</div><div>-</div><div class="record-price">¥${totalRevenue.toFixed(2)}</div><div>-</div></div>`;}
            else{html+=`<div class="summary-row"><div class="summary-label">合计 (${filtered.length}笔)</div><div>-</div><div class="record-price">¥${totalCost.toFixed(2)}</div><div>-</div><div class="record-price">¥${totalRevenue.toFixed(2)}</div><div class="record-profit ${totalProfit>=0?'profit-positive':'profit-negative'}">${totalProfit>=0?'+':''}¥${totalProfit.toFixed(2)}</div><div>-</div></div>`;}
            list.innerHTML=html;
        }

        function openEdit(id){
            if(currentRole!=='boss'){alert('只有老板才能修改成本');return}
            const record=records.find(r=>r.id===id);
            if(!record)return;
            editingId=id;
            document.getElementById('current-info').innerHTML=`<strong>${record.name}</strong><br>当前成本：¥${record.costPrice.toFixed(2)} | 售价：¥${record.salePrice.toFixed(2)} | 数量：${record.quantity}`;
            document.getElementById('edit-cost').value=record.costPrice;
            document.getElementById('edit-modal').classList.add('active');
            document.getElementById('edit-cost').focus();
        }

        function saveEdit(){
            const newCost=parseFloat(document.getElementById('edit-cost').value);
            if(isNaN(newCost)||newCost<0){alert('请输入有效的成本金额');return}
            const record=records.find(r=>r.id===editingId);
            if(record){record.costPrice=newCost;record.modifiedBy='boss';record.modifiedAt=new Date().toISOString();saveData();updateStats();updateTable();showToast('✅ 成本修改成功！')}
            closeModal();
        }

        function closeModal(){document.getElementById('edit-modal').classList.remove('active');editingId=null}
        document.getElementById('edit-modal').addEventListener('click',function(e){if(e.target===this)closeModal()});

        function getFilteredRecords(){
            const now=new Date(),today=new Date(now.getFullYear(),now.getMonth(),now.getDate());
            return records.filter(r=>{
                const rDate=new Date(r.date);
                if(currentFilter==='today')return rDate>=today;
                else if(currentFilter==='week'){const weekAgo=new Date(today.getTime()-7*24*60*60*1000);return rDate>=weekAgo}
                else if(currentFilter==='month')return rDate.getMonth()===now.getMonth()&&rDate.getFullYear()===now.getFullYear();
                return true;
            });
        }

        function filterDate(type){
            currentFilter=type;
            document.querySelectorAll('.date-filter button').forEach(btn=>btn.classList.remove('active'));
            document.getElementById('btn-'+type).classList.add('active');
            updateStats();updateTable();
        }

        function deleteRecord(id){if(confirm('确定删除这条记录吗？')){records=records.filter(r=>r.id!==id);saveData();updateStats();updateTable();showToast('🗑️ 已删除')}}

        function exportData(){
            const filtered=getFilteredRecords();
            if(filtered.length===0){alert('没有数据可导出');return}
            let csv='\uFEFF商品名称,分类,售价,成本,数量,日期,营业收入,成本合计,利润,录入者,修改者\n';
            filtered.forEach(r=>{const revenue=r.salePrice*r.quantity,cost=r.costPrice*r.quantity,profit=revenue-cost;csv+=`${r.name},${r.category},${r.salePrice},${r.costPrice},${r.quantity},${r.date},${revenue},${cost},${profit},${r.createdBy==='staff'?'店员':'老板'},${r.modifiedBy?'老板':''}\n`});
            let totalRevenue=0,totalCost=0;
            filtered.forEach(r=>{totalRevenue+=r.salePrice*r.quantity;totalCost+=r.costPrice*r.quantity});
            csv+=`合计,,,,,,${totalRevenue},${totalCost},${totalRevenue-totalCost},,\n`;
            const blob=new Blob([csv],{type:'text/csv;charset=utf-8;'}),link=document.createElement('a');
            link.href=URL.createObjectURL(blob);
            link.download=`销售记录_${new Date().toLocaleDateString()}.csv`;
            link.click();
            showToast('📥 导出成功！');
        }

        function clearAll(){
            if(currentRole!=='boss'){alert('只有老板才能清空数据');return}
            if(confirm('确定要清空所有数据吗？此操作不可恢复！')){records=[];saveData();updateStats();updateTable();showToast('🗑️ 已清空所有数据')}
        }

        function showToast(msg){
            const toast=document.createElement('div');
            toast.style.cssText='position:fixed;top:20px;left:50%;transform:translateX(-50%);background:#333;color:white;padding:12px 24px;border-radius:8px;z-index:1000;animation:slideDown 0.3s ease';
            toast.textContent=msg;
            document.body.appendChild(toast);
            setTimeout(()=>toast.remove(),2000);
        }

        document.getElementById('boss-password').addEventListener('keypress',function(e){if(e.key==='Enter')enterBoss()});
    </script>
</body>
</html>
