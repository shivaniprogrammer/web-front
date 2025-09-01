# web-front
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>AI Resume + Portfolio Builder</title>
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <style>
    :root {
      --bg: #f8f9fa;
      --fg: #202124;
      --muted: #b0b6bc;
      --card: #fff;
      --accent: #2563eb;
      --border: #e5e7eb;
      --shadow: 0 2px 8px rgba(0,0,0,0.05);
      --radius: 10px;
      --focus: 0 0 0 2px var(--accent);
      --transition: 0.23s cubic-bezier(.4,0,.2,1);
    }
    @media (prefers-color-scheme: dark) {
      :root {
        --bg: #181a1b;
        --fg: #f3f4f6;
        --muted: #4b5563;
        --card: #222325;
        --accent: #60a5fa;
        --border: #26282a;
        --shadow: 0 2px 8px rgba(0,0,0,0.13);
      }
    }
    html[data-theme="dark"] {
      --bg: #181a1b;
      --fg: #f3f4f6;
      --muted: #4b5563;
      --card: #222325;
      --accent: #60a5fa;
      --border: #26282a;
      --shadow: 0 2px 8px rgba(0,0,0,0.13);
    }
    html[data-theme="light"] {
      --bg: #f8f9fa;
      --fg: #202124;
      --muted: #b0b6bc;
      --card: #fff;
      --accent: #2563eb;
      --border: #e5e7eb;
      --shadow: 0 2px 8px rgba(0,0,0,0.05);
    }
    html, body {
      margin: 0; padding: 0;
      font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Arial, sans-serif;
      background: var(--bg);
      color: var(--fg);
      min-height: 100%;
      height: 100%;
    }
    body {
      display: flex;
      flex-direction: column;
      min-height: 100vh;
    }
    header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 1.1rem 1.5rem 0.7rem 1.5rem;
      background: var(--bg);
      border-bottom: 1px solid var(--border);
      position: sticky;
      top: 0;
      z-index: 20;
    }
    header h1 {
      font-size: 1.2rem;
      font-weight: 600;
      letter-spacing: 0.01em;
      margin: 0;
      user-select: none;
    }
    .theme-toggle {
      background: var(--card);
      color: var(--fg);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 0.4em 1em;
      cursor: pointer;
      margin-left: 1.5rem;
      transition: box-shadow var(--transition), border-color var(--transition);
    }
    .theme-toggle:focus {
      outline: none;
      box-shadow: var(--focus);
      border-color: var(--accent);
    }
    nav {
      background: var(--bg);
      border-bottom: 1px solid var(--border);
      overflow-x: auto;
      scrollbar-width: thin;
    }
    .tablist {
      display: flex;
      gap: 0.5rem;
      padding: 0.5rem 1rem 0.1rem 1rem;
      margin: 0 auto;
      list-style: none;
    }
    .tab {
      font-size: 1rem;
      font-weight: 500;
      background: none;
      color: var(--fg);
      border: none;
      border-bottom: 2.5px solid transparent;
      padding: 0.6rem 1.3rem 0.5rem 1.3rem;
      cursor: pointer;
      border-radius: var(--radius) var(--radius) 0 0;
      transition: color var(--transition), border-color var(--transition), background var(--transition);
      position: relative;
      z-index: 2;
    }
    .tab[aria-selected="true"] {
      color: var(--accent);
      border-bottom: 2.5px solid var(--accent);
      background: var(--card);
    }
    .tab:focus {
      outline: none;
      box-shadow: var(--focus);
      z-index: 3;
    }
    main {
      flex: 1 1 auto;
      padding: 2.2rem 2vw 1.2rem 2vw;
      max-width: 670px;
      margin: 0 auto;
      width: 100%;
      display: flex;
      flex-direction: column;
      min-height: 0;
      position: relative;
    }
    .tabpanel {
      display: none;
      animation: fadeInTab 0.36s;
      min-height: 210px;
    }
    .tabpanel.active {
      display: block;
      animation: fadeInTab 0.36s;
    }
    @keyframes fadeInTab {
      from { opacity: 0; transform: translateY(7px);}
      to   { opacity: 1; transform: none;}
    }
    .options-grid {
      margin-top: 0.1rem;
      display: flex;
      flex-wrap: wrap;
      gap: 0.7rem;
    }
    .option-btn {
      background: var(--card);
      color: var(--fg);
      border: 1.5px solid var(--border);
      border-radius: var(--radius);
      padding: 0.65em 1.3em;
      font-size: 1rem;
      font-weight: 500;
      cursor: pointer;
      transition: background var(--transition), border-color var(--transition), box-shadow var(--transition), color var(--transition);
      margin-bottom: 0.13rem;
      min-width: 85px;
      position: relative;
      z-index: 1;
      outline: none;
    }
    .option-btn.selected, .option-btn[aria-pressed="true"] {
      background: var(--accent);
      color: #fff;
      border-color: var(--accent);
      box-shadow: 0 3px 18px 0 rgba(37,99,235,0.08);
    }
    .option-btn:focus {
      box-shadow: var(--focus);
      border-color: var(--accent);
    }
    .response-card {
      background: var(--card);
      border: 1.5px solid var(--border);
      border-radius: var(--radius);
      margin-top: 1.2rem;
      box-shadow: var(--shadow);
      padding: 1.2rem 1rem 1.2rem 1.3rem;
      font-size: 1.04rem;
      color: var(--fg);
      opacity: 0;
      transform: translateY(24px);
      animation: fadeInCard 0.45s forwards;
      max-width: 95vw;
    }
    @keyframes fadeInCard {
      to {
        opacity: 1;
        transform: none;
      }
    }
    .summary-section {
      margin-bottom: 1.7rem;
    }
    .summary-section h2 {
      font-size: 1.1rem;
      font-weight: 600;
      margin-bottom: 0.5em;
      margin-top: 1.2em;
      color: var(--accent);
      letter-spacing: 0.01em;
    }
    .summary-list, .summary-list-projects {
      padding-left: 1.1em;
      margin: 0;
    }
    .summary-list li, .summary-list-projects li {
      margin-bottom: 0.37em;
      font-size: 1.01rem;
      line-height: 1.5;
    }
    footer {
      position: sticky;
      bottom: 0;
      left: 0; right: 0;
      background: var(--bg);
      border-top: 1px solid var(--border);
      padding: 0.7rem 1rem 0.7rem 1rem;
      display: flex;
      align-items: center;
      justify-content: space-between;
      z-index: 99;
    }
    .footer-actions {
      display: flex;
      gap: 0.7rem;
    }
    .pdf-btn, .clear-btn {
      background: var(--accent);
      color: #fff;
      border: none;
      border-radius: var(--radius);
      padding: 0.55em 1.2em;
      font-size: 1rem;
      font-weight: 500;
      cursor: pointer;
      transition: background var(--transition), box-shadow var(--transition);
      box-shadow: 0 2px 8px rgba(37,99,235,0.11);
      outline: none;
    }
    .clear-btn {
      background: var(--muted);
      color: #fff;
      font-weight: 400;
      box-shadow: none;
    }
    .pdf-btn:focus, .clear-btn:focus {
      box-shadow: var(--focus);
    }
    @media (max-width: 700px) {
      main { padding-top: 1.1rem;}
      .response-card { padding: 1rem 0.7rem 1rem 1rem;}
    }
    @media (max-width: 550px) {
      .tablist {
        gap: 0.2rem;
        padding: 0.5rem 0.3rem 0.1rem 0.3rem;
      }
      .tab { padding: 0.55rem 0.8rem 0.45rem 0.8rem; font-size: 0.97rem;}
      main { padding: 1.1rem 0.3rem 1rem 0.3rem;}
      .options-grid { gap: 0.5rem;}
    }
    @media (max-width: 425px) {
      .tablist {
        overflow-x: auto;
        flex-wrap: nowrap;
        white-space: nowrap;
        scrollbar-width: thin;
      }
      .tab { min-width: 90px;}
    }
    /* Print Styles */
    @media print {
      html, body {
        background: #fff !important; color: #000 !important;
      }
      header, nav, footer, .response-card, .options-grid, .theme-toggle {
        display: none !important;
      }
      main {
        padding: 0 !important;
        max-width: none !important;
      }
      .summary-section {
        page-break-inside: avoid;
      }
      .summary-section h2 {
        color: #111 !important;
        font-size: 1.08rem;
        margin-top: 1.2em;
        margin-bottom: 0.3em;
      }
      .summary-list, .summary-list-projects {
        margin-bottom: 1.2em;
      }
      .summary-list li, .summary-list-projects li {
        color: #111;
        font-size: 1.01rem;
      }
      .summary-section:last-child {
        margin-bottom: 0;
      }
    }
    /* Visually Hidden for a11y */
    .visually-hidden {
      position: absolute;
      width: 1px; height: 1px;
      padding: 0; margin: -1px; overflow: hidden;
      clip: rect(0,0,0,0); border: 0;
    }
  </style>
</head>
<body>
  <header>
    <h1>AI Resume + Portfolio Builder</h1>
    <button id="themeToggle" class="theme-toggle" aria-label="Toggle dark/light mode" title="Toggle dark/light mode">🌙</button>
  </header>
  <nav aria-label="Resume sections">
    <div class="tablist" role="tablist" id="tablist">
      <button class="tab" role="tab" aria-selected="true" tabindex="0" aria-controls="tabpanel-education" id="tab-education" data-section="education">Education</button>
      <button class="tab" role="tab" aria-selected="false" tabindex="-1" aria-controls="tabpanel-skills" id="tab-skills" data-section="skills">Skills</button>
      <button class="tab" role="tab" aria-selected="false" tabindex="-1" aria-controls="tabpanel-experience" id="tab-experience" data-section="experience">Experience</button>
      <button class="tab" role="tab" aria-selected="false" tabindex="-1" aria-controls="tabpanel-certifications" id="tab-certifications" data-section="certifications">Certifications</button>
      <button class="tab" role="tab" aria-selected="false" tabindex="-1" aria-controls="tabpanel-achievements" id="tab-achievements" data-section="achievements">Achievements</button>
      <button class="tab" role="tab" aria-selected="false" tabindex="-1" aria-controls="tabpanel-projects" id="tab-projects" data-section="projects">Projects</button>
      <button class="tab" role="tab" aria-selected="false" tabindex="-1" aria-controls="tabpanel-summary" id="tab-summary" data-section="summary">Summary</button>
    </div>
  </nav>
  <main id="mainContent">
    <!-- Tab panels injected here -->
  </main>
  <footer>
    <div class="footer-actions">
      <button class="pdf-btn" id="pdfBtn" title="Download or print your resume summary (PDF)">Download as PDF</button>
      <button class="clear-btn" id="clearBtn" title="Clear all selections and start over">Clear All</button>
    </div>
  </footer>
  <script>
    // --- SECTION & OPTIONS DATA ---
    const SECTIONS = [
      {
        key: 'education',
        label: 'Education',
        options: [
          { key: 'bsc_cs', label: "B.Sc. Computer Science" },
          { key: 'msc_ai', label: "M.Sc. Artificial Intelligence" },
          { key: 'bootcamp_web', label: "Full-Stack Web Dev Bootcamp" },
          { key: 'phd_ml', label: "Ph.D. Machine Learning" }
        ]
      },
      {
        key: 'skills',
        label: 'Skills',
        options: [
          { key: 'python', label: "Python" },
          { key: 'javascript', label: "JavaScript" },
          { key: 'data_analysis', label: "Data Analysis" },
          { key: 'cloud', label: "Cloud Computing" },
          { key: 'uiux', label: "UI/UX Design" },
          { key: 'mlops', label: "MLOps" }
        ]
      },
      {
        key: 'experience',
        label: 'Experience',
        options: [
          { key: 'frontend_dev', label: "Frontend Developer" },
          { key: 'data_scientist', label: "Data Scientist" },
          { key: 'project_manager', label: "Project Manager" },
          { key: 'devops_engineer', label: "DevOps Engineer" }
        ]
      },
      {
        key: 'certifications',
        label: 'Certifications',
        options: [
          { key: 'aws_cp', label: "AWS Certified Cloud Practitioner" },
          { key: 'google_data', label: "Google Data Analytics" },
          { key: 'pm_pmp', label: "PMP Project Management" },
          { key: 'ux_nng', label: "NN/g UX Certified" }
        ]
      },
      {
        key: 'achievements',
        label: 'Achievements',
        options: [
          { key: 'open_source', label: "Open Source Contributor" },
          { key: 'hackathon', label: "Hackathon Winner" },
          { key: 'patent', label: "Patent Holder" },
          { key: 'conference', label: "Conference Speaker" }
        ]
      },
      {
        key: 'projects',
        label: 'Projects',
        options: [
          { key: 'ai_resume', label: "AI Resume Builder" },
          { key: 'fin_dash', label: "Financial Dashboard" },
          { key: 'ehealth', label: "eHealth Tracker" },
          { key: 'chatbot', label: "Customer Service Chatbot" }
        ]
      }
    ];
    // --- UNIQUE RESPONSES FOR EACH OPTION ---
    const RESPONSES = {
      // Education
      'education:bsc_cs': "Earned a Bachelor of Science in Computer Science, building a strong foundation in algorithms, data structures, and software engineering principles.",
      'education:msc_ai': "Completed a Master’s in Artificial Intelligence, focusing on machine learning, deep learning, and AI-driven problem solving.",
      'education:bootcamp_web': "Graduated from an intensive Full-Stack Web Development Bootcamp, mastering modern frameworks and agile methodologies in real-world projects.",
      'education:phd_ml': "Awarded a Ph.D. in Machine Learning, contributing original research to advanced neural architectures and published in leading journals.",
      // Skills
      'skills:python': "Proficient in Python for rapid prototyping, automation, data science, and scalable backend development.",
      'skills:javascript': "Skilled in JavaScript with expertise in both client-side and server-side development, enabling interactive and performant web applications.",
      'skills:data_analysis': "Adept at data analysis using statistical techniques and visualization tools to uncover actionable insights.",
      'skills:cloud': "Experienced in deploying, scaling, and managing applications on leading cloud platforms such as AWS, Azure, and GCP.",
      'skills:uiux': "Brings user-centric thinking to UI/UX design, crafting intuitive interfaces that enhance user satisfaction.",
      'skills:mlops': "Implements MLOps practices to streamline machine learning workflows, from model training to production deployment.",
      // Experience
      'experience:frontend_dev': "Worked as a Frontend Developer, delivering responsive, accessible UIs and collaborating closely with designers and backend teams.",
      'experience:data_scientist': "Served as a Data Scientist, developing predictive models and driving impact through data-driven business solutions.",
      'experience:project_manager': "Led cross-functional teams as a Project Manager, ensuring timely delivery and alignment with strategic goals.",
      'experience:devops_engineer': "Acted as a DevOps Engineer, automating CI/CD pipelines and optimizing cloud infrastructure for reliability and performance.",
      // Certifications
      'certifications:aws_cp': "Achieved AWS Certified Cloud Practitioner, validating proficiency in cloud concepts and AWS core services.",
      'certifications:google_data': "Earned the Google Data Analytics Certification, demonstrating expertise in data cleaning, analysis, and visualization.",
      'certifications:pm_pmp': "Certified as a Project Management Professional (PMP), evidencing advanced project leadership and organizational skills.",
      'certifications:ux_nng': "Obtained Nielsen Norman Group UX Certification, specializing in usability and evidence-based design.",
      // Achievements
      'achievements:open_source': "Contributed to prominent open source projects, collaborating globally and enhancing software used by thousands.",
      'achievements:hackathon': "Won first place in a major hackathon, delivering an innovative tech solution under tight deadlines.",
      'achievements:patent': "Invented and patented a novel technology, demonstrating creativity and technical depth.",
      'achievements:conference': "Invited to speak at international conferences, sharing expertise and advancing community knowledge.",
      // Projects
      'projects:ai_resume': "Built an AI-powered Resume Builder that personalizes content for users, leveraging NLP and interactive UX.",
      'projects:fin_dash': "Developed a Financial Dashboard enabling real-time analytics and custom reporting for business stakeholders.",
      'projects:ehealth': "Created an eHealth Tracker to monitor patient metrics and provide actionable health recommendations.",
      'projects:chatbot': "Engineered a Customer Service Chatbot, automating support and improving client satisfaction with AI-driven responses."
    };
    // --- LOCAL STORAGE KEYS ---
    const STORAGE_KEY = 'ai-resume-builder-v1';
    const THEME_KEY = 'ai-resume-theme';
    // --- STATE ---
    let state = {
      selected: {
        // section: Set of keys
        education: [],
        skills: [],
        experience: [],
        certifications: [],
        achievements: [],
        projects: []
      },
      // { section: { optionKey: true } }
      responses: {},
      activeTab: 'education',
      theme: null // 'light', 'dark', or null (system)
    };

    // --- UTILITIES ---
    function saveState() {
      localStorage.setItem(STORAGE_KEY, JSON.stringify({
        selected: state.selected,
        responses: state.responses,
        activeTab: state.activeTab
      }));
    }
    function loadState() {
      const stored = localStorage.getItem(STORAGE_KEY);
      if (stored) {
        try {
          const parsed = JSON.parse(stored);
          if (parsed.selected && parsed.responses) {
            state.selected = parsed.selected;
            state.responses = parsed.responses;
            state.activeTab = parsed.activeTab || 'education';
          }
        } catch {}
      }
    }
    function saveTheme(theme) {
      state.theme = theme;
      localStorage.setItem(THEME_KEY, theme);
      setTheme(theme);
    }
    function loadTheme() {
      let theme = localStorage.getItem(THEME_KEY);
      if (theme) {
        setTheme(theme);
        state.theme = theme;
      } else {
        setTheme(null);
        state.theme = null;
      }
    }
    function setTheme(theme) {
      if (!theme) {
        document.documentElement.removeAttribute('data-theme');
        document.getElementById('themeToggle').innerText = window.matchMedia('(prefers-color-scheme: dark)').matches ? '☀️' : '🌙';
      } else {
        document.documentElement.setAttribute('data-theme', theme);
        document.getElementById('themeToggle').innerText = theme === 'dark' ? '☀️' : '🌙';
      }
    }
    // --- TABS ---
    function getTabIdx(section) {
      return SECTIONS.concat({key:'summary'}).findIndex(s => s.key === section);
    }
    // --- RENDERING ---
    function renderTabs() {
      const tablist = document.getElementById('tablist');
      const tabs = tablist.querySelectorAll('.tab');
      tabs.forEach(tab => {
        const section = tab.dataset.section;
        if (section === state.activeTab) {
          tab.setAttribute('aria-selected', 'true');
          tab.tabIndex = 0;
        } else {
          tab.setAttribute('aria-selected', 'false');
          tab.tabIndex = -1;
        }
      });
    }
    function clearMainContent() {
      document.getElementById('mainContent').innerHTML = '';
    }
    function renderPanel(section) {
      clearMainContent();
      const main = document.getElementById('mainContent');
      let panel = document.createElement('section');
      panel.className = 'tabpanel active';
      panel.setAttribute('role', 'tabpanel');
      panel.id = 'tabpanel-' + section;
      panel.setAttribute('aria-labelledby', 'tab-' + section);

      if (section === 'summary') {
        panel.innerHTML = renderSummary();
      } else {
        // Show options as buttons
        const secObj = SECTIONS.find(s => s.key === section);
        const grid = document.createElement('div');
        grid.className = 'options-grid';
        secObj.options.forEach(opt => {
          const btn = document.createElement('button');
          btn.type = 'button';
          btn.className = 'option-btn';
          btn.setAttribute('data-section', section);
          btn.setAttribute('data-key', opt.key);
          btn.setAttribute('aria-pressed', state.selected[section].includes(opt.key) ? 'true' : 'false');
          if (state.selected[section].includes(opt.key)) btn.classList.add('selected');
          btn.textContent = opt.label;
          btn.addEventListener('click', () => handleOptionClick(section, opt.key));
          grid.appendChild(btn);
        });
        panel.appendChild(grid);
        // Show response cards for selected options
        if (state.selected[section].length > 0) {
          state.selected[section].forEach(optKey => {
            const respKey = `${section}:${optKey}`;
            if (RESPONSES[respKey]) {
              const card = document.createElement('div');
              card.className = 'response-card';
              card.setAttribute('data-section', section);
              card.setAttribute('data-key', optKey);
              card.textContent = RESPONSES[respKey];
              panel.appendChild(card);
            }
          });
        }
      }
      main.appendChild(panel);
    }
    function renderSummary() {
      // Compile by sections
      let html = '';
      SECTIONS.forEach(sec => {
        if (state.selected[sec.key].length === 0) return;
        html += `<div class="summary-section"><h2>${sec.label}</h2><ul class="${sec.key==='projects'?'summary-list-projects':'summary-list'}">`;
        state.selected[sec.key].forEach(optKey => {
          const respKey = `${sec.key}:${optKey}`;
          if (RESPONSES[respKey]) {
            html += `<li>${RESPONSES[respKey]}</li>`;
          }
        });
        html += '</ul></div>';
      });
      if (!html) {
        html = `<div class="summary-section"><p style="color:var(--muted);font-size:1.04rem;">No selections yet. Choose options in each section to build your summary.</p></div>`;
      }
      return html;
    }
    // --- OPTION BUTTON HANDLING ---
    function handleOptionClick(section, optKey) {
      const idx = state.selected[section].indexOf(optKey);
      if (idx === -1) {
        state.selected[section].push(optKey);
      } else {
        state.selected[section].splice(idx, 1);
      }
      saveState();
      renderPanel(section);
    }
    // --- TAB HANDLING ---
    function handleTabClick(e) {
      const section = e.currentTarget.dataset.section;
      if (state.activeTab !== section) {
        state.activeTab = section;
        saveState();
        renderTabs();
        renderPanel(section);
      }
    }
    function handleTabKeydown(e) {
      const tabs = Array.from(document.querySelectorAll('.tab'));
      const idx = tabs.findIndex(tab => tab.dataset.section === state.activeTab);
      if (e.key === 'ArrowRight' || e.key === 'Right') {
        e.preventDefault();
        const next = (idx + 1) % tabs.length;
        tabs[next].focus();
      } else if (e.key === 'ArrowLeft' || e.key === 'Left') {
        e.preventDefault();
        const prev = (idx - 1 + tabs.length) % tabs.length;
        tabs[prev].focus();
      } else if (e.key === 'Home') {
        e.preventDefault();
        tabs[0].focus();
      } else if (e.key === 'End') {
        e.preventDefault();
        tabs[tabs.length-1].focus();
      } else if (e.key === 'Enter' || e.key === ' ' || e.key === 'Spacebar') {
        e.preventDefault();
        tabs[idx].click();
      }
    }
    function setupTabs() {
      const tabs = document.querySelectorAll('.tab');
      tabs.forEach(tab => {
        tab.addEventListener('click', handleTabClick);
        tab.addEventListener('keydown', handleTabKeydown);
      });
    }
    // --- THEME TOGGLE ---
    function setupThemeToggle() {
      const btn = document.getElementById('themeToggle');
      btn.addEventListener('click', () => {
        let newTheme;
        if (state.theme === 'dark') newTheme = 'light';
        else if (state.theme === 'light') newTheme = 'dark';
        else newTheme = window.matchMedia('(prefers-color-scheme: dark)').matches ? 'light' : 'dark';
        saveTheme(newTheme);
      });
      // Listen for system theme changes
      window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', (e) => {
        if (!state.theme) setTheme(null);
      });
    }
    // --- PDF PRINT ---
    function setupPdfBtn() {
      document.getElementById('pdfBtn').addEventListener('click', () => {
        // Switch to summary if not already, then print
        if (state.activeTab !== 'summary') {
          state.activeTab = 'summary';
          saveState();
          renderTabs();
          renderPanel('summary');
          setTimeout(() => window.print(), 150);
        } else {
          window.print();
        }
      });
    }
    // --- CLEAR ALL ---
    function setupClearBtn() {
      document.getElementById('clearBtn').addEventListener('click', () => {
        if (confirm("Clear all selections and restart?")) {
          localStorage.removeItem(STORAGE_KEY);
          state.selected = {
            education: [],
            skills: [],
            experience: [],
            certifications: [],
            achievements: [],
            projects: []
          };
          state.responses = {};
          state.activeTab = 'education';
          saveState();
          renderTabs();
          renderPanel('education');
        }
      });
    }
    // --- INIT ---
    function init() {
      loadState();
      loadTheme();
      renderTabs();
      renderPanel(state.activeTab);
      setupTabs();
      setupThemeToggle();
      setupPdfBtn();
      setupClearBtn();
      // Keyboard navigation: focus tab on Alt+[1-7]
      document.addEventListener('keydown', e => {
        if (e.altKey && !e.shiftKey && !e.ctrlKey && !e.metaKey) {
          const num = Number(e.key);
          if (num >= 1 && num <= 7) {
            const section = (SECTIONS.concat({key:'summary'}))[num-1].key;
            state.activeTab = section;
            saveState();
            renderTabs();
            renderPanel(section);
            document.getElementById('tab-' + section).focus();
          }
        }
      });
    }
    window.addEventListener('DOMContentLoaded', init);
  </script>
</body>
</html>
