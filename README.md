<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>HTML NBSP Cleaner</title>
  <style>
    :root {
      --bg: #0f172a;
      --panel: #111827;
      --panel-2: #1f2937;
      --text: #e5e7eb;
      --muted: #94a3b8;
      --accent: #22c55e;
      --accent-2: #38bdf8;
      --border: #334155;
      --danger: #ef4444;
      --warning: #f59e0b;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      font-family: Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
      background: linear-gradient(180deg, #0b1220 0%, #111827 100%);
      color: var(--text);
      min-height: 100vh;
    }

    .wrap {
      max-width: 1600px;
      margin: 0 auto;
      padding: 24px;
    }

    h1 {
      margin: 0 0 8px;
      font-size: 28px;
      line-height: 1.2;
    }

    .sub {
      color: var(--muted);
      margin-bottom: 20px;
      max-width: 1100px;
      line-height: 1.6;
    }

    .panel {
      background: rgba(17, 24, 39, 0.92);
      border: 1px solid var(--border);
      border-radius: 16px;
      padding: 16px;
      box-shadow: 0 12px 40px rgba(0, 0, 0, 0.25);
    }

    .controls {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
      gap: 12px;
      margin-bottom: 18px;
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
      transform: scale(1.1);
      accent-color: var(--accent);
    }

    .grid {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 16px;
    }

    @media (max-width: 1200px) {
      .grid { grid-template-columns: 1fr; }
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
      align-items: center;
      gap: 12px;
      flex-wrap: wrap;
    }

    .editor-head h2 {
      margin: 0;
      font-size: 16px;
    }

    .editor-head small {
      color: var(--muted);
      display: block;
      margin-top: 4px;
    }

    textarea {
      width: 100%;
      min-height: 460px;
      resize: vertical;
      border-radius: 12px;
      border: 1px solid var(--border);
      background: #020617;
      color: #e2e8f0;
      padding: 14px;
      font: 13px/1.5 ui-monospace, SFMono-Regular, Menlo, Consolas, monospace;
    }

    .buttons {
      display: flex;
      gap: 10px;
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
    .info { background: var(--accent-2); color: #082f49; }

    .stats {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
      gap: 12px;
      margin-top: 16px;
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
      padding: 5px 9px;
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

    .note {
      margin-top: 18px;
      font-size: 13px;
      color: var(--muted);
      line-height: 1.6;
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
    <h1>HTML NBSP Cleaner</h1>
    <div class="sub">
      Paste plain text in the first column. The second column generates an HTML version and highlights whether it contains unnecessary
      <code class="inline">&amp;nbsp;</code> patterns. The third column gives you cleaned HTML with unnecessary non-breaking spaces removed.
    </div>

    <div class="panel">
      <div class="controls">
        <div class="control">
          <label>
            <input id="replaceNbsp" type="checkbox" checked />
            <span>Replace NBSP characters and <code class="inline">&amp;nbsp;</code> entities with normal spaces in text nodes.</span>
          </label>
        </div>
        <div class="control">
          <label>
            <input id="collapseRuns" type="checkbox" checked />
            <span>Collapse repeated spaces created by conversion into a single normal space.</span>
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
              <small>Paste the raw content here.</small>
            </div>
            <div class="buttons">
              <button class="ghost" id="loadSampleBtn">Load Sample</button>
              <button class="danger" id="clearBtn">Clear</button>
            </div>
          </div>
          <textarea id="plainTextInput" placeholder="Paste plain text here..."></textarea>
        </div>

        <div class="editor">
          <div class="editor-head">
            <div>
              <h2>Column 2: Generated HTML</h2>
              <small>Shows the HTML version and detected NBSP count.</small>
            </div>
            <span class="badge warning" id="detectedBadge">Detected unnecessary NBSP: 0</span>
          </div>
          <textarea id="generatedHtml" placeholder="Generated HTML with detected NBSP patterns will appear here..."></textarea>
        </div>

        <div class="editor">
          <div class="editor-head">
            <div>
              <h2>Column 3: Clean HTML</h2>
              <small>Clean version with unnecessary NBSP removed.</small>
            </div>
            <div class="buttons">
              <button class="primary" id="processBtn">Process</button>
              <button class="secondary" id="copyBtn">Copy Clean HTML</button>
            </div>
          </div>
          <textarea id="cleanHtml" placeholder="Clean HTML will appear here..."></textarea>
        </div>
      </div>

      <div class="stats">
        <div class="stat">
          <span class="label">NBSP in generated HTML</span>
          <span class="value" id="nbspBefore">0</span>
        </div>
        <div class="stat">
          <span class="label">NBSP in clean HTML</span>
          <span class="value" id="nbspAfter">0</span>
        </div>
        <div class="stat">
          <span class="label">Removed</span>
          <span class="value" id="nbspRemoved">0</span>
        </div>
      </div>

      <div class="note">
        Workflow: plain text → generated HTML with NBSP issues → cleaned HTML. This makes it easier to compare what the messy HTML looks like before and after cleanup.
      </div>
    </div>
  </div>

  <script>
    const SKIP_TAGS = new Set(["PRE", "CODE", "SCRIPT", "STYLE", "TEXTAREA"]);
    const EMPTY_INLINE_SELECTORS = ["span", "b", "strong", "i", "em", "u", "small", "sup", "sub", "font"];

    const plainTextInput = document.getElementById("plainTextInput");
    const generatedHtml = document.getElementById("generatedHtml");
    const cleanHtmlOutput = document.getElementById("cleanHtml");
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
    const nbspAfter = document.getElementById("nbspAfter");
    const nbspRemoved = document.getElementById("nbspRemoved");

    function escapeHtml(str) {
      return str
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/\"/g, "&quot;")
        .replace(/'/g, "&#39;");
    }

    function countNbsp(str) {
      if (!str) return 0;
      const entityCount = (str.match(/&nbsp;/g) || []).length;
      const charCount = (str.match(/\u00A0/g) || []).length;
      return entityCount + charCount;
    }

    function plainTextToMessyHtml(text) {
      const normalized = text.replace(/\r\n/g, "\n");
      const paragraphs = normalized
        .split(/\n{2,}/)
        .map(block => block.trim())
        .filter(Boolean);

      if (paragraphs.length === 0) return "";

      return paragraphs.map(paragraph => {
        const lines = paragraph.split("\n").map(line => line.trim()).filter(Boolean);
        const joined = lines.join(" ");

        const withFakeNbsp = escapeHtml(joined)
          .replace(/ {2,}/g, match => "&nbsp;".repeat(match.length))
          .replace(/([^\s]) ([^\s])/g, "$1&nbsp;$2");

        return `<p>${withFakeNbsp}</p>`;
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
        result = result.replace(/\u00A0/g, " ").replace(/&nbsp;/g, " ");
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
      const doc = parser.parseFromString(`<div id="root">${html}</div>`, "text/html");
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
        .replace(/\n{3,}/g, "\n\n");
    }

    function updateStats(before, after) {
      const beforeCount = countNbsp(before);
      const afterCount = countNbsp(after);
      nbspBefore.textContent = beforeCount;
      nbspAfter.textContent = afterCount;
      nbspRemoved.textContent = Math.max(beforeCount - afterCount, 0);
      detectedBadge.textContent = `Detected unnecessary NBSP: ${beforeCount}`;
    }

    function runProcess() {
      const text = plainTextInput.value;
      const messy = plainTextToMessyHtml(text);
      const cleaned = cleanHtml(messy, {
        replaceNbsp: replaceNbsp.checked,
        collapseRuns: collapseRuns.checked,
        trimText: trimText.checked,
        removeEmptyInline: removeEmptyInline.checked,
      });

      generatedHtml.value = messy;
      cleanHtmlOutput.value = cleaned;
      updateStats(messy, cleaned);
    }

    processBtn.addEventListener("click", runProcess);

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
      generatedHtml.value = "";
      cleanHtmlOutput.value = "";
      updateStats("", "");
    });

    loadSampleBtn.addEventListener("click", () => {
      plainTextInput.value = `Trampilla  Doble para Piso – Receso de 1/8” para Loseta Vinílica/Alfombra\n\nLa PA-BA-FT-8040-DL ofrece una solución de perfil bajo para accesos en pisos terminados con loseta vinílica o alfombra.\n\nConsulta la ficha técnica para datos de carga, herrajes y recomendaciones de instalación.`;
      generatedHtml.value = "";
      cleanHtmlOutput.value = "";
      updateStats("", "");
    });
  </script>
</body>
</html>
