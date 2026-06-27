<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบแนะนำห้องเรียน โรงเรียนศรียาภัย | AI Classroom Recommender</title>
    <meta name="description" content="ระบบ AI แนะนำห้องเรียนที่เหมาะสมตามความสนใจและทักษะ สำหรับนักเรียนมัธยมต้นและมัธยมปลาย โรงเรียนศรียาภัย (SMTE, SMT, IEP, EIS, SMEP)">
    <meta name="keywords" content="โรงเรียนศรียาภัย, แนะนำห้องเรียน, SMTE, SMT, IEP, EIS, SMEP, แบบทดสอบเรียนต่อ">
    <meta name="author" content="Sriyapai School">
    
    <link rel="stylesheet" href="styles.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
</head>
<body class="light-mode">
    <div class="bg-shape shape-1"></div>
    <div class="bg-shape shape-2"></div>
    <div class="bg-shape shape-3"></div>

    <header class="glass-nav">
        <div class="logo">
            <i class="fa-solid fa-graduation-cap"></i>
            <div>
                <h1>โรงเรียนศรียาภัย</h1>
                <p>ระบบแนะนำห้องเรียน</p>
            </div>
        </div>
        <nav aria-label="Main Navigation">
            <button class="nav-btn" onclick="app.navigate('home')">หน้าแรก</button>
            <button class="nav-btn" onclick="app.navigate('rooms')">ข้อมูลห้องเรียน</button>
            <button class="nav-btn" onclick="app.navigate('compare')">เปรียบเทียบ</button>
            <button class="nav-btn" onclick="app.navigate('admin')">แอดมิน</button>
        </nav>
        <button id="theme-toggle" class="icon-btn" aria-label="สลับโหมดสว่างและมืด">
            <i class="fa-solid fa-moon"></i>
        </button>
    </header>

    <main id="app-container" class="glass-container">
        
        <section id="home" class="page-section active">
            <div class="hero">
                <h2 class="animate-slide-up">ค้นหาห้องเรียนที่ใช่ สำหรับอนาคตของคุณ</h2>
                <p class="animate-slide-up delay-1">ให้ AI ของเราช่วยวิเคราะห์ความสนใจ ความถนัด และเป้าหมายของคุณ เพื่อแนะนำแผนการเรียนที่เหมาะสมที่สุดสำหรับคุณในโรงเรียนศรียาภัย</p>
                <div class="hero-actions animate-slide-up delay-2">
                    <button class="btn btn-primary" onclick="app.navigate('test')"><i class="fa-solid fa-brain"></i> ทำแบบทดสอบ AI</button>
                    <button class="btn btn-secondary" onclick="app.navigate('rooms')"><i class="fa-solid fa-book-open"></i> ดูข้อมูลห้องเรียน</button>
                </div>
            </div>
            <div class="faq-section animate-fade-in delay-2">
                <h3>คำถามที่พบบ่อย (FAQ)</h3>
                <div id="faq-container"></div>
            </div>
        </section>

        <section id="rooms" class="page-section hidden">
            <div class="section-header">
                <h2>ข้อมูลแผนการเรียน</h2>
                <div class="filter-btns">
                    <button class="btn btn-sm btn-active" onclick="app.filterRooms('all', this)">ทั้งหมด</button>
                    <button class="btn btn-sm" onclick="app.filterRooms('junior', this)">มัธยมศึกษาตอนต้น</button>
                    <button class="btn btn-sm" onclick="app.filterRooms('senior', this)">มัธยมศึกษาตอนปลาย</button>
                </div>
            </div>
            <div id="rooms-grid" class="cards-grid">
                </div>
        </section>

        <section id="test" class="page-section hidden">
            <h2>แบบทดสอบวิเคราะห์ความถนัด</h2>
            <p>กรุณาเลือกระดับความเห็นด้วยในแต่ละข้อ (1 = น้อยที่สุด, 5 = มากที่สุด)</p>
            <form id="quiz-form">
                <div id="questions-container">
                    </div>
                <div class="form-actions">
                    <button type="submit" class="btn btn-primary w-100">ประมวลผลผลลัพธ์</button>
                </div>
            </form>
        </section>

        <section id="result" class="page-section hidden">
            <h2>ผลการวิเคราะห์ของคุณ</h2>
            <p>นี่คือห้องเรียนที่เหมาะสมกับคุณที่สุด 3 อันดับแรก</p>
            <div id="results-container" class="results-layout">
                </div>
            <div class="form-actions mt-2">
                <button class="btn btn-secondary" onclick="app.navigate('test')">ทำแบบทดสอบอีกครั้ง</button>
            </div>
        </section>

        <section id="compare" class="page-section hidden">
            <h2>เปรียบเทียบแผนการเรียน</h2>
            <div class="compare-selectors">
                <select id="compare-1" class="glass-input" aria-label="เลือกห้องเรียนที่ 1"></select>
                <span class="vs">VS</span>
                <select id="compare-2" class="glass-input" aria-label="เลือกห้องเรียนที่ 2"></select>
                <button class="btn btn-primary" onclick="app.renderComparison()">เปรียบเทียบ</button>
            </div>
            <div id="compare-result" class="compare-table-container">
                </div>
        </section>

        <section id="admin" class="page-section hidden">
            <h2>Admin Dashboard (จำลอง)</h2>
            <p>จัดการข้อมูลห้องเรียน (ข้อมูลจะบันทึกชั่วคราวในหน่วยความจำ)</p>
            
            <div class="admin-grid">
                <div class="glass-card">
                    <h3>เพิ่มห้องเรียนใหม่</h3>
                    <form id="add-room-form">
                        <input type="text" id="admin-id" placeholder="รหัส (เช่น scp)" required class="glass-input mb-1">
                        <input type="text" id="admin-name" placeholder="ชื่อย่อ (เช่น SCP)" required class="glass-input mb-1">
                        <input type="text" id="admin-desc" placeholder="ชื่อเต็ม" required class="glass-input mb-1">
                        <select id="admin-level" class="glass-input mb-1">
                            <option value="junior">มัธยมศึกษาตอนต้น</option>
                            <option value="senior">มัธยมศึกษาตอนปลาย</option>
                            <option value="both">ทั้งต้นและปลาย</option>
                        </select>
                        <button type="submit" class="btn btn-primary w-100">เพิ่มข้อมูล</button>
                    </form>
                </div>
                <div class="glass-card" style="overflow-x: auto;">
                    <h3>รายชื่อห้องเรียนในระบบ</h3>
                    <table class="glass-table w-100" id="admin-table">
                        <thead>
                            <tr>
                                <th>รหัส</th>
                                <th>ชื่อ</th>
                                <th>จัดการ</th>
                            </tr>
                        </thead>
                        <tbody></tbody>
                    </table>
                </div>
            </div>
        </section>

    </main>

    <div id="room-modal" class="modal hidden" aria-hidden="true">
        <div class="modal-content glass-card">
            <span class="close-btn" onclick="app.closeModal()">&times;</span>
            <div id="modal-body"></div>
        </div>
    </div>

    <script src="script.js"></script>
</body>
</html>
/* =========================================================
   CSS3 - Modern Glassmorphism & Responsive Variables
   ========================================================= */
:root {
    /* Light Mode Variables */
    --bg-main: #e0f2fe; /* Light blue */
    --bg-shape1: #3b82f6;
    --bg-shape2: #93c5fd;
    --bg-shape3: #bfdbfe;
    
    --glass-bg: rgba(255, 255, 255, 0.6);
    --glass-border: rgba(255, 255, 255, 0.4);
    --glass-shadow: rgba(31, 38, 135, 0.15);
    
    --text-primary: #1e3a8a;
    --text-secondary: #334155;
    
    --primary-color: #2563eb;
    --primary-hover: #1d4ed8;
    --accent-color: #0ea5e9;
    --success-color: #10b981;
}

/* Dark Mode Variables */
body.dark-mode {
    --bg-main: #0f172a;
    --bg-shape1: #1e3a8a;
    --bg-shape2: #0369a1;
    --bg-shape3: #0c4a6e;
    
    --glass-bg: rgba(15, 23, 42, 0.7);
    --glass-border: rgba(255, 255, 255, 0.1);
    --glass-shadow: rgba(0, 0, 0, 0.5);
    
    --text-primary: #f8fafc;
    --text-secondary: #cbd5e1;
}

/* Base Styles */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    transition: background-color 0.3s, color 0.3s;
}

body {
    background-color: var(--bg-main);
    color: var(--text-primary);
    min-height: 100vh;
    overflow-x: hidden;
    position: relative;
}

/* Background Shapes for Glass Effect */
.bg-shape {
    position: fixed;
    border-radius: 50%;
    filter: blur(80px);
    z-index: -1;
    animation: float 20s infinite ease-in-out alternate;
}
.shape-1 { width: 400px; height: 400px; background: var(--bg-shape1); top: -100px; left: -100px; }
.shape-2 { width: 500px; height: 500px; background: var(--bg-shape2); bottom: -150px; right: -100px; animation-delay: -5s;}
.shape-3 { width: 300px; height: 300px; background: var(--bg-shape3); top: 40%; left: 30%; animation-delay: -10s;}

@keyframes float {
    0% { transform: translate(0, 0) scale(1); }
    100% { transform: translate(50px, 50px) scale(1.1); }
}

/* Glassmorphism Utilities */
.glass-nav, .glass-container, .glass-card, .glass-input {
    background: var(--glass-bg);
    backdrop-filter: blur(12px);
    -webkit-backdrop-filter: blur(12px);
    border: 1px solid var(--glass-border);
    box-shadow: 0 8px 32px 0 var(--glass-shadow);
}

/* Navigation */
.glass-nav {
    position: sticky;
    top: 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem 5%;
    z-index: 100;
    border-bottom-left-radius: 20px;
    border-bottom-right-radius: 20px;
}

.logo {
    display: flex;
    align-items: center;
    gap: 10px;
}
.logo h1 { font-size: 1.2rem; }
.logo p { font-size: 0.8rem; color: var(--text-secondary); }

nav { display: flex; gap: 15px; }

.nav-btn, .icon-btn {
    background: transparent;
    border: none;
    color: var(--text-primary);
    font-weight: 600;
    cursor: pointer;
    padding: 5px 10px;
    border-radius: 5px;
    transition: 0.3s;
}
.nav-btn:hover { background: rgba(255,255,255,0.2); }

/* Main Container */
.glass-container {
    width: 90%;
    max-width: 1200px;
    margin: 2rem auto;
    border-radius: 20px;
    padding: 2rem;
    min-height: 70vh;
}

/* Sections (SPA Hide/Show) */
.page-section { display: none; animation: fadeIn 0.5s ease-out; }
.page-section.active { display: block; }

/* Typography & Utilities */
h2 { margin-bottom: 1rem; color: var(--primary-color); }
.text-center { text-align: center; }
.w-100 { width: 100%; }
.mb-1 { margin-bottom: 1rem; }
.mt-2 { margin-top: 2rem; }

/* Buttons */
.btn {
    padding: 10px 20px;
    border-radius: 10px;
    border: none;
    cursor: pointer;
    font-weight: bold;
    transition: transform 0.2s, box-shadow 0.2s;
    display: inline-flex;
    align-items: center;
    gap: 8px;
    justify-content: center;
}
.btn:hover { transform: translateY(-2px); box-shadow: 0 5px 15px var(--glass-shadow); }
.btn-primary { background: var(--primary-color); color: white; }
.btn-secondary { background: var(--glass-bg); color: var(--text-primary); border: 1px solid var(--primary-color); }
.btn-sm { padding: 5px 15px; font-size: 0.9rem; }
.btn-active { background: var(--primary-color); color: white; }

/* Hero Section */
.hero { text-align: center; padding: 4rem 0; }
.hero h2 { font-size: 2.5rem; }
.hero p { font-size: 1.1rem; color: var(--text-secondary); max-width: 600px; margin: 1rem auto 2rem; }
.hero-actions { display: flex; justify-content: center; gap: 15px; }

/* Cards Grid */
.cards-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
    gap: 20px;
    margin-top: 20px;
}
.glass-card {
    border-radius: 15px;
    padding: 1.5rem;
    cursor: pointer;
    transition: 0.3s;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
}
.glass-card:hover { transform: translateY(-5px); border-color: var(--primary-color); }
.card-tags { display: flex; gap: 5px; margin-bottom: 10px; flex-wrap: wrap;}
.tag { font-size: 0.75rem; padding: 3px 8px; border-radius: 20px; background: rgba(37, 99, 235, 0.2); color: var(--primary-color); font-weight: bold; }

/* Forms & Inputs */
.glass-input {
    width: 100%;
    padding: 12px;
    border-radius: 10px;
    color: var(--text-primary);
    outline: none;
}
.question-block {
    background: rgba(255,255,255,0.05);
    padding: 15px;
    border-radius: 10px;
    margin-bottom: 15px;
    border: 1px solid var(--glass-border);
}
.question-text { font-weight: bold; margin-bottom: 10px; }
.options { display: flex; justify-content: space-between; gap: 10px; }
.options label {
    flex: 1; text-align: center; background: var(--glass-bg);
    padding: 10px; border-radius: 5px; cursor: pointer; transition: 0.2s;
}
.options input[type="radio"] { display: none; }
.options input[type="radio"]:checked + span { color: var(--primary-color); font-weight: bold; }
.options label:has(input:checked) { border: 2px solid var(--primary-color); }

/* Progress Bars */
.progress-container { width: 100%; background: var(--glass-border); border-radius: 10px; height: 12px; overflow: hidden; margin-top: 5px; }
.progress-bar { height: 100%; background: var(--success-color); border-radius: 10px; transition: width 1s ease-in-out; }

/* Results Layout */
.results-layout { display: grid; gap: 20px; margin-top: 20px; }
.result-top1 { grid-column: 1 / -1; border: 2px solid var(--primary-color); transform: scale(1.02); }
.result-top1 h3 { font-size: 1.5rem; }

/* Compare Table & Admin */
.compare-selectors { display: flex; gap: 10px; align-items: center; margin-bottom: 20px; flex-wrap: wrap;}
.glass-table { border-collapse: collapse; text-align: left; margin-top: 10px;}
.glass-table th, .glass-table td { padding: 12px; border-bottom: 1px solid var(--glass-border); }
.admin-grid { display: grid; grid-template-columns: 1fr 2fr; gap: 20px; }

/* Modal */
.modal {
    position: fixed; top: 0; left: 0; width: 100%; height: 100%;
    background: rgba(0, 0, 0, 0.5); z-index: 1000;
    display: flex; justify-content: center; align-items: center;
    opacity: 0; visibility: hidden; transition: 0.3s;
}
.modal:not(.hidden) { opacity: 1; visibility: visible; }
.modal-content {
    width: 90%; max-width: 600px; max-height: 90vh; overflow-y: auto;
    position: relative; animation: slideUp 0.4s ease-out;
}
.close-btn { position: absolute; top: 15px; right: 20px; font-size: 1.5rem; cursor: pointer; color: var(--text-primary); }

/* Accordion */
.accordion-item { border-bottom: 1px solid var(--glass-border); margin-bottom: 10px; }
.accordion-header { padding: 15px; cursor: pointer; font-weight: bold; background: rgba(0,0,0,0.02); }
.accordion-body { padding: 0 15px; max-height: 0; overflow: hidden; transition: 0.3s; }
.accordion-item.open .accordion-body { padding: 15px; max-height: 300px; }

/* Animations */
@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
@keyframes slideUp { from { opacity: 0; transform: translateY(30px); } to { opacity: 1; transform: translateY(0); } }
.animate-slide-up { animation: slideUp 0.8s ease-out backwards; }
.animate-fade-in { animation: fadeIn 1s ease-out backwards; }
.delay-1 { animation-delay: 0.2s; }
.delay-2 { animation-delay: 0.4s; }

/* Responsive */
@media (max-width: 768px) {
    nav { display: none; /* In real app, add hamburger menu. Hidden here for simplicity */ }
    .admin-grid { grid-template-columns: 1fr; }
    .hero h2 { font-size: 2rem; }
    .hero-actions { flex-direction: column; }
}
/* =========================================================
   Vanilla JS (ES6+) - System Logic & Rule-based AI Engine
   ========================================================= */

// 1. Data Source: ข้อมูลห้องเรียนทั้งหมด
let roomsData = [
    { 
        id: 'smte', name: 'SMTE', levels: ['junior', 'senior'], 
        fullName: 'Science Mathematics and Technology Education', 
        focus: 'เน้นวิทย์, คณิต, เทคโนโลยี, วิจัย/นวัตกรรม', 
        subjects: 'ฟิสิกส์, เคมี, ชีววิทยา, คอมพิวเตอร์', 
        careers: 'แพทย์, วิศวกร, นักวิจัย, นักวิเคราะห์ข้อมูล',
        strengths: 'ทักษะกระบวนการคิดเชิงวิทยาศาสตร์และการวิจัยขั้นสูง'
    },
    { 
        id: 'smt', name: 'SMT', levels: ['junior', 'senior'], 
        fullName: 'Science Mathematics and Technology Program', 
        focus: 'เน้นพื้นฐานวิทย์-คณิต พัฒนาการคิดวิเคราะห์', 
        subjects: 'คณิต, วิทย์, เทคโนโลยี', 
        careers: 'วิศวกร, ครู, นักวิทย์, โปรแกรมเมอร์',
        strengths: 'การแก้ปัญหาตรรกะและการคิดวิเคราะห์'
    },
    { 
        id: 'iep', name: 'IEP', levels: ['junior', 'senior'], 
        fullName: 'Intensive English Program', 
        focus: 'เรียนวิชาหลักเป็นภาษาอังกฤษ สื่อสารระดับสากล', 
        subjects: 'อังกฤษ, คณิต, วิทย์เพื่อการสื่อสาร', 
        careers: 'ล่าม, นักการทูต, พนักงานสายการบิน, บริษัทต่างประเทศ',
        strengths: 'ทักษะการสื่อสารภาษาอังกฤษและวิสัยทัศน์สากล'
    },
    { 
        id: 'eis', name: 'EIS', levels: ['junior'], 
        fullName: 'English for Integrated Studies', 
        focus: 'ใช้ภาษาอังกฤษเป็นสื่อการสอนควบคู่กับวิชาหลัก', 
        subjects: 'อังกฤษ, วิทย์, คณิต, สังคม', 
        careers: 'ครู, นักแปล, นักธุรกิจ, นักการตลาดข้ามชาติ',
        strengths: 'ความคุ้นเคยกับการใช้ภาษาอังกฤษในชีวิตประจำวัน'
    },
    { 
        id: 'smep', name: 'SMEP', levels: ['senior'], 
        fullName: 'Science Mathematics and English Program', 
        focus: 'ผสมผสาน STEM ควบคู่ภาษาอังกฤษระดับสูง', 
        subjects: 'วิทย์, คณิต, อังกฤษ, เทคโนโลยี', 
        careers: 'แพทย์, วิศวกรระหว่างประเทศ, นักวิจัย, นักธุรกิจระหว่างประเทศ',
        strengths: 'ความเป็นเลิศทั้งด้านวิชาการ (STEM) และภาษาต่างประเทศ'
    }
];

// 2. Data Source: คำถาม 15 ข้อ และน้ำหนักคะแนน (Weights) สำหรับ AI Algorithm
const questionsData = [
    { text: "1. ฉันชอบเรียนวิชาคณิตศาสตร์และวิทยาศาสตร์เป็นพิเศษ", weights: { smte: 3, smt: 2, smep: 2, iep: 0, eis: 0 } },
    { text: "2. ฉันสนใจการทดลองทางวิทยาศาสตร์และสร้างสรรค์นวัตกรรมใหม่ๆ", weights: { smte: 3, smep: 2, smt: 1, iep: 0, eis: 0 } },
    { text: "3. ฉันชอบที่จะพูดคุยและสื่อสารภาษาอังกฤษกับชาวต่างชาติ", weights: { iep: 3, eis: 2, smep: 2, smte: 0, smt: 0 } },
    { text: "4. ฉันใฝ่ฝันอยากมีอาชีพที่ต้องใช้ทักษะภาษาอังกฤษเป็นหลัก (เช่น นักการทูต, ล่าม)", weights: { iep: 3, eis: 2, smep: 1, smte: 0, smt: 0 } },
    { text: "5. ฉันสนุกกับการเขียนโปรแกรม ใช้คอมพิวเตอร์ และเรียนรู้เทคโนโลยีใหม่ๆ", weights: { smte: 2, smt: 2, smep: 2, iep: 0, eis: 0 } },
    { text: "6. ฉันสามารถทำความเข้าใจเนื้อหาวิชาวิทย์-คณิต ที่เขียนเป็นภาษาอังกฤษได้", weights: { smep: 3, eis: 2, iep: 1, smte: 1, smt: 0 } },
    { text: "7. ฉันชอบการทำโครงงาน (Project) และการค้นคว้าวิจัยอย่างลึกซึ้ง", weights: { smte: 3, smep: 2, smt: 1, iep: 0, eis: 0 } },
    { text: "8. ฉันชอบเรียนรู้เกี่ยวกับวัฒนธรรมต่างประเทศและภาษาที่สอง", weights: { iep: 3, eis: 2, smep: 1, smte: 0, smt: 0 } },
    { text: "9. ฉันมีความถนัดในการคิดคำนวณและแก้ปัญหาโจทย์ที่ซับซ้อน", weights: { smte: 2, smt: 2, smep: 2, iep: 0, eis: 0 } },
    { text: "10. ฉันต้องการต่อยอดสายอาชีพด้านการแพทย์ วิศวกรรม หรือวิทยาศาสตร์ประยุกต์", weights: { smte: 3, smep: 3, smt: 2, iep: 0, eis: 0 } },
    { text: "11. ฉันอยากเรียนวิชาพื้นฐานอย่าง สังคมศึกษาและวิทยาศาสตร์ เป็นภาษาอังกฤษ", weights: { eis: 3, iep: 2, smep: 1, smte: 0, smt: 0 } },
    { text: "12. ฉันชอบการทำงานกลุ่มและการนำเสนอผลงาน (Presentation) หน้าชั้นเรียน", weights: { iep: 2, eis: 2, smep: 1, smte: 1, smt: 1 } },
    { text: "13. ฉันสนใจการใช้งานเครื่องมือวิทยาศาสตร์ในห้องปฏิบัติการอย่างจริงจัง", weights: { smte: 3, smt: 2, smep: 2, iep: 0, eis: 0 } },
    { text: "14. ฉันมักจะติดตามข่าวสารด้านเทคโนโลยีและนวัตกรรมระดับโลกอยู่เสมอ", weights: { smte: 2, smep: 2, smt: 1, iep: 0, eis: 0 } },
    { text: "15. ฉันต้องการเตรียมความพร้อมเพื่อการศึกษาต่อในระดับมหาวิทยาลัยที่ต่างประเทศ", weights: { iep: 3, smep: 3, eis: 1, smte: 1, smt: 0 } }
];

// FAQ Data
const faqData = [
    { q: "แบบทดสอบนี้แม่นยำแค่ไหน?", a: "AI Algorithm ใช้ระบบ Rule-based ถ่วงน้ำหนักคะแนนตามความสอดคล้องของหลักสูตร เพื่อเป็นแนวทางเบื้องต้นประกอบการตัดสินใจ" },
    { q: "มัธยมต้นและมัธยมปลายมีแผนการเรียนต่างกันอย่างไร?", a: "ม.ต้น จะมี SMTE, SMT, IEP, EIS ส่วน ม.ปลาย จะไม่มี EIS แต่มี SMEP เข้ามาแทน เพื่อความเข้มข้นทางวิชาการและภาษา" }
];

// 3. Application Core Logic
const app = {
    // Navigation (SPA)
    navigate: function(sectionId) {
        document.querySelectorAll('.page-section').forEach(sec => sec.classList.add('hidden'));
        document.querySelectorAll('.page-section').forEach(sec => sec.classList.remove('active'));
        
        const target = document.getElementById(sectionId);
        target.classList.remove('hidden');
        
        // Trigger reflow to restart animation
        void target.offsetWidth; 
        target.classList.add('active');
        window.scrollTo({ top: 0, behavior: 'smooth' });
    },

    // Initialize Application
    init: function() {
        this.renderRooms('all');
        this.renderQuestions();
        this.renderFAQ();
        this.setupThemeToggle();
        this.setupFormSubmit();
        this.populateCompareSelectors();
        this.renderAdminTable();
        this.setupAdminForm();
    },

    // Render Room Cards
    renderRooms: function(filter) {
        const container = document.getElementById('rooms-grid');
        container.innerHTML = '';
        
        let filteredData = roomsData;
        if (filter !== 'all') {
            filteredData = roomsData.filter(room => room.levels.includes(filter));
        }

        filteredData.forEach(room => {
            const card = document.createElement('div');
            card.className = 'glass-card';
            
            // Map tags
            const tagsHtml = room.levels.map(l => `<span class="tag">${l === 'junior' ? 'ม.ต้น' : 'ม.ปลาย'}</span>`).join('');
            
            card.innerHTML = `
                <div>
                    <div class="card-tags">${tagsHtml}</div>
                    <h3 style="color: var(--primary-color);">${room.name}</h3>
                    <p style="font-size: 0.9rem; margin-bottom: 10px;">${room.fullName}</p>
                    <p><i class="fa-solid fa-bullseye"></i> <strong>เน้น:</strong> ${room.focus}</p>
                </div>
                <button class="btn btn-secondary btn-sm mt-2 w-100" onclick="app.openModal('${room.id}')">ดูรายละเอียด</button>
            `;
            container.appendChild(card);
        });
    },

    filterRooms: function(filter, btnElement) {
        document.querySelectorAll('.filter-btns .btn').forEach(b => b.classList.remove('btn-active'));
        btnElement.classList.add('btn-active');
        this.renderRooms(filter);
    },

    // Modal Logic
    openModal: function(roomId) {
        const room = roomsData.find(r => r.id === roomId);
        const modal = document.getElementById('room-modal');
        const body = document.getElementById('modal-body');
        
        body.innerHTML = `
            <h2 style="color: var(--primary-color);">${room.name}</h2>
            <p><strong>ชื่อเต็ม:</strong> ${room.fullName}</p>
            <hr style="margin: 10px 0; border: 0.5px solid var(--glass-border);">
            <p><strong>เป้าหมาย/จุดเน้น:</strong> ${room.focus}</p>
            <p><strong>วิชาที่เรียนหลัก:</strong> ${room.subjects}</p>
            <p><strong>จุดแข็งของนักเรียน:</strong> ${room.strengths}</p>
            <p><strong>แนวทางอาชีพในอนาคต:</strong> ${room.careers}</p>
        `;
        
        modal.classList.remove('hidden');
    },

    closeModal: function() {
        document.getElementById('room-modal').classList.add('hidden');
    },

    // Render Quiz
    renderQuestions: function() {
        const container = document.getElementById('questions-container');
        container.innerHTML = '';
        
        questionsData.forEach((q, index) => {
            const div = document.createElement('div');
            div.className = 'question-block';
            div.innerHTML = `
                <div class="question-text">${q.text}</div>
                <div class="options">
                    ${[1, 2, 3, 4, 5].map(val => `
                        <label>
                            <input type="radio" name="q${index}" value="${val}" required>
                            <span>${val}</span>
                        </label>
                    `).join('')}
                </div>
            `;
            container.appendChild(div);
        });
    },

    // Handle Form Submit & AI Logic
    setupFormSubmit: function() {
        document.getElementById('quiz-form').addEventListener('submit', (e) => {
            e.preventDefault();
            const formData = new FormData(e.target);
            const answers = [];
            
            for (let i = 0; i < questionsData.length; i++) {
                answers.push(parseInt(formData.get(`q${i}`)));
            }
            
            this.calculateResults(answers);
        });
    },

    // AI Analysis Engine (Rule-based Algorithm)
    calculateResults: function(answers) {
        // 1. กำหนดคะแนนเริ่มต้นให้ทุกห้องเรียนเป็น 0
        let scores = { smte: 0, smt: 0, iep: 0, eis: 0, smep: 0 };
        let maxPossibleScores = { smte: 0, smt: 0, iep: 0, eis: 0, smep: 0 };

        // 2. คำนวณคะแนนตามน้ำหนัก (Weight * Answer[1-5])
        questionsData.forEach((q, index) => {
            const userAns = answers[index]; // 1 ถึง 5
            for (const [room, weight] of Object.entries(q.weights)) {
                scores[room] += weight * userAns;
                maxPossibleScores[room] += weight * 5; // คะแนนเต็มถ้าตอบ 5
            }
        });

        // 3. คิดเป็นเปอร์เซ็นต์
        let results = [];
        for (const room in scores) {
            let percentage = (scores[room] / maxPossibleScores[room]) * 100;
            // กันกรณีหาร 0
            if (isNaN(percentage)) percentage = 0;
            
            const roomInfo = roomsData.find(r => r.id === room);
            results.push({
                id: room,
                name: roomInfo.name,
                percentage: Math.round(percentage),
                data: roomInfo
            });
        }

        // 4. เรียงลำดับจากมากไปน้อย และเอา Top 3
        results.sort((a, b) => b.percentage - a.percentage);
        const top3 = results.slice(0, 3);
        
        this.renderResults(top3);
    },

    renderResults: function(top3) {
        const container = document.getElementById('results-container');
        container.innerHTML = '';

        top3.forEach((res, index) => {
            const div = document.createElement('div');
            div.className = `glass-card ${index === 0 ? 'result-top1' : ''}`;
            
            let rankBadge = index === 0 ? '🥇 แนะนำอันดับ 1' : index === 1 ? '🥈 อันดับ 2' : '🥉 อันดับ 3';
            
            div.innerHTML = `
                <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px;">
                    <span style="font-weight: bold; color: var(--accent-color);">${rankBadge}</span>
                    <span style="font-size: 1.2rem; font-weight: bold;">${res.percentage}%</span>
                </div>
                <h3>${res.name} <span style="font-size: 0.9rem; font-weight: normal; color: var(--text-secondary);">(${res.data.fullName})</span></h3>
                
                <div class="progress-container">
                    <div class="progress-bar" style="width: 0%" data-target="${res.percentage}%"></div>
                </div>
                
                <div style="margin-top: 15px; font-size: 0.95rem;">
                    <p><strong><i class="fa-solid fa-lightbulb"></i> จุดแข็งของคุณ:</strong> เหมาะกับ${res.data.strengths}</p>
                    <p><strong><i class="fa-solid fa-book"></i> วิชาที่เน้น:</strong> ${res.data.subjects}</p>
                    <p><strong><i class="fa-solid fa-briefcase"></i> เส้นทางอาชีพ:</strong> ${res.data.careers}</p>
                </div>
            `;
            container.appendChild(div);
        });

        this.navigate('result');

        // Animate Progress Bars
        setTimeout(() => {
            document.querySelectorAll('.progress-bar').forEach(bar => {
                bar.style.width = bar.getAttribute('data-target');
            });
        }, 100);
    },

    // Compare Feature
    populateCompareSelectors: function() {
        const sel1 = document.getElementById('compare-1');
        const sel2 = document.getElementById('compare-2');
        
        let optionsHtml = roomsData.map(r => `<option value="${r.id}">${r.name}</option>`).join('');
        sel1.innerHTML = optionsHtml;
        sel2.innerHTML = optionsHtml;
        sel2.selectedIndex = 1; // Default to second item
    },

    renderComparison: function() {
        const id1 = document.getElementById('compare-1').value;
        const id2 = document.getElementById('compare-2').value;
        const r1 = roomsData.find(r => r.id === id1);
        const r2 = roomsData.find(r => r.id === id2);
        
        const container = document.getElementById('compare-result');
        container.innerHTML = `
            <table class="glass-table w-100 mt-2">
                <thead>
                    <tr>
                        <th>หัวข้อ</th>
                        <th style="color:var(--primary-color)">${r1.name}</th>
                        <th style="color:var(--accent-color)">${r2.name}</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td><strong>จุดเน้น</strong></td>
                        <td>${r1.focus}</td>
                        <td>${r2.focus}</td>
                    </tr>
                    <tr>
                        <td><strong>วิชาหลัก</strong></td>
                        <td>${r1.subjects}</td>
                        <td>${r2.subjects}</td>
                    </tr>
                    <tr>
                        <td><strong>ระดับชั้น</strong></td>
                        <td>${r1.levels.map(l => l==='junior'?'ม.ต้น':'ม.ปลาย').join(', ')}</td>
                        <td>${r2.levels.map(l => l==='junior'?'ม.ต้น':'ม.ปลาย').join(', ')}</td>
                    </tr>
                </tbody>
            </table>
        `;
    },

    // Admin Simulation Logic (CRUD Local Array)
    renderAdminTable: function() {
        const tbody = document.querySelector('#admin-table tbody');
        tbody.innerHTML = '';
        roomsData.forEach((r, idx) => {
            tbody.innerHTML += `
                <tr>
                    <td>${r.id.toUpperCase()}</td>
                    <td>${r.name}</td>
                    <td><button class="btn btn-sm" style="background: #ef4444; color:white;" onclick="app.deleteRoom(${idx})">ลบ</button></td>
                </tr>
            `;
        });
        this.populateCompareSelectors(); // Update Selectors
    },

    setupAdminForm: function() {
        document.getElementById('add-room-form').addEventListener('submit', (e) => {
            e.preventDefault();
            const id = document.getElementById('admin-id').value.toLowerCase();
            const name = document.getElementById('admin-name').value;
            const desc = document.getElementById('admin-desc').value;
            const level = document.getElementById('admin-level').value;
            
            let levelsArr = level === 'both' ? ['junior', 'senior'] : [level];

            roomsData.push({
                id: id, name: name, fullName: desc, levels: levelsArr,
                focus: 'ข้อมูลชั่วคราว', subjects: 'ข้อมูลชั่วคราว', careers: 'ข้อมูลชั่วคราว', strengths: 'ข้อมูลชั่วคราว'
            });

            this.renderAdminTable();
            this.renderRooms('all'); // Re-render visual
            e.target.reset();
            alert('เพิ่มข้อมูลจำลองสำเร็จ!');
        });
    },

    deleteRoom: function(index) {
        roomsData.splice(index, 1);
        this.renderAdminTable();
        this.renderRooms('all');
    },

    // FAQ Accordion
    renderFAQ: function() {
        const container = document.getElementById('faq-container');
        faqData.forEach((item, index) => {
            const div = document.createElement('div');
            div.className = 'accordion-item glass-card';
            div.style.padding = '0';
            div.innerHTML = `
                <div class="accordion-header" onclick="this.parentElement.classList.toggle('open')">
                    <i class="fa-solid fa-chevron-down" style="font-size: 0.8rem; margin-right: 10px;"></i> ${item.q}
                </div>
                <div class="accordion-body">
                    <p style="margin-top: 10px; color: var(--text-secondary);">${item.a}</p>
                </div>
            `;
            container.appendChild(div);
        });
    },

    // Dark/Light Mode Theme Toggle
    setupThemeToggle: function() {
        const btn = document.getElementById('theme-toggle');
        const icon = btn.querySelector('i');
        
        // เช็คค่าจาก localStorage
        if(localStorage.getItem('theme') === 'dark') {
            document.body.classList.add('dark-mode');
            document.body.classList.remove('light-mode');
            icon.classList.replace('fa-moon', 'fa-sun');
        }

        btn.addEventListener('click', () => {
            document.body.classList.toggle('dark-mode');
            document.body.classList.toggle('light-mode');
            
            if (document.body.classList.contains('dark-mode')) {
                icon.classList.replace('fa-moon', 'fa-sun');
                localStorage.setItem('theme', 'dark');
            } else {
                icon.classList.replace('fa-sun', 'fa-moon');
                localStorage.setItem('theme', 'light');
            }
        });
    }
};

// Initialize App when DOM is fully loaded
document.addEventListener('DOMContentLoaded', () => app.init());
