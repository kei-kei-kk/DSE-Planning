# DSE-Planning
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>HKDSE Revision Planner Calculator</title>
  <style>
    :root {
      --bg: #f4f6f8;
      --card-bg: #ffffff;
      --primary: #2563eb;
      --primary-light: #bfdbfe;
      --primary-dark: #1d4ed8;
      --text: #1f2937;
      --text-muted: #6b7280;
      --border: #e5e7eb;
      --block-grey: #e5e7eb;
      --block-grey-dark: #9ca3af;
      --block-blue: #3b82f6;
      --danger: #ef4444;
      --success: #10b981;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
      -webkit-tap-highlight-color: transparent;
    }

    body {
      background-color: var(--bg);
      color: var(--text);
      padding: 12px;
      padding-bottom: 40px;
    }

    .container {
      max-width: 600px;
      margin: 0 auto;
    }

    h1 {
      font-size: 1.25rem;
      text-align: center;
      margin-bottom: 12px;
      color: #111827;
    }

    .card {
      background: var(--card-bg);
      border-radius: 12px;
      padding: 14px;
      margin-bottom: 14px;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
      border: 1px solid var(--border);
    }

    /* Total Section */
    .total-banner {
      display: flex;
      justify-content: space-between;
      align-items: center;
      background: linear-gradient(135deg, #1e40af, #3b82f6);
      color: white;
      padding: 14px 18px;
      border-radius: 10px;
      margin-bottom: 12px;
    }

    .total-banner .label {
      font-size: 0.95rem;
      font-weight: 500;
    }

    .total-banner .value {
      font-size: 1.6rem;
      font-weight: 700;
    }

    /* Daily Timelines */
    .day-row {
      display: flex;
      flex-direction: column;
      padding: 10px 0;
      border-bottom: 1px solid var(--border);
    }

    .day-row:last-child {
      border-bottom: none;
    }

    .day-header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 8px;
    }

    .day-name {
      font-weight: 600;
      width: 45px;
    }

    .day-count {
      font-size: 0.85rem;
      color: var(--text-muted);
      flex: 1;
      margin-left: 8px;
    }

    .lock-btn {
      background: none;
      border: 1px solid var(--border);
      padding: 4px 8px;
      border-radius: 6px;
      font-size: 0.75rem;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 4px;
    }

    .lock-btn.locked {
      background: #eff6ff;
      border-color: #93c5fd;
      color: var(--primary-dark);
    }

    .timeline-container {
      display: flex;
      gap: 6px;
      overflow-x: auto;
      padding: 4px 0 8px 0;
      min-height: 48px;
      align-items: center;
    }

    .block {
      min-width: 38px;
      height: 38px;
      border-radius: 6px;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 0.75rem;
      font-weight: 600;
      user-select: none;
      flex-shrink: 0;
      transition: all 0.2s ease;
    }

    .block.grey {
      background-color: var(--block-grey);
      color: #4b5563;
      border: 1px dashed var(--block-grey-dark);
    }

    .block.blue {
      background-color: var(--block-blue);
      color: white;
      border: 1px solid #2563eb;
      box-shadow: 0 2px 4px rgba(59, 130, 246, 0.3);
    }

    .add-block-btn {
      min-width: 38px;
      height: 38px;
      border-radius: 6px;
      border: 2px dashed #d1d5db;
      background: none;
      color: #9ca3af;
      font-size: 1.2rem;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      flex-shrink: 0;
    }

    .add-block-btn:active {
      background: #f3f4f6;
    }

    /* Allocation Table */
    .table-wrapper {
      overflow-x: auto;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      text-align: left;
      font-size: 0.85rem;
    }

    th, td {
      padding: 8px 6px;
      border-bottom: 1px solid var(--border);
    }

    th {
      color: var(--text-muted);
      font-weight: 600;
      font-size: 0.75rem;
      text-transform: uppercase;
    }

    td input {
      width: 50px;
      padding: 6px;
      border: 1px solid var(--border);
      border-radius: 4px;
      text-align: center;
      font-size: 0.85rem;
    }

    td input:focus {
      outline: 2px solid var(--primary);
      border-color: transparent;
    }

    .validation-status {
      margin-top: 10px;
      padding: 8px 12px;
      border-radius: 6px;
      font-size: 0.8rem;
      font-weight: 500;
      display: none;
    }

    .validation-status.error {
      display: block;
      background-color: #fef2f2;
      color: var(--danger);
      border: 1px solid #fecaca;
    }

    .validation-status.success {
      display: block;
      background-color: #ecfdf5;
      color: var(--success);
      border: 1px solid #a7f3d0;
    }

    .section-title {
      font-size: 0.95rem;
      font-weight: 600;
      margin-bottom: 10px;
      color: #374151;
    }

    .actions-row {
      display: flex;
      gap: 8px;
      margin-top: 10px;
    }

    .btn {
      flex: 1;
      padding: 8px 12px;
      border: none;
      border-radius: 6px;
      font-size: 0.8rem;
      font-weight: 600;
      cursor: pointer;
      background: var(--bg);
      color: var(--text);
      border: 1px solid var(--border);
    }

    .btn-primary {
      background: var(--primary);
      color: white;
      border: none;
    }
  </style>
</head>
<body>

<div class="container">
  <h1>HKDSE Revision Planner</h1>

  <!-- Week Total Card -->
  <div class="card">
    <div class="total-banner">
      <span class="label">Week Total Blocks:</span>
      <span class="value" id="week-total-display">0</span>
    </div>

    <!-- Daily Timelines -->
    <div id="days-container"></div>
  </div>

  <!-- Subject Allocation Card -->
  <div class="card">
    <div class="section-title">Subject Allocation Calculator</div>
    <div class="table-wrapper">
      <table>
        <thead>
          <tr>
            <th>Subject</th>
            <th>Suggested</th>
            <th>Allocation</th>
            <th>Share</th>
          </tr>
        </thead>
        <tbody id="allocation-tbody"></tbody>
      </table>
    </div>

    <div id="validation-msg" class="validation-status"></div>

    <div class="actions-row">
      <button class="btn" onclick="addSubject()">+ Add Subject</button>
      <button class="btn btn-primary" onclick="autoDistribute()">Auto Fill</button>
    </div>
  </div>
</div>

<script>
  const days = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday'];
  
  // App State
  let plannerData = {
    Monday: { blocks: [], locked: false },
    Tuesday: { blocks: [], locked: false },
    Wednesday: { blocks: [], locked: false },
    Thursday: { blocks: [], locked: false },
    Friday: { blocks: [], locked: false },
    Saturday: { blocks: [], locked: false },
    Sunday: { blocks: [], locked: false }
  };

  let subjects = [
    { name: 'Chinese', allocation: 0 },
    { name: 'English', allocation: 0 },
    { name: 'Maths', allocation: 0 },
    { name: 'Elective 1', allocation: 0 },
    { name: 'Elective 2', allocation: 0 }
  ];

  // Initialize UI
  function init() {
    renderDays();
    renderAllocationTable();
    updateCalculations();
  }

  // Render Daily Planning Section
  function renderDays() {
    const container = document.getElementById('days-container');
    container.innerHTML = '';

    days.forEach(day => {
      const dayData = plannerData[day];
      const row = document.createElement('div');
      row.className = 'day-row';

      const totalBlocks = dayData.blocks.length;
      const completedBlocks = dayData.blocks.filter(b => b.completed).length;

      let blocksHTML = dayData.blocks.map((block, idx) => {
        if (dayData.locked) {
          return `<div class="block ${block.completed ? 'blue' : 'grey'}" onclick="toggleBlockCompletion('${day}', ${idx})">
                    ${block.completed ? '✓' : idx + 1}
                  </div>`;
        } else {
          return `<div class="block grey" onclick="removeBlock('${day}', ${idx})">
                    ${idx + 1}
                  </div>`;
        }
      }).join('');

      const addBtnHTML = !dayData.locked ? `<button class="add-block-btn" onclick="addBlock('${day}')">+</button>` : '';

      row.innerHTML = `
        <div class="day-header">
          <span class="day-name">${day.slice(0, 3)}</span>
          <span class="day-count">${dayData.locked ? `${completedBlocks}/` : ''}${totalBlocks} Blocks</span>
          <button class="lock-btn ${dayData.locked ? 'locked' : ''}" onclick="toggleLock('${day}')">
            ${dayData.locked ? '🔒 Locked' : '🔓 Plan'}
          </button>
        </div>
        <div class="timeline-container">
          ${blocksHTML}
          ${addBtnHTML}
        </div>
      `;
      container.appendChild(row);
    });
  }

  // Block Operations
  function addBlock(day) {
    if (!plannerData[day].locked) {
      plannerData[day].blocks.push({ completed: false });
      renderDays();
      updateCalculations();
    }
  }

  function removeBlock(day, index) {
    if (!plannerData[day].locked) {
      plannerData[day].blocks.splice(index, 1);
      renderDays();
      updateCalculations();
    }
  }

  function toggleBlockCompletion(day, index) {
    if (plannerData[day].locked) {
      plannerData[day].blocks[index].completed = !plannerData[day].blocks[index].completed;
      renderDays();
    }
  }

  function toggleLock(day) {
    plannerData[day].locked = !plannerData[day].locked;
    renderDays();
  }

  // Calculate & Update Values
  function getWeekTotal() {
    return Object.values(plannerData).reduce((acc, curr) => acc + curr.blocks.length, 0);
  }

  function updateCalculations() {
    const weekTotal = getWeekTotal();
    document.getElementById('week-total-display').innerText = weekTotal;

    renderAllocationTable();
    validateAllocation();
  }

  // Allocation Table Rendering
  function renderAllocationTable() {
    const tbody = document.getElementById('allocation-tbody');
    const weekTotal = getWeekTotal();
    tbody.innerHTML = '';

    subjects.forEach((subj, idx) => {
      const suggested = weekTotal > 0 ? Math.round(weekTotal / subjects.length) : 0;
      const totalAllocated = subjects.reduce((sum, s) => sum + (parseInt(s.allocation) || 0), 0);
      const share = totalAllocated > 0 ? ((subj.allocation / totalAllocated) * 100).toFixed(1) : 0;

      const tr = document.createElement('tr');
      tr.innerHTML = `
        <td><input type="text" value="${subj.name}" onchange="updateSubjectName(${idx}, this.value)" style="width: 85px; text-align: left;"></td>
        <td>~${suggested}</td>
        <td><input type="number" min="0" value="${subj.allocation}" onchange="updateAllocation(${idx}, this.value)"></td>
        <td>${share}%</td>
      `;
      tbody.appendChild(tr);
    });

    validateAllocation();
  }

  function updateSubjectName(index, value) {
    subjects[index].name = value;
  }

  function updateAllocation(index, value) {
    subjects[index].allocation = parseInt(value) || 0;
    renderAllocationTable();
  }

  function addSubject() {
    subjects.push({ name: `Subject ${subjects.length + 1}`, allocation: 0 });
    renderAllocationTable();
  }

  function autoDistribute() {
    const weekTotal = getWeekTotal();
    if (weekTotal === 0 || subjects.length === 0) return;

    const baseAllocation = Math.floor(weekTotal / subjects.length);
    let remainder = weekTotal % subjects.length;

    subjects.forEach((subj, i) => {
      subj.allocation = baseAllocation + (i < remainder ? 1 : 0);
    });

    renderAllocationTable();
  }

  function validateAllocation() {
    const weekTotal = getWeekTotal();
    const allocatedSum = subjects.reduce((sum, s) => sum + (parseInt(s.allocation) || 0), 0);
    const msgDiv = document.getElementById('validation-msg');

    if (weekTotal === 0) {
      msgDiv.style.display = 'none';
      return;
    }

    if (allocatedSum === weekTotal) {
      msgDiv.className = 'validation-status success';
      msgDiv.innerText = ` Perfect! Allocated ${allocatedSum} / ${weekTotal} blocks.`;
    } else {
      msgDiv.className = 'validation-status error';
      msgDiv.innerText = ` Mismatch: Allocated ${allocatedSum} blocks, but total is ${weekTotal}.`;
    }
  }

  window.onload = init;
</script>

</body>
</html>

