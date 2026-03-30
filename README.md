<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>NBSP Cleaner</title>
  <style>
    :root {
      --panel: #111827;
      --panel-2: #1f2937;
      --text: #e5e7eb;
      --muted: #94a3b8;
      --accent: #22c55e;
      --border: #334155;
      --danger: #ef4444;
      --warning: #f59e0b;
      --highlight: rgba(245, 158, 11, 0.24);
    }

    * { box-sizing: border-box; }

    html, body {
      width: 100%;
      min-height: 100%;
    }

    body {
      margin: 0;
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: linear-gradient(180deg, #0b1220 0%, #111827 100%);
      color: var(--text);
      min-height: 100vh;
    }

    .wrap {
      width: 100%;
      max-width: none;
      margin: 0;
      padding: 10px 14px;
    }

    .panel {
      width: 100%;
      background: rgba(17, 24, 39, 0.96);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 14px;
      box-shadow: 0 12px 40px rgba(0, 0, 0, 0.25);
    }

    .topbar {
      display: flex;
      justify-content: space-between;
      align-items: center;
      gap: 10px;
      flex-wrap: wrap;
      margin-bottom: 12px;
    }

    .topbar .desc {
      color: var(--muted);
      font-size: 14px;
      line-height: 1.5;
      max-width: 900px;
    }

    .controls {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
      gap: 10px;
      margin-bottom: 14px;
    }

    .control {
      background: var(--panel-2);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 12px;
    }

    label {
      display: flex;
      align-items: flex-start;
      gap: 10px;
      cursor: pointer;
      line-height: 1.45;
      color: var(--text);
      font-size: 14px;
    }

    input[type="checkbox"] {
      margin-top: 2px;
      transform: scale(1.05);
      accent-color: var(--accent);
    }

    .grid {
      display: grid;
      grid-template-columns: minmax(0, 1fr) minmax(0, 1.15fr) minmax(0, 1fr);
      gap: 12px;
      width: 100%;
      align-items: start;
    }

    @media (max-width: 1280px) {
      .grid { grid-template-columns: 1fr; }
      .wrap { padding: 10px; }
    }

    .editor {
      display: flex;
      flex-direction: column;
      gap: 10px;
      min-width: 0;
    }

    .editor-head {
      display: flex;
      justify-content: space-between;
      align-items: flex-start;
      gap: 10px;
      flex-wrap: wrap;
    }

    .editor-head h2 {
      margin: 0;
      font-size: 16px;
      line-height: 1.3;
    }

    .editor-head small {
      color: var(--muted);
      display: block;
      margin-top: 4px;
      line-height: 1.45;
    }

    .editor-box {
      position: relative;
      border-radius: 12px;
      overflow: hidden;
      border: 1px solid var(--border);
      background: #020617;
    }

    textarea, .preview {
      width: 100%;
      min-height: 520px;
      resize: vertical;
      border: 0;
      outline: 0;
      background: #020617;
      color: #e2e8f0;
      padding: 14px;
      font: 13px/1.55 ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
      white-space: pre-wrap;
      word-break: break-word;
      tab-size: 2;
    }

    textarea[readonly] {
      opacity: 0.98;
    }

    .preview {
      overflow: auto;
    }

    .preview mark {
      background: var(--highlight);
      color: #fde68a;
      padding: 0 1px;
      border-radius: 3px;
      box-shadow: inset 0 0 0 1px rgba(245, 158, 11, 0.2);
    }

    .split-preview {
      display: grid;
      grid-template-rows: minmax(280px, auto) minmax(220px, auto);
      gap: 8px;
    }

    .subpanel {
      border-top: 1px solid var(--border);
      background: rgba(255,255,255,0.01);
    }

    .subpanel-title {
      padding: 10px 14px 0;
      color: var(--muted);
      font-size: 12px;
      text-transform: uppercase;
      letter-spacing: 0.06em;
    }

    .buttons {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
    }

    button {
      border: 0;
      border-radius: 10px;
      padding: 10px 14px;
      font-weight: 600;
      cursor: pointer;
      transition: transform 0.12s ease, opacity 0.12s ease;
    }

    button:hover { transform: translateY(-1px); }
    button:active { transform: translateY(0); }

    .primary { background: var(--accent); color: #052e16; }
    .secondary { background: #cbd5e1; color: #0f172a; }
    .ghost { background: transparent; color: var(--text); border: 1px solid var(--border); }
    .danger { background: var(--danger); color: white; }

    .stats {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
      gap: 10px;
      margin-top: 14px;
    }

    .stat {
      background: var(--panel-2);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 12px;
    }

    .stat .label {
      display: block;
      color: var(--muted);
      font-size: 12px;
      margin-bottom: 6px;
      text-transform: uppercase;
      letter-spacing: 0.06em;
    }

    .stat .value {
      font-size: 22px;
      font-weight: 700;
    }

    .badge {
      display: inline-flex;
      align-items: center;
      gap: 6px;
      padding: 6px 10px;
      border-radius: 999px;
      font-size: 12px;
      font-weight: 700;
      border: 1px solid var(--border);
      background: rgba(148, 163, 184, 0.08);
      color: var(--text);
    }

    .badge.warning {
      color: #fde68a;
      border-color: rgba(245, 158, 11, 0.35);
      background: rgba(245, 158, 11, 0.12);
    }

    code.inline {
      background: rgba(148, 163, 184, 0.15);
      padding: 2px 6px;
      border-radius: 6px;
      font-family: ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
    }
  </style>
</head>
<body>
  <div class="wrap">
    <div class="panel">
      <div class="topbar">
        <div class="desc">Paste plain text in column 1 or paste converted HTML directly into column 2. The tool detects both <code class="inline">&amp;nbsp;</code> entities and real non-breaking space characters, highlights them in column 2, and outputs cleaned HTML in column 3.</div>
        <span class="badge warning" id="detectedBadge">Detected NBSP: 0</span>
      </div>

      <div class="controls">
        <div class="control">
          <label>
            <input id="replaceNbsp" type="checkbox" checked />
            <span>Replace detected NBSP values with normal spaces in text nodes.</span>
          </label>
        </div>
        <div class="control">
          <label>
            <input id="collapseRuns" type="checkbox" checked />
            <span>Collapse repeated spaces after NBSP cleanup into a single space.</span>
          </label>
        </div>
        <div class="control">
          <label>
            <input id="trimText" type="checkbox" checked />
            <span>Trim leading and trailing whitespace inside text nodes where safe.</span>
          </label>
        </div>
        <div class="control">
          <label>
            <input id="removeEmptyInline" type="checkbox" checked />
            <span>Remove empty inline tags such as empty <code class="inline">span</code>, <code class="inline">b</code>, and <code class="inline">i</code>.</span>
          </label>
        </div>
      </div>

      <div class="grid">
        <div class="editor">
          <div class="editor-head">
            <div>
              <h2>Column 1: Plain Text</h2>
              <small>Optional. Paste raw text here, then convert it to basic HTML.</small>
            </div>
            <div class="buttons">
              <button class="ghost" id="convertBtn">Convert to HTML</button>
              <button class="ghost" id="loadSampleBtn">Load Sample</button>
              <button class="danger" id="clearBtn">Clear</button>
            </div>
          </div>
          <div class="editor-box">
            <textarea id="plainTextInput" placeholder="Paste plain text here..."></textarea>
          </div>
        </div>

        <div class="editor">
          <div class="editor-head">
            <div>
              <h2>Column 2: HTML to Inspect</h2>
              <small>Paste converted HTML here. Every detected <code class="inline">&amp;nbsp;</code> entity and real NBSP character is highlighted in the preview below.</small>
            </div>
            <div class="buttons">
              <button class="primary" id="processBtn">Clean HTML</button>
            </div>
          </div>
          <div class="editor-box split-preview">
            <textarea id="htmlInput" placeholder="Paste or generate HTML here..."></textarea>
            <div class="subpanel">
              <div class="subpanel-title">Highlighted preview</div>
              <div id="htmlPreview" class="preview" aria-label="HTML preview with highlighted NBSP"></div>
            </div>
          </div>
        </div>

        <div class="editor">
          <div class="editor-head">
            <div>
              <h2>Column 3: Clean HTML</h2>
              <small>Output with detected NBSP removed.</small>
            </div>
            <div class="buttons">
              <button class="secondary" id="copyBtn">Copy Clean HTML</button>
            </div>
          </div>
          <div class="editor-box">
            <textarea id="cleanHtml" placeholder="Clean HTML will appear here..." readonly></textarea>
          </div>
        </div>
      </div>

      <div class="stats">
        <div class="stat">
          <span class="label">Total NBSP detected</span>
          <span class="value" id="nbspBefore">0</span>
        </div>
        <div class="stat">
          <span class="label">Entity <code>&amp;nbsp;</code></span>
          <span class="value" id="entityCount">0</span>
        </div>
        <div class="stat">
          <span class="label">Unicode NBSP</span>
          <span class="value" id="charCount">0</span>
        </div>
        <div class="stat">
          <span class="label">NBSP after cleaning</span>
          <span class="value" id="nbspAfter">0</span>
        </div>
        <div class="stat">
          <span class="label">Removed</span>
          <span class="value" id="nbspRemoved">0</span>
        </div>
      </div>
    </div>
  </div>

  <script>
    const SKIP_TAGS = new Set(["PRE", "CODE", "SCRIPT", "STYLE", "TEXTAREA"]);
    const EMPTY_INLINE_SELECTORS = ["span", "b", "strong", "i", "em", "u", "small", "sup", "sub", "font"];

    const plainTextInput = document.getElementById("plainTextInput");
    const htmlInput = document.getElementById("htmlInput");
    const htmlPreview = document.getElementById("htmlPreview");
    const cleanHtmlOutput = document.getElementById("cleanHtml");
    const convertBtn = document.getElementById("convertBtn");
    const processBtn = document.getElementById("processBtn");
    const copyBtn = document.getElementById("copyBtn");
    const clearBtn = document.getElementById("clearBtn");
    const loadSampleBtn = document.getElementById("loadSampleBtn");
    const detectedBadge = document.getElementById("detectedBadge");

    const replaceNbsp = document.getElementById("replaceNbsp");
    const collapseRuns = document.getElementById("collapseRuns");
    const trimText = document.getElementById("trimText");
    const removeEmptyInline = document.getElementById("removeEmptyInline");

    const nbspBefore = document.getElementById("nbspBefore");
    const entityCountEl = document.getElementById("entityCount");
    const charCountEl = document.getElementById("charCount");
    const nbspAfter = document.getElementById("nbspAfter");
    const nbspRemoved = document.getElementById("nbspRemoved");

    function escapeHtml(str) {
      return String(str || "")
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/\"/g, "&quot;")
        .replace(/'/g, "&#39;");
    }

    function countNbspTypes(str) {
      const text = String(str || "");
      const entityCount = (text.match(/&nbsp;/g) || []).length;
      const charCount = (text.match(/\u00A0/g) || []).length;
      return {
        entityCount,
        charCount,
        total: entityCount + charCount,
      };
    }

    function plainTextToHtml(text) {
      const normalized = String(text || "").replace(/\r\n/g, "\n");
      const paragraphs = normalized
        .split(/\n{2,}/)
        .map(block => block.trim())
        .filter(Boolean);

      if (paragraphs.length === 0) return "";

      return paragraphs.map(paragraph => {
        const lines = paragraph.split("\n").map(line => line.trim()).filter(Boolean);
        const joined = lines.join(" ");
        return `<p>${escapeHtml(joined)}</p>`;
      }).join("\n");
    }

    function shouldSkipNode(node) {
      let current = node.parentNode;
      while (current && current.nodeType === Node.ELEMENT_NODE) {
        if (SKIP_TAGS.has(current.tagName)) return true;
        current = current.parentNode;
      }
      return false;
    }

    function cleanTextValue(value, options) {
      let result = value;

      if (options.replaceNbsp) {
        result = result.replace(/\u00A0/g, " ");
        result = result.replace(/&nbsp;/g, " ");
      }

      if (options.collapseRuns) {
        result = result.replace(/[ \t]{2,}/g, " ");
      }

      if (options.trimText) {
        result = result.replace(/^\s+/g, "").replace(/\s+$/g, "");
      }

      return result;
    }

    function removeEmptyInlineNodes(root) {
      let changed = true;
      while (changed) {
        changed = false;
        for (const selector of EMPTY_INLINE_SELECTORS) {
          root.querySelectorAll(selector).forEach((el) => {
            const hasMeaningfulText = el.textContent.replace(/[\s\u00A0]/g, "").length > 0;
            const hasMedia = el.querySelector("img, svg, video, audio, iframe, br") !== null;
            const hasAttrs = Array.from(el.attributes).some(attr => !["class", "style"].includes(attr.name));
            if (!hasMeaningfulText && !hasMedia && !hasAttrs) {
              el.remove();
              changed = true;
            }
          });
        }
      }
    }

    function cleanHtml(html, options) {
      const parser = new DOMParser();
      const doc = parser.parseFromString(`<div id="root">${String(html || "")}</div>`, "text/html");
      const root = doc.getElementById("root");
      const walker = doc.createTreeWalker(root, NodeFilter.SHOW_TEXT, null);
      const textNodes = [];

      while (walker.nextNode()) {
        textNodes.push(walker.currentNode);
      }

      for (const node of textNodes) {
        if (shouldSkipNode(node)) continue;
        node.nodeValue = cleanTextValue(node.nodeValue, options);
      }

      if (options.removeEmptyInline) {
        removeEmptyInlineNodes(root);
      }

      return root.innerHTML
        .replace(/>\s+</g, "><")
        .replace(/\n{3,}/g, "\n\n")
        .trim();
    }

    function buildHighlightedPreview(str) {
      const text = String(str || "");
      let out = "";
      for (let i = 0; i < text.length; i += 1) {
        if (text.startsWith("&nbsp;", i)) {
          out += "<mark>&amp;nbsp;</mark>";
          i += 5;
          continue;
        }
        if (text[i] === "\u00A0") {
          out += "<mark>\\u00A0</mark>";
          continue;
        }
        out += escapeHtml(text[i]);
      }
      return out;
    }

    function updatePreviewAndStats() {
      const html = htmlInput.value;
      const counts = countNbspTypes(html);
      htmlPreview.innerHTML = buildHighlightedPreview(html);
      detectedBadge.textContent = `Detected NBSP: ${counts.total}`;
      nbspBefore.textContent = counts.total;
      entityCountEl.textContent = counts.entityCount;
      charCountEl.textContent = counts.charCount;
    }

    function runClean() {
      const html = htmlInput.value;
      const cleaned = cleanHtml(html, {
        replaceNbsp: replaceNbsp.checked,
        collapseRuns: collapseRuns.checked,
        trimText: trimText.checked,
        removeEmptyInline: removeEmptyInline.checked,
      });
      cleanHtmlOutput.value = cleaned;
      const after = countNbspTypes(cleaned).total;
      nbspAfter.textContent = after;
      nbspRemoved.textContent = Math.max(countNbspTypes(html).total - after, 0);
    }

    convertBtn.addEventListener("click", () => {
      htmlInput.value = plainTextToHtml(plainTextInput.value);
      updatePreviewAndStats();
      cleanHtmlOutput.value = "";
      nbspAfter.textContent = "0";
      nbspRemoved.textContent = "0";
    });

    processBtn.addEventListener("click", () => {
      updatePreviewAndStats();
      runClean();
    });

    htmlInput.addEventListener("input", () => {
      updatePreviewAndStats();
      cleanHtmlOutput.value = "";
      nbspAfter.textContent = "0";
      nbspRemoved.textContent = "0";
    });

    copyBtn.addEventListener("click", async () => {
      try {
        await navigator.clipboard.writeText(cleanHtmlOutput.value);
        copyBtn.textContent = "Copied";
        setTimeout(() => {
          copyBtn.textContent = "Copy Clean HTML";
        }, 1400);
      } catch (err) {
        alert("Copy failed. Please copy the clean HTML manually.");
      }
    });

    clearBtn.addEventListener("click", () => {
      plainTextInput.value = "";
      htmlInput.value = "";
      htmlPreview.textContent = "";
      cleanHtmlOutput.value = "";
      detectedBadge.textContent = "Detected NBSP: 0";
      nbspBefore.textContent = "0";
      entityCountEl.textContent = "0";
      charCountEl.textContent = "0";
      nbspAfter.textContent = "0";
      nbspRemoved.textContent = "0";
    });

    loadSampleBtn.addEventListener("click", () => {
      plainTextInput.value = `Trampilla  Doble para Piso – Receso de 1/8” para Loseta Vinílica/Alfombra\n\nLa PA-BA-FT-8040-DL ofrece una solución de perfil bajo para accesos en pisos terminados con loseta vinílica o alfombra.`;
      htmlInput.value = `<p>Trampilla&nbsp;Doble para Piso&nbsp;– Receso de 1/8” para Loseta Vinílica/Alfombra</p>\n<p>La PA-BA-FT-8040-DL ofrece una solución de&nbsp;perfil bajo para accesos en pisos terminados con loseta vinílica o alfombra.</p>\n<p>Texto con NBSP real aquí: A\u00A0B</p>`;
      updatePreviewAndStats();
      cleanHtmlOutput.value = "";
      nbspAfter.textContent = "0";
      nbspRemoved.textContent = "0";
    });
  </script>
</body>
</html>
