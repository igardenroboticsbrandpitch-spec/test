<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>客户管理应用</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f4f7f9; /* 柔和的浅灰色背景 */
        }
        .container {
            max-width: 1024px;
        }
        /* 简单的确认模态框样式 */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0,0,0,0.4);
        }
        .modal-content {
            background-color: #fefefe;
            margin: 15% auto;
            padding: 24px;
            border-radius: 12px;
            width: 80%;
            max-width: 400px;
            text-align: center;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
        }
        /* 自定义消息框样式 */
        #message-box {
            display: none;
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            padding: 12px 24px;
            border-radius: 8px;
            color: white;
            z-index: 1000;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            animation: fadeout 3s forwards;
        }
        @keyframes fadeout {
            0% { opacity: 1; }
            80% { opacity: 1; }
            100% { opacity: 0; }
        }
    </style>
</head>
<body class="bg-gray-100 p-4 min-h-screen">
    <div id="app" class="container mx-auto bg-white shadow-xl rounded-2xl p-6 md:p-10 my-8">

        <!-- 客户汇总表视图 -->
        <div id="summary-view" class="space-y-8">
            <h1 class="text-3xl font-bold text-gray-800 text-center mb-6">客户汇总表</h1>
            <p id="user-info" class="text-sm text-gray-500 text-center">
                <span id="auth-status" class="font-semibold text-blue-500">正在连接...</span> | 
                用户ID: <span id="user-id-display">未认证</span>
            </p>

            <!-- 搜索框和导入/导出按钮 -->
            <div class="flex flex-col md:flex-row items-center justify-between space-y-4 md:space-y-0 md:space-x-4 mb-6">
                <input type="text" id="search-input" placeholder="搜索客户姓名、联系人或行业..."
                       class="w-full md:flex-grow p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                <div class="flex space-x-2">
                    <button id="export-button" class="bg-blue-600 text-white font-bold py-3 px-6 rounded-xl hover:bg-blue-700 transition-colors duration-200 ease-in-out shadow-lg whitespace-nowrap">
                        导出客户列表
                    </button>
                    <label for="import-file-input" class="bg-green-600 text-white font-bold py-3 px-6 rounded-xl hover:bg-green-700 transition-colors duration-200 ease-in-out shadow-lg whitespace-nowrap cursor-pointer">
                        导入客户列表
                    </label>
                    <input type="file" id="import-file-input" accept=".csv" class="hidden">
                </div>
            </div>

            <!-- 添加/编辑客户表单 -->
            <div class="bg-blue-50 p-6 rounded-xl shadow-inner">
                <h2 id="form-title" class="text-2xl font-semibold text-gray-700 mb-4">添加新客户</h2>
                <form id="customer-form" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
                    <input type="hidden" id="customer-id-input">
                    <input type="text" id="customer-name" placeholder="客户姓名/公司名" required
                           class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <input type="text" id="contact-person" placeholder="联系人"
                           class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <input type="text" id="position" placeholder="职位"
                           class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <input type="tel" id="phone" placeholder="电话"
                           class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <input type="email" id="email" placeholder="邮箱"
                           class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <input type="text" id="nationality" placeholder="国籍"
                           class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <input type="text" id="industry" placeholder="行业"
                           class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    
                    <!-- 新增头像上传和背景介绍字段 -->
                    <div class="lg:col-span-2">
                        <label for="customer-avatar" class="block text-sm font-medium text-gray-700 mb-1">头像 (可选)</label>
                        <input type="file" id="customer-avatar" accept="image/*"
                               class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div class="lg:col-span-2">
                        <label for="background-info" class="block text-sm font-medium text-gray-700 mb-1">背景介绍 (可选)</label>
                        <textarea id="background-info" rows="3"
                                  class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"></textarea>
                    </div>

                    <div class="md:col-span-2 lg:col-span-4 flex justify-between items-center space-x-4">
                        <button type="submit" id="submit-btn"
                                class="w-full bg-blue-600 text-white font-bold py-3 px-6 rounded-xl hover:bg-blue-700 transition-colors duration-200 ease-in-out shadow-lg">
                            添加客户
                        </button>
                        <button type="button" id="cancel-edit-btn" class="hidden w-1/4 bg-gray-300 text-gray-800 font-bold py-3 px-6 rounded-xl hover:bg-gray-400 transition-colors duration-200 ease-in-out">
                            取消
                        </button>
                    </div>
                </form>
            </div>

            <!-- 客户列表 -->
            <div class="bg-white p-6 rounded-xl shadow-lg">
                <h2 class="text-2xl font-semibold text-gray-700 mb-4">客户列表</h2>
                <div class="overflow-x-auto">
                    <table class="min-w-full divide-y divide-gray-200">
                        <thead class="bg-gray-50">
                            <tr>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">头像</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">客户姓名</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">联系人</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">职位</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">电话</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">邮箱</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">国籍</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">行业</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">客户等级</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">最近跟进日期</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">跟进状态</th>
                                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">操作</th>
                            </tr>
                        </thead>
                        <tbody id="customer-table-body" class="bg-white divide-y divide-gray-200">
                            <!-- 数据将由 JavaScript 动态生成 -->
                        </tbody>
                    </table>
                </div>
                <p id="no-customers-message" class="text-center text-gray-500 mt-4 hidden">暂无客户信息。</p>
            </div>
        </div>

        <!-- 客户跟进详情表视图 -->
        <div id="detail-view" class="hidden space-y-8">
            <div class="flex items-center space-x-4 mb-6">
                <button id="back-button" class="bg-gray-200 text-gray-700 font-bold py-2 px-4 rounded-xl hover:bg-gray-300 transition-colors duration-200 ease-in-out shadow-md">
                    &larr; 返回
                </button>
                <h1 id="detail-title" class="text-3xl font-bold text-gray-800"></h1>
            </div>

            <!-- 客户基础信息 -->
            <div class="bg-blue-50 p-6 rounded-xl shadow-inner">
                <h2 class="text-2xl font-semibold text-gray-700 mb-4">客户基础信息</h2>
                <div class="flex items-center mb-6">
                    <img id="detail-avatar" class="w-24 h-24 rounded-full object-cover border-4 border-white shadow-lg" src="" alt="客户头像">
                    <div class="ml-6 grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 text-gray-700 flex-grow">
                        <p><strong>客户姓名：</strong><span id="detail-name"></span></p>
                        <p><strong>联系人：</strong><span id="detail-contact"></span></p>
                        <p><strong>职位：</strong><span id="detail-position"></span></p>
                        <p><strong>电话：</strong><span id="detail-phone"></span></p>
                        <p><strong>邮箱：</strong><span id="detail-email"></span></p>
                        <p><strong>国籍：</strong><span id="detail-nationality"></span></p>
                        <p><strong>行业：</strong><span id="detail-industry"></span></p>
                        <p><strong>客户等级：</strong><span id="detail-level"></span></p>
                    </div>
                </div>
                <div class="mt-4">
                    <p><strong>背景介绍：</strong></p>
                    <p id="detail-background-info" class="mt-2 text-gray-600 whitespace-pre-wrap"></p>
                </div>
            </div>

            <!-- 跟进记录 -->
            <div class="bg-white p-6 rounded-xl shadow-lg">
                <h2 class="text-2xl font-semibold text-gray-700 mb-4">跟进记录</h2>

                <!-- 添加跟进记录表单 -->
                <form id="add-followup-form" class="mb-6 space-y-4">
                    <input type="date" id="followup-date" required
                           class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    <textarea id="followup-notes" placeholder="跟进内容摘要" required
                              class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500" rows="3"></textarea>
                    
                    <!-- 媒体上传字段 -->
                    <label for="followup-media" class="block text-gray-700 font-semibold mt-2">上传图片/视频 (可选，最大100MB)</label>
                    <input type="file" id="followup-media" accept="image/*,video/*"
                           class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    
                    <button type="submit"
                            class="w-full bg-blue-500 text-white font-bold py-3 px-6 rounded-xl hover:bg-blue-600 transition-colors duration-200 ease-in-out">
                        添加跟进记录
                    </button>
                </form>

                <!-- 跟进记录列表 -->
                <div id="followup-list" class="space-y-4">
                    <!-- 跟进记录将由 JavaScript 动态生成 -->
                </div>
                <p id="no-followups-message" class="text-center text-gray-500 mt-4 hidden">暂无跟进记录。</p>
            </div>
        </div>

        <!-- 确认删除模态框 -->
        <div id="delete-modal" class="modal">
            <div class="modal-content">
                <p class="text-lg font-semibold text-gray-800 mb-4">确定要删除此客户吗？</p>
                <div class="flex justify-center space-x-4">
                    <button id="confirm-delete" class="bg-red-500 text-white font-bold py-2 px-4 rounded-lg hover:bg-red-600 transition-colors">
                        确定
                    </button>
                    <button id="cancel-delete" class="bg-gray-300 text-gray-800 font-bold py-2 px-4 rounded-lg hover:bg-gray-400 transition-colors">
                        取消
                    </button>
                </div>
            </div>
        </div>
        
        <!-- 自定义消息框 -->
        <div id="message-box" class="bg-green-500"></div>

    </div>

    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, onAuthStateChanged, signInWithCustomToken } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, doc, collection, onSnapshot, addDoc, updateDoc, deleteDoc, query, where, getDocs, getDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";
        
        // 此处的 firebaseConfig 由 Canvas 环境自动注入，请勿手动修改。
        // 如果您在本地运行，需要在这里手动粘贴您的 firebaseConfig 对象。
        // 例如：const firebaseConfig = { apiKey: "...", ... };
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = JSON.parse(typeof __firebase_config !== 'undefined' ? __firebase_config : '{}');
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

        // 获取DOM元素
        const summaryView = document.getElementById('summary-view');
        const detailView = document.getElementById('detail-view');
        const customerForm = document.getElementById('customer-form');
        const customerTableBody = document.getElementById('customer-table-body');
        const addFollowupForm = document.getElementById('add-followup-form');
        const followupList = document.getElementById('followup-list');
        const backButton = document.getElementById('back-button');
        const detailTitle = document.getElementById('detail-title');
        const noCustomersMessage = document.getElementById('no-customers-message');
        const noFollowupsMessage = document.getElementById('no-followups-message');
        const searchInput = document.getElementById('search-input');
        const deleteModal = document.getElementById('delete-modal');
        const confirmDeleteBtn = document.getElementById('confirm-delete');
        const cancelDeleteBtn = document.getElementById('cancel-delete');
        const customerAvatarInput = document.getElementById('customer-avatar');
        const backgroundInfoInput = document.getElementById('background-info');
        const detailAvatar = document.getElementById('detail-avatar');
        const detailBackgroundInfo = document.getElementById('detail-background-info');
        const formTitle = document.getElementById('form-title');
        const submitBtn = document.getElementById('submit-btn');
        const customerIdInput = document.getElementById('customer-id-input');
        const cancelEditBtn = document.getElementById('cancel-edit-btn');
        const messageBox = document.getElementById('message-box');
        const userIdDisplay = document.getElementById('user-id-display');
        const authStatusDisplay = document.getElementById('auth-status');
        const exportButton = document.getElementById('export-button');
        const importFileInput = document.getElementById('import-file-input');

        // 定义常量和全局变量
        const MAX_FILE_SIZE = 100 * 1024 * 1024; // 100MB
        let db;
        let auth;
        let customersData = [];
        let followUpsData = [];
        let currentCustomerId = null;
        let customerToDeleteId = null;

        // 用于生成字母头像的SVG
        function generateAvatarSVG(name) {
            const firstLetter = name.charAt(0).toUpperCase();
            const colors = ['#3b82f6', '#10b981', '#f59e0b', '#ef4444', '#8b5cf6', '#0ea5e9'];
            const color = colors[firstLetter.charCodeAt(0) % colors.length];
            return `<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100" class="w-full h-full"><circle cx="50" cy="50" r="50" fill="${color}"/><text x="50%" y="50%" dominant-baseline="central" text-anchor="middle" font-size="50" fill="white" font-weight="bold">${firstLetter}</text></svg>`;
        }

        // 自定义消息提示
        const showMessage = (text, type = 'success') => {
            messageBox.textContent = text;
            messageBox.style.display = 'block';
            if (type === 'success') {
                messageBox.className = 'bg-green-500';
            } else if (type === 'error') {
                messageBox.className = 'bg-red-500';
            }
            // 隐藏消息框
            setTimeout(() => {
                messageBox.style.display = 'none';
            }, 3000);
        };

        // Firebase 初始化和认证
        const initializeFirebase = async () => {
            // 修复：如果配置为空，则不初始化 Firebase
            if (Object.keys(firebaseConfig).length === 0) {
                console.error("Firebase config is missing. Data will not be persisted.");
                showMessage("无法连接到数据库，请在代码中填写 Firebase 配置。", "error");
                authStatusDisplay.textContent = '连接失败';
                authStatusDisplay.className = 'font-semibold text-red-500';
                return;
            }

            const app = initializeApp(firebaseConfig);
            db = getFirestore(app);
            auth = getAuth(app);

            // 修复：使用提供的令牌进行认证
            try {
                if (initialAuthToken) {
                    await signInWithCustomToken(auth, initialAuthToken);
                } else {
                    await signInAnonymously(auth);
                }
            } catch (error) {
                console.error("Firebase 认证失败: ", error);
                showMessage("认证失败，请刷新页面", "error");
                authStatusDisplay.textContent = '认证失败';
                authStatusDisplay.className = 'font-semibold text-red-500';
            }
            
            onAuthStateChanged(auth, async (user) => {
                if (user) {
                    userIdDisplay.textContent = user.uid;
                    authStatusDisplay.textContent = '连接成功';
                    authStatusDisplay.className = 'font-semibold text-green-500';
                    listenForData();
                } else {
                    userIdDisplay.textContent = '未认证';
                    console.log("用户未登录");
                }
            });
        };

        // 监听 Firestore 数据变化
        const listenForData = () => {
            if (!auth.currentUser) return;
            const userId = auth.currentUser.uid;
            const customersRef = collection(db, `artifacts/${appId}/users/${userId}/customers`);
            const followupsRef = collection(db, `artifacts/${appId}/users/${userId}/followups`);

            // 监听客户数据
            onSnapshot(customersRef, (snapshot) => {
                customersData = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
                renderSummaryTable();
            }, (error) => {
                console.error("监听客户数据失败: ", error);
                showMessage("获取客户数据失败", "error");
            });

            // 监听跟进数据
            onSnapshot(followupsRef, (snapshot) => {
                followUpsData = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
                // Re-render summary table to update latest follow-up date and level
                renderSummaryTable();
                if (currentCustomerId) {
                    renderDetailView();
                }
            }, (error) => {
                console.error("监听跟进数据失败: ", error);
                showMessage("获取跟进数据失败", "error");
            });
        };

        // 根据跟进记录数量计算客户等级
        const calculateCustomerLevel = (customerId) => {
            const count = followUpsData.filter(f => f.customerId === customerId).length;
            if (count > 5) {
                return 'A';
            } else if (count > 2) {
                return 'B';
            } else if (count > 0) {
                return 'C';
            } else {
                return '新客户';
            }
        };

        // 渲染客户汇总表
        const renderSummaryTable = (filteredCustomers = null) => {
            customerTableBody.innerHTML = '';
            const displayCustomers = filteredCustomers || customersData;
            
            if (displayCustomers.length === 0) {
                noCustomersMessage.classList.remove('hidden');
            } else {
                noCustomersMessage.classList.add('hidden');
                
                // 按最近跟进日期排序
                const sortedCustomers = displayCustomers.sort((a, b) => {
                    const latestFollowUpA = followUpsData.filter(f => f.customerId === a.id)
                                                         .sort((x, y) => new Date(y.date) - new Date(x.date))[0];
                    const latestFollowUpB = followUpsData.filter(f => f.customerId === b.id)
                                                         .sort((x, y) => new Date(y.date) - new Date(x.date))[0];
                    const dateA = latestFollowUpA ? new Date(latestFollowUpA.date) : new Date(0);
                    const dateB = latestFollowUpB ? new Date(latestFollowUpB.date) : new Date(0);
                    return dateB - dateA;
                });

                sortedCustomers.forEach(customer => {
                    const latestFollowUp = followUpsData.filter(f => f.customerId === customer.id)
                                                   .sort((a, b) => new Date(b.date) - new Date(a.date))[0];
                    const customerLevel = calculateCustomerLevel(customer.id);
                    
                    const row = document.createElement('tr');
                    row.className = 'hover:bg-gray-50 transition-colors duration-100 ease-in-out';
                    
                    const avatarHtml = customer.avatarData 
                        ? `<img src="${customer.avatarData}" class="w-10 h-10 rounded-full object-cover">`
                        : `<div class="w-10 h-10 flex items-center justify-center rounded-full bg-gray-200 text-gray-500">${generateAvatarSVG(customer.name)}</div>`;
                    
                    row.innerHTML = `
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                            ${avatarHtml}
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-blue-600 hover:text-blue-800 cursor-pointer transition-colors duration-100 ease-in-out">
                            ${customer.name}
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${customer.contact || 'N/A'}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${customer.position || 'N/A'}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${customer.phone || 'N/A'}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${customer.email || 'N/A'}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${customer.nationality || 'N/A'}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${customer.industry || 'N/A'}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${customerLevel}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${latestFollowUp ? latestFollowUp.date : '暂无'}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">${latestFollowUp ? '已跟进' : '新客户'}</td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm space-x-2">
                            <button class="edit-btn text-blue-600 hover:text-blue-800 transition-colors" data-id="${customer.id}">编辑</button>
                            <button class="delete-btn text-red-600 hover:text-red-800 transition-colors" data-id="${customer.id}">删除</button>
                        </td>
                    `;
                    customerTableBody.appendChild(row);

                    // 为客户姓名添加点击事件
                    row.querySelector('td:nth-child(2)').addEventListener('click', () => {
                        showDetailView(customer.id);
                    });
                });

                // 为每个删除和编辑按钮添加事件监听器
                document.querySelectorAll('.delete-btn').forEach(button => {
                    button.addEventListener('click', (e) => {
                        customerToDeleteId = e.target.dataset.id;
                        deleteModal.style.display = 'block';
                    });
                });
                document.querySelectorAll('.edit-btn').forEach(button => {
                    button.addEventListener('click', (e) => {
                        const customerId = e.target.dataset.id;
                        const customer = customersData.find(c => c.id === customerId);
                        if (customer) {
                            populateFormForEdit(customer);
                        }
                    });
                });
            }
        };

        // 渲染客户详情表
        const renderDetailView = () => {
            const customer = customersData.find(c => c.id === currentCustomerId);
            if (!customer) return;

            // 自动填充客户基础信息
            document.getElementById('detail-title').textContent = customer.name;
            document.getElementById('detail-name').textContent = customer.name;
            document.getElementById('detail-contact').textContent = customer.contact || 'N/A';
            document.getElementById('detail-position').textContent = customer.position || 'N/A';
            document.getElementById('detail-phone').textContent = customer.phone || 'N/A';
            document.getElementById('detail-email').textContent = customer.email || 'N/A';
            document.getElementById('detail-nationality').textContent = customer.nationality || 'N/A';
            document.getElementById('detail-industry').textContent = customer.industry || 'N/A';
            document.getElementById('detail-level').textContent = calculateCustomerLevel(customer.id);
            document.getElementById('detail-background-info').textContent = customer.backgroundInfo || 'N/A';

            // 设置头像
            if (customer.avatarData) {
                detailAvatar.src = customer.avatarData;
                detailAvatar.classList.remove('hidden');
            } else {
                detailAvatar.src = `data:image/svg+xml;base64,${btoa(generateAvatarSVG(customer.name))}`;
                detailAvatar.classList.remove('hidden');
            }

            // 渲染跟进记录
            followupList.innerHTML = '';
            const customerFollowUps = followUpsData.filter(f => f.customerId === currentCustomerId)
                                               .sort((a, b) => new Date(b.date) - new Date(a.date));
            
            if (customerFollowUps.length === 0) {
                noFollowupsMessage.classList.remove('hidden');
            } else {
                noFollowupsMessage.classList.add('hidden');
                customerFollowUps.forEach(followUp => {
                    const item = document.createElement('div');
                    item.className = 'bg-gray-100 p-4 rounded-lg shadow-sm space-y-2';
                    let mediaHtml = '';

                    // 根据媒体类型渲染不同的标签
                    if (followUp.mediaData) {
                        if (followUp.mediaType === 'image') {
                            mediaHtml = `<img src="${followUp.mediaData}" class="mt-2 rounded-lg max-w-full h-auto border border-gray-300">`;
                        } else if (followUp.mediaType === 'video') {
                            mediaHtml = `<video controls src="${followUp.mediaData}" class="mt-2 rounded-lg max-w-full h-auto border border-gray-300"></video>`;
                        }
                    }
                    
                    item.innerHTML = `
                        <p class="text-sm font-semibold text-gray-600">${followUp.date}</p>
                        <p class="text-gray-800">${followUp.notes}</p>
                        ${mediaHtml}
                    `;
                    followupList.appendChild(item);
                });
            }
        };

        // 切换到客户详情视图
        const showDetailView = (customerId) => {
            currentCustomerId = customerId;
            summaryView.classList.add('hidden');
            detailView.classList.remove('hidden');
            renderDetailView();
        };

        // 换回客户汇总视图
        const showSummaryView = () => {
            currentCustomerId = null;
            detailView.classList.add('hidden');
            summaryView.classList.remove('hidden');
        };

        // 填充表单以进行编辑
        const populateFormForEdit = (customer) => {
            formTitle.textContent = "编辑客户信息";
            submitBtn.textContent = "更新客户";
            customerIdInput.value = customer.id;
            document.getElementById('customer-name').value = customer.name;
            document.getElementById('contact-person').value = customer.contact || '';
            document.getElementById('position').value = customer.position || '';
            document.getElementById('phone').value = customer.phone || '';
            document.getElementById('email').value = customer.email || '';
            document.getElementById('nationality').value = customer.nationality || '';
            document.getElementById('industry').value = customer.industry || '';
            backgroundInfoInput.value = customer.backgroundInfo || '';
            document.getElementById('customer-name').disabled = true; // 禁用姓名编辑，因为它是唯一标识
            cancelEditBtn.classList.remove('hidden');
        };

        // 重置表单为添加模式
        const resetForm = () => {
            formTitle.textContent = "添加新客户";
            submitBtn.textContent = "添加客户";
            customerIdInput.value = '';
            document.getElementById('customer-name').disabled = false;
            customerForm.reset();
            cancelEditBtn.classList.add('hidden');
        };

        // 处理客户表单提交
        customerForm.addEventListener('submit', async (e) => {
            e.preventDefault();

            if (!auth.currentUser) {
                showMessage("用户未认证，无法进行操作", "error");
                return;
            }

            const isEditing = !!customerIdInput.value;
            const name = document.getElementById('customer-name').value.trim();
            const contact = document.getElementById('contact-person').value.trim();
            const position = document.getElementById('position').value.trim();
            const phone = document.getElementById('phone').value.trim();
            const email = document.getElementById('email').value.trim();
            const nationality = document.getElementById('nationality').value.trim();
            const industry = document.getElementById('industry').value.trim();
            const backgroundInfo = backgroundInfoInput.value.trim();
            const avatarFile = customerAvatarInput.files[0];

            if (!name) {
                showMessage('客户姓名不能为空！', 'error');
                return;
            }

            const customerData = {
                name, contact, position, phone, email, nationality, industry, backgroundInfo
            };

            const processForm = async (data) => {
                const userId = auth.currentUser.uid;
                const customersRef = collection(db, `artifacts/${appId}/users/${userId}/customers`);
                
                if (isEditing) {
                    try {
                        await updateDoc(doc(db, customersRef.path, customerIdInput.value), data);
                        showMessage("客户信息更新成功！");
                        resetForm();
                    } catch (error) {
                        console.error("更新客户信息失败: ", error);
                        showMessage("更新客户信息失败", "error");
                    }
                } else {
                    try {
                         // 检查客户姓名是否已存在
                        const q = query(customersRef, where("name", "==", name));
                        const querySnapshot = await getDocs(q);
                        if (!querySnapshot.empty) {
                            showMessage('客户姓名已存在！', 'error');
                            return;
                        }

                        await addDoc(customersRef, data);
                        showMessage("新客户添加成功！");
                        customerForm.reset();
                    } catch (error) {
                        console.error("添加客户失败: ", error);
                        showMessage("添加客户失败", "error");
                    }
                }
            };
            
            // 处理头像文件上传
            if (avatarFile) {
                const reader = new FileReader();
                reader.onload = async (e) => {
                    customerData.avatarData = e.target.result;
                    await processForm(customerData);
                };
                reader.readAsDataURL(avatarFile);
            } else {
                await processForm(customerData);
            }
        });

        // 处理跟进记录表单提交
        addFollowupForm.addEventListener('submit', async (e) => {
            e.preventDefault();

            if (!auth.currentUser) {
                showMessage("用户未认证，无法进行操作", "error");
                return;
            }

            const date = document.getElementById('followup-date').value;
            const notes = document.getElementById('followup-notes').value.trim();
            const mediaFile = document.getElementById('followup-media').files[0];
            
            if (date && notes && currentCustomerId) {
                const followUpData = {
                    customerId: currentCustomerId,
                    date,
                    notes,
                    mediaData: null,
                    mediaType: null
                };

                const processFollowUp = async (data) => {
                    const userId = auth.currentUser.uid;
                    const followupsRef = collection(db, `artifacts/${appId}/users/${userId}/followups`);
                    try {
                        await addDoc(followupsRef, data);
                        showMessage("跟进记录添加成功！");
                        addFollowupForm.reset();
                    } catch (error) {
                        console.error("添加跟进记录失败: ", error);
                        showMessage("添加跟进记录失败", "error");
                    }
                };

                if (mediaFile) {
                    if (mediaFile.size > MAX_FILE_SIZE) {
                        showMessage(`文件大小不能超过${MAX_FILE_SIZE / 1024 / 1024}MB！`, 'error');
                        return;
                    }
                    const reader = new FileReader();
                    reader.onload = async function(e) {
                        followUpData.mediaData = e.target.result;
                        followUpData.mediaType = mediaFile.type.startsWith('image/') ? 'image' : 'video';
                        await processFollowUp(followUpData);
                    };
                    reader.readAsDataURL(mediaFile);
                } else {
                    await processFollowUp(followUpData);
                }
            } else {
                showMessage('日期和跟进内容不能为空！', 'error');
            }
        });

        // 返回按钮事件
        backButton.addEventListener('click', () => {
            showSummaryView();
        });

        // 搜索功能
        searchInput.addEventListener('input', (e) => {
            const query = e.target.value.toLowerCase();
            const filteredCustomers = customersData.filter(customer => {
                return customer.name.toLowerCase().includes(query) ||
                       (customer.contact && customer.contact.toLowerCase().includes(query)) ||
                       (customer.industry && customer.industry.toLowerCase().includes(query));
            });
            renderSummaryTable(filteredCustomers);
        });

        // 取消编辑按钮事件
        cancelEditBtn.addEventListener('click', () => {
            resetForm();
        });

        // 模态框删除功能
        confirmDeleteBtn.addEventListener('click', async () => {
            if (!auth.currentUser || !customerToDeleteId) {
                deleteModal.style.display = 'none';
                return;
            }
            const userId = auth.currentUser.uid;
            const customerDocRef = doc(db, `artifacts/${appId}/users/${userId}/customers`, customerToDeleteId);
            
            try {
                // 删除客户文档
                await deleteDoc(customerDocRef);
                
                // 删除所有相关的跟进记录
                const q = query(collection(db, `artifacts/${appId}/users/${userId}/followups`), where("customerId", "==", customerToDeleteId));
                const querySnapshot = await getDocs(q);
                const deletePromises = querySnapshot.docs.map(d => deleteDoc(d.ref));
                await Promise.all(deletePromises);

                showMessage("客户及所有跟进记录删除成功！");
                deleteModal.style.display = 'none';
                customerToDeleteId = null;
            } catch (error) {
                console.error("删除客户失败: ", error);
                showMessage("删除客户失败", "error");
            }
        });

        // 取消删除
        cancelDeleteBtn.addEventListener('click', () => {
            deleteModal.style.display = 'none';
            customerToDeleteId = null;
        });

        // ----- 新增的导入/导出功能 -----

        // 导出功能
        exportButton.addEventListener('click', () => {
            if (customersData.length === 0) {
                showMessage('没有客户数据可以导出！', 'error');
                return;
            }
            // 定义 CSV 文件的头部
            const headers = ["客户姓名", "联系人", "职位", "电话", "邮箱", "国籍", "行业", "客户等级", "最近跟进日期", "跟进状态", "背景介绍"];
            
            // 准备数据行
            const rows = customersData.map(customer => {
                const latestFollowUp = followUpsData.filter(f => f.customerId === customer.id)
                                                    .sort((a, b) => new Date(b.date) - new Date(a.date))[0];
                const customerLevel = calculateCustomerLevel(customer.id);
                const followUpDate = latestFollowUp ? latestFollowUp.date : '暂无';
                const followUpStatus = latestFollowUp ? '已跟进' : '新客户';

                // 将数据转换为字符串数组
                const data = [
                    customer.name || '',
                    customer.contact || '',
                    customer.position || '',
                    customer.phone || '',
                    customer.email || '',
                    customer.nationality || '',
                    customer.industry || '',
                    customerLevel,
                    followUpDate,
                    followUpStatus,
                    customer.backgroundInfo || ''
                ];
                // 确保数据中的逗号被正确处理
                return data.map(item => `"${item.toString().replace(/"/g, '""')}"`).join(',');
            });

            // 拼接头部和数据行
            const csvContent = "\ufeff" + headers.join(',') + "\n" + rows.join("\n");
            
            // 创建 Blob 对象并下载
            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement("a");
            if (link.download !== undefined) {
                const url = URL.createObjectURL(blob);
                link.setAttribute("href", url);
                link.setAttribute("download", "客户汇总表.csv");
                link.style.visibility = 'hidden';
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
                showMessage('客户数据导出成功！');
            }
        });

        // 导入功能
        importFileInput.addEventListener('change', (e) => {
            const file = e.target.files[0];
            if (!file) {
                return;
            }
            if (file.type !== 'text/csv' && !file.name.endsWith('.csv')) {
                showMessage('请选择一个有效的CSV文件！', 'error');
                return;
            }
            
            const reader = new FileReader();
            reader.onload = async function(event) {
                try {
                    const csvContent = event.target.result;
                    const lines = csvContent.split(/\r\n|\n/).filter(line => line.trim() !== '');
                    
                    if (lines.length <= 1) {
                        showMessage('CSV文件为空或格式不正确！', 'error');
                        return;
                    }
                    
                    const headers = lines[0].split(/,(?=(?:(?:[^"]*"){2})*[^"]*$)/).map(h => h.trim().replace(/^"|"$/g, '').replace(/""/g, '"'));
                    const customerNameIndex = headers.indexOf("客户姓名");
                    
                    if (customerNameIndex === -1) {
                        showMessage('CSV文件中缺少“客户姓名”列！', 'error');
                        return;
                    }
                    
                    let importedCount = 0;
                    const newCustomers = [];
                    for (let i = 1; i < lines.length; i++) {
                        const values = lines[i].split(/,(?=(?:(?:[^"]*"){2})*[^"]*$)/).map(v => v.trim().replace(/^"|"$/g, '').replace(/""/g, '"'));
                        if (values.length !== headers.length) {
                             console.warn(`跳过第${i+1}行，列数不匹配: ${lines[i]}`);
                             continue;
                        }
                        
                        const customerData = {};
                        headers.forEach((header, index) => {
                            customerData[header] = values[index];
                        });
                        
                        const name = customerData['客户姓名'];
                        if (!name) {
                            console.warn(`跳过第${i+1}行，客户姓名为空`);
                            continue;
                        }
                        
                        // 检查是否已存在同名客户
                        const exists = customersData.some(c => c.name === name);
                        if (exists) {
                            console.warn(`客户“${name}”已存在，跳过导入。`);
                            continue;
                        }

                        // 将中文表头映射到英文字段
                        const newCustomer = {
                            name: customerData['客户姓名'],
                            contact: customerData['联系人'] || null,
                            position: customerData['职位'] || null,
                            phone: customerData['电话'] || null,
                            email: customerData['邮箱'] || null,
                            nationality: customerData['国籍'] || null,
                            industry: customerData['行业'] || null,
                            backgroundInfo: customerData['背景介绍'] || null,
                        };
                        newCustomers.push(newCustomer);
                    }
                    
                    if (newCustomers.length > 0) {
                        const userId = auth.currentUser.uid;
                        const customersRef = collection(db, `artifacts/${appId}/users/${userId}/customers`);
                        const batchPromises = newCustomers.map(c => addDoc(customersRef, c));
                        await Promise.all(batchPromises);
                        showMessage(`成功导入了 ${newCustomers.length} 位客户！`);
                    } else {
                        showMessage('没有新客户可以导入。', 'error');
                    }
                } catch (error) {
                    console.error("导入文件失败: ", error);
                    showMessage("导入文件失败，请检查文件格式。", "error");
                } finally {
                    importFileInput.value = ''; // 重置文件输入，以便可以再次上传相同文件
                }
            };
            reader.readAsText(file, 'UTF-8');
        });


        // 页面初始化
        initializeFirebase();
    </script>
</body>
</html>
