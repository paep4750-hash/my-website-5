<!DOCTYPE html>
<html lang="th">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Dark Chat</title>
  <!-- Lucide Icons -->
  <script src="https://unpkg.com/lucide@latest"></script>
  <style>
    :root {
      --bg-dark: #0b0f19;
      --sidebar-bg: #111827;
      --chat-bg: #0f172a;
      --card-active: #1e293b;
      --accent-blue: #2563eb;
      --text-main: #f8fafc;
      --text-muted: #94a3b8;
      --border-color: #1e293b;
      --msg-other: #1e293b;
      --msg-my: #2563eb;
      --danger-color: #7f1d1d;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    }

    html, body {
      width: 100%;
      height: 100%;
      height: 100dvh;
      background-color: var(--bg-dark);
      color: var(--text-main);
      overflow: hidden;
      margin: 0;
      padding: 0;
    }

    .app-container {
      width: 100vw;
      height: 100vh;
      height: 100dvh;
      display: flex;
      background-color: var(--chat-bg);
      overflow: hidden;
      position: relative;
    }

    /* --- SIDEBAR --- */
    .sidebar {
      width: 260px;
      background-color: var(--sidebar-bg);
      border-right: 1px solid var(--border-color);
      display: flex;
      flex-direction: column;
      flex-shrink: 0;
      height: 100%;
      overflow: hidden;
    }

    .sidebar-header {
      padding: 12px 14px;
      border-bottom: 1px solid var(--border-color);
      flex-shrink: 0;
    }

    .top-status {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 0.7rem;
      color: var(--text-muted);
      margin-bottom: 8px;
    }

    .status-badge {
      background-color: #064e3b;
      color: #34d399;
      padding: 2px 6px;
      border-radius: 10px;
      font-size: 0.6rem;
      font-weight: 600;
    }

    .main-title-box {
      cursor: pointer;
      padding: 6px;
      border-radius: 6px;
      transition: background 0.2s;
    }

    .main-title-box:hover {
      background-color: var(--card-active);
    }

    .main-title {
      font-size: 0.9rem;
      font-weight: 700;
      display: flex;
      align-items: center;
      gap: 6px;
      color: var(--text-main);
    }

    .main-subtitle {
      font-size: 0.65rem;
      color: var(--text-muted);
      margin-top: 2px;
    }

    /* Category List */
    .category-section {
      padding: 10px 14px;
      flex: 1;
      overflow-y: auto;
    }

    .category-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      font-size: 0.75rem;
      color: var(--text-muted);
      margin-bottom: 6px;
      font-weight: 600;
    }

    .add-cat-btn {
      background: none;
      border: none;
      color: var(--text-muted);
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 2px;
      border-radius: 4px;
    }

    .add-cat-btn:hover {
      background-color: var(--card-active);
      color: var(--text-main);
    }

    .category-list {
      display: flex;
      flex-direction: column;
      gap: 4px;
    }

    .category-item {
      display: flex;
      align-items: center;
      gap: 8px;
      padding: 8px 10px;
      border-radius: 6px;
      cursor: pointer;
      font-size: 0.8rem;
      color: var(--text-muted);
      transition: all 0.2s;
    }

    .category-item:hover {
      background-color: rgba(255, 255, 255, 0.05);
      color: var(--text-main);
    }

    .category-item.active {
      background-color: var(--card-active);
      color: var(--text-main);
      border: 1px solid rgba(255, 255, 255, 0.1);
      font-weight: 600;
    }

    /* Sidebar Footer */
    .sidebar-footer {
      padding: 10px 14px;
      border-top: 1px solid var(--border-color);
      background-color: var(--sidebar-bg);
      flex-shrink: 0;
    }

    .clear-all-btn {
      width: 100%;
      padding: 8px;
      background-color: transparent;
      border: 1px solid var(--danger-color);
      color: #f87171;
      border-radius: 6px;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 6px;
      font-size: 0.75rem;
      transition: all 0.2s;
    }

    .clear-all-btn:hover {
      background-color: var(--danger-color);
      color: #ffffff;
    }

    /* --- CHAT AREA --- */
    .chat-area {
      flex: 1;
      display: flex;
      flex-direction: column;
      height: 100%;
      background-color: var(--chat-bg);
      overflow: hidden;
      position: relative;
    }

    .chat-header {
      height: 52px;
      padding: 0 16px;
      border-bottom: 1px solid var(--border-color);
      display: flex;
      justify-content: space-between;
      align-items: center;
      background-color: var(--sidebar-bg);
      flex-shrink: 0;
    }

    .chat-title-group {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .avatar-icon {
      width: 28px;
      height: 28px;
      border-radius: 50%;
      background-color: var(--accent-blue);
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      flex-shrink: 0;
    }

    .chat-title-text {
      font-weight: 700;
      font-size: 0.85rem;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .chat-subtitle {
      font-size: 0.65rem;
      color: var(--text-muted);
    }

    .clear-this-btn {
      background-color: #331e08;
      border: 1px solid #78350f;
      color: #fbbf24;
      padding: 5px 10px;
      border-radius: 6px;
      font-size: 0.7rem;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 4px;
      flex-shrink: 0;
    }

    /* Messages Area */
    .messages-container {
      flex: 1;
      padding: 16px;
      overflow-y: auto;
      display: flex;
      flex-direction: column;
      gap: 10px;
      min-height: 0; /* สำคัญมาก ป้องกันไม่ให้แชทดันคอนเทนเนอร์หลุดขอบ */
    }

    .message-wrapper {
      display: flex;
      flex-direction: column;
      max-width: 75%;
    }

    .message-wrapper.my {
      align-self: flex-end;
      align-items: flex-end;
    }

    .message-wrapper.other {
      align-self: flex-start;
      align-items: flex-start;
    }

    .message-bubble {
      padding: 8px 12px;
      border-radius: 10px;
      font-size: 0.85rem;
      line-height: 1.4;
      word-break: break-word;
    }

    .message-wrapper.my .message-bubble {
      background-color: var(--msg-my);
      color: #ffffff;
      border-bottom-right-radius: 2px;
    }

    .message-wrapper.other .message-bubble {
      background-color: var(--msg-other);
      color: var(--text-main);
      border-bottom-left-radius: 2px;
    }

    .message-time {
      font-size: 0.6rem;
      color: var(--text-muted);
      margin-top: 2px;
    }

    /* Input Area - ล็อกล่างสุดเสมอ */
    .chat-input-area {
      padding: 10px 14px;
      border-top: 1px solid var(--border-color);
      background-color: var(--sidebar-bg);
      display: flex;
      flex-direction: column;
      gap: 6px;
      flex-shrink: 0;
    }

    .role-selector {
      display: flex;
      align-items: center;
      gap: 6px;
      font-size: 0.7rem;
    }

    .role-label {
      color: var(--text-muted);
    }

    .role-btn {
      background-color: var(--card-active);
      border: 1px solid var(--border-color);
      color: var(--text-muted);
      padding: 3px 8px;
      border-radius: 5px;
      cursor: pointer;
      font-size: 0.7rem;
      display: flex;
      align-items: center;
      gap: 4px;
    }

    .role-btn.active {
      background-color: rgba(37, 99, 235, 0.2);
      border-color: var(--accent-blue);
      color: #60a5fa;
      font-weight: 600;
    }

    .input-box-wrapper {
      display: flex;
      gap: 8px;
    }

    .chat-input {
      flex: 1;
      background-color: var(--bg-dark);
      border: 1px solid var(--border-color);
      border-radius: 6px;
      padding: 8px 12px;
      color: var(--text-main);
      outline: none;
      font-size: 0.85rem;
    }

    .chat-input:focus {
      border-color: var(--accent-blue);
    }

    .send-btn {
      background-color: var(--accent-blue);
      color: white;
      border: none;
      padding: 0 14px;
      border-radius: 6px;
      cursor: pointer;
      font-weight: 600;
      display: flex;
      align-items: center;
      gap: 4px;
      font-size: 0.8rem;
      flex-shrink: 0;
    }
  </style>
</head>
<body>

  <div class="app-container">
    <!-- SIDEBAR -->
    <div class="sidebar">
      <div class="sidebar-header">
        <div class="top-status">
          <span id="currentDateText">วันนี้: พฤหัส 30 ก.ค. 2569</span>
          <span class="status-badge">Online</span>
        </div>
        <div class="main-title-box" onclick="selectCategory('cat_default')">
          <div class="main-title">
            <i data-lucide="headphone-off" style="width: 14px; height: 14px; color: #60a5fa;"></i>
            <span>ติดต่อผู้สร้างเซิร์ฟเวอร์</span>
            <i data-lucide="chevron-right" style="width: 12px; height: 12px; color: var(--text-muted); margin-left: auto;"></i>
          </div>
          <div class="main-subtitle">ช่องทางติดต่อสอบถามและแจ้งปัญหา</div>
        </div>
      </div>

      <!-- หมวดหมู่แชทลับ -->
      <div class="category-section">
        <div class="category-header">
          <span>หมวดหมู่แชทลับ</span>
          <button class="add-cat-btn" onclick="addNewCategory()" title="เพิ่มหมวดหมู่ใหม่">
            <i data-lucide="plus" style="width: 14px; height: 14px;"></i>
          </button>
        </div>
        <div class="category-list" id="categoryList">
          <!-- โหลดหมวดหมู่ JS -->
        </div>
      </div>

      <!-- ปุ่มล้างข้อมูลทั้งหมด -->
      <div class="sidebar-footer">
        <button class="clear-all-btn" onclick="clearAllData()">
          <i data-lucide="trash-2" style="width: 14px; height: 14px;"></i>
          <span>ล้างข้อมูลแชททั้งหมด</span>
        </button>
      </div>
    </div>

    <!-- CHAT AREA -->
    <div class="chat-area">
      <!-- Chat Header -->
      <div class="chat-header">
        <div class="chat-title-group">
          <div class="avatar-icon">
            <i data-lucide="user-check" style="width: 16px; height: 16px;"></i>
          </div>
          <div>
            <div class="chat-title-text">
              <i data-lucide="headphone-off" style="width: 14px; height: 14px; color: #60a5fa;"></i>
              <span id="activeCategoryTitle">ติดต่อผู้สร้างเซิร์ฟเวอร์</span>
            </div>
            <div class="chat-subtitle" id="activeCategorySubtitle">หมวดหมู่: ติดต่อผู้สร้างเซิร์ฟเวอร์ (บันทึกข้อมูลแล้ว)</div>
          </div>
        </div>
        <button class="clear-this-btn" onclick="clearCurrentCategoryChat()">
          <i data-lucide="eraser" style="width: 12px; height: 12px;"></i>
          <span>ล้างข้อมูลแชทนี้</span>
        </button>
      </div>

      <!-- Messages Display -->
      <div class="messages-container" id="messagesContainer">
        <!-- แสดงข้อความแชท -->
      </div>

      <!-- Chat Input Area (ส่วนพิมพ์ล่างสุด) -->
      <div class="chat-input-area">
        <div style="display: flex; justify-content: space-between; align-items: center;">
          <div class="role-selector">
            <span class="role-label">บทบาท:</span>
            <button class="role-btn active" id="roleMyBtn" onclick="setRole('my')">
              <i data-lucide="user" style="width: 10px; height: 10px;"></i>
              <span>ฝั่งเรา</span>
            </button>
            <button class="role-btn" id="roleOtherBtn" onclick="setRole('other')">
              <i data-lucide="corner-up-left" style="width: 10px; height: 10px;"></i>
              <span>ตอบกลับเรา</span>
            </button>
          </div>
          <div style="font-size: 0.6rem; color: var(--text-muted); display: flex; align-items: center; gap: 4px;">
            <i data-lucide="lock" style="width: 10px; height: 10px;"></i>
            <span>บันทึกอัตโนมัติ</span>
          </div>
        </div>

        <div class="input-box-wrapper">
          <input type="text" id="messageInput" class="chat-input" placeholder="พิมพ์ข้อความที่ต้องการบันทึก..." onkeypress="handleKeyPress(event)">
          <button class="send-btn" onclick="sendMessage()">
            <span>ส่ง</span>
            <i data-lucide="send" style="width: 12px; height: 12px;"></i>
          </button>
        </div>
      </div>
    </div>
  </div>

  <script>
    // --- Data Management ---
    const DEFAULT_CATEGORIES = [
      { id: 'cat_default', name: 'ติดต่อผู้สร้างเซิร์ฟเวอร์', icon: 'headphone-off' },
      { id: 'cat_code', name: 'โน้ตโค้ดลับ', icon: 'code-2' },
      { id: 'cat_personal', name: 'บันทึกส่วนตัว', icon: 'landmark' }
    ];

    let categories = JSON.parse(localStorage.getItem('dark_chat_categories')) || DEFAULT_CATEGORIES;
    let chats = JSON.parse(localStorage.getItem('dark_chat_messages')) || {
      'cat_default': [
        { sender: 'other', text: 'สวัสดีครับ มีอะไรให้ผู้ดูแลเซิร์ฟเวอร์ช่วยเหลือไหมครับ?', time: '10:00 AM' },
        { sender: 'my', text: 'สอบถามเรื่องการจัดเก็บข้อมูลครับ', time: '10:01 AM' }
      ]
    };

    let activeCategoryId = localStorage.getItem('dark_chat_active_cat') || 'cat_default';
    let currentRole = 'my';

    window.onload = function() {
      updateDateDisplay();
      renderCategories();
      renderMessages();
      lucide.createIcons();
    };

    function updateDateDisplay() {
      const now = new Date();
      const days = ['อาทิตย์', 'จันทร์', 'อังคาร', 'พุธ', 'พฤหัส', 'ศุกร์', 'เสาร์'];
      const months = ['ม.ค.', 'ก.พ.', 'มี.ค.', 'เม.ย.', 'พ.ค.', 'มิ.ย.', 'ก.ค.', 'ส.ค.', 'ก.ย.', 'ต.ค.', 'พ.ย.', 'ธ.ค.'];
      
      const dayName = days[now.getDay()];
      const dayNum = now.getDate();
      const monthName = months[now.getMonth()];
      const yearBE = now.getFullYear() + 543;

      document.getElementById('currentDateText').textContent = `วันนี้: ${dayName} ${dayNum} ${monthName} ${yearBE}`;
    }

    function renderCategories() {
      const categoryList = document.getElementById('categoryList');
      categoryList.innerHTML = '';

      categories.forEach(cat => {
        const item = document.createElement('div');
        item.className = `category-item ${cat.id === activeCategoryId ? 'active' : ''}`;
        item.onclick = () => selectCategory(cat.id);

        const iconName = cat.icon || 'folder';
        item.innerHTML = `
          <i data-lucide="${iconName}" style="width: 12px; height: 12px;"></i>
          <span style="flex: 1; overflow: hidden; text-overflow: ellipsis; white-space: nowrap;">${escapeHtml(cat.name)}</span>
        `;
        categoryList.appendChild(item);
      });

      lucide.createIcons();
    }

    function selectCategory(catId) {
      activeCategoryId = catId;
      localStorage.setItem('dark_chat_active_cat', catId);
      
      const activeCat = categories.find(c => c.id === catId) || { name: 'หมวดหมู่ลับ' };
      document.getElementById('activeCategoryTitle').textContent = activeCat.name;
      document.getElementById('activeCategorySubtitle').textContent = `หมวดหมู่: ${activeCat.name} (บันทึกข้อมูลแล้ว)`;

      renderCategories();
      renderMessages();
    }

    function addNewCategory() {
      const catName = prompt('กรอกชื่อหมวดหมู่แชทลับใหม่:');
      if (catName && catName.trim() !== '') {
        const newId = 'cat_' + Date.now();
        categories.push({
          id: newId,
          name: catName.trim(),
          icon: 'folder'
        });
        chats[newId] = [];
        
        saveCategories();
        saveChats();
        selectCategory(newId);
      }
    }

    function renderMessages() {
      const container = document.getElementById('messagesContainer');
      container.innerHTML = '';

      const currentChats = chats[activeCategoryId] || [];

      currentChats.forEach(msg => {
        const wrapper = document.createElement('div');
        wrapper.className = `message-wrapper ${msg.sender}`;

        wrapper.innerHTML = `
          <div class="message-bubble">${escapeHtml(msg.text)}</div>
          <div class="message-time">${msg.time}</div>
        `;
        container.appendChild(wrapper);
      });

      container.scrollTop = container.scrollHeight;
    }

    function setRole(role) {
      currentRole = role;
      const myBtn = document.getElementById('roleMyBtn');
      const otherBtn = document.getElementById('roleOtherBtn');

      if (role === 'my') {
        myBtn.classList.add('active');
        otherBtn.classList.remove('active');
      } else {
        otherBtn.classList.add('active');
        myBtn.classList.remove('active');
      }
    }

    function sendMessage() {
      const input = document.getElementById('messageInput');
      const text = input.value.trim();

      if (text === '') return;

      if (!chats[activeCategoryId]) {
        chats[activeCategoryId] = [];
      }

      const now = new Date();
      const timeStr = now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' });

      chats[activeCategoryId].push({
        sender: currentRole,
        text: text,
        time: timeStr
      });

      saveChats();
      renderMessages();
      input.value = '';
    }

    function handleKeyPress(e) {
      if (e.key === 'Enter') {
        sendMessage();
      }
    }

    function clearCurrentCategoryChat() {
      if (confirm('คุณต้องการล้างข้อมูลแชทในหมวดหมู่นี้ใช่หรือไม่?')) {
        chats[activeCategoryId] = [];
        saveChats();
        renderMessages();
      }
    }

    function clearAllData() {
      if (confirm('คุณต้องการล้างข้อมูลทั้งหมดใช่หรือไม่?')) {
        localStorage.removeItem('dark_chat_categories');
        localStorage.removeItem('dark_chat_messages');
        localStorage.removeItem('dark_chat_active_cat');

        categories = DEFAULT_CATEGORIES;
        chats = {
          'cat_default': [
            { sender: 'other', text: 'สวัสดีครับ มีอะไรให้ผู้ดูแลเซิร์ฟเวอร์ช่วยเหลือไหมครับ?', time: '10:00 AM' },
            { sender: 'my', text: 'สอบถามเรื่องการจัดเก็บข้อมูลครับ', time: '10:01 AM' }
          ]
        };
        activeCategoryId = 'cat_default';

        saveCategories();
        saveChats();
        selectCategory('cat_default');
      }
    }

    function saveCategories() {
      localStorage.setItem('dark_chat_categories', JSON.stringify(categories));
    }

    function saveChats() {
      localStorage.setItem('dark_chat_messages', JSON.stringify(chats));
    }

    function escapeHtml(str) {
      return str.replace(/&/g, "&amp;")
                .replace(/</g, "&lt;")
                .replace(/>/g, "&gt;")
                .replace(/"/g, "&quot;")
                .replace(/'/g, "&#039;");
    }
  </script>
</body>
</html>

