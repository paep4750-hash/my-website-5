<!DOCTYPE html>
<html lang="th" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Server Support & Console</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        darkBg: '#0b0f19',
                        darkCard: '#111827',
                        darkSidebar: '#0d111d',
                        darkInput: '#1f2937',
                        accent: '#3b82f6',
                        secretAccent: '#6366f1'
                    }
                }
            }
        }
    </script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #0b0f19;
        }
        ::-webkit-scrollbar-thumb {
            background: #374151;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #4b5563;
        }
    </style>
</head>
<body class="bg-darkBg text-gray-200 font-sans antialiased h-screen flex flex-col md:flex-row overflow-hidden select-none">

    <!-- Mobile Header -->
    <div class="md:hidden bg-darkSidebar border-b border-gray-800 p-4 flex justify-between items-center z-20">
        <div>
            <div id="mobileDateHeader" class="text-xs text-gray-400 font-mono">กำลังโหลด...</div>
            <button id="mobileSecretBtn" class="text-sm font-semibold text-gray-200 hover:text-white transition flex items-center gap-2 mt-0.5">
                <i class="fa-solid me-1 fa-headset text-blue-500"></i> ติดต่อผู้สร้างเซิร์ฟเวอร์
            </button>
        </div>
        <button id="toggleMenuBtn" class="text-gray-400 hover:text-white p-2">
            <i class="fa-solid fa-bars text-xl"></i>
        </button>
    </div>

    <!-- Sidebar -->
    <aside id="sidebar" class="fixed md:relative inset-y-0 left-0 transform -translate-x-full md:translate-x-0 transition duration-200 ease-in-out z-30 w-72 bg-darkSidebar border-r border-gray-800 flex flex-col h-full">
        <!-- Sidebar Header (หัวข้อเล็ก + หัวข้อใหญ่) -->
        <div class="p-4 border-b border-gray-800/80 bg-slate-950/40">
            <!-- หัวข้อเล็ก: วันนี้/แนววันนี้ -->
            <div class="flex items-center justify-between mb-1">
                <span id="currentDateDisplay" class="text-xs font-mono text-gray-400 tracking-wider">
                    <!-- แสดงวันที่ปัจจุบัน -->
                </span>
                <span class="text-[10px] bg-emerald-950 text-emerald-400 px-1.5 py-0.5 rounded border border-emerald-800/50">Online</span>
            </div>

            <!-- หัวข้อใหญ่: ติดต่อผู้สร้างเซิร์ฟเวอร์ (ปุ่มเปิด/สลับเข้าแชทลับ) -->
            <button id="secretHeaderBtn" class="w-full text-left group transition-all duration-200 p-2 -mx-2 rounded-lg hover:bg-gray-800/50 active:scale-[0.98]">
                <div class="text-base font-bold text-gray-100 group-hover:text-blue-400 flex items-center justify-between">
                    <span class="flex items-center gap-2">
                        <i class="fa-solid fa-headset text-blue-500 text-sm"></i>
                        ติดต่อผู้สร้างเซิร์ฟเวอร์
                    </span>
                    <i class="fa-solid fa-chevron-right text-xs text-gray-600 group-hover:text-blue-400 transition-transform group-hover:translate-x-1"></i>
                </div>
                <p class="text-[11px] text-gray-500 mt-0.5">ช่องทางติดต่อสอบถามและแจ้งปัญหา</p>
            </button>
        </div>

        <!-- Categories & Chat List (การจัดหมวดหมู่แชท) -->
        <div class="flex-1 overflow-y-auto p-3 space-y-4">
            <div>
                <div class="flex items-center justify-between px-2 mb-2 text-xs font-semibold text-gray-400 uppercase tracking-wider">
                    <span>หมวดหมู่แชทลับ</span>
                    <button id="addCategoryBtn" class="hover:text-blue-400 text-gray-500 transition p-1" title="เพิ่มหมวดหมู่ใหม่">
                        <i class="fa-solid fa-plus"></i>
                    </button>
                </div>
                
                <!-- Category List Dynamic Container -->
                <div id="categoryList" class="space-y-1">
                    <!-- จะถูกสร้างผ่าน JS -->
                </div>
            </div>
        </div>

        <!-- Sidebar Actions (ปุ่มล้างข้อมูลทั้งหมด) -->
        <div class="p-3 border-t border-gray-800 bg-darkSidebar/50">
            <button id="clearAllBtn" class="w-full flex items-center justify-center gap-2 py-2 px-3 text-xs text-red-400 hover:text-red-300 hover:bg-red-950/30 border border-red-900/30 hover:border-red-800/50 rounded-lg transition duration-150">
                <i class="fa-solid fa-trash-can"></i>
                <span>ล้างข้อมูลแชททั้งหมด</span>
            </button>
        </div>
    </aside>

    <!-- Main Chat Area -->
    <main class="flex-1 flex flex-col h-full bg-darkBg relative overflow-hidden">
        
        <!-- Top Chat Header -->
        <header class="bg-darkCard/80 backdrop-blur border-b border-gray-800 p-3 px-4 flex items-center justify-between">
            <div class="flex items-center gap-3">
                <div class="w-9 h-9 rounded-full bg-gradient-to-tr from-blue-600 to-indigo-600 flex items-center justify-center text-white font-bold shadow-lg shadow-blue-500/10">
                    <i class="fa-solid fa-[#fa-user-shield] fa-user-shield text-sm"></i>
                </div>
                <div>
                    <h2 id="currentCategoryTitle" class="text-sm font-semibold text-gray-100 flex items-center gap-2">
                        ติดต่อผู้สร้างเซิร์ฟเวอร์
                    </h2>
                    <span id="chatStatusText" class="text-[11px] text-gray-400">หมวดหมู่: ทั่วไป (บันทึกอัตโนมัติ)</span>
                </div>
            </div>

            <!-- Actions Header (ล้างเฉพาะแชทนี้) -->
            <div class="flex items-center gap-2">
                <button id="clearCurrentChatBtn" class="flex items-center gap-1.5 px-2.5 py-1.5 text-xs text-amber-400 hover:text-amber-300 bg-amber-950/20 hover:bg-amber-950/40 border border-amber-900/30 rounded-lg transition" title="ล้างข้อมูลแชทห้องนี้">
                    <i class="fa-solid fa-eraser"></i>
                    <span class="hidden sm:inline">ล้างข้อมูลแชทนี้</span>
                </button>
            </div>
        </header>

        <!-- Messages Container -->
        <div id="messagesContainer" class="flex-1 overflow-y-auto p-4 space-y-4">
            <!-- Messages will be populated here dynamically -->
        </div>

        <!-- Input Area -->
        <footer class="p-3 sm:p-4 bg-darkCard/90 border-t border-gray-800">
            <form id="chatForm" class="flex flex-col gap-2 max-w-4xl mx-auto">
                <!-- Message Role Switcher (สลับการพิมพ์: เราพิมพ์ หรือ ตอบกลับเรา) -->
                <div class="flex items-center justify-between text-xs text-gray-400 px-1">
                    <div class="flex items-center gap-2">
                        <span class="text-gray-500">บทบาทการพิมพ์:</span>
                        <div class="inline-flex rounded-lg p-0.5 bg-gray-800 border border-gray-700">
                            <button type="button" id="roleUserBtn" class="px-2.5 py-1 rounded-md text-[11px] font-medium transition bg-blue-600 text-white shadow">
                                <i class="fa-solid fa-user me-1"></i> ฝั่งเรา
                            </button>
                            <button type="button" id="roleOtherBtn" class="px-2.5 py-1 rounded-md text-[11px] font-medium transition text-gray-400 hover:text-gray-200">
                                <i class="fa-solid fa-reply me-1"></i> ตอบกลับเรา
                            </button>
                        </div>
                    </div>
                    <span class="text-[10px] text-gray-500 italic hidden sm:inline"><i class="fa-solid fa-lock me-1"></i> บันทึกลงไดรฟ์เครื่องอัตโนมัติ</span>
                </div>

                <!-- Input Box & Send Button -->
                <div class="flex gap-2 items-end">
                    <div class="relative flex-1">
                        <textarea id="messageInput" rows="1" placeholder="พิมพ์ข้อความที่ต้องการเก็บบันทึก..." class="w-full bg-darkInput text-gray-100 rounded-xl px-4 py-3 text-sm focus:outline-none focus:ring-2 focus:ring-blue-500 border border-gray-700/60 resize-none transition min-h-[46px] max-h-32 placeholder-gray-500"></textarea>
                    </div>
                    <button type="submit" class="h-[46px] px-5 bg-blue-600 hover:bg-blue-500 active:scale-95 text-white font-medium rounded-xl transition duration-150 flex items-center justify-center gap-2 shadow-lg shadow-blue-600/20">
                        <span>ส่ง</span>
                        <i class="fa-solid fa-paper-plane text-xs"></i>
                    </button>
                </div>
            </form>
        </footer>
    </main>

    <!-- Overlay for Mobile Sidebar -->
    <div id="sidebarOverlay" class="fixed inset-0 bg-black/60 z-20 hidden md:hidden"></div>

    <script>
        // --- State Management ---
        const STORAGE_KEY = 'DARK_CHAT_SECRET_DATA_V1';
        
        let appData = {
            categories: [
                { id: 'cat_general', name: 'ติดต่อผู้สร้างเซิร์ฟเวอร์', icon: 'fa-headset' },
                { id: 'cat_code', name: 'โน้ตโค้ดลับ', icon: 'fa-code' },
                { id: 'cat_personal', name: 'บันทึกส่วนตัว', icon: 'fa-user-secret' }
            ],
            activeCategoryId: 'cat_general',
            messages: {
                'cat_general': [
                    { id: 'm1', sender: 'other', text: 'สวัสดีครับ มีอะไรให้ผู้ดูแลเซิร์ฟเวอร์ช่วยเหลือไหมครับ?', time: '10:00 AM' },
                ]
            }
        };

        let currentRole = 'user'; // 'user' (ฝั่งเรา) หรือ 'other' (เข้าร่วม discord ติดตามข่าว)

        // --- DOM Elements ---
        const secretHeaderBtn = document.getElementById('secretHeaderBtn');
        const mobileSecretBtn = document.getElementById('mobileSecretBtn');
        const currentDateDisplay = document.getElementById('currentDateDisplay');
        const mobileDateHeader = document.getElementById('mobileDateHeader');
        const categoryList = document.getElementById('categoryList');
        const addCategoryBtn = document.getElementById('addCategoryBtn');
        const messagesContainer = document.getElementById('messagesContainer');
        const chatForm = document.getElementById('chatForm');
        const messageInput = document.getElementById('messageInput');
        const roleUserBtn = document.getElementById('roleUserBtn');
        const roleOtherBtn = document.getElementById('roleOtherBtn');
        const currentCategoryTitle = document.getElementById('currentCategoryTitle');
        const chatStatusText = document.getElementById('chatStatusText');
        const clearCurrentChatBtn = document.getElementById('clearCurrentChatBtn');
        const clearAllBtn = document.getElementById('clearAllBtn');
        const toggleMenuBtn = document.getElementById('toggleMenuBtn');
        const sidebar = document.getElementById('sidebar');
        const sidebarOverlay = document.getElementById('sidebarOverlay');

        // --- Init Function ---
        function init() {
            loadData();
            setupDateDisplay();
            renderCategories();
            renderMessages();
            setupEventListeners();
        }

        // --- Date Setup ---
        function setupDateDisplay() {
            const now = new Date();
            const options = { weekday: 'short', month: 'short', day: 'numeric', year: 'numeric' };
            const formattedDate = now.toLocaleDateString('th-TH', options);
            
            currentDateDisplay.textContent = `วันนี้: ${formattedDate}`;
            mobileDateHeader.textContent = formattedDate;
        }

        // --- LocalStorage Operations (บันทึกข้อมูลอัตโนมัติ) ---
        function saveData() {
            try {
                localStorage.setItem(STORAGE_KEY, JSON.stringify(appData));
            } catch (e) {
                console.error('Failed to save data to localStorage', e);
            }
        }

        function loadData() {
            try {
                const saved = localStorage.getItem(STORAGE_KEY);
                if (saved) {
                    appData = JSON.parse(saved);
                }
            } catch (e) {
                console.error('Failed to load data from localStorage', e);
            }
        }

        // --- Render UI Functions ---
        function renderCategories() {
            categoryList.innerHTML = '';

            appData.categories.forEach(cat => {
                const isActive = cat.id === appData.activeCategoryId;
                const button = document.createElement('button');
                button.className = `w-full flex items-center justify-between px-3 py-2.5 rounded-lg text-xs font-medium transition duration-150 ${
                    isActive 
                        ? 'bg-blue-600/20 text-blue-400 border border-blue-500/30' 
                        : 'text-gray-400 hover:bg-gray-800/60 hover:text-gray-200'
                }`;
                
                button.innerHTML = `
                    <div class="flex items-center gap-2 truncate">
                        <i class="fa-solid ${cat.icon || 'fa-comments'} text-xs"></i>
                        <span class="truncate">${escapeHtml(cat.name)}</span>
                    </div>
                    ${appData.categories.length > 1 ? `
                        <i class="fa-solid fa-xmark delete-cat-btn opacity-0 group-hover:opacity-100 hover:text-red-400 p-1 text-[10px]" data-id="${cat.id}" title="ลบหมวดหมู่นี้"></i>
                    ` : ''}
                `;

                button.addEventListener('click', (e) => {
                    if (e.target.classList.contains('delete-cat-btn')) {
                        e.stopPropagation();
                        deleteCategory(cat.id);
                        return;
                    }
                    switchCategory(cat.id);
                });

                categoryList.appendChild(button);
            });
        }

        function renderMessages() {
            messagesContainer.innerHTML = '';
            
            const currentCat = appData.categories.find(c => c.id === appData.activeCategoryId);
            if (currentCat) {
                currentCategoryTitle.innerHTML = `<i class="fa-solid ${currentCat.icon || 'fa-comments'} text-blue-500"></i> ${escapeHtml(currentCat.name)}`;
                chatStatusText.textContent = `หมวดหมู่: ${currentCat.name} (บันทึกข้อมูลแล้ว)`;
            }

            const currentMessages = appData.messages[appData.activeCategoryId] || [];

            if (currentMessages.length === 0) {
                messagesContainer.innerHTML = `
                    <div class="h-full flex flex-col items-center justify-center text-gray-500 text-xs py-12">
                        <div class="w-12 h-12 rounded-full bg-gray-800/50 flex items-center justify-center mb-2">
                            <i class="fa-solid fa-message text-gray-600 text-lg"></i>
                        </div>
                        <p>ยังไม่มีข้อความในหมวดหมู่นี้</p>
                        <p class="text-[11px] text-gray-600 mt-1">เริ่มพิมพ์ข้อความเพื่อบันทึกได้เลย</p>
                    </div>
                `;
                return;
            }

            currentMessages.forEach(msg => {
                const isUser = msg.sender === 'user';
                const msgWrapper = document.createElement('div');
                msgWrapper.className = `flex flex-col ${isUser ? 'items-end' : 'items-start'} space-y-1 mb-2`;

                msgWrapper.innerHTML = `
                    <div class="flex items-end gap-2 max-w-[85%] sm:max-w-[75%] ${isUser ? 'flex-row-reverse' : 'flex-row'}">
                        <div class="w-7 h-7 rounded-full flex items-center justify-center text-[10px] text-white flex-shrink-0 ${
                            isUser ? 'bg-blue-600' : 'bg-gray-700'
                        }">
                            <i class="fa-solid ${isUser ? 'fa-user' : 'fa-headset'}"></i>
                        </div>
                        <div class="rounded-2xl px-4 py-2.5 text-sm ${
                            isUser 
                                ? 'bg-blue-600 text-white rounded-br-xs' 
                                : 'bg-gray-800 text-gray-100 border border-gray-700/50 rounded-bl-xs'
                        } whitespace-pre-wrap break-words shadow-sm">
                            ${escapeHtml(msg.text)}
                        </div>
                    </div>
                    <span class="text-[10px] text-gray-500 px-9">${msg.time}</span>
                `;

                messagesContainer.appendChild(msgWrapper);
            });

            // Auto scroll to bottom
            messagesContainer.scrollTop = messagesContainer.scrollHeight;
        }

        // --- Category Management ---
        function switchCategory(catId) {
            appData.activeCategoryId = catId;
            saveData();
            renderCategories();
            renderMessages();
            closeMobileSidebar();
        }

        function addCategory() {
            const name = prompt('กรุณาระบุชื่อหมวดหมู่แชทลับใหม่:');
            if (name && name.trim()) {
                const newId = 'cat_' + Date.now();
                appData.categories.push({
                    id: newId,
                    name: name.trim(),
                    icon: 'fa-folder'
                });
                appData.messages[newId] = [];
                appData.activeCategoryId = newId;
                saveData();
                renderCategories();
                renderMessages();
            }
        }

        function deleteCategory(catId) {
            if (appData.categories.length <= 1) return;
            
            if (confirm('คุณแน่ใจหรือไม่ว่าต้องการลบหมวดหมู่นี้? ข้อความทั้งหมดในหมวดนี้จะหายไป')) {
                appData.categories = appData.categories.filter(c => c.id !== catId);
                delete appData.messages[catId];
                
                if (appData.activeCategoryId === catId) {
                    appData.activeCategoryId = appData.categories[0].id;
                }
                saveData();
                renderCategories();
                renderMessages();
            }
        }

        // --- Chat Actions ---
        function sendMessage(text) {
            if (!text.trim()) return;

            const now = new Date();
            const timeStr = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });

            if (!appData.messages[appData.activeCategoryId]) {
                appData.messages[appData.activeCategoryId] = [];
            }

            appData.messages[appData.activeCategoryId].push({
                id: 'msg_' + Date.now(),
                sender: currentRole,
                text: text.trim(),
                time: timeStr
            });

            saveData();
            renderMessages();
        }

        function clearCurrentChat() {
            if (confirm('คุณต้องการล้างข้อมูลแชทในห้องนี้ใช่หรือไม่?')) {
                appData.messages[appData.activeCategoryId] = [];
                saveData();
                renderMessages();
            }
        }

        function clearAllData() {
            if (confirm('⚠️ เตือน :  คุณต้องการล้างข้อมูลแชททั้งหมดทุกหมวดหมู่ใช่หรือไม่? ข้อมูลทั้งหมดจะถูกลบอย่างถาวร!')) {

            appData = {
                    categories: [
                        { id: 'cat_general', name: 'ติดต่อผู้สร้างเซิร์ฟเวอร์', icon: 'fa-headset' }
                    ],
                    activeCategoryId: 'cat_general',
                    messages: {
                        'cat_general': []
                    }
                };
                saveData();
                renderCategories();
                renderMessages();
            }
        }

        // --- Role Switcher ---
        function setRole(role) {
            currentRole = role;
            if (role === 'user') {
                roleUserBtn.className = 'px-2.5 py-1 rounded-md text-[11px] font-medium transition bg-blue-600 text-white shadow';
                roleOtherBtn.className = 'px-2.5 py-1 rounded-md text-[11px] font-medium transition text-gray-400 hover:text-gray-200';
            } else {
                roleOtherBtn.className = 'px-2.5 py-1 rounded-md text-[11px] font-medium transition bg-gray-700 text-white shadow';
                roleUserBtn.className = 'px-2.5 py-1 rounded-md text-[11px] font-medium transition text-gray-400 hover:text-gray-200';
            }
        }

        // --- Helper Utilities ---
        function escapeHtml(str) {
            return str.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;").replace(/"/g, "&quot;").replace(/'/g, "&#039;");
        }

        function closeMobileSidebar() {
            sidebar.classList.add('-translate-x-full');
            sidebarOverlay.classList.add('hidden');
        }

        // --- Event Listeners ---
        function setupEventListeners() {
            // Secret Header Click Event (กด 1 ครั้งเพื่อสลับมาห้องแชทลับหลัก)
            const openMainSecretChat = () => {
                switchCategory('cat_general');
            };

            secretHeaderBtn.addEventListener('click', openMainSecretChat);
            mobileSecretBtn.addEventListener('click', openMainSecretChat);

            // Form Submit
            chatForm.addEventListener('submit', (e) => {
                e.preventDefault();
                sendMessage(messageInput.value);
                messageInput.value = '';
                messageInput.style.height = 'auto';
            });

            // Auto resize textarea
            messageInput.addEventListener('input', function() {
                this.style.height = 'auto';
                this.style.height = (this.scrollHeight) + 'px';
            });

            // Enter to send (Shift + Enter for new line)
            messageInput.addEventListener('keydown', (e) => {
                if (e.key === 'Enter' && !e.shiftKey) {
                    e.preventDefault();
                    chatForm.dispatchEvent(new Event('submit'));
                }
            });

            // Role Switch Buttons
            roleUserBtn.addEventListener('click', () => setRole('user'));
            roleOtherBtn.addEventListener('click', () => setRole('other'));

            // Category & Chat Management
            addCategoryBtn.addEventListener('click', addCategory);
            clearCurrentChatBtn.addEventListener('click', clearCurrentChat);
            clearAllBtn.addEventListener('click', clearAllData);

            // Mobile Menu Toggle
            toggleMenuBtn.addEventListener('click', () => {
                sidebar.classList.toggle('-translate-x-full');
                sidebarOverlay.classList.toggle('hidden');
            });

            sidebarOverlay.addEventListener('click', closeMobileSidebar);
        }

        // --- Initialize App ---
        window.addEventListener('DOMContentLoaded', init);
    </script>
</body>
</html>
