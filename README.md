<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Canvas Page Creator</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500&family=IBM+Plex+Sans:wght@400;500;600&display=swap" rel="stylesheet" />
<style>
  :root {
    --bg: #0f1117;
    --surface: #181c27;
    --surface2: #1f2535;
    --border: #2b3147;
    --border2: #3a4160;
    --text: #e8eaf0;
    --muted: #7a83a0;
    --accent: #4f80ff;
    --accent-dim: #1e2d5a;
    --green: #3ddc84;
    --green-dim: #1a3a2a;
    --red: #ff5f5f;
    --red-dim: #3a1a1a;
    --yellow: #f5c542;
    --radius: 8px;
    --mono: 'IBM Plex Mono', monospace;
    --sans: 'IBM Plex Sans', sans-serif;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    font-family: var(--sans);
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    padding: 2rem 1rem;
  }

  .shell {
    max-width: 760px;
    margin: 0 auto;
  }

  header {
    margin-bottom: 2rem;
    border-bottom: 1px solid var(--border);
    padding-bottom: 1.25rem;
  }

  header h1 {
    font-family: var(--mono);
    font-size: 1.1rem;
    font-weight: 500;
    color: var(--accent);
    letter-spacing: 0.02em;
  }

  header p {
    font-size: 0.8rem;
    color: var(--muted);
    margin-top: 4px;
    font-family: var(--mono);
  }

  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 1.25rem;
    margin-bottom: 1rem;
  }

  .card-label {
    font-family: var(--mono);
    font-size: 0.68rem;
    font-weight: 500;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.08em;
    margin-bottom: 1rem;
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .card-label::before {
    content: '';
    display: inline-block;
    width: 6px;
    height: 6px;
    border-radius: 50%;
    background: var(--accent);
  }

  .field { margin-bottom: 0.875rem; }
  .field:last-child { margin-bottom: 0; }

  label {
    display: block;
    font-family: var(--mono);
    font-size: 0.72rem;
    color: var(--muted);
    margin-bottom: 5px;
  }

  input[type="text"],
  input[type="password"],
  input[type="number"],
  select,
  textarea {
    width: 100%;
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    color: var(--text);
    font-family: var(--mono);
    font-size: 0.82rem;
    padding: 8px 12px;
    outline: none;
    transition: border-color 0.15s;
    appearance: none;
  }

  input:focus, select:focus, textarea:focus {
    border-color: var(--accent);
  }

  select {
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='12' viewBox='0 0 24 24' fill='none' stroke='%237a83a0' stroke-width='2'%3E%3Cpolyline points='6 9 12 15 18 9'%3E%3C/polyline%3E%3C/svg%3E");
    background-repeat: no-repeat;
    background-position: right 10px center;
    padding-right: 32px;
    cursor: pointer;
  }

  textarea { resize: vertical; min-height: 72px; line-height: 1.5; }

  .grid2 { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
  .grid4 { display: grid; grid-template-columns: repeat(4, 1fr); gap: 10px; }

  .token-wrap { position: relative; }
  .token-wrap input { padding-right: 40px; }
  .eye-btn {
    position: absolute;
    right: 10px;
    top: 50%;
    transform: translateY(-50%);
    background: none;
    border: none;
    cursor: pointer;
    color: var(--muted);
    padding: 0;
    display: flex;
    align-items: center;
    font-size: 0.8rem;
    font-family: var(--mono);
    transition: color 0.15s;
  }
  .eye-btn:hover { color: var(--text); }

  /* Tabs */
  .tabs { display: flex; gap: 4px; margin-bottom: 1rem; }
  .tab {
    font-family: var(--mono);
    font-size: 0.72rem;
    padding: 5px 12px;
    border: 1px solid var(--border);
    border-radius: var(--radius);
    background: transparent;
    color: var(--muted);
    cursor: pointer;
    transition: all 0.15s;
  }
  .tab.active {
    background: var(--accent-dim);
    border-color: var(--accent);
    color: var(--accent);
  }

  .helper {
    font-family: var(--mono);
    font-size: 0.7rem;
    color: var(--muted);
    margin-top: 6px;
    line-height: 1.6;
  }
  .helper code {
    background: var(--surface2);
    border: 1px solid var(--border);
    padding: 1px 5px;
    border-radius: 4px;
    color: var(--yellow);
  }

  /* Preview */
  .preview-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 0.75rem;
  }

  .badge {
    font-family: var(--mono);
    font-size: 0.68rem;
    padding: 2px 8px;
    border-radius: 999px;
    background: var(--accent-dim);
    color: var(--accent);
    border: 1px solid var(--accent);
  }

  .preview-scroll {
    max-height: 200px;
    overflow-y: auto;
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 6px;
  }

  .preview-scroll::-webkit-scrollbar { width: 4px; }
  .preview-scroll::-webkit-scrollbar-track { background: transparent; }
  .preview-scroll::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 4px; }

  .preview-item {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 5px 8px;
    border-radius: 4px;
    font-family: var(--mono);
    font-size: 0.75rem;
    color: var(--text);
  }
  .preview-item:hover { background: var(--surface2); }
  .preview-item .num {
    color: var(--muted);
    min-width: 28px;
    text-align: right;
    font-size: 0.68rem;
  }
  .preview-empty {
    font-family: var(--mono);
    font-size: 0.75rem;
    color: var(--muted);
    padding: 12px 8px;
    text-align: center;
  }

  /* Run section */
  .status-row {
    display: flex;
    align-items: center;
    gap: 8px;
    font-family: var(--mono);
    font-size: 0.75rem;
    color: var(--muted);
    margin-bottom: 10px;
  }

  .status-dot {
    width: 8px;
    height: 8px;
    border-radius: 50%;
    background: var(--border2);
    flex-shrink: 0;
    transition: background 0.3s;
  }
  .status-dot.running { background: var(--yellow); box-shadow: 0 0 6px var(--yellow); }
  .status-dot.done    { background: var(--green);  box-shadow: 0 0 6px var(--green); }
  .status-dot.error   { background: var(--red);    box-shadow: 0 0 6px var(--red); }

  .progress-track {
    height: 3px;
    background: var(--border);
    border-radius: 999px;
    overflow: hidden;
    margin-bottom: 10px;
  }
  .progress-fill {
    height: 100%;
    background: var(--accent);
    border-radius: 999px;
    width: 0%;
    transition: width 0.25s;
  }

  .log {
    background: var(--bg);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 10px 12px;
    font-family: var(--mono);
    font-size: 0.72rem;
    max-height: 220px;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 3px;
    margin-bottom: 1rem;
  }
  .log::-webkit-scrollbar { width: 4px; }
  .log::-webkit-scrollbar-thumb { background: var(--border2); border-radius: 4px; }

  .log-line { display: flex; gap: 8px; line-height: 1.5; }
  .log-line .sym { flex-shrink: 0; }
  .log-ok  .sym { color: var(--green); }
  .log-err .sym { color: var(--red); }
  .log-info .sym { color: var(--muted); }
  .log-ok  .msg { color: var(--text); }
  .log-err .msg { color: var(--red); }
  .log-info .msg { color: var(--muted); }

  .run-btn {
    width: 100%;
    background: var(--accent);
    color: #fff;
    border: none;
    border-radius: var(--radius);
    font-family: var(--mono);
    font-size: 0.82rem;
    font-weight: 500;
    padding: 11px;
    cursor: pointer;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 8px;
    transition: opacity 0.15s, transform 0.1s;
    letter-spacing: 0.02em;
  }
  .run-btn:hover { opacity: 0.88; }
  .run-btn:active { transform: scale(0.99); }
  .run-btn:disabled { opacity: 0.35; cursor: not-allowed; }

  @media (max-width: 520px) {
    .grid2 { grid-template-columns: 1fr; }
    .grid4 { grid-template-columns: 1fr 1fr; }
  }
</style>
</head>
<body>
<div class="shell">

  <header>
    <h1>// canvas_page_creator</h1>
    <p>Bulk-create pages in a Canvas LMS course via the REST API</p>
  </header>

  <!-- Connection -->
  <div class="card">
    <div class="card-label">Connection</div>
    <div class="field">
      <label>Canvas domain</label>
      <input type="text" id="domain" placeholder="yourschool.instructure.com" />
    </div>
    <div class="grid2">
      <div class="field">
        <label>Access token</label>
        <div class="token-wrap">
          <input type="password" id="token" placeholder="Paste token…" />
          <button class="eye-btn" id="toggleToken" title="Toggle visibility">show</button>
        </div>
      </div>
      <div class="field">
        <label>Course ID</label>
        <input type="text" id="courseId" placeholder="e.g. 12345" />
      </div>
    </div>
  </div>

  <!-- Page Names -->
  <div class="card">
    <div class="card-label">Page names</div>
    <div class="tabs">
      <button class="tab active" id="tabSeq" onclick="switchTab('seq')">Sequence pattern</button>
      <button class="tab" id="tabMan" onclick="switchTab('man')">Individual names</button>
    </div>

    <div id="panelSeq">
      <div class="grid4">
        <div class="field" style="grid-column: span 2">
          <label>Prefix</label>
          <input type="text" id="seqPrefix" placeholder="Week " oninput="updatePreview()" />
        </div>
        <div class="field">
          <label>Start #</label>
          <input type="number" id="seqStart" min="1" value="1" oninput="updatePreview()" />
        </div>
        <div class="field">
          <label>Pad digits</label>
          <input type="number" id="seqPad" min="1" max="4" value="2" oninput="updatePreview()" />
        </div>
      </div>
      <div class="grid2">
        <div class="field">
          <label>Number of pages</label>
          <input type="number" id="seqCount" min="1" max="300" value="12" oninput="updatePreview()" />
        </div>
        <div class="field">
          <label>Suffix (optional)</label>
          <input type="text" id="seqSuffix" placeholder="" oninput="updatePreview()" />
        </div>
      </div>
      <p class="helper">Generates: <code>Week 01</code>, <code>Week 02</code>, … Zero-padded automatically.</p>
    </div>

    <div id="panelMan" style="display:none">
      <div class="field">
        <label>Names — one per line, or bracket list</label>
        <textarea id="manualNames" rows="5"
          placeholder="Week 01&#10;Week 02&#10;Week 03&#10;&#10;— or bracket syntax —&#10;[Monday...Tuesday...Wednesday]"
          oninput="updatePreview()"></textarea>
      </div>
      <p class="helper">
        Put each name on its own line, <b>or</b> use bracket syntax: <code>[Item A...Item B...Item C]</code>
        with items separated by <code>...</code>
      </p>
    </div>
  </div>

  <!-- Preview -->
  <div class="card">
    <div class="preview-header">
      <div class="card-label" style="margin-bottom:0">Preview</div>
      <span class="badge" id="countBadge">0 pages</span>
    </div>
    <div class="preview-scroll" id="previewList">
      <div class="preview-empty">Fill in the fields above…</div>
    </div>
  </div>

  <!-- Page Options -->
  <div class="card">
    <div class="card-label">Page options</div>
    <div class="grid2">
      <div class="field">
        <label>Editing roles</label>
        <select id="editingRoles">
          <option value="teachers">Teachers only</option>
          <option value="students">Students</option>
          <option value="public">Public</option>
        </select>
      </div>
      <div class="field">
        <label>Publish state</label>
        <select id="publishState">
          <option value="false">Unpublished</option>
          <option value="true">Published</option>
        </select>
      </div>
    </div>
    <div class="field">
      <label>Page body HTML (applied to all pages)</label>
      <textarea id="bodyContent" rows="3" placeholder="&lt;p&gt;Page content here…&lt;/p&gt;"></textarea>
    </div>
  </div>

  <!-- Run -->
  <div class="card">
    <div class="status-row">
      <div class="status-dot" id="statusDot"></div>
      <span id="statusText">Ready</span>
    </div>
    <div class="progress-track"><div class="progress-fill" id="progressFill"></div></div>
    <div class="log" id="logBox">
      <div class="log-line log-info"><span class="sym">—</span><span class="msg">Waiting to run…</span></div>
    </div>
    <button class="run-btn" id="runBtn" onclick="runCreation()">
      ▶ &nbsp;Create pages
    </button>
  </div>

</div>

<script>
  let activeTab = 'seq';

  function switchTab(tab) {
    activeTab = tab;
    document.getElementById('panelSeq').style.display = tab === 'seq' ? '' : 'none';
    document.getElementById('panelMan').style.display = tab === 'man' ? '' : 'none';
    document.getElementById('tabSeq').className = 'tab' + (tab === 'seq' ? ' active' : '');
    document.getElementById('tabMan').className = 'tab' + (tab === 'man' ? ' active' : '');
    updatePreview();
  }

  function pad(n, d) { return String(n).padStart(d, '0'); }

  function parseManualNames(raw) {
    const m = raw.match(/^\[(.+?)\]$/s);
    if (m) return m[1].split('...').map(s => s.trim()).filter(Boolean);
    return raw.split('\n').map(s => s.trim()).filter(Boolean);
  }

  function getPageNames() {
    if (activeTab === 'seq') {
      const prefix = document.getElementById('seqPrefix').value;
      const suffix = document.getElementById('seqSuffix').value;
      const count  = parseInt(document.getElementById('seqCount').value)  || 0;
      const start  = parseInt(document.getElementById('seqStart').value)  || 1;
      const padLen = parseInt(document.getElementById('seqPad').value)    || 2;
      const out = [];
      for (let i = 0; i < count; i++) out.push(prefix + pad(start + i, padLen) + suffix);
      return out;
    } else {
      const raw = document.getElementById('manualNames').value.trim();
      return raw ? parseManualNames(raw) : [];
    }
  }

  function esc(s) {
    return String(s).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');
  }

  function updatePreview() {
    const names = getPageNames();
    const badge = document.getElementById('countBadge');
    const list  = document.getElementById('previewList');
    badge.textContent = names.length + (names.length === 1 ? ' page' : ' pages');
    if (!names.length) {
      list.innerHTML = '<div class="preview-empty">No pages to preview yet.</div>';
      return;
    }
    const shown = names.slice(0, 60);
    list.innerHTML = shown.map((n, i) =>
      `<div class="preview-item"><span class="num">${i + 1}</span><span>${esc(n)}</span></div>`
    ).join('') + (names.length > 60
      ? `<div class="preview-item" style="color:var(--muted)"><span class="num">…</span><span>and ${names.length - 60} more</span></div>`
      : '');
  }

  document.getElementById('toggleToken').addEventListener('click', function() {
    const inp = document.getElementById('token');
    if (inp.type === 'password') { inp.type = 'text'; this.textContent = 'hide'; }
    else                         { inp.type = 'password'; this.textContent = 'show'; }
  });

  function addLog(msg, type) {
    const box = document.getElementById('logBox');
    const sym = type === 'ok' ? '✓' : type === 'err' ? '✗' : '—';
    const el = document.createElement('div');
    el.className = 'log-line log-' + type;
    el.innerHTML = `<span class="sym">${sym}</span><span class="msg">${esc(msg)}</span>`;
    box.appendChild(el);
    box.scrollTop = box.scrollHeight;
  }

  function setStatus(state, text) {
    document.getElementById('statusDot').className = 'status-dot ' + state;
    document.getElementById('statusText').textContent = text;
  }

  async function createPage(domain, token, courseId, title, body, editing, published) {
    const res = await fetch(`https://${domain}/api/v1/courses/${courseId}/pages`, {
      method: 'POST',
      headers: {
        'Authorization': 'Bearer ' + token,
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        wiki_page: {
          title,
          body,
          editing_roles: editing,
          published: published === 'true'
        }
      })
    });
    if (!res.ok) {
      const txt = await res.text().catch(() => String(res.status));
      throw new Error(`HTTP ${res.status}: ${txt}`);
    }
    return res.json();
  }

  async function runCreation() {
    const domain    = document.getElementById('domain').value.trim().replace(/^https?:\/\//, '').replace(/\/$/, '');
    const token     = document.getElementById('token').value.trim();
    const courseId  = document.getElementById('courseId').value.trim();
    const names     = getPageNames();
    const body      = document.getElementById('bodyContent').value;
    const editing   = document.getElementById('editingRoles').value;
    const published = document.getElementById('publishState').value;

    const logBox = document.getElementById('logBox');
    logBox.innerHTML = '';

    if (!domain)    { addLog('Canvas domain is required.', 'err'); return; }
    if (!token)     { addLog('Access token is required.', 'err'); return; }
    if (!courseId)  { addLog('Course ID is required.', 'err'); return; }
    if (!names.length) { addLog('No page names to create.', 'err'); return; }

    const btn = document.getElementById('runBtn');
    btn.disabled = true;
    setStatus('running', `Creating ${names.length} pages…`);
    addLog(`Starting — ${names.length} pages in course ${courseId}`, 'info');

    const fill = document.getElementById('progressFill');
    let ok = 0, err = 0;

    for (let i = 0; i < names.length; i++) {
      try {
        await createPage(domain, token, courseId, names[i], body, editing, published);
        addLog(`Created: ${names[i]}`, 'ok');
        ok++;
      } catch(e) {
        addLog(`Failed: ${names[i]} — ${e.message}`, 'err');
        err++;
      }
      fill.style.width = Math.round(((i + 1) / names.length) * 100) + '%';
      await new Promise(r => setTimeout(r, 120));
    }

    if (err === 0) {
      setStatus('done', `Done — ${ok} pages created`);
      addLog(`All ${ok} pages created successfully.`, 'ok');
    } else {
      setStatus('error', `Done with errors — ${ok} ok, ${err} failed`);
      addLog(`Finished: ${ok} succeeded, ${err} failed.`, 'err');
    }
    btn.disabled = false;
  }

  updatePreview();
</script>
</body>
</html>
