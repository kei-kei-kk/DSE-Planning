<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>HKDSE Revision Planner Calculator</title>
  <style>
    :root {
      --bg: #f0f4f8;
      --card-bg: #ffffff;
      --primary: #3b82f6;
      --primary-dark: #1d4ed8;
      --accent-blue: #60a5fa;
      --text: #1e293b;
      --text-muted: #64748b;
      --border: #cbd5e1;
      
      --block-grey: #94a3b8;
      --danger: #ef4444;
      --danger-soft: #fef2f2;
      --danger-border: #fecaca;
      --danger-hover: #fee2e2;
      --success: #10b981;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
      -webkit-tap-highlight-color: transparent;
      touch-action: manipulation;
    }

    body {
      background-color: #e2e8f0;
      background-image: linear-gradient(135deg, #e2e8f0 0%, #dbeafe 100%);
      min-height: 100vh;
      color: var(--text);
      padding: 12px;
      padding-bottom: 40px;
    }

    /* Sticky Drop Bar on Scroll */
    .sticky-drop-bar {
      position: sticky;
      top: 10px;
      z-index: 1000;
      background: rgba(255, 255, 255, 0.95);
      backdrop-filter: blur(8px);
      padding: 8px 14px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
      border: 2px dashed var(--primary);
      display: flex;
      align-items: center;
      gap: 12px;
      margin-bottom: 12px;
    }

    /* Auth Overlay */
    .auth-overlay {
      position: fixed;
      top: 0; left: 0; width: 100vw; height: 100vh;
      background: rgba(15, 23, 42, 0.6);
      backdrop-filter: blur(4px);
      display: flex;
      align-items: center;
      justify-content: center;
      z-index: 9999;
    }

    .auth-card {
      background: var(--card-bg);
      border-radius: 16px;
      padding: 24px;
      width: 90%;
      max-width: 380px;
      box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.1);
      text-align: center;
    }

    .auth-card h2 { font-size: 1.3rem; margin-bottom: 12px; color: #0f172a; }

    .auth-tabs {
      display: flex; gap: 8px; margin-bottom: 16px;
      background: #f1f5f9; padding: 4px; border-radius: 8px;
    }

    .auth-tab {
      flex: 1; padding: 8px; font-size: 0.85rem; font-weight: 600;
      border: none; background: transparent; color: var(--text-muted);
      cursor: pointer; border-radius: 6px;
    }

    .auth-tab.active {
      background: white; color: var(--primary-dark);
      box-shadow: 0 1px 3px rgba(0,0,0,0.1);
    }

    .auth-input {
      width: 100%; padding: 10px 12px; margin-bottom: 12px;
      border: 1px solid var(--border); border-radius: 8px; font-size: 0.9rem;
    }

    .auth-btn {
      width: 100%; padding: 10px; background: var(--primary);
      color: white; border: none; border-radius: 8px; font-weight: 600;
      cursor: pointer; font-size: 0.9rem;
    }

    .auth-btn:hover { background: var(--primary-dark); }

    .guest-btn {
      background: transparent; border: none; color: var(--text-muted);
      font-size: 0.8rem; margin-top: 14px; cursor: pointer; text-decoration: underline;
    }

    .auth-error {
      color: var(--danger); font-size: 0.75rem; margin-bottom: 10px; display: none;
    }

    .container { max-width: 900px; margin: 0 auto; }

    .user-bar {
      display: flex; justify-content: space-between; align-items: center;
      background: rgba(255, 255, 255, 0.7); padding: 8px 14px;
      border-radius: 10px; margin-bottom: 12px; font-size: 0.85rem; font-weight: 600;
    }

    .logout-btn {
      background: #e2e8f0; border: none; padding: 4px 10px;
      border-radius: 6px; cursor: pointer; font-size: 0.75rem; color: var(--text-muted);
    }

    h1 { font-size: 1.25rem; text-align: center; margin-bottom: 12px; color: #0f172a; font-weight: 700; }

    .card {
      background: var(--card-bg); border-radius: 14px; padding: 14px;
      margin-bottom: 14px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05);
      border: 1px solid rgba(226, 232, 240, 0.8);
    }

    /* Encourage Tip Banner */
    .tip-banner {
      background: #eff6ff;
      border: 1px solid #bfdbfe;
      color: #1e40af;
      padding: 8px 12px;
      border-radius: 8px;
      font-size: 0.8rem;
      font-weight: 600;
      text-align: center;
      margin-bottom: 12px;
    }

    .total-banner {
      display: flex; justify-content: center; align-items: center;
      background: linear-gradient(135deg, #2563eb, #3b82f6); color: white;
      padding: 14px 18px; border-radius: 10px; margin-bottom: 14px;
      box-shadow: 0 2px 4px rgba(37, 99, 235, 0.2);
    }

    .total-banner .value-group { font-size: 1.2rem; font-weight: 700; text-align: center; }

    .sample-block {
      width: 75px; height: 38px; background: var(--block-grey);
      color: white; font-weight: 600; font-size: 0.75rem; border-radius: 6px;
      display: flex; align-items: center; justify-content: center;
      cursor: grab; user-select: none; box-shadow: 0 2px 4px rgba(0,0,0,0.12);
      touch-action: none; flex-shrink: 0;
    }

    .day-row { margin-bottom: 14px; border-bottom: 1px solid var(--border); padding-bottom: 10px; }
    .day-row:last-child { border-bottom: none; }

    .day-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 6px; }
    .day-title { font-weight: 700; font-size: 0.9rem; color: #334155; }

    .lock-btn {
      background: #f1f5f9; border: 1px solid var(--border);
      padding: 4px 10px; border-radius: 6px; font-size: 0.75rem; font-weight: 600;
      cursor: pointer; color: #475569; transition: all 0.2s ease;
    }
    .lock-btn.locked { background: #dbeafe; color: var(--primary-dark); border-color: #93c5fd; }

    .timeline-wrapper {
      overflow-x: auto; background: #f8fafc; border: 1px solid var(--border);
      border-radius: 8px; padding: 4px; -webkit-overflow-scrolling: touch;
    }

    .timeline-track {
      position: relative; width: 1440px; height: 48px;
      background-size: 30px 100%;
      background-image: linear-gradient(to right, #e2e8f0 1px, transparent 1px);
    }

    .time-labels { position: relative; width: 1440px; height: 20px; border-bottom: 1px solid var(--border); }
    .time-label { position: absolute; font-size: 0.65rem; color: var(--text-muted); transform: translateX(-50%); font-weight: 500; }

    .placed-block {
      position: absolute; top: 4px; height: 40px; width: 60px;
      border-radius: 6px; display: flex; flex-direction: column;
      align-items: center; justify-content: center; font-size: 0.65rem;
      font-weight: 700; color: white; cursor: grab; user-select: none;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1); transition: transform 0.15s ease;
      padding: 2px; text-align: center; line-height: 1.1; touch-action: none;
    }

    .placed-block.grey { background-color: var(--block-grey); }

    table { width: 100%; border-collapse: collapse; font-size: 0.825rem; }
    th, td { padding: 8px 4px; border-bottom: 1px solid var(--border); text-align: center; }
    th { color: var(--text-muted); font-size: 0.75rem; font-weight: 600; }

    td input[type="number"] {
      width: 50px; padding: 6px 2px; border: 1px solid var(--border);
      border-radius: 6px; text-align: center; font-weight: 600; color: var(--text);
    }

    /* Editable Subject Name Input */
    .subject-edit-input {
      border: 1px transparent;
      background: transparent;
      font-weight: 600;
      font-size: 0.825rem;
      color: var(--text);
      padding: 2px 4px;
      border-radius: 4px;
      width: 110px;
    }
    .subject-edit-input:hover, .subject-edit-input:focus {
      background: #f1f5f9;
      border: 1px solid var(--border);
    }

    .subject-tag { display: inline-block; width: 10px; height: 10px; border-radius: 50%; margin-right: 6px; }
    .special-row { background-color: #f8fafc; }

    .add-subject-btn {
      width: 100%;
      margin-top: 8px;
      padding: 8px;
      background: #f1f5f9;
      border: 1px dashed var(--primary);
      color: var(--primary-dark);
      border-radius: 8px;
      font-size: 0.8rem;
      font-weight: 600;
      cursor: pointer;
      transition: background 0.2s ease;
    }
    .add-subject-btn:hover {
      background: #e2e8f0;
    }

    .suggestion-note {
      margin-top: 10px;
      font-size: 0.75rem;
      color: var(--text-muted);
      text-align: center;
      font-style: italic;
    }

    .status-box { margin-top: 12px; padding: 10px; border-radius: 8px; font-size: 0.8rem; font-weight: 600; display: none; }
    .status-box.error { display: block; background: #fef2f2; color: var(--danger); border: 1px solid #fecaca; }
    .status-box.success { display: block; background: #ecfdf5; color: var(--success); border: 1px solid #a7f3d0; }

    .reset-area { display: flex; justify-content: center; margin-top: 18px; margin-bottom: 12px; }
    .reset-btn {
      background-color: var(--danger-soft); color: var(--danger);
      border: 1px solid var(--danger-border); padding: 10px 20px;
      border-radius: 8px; font-size: 0.825rem; font-weight: 600; cursor: pointer;
    }

    .subject-dialog-overlay {
      position: fixed; top: 0; left: 0; width: 100vw; height: 100vh;
      background: rgba(0, 0, 0, 0.4); display: flex; align-items: center; justify-content: center; z-index: 10000;
    }

    .subject-dialog { background: white; padding: 20px; border-radius: 12px; width: 280px; text-align: center; }
    .subject-dialog h3 { font-size: 1rem; margin-bottom: 12px; }
    .subject-option-btn {
      width: 100%; padding: 10px; margin-bottom: 8px; border: none;
      border-radius: 8px; color: white; font-weight: 600; cursor: pointer; font-size: 0.85rem;
    }
  </style>
</head>
<body>

<div class="auth-overlay" id="auth-overlay">
  <div class="auth-card">
    <h2>Revision Planner Login</h2>
    <div class="auth-tabs">
      <button class="auth-tab active" id="tab-login" onclick="switchAuthTab('login')">Login</button>
      <button class="auth-tab" id="tab-register" onclick="switchAuthTab('register')">Create Account</button>
    </div>
    
    <div id="auth-error" class="auth-error"></div>
    
    <input type="text" id="auth-username" class="auth-input" placeholder="Username (3-20 chars)" maxlength="20">
    <input type="password" id="auth-password" class="auth-input" placeholder="Password (3-20 chars)" maxlength="20">
    
    <button class="auth-btn" id="auth-submit-btn" onclick="handleAuthSubmit()">Login</button>
    <button class="guest-btn" onclick="loginAsGuest()">Continue as Guest (Session Only)</button>
  </div>
</div>

<div class="container" id="main-app" style="display: none;">
  <div class="user-bar">
    <span>User: <strong id="current-user-display">Guest</strong></span>
    <button class="logout-btn" onclick="logout()">Logout / Switch User</button>
  </div>

  <h1>HKDSE Revision Planner</h1>

  <div class="tip-banner" id="tip-banner" style="display: none;">
    💡 <strong>Revision Tip:</strong> 1 block = 50 min revision + 10 min break
  </div>

  <div class="sticky-drop-bar">
    <div class="sample-block" id="sticky-sample-block" draggable="true">1 Block</div>
    <span style="font-size: 0.75rem; color: var(--text-muted); font-weight: 600;">
      <strong>Quick Drop:</strong> Drag this block directly down to any timeline without scrolling up!
    </span>
  </div>

  <div class="card">
    <div class="total-banner">
      <div class="value-group">
        Total Week Blocks: <span id="week-finished-display">0</span> / <span id="week-total-display">0</span>
      </div>
    </div>

    <div id="days-container"></div>
  </div>

  <div class="card">
    <div style="font-weight: 700; font-size: 0.95rem; margin-bottom: 10px; color: #334155;">Time Allocation</div>
    <table>
      <thead>
        <tr>
          <th style="text-align: left;">Category</th>
          <th>Suggested</th>
          <th>Allocated</th>
          <th>Finished</th>
          <th>Share</th>
        </tr>
      </thead>
      <tbody id="allocation-tbody"></tbody>
    </table>
    
    <div id="non-writer-extras" style="display: none;">
      <button class="add-subject-btn" onclick="addNewSubject()">+ Add New Subject</button>
      <div class="suggestion-note">Tip: Buffer time or extra study columns can be added as custom subjects if needed.</div>
    </div>

    <div id="validation-msg" class="status-box"></div>
  </div>

  <div class="reset-area">
    <button class="reset-btn" onclick="clearPlannerData()">
      🗑 Clear All Records & Reset
    </button>
  </div>
</div>

<div id="subject-dialog-overlay" class="subject-dialog-overlay" style="display: none;">
  <div class="subject-dialog">
    <h3>Select Subject</h3>
    <div id="subject-options-container"></div>
    <button onclick="closeSubjectDialog()" style="margin-top: 6px; background: transparent; border: none; color: var(--text-muted); cursor: pointer; font-size: 0.8rem;">Cancel</button>
  </div>
</div>

<script>
  const days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];
  
  const macaronColors = [
    '#70a1ff', '#ff7875', '#ffc069', '#5cdbd3', 
    '#b37feb', '#ff9c6e', '#95de64', '#ffd666',
    '#ff85c0', '#36cfc9'
  ];

  const defaultWriterItems = [
    { name: 'Chinese', weight: 0.20, allocation: 0, type: 'subject', color: macaronColors[0] },
    { name: 'English', weight: 0.20, allocation: 0, type: 'subject', color: macaronColors[1] },
    { name: 'Mathematics', weight: 0.15, allocation: 0, type: 'subject', color: macaronColors[2] },
    { name: 'Geography', weight: 0.15, allocation: 0, type: 'subject', color: macaronColors[3] },
    { name: 'Biology', weight: 0.15, allocation: 0, type: 'subject', color: macaronColors[4] },
    { name: 'Economics', weight: 0.15, allocation: 0, type: 'subject', color: macaronColors[5] },
    { name: 'Buffer Time', weight: 0, allocation: 0, type: 'buffer', color: '#94a3b8' },
    { name: 'Extra Learning', weight: 0, allocation: 0, type: 'extra', color: '#64748b' }
  ];

  // Non-Writer defaults: 3 subjects only
  const defaultUserSubjects = ['Subject 1', 'Subject 2', 'Subject 3'];

  let currentAuthMode = 'login';
  let currentUser = null; 
  let isGuest = false;

  let accountsDB = {}; 
  let items = [];
  let plannerData = {};
  let scrollPositions = {};
  let grabOffsetX = 0;

  let activeBlockContext = null;
  let draggedBlock = null;

  // Timers for 0.2s redrop & 0.8s continuous duplicate
  let holdTimer = null;
  let dupInterval = null;
  let isHoldPickedUp = false;

  function initApp() {
    loadAccountsDB();
    setupSampleBlockEvents();
    checkExistingSession();
  }

  function loadAccountsDB() {
    const db = localStorage.getItem('hkdse_accounts_db');
    accountsDB = db ? JSON.parse(db) : {};
  }

  function saveAccountsDB() {
    localStorage.setItem('hkdse_accounts_db', JSON.stringify(accountsDB));
  }

  // Restore session upon page refresh without forcing re-login
  function checkExistingSession() {
    const activeSession = localStorage.getItem('hkdse_active_session');
    if (activeSession) {
      if (activeSession === 'Guest') {
        loginAsGuest();
      } else if (accountsDB[activeSession] || activeSession === 'Writer') {
        loginUser(activeSession);
      }
    }
  }

  function switchAuthTab(mode) {
    currentAuthMode = mode;
    document.getElementById('tab-login').className = `auth-tab ${mode === 'login' ? 'active' : ''}`;
    document.getElementById('tab-register').className = `auth-tab ${mode === 'register' ? 'active' : ''}`;
    document.getElementById('auth-submit-btn').innerText = mode === 'login' ? 'Login' : 'Create Account';
    hideAuthError();
  }

  function showAuthError(msg) {
    const err = document.getElementById('auth-error');
    err.innerText = msg;
    err.style.display = 'block';
  }

  function hideAuthError() {
    document.getElementById('auth-error').style.display = 'none';
  }

  function handleAuthSubmit() {
    const uInput = document.getElementById('auth-username').value.trim();
    const pInput = document.getElementById('auth-password').value.trim();

    if (!/^[a-zA-Z0-9]{3,20}$/.test(uInput)) {
      showAuthError('Username must be 3-20 alphanumeric characters.');
      return;
    }

    if (pInput.length < 3 || pInput.length > 20) {
      showAuthError('Password must be 3-20 characters.');
      return;
    }

    if (uInput.toLowerCase() === 'writer') {
      if (uInput === 'Writer' && pInput === 'Charlie1992929') {
        loginUser('Writer');
        return;
      } else {
        showAuthError('Invalid credentials for reserved account.');
        return;
      }
    }

    if (currentAuthMode === 'register') {
      if (accountsDB[uInput]) {
        showAuthError('Username already exists. Please pick another.');
        return;
      }
      accountsDB[uInput] = { password: pInput, plannerData: createEmptyPlannerData(), items: createDefaultUserItems() };
      saveAccountsDB();
      loginUser(uInput);
    } else {
      if (!accountsDB[uInput] || accountsDB[uInput].password !== pInput) {
        showAuthError('Incorrect username or password.');
        return;
      }
      loginUser(uInput);
    }
  }

  function loginAsGuest() {
    isGuest = true;
    currentUser = null;
    localStorage.setItem('hkdse_active_session', 'Guest');
    document.getElementById('current-user-display').innerText = 'Guest (Session Saved)';
    
    const guestData = localStorage.getItem('hkdse_guest_planner');
    const guestItems = localStorage.getItem('hkdse_guest_items');

    plannerData = guestData ? JSON.parse(guestData) : createEmptyPlannerData();
    items = guestItems ? JSON.parse(guestItems) : createDefaultUserItems();

    startSession();
  }

  function loginUser(username) {
    isGuest = false;
    currentUser = username;
    localStorage.setItem('hkdse_active_session', username);
    document.getElementById('current-user-display').innerText = username;

    if (username === 'Writer' && !accountsDB['Writer']) {
      accountsDB['Writer'] = { password: 'Charlie1992929', plannerData: createEmptyPlannerData(), items: JSON.parse(JSON.stringify(defaultWriterItems)) };
      saveAccountsDB();
    }

    plannerData = accountsDB[username] ? accountsDB[username].plannerData || createEmptyPlannerData() : createEmptyPlannerData();
    items = accountsDB[username] ? accountsDB[username].items || (username === 'Writer' ? JSON.parse(JSON.stringify(defaultWriterItems)) : createDefaultUserItems()) : createDefaultUserItems();

    startSession();
  }

  function startSession() {
    document.getElementById('auth-overlay').style.display = 'none';
    document.getElementById('main-app').style.display = 'block';
    
    const isWriter = (currentUser === 'Writer');
    
    document.getElementById('tip-banner').style.display = isWriter ? 'none' : 'block';
    document.getElementById('non-writer-extras').style.display = isWriter ? 'none' : 'block';

    renderDays();
    updateCalculations();
  }

  function logout() {
    localStorage.removeItem('hkdse_active_session');
    document.getElementById('main-app').style.display = 'none';
    document.getElementById('auth-overlay').style.display = 'flex';
    document.getElementById('auth-username').value = '';
    document.getElementById('auth-password').value = '';
    hideAuthError();
  }

  function createEmptyPlannerData() {
    let pData = {};
    days.forEach(day => { pData[day] = { blocks: [], locked: false }; });
    return pData;
  }

  function createDefaultUserItems() {
    let userItems = defaultUserSubjects.map((name, idx) => ({
      name: name, weight: 0, allocation: '', type: 'subject', color: macaronColors[idx % macaronColors.length]
    }));
    return userItems;
  }

  function addNewSubject() {
    const nextIdx = items.length + 1;
    items.push({
      name: `Subject ${nextIdx}`,
      weight: 0,
      allocation: '',
      type: 'subject',
      color: macaronColors[(items.length) % macaronColors.length]
    });
    saveData();
    updateCalculations();
  }

  function saveData() {
    if (isGuest) {
      localStorage.setItem('hkdse_guest_planner', JSON.stringify(plannerData));
      localStorage.setItem('hkdse_guest_items', JSON.stringify(items));
    } else if (currentUser && accountsDB[currentUser]) {
      accountsDB[currentUser].plannerData = plannerData;
      accountsDB[currentUser].items = items;
      saveAccountsDB();
    }
  }

  function clearPlannerData() {
    if (confirm('Are you sure you want to clear all timeline blocks and allocations?')) {
      plannerData = createEmptyPlannerData();
      items = (currentUser === 'Writer') ? JSON.parse(JSON.stringify(defaultWriterItems)) : createDefaultUserItems();
      saveData();
      renderDays();
      updateCalculations();
    }
  }

  function saveScrollPositions() {
    days.forEach(day => {
      const wrapper = document.getElementById(`wrapper-${day}`);
      if (wrapper) scrollPositions[day] = wrapper.scrollLeft;
    });
  }

  function restoreScrollPositions() {
    days.forEach(day => {
      const wrapper = document.getElementById(`wrapper-${day}`);
      if (wrapper && scrollPositions[day] !== undefined) wrapper.scrollLeft = scrollPositions[day];
    });
  }

  function renderDays() {
    saveScrollPositions();
    const container = document.getElementById('days-container');
    container.innerHTML = '';

    days.forEach(day => {
      const dayData = plannerData[day];
      const row = document.createElement('div');
      row.className = 'day-row';

      const totalCount = dayData.blocks.length;
      const doneCount = dayData.blocks.filter(b => b.completed).length;

      let labelsHTML = '';
      for (let h = 0; h <= 24; h += 2) {
        let labelText = h === 0 || h === 24 ? '12AM' : h === 12 ? '12PM' : h > 12 ? `${h-12}PM` : `${h}AM`;
        labelsHTML += `<div class="time-label" style="left: ${h * 60}px;">${labelText}</div>`;
      }

      let blocksHTML = dayData.blocks.map((b, idx) => {
        const leftPos = b.startMinutes * 1; 
        const timeStr = formatMinutes(b.startMinutes);
        
        let blockStyle = `left: ${leftPos}px;`;
        let blockClass = "placed-block grey";
        let contentDisplay = timeStr;

        if (b.completed && b.assignedSubject) {
          const subItem = items.find(i => i.name === b.assignedSubject);
          const subColor = subItem ? subItem.color : '#34d399';
          blockStyle += ` background-color: ${subColor};`;
          blockClass = "placed-block";
          contentDisplay = `<div>✓</div><div style="font-size:0.55rem; overflow:hidden; text-overflow:ellipsis; white-space:nowrap; max-width:55px;">${b.assignedSubject}</div>`;
        }

        return `
          <div class="${blockClass}" 
               style="${blockStyle}" 
               onmousedown="startHoldAndDrag(event, '${day}', ${idx})"
               onmouseleave="cancelHoldAndDrag()"
               onmouseup="cancelHoldAndDrag()"
               ontouchstart="startHoldAndDrag(event, '${day}', ${idx})"
               ontouchend="cancelHoldAndDrag()"
               onclick="handleBlockClick('${day}', ${idx})">
            ${contentDisplay}
          </div>`;
      }).join('');

      row.innerHTML = `
        <div class="day-header">
          <span class="day-title">${day}</span>
          <span style="font-size: 0.75rem; color: var(--text-muted); font-weight: 500;">
            ${dayData.locked ? `Done: ${doneCount} / ` : ''}${totalCount} Blocks
          </span>
          <button class="lock-btn ${dayData.locked ? 'locked' : ''}" onclick="toggleLock('${day}')">
            ${dayData.locked ? '🔒 Locked' : '🔓 Lock'}
          </button>
        </div>
        <div class="timeline-wrapper" id="wrapper-${day}" ondragover="allowDrop(event)" ondrop="handleDrop(event, '${day}')">
          <div class="time-labels">${labelsHTML}</div>
          <div class="timeline-track" id="track-${day}">
            ${blocksHTML}
          </div>
        </div>
      `;
      container.appendChild(row);
    });

    restoreScrollPositions();
  }

  function formatMinutes(mins) {
    let h = Math.floor(mins / 60);
    let m = mins % 60;
    let ampm = h >= 12 && h < 24 ? 'P' : 'A';
    let displayH = h % 12 === 0 ? 12 : h % 12;
    return `${displayH}:${m === 0 ? '00' : m}${ampm}`;
  }

  function startHoldAndDrag(e, day, index) {
    if (plannerData[day].locked) return;
    isHoldPickedUp = false;

    holdTimer = setTimeout(() => {
      isHoldPickedUp = true;
      draggedBlock = plannerData[day].blocks[index];
      plannerData[day].blocks.splice(index, 1);
      saveData();
      renderDays();
    }, 200);

    dupInterval = setTimeout(() => {
      cancelHoldAndDrag();
      
      let baseBlock = plannerData[day].blocks[index];
      if (!baseBlock) return;

      let lastMins = baseBlock.startMinutes;
      
      dupInterval = setInterval(() => {
        let newMins = lastMins + 60;
        if (newMins <= 1380) {
          plannerData[day].blocks.push({
            startMinutes: newMins,
            completed: false,
            assignedSubject: null
          });
          lastMins = newMins;
          saveData();
          renderDays();
          updateCalculations();
        } else {
          clearInterval(dupInterval);
        }
      }, 200);
    }, 800);
  }

  function cancelHoldAndDrag() {
    if (holdTimer) {
      clearTimeout(holdTimer);
      holdTimer = null;
    }
    if (dupInterval) {
      clearInterval(dupInterval);
      clearTimeout(dupInterval);
      dupInterval = null;
    }
  }

  function toggleLock(day) {
    plannerData[day].locked = !plannerData[day].locked;
    saveData();
    renderDays();
  }

  function handleBlockClick(day, index) {
    if (isHoldPickedUp) {
      isHoldPickedUp = false;
      return;
    }

    if (plannerData[day].locked) {
      const block = plannerData[day].blocks[index];
      if (!block.completed) {
        activeBlockContext = { day, index };
        openSubjectDialog();
      } else {
        block.completed = false;
        block.assignedSubject = null;
        saveData();
        renderDays();
        updateCalculations();
      }
    } else {
      plannerData[day].blocks.splice(index, 1);
      saveData();
      renderDays();
      updateCalculations();
    }
  }

  function openSubjectDialog() {
    const container = document.getElementById('subject-options-container');
    container.innerHTML = '';

    items.forEach(item => {
      const btn = document.createElement('button');
      btn.className = 'subject-option-btn';
      btn.style.backgroundColor = item.color;
      btn.innerText = item.name;
      btn.onclick = () => selectSubjectForBlock(item.name);
      container.appendChild(btn);
    });

    document.getElementById('subject-dialog-overlay').style.display = 'flex';
  }

  function closeSubjectDialog() {
    document.getElementById('subject-dialog-overlay').style.display = 'none';
    activeBlockContext = null;
  }

  function selectSubjectForBlock(subjectName) {
    if (activeBlockContext) {
      const { day, index } = activeBlockContext;
      const block = plannerData[day].blocks[index];
      block.completed = true;
      block.assignedSubject = subjectName;
      saveData();
      renderDays();
      updateCalculations();
    }
    closeSubjectDialog();
  }

  function calculateLeftEdgeMinutes(leftEdgeX) {
    let snappedMins = Math.round(leftEdgeX / 30) * 30;
    if (snappedMins < 0) snappedMins = 0;
    if (snappedMins > 1380) snappedMins = 1380;
    return snappedMins;
  }

  function allowDrop(ev) { ev.preventDefault(); }

  function handleDrop(ev, day) {
    ev.preventDefault();
    if (plannerData[day].locked) return;
    const track = document.getElementById(`track-${day}`);
    const rect = track.getBoundingClientRect();
    const cursorXOnTrack = ev.clientX - rect.left;
    const actualLeftEdgeX = cursorXOnTrack - grabOffsetX;
    const snappedMins = calculateLeftEdgeMinutes(actualLeftEdgeX);

    if (draggedBlock) {
      draggedBlock.startMinutes = snappedMins;
      plannerData[day].blocks.push(draggedBlock);
      draggedBlock = null;
    } else {
      plannerData[day].blocks.push({ startMinutes: snappedMins, completed: false, assignedSubject: null });
    }

    saveData();
    renderDays();
    updateCalculations();
  }

  function setupSampleBlockEvents() {
    const stickySample = document.getElementById('sticky-sample-block');
    let ghostEl = null;

    function handleTouchStart(e, elem) {
      const touch = e.touches[0];
      const rect = elem.getBoundingClientRect();
      grabOffsetX = touch.clientX - rect.left;

      ghostEl = elem.cloneNode(true);
      ghostEl.style.position = 'fixed';
      ghostEl.style.opacity = '0.85';
      ghostEl.style.pointerEvents = 'none';
      ghostEl.style.zIndex = '10000';
      document.body.appendChild(ghostEl);
      moveGhost(touch);
    }

    stickySample.addEventListener('dragstart', (e) => {
      grabOffsetX = e.clientX - stickySample.getBoundingClientRect().left;
    });

    stickySample.addEventListener('touchstart', (e) => handleTouchStart(e, stickySample));

    document.addEventListener('touchmove', (e) => {
      if (ghostEl) moveGhost(e.touches[0]);
    });

    document.addEventListener('touchend', (e) => {
      if (!ghostEl) return;
      const touch = e.changedTouches[0];
      ghostEl.remove();
      ghostEl = null;

      days.forEach(day => {
        const track = document.getElementById(`track-${day}`);
        if (track) {
          const rect = track.getBoundingClientRect();
          if (touch.clientX >= rect.left && touch.clientX <= rect.right &&
              touch.clientY >= rect.top && touch.clientY <= rect.bottom) {
            
            if (plannerData[day].locked) return;
            const cursorXOnTrack = touch.clientX - rect.left;
            const actualLeftEdgeX = cursorXOnTrack - grabOffsetX;
            const snappedMins = calculateLeftEdgeMinutes(actualLeftEdgeX);

            if (draggedBlock) {
              draggedBlock.startMinutes = snappedMins;
              plannerData[day].blocks.push(draggedBlock);
              draggedBlock = null;
            } else {
              plannerData[day].blocks.push({ startMinutes: snappedMins, completed: false, assignedSubject: null });
            }

            saveData();
            renderDays();
            updateCalculations();
          }
        }
      });
    });

    function moveGhost(touch) {
      ghostEl.style.left = `${touch.clientX - grabOffsetX}px`;
      ghostEl.style.top = `${touch.clientY - 19}px`;
    }
  }

  function updateSubjectName(index, newName) {
    const oldName = items[index].name;
    const trimmed = newName.trim();
    if (!trimmed) return;

    items[index].name = trimmed;

    Object.values(plannerData).forEach(day => {
      day.blocks.forEach(b => {
        if (b.assignedSubject === oldName) b.assignedSubject = trimmed;
      });
    });

    saveData();
    renderDays();
    updateCalculations();
  }

  function getActiveDaysCount() {
    return Object.values(plannerData).filter(day => day.blocks.length > 0).length;
  }

  function getWeekTotal() {
    return Object.values(plannerData).reduce((acc, curr) => acc + curr.blocks.length, 0);
  }

  function getWeekFinishedTotal() {
    return Object.values(plannerData).reduce((acc, curr) => acc + curr.blocks.filter(b => b.completed).length, 0);
  }

  function getFinishedCountBySubject(subjectName) {
    let count = 0;
    Object.values(plannerData).forEach(day => {
      day.blocks.forEach(b => {
        if (b.completed && b.assignedSubject === subjectName) count++;
      });
    });
    return count;
  }

  function updateCalculations() {
    const weekTotal = getWeekTotal();
    const weekFinished = getWeekFinishedTotal();
    const activeDays = getActiveDaysCount();

    document.getElementById('week-total-display').innerText = weekTotal;
    document.getElementById('week-finished-display').innerText = weekFinished;

    const bufferSuggested = Math.max(activeDays - 1, 0);
    const extraSuggested = activeDays;

    let suggestions = [];

    if (currentUser === 'Writer') {
      const academicTotal = Math.max(weekTotal - bufferSuggested - extraSuggested, 0);
      let remainingSubjectTotal = academicTotal;
      const subjectsOnly = items.filter(i => i.type === 'subject');

      suggestions = items.map((item) => {
        if (item.type === 'buffer') return bufferSuggested;
        if (item.type === 'extra') return extraSuggested;

        const subjectIdx = subjectsOnly.findIndex(s => s.name === item.name);
        if (subjectIdx === subjectsOnly.length - 1) return remainingSubjectTotal;
        let val = Math.round(academicTotal * item.weight);
        remainingSubjectTotal -= val;
        return val;
      });
    } else {
      const subjectsOnly = items.filter(i => i.type === 'subject');
      const evenBase = subjectsOnly.length > 0 ? Math.floor(weekTotal / subjectsOnly.length) : 0;
      let remainder = subjectsOnly.length > 0 ? weekTotal % subjectsOnly.length : 0;

      suggestions = items.map((item) => {
        if (item.type === 'buffer') return bufferSuggested;
        if (item.type === 'extra') return extraSuggested;

        let val = evenBase + (remainder > 0 ? 1 : 0);
        if (remainder > 0) remainder--;
        return val;
      });
    }

    const tbody = document.getElementById('allocation-tbody');
    tbody.innerHTML = '';

    const totalAllocated = items.reduce((sum, item) => sum + (parseInt(item.allocation) || 0), 0);

    items.forEach((item, idx) => {
      const share = totalAllocated > 0 && item.allocation !== '' 
        ? ((parseInt(item.allocation) / totalAllocated) * 100).toFixed(1) 
        : '0.0';

      const finishedCount = getFinishedCountBySubject(item.name);
      const isSpecial = item.type !== 'subject';
      const tr = document.createElement('tr');
      if (isSpecial) tr.className = 'special-row';

      tr.innerHTML = `
        <td style="text-align: left; font-weight: 600; ${isSpecial ? 'color: var(--primary-dark);' : ''}">
          <span class="subject-tag" style="background-color: ${item.color};"></span>
          ${isSpecial 
            ? item.name 
            : `<input type="text" class="subject-edit-input" value="${item.name}" onchange="updateSubjectName(${idx}, this.value)" placeholder="Subject Name">`
          }
        </td>
        <td>${suggestions[idx]}</td>
        <td><input type="number" min="0" value="${item.allocation}" placeholder="0" onchange="updateAllocation(${idx}, this.value)"></td>
        <td style="font-weight: 700; color: var(--primary-dark);">${finishedCount}</td>
        <td>${share}%</td>
      `;
      tbody.appendChild(tr);
    });

    validateAllocation(totalAllocated, weekTotal);
  }

  function updateAllocation(index, val) {
    items[index].allocation = val === '' ? '' : Math.max(0, parseInt(val) || 0);
    saveData();
    updateCalculations();
  }

  function validateAllocation(allocated, total) {
    const msg = document.getElementById('validation-msg');
    if (total === 0) { msg.style.display = 'none'; return; }

    if (allocated === total) {
      msg.className = 'status-box success';
      msg.innerText = `Great! Let's begin our work!`;
    } else {
      msg.className = 'status-box error';
      msg.innerText = `Let's try to allocate our blocks!! (Allocated: ${allocated} / Total: ${total})`;
    }
  }

  window.onload = initApp;
</script>
</body>
</html>
