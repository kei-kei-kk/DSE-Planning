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
      --text: #1e293b;
      --text-muted: #64748b;
      --border: #cbd5e1;
      
      --block-grey: #94a3b8;
      --danger: #ef4444;
      --danger-soft: #fef2f2;
      --danger-border: #fecaca;
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

    .container { max-width: 900px; margin: 0 auto; }

    .user-bar {
      display: flex; justify-content: space-between; align-items: center;
      background: rgba(255, 255, 255, 0.7); padding: 8px 14px;
      border-radius: 10px; margin-bottom: 12px; font-size: 0.85rem; font-weight: 600;
    }

    h1 { font-size: 1.25rem; text-align: center; margin-bottom: 12px; color: #0f172a; font-weight: 700; }

    .card {
      background: var(--card-bg); border-radius: 14px; padding: 14px;
      margin-bottom: 14px; box-shadow: 0 4px 6px -1px rgba(0,0,0,0.05);
      border: 1px solid rgba(226, 232, 240, 0.8);
    }

    /* Total Banner Right-Aligned */
    .total-banner {
      display: flex; justify-content: flex-end; align-items: center;
      background: linear-gradient(135deg, #2563eb, #3b82f6); color: white;
      padding: 10px 18px; border-radius: 10px; margin-bottom: 14px;
      box-shadow: 0 2px 4px rgba(37, 99, 235, 0.2);
    }

    .total-banner .value-group { font-size: 1.1rem; font-weight: 700; text-align: right; }

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

    .subject-tag { display: inline-block; width: 10px; height: 10px; border-radius: 50%; margin-right: 6px; }
    .special-row { background-color: #f8fafc; }

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

<div class="container" id="main-app">
  <div class="user-bar">
    <span>Profile: <strong>Writer Mode</strong></span>
  </div>

  <h1>HKDSE Revision Planner</h1>

  <!-- Sticky Top Drop Zone -->
  <div class="sticky-drop-bar">
    <div class="sample-block" id="sticky-sample-block" draggable="true">1 Block</div>
    <span style="font-size: 0.75rem; color: var(--text-muted); font-weight: 600;">
      <strong>Quick Drop:</strong> Drag or touch-drag this block down to any timeline!
    </span>
  </div>

  <div class="card">
    <!-- Total Week Blocks Display (Right Aligned) -->
    <div class="total-banner">
      <div class="value-group">
        Total Week Blocks : <span id="week-finished-display">0</span> / <span id="week-total-display">0</span>
      </div>
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
          <th>Finished</th>
          <th>Share</th>
        </tr>
      </thead>
      <tbody id="allocation-tbody"></tbody>
    </table>

    <div id="validation-msg" class="status-box"></div>
  </div>

  <div class="reset-area">
    <button class="reset-btn" onclick="clearPlannerData()">
      🗑 Clear All Records & Reset
    </button>
  </div>
</div>

<!-- Modal for selecting subject when completing block -->
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

  let items = JSON.parse(JSON.stringify(defaultWriterItems));
  let plannerData = {};
  let scrollPositions = {};
  let grabOffsetX = 0;

  let activeBlockContext = null;
  let draggedBlock = null;

  // Fixed Press Handlers
  let holdTimer = null;
  let dupInterval = null;
  let isHoldPickedUp = false;
  let isDupActive = false;

  function initApp() {
    loadSavedData();
    setupSampleBlockEvents();
    renderDays();
    updateCalculations();
  }

  function loadSavedData() {
    const localData = localStorage.getItem('hkdse_writer_planner');
    const localItems = localStorage.getItem('hkdse_writer_items');
    
    plannerData = localData ? JSON.parse(localData) : createEmptyPlannerData();
    if (localItems) items = JSON.parse(localItems);
  }

  function saveData() {
    localStorage.setItem('hkdse_writer_planner', JSON.stringify(plannerData));
    localStorage.setItem('hkdse_writer_items', JSON.stringify(items));
  }

  function createEmptyPlannerData() {
    let pData = {};
    days.forEach(day => { pData[day] = { blocks: [], locked: false }; });
    return pData;
  }

  function clearPlannerData() {
    if (confirm('Are you sure you want to clear all timeline blocks and allocations?')) {
      plannerData = createEmptyPlannerData();
      items = JSON.parse(JSON.stringify(defaultWriterItems));
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
               onmousedown="handleBlockMouseDown(event, '${day}', ${idx})"
               onmouseup="handleBlockMouseUp(event, '${day}', ${idx})"
               onmouseleave="handleBlockMouseLeave(event)"
               ontouchstart="handleBlockTouchStart(event, '${day}', ${idx})"
               ontouchend="handleBlockTouchEnd(event, '${day}', ${idx})">
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

  /* Fixed 0.2s Pick-up & 0.8s Continuous Duplication Engine */
  function handleBlockMouseDown(e, day, idx) { startHoldTimers(day, idx); }
  function handleBlockTouchStart(e, day, idx) { startHoldTimers(day, idx); }

  function startHoldTimers(day, idx) {
    if (plannerData[day].locked) return;
    
    cancelHoldTimers();
    isHoldPickedUp = false;
    isDupActive = false;

    // 0.2-second pickup logic
    holdTimer = setTimeout(() => {
      isHoldPickedUp = true;
      draggedBlock = plannerData[day].blocks[idx];
      plannerData[day].blocks.splice(idx, 1);
      saveData();
      renderDays();
    }, 200);

    // 0.8-second continuous duplication logic
    dupInterval = setTimeout(() => {
      cancelHoldTimers();
      isDupActive = true;
      
      let baseBlock = plannerData[day].blocks[idx];
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
          cancelHoldTimers();
        }
      }, 200);
    }, 800);
  }

  function handleBlockMouseUp(e, day, idx) { processBlockRelease(day, idx); }
  function handleBlockTouchEnd(e, day, idx) { processBlockRelease(day, idx); }
  function handleBlockMouseLeave(e) { cancelHoldTimers(); }

  function processBlockRelease(day, idx) {
    const tookAction = isHoldPickedUp || isDupActive;
    cancelHoldTimers();

    // If it was a quick single click/tap without holding
    if (!tookAction) {
      handleBlockClick(day, idx);
    }
  }

  function cancelHoldTimers() {
    if (holdTimer) { clearTimeout(holdTimer); holdTimer = null; }
    if (dupInterval) { clearInterval(dupInterval); clearTimeout(dupInterval); dupInterval = null; }
  }

  function toggleLock(day) {
    plannerData[day].locked = !plannerData[day].locked;
    saveData();
    renderDays();
  }

  function handleBlockClick(day, index) {
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

    const academicTotal = Math.max(weekTotal - bufferSuggested - extraSuggested, 0);
    let remainingSubjectTotal = academicTotal;
    const subjectsOnly = items.filter(i => i.type === 'subject');

    const suggestions = items.map((item) => {
      if (item.type === 'buffer') return bufferSuggested;
      if (item.type === 'extra') return extraSuggested;

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
      const share = totalAllocated > 0 && item.allocation !== '' 
        ? ((parseInt(item.allocation) / totalAllocated) * 100).toFixed(1) 
        : '0.0';

      const finishedCount = getFinishedCountBySubject(item.name);
      const isSpecial = item.type !== 'subject';
      const tr = document.createElement('tr');
      if (isSpecial) tr.className = 'special-row';

      // Read-only subject names (no editing inputs)
      tr.innerHTML = `
        <td style="text-align: left; font-weight: 600; ${isSpecial ? 'color: var(--primary-dark);' : ''}">
          <span class="subject-tag" style="background-color: ${item.color};"></span>
          ${item.name}
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
