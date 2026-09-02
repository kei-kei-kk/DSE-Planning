<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>HKDSE Revision Planner Calculator</title>
  <style>
    :root {
      /* Calming Study-Focused Blue Palette */
      --bg: #f0f4f8;
      --card-bg: #ffffff;
      --primary: #3b82f6;
      --primary-dark: #1d4ed8;
      --accent-blue: #60a5fa;
      --text: #1e293b;
      --text-muted: #64748b;
      --border: #cbd5e1;
      
      /* Block Colors - Soft/Gentle Tones */
      --block-grey: #94a3b8;      /* Soft Slate Grey (Planned) */
      --block-green: #34d399;     /* Gentle Sage/Emerald Green (Completed) */
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

    .container {
      max-width: 900px;
      margin: 0 auto;
    }

    h1 {
      font-size: 1.25rem;
      text-align: center;
      margin-bottom: 12px;
      color: #0f172a;
      font-weight: 700;
    }

    .card {
      background: var(--card-bg);
      border-radius: 14px;
      padding: 14px;
      margin-bottom: 14px;
      box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -1px rgba(0, 0, 0, 0.03);
      border: 1px solid rgba(226, 232, 240, 0.8);
    }

    .total-banner {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: linear-gradient(135deg, #2563eb, #3b82f6);
      color: white;
      padding: 14px 18px;
      border-radius: 10px;
      margin-bottom: 14px;
      box-shadow: 0 2px 4px rgba(37, 99, 235, 0.2);
    }

    .total-banner .value {
      font-size: 1.6rem;
      font-weight: 700;
    }

    /* Draggable Sample Block Area */
    .sample-block-area {
      display: flex;
      align-items: center;
      gap: 12px;
      background: #eff6ff;
      border: 2px dashed #bfdbfe;
      padding: 10px 14px;
      border-radius: 10px;
      margin-bottom: 16px;
    }

    .sample-block {
      width: 75px;
      height: 38px;
      background: var(--block-grey);
      color: white;
      font-weight: 600;
      font-size: 0.75rem;
      border-radius: 6px;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: grab;
      user-select: none;
      box-shadow: 0 2px 4px rgba(0,0,0,0.12);
      touch-action: none;
      flex-shrink: 0;
    }

    /* Daily Timelines */
    .day-row {
      margin-bottom: 14px;
      border-bottom: 1px solid var(--border);
      padding-bottom: 10px;
    }

    .day-row:last-child {
      border-bottom: none;
    }

    .day-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 6px;
    }

    .day-title {
      font-weight: 700;
      font-size: 0.9rem;
      color: #334155;
    }

    .lock-btn {
      background: #f1f5f9;
      border: 1px solid var(--border);
      padding: 4px 10px;
      border-radius: 6px;
      font-size: 0.75rem;
      font-weight: 600;
      cursor: pointer;
      color: #475569;
      transition: all 0.2s ease;
    }

    .lock-btn.locked {
      background: #dbeafe;
      color: var(--primary-dark);
      border-color: #93c5fd;
    }

    /* Scrollable Timeline */
    .timeline-wrapper {
      overflow-x: auto;
      background: #f8fafc;
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 4px;
      -webkit-overflow-scrolling: touch;
    }

    .timeline-track {
      position: relative;
      width: 1440px; /* 24 hrs * 60px/hr */
      height: 48px;
      background-size: 30px 100%; /* Grid lines every 30 mins */
      background-image: linear-gradient(to right, #e2e8f0 1px, transparent 1px);
    }

    .time-labels {
      position: relative;
      width: 1440px;
      height: 20px;
      border-bottom: 1px solid var(--border);
    }

    .time-label {
      position: absolute;
      font-size: 0.65rem;
      color: var(--text-muted);
      transform: translateX(-50%);
      font-weight: 500;
    }

    .placed-block {
      position: absolute;
      top: 4px;
      height: 40px;
      width: 60px; /* 1 Hour = 60px */
      border-radius: 6px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.7rem;
      font-weight: 700;
      color: white;
      cursor: pointer;
      user-select: none;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
      transition: background-color 0.2s ease, transform 0.15s ease;
    }

    .placed-block:active {
      transform: scale(0.96);
    }

    .placed-block.grey { 
      background-color: var(--block-grey); 
    }
    
    .placed-block.green { 
      background-color: var(--block-green); 
      color: #064e3b;
      box-shadow: 0 2px 4px rgba(16, 185, 129, 0.2);
    }

    /* Allocation Table */
    table {
      width: 100%;
      border-collapse: collapse;
      font-size: 0.85rem;
    }

    th, td {
      padding: 8px 4px;
      border-bottom: 1px solid var(--border);
      text-align: center;
    }

    th { 
      color: var(--text-muted); 
      font-size: 0.75rem; 
      font-weight: 600;
    }

    td input {
      width: 55px;
      padding: 6px;
      border: 1px solid var(--border);
      border-radius: 6px;
      text-align: center;
      font-weight: 600;
      color: var(--text);
    }

    td input:focus {
      outline: 2px solid var(--primary);
      border-color: transparent;
    }

    .special-row {
      background-color: #f8fafc;
    }

    .status-box {
      margin-top: 12px;
      padding: 10px;
      border-radius: 8px;
      font-size: 0.8rem;
      font-weight: 600;
      display: none;
    }
    .status-box.error { display: block; background: #fef2f2; color: var(--danger); border: 1px solid #fecaca; }
    .status-box.success { display: block; background: #ecfdf5; color: var(--success); border: 1px solid #a7f3d0; }

    /* Gentle Red Reset Button */
    .reset-area {
      display: flex;
      justify-content: center;
      margin-top: 18px;
      margin-bottom: 12px;
    }

    .reset-btn {
      background-color: var(--danger-soft);
      color: var(--danger);
      border: 1px solid var(--danger-border);
      padding: 10px 20px;
      border-radius: 8px;
      font-size: 0.825rem;
      font-weight: 600;
      cursor: pointer;
      transition: all 0.2s ease;
      display: flex;
      align-items: center;
      gap: 6px;
    }

    .reset-btn:hover {
      background-color: var(--danger-hover);
    }

    .reset-btn:active {
      transform: scale(0.98);
    }
  </style>
</head>
<body>

<div class="container">
  <h1>HKDSE Revision Planner</h1>

  <div class="card">
    <div class="total-banner">
      <span>Week Total Blocks (Hours):</span>
      <span class="value" id="week-total-display">0</span>
    </div>

    <!-- Unlimited Sample Block Source -->
    <div class="sample-block-area">
      <div class="sample-block" id="sample-block" draggable="true">1 Block</div>
      <span style="font-size: 0.75rem; color: var(--text-muted);">
        <strong>Placement:</strong> Block snaps accurately based on its left edge. Tap to remove/complete. <strong>Long press (0.8s)</strong> to duplicate!
      </span>
    </div>

    <div id="days-container"></div>
  </div>

  <!-- Subject Allocation Section -->
  <div class="card">
    <div style="font-weight: 700; font-size: 0.95rem; margin-bottom: 10px; color: #334155;">Time Allocation</div>
    <table>
      <thead>
        <tr>
          <th style="text-align: left;">Category</th>
          <th>Suggested</th>
          <th>Allocated</th>
          <th>Share</th>
        </tr>
      </thead>
      <tbody id="allocation-tbody"></tbody>
    </table>
    <div id="validation-msg" class="status-box"></div>
  </div>

  <!-- Clear Record Reset Area -->
  <div class="reset-area">
    <button class="reset-btn" onclick="clearPlannerData()">
      🗑 Clear All Records & Reset
    </button>
  </div>
</div>

<script>
  const days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];
  
  const defaultItems = [
    { name: 'Chinese', weight: 0.20, allocation: 0, type: 'subject' },
    { name: 'English', weight: 0.20, allocation: 0, type: 'subject' },
    { name: 'Mathematics', weight: 0.15, allocation: 0, type: 'subject' },
    { name: 'Geography', weight: 0.15, allocation: 0, type: 'subject' },
    { name: 'Biology', weight: 0.15, allocation: 0, type: 'subject' },
    { name: 'Economics', weight: 0.15, allocation: 0, type: 'subject' },
    { name: 'Buffer Time', weight: 0, allocation: 0, type: 'buffer' },
    { name: 'Extra Learning', weight: 0, allocation: 0, type: 'extra' }
  ];

  let items = [];
  let plannerData = {};
  let scrollPositions = {};
  let grabOffsetX = 0;

  // Local Storage Logic
  function loadSavedData() {
    const savedPlanner = localStorage.getItem('hkdse_plannerData');
    const savedItems = localStorage.getItem('hkdse_items');

    if (savedPlanner) {
      plannerData = JSON.parse(savedPlanner);
    } else {
      plannerData = {};
      days.forEach(day => {
        plannerData[day] = { blocks: [], locked: false };
      });
    }

    if (savedItems) {
      items = JSON.parse(savedItems);
    } else {
      items = JSON.parse(JSON.stringify(defaultItems));
    }
  }

  function saveData() {
    localStorage.setItem('hkdse_plannerData', JSON.stringify(plannerData));
    localStorage.setItem('hkdse_items', JSON.stringify(items));
  }

  function clearPlannerData() {
    if (confirm('Are you sure you want to clear all timeline blocks and allocations?')) {
      localStorage.removeItem('hkdse_plannerData');
      localStorage.removeItem('hkdse_items');
      loadSavedData();
      renderDays();
      updateCalculations();
    }
  }

  function saveScrollPositions() {
    days.forEach(day => {
      const wrapper = document.getElementById(`wrapper-${day}`);
      if (wrapper) {
        scrollPositions[day] = wrapper.scrollLeft;
      }
    });
  }

  function restoreScrollPositions() {
    days.forEach(day => {
      const wrapper = document.getElementById(`wrapper-${day}`);
      if (wrapper && scrollPositions[day] !== undefined) {
        wrapper.scrollLeft = scrollPositions[day];
      }
    });
  }

  function init() {
    loadSavedData();
    renderDays();
    setupSampleBlockEvents();
    updateCalculations();
  }

  // Render Daily Timelines (12 AM to 12 PM to 12 AM)
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
        return `
          <div class="placed-block ${dayData.locked ? (b.completed ? 'green' : 'grey') : 'grey'}" 
               style="left: ${leftPos}px;" 
               onmousedown="startLongPress(event, '${day}', ${idx})"
               onmouseleave="cancelLongPress()"
               onmouseup="cancelLongPress()"
               ontouchstart="startLongPress(event, '${day}', ${idx})"
               ontouchend="cancelLongPress()"
               onclick="handleBlockClick('${day}', ${idx})">
            ${dayData.locked && b.completed ? '✓' : timeStr}
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

  // Long-Press Duplicate Logic (0.8s)
  let longPressTimer = null;
  let isLongPressTriggered = false;

  function startLongPress(e, day, index) {
    if (plannerData[day].locked) return;
    isLongPressTriggered = false;

    longPressTimer = setTimeout(() => {
      isLongPressTriggered = true;
      duplicateBlock(day, index);
    }, 800);
  }

  function cancelLongPress() {
    if (longPressTimer) {
      clearTimeout(longPressTimer);
      longPressTimer = null;
    }
  }

  function duplicateBlock(day, index) {
    const originalBlock = plannerData[day].blocks[index];
    if (!originalBlock) return;

    let nextStartMins = originalBlock.startMinutes + 60;
    if (nextStartMins > 1380) nextStartMins = 1380;

    plannerData[day].blocks.push({ startMinutes: nextStartMins, completed: false });
    saveData();
    renderDays();
    updateCalculations();
  }

  function toggleLock(day) {
    plannerData[day].locked = !plannerData[day].locked;
    saveData();
    renderDays();
  }

  function handleBlockClick(day, index) {
    if (isLongPressTriggered) {
      isLongPressTriggered = false;
      return;
    }

    if (plannerData[day].locked) {
      plannerData[day].blocks[index].completed = !plannerData[day].blocks[index].completed;
    } else {
      plannerData[day].blocks.splice(index, 1);
    }
    saveData();
    renderDays();
    updateCalculations();
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

    plannerData[day].blocks.push({ startMinutes: snappedMins, completed: false });
    saveData();
    renderDays();
    updateCalculations();
  }

  function setupSampleBlockEvents() {
    const sample = document.getElementById('sample-block');
    let ghostEl = null;

    sample.addEventListener('dragstart', (e) => {
      const rect = sample.getBoundingClientRect();
      grabOffsetX = e.clientX - rect.left;
    });

    sample.addEventListener('touchstart', (e) => {
      const touch = e.touches[0];
      const rect = sample.getBoundingClientRect();
      grabOffsetX = touch.clientX - rect.left;

      ghostEl = sample.cloneNode(true);
      ghostEl.style.position = 'fixed';
      ghostEl.style.opacity = '0.85';
      ghostEl.style.pointerEvents = 'none';
      ghostEl.style.zIndex = '1000';
      document.body.appendChild(ghostEl);
      moveGhost(touch);
    });

    sample.addEventListener('touchmove', (e) => {
      if (ghostEl) moveGhost(e.touches[0]);
    });

    sample.addEventListener('touchend', (e) => {
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

            plannerData[day].blocks.push({ startMinutes: snappedMins, completed: false });
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

  // Count active days filled with at least 1 block
  function getActiveDaysCount() {
    return Object.values(plannerData).filter(day => day.blocks.length > 0).length;
  }

  function getWeekTotal() {
    return Object.values(plannerData).reduce((acc, curr) => acc + curr.blocks.length, 0);
  }

  function updateCalculations() {
    const weekTotal = getWeekTotal();
    const activeDays = getActiveDaysCount();
    document.getElementById('week-total-display').innerText = weekTotal;

    // Buffer: Active Days - 1 (min 0)
    const bufferSuggested = Math.max(activeDays - 1, 0);
    // Extra Learning: Active Days
    const extraSuggested = activeDays;

    // Remaining total for academic subjects after Buffer & Extra
    const academicTotal = Math.max(weekTotal - bufferSuggested - extraSuggested, 0);

    let remainingSubjectTotal = academicTotal;
    const subjectsOnly = items.filter(i => i.type === 'subject');

    const suggestions = items.map((item) => {
      if (item.type === 'buffer') return bufferSuggested;
      if (item.type === 'extra') return extraSuggested;

      // Calculate subject distribution dynamically
      const subjectIdx = subjectsOnly.findIndex(s => s.name === item.name);
      if (subjectIdx === subjectsOnly.length - 1) return remainingSubjectTotal;
      let val = Math.round(academicTotal * item.weight);
      remainingSubjectTotal -= val;
      return val;
    });

    const tbody = document.getElementById('allocation-tbody');
    tbody.innerHTML = '';

    const totalAllocated = items.reduce((sum, item) => sum + (parseInt(item.allocation) || 0), 0);

    items.forEach((item, idx) => {
      const share = totalAllocated > 0 ? ((item.allocation / totalAllocated) * 100).toFixed(1) : '0.0';
      const isSpecial = item.type !== 'subject';
      const tr = document.createElement('tr');
      if (isSpecial) tr.className = 'special-row';

      tr.innerHTML = `
        <td style="text-align: left; font-weight: 600; ${isSpecial ? 'color: var(--primary-dark);' : ''}">${item.name}</td>
        <td>${suggestions[idx]}</td>
        <td><input type="number" min="0" value="${item.allocation}" onchange="updateAllocation(${idx}, this.value)"></td>
        <td>${share}%</td>
      `;
      tbody.appendChild(tr);
    });

    validateAllocation(totalAllocated, weekTotal);
  }

  function updateAllocation(index, val) {
    items[index].allocation = parseInt(val) || 0;
    saveData();
    updateCalculations();
  }

  function validateAllocation(allocated, total) {
    const msg = document.getElementById('validation-msg');
    if (total === 0) { msg.style.display = 'none'; return; }

    if (allocated === total) {
      msg.className = 'status-box success';
      msg.innerText = `✓ Perfect! Total allocated blocks (${allocated}) equals Week Total (${total}).`;
    } else {
      msg.className = 'status-box error';
      msg.innerText = `⚠ Mismatch: Allocated ${allocated} blocks, but Week Total is ${total}. Adjust input values.`;
    }
  }

  window.onload = init;
</script>
</body>
</html>
