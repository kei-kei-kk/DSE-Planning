# DSE-Planning
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>HKDSE Revision Planner Calculator</title>
  <style>
    :root {
      --bg: #f3f4f6;
      --card-bg: #ffffff;
      --primary: #2563eb;
      --primary-dark: #1d4ed8;
      --text: #1f2937;
      --text-muted: #6b7280;
      --border: #e5e7eb;
      --block-grey: #9ca3af;
      --block-blue: #2563eb;
      --danger: #ef4444;
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
      background-color: var(--bg);
      color: var(--text);
      padding: 10px;
      padding-bottom: 40px;
    }

    .container {
      max-width: 900px;
      margin: 0 auto;
    }

    h1 {
      font-size: 1.2rem;
      text-align: center;
      margin-bottom: 10px;
    }

    .card {
      background: var(--card-bg);
      border-radius: 12px;
      padding: 12px;
      margin-bottom: 12px;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
      border: 1px solid var(--border);
    }

    .total-banner {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: linear-gradient(135deg, #1e40af, #3b82f6);
      color: white;
      padding: 12px 16px;
      border-radius: 10px;
      margin-bottom: 12px;
    }

    .total-banner .value {
      font-size: 1.5rem;
      font-weight: 700;
    }

    /* Draggable Source Area */
    .sample-block-area {
      display: flex;
      align-items: center;
      gap: 12px;
      background: #eff6ff;
      border: 2px dashed #93c5fd;
      padding: 10px;
      border-radius: 8px;
      margin-bottom: 14px;
    }

    .sample-block {
      width: 70px;
      height: 36px;
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
      box-shadow: 0 2px 4px rgba(0,0,0,0.15);
      touch-action: none;
    }

    /* Daily Rows */
    .day-row {
      margin-bottom: 12px;
      border-bottom: 1px solid var(--border);
      padding-bottom: 8px;
    }

    .day-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 4px;
    }

    .day-title {
      font-weight: 700;
      font-size: 0.9rem;
    }

    .lock-btn {
      background: #f3f4f6;
      border: 1px solid var(--border);
      padding: 4px 8px;
      border-radius: 6px;
      font-size: 0.75rem;
      cursor: pointer;
    }

    .lock-btn.locked {
      background: #dbeafe;
      color: var(--primary-dark);
      border-color: #93c5fd;
    }

    /* Timeline Styling */
    .timeline-wrapper {
      overflow-x: auto;
      background: #fafafa;
      border: 1px solid var(--border);
      border-radius: 6px;
      padding: 4px;
    }

    .timeline-track {
      position: relative;
      width: 1440px; /* 24 hours * 60px/hr */
      height: 48px;
      background-size: 30px 100%; /* 30-min grid lines */
      background-image: linear-gradient(to right, #e5e7eb 1px, transparent 1px);
    }

    .time-labels {
      position: relative;
      width: 1440px;
      height: 18px;
      border-bottom: 1px solid var(--border);
    }

    .time-label {
      position: absolute;
      font-size: 0.65rem;
      color: var(--text-muted);
      transform: translateX(-50%);
    }

    .placed-block {
      position: absolute;
      top: 4px;
      height: 40px;
      width: 60px; /* 1 Hour = 60px */
      border-radius: 4px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.7rem;
      font-weight: 700;
      color: white;
      cursor: pointer;
      user-select: none;
      box-shadow: 0 1px 3px rgba(0,0,0,0.2);
    }

    .placed-block.grey { background-color: var(--block-grey); }
    .placed-block.blue { background-color: var(--block-blue); }

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

    th { color: var(--text-muted); font-size: 0.75rem; }

    td input {
      width: 55px;
      padding: 4px;
      border: 1px solid var(--border);
      border-radius: 4px;
      text-align: center;
    }

    .status-box {
      margin-top: 10px;
      padding: 8px;
      border-radius: 6px;
      font-size: 0.8rem;
      font-weight: 600;
      display: none;
    }
    .status-box.error { display: block; background: #fef2f2; color: var(--danger); }
    .status-box.success { display: block; background: #ecfdf5; color: var(--success); }
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

    <!-- Unlimited Drag Block Source -->
    <div class="sample-block-area">
      <div class="sample-block" id="sample-block" draggable="true">1 Block</div>
      <span style="font-size: 0.75rem; color: var(--text-muted);">
        <strong>Drag & Drop:</strong> Drag block onto any timeline slot (:00 or :30). Tap placed blocks to delete (unlocked) or complete (locked).
      </span>
    </div>

    <div id="days-container"></div>
  </div>

  <!-- Subject Allocation Section -->
  <div class="card">
    <div style="font-weight: 700; font-size: 0.9rem; margin-bottom: 8px;">Subject Allocation</div>
    <table>
      <thead>
        <tr>
          <th style="text-align: left;">Subject</th>
          <th>Suggested</th>
          <th>Allocated</th>
          <th>Share</th>
        </tr>
      </thead>
      <tbody id="allocation-tbody"></tbody>
    </table>
    <div id="validation-msg" class="status-box"></div>
  </div>
</div>

<script>
  const days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];
  
  // DSE Fixed Subjects with Recommended Weightings (Ref: Images 1-2)
  let subjects = [
    { name: 'Chinese', weight: 0.20, allocation: 0 },
    { name: 'English', weight: 0.20, allocation: 0 },
    { name: 'Mathematics', weight: 0.15, allocation: 0 },
    { name: 'Geography', weight: 0.15, allocation: 0 },
    { name: 'Biology', weight: 0.15, allocation: 0 },
    { name: 'Economics', weight: 0.15, allocation: 0 }
  ];

  let plannerData = {};
  days.forEach(day => {
    plannerData[day] = { blocks: [], locked: false };
  });

  function init() {
    renderDays();
    setupSampleBlockTouch();
    updateCalculations();
  }

  // Render Daily Timelines (12 AM to 12 PM to 12 AM)
  function renderDays() {
    const container = document.getElementById('days-container');
    container.innerHTML = '';

    days.forEach(day => {
      const dayData = plannerData[day];
      const row = document.createElement('div');
      row.className = 'day-row';

      const totalCount = dayData.blocks.length;
      const doneCount = dayData.blocks.filter(b => b.completed).length;

      // Render 24-hour time labels
      let labelsHTML = '';
      for (let h = 0; h <= 24; h += 2) {
        let labelText = h === 0 || h === 24 ? '12AM' : h === 12 ? '12PM' : h > 12 ? `${h-12}PM` : `${h}AM`;
        labelsHTML += `<div class="time-label" style="left: ${h * 60}px;">${labelText}</div>`;
      }

      // Render placed blocks
      let blocksHTML = dayData.blocks.map((b, idx) => {
        const leftPos = b.startMinutes * 1; // 1 min = 1px, 60px per hour
        const timeStr = formatMinutes(b.startMinutes);
        return `
          <div class="placed-block ${dayData.locked ? (b.completed ? 'blue' : 'grey') : 'grey'}" 
               style="left: ${leftPos}px;" 
               onclick="handleBlockClick('${day}', ${idx})">
            ${dayData.locked && b.completed ? '✓' : timeStr}
          </div>`;
      }).join('');

      row.innerHTML = `
        <div class="day-header">
          <span class="day-title">${day}</span>
          <span style="font-size: 0.75rem; color: var(--text-muted);">
            ${dayData.locked ? `Done: ${doneCount} / ` : ''}${totalCount} Blocks
          </span>
          <button class="lock-btn ${dayData.locked ? 'locked' : ''}" onclick="toggleLock('${day}')">
            ${dayData.locked ? '🔒 Locked' : '🔓 Lock'}
          </button>
        </div>
        <div class="timeline-wrapper" ondragover="allowDrop(event)" ondrop="handleDrop(event, '${day}')">
          <div class="time-labels">${labelsHTML}</div>
          <div class="timeline-track" id="track-${day}">
            ${blocksHTML}
          </div>
        </div>
      `;
      container.appendChild(row);
    });
  }

  function formatMinutes(mins) {
    let h = Math.floor(mins / 60);
    let m = mins % 60;
    let ampm = h >= 12 && h < 24 ? 'P' : 'A';
    let displayH = h % 12 === 0 ? 12 : h % 12;
    return `${displayH}:${m === 0 ? '00' : m}${ampm}`;
  }

  // Lock / Unlock Toggle
  function toggleLock(day) {
    plannerData[day].locked = !plannerData[day].locked;
    renderDays();
  }

  // Block Interaction: Tap to Complete (Locked) or Remove (Unlocked)
  function handleBlockClick(day, index) {
    if (plannerData[day].locked) {
      plannerData[day].blocks[index].completed = !plannerData[day].blocks[index].completed;
    } else {
      plannerData[day].blocks.splice(index, 1);
    }
    renderDays();
    updateCalculations();
  }

  // HTML5 Drag & Drop Logic
  function allowDrop(ev) { ev.preventDefault(); }

  function handleDrop(ev, day) {
    ev.preventDefault();
    if (plannerData[day].locked) return;

    const track = document.getElementById(`track-${day}`);
    const rect = track.getBoundingClientRect();
    const dropX = ev.clientX - rect.left;

    // Snap to nearest 30 mins (30px = 30 mins)
    let snappedMins = Math.floor(dropX / 30) * 30;
    if (snappedMins < 0) snappedMins = 0;
    if (snappedMins > 1380) snappedMins = 1380; // Max start 11:00 PM

    plannerData[day].blocks.push({ startMinutes: snappedMins, completed: false });
    renderDays();
    updateCalculations();
  }

  // Touch Support for iPad / Mobile Drag-and-Drop
  function setupSampleBlockTouch() {
    const sample = document.getElementById('sample-block');
    let ghostEl = null;

    sample.addEventListener('touchstart', (e) => {
      const touch = e.touches[0];
      ghostEl = sample.cloneNode(true);
      ghostEl.style.position = 'fixed';
      ghostEl.style.opacity = '0.8';
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
            const dropX = touch.clientX - rect.left;
            let snappedMins = Math.floor(dropX / 30) * 30;
            if (snappedMins < 0) snappedMins = 0;
            if (snappedMins > 1380) snappedMins = 1380;

            plannerData[day].blocks.push({ startMinutes: snappedMins, completed: false });
            renderDays();
            updateCalculations();
          }
        }
      });
    });

    function moveGhost(touch) {
      ghostEl.style.left = `${touch.clientX - 35}px`;
      ghostEl.style.top = `${touch.clientY - 18}px`;
    }
  }

  // Calculations & Subject Suggestions
  function getWeekTotal() {
    return Object.values(plannerData).reduce((acc, curr) => acc + curr.blocks.length, 0);
  }

  function updateCalculations() {
    const weekTotal = getWeekTotal();
    document.getElementById('week-total-display').innerText = weekTotal;

    // Calculate Suggested Blocks to sum EXACTLY to Week Total
    let remaining = weekTotal;
    const suggestions = subjects.map((subj, idx) => {
      if (idx === subjects.length - 1) return remaining; // Final subject takes remainder
      let val = Math.round(weekTotal * subj.weight);
      remaining -= val;
      return val;
    });

    // Render Table
    const tbody = document.getElementById('allocation-tbody');
    tbody.innerHTML = '';

    const totalAllocated = subjects.reduce((sum, s) => sum + (parseInt(s.allocation) || 0), 0);

    subjects.forEach((subj, idx) => {
      const share = totalAllocated > 0 ? ((subj.allocation / totalAllocated) * 100).toFixed(1) : '0.0';
      const tr = document.createElement('tr');
      tr.innerHTML = `
        <td style="text-align: left; font-weight: 600;">${subj.name}</td>
        <td>${suggestions[idx]}</td>
        <td><input type="number" min="0" value="${subj.allocation}" onchange="updateAllocation(${idx}, this.value)"></td>
        <td>${share}%</td>
      `;
      tbody.appendChild(tr);
    });

    validateAllocation(totalAllocated, weekTotal);
  }

  function updateAllocation(index, val) {
    subjects[index].allocation = parseInt(val) || 0;
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
