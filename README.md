<div align="center">

<img src="https://img.shields.io/badge/bookmarklet-browser%20only-4f98a3?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Browser only">
<img src="https://img.shields.io/badge/license-CC%20BY%204.0-6daa45?style=for-the-badge" alt="CC BY 4.0">
<img src="https://img.shields.io/badge/size-~4%20KB-797876?style=for-the-badge" alt="~4 KB">
<img src="https://img.shields.io/badge/tracking-none-437a22?style=for-the-badge" alt="No tracking">

<br/><br/>

# 📦 MW Metadata Exporter

**A one-click bookmarklet for [mattw.io/youtube-metadata](https://mattw.io/youtube-metadata/)**  
Export any YouTube video, channel, or playlist metadata to **JSON**, **CSV**, or **Markdown** — instantly, in the browser, with zero setup.

<br/>

</div>

---

## ✨ Features

- 🔍 **Scrapes all rendered fields** — title, stats, channel info, tags, thumbnails, and more
- 📦 **Raw API JSON blocks** preserved verbatim as `_jsonBlock_1`, `_jsonBlock_2`…
- 📊 **Three export formats** — JSON (nested), CSV (flat Key/Value), Markdown (table)
- 🔁 **Toggle overlay** — click to open, click again to close, no page reload
- 🚫 **No server, no tracking, no dependencies** — ~4 KB, runs entirely in your browser
- 🕓 **Timestamped filenames** — e.g. `yt-metadata-2026-06-05T18-00-00.json`

---

## 🚀 Installation

### Option A — Installer page *(recommended)*

> **Drag** the button from [`mw-metadata-exporter.html`](https://www.perplexity.ai/computer/a/e26e7f7d-7fc8-5e7b-ae25-daf882f99615) to your **Bookmarks Bar**.  

### Option B — Manual

1. Open your browser's bookmark manager
2. Create a new bookmark — name it anything, e.g. `MW Export`
3. Paste the full code below into the **URL / Address** field

<details>
<summary><b>▶ Show full bookmarklet code</b></summary>

```javascript
javascript:(function(){(function(){function getRawJsonBlocks(){const blocks=[];document.querySelectorAll('pre code,pre,code').forEach(el=>{const txt=el.textContent.trim();if(txt.startsWith('{')||txt.startsWith('[')){try{blocks.push(JSON.parse(txt));}catch(_){}}});return blocks;}function extractVideoData(){const data={};const titleEl=document.querySelector('h1,h2,[class*="title"],[id*="title"],.mat-card-title,.video-title');if(titleEl)data.title=titleEl.textContent.trim();document.querySelectorAll('dt,.label,[class*="label"],th').forEach(el=>{const key=el.textContent.trim().replace(/:$/,'');const val=el.nextElementSibling?el.nextElementSibling.textContent.trim():'';if(key&&val)data[key]=val;});getRawJsonBlocks().forEach((block,i)=>{data[`_jsonBlock_${i+1}`]=block;});document.querySelectorAll('table tr').forEach(tr=>{const cells=tr.querySelectorAll('td,th');if(cells.length>=2){const key=cells.textContent.trim().replace(/:$/,'');const val=cells[^1].textContent.trim();if(key&&val)data[key]=val;}});document.querySelectorAll('[class*="card"],[class*="Card"],mat-card').forEach(card=>{card.querySelectorAll('p,span,div').forEach(el=>{const txt=el.textContent.trim();const c=txt.indexOf(':');if(c>0&&c<60&&txt.length<300){const k=txt.slice(0,c).trim();const v=txt.slice(c+1).trim();if(k&&v&&!data[k])data[k]=v;}});});const inp=document.querySelector('input[type="text"],input[aria-label]');if(inp&&inp.value)data['_inputUrl']=inp.value;data['_pageUrl']=window.location.href;data['_exportedAt']=new Date().toISOString();return data;}function flattenForTable(obj,prefix){const rows=[];prefix=prefix||'';Object.entries(obj).forEach(([k,v])=>{const key=prefix?`${prefix}.${k}`:k;if(v&&typeof v==='object'&&!Array.isArray(v)){rows.push(...flattenForTable(v,key));}else if(Array.isArray(v)){rows.push([key,JSON.stringify(v)]);}else{rows.push([key,String(v??'')]);}});return rows;}function toCSV(rows){return'Key,Value\n'+rows.map(([k,v])=>{const s=v.replace(/"/g,'""');return`"${k}","${s}"`;}).join('\n');}function toMarkdown(rows){const lines=['# YouTube Metadata Export',`> Exported from: ${window.location.href}`,`> Date: ${new Date().toLocaleString()}`,'','| Key | Value |','|-----|-------|'];rows.forEach(([k,v])=>{lines.push(`| ${k.replace(/\|/g,'\\|')} | ${v.replace(/\|/g,'\\|').replace(/\n/g,' ')} |`);});return lines.join('\n');}function downloadFile(content,filename,mime){const blob=new Blob([content],{type:mime});const a=document.createElement('a');a.href=URL.createObjectURL(blob);a.download=filename;document.body.appendChild(a);a.click();setTimeout(()=>{URL.revokeObjectURL(a.href);a.remove();},1000);}const ID='__mw_exporter_overlay__';if(document.getElementById(ID)){document.getElementById(ID).remove();return;}const data=extractVideoData();const rows=flattenForTable(data);const ts=new Date().toISOString().replace(/[:.]/g,'-').slice(0,19);const ov=document.createElement('div');ov.id=ID;ov.style.cssText='position:fixed;top:16px;right:16px;z-index:2147483647;width:340px;background:#1c1b19;color:#cdccca;border:1px solid #393836;border-radius:12px;box-shadow:0 12px 40px rgba(0,0,0,.6);font:14px/1.5 Inter,sans-serif;padding:0;overflow:hidden';ov.innerHTML=`<div style="display:flex;align-items:center;justify-content:space-between;padding:12px 16px;background:#22211f;border-bottom:1px solid #393836"><span style="font-weight:600;font-size:13px;color:#f0efed">📦 MW Metadata Exporter</span><button id="__mw_x__" style="background:none;border:none;color:#797876;font-size:18px;cursor:pointer;line-height:1">×</button></div><div style="padding:14px 16px"><div style="font-size:12px;color:#797876;margin-bottom:10px">Fields found: <strong style="color:#4f98a3">${rows.length}</strong> &nbsp;|&nbsp; JSON blocks: <strong style="color:#4f98a3">${getRawJsonBlocks().length}</strong></div><div style="max-height:200px;overflow-y:auto;background:#171614;border:1px solid #262523;border-radius:8px;padding:8px 10px;font-size:11px;line-height:1.6;font-family:monospace;margin-bottom:14px">${rows.slice(0,40).map(([k,v])=>`<div><span style="color:#4f98a3">${String(k).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;')}</span>: <span style="color:#cdccca">${String(v).slice(0,120).replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;')}${v.length>120?'…':''}</span></div>`).join('')}${rows.length>40?`<div style="color:#797876">…and ${rows.length-40} more fields</div>`:''}</div><div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px"><button id="__mw_j__" style="background:#01696f;color:#fff;border:none;border-radius:6px;padding:8px 4px;font-size:12px;font-weight:600;cursor:pointer">⬇ JSON</button><button id="__mw_c__" style="background:#437a22;color:#fff;border:none;border-radius:6px;padding:8px 4px;font-size:12px;font-weight:600;cursor:pointer">⬇ CSV</button><button id="__mw_m__" style="background:#006494;color:#fff;border:none;border-radius:6px;padding:8px 4px;font-size:12px;font-weight:600;cursor:pointer">⬇ MD</button></div><div id="__mw_s__" style="margin-top:10px;font-size:11px;color:#6daa45;text-align:center;min-height:16px"></div></div>`;document.body.appendChild(ov);document.getElementById('__mw_x__').addEventListener('click',()=>ov.remove());function st(msg){document.getElementById('__mw_s__').textContent=msg;setTimeout(()=>{const s=document.getElementById('__mw_s__');if(s)s.textContent='';},2500);}document.getElementById('__mw_j__').addEventListener('click',()=>{downloadFile(JSON.stringify(data,null,2),`yt-metadata-${ts}.json`,'application/json');st('✓ JSON saved');});document.getElementById('__mw_c__').addEventListener('click',()=>{downloadFile(toCSV(rows),`yt-metadata-${ts}.csv`,'text/csv;charset=utf-8');st('✓ CSV saved');});document.getElementById('__mw_m__').addEventListener('click',()=>{downloadFile(toMarkdown(rows),`yt-metadata-${ts}.md`,'text/markdown;charset=utf-8');st('✓ Markdown saved');});['__mw_j__','__mw_c__','__mw_m__'].forEach(id=>{const b=document.getElementById(id);if(!b)return;b.addEventListener('mouseenter',()=>b.style.filter='brightness(1.15)');b.addEventListener('mouseleave',()=>b.style.filter='');});})();})();
```

</details>

---

## 🎬 How to use

```text
1. Open  →  https://mattw.io/youtube-metadata/
2. Paste →  a YouTube video / channel / playlist URL, click Submit
3. Wait  →  for the metadata to fully load on the page
4. Click →  the "MW Export" bookmarklet in your bar
5. Pick  →  JSON, CSV, or MD — file downloads instantly
6. Click →  the bookmarklet again to close the panel
```


---

## 📤 Export formats

| Format | Structure | Best for |
| :-- | :-- | :-- |
| **JSON** | Full nested object, raw API blocks preserved as `_jsonBlock_N` | Python scripts, `jq`, any JSON toolchain |
| **CSV** | Flat `Key,Value` table — nested objects dot-flattened | Excel, Google Sheets, pandas, LibreOffice |
| **Markdown** | GFM table with title header and export timestamp | Obsidian, Notion, GitHub wikis, reports |


---

## 🔬 How data is extracted

Four parallel strategies cover MW Metadata's Angular-rendered DOM:


| Strategy | Targets | What it captures |
| :-- | :-- | :-- |
| **JSON blocks** | `<pre><code>` | Raw API response objects — the most complete data source |
| **Label pairs** | `<dt>`, `<th>` + next sibling | Rendered key → value metadata rows |
| **Table rows** | `<table tr td>` | Standard HTML tables |
| **Card text** | `[class*="card"]` children | Angular Material cards with `Key: Value` text |

Three system fields are always appended:


| Field | Value |
| :-- | :-- |
| `_inputUrl` | The URL entered in the search field |
| `_pageUrl` | Current page URL |
| `_exportedAt` | ISO 8601 export timestamp |

---

## 🔒 Privacy \& security

- Runs **entirely in your browser** — no data leaves your machine
- Makes **zero network requests** — no APIs, no analytics, no telemetry
- Uses **in-memory state only** — no `localStorage`, no cookies
- Injects a self-contained overlay that removes itself on dismissal

---

## 📄 License

**[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)**

Built by **Pavel "Pogoda" Bannikov** for [Provereno.Media](https://provereno.media), 2026.
Bookmarklet for [MW Metadata](https://mattw.io/youtube-metadata/) by [Matt W](https://mattw.io).
