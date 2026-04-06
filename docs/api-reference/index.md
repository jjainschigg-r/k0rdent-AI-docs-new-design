---
hide_title: true
hide:
  - navigation
  - toc
  - feedback
---

<style>
  .md-main .md-grid { max-width: none !important; }
  .md-content, .md-content__inner { margin: 0 !important; padding: 0 !important; }
  .md-sidebar--primary, .md-sidebar--secondary { display: none !important; }
  .md-footer { display: none !important; }
  html, body { height: 100%; }

  /* Tab bar */
  #api-tabs {
    display: flex;
    background: var(--md-default-bg-color);
    border-bottom: 2px solid var(--md-default-fg-color--lightest);
    position: sticky;
    top: var(--md-header-height, 48px);
    z-index: 100;
    padding: 0 16px;
  }
  #api-tabs button {
    padding: 10px 20px;
    border: none;
    background: none;
    cursor: pointer;
    font-size: 14px;
    font-weight: 500;
    color: var(--md-default-fg-color--light);
    border-bottom: 3px solid transparent;
    margin-bottom: -2px;
    transition: color 0.15s, border-color 0.15s;
  }
  #api-tabs button:hover {
    color: var(--md-default-fg-color);
  }
  #api-tabs button.active {
    color: var(--md-accent-fg-color);
    border-bottom-color: var(--md-accent-fg-color);
  }

  /* Viewer panels */
  .api-panel {
    display: none;
    width: 100%;
    min-height: calc(100vh - var(--md-header-height, 48px) - 43px);
  }
  .api-panel.active { display: block; }

  /* Scalar */
  #scalar-panel #api-reference {
    width: 100%;
    min-height: calc(100vh - var(--md-header-height, 48px) - 43px);
  }

  /* ReDoc */
  #redoc-container {
    min-height: calc(100vh - var(--md-header-height, 48px) - 43px);
  }

  /* ReDoc dark mode — central content area has no theme property, patch with CSS */
  body[data-md-color-scheme="slate"] .redoc-wrap { background-color: #111315; }
  body[data-md-color-scheme="slate"] .api-content { background-color: #111315; color: #e0e0e0; }
  body[data-md-color-scheme="slate"] .api-content h2 { color: #e0e0e0 !important; }

  /* Swagger */
  #swagger-ui {
    min-height: calc(100vh - var(--md-header-height, 48px) - 43px);
  }
</style>

<div id="api-tabs">
  <button id="tab-scalar" onclick="apiSwitchTab('scalar')">Scalar</button>
  <button id="tab-redoc" onclick="apiSwitchTab('redoc')">ReDoc</button>
  <button id="tab-swagger" onclick="apiSwitchTab('swagger')">Swagger UI</button>
</div>

<div id="scalar-panel" class="api-panel">
  <div id="api-reference" data-url="../openapi/openapi.bundled.yaml"></div>
</div>

<div id="redoc-panel" class="api-panel">
  <span id="redoc"></span>
  <div id="redoc-container"></div>
</div>

<div id="swagger-panel" class="api-panel">
  <span id="swagger"></span>
  <div id="swagger-ui"></div>
</div>

<script src="https://cdn.jsdelivr.net/npm/@scalar/api-reference@1.49.4"></script>

<script>
(function () {
  var specUrl = '../openapi/openapi.bundled.yaml';
  var redocLoaded = false;
  var swaggerLoaded = false;
  var redocContainer = document.getElementById('redoc-container');

  function isDark() {
    return document.body.getAttribute('data-md-color-scheme') === 'slate';
  }

  // ---- Scalar theme (body.dark-mode class is the only reliable mechanism) ----
  function syncScalarTheme() {
    document.body.classList.toggle('dark-mode', isDark());
  }
  syncScalarTheme();
  setTimeout(syncScalarTheme, 50);

  // ---- ReDoc themes ----
  var redocLightTheme = { colors: { primary: { main: '#9a6b00' } } };
  var redocDarkTheme = {
    colors: { primary: { main: '#F8E201' } },
    sidebar: { backgroundColor: '#1a1d20', textColor: '#e0e0e0', activeTextColor: '#F8E201' },
    rightPanel: { backgroundColor: '#0d1117', textColor: '#ffffff' }
  };

  function freshRedocContainer() {
    var el = document.createElement('div');
    redocContainer.parentNode.replaceChild(el, redocContainer);
    redocContainer = el;
    return redocContainer;
  }

  function initRedoc() {
    if (!window.Redoc) return;
    Redoc.init(specUrl, { theme: isDark() ? redocDarkTheme : redocLightTheme }, freshRedocContainer());
  }

  function loadRedoc() {
    if (redocLoaded) return;
    redocLoaded = true;
    var s = document.createElement('script');
    s.src = 'https://cdn.redoc.ly/redoc/v2.5.2/bundles/redoc.standalone.js';
    s.onload = initRedoc;
    document.head.appendChild(s);
  }

  // ---- Swagger theme (html.dark-mode class) ----
  function syncSwaggerTheme() {
    document.documentElement.classList.toggle('dark-mode', isDark());
  }

  function loadSwagger() {
    if (swaggerLoaded) return;
    swaggerLoaded = true;
    var link = document.createElement('link');
    link.rel = 'stylesheet';
    link.href = 'https://unpkg.com/swagger-ui-dist@5/swagger-ui.css';
    document.head.appendChild(link);
    var s = document.createElement('script');
    s.src = 'https://unpkg.com/swagger-ui-dist@5/swagger-ui-bundle.js';
    s.onload = function () {
      syncSwaggerTheme();
      window.ui = SwaggerUIBundle({
        url: specUrl,
        dom_id: '#swagger-ui',
        presets: [SwaggerUIBundle.presets.apis],
        layout: 'BaseLayout'
      });
    };
    document.head.appendChild(s);
  }

  // ---- Tab switching ----
  window.apiSwitchTab = function (tab) {
    document.querySelectorAll('.api-panel').forEach(function (p) { p.classList.remove('active'); });
    document.querySelectorAll('#api-tabs button').forEach(function (b) { b.classList.remove('active'); });
    document.getElementById(tab + '-panel').classList.add('active');
    document.getElementById('tab-' + tab).classList.add('active');
    if (tab === 'redoc') loadRedoc();
    if (tab === 'swagger') loadSwagger();
    history.replaceState(null, '', '#' + tab);
  };

  // ---- Initial tab from URL hash ----
  var hash = (window.location.hash || '').replace('#', '');
  if (hash !== 'redoc' && hash !== 'swagger') hash = 'scalar';
  apiSwitchTab(hash);

  // ---- Palette change sync ----
  new MutationObserver(function (mutations) {
    mutations.forEach(function (m) {
      if (m.attributeName !== 'data-md-color-scheme') return;
      syncScalarTheme();
      if (swaggerLoaded) syncSwaggerTheme();
      if (redocLoaded && window.Redoc) initRedoc();
    });
  }).observe(document.body, { attributes: true });
})();
</script>
