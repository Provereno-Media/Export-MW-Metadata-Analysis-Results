
<!DOCTYPE html>
<html lang="en" data-theme="dark">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>MW Metadata Exporter — Bookmarklet</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
<style>
:root,[data-theme="dark"]{
  --bg:#171614;--surface:#1c1b19;--surface-2:#22211f;--surface-3:#2d2c2a;
  --border:#393836;--divider:#262523;
  --text:#cdccca;--muted:#797876;--faint:#5a5957;
  --primary:#4f98a3;--primary-h:#227f8b;
  --green:#6daa45;--green-h:#4d8f25;
  --blue:#5591c7;--blue-h:#3b78ab;
  --orange:#fdab43;
  --radius-sm:6px;--radius-md:10px;--radius-lg:14px;--radius-xl:18px;
  --font-body:'Inter','Segoe UI',sans-serif;
  --font-mono:'JetBrains Mono','Fira Code',monospace;
  --shadow:0 12px 40px rgba(0,0,0,.5);
}
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0}
html{-webkit-font-smoothing:antialiased;scroll-behavior:smooth}
body{background:var(--bg);color:var(--text);font-family:var(--font-body);
     font-size:15px;line-height:1.6;min-height:100dvh}

a{color:var(--primary);text-decoration:none}
a:hover{color:var(--primary-h);text-decoration:underline}

.page{max-width:760px;margin:0 auto;padding:48px 24px 80px}

/* Header */
.header{text-align:center;margin-bottom:48px}
.logo{display:inline-flex;align-items:center;gap:10px;margin-bottom:20px}
.logo svg{color:var(--primary)}
.logo-text{font-size:20px;font-weight:700;letter-spacing:-.02em;color:#f0efed}
h1{font-size:clamp(1.6rem,4vw,2.4rem);font-weight:700;letter-spacing:-.03em;
   color:#f0efed;margin-bottom:12px;line-height:1.15}
.subtitle{color:var(--muted);font-size:15px;max-width:52ch;margin:0 auto}

/* Install card */
.install-card{background:var(--surface);border:1px solid var(--border);
              border-radius:var(--radius-xl);padding:28px;margin-bottom:32px;
              box-shadow:var(--shadow)}
.install-label{font-size:11px;font-weight:600;letter-spacing:.08em;
               text-transform:uppercase;color:var(--muted);margin-bottom:14px}

.bookmarklet-btn{
  display:inline-flex;align-items:center;gap:10px;
  background:linear-gradient(135deg,var(--primary) 0%,var(--blue) 100%);
  color:#fff;font-weight:700;font-size:15px;
  padding:14px 24px;border-radius:var(--radius-md);
  cursor:grab;user-select:none;border:2px solid rgba(255,255,255,.15);
  box-shadow:0 4px 16px rgba(79,152,163,.3);
  transition:transform .15s,box-shadow .15s;
  text-decoration:none;margin-bottom:14px;
}
.bookmarklet-btn:hover{transform:translateY(-2px);box-shadow:0 8px 24px rgba(79,152,163,.4);
  text-decoration:none;color:#fff}
.bookmarklet-btn:active{cursor:grabbing;transform:translateY(0)}
.bookmarklet-btn svg{flex-shrink:0}

.hint{font-size:12px;color:var(--muted);display:flex;align-items:flex-start;gap:8px;
      background:var(--surface-2);border:1px solid var(--divider);
      border-radius:var(--radius-sm);padding:10px 12px}
.hint svg{flex-shrink:0;margin-top:1px;color:var(--orange)}

/* Steps */
.steps{display:flex;flex-direction:column;gap:16px;margin-bottom:32px}
.step{display:flex;gap:16px;background:var(--surface);border:1px solid var(--border);
      border-radius:var(--radius-lg);padding:20px}
.step-num{width:32px;height:32px;flex-shrink:0;border-radius:50%;
          background:var(--surface-3);border:1px solid var(--border);
          display:flex;align-items:center;justify-content:center;
          font-size:13px;font-weight:700;color:var(--primary)}
.step-body h3{font-size:14px;font-weight:600;color:#f0efed;margin-bottom:4px}
.step-body p{font-size:13px;color:var(--muted);line-height:1.5}

/* Code block */
.code-section{background:var(--surface);border:1px solid var(--border);
              border-radius:var(--radius-xl);overflow:hidden;margin-bottom:32px}
.code-header{display:flex;align-items:center;justify-content:space-between;
             padding:12px 16px;background:var(--surface-2);border-bottom:1px solid var(--border)}
.code-header span{font-size:12px;font-weight:600;color:var(--muted);letter-spacing:.05em}
.copy-btn{background:var(--surface-3);border:1px solid var(--border);color:var(--text);
          font-size:12px;font-weight:500;padding:5px 12px;border-radius:var(--radius-sm);
          cursor:pointer;transition:background .15s,color .15s}
.copy-btn:hover{background:var(--primary);color:#fff;border-color:var(--primary)}
.code-body{padding:16px;overflow-x:auto;font-family:var(--font-mono);
           font-size:12px;line-height:1.7;color:#aac8b0;max-height:220px}

/* Features */
.features{display:grid;grid-template-columns:repeat(auto-fill,minmax(200px,1fr));gap:14px;
          margin-bottom:32px}
.feat{background:var(--surface);border:1px solid var(--border);
      border-radius:var(--radius-lg);padding:18px}
.feat-icon{width:36px;height:36px;border-radius:var(--radius-sm);
           background:var(--surface-3);display:flex;align-items:center;
           justify-content:center;margin-bottom:12px;font-size:18px}
.feat h4{font-size:13px;font-weight:600;color:#f0efed;margin-bottom:4px}
.feat p{font-size:12px;color:var(--muted);line-height:1.5}

/* Badge */
.badge{display:inline-flex;align-items:center;gap:5px;background:var(--surface-2);
       border:1px solid var(--border);border-radius:var(--radius-full);
       font-size:11px;font-weight:500;color:var(--muted);padding:3px 9px}
.badge .dot{width:6px;height:6px;border-radius:50%;background:var(--green)}

footer{text-align:center;color:var(--faint);font-size:12px;margin-top:48px;
       padding-top:24px;border-top:1px solid var(--divider)}
</style>
</head>
<body>
<div class="page">

  <!-- Header -->
  <div class="header">
    <div class="logo">
      <svg width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <polygon points="23 7 16 12 23 17 23 7"/><rect x="1" y="5" width="15" height="14" rx="2" ry="2"/>
      </svg>
      <span class="logo-text">MW Metadata Exporter</span>
    </div>
    <h1>Export YouTube Metadata<br>in One Click</h1>
    <p class="subtitle">A bookmarklet for <a href="https://mattw.io/youtube-metadata/" target="_blank" rel="noopener">mattw.io/youtube-metadata</a> — saves all video metadata as JSON, CSV, or Markdown.</p>
  </div>

  <!-- Install -->
  <div class="install-card">
    <div class="install-label">Installation — drag to your bookmarks bar</div>
    <a class="bookmarklet-btn" href="javascript:(function()%7B(function () %7B function getText(sel, root) %7B const el = (root %7C%7C document).querySelector(sel); return el ? el.textContent.trim() : %27%27; %7D function getAttr(sel, attr, root) %7B const el = (root %7C%7C document).querySelector(sel); return el ? (el.getAttribute(attr) %7C%7C %27%27).trim() : %27%27; %7D function getRawJsonBlocks() %7B const blocks = %5B%5D; document.querySelectorAll(%27pre code, pre, code%27).forEach(el => %7B const txt = el.textContent.trim(); if (txt.startsWith(%27%7B%27) %7C%7C txt.startsWith(%27%5B%27)) %7B try %7B blocks.push(JSON.parse(txt)); %7D catch (_) %7B%7D %7D %7D); return blocks; %7D function extractVideoData() %7B const data = %7B%7D; const titleEl = document.querySelector( %27h1, h2, %5Bclass*=&quot;title&quot;%5D, %5Bid*=&quot;title&quot;%5D, .mat-card-title, .video-title%27 ); if (titleEl) data.title = titleEl.textContent.trim(); document.querySelectorAll(%27dt, .label, %5Bclass*=&quot;label&quot;%5D, th%27).forEach(el => %7B const key = el.textContent.trim().replace(/:$/, %27%27); const val = el.nextElementSibling ? el.nextElementSibling.textContent.trim() : %27%27; if (key &amp;&amp; val) data%5Bkey%5D = val; %7D); const jsonBlocks = getRawJsonBlocks(); jsonBlocks.forEach((block, i) => %7B data%5B%60_jsonBlock_$%7Bi + 1%7D%60%5D = block; %7D); document.querySelectorAll(%27table tr%27).forEach(tr => %7B const cells = tr.querySelectorAll(%27td, th%27); if (cells.length >= 2) %7B const key = cells%5B0%5D.textContent.trim().replace(/:$/, %27%27); const val = cells%5B1%5D.textContent.trim(); if (key &amp;&amp; val) data%5Bkey%5D = val; %7D %7D); document.querySelectorAll(%27%5Bclass*=&quot;card&quot;%5D, %5Bclass*=&quot;Card&quot;%5D, mat-card%27).forEach(card => %7B card.querySelectorAll(%27p, span, div%27).forEach(el => %7B const txt = el.textContent.trim(); const colonIdx = txt.indexOf(%27:%27); if (colonIdx > 0 &amp;&amp; colonIdx < 60 &amp;&amp; txt.length < 300) %7B const key = txt.slice(0, colonIdx).trim(); const val = txt.slice(colonIdx + 1).trim(); if (key &amp;&amp; val &amp;&amp; !data%5Bkey%5D) data%5Bkey%5D = val; %7D %7D); %7D); const inputEl = document.querySelector(%27input%5Btype=&quot;text&quot;%5D, input%5Baria-label%5D%27); if (inputEl &amp;&amp; inputEl.value) data%5B%27_inputUrl%27%5D = inputEl.value; data%5B%27_pageUrl%27%5D = window.location.href; data%5B%27_exportedAt%27%5D = new Date().toISOString(); return data; %7D function flattenForTable(obj, prefix) %7B const rows = %5B%5D; prefix = prefix %7C%7C %27%27; Object.entries(obj).forEach((%5Bk, v%5D) => %7B const key = prefix ? %60$%7Bprefix%7D.$%7Bk%7D%60 : k; if (v &amp;&amp; typeof v === %27object%27 &amp;&amp; !Array.isArray(v)) %7B rows.push(...flattenForTable(v, key)); %7D else if (Array.isArray(v)) %7B rows.push(%5Bkey, JSON.stringify(v)%5D); %7D else %7B rows.push(%5Bkey, String(v ?? %27%27)%5D); %7D %7D); return rows; %7D function toCSV(rows) %7B return %27Key,Value%5Cn%27 + rows.map((%5Bk, v%5D) => %7B const safe = v.replace(/&quot;/g, %27&quot;&quot;%27); return %60&quot;$%7Bk%7D&quot;,&quot;$%7Bsafe%7D&quot;%60; %7D).join(%27%5Cn%27); %7D function toMarkdown(rows) %7B const lines = %5B %27%23 YouTube Metadata Export%27, %60> Exported from: $%7Bwindow.location.href%7D%60, %60> Date: $%7Bnew Date().toLocaleString()%7D%60, %27%27, %27%7C Key %7C Value %7C%27, %27%7C-----%7C-------%7C%27 %5D; rows.forEach((%5Bk, v%5D) => %7B lines.push(%60%7C $%7Bk.replace(/%5C%7C/g, %27%5C%5C%7C%27)%7D %7C $%7Bv.replace(/%5C%7C/g, %27%5C%5C%7C%27).replace(/%5Cn/g, %27 %27)%7D %7C%60); %7D); return lines.join(%27%5Cn%27); %7D function downloadFile(content, filename, mime) %7B const blob = new Blob(%5Bcontent%5D, %7B type: mime %7D); const a = document.createElement(%27a%27); a.href = URL.createObjectURL(blob); a.download = filename; document.body.appendChild(a); a.click(); setTimeout(() => %7B URL.revokeObjectURL(a.href); a.remove(); %7D, 1000); %7D const OVERLAY_ID = %27__mw_exporter_overlay__%27; if (document.getElementById(OVERLAY_ID)) %7B document.getElementById(OVERLAY_ID).remove(); return; %7D const data = extractVideoData(); const rows = flattenForTable(data); const ts = new Date().toISOString().replace(/%5B:.%5D/g, %27-%27).slice(0, 19); const overlay = document.createElement(%27div%27); overlay.id = OVERLAY_ID; overlay.style.cssText = %5B %27position:fixed%27, %27top:16px%27, %27right:16px%27, %27z-index:2147483647%27, %27width:340px%27, %27background:%231c1b19%27, %27color:%23cdccca%27, %27border:1px solid %23393836%27, %27border-radius:12px%27, %27box-shadow:0 12px 40px rgba(0,0,0,.6)%27, %27font:14px/1.5 &quot;Inter&quot;,&quot;Segoe UI&quot;,sans-serif%27, %27padding:0%27, %27overflow:hidden%27 %5D.join(%27;%27); overlay.innerHTML = %60 <div style=&quot;display:flex;align-items:center;justify-content:space-between; padding:12px 16px;background:%2322211f;border-bottom:1px solid %23393836&quot;> <span style=&quot;font-weight:600;font-size:13px;letter-spacing:.02em;color:%23f0efed&quot;> 📦 MW Metadata Exporter </span> <button id=&quot;__mw_close__&quot; style=&quot;background:none;border:none;color:%23797876; font-size:18px;cursor:pointer;line-height:1;padding:0 2px&quot; title=&quot;Close&quot;>×</button> </div> <div style=&quot;padding:14px 16px&quot;> <div style=&quot;font-size:12px;color:%23797876;margin-bottom:10px&quot;> Fields found: <strong style=&quot;color:%234f98a3&quot;>$%7Brows.length%7D</strong> &amp;nbsp;%7C&amp;nbsp; JSON blocks: <strong style=&quot;color:%234f98a3&quot;>$%7BgetRawJsonBlocks().length%7D</strong> </div> <div style=&quot;max-height:200px;overflow-y:auto;background:%23171614; border:1px solid %23262523;border-radius:8px; padding:8px 10px;font-size:11px;line-height:1.6; font-family:monospace;margin-bottom:14px&quot;> $%7Brows.slice(0,40).map((%5Bk,v%5D)=>%60<div><span style=&quot;color:%234f98a3&quot;>$%7B escHtml(k)%7D</span>: <span style=&quot;color:%23cdccca&quot;>$%7BescHtml(v.slice(0,120))%7D$%7B v.length>120?%27…%27:%27%27%7D</span></div>%60).join(%27%27)%7D $%7Brows.length>40?%60<div style=&quot;color:%23797876&quot;>…and $%7Brows.length-40%7D more fields</div>%60:%27%27%7D </div> <div style=&quot;display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px&quot;> <button id=&quot;__mw_json__&quot; style=&quot;$%7BbtnStyle(%27%2301696f%27,%27%230c4e54%27)%7D&quot;>⬇ JSON</button> <button id=&quot;__mw_csv__&quot; style=&quot;$%7BbtnStyle(%27%23437a22%27,%27%232e5c10%27)%7D&quot;>⬇ CSV</button> <button id=&quot;__mw_md__&quot; style=&quot;$%7BbtnStyle(%27%23006494%27,%27%230b5177%27)%7D&quot;>⬇ MD</button> </div> <div id=&quot;__mw_status__&quot; style=&quot;margin-top:10px;font-size:11px; color:%236daa45;text-align:center;min-height:16px&quot;></div> </div> %60; function escHtml(s) %7B return String(s).replace(/&amp;/g,%27&amp;amp;%27).replace(/</g,%27&amp;lt;%27).replace(/>/g,%27&amp;gt;%27); %7D function btnStyle(bg, hover) %7B return %60background:$%7Bbg%7D;color:%23fff;border:none;border-radius:6px; padding:8px 4px;font-size:12px;font-weight:600;cursor:pointer; transition:background .15s;text-align:center%60; %7D document.body.appendChild(overlay); document.getElementById(%27__mw_close__%27).addEventListener(%27click%27, () => overlay.remove()); function setStatus(msg) %7B document.getElementById(%27__mw_status__%27).textContent = msg; setTimeout(() => %7B const s = document.getElementById(%27__mw_status__%27); if (s) s.textContent = %27%27; %7D, 2500); %7D document.getElementById(%27__mw_json__%27).addEventListener(%27click%27, () => %7B downloadFile(JSON.stringify(data, null, 2), %60yt-metadata-$%7Bts%7D.json%60, %27application/json%27); setStatus(%27✓ JSON saved%27); %7D); document.getElementById(%27__mw_csv__%27).addEventListener(%27click%27, () => %7B downloadFile(toCSV(rows), %60yt-metadata-$%7Bts%7D.csv%60, %27text/csv;charset=utf-8%27); setStatus(%27✓ CSV saved%27); %7D); document.getElementById(%27__mw_md__%27).addEventListener(%27click%27, () => %7B downloadFile(toMarkdown(rows), %60yt-metadata-$%7Bts%7D.md%60, %27text/markdown;charset=utf-8%27); setStatus(%27✓ Markdown saved%27); %7D); %5B%27__mw_json__%27,%27__mw_csv__%27,%27__mw_md__%27%5D.forEach(id => %7B const btn = document.getElementById(id); if (!btn) return; btn.addEventListener(%27mouseenter%27, () => btn.style.filter = %27brightness(1.15)%27); btn.addEventListener(%27mouseleave%27, () => btn.style.filter = %27%27); %7D); %7D)();%7D)();" onclick="return false;" title="Drag this button to your browser bookmarks bar">
      <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round">
        <path d="M19 21l-7-5-7 5V5a2 2 0 0 1 2-2h10a2 2 0 0 1 2 2z"/>
      </svg>
      ⬇ MW Metadata Exporter
    </a>
    <div class="hint">
      <svg width="15" height="15" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
        <circle cx="12" cy="12" r="10"/><line x1="12" y1="8" x2="12" y2="12"/><line x1="12" y1="16" x2="12.01" y2="16"/>
      </svg>
      <span>Don't click — <strong>drag</strong> the button to your Bookmarks Bar. If the bar is hidden, enable it with <kbd>Ctrl+Shift+B</kbd>.</span>
    </div>
  </div>

  <!-- Steps -->
  <div class="steps">
    <div class="step">
      <div class="step-num">1</div>
      <div class="step-body">
        <h3>Open mattw.io/youtube-metadata</h3>
        <p>Paste a video, channel, or playlist link and click Submit. Wait for the data to load.</p>
      </div>
    </div>
    <div class="step">
      <div class="step-num">2</div>
      <div class="step-body">
        <h3>Click the bookmarklet</h3>
        <p>An export panel will appear in the top-right corner showing the number of fields and JSON blocks found.</p>
      </div>
    </div>
    <div class="step">
      <div class="step-num">3</div>
      <div class="step-body">
        <h3>Choose a format</h3>
        <p>Click <strong>JSON</strong>, <strong>CSV</strong>, or <strong>MD</strong> — the file downloads automatically with a timestamp in the filename. Clicking the bookmarklet again closes the panel.</p>
      </div>
    </div>
  </div>

  <!-- Features -->
  <div class="features">
    <div class="feat">
      <div class="feat-icon">📦</div>
      <h4>JSON</h4>
      <p>Full nested object with all fields and raw API JSON blocks included.</p>
    </div>
    <div class="feat">
      <div class="feat-icon">📊</div>
      <h4>CSV</h4>
      <p>Flat Key/Value table compatible with Excel, Google Sheets, and pandas.</p>
    </div>
    <div class="feat">
      <div class="feat-icon">📝</div>
      <h4>Markdown</h4>
      <p>Readable table with heading and export date — great for Obsidian or Notion.</p>
    </div>
    <div class="feat">
      <div class="feat-icon">🔁</div>
      <h4>Toggle</h4>
      <p>Click to open the panel. Click again to dismiss — no page reload required.</p>
    </div>
  </div>

  <!-- Raw code -->
  <div class="code-section">
    <div class="code-header">
      <span>SOURCE CODE (for manual installation)</span>
      <button class="copy-btn" id="copyBtn">Copy</button>
    </div>
    <div class="code-body" id="codeBody">javascript:(function(){(function () { function getText(sel, root) { const el = (root || document).querySelector(sel); return el ? el.textContent.trim() : ''; } function getAttr(sel, attr, root) { const el = (root || document).querySelector(sel); return el ? (el.getAttribute(attr) || '').trim() : ''; } function getRawJsonBlocks() { const blocks = []; document.querySelectorAll('pre code, pre, code').forEach(el =&gt; { const txt = el.textContent.trim(); if (txt.startsWith('{') || txt.startsWith('[')) { try { blocks.push(JSON.parse(txt)); } catch (_) {} } }); return blocks; } function extractVideoData() { const data = {}; const titleEl = document.querySelector( 'h1, h2, [class*="title"], [id*="title"], .mat-card-title, .video-title' ); if (titleEl) data.title = titleEl.textContent.trim(); document.querySelectorAll('dt, .label, [class*="label"], th').forEach(el =&gt; { const key = el.textContent.trim().replace(/:$/, ''); const val = el.nextElementSibling ? el.nextElementSibling.textContent.trim() : ''; if (key &amp;&amp; val) data[key] = val; }); const jsonBlocks = getRawJsonBlocks(); jsonBlocks.forEach((block, i) =&gt; { data[`_jsonBlock_${i + 1}`] = block; }); document.querySelectorAll('table tr').forEach(tr =&gt; { const cells = tr.querySelectorAll('td, th'); if (cells.length &gt;= 2) { const key = cells[0].textContent.trim().replace(/:$/, ''); const val = cells[1].textContent.trim(); if (key &amp;&amp; val) data[key] = val; } }); document.querySelectorAll('[class*="card"], [class*="Card"], mat-card').forEach(card =&gt; { card.querySelectorAll('p, span, div').forEach(el =&gt; { const txt = el.textContent.trim(); const colonIdx = txt.indexOf(':'); if (colonIdx &gt; 0 &amp;&amp; colonIdx &lt; 60 &amp;&amp; txt.length &lt; 300) { const key = txt.slice(0, colonIdx).trim(); const val = txt.slice(colonIdx + 1).trim(); if (key &amp;&amp; val &amp;&amp; !data[key]) data[key] = val; } }); }); const inputEl = document.querySelector('input[type="text"], input[aria-label]'); if (inputEl &amp;&amp; inputEl.value) data['_inputUrl'] = inputEl.value; data['_pageUrl'] = window.location.href; data['_exportedAt'] = new Date().toISOString(); return data; } function flattenForTable(obj, prefix) { const rows = []; prefix = prefix || ''; Object.entries(obj).forEach(([k, v]) =&gt; { const key = prefix ? `${prefix}.${k}` : k; if (v &amp;&amp; typeof v === 'object' &amp;&amp; !Array.isArray(v)) { rows.push(...flattenForTable(v, key)); } else if (Array.isArray(v)) { rows.push([key, JSON.stringify(v)]); } else { rows.push([key, String(v ?? '')]); } }); return rows; } function toCSV(rows) { return 'Key,Value
' + rows.map(([k, v]) =&gt; { const safe = v.replace(/"/g, '""'); return `"${k}","${safe}"`; }).join('
'); } function toMarkdown(rows) { const lines = [ '# YouTube Metadata Export', `&gt; Exported from: ${window.location.href}`, `&gt; Date: ${new Date().toLocaleString()}`, '', '| Key | Value |', '|-----|-------|' ]; rows.forEach(([k, v]) =&gt; { lines.push(`| ${k.replace(/\|/g, '\|')} | ${v.replace(/\|/g, '\|').replace(/
/g, ' ')} |`); }); return lines.join('
'); } function downloadFile(content, filename, mime) { const blob = new Blob([content], { type: mime }); const a = document.createElement('a'); a.href = URL.createObjectURL(blob); a.download = filename; document.body.appendChild(a); a.click(); setTimeout(() =&gt; { URL.revokeObjectURL(a.href); a.remove(); }, 1000); } const OVERLAY_ID = '__mw_exporter_overlay__'; if (document.getElementById(OVERLAY_ID)) { document.getElementById(OVERLAY_ID).remove(); return; } const data = extractVideoData(); const rows = flattenForTable(data); const ts = new Date().toISOString().replace(/[:.]/g, '-').slice(0, 19); const overlay = document.createElement('div'); overlay.id = OVERLAY_ID; overlay.style.cssText = [ 'position:fixed', 'top:16px', 'right:16px', 'z-index:2147483647', 'width:340px', 'background:#1c1b19', 'color:#cdccca', 'border:1px solid #393836', 'border-radius:12px', 'box-shadow:0 12px 40px rgba(0,0,0,.6)', 'font:14px/1.5 "Inter","Segoe UI",sans-serif', 'padding:0', 'overflow:hidden' ].join(';'); overlay.innerHTML = ` &lt;div style="display:flex;align-items:center;justify-content:space-between; padding:12px 16px;background:#22211f;border-bottom:1px solid #393836"&gt; &lt;span style="font-weight:600;font-size:13px;letter-spacing:.02em;color:#f0efed"&gt; 📦 MW Metadata Exporter &lt;/span&gt; &lt;button id="__mw_close__" style="background:none;border:none;color:#797876; font-size:18px;cursor:pointer;line-height:1;padding:0 2px" title="Close"&gt;×&lt;/button&gt; &lt;/div&gt; &lt;div style="padding:14px 16px"&gt; &lt;div style="font-size:12px;color:#797876;margin-bottom:10px"&gt; Fields found: &lt;strong style="color:#4f98a3"&gt;${rows.length}&lt;/strong&gt; &amp;nbsp;|&amp;nbsp; JSON blocks: &lt;strong style="color:#4f98a3"&gt;${getRawJsonBlocks().length}&lt;/strong&gt; &lt;/div&gt; &lt;div style="max-height:200px;overflow-y:auto;background:#171614; border:1px solid #262523;border-radius:8px; padding:8px 10px;font-size:11px;line-height:1.6; font-family:monospace;margin-bottom:14px"&gt; ${rows.slice(0,40).map(([k,v])=&gt;`&lt;div&gt;&lt;span style="color:#4f98a3"&gt;${ escHtml(k)}&lt;/span&gt;: &lt;span style="color:#cdccca"&gt;${escHtml(v.slice(0,120))}${ v.length&gt;120?'…':''}&lt;/span&gt;&lt;/div&gt;`).join('')} ${rows.length&gt;40?`&lt;div style="color:#797876"&gt;…and ${rows.length-40} more fields&lt;/div&gt;`:''} &lt;/div&gt; &lt;div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px"&gt; &lt;button id="__mw_json__" style="${btnStyle('#01696f','#0c4e54')}"&gt;⬇ JSON&lt;/button&gt; &lt;button id="__mw_csv__" style="${btnStyle('#437a22','#2e5c10')}"&gt;⬇ CSV&lt;/button&gt; &lt;button id="__mw_md__" style="${btnStyle('#006494','#0b5177')}"&gt;⬇ MD&lt;/button&gt; &lt;/div&gt; &lt;div id="__mw_status__" style="margin-top:10px;font-size:11px; color:#6daa45;text-align:center;min-height:16px"&gt;&lt;/div&gt; &lt;/div&gt; `; function escHtml(s) { return String(s).replace(/&amp;/g,'&amp;amp;').replace(/&lt;/g,'&amp;lt;').replace(/&gt;/g,'&amp;gt;'); } function btnStyle(bg, hover) { return `background:${bg};color:#fff;border:none;border-radius:6px; padding:8px 4px;font-size:12px;font-weight:600;cursor:pointer; transition:background .15s;text-align:center`; } document.body.appendChild(overlay); document.getElementById('__mw_close__').addEventListener('click', () =&gt; overlay.remove()); function setStatus(msg) { document.getElementById('__mw_status__').textContent = msg; setTimeout(() =&gt; { const s = document.getElementById('__mw_status__'); if (s) s.textContent = ''; }, 2500); } document.getElementById('__mw_json__').addEventListener('click', () =&gt; { downloadFile(JSON.stringify(data, null, 2), `yt-metadata-${ts}.json`, 'application/json'); setStatus('✓ JSON saved'); }); document.getElementById('__mw_csv__').addEventListener('click', () =&gt; { downloadFile(toCSV(rows), `yt-metadata-${ts}.csv`, 'text/csv;charset=utf-8'); setStatus('✓ CSV saved'); }); document.getElementById('__mw_md__').addEventListener('click', () =&gt; { downloadFile(toMarkdown(rows), `yt-metadata-${ts}.md`, 'text/markdown;charset=utf-8'); setStatus('✓ Markdown saved'); }); ['__mw_json__','__mw_csv__','__mw_md__'].forEach(id =&gt; { const btn = document.getElementById(id); if (!btn) return; btn.addEventListener('mouseenter', () =&gt; btn.style.filter = 'brightness(1.15)'); btn.addEventListener('mouseleave', () =&gt; btn.style.filter = ''); }); })();})();</div>
  </div>

  <div style="display:flex;align-items:center;gap:10px;margin-bottom:24px">
    <span class="badge"><span class="dot"></span>Browser-only, no server</span>
    <span class="badge"><span class="dot"></span>No tracking</span>
    <span class="badge"><span class="dot"></span>~4 KB</span>
    <span class="badge"><span class="dot" style="background:var(--orange)"></span>CC BY 4.0</span>
  </div>

  <footer>Bookmarklet for <a href="https://mattw.io/youtube-metadata/" target="_blank" rel="noopener">MW Metadata</a> by Matt W &nbsp;·&nbsp; <a href="https://creativecommons.org/licenses/by/4.0/" target="_blank" rel="noopener">CC BY 4.0</a> | Pavel Bannikov for <a href="https://provereno.media" target="_blank" rel="noopener">Provereno.Media</a></footer>
</div>

<script>
const RAW_CODE = document.getElementById('codeBody').textContent;
document.getElementById('copyBtn').addEventListener('click', function() {
  navigator.clipboard.writeText(RAW_CODE).then(() => {
    this.textContent = '✓ Copied!';
    this.style.background = 'var(--green)';
    this.style.color = '#fff';
    setTimeout(() => {
      this.textContent = 'Copy';
      this.style.background = '';
      this.style.color = '';
    }, 2000);
  });
});
</script>
</body>
</html>
