<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="ระบบแนะนำห้องเรียนตามความถนัดและความสนใจ สำหรับนักเรียนโรงเรียนศรียาภัย (SMTE, SMT, IEP, EIS, SMEP) ค้นหาแผนการเรียนที่ใช่ด้วยระบบ AI">
    <meta name="keywords" content="โรงเรียนศรียาภัย, แนะนำห้องเรียน, SMTE, SMT, IEP, EIS, SMEP, แบบทดสอบค้นหาตัวเอง">
    <title>ระบบแนะนำห้องเรียน | โรงเรียนศรียาภัย</title>
    
    <style>
        /* =========================================
           1. Root Variables & Themes (รองรับ Light/Dark Mode)
           ========================================= */
        :root {
            /* Light Theme (โทนขาว-ฟ้า-น้ำเงิน) */
            --bg-color: #e0f2fe; 
            --text-color: #0f172a;
            --primary-color: #0284c7;
            --accent-color: #38bdf8;
            
            /* Glassmorphism Effects */
            --glass-bg: rgba(255, 255, 255, 0.4);
            --glass-border: rgba(255, 255, 255, 0.6);
            --glass-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.15);
            
            /* Background Blobs */
            --blob-1: #bae6fd;
            --blob-2: #7dd3fc;
            --blob-3: #e0f2fe;
        }

        [data-theme="dark"] {
            /* Dark Theme */
            --bg-color: #0f172a; 
            --text-color: #f8fafc;
            --primary-color: #38bdf8;
            --accent-color: #0ea5e9;
            
            --glass-bg: rgba(15, 23, 42, 0.6);
            --glass-border: rgba(255, 255, 255, 0.1);
            --glass-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3);
            
            --blob-1: #0369a1;
            --blob-2: #0284c7;
            --blob-3: #075985;
        }

        /* =========================================
           2. Base Styles & Typography
           ========================================= */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Sarabun', 'Prompt', system-ui, -apple-system, sans-serif;
            transition: background-color 0.4s ease, color 0.4s ease, border-color 0.4s ease;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-color);
            min-height: 100vh;
            overflow-x: hidden;
            position: relative;
        }

        /* =========================================
           3. Animated Blobs (พื้นหลังเคลื่อนไหวแบบละมุน)
           ========================================= */
        .blob {
            position: fixed;
            border-radius: 50%;
            filter: blur(80px);
            z-index: -1;
            opacity: 0.7;
            animation: float 12s infinite alternate ease-in-out;
        }

        .blob-1 { width: 400px; height: 400px; background: var(--blob-1); top: -100px; left: -100px; }
        .blob-2 { width: 350px; height: 350px; background: var(--blob-2); bottom: -50px; right: -50px; animation-delay: -2s; }
        .blob-3 { width: 300px; height: 300px; background: var(--blob-3); top: 40%; left: 40%; animation-delay: -4s; }

        @keyframes float {
            0% { transform: translate(0, 0) scale(1); }
            100% { transform: translate(40px, 60px) scale(1.15); }
        }

        /* =========================================
           4. Glassmorphism Utilities
           ========================================= */
        .glass {
            background: var(--glass-bg);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid var(--glass-border);
            box-shadow: var(--glass-shadow);
            border-radius: 16px;
        }

        .glass-btn {
            background: var(--glass-bg);
            color: var(--text-color);
            border: 1px solid var(--glass-border);
            padding: 10px 20px;
            border-radius: 8px;
            cursor: pointer;
            font-weight: bold;
            backdrop-filter: blur(5px);
            transition: all 0.3s ease;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }

        .glass-btn:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: translateY(-2px);
            box-shadow: var(--glass-shadow);
        }

        .glass-btn.primary {
            background: var(--primary-color);
            color: #fff;
            border: none;
        }
        .glass-btn.primary:hover { background: var(--accent-color); }
        .glass-btn.full-width { width: 100%; margin-top: 20px; padding: 15px; font-size: 1.1rem; }

        .glass-input {
            width: 100%;
            padding: 12px;
            border-radius: 8px;
            border: 1px solid var(--glass-border);
            background: var(--glass-bg);
            color: var(--text-color);
            outline: none;
            margin-bottom: 1rem;
            font-size: 1rem;
        }
        .glass-input:focus {
            border-color: var(--primary-color);
        }

        /* =========================================
           5. Layout & Components
           ========================================= */
        /* Navigation Bar */
        .nav-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 15px 30px;
            margin: 20px auto;
            width: 95%;
            max-width: 1200px;
            position: sticky;
            top: 20px;
            z-index: 100;
        }

        .logo-container h1 { font-size: 1.3rem; color: var(--primary-color); font-weight: 700; }
        .system-name { font-size: 0.9rem; opacity: 0.8; block-size: auto; }

        .nav-links { list-style: none; display: flex; gap: 15px; }
        .nav-btn { background: none; border: none; color: var(--text-color); cursor: pointer; font-size: 1rem; font-weight: 600; padding: 8px 12px; border-radius: 6px; }
        .nav-btn:hover { background: rgba(255,255,255,0.1); }
        .nav-btn.active { border-bottom: 2px solid var(--primary-color); color: var(--primary-color); }

        /* Main Container */
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
            min-height: 80vh;
        }

        .section { display: none; }
        .section.active { display: block; }
        .section-title { padding: 15px; margin-bottom: 25px; text-align: center; font-size: 1.8rem; }

        /* Hero Section */
        .hero { text-align: center; padding: 60px 20px; margin-bottom: 30px; }
        .hero h2 { font-size: 2.3rem; margin-bottom: 15px; color: var(--primary-color); }
        .hero p { font-size: 1.1rem; margin-bottom: 25px; line-height: 1.6; opacity: 0.9; }
        .action-buttons { display: flex; gap: 15px; justify-content: center; flex-wrap: wrap; }

        /* Grids Layout */
        .features-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; }
        .feature-card { padding: 25px; text-align: center; transition: transform 0.3s ease; }
        .feature-card h3 { margin-bottom: 10px; color: var(--primary-color); }
        .feature-card:hover { transform: translateY(-5px); }

        .cards-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 25px; }
        .class-card { padding: 25px; display: flex; flex-direction: column; justify-content: space-between; transition: all 0.3s ease; }
        .class-card:hover { transform: translateY(-5px) scale(1.01); }
        .tag { display: inline-block; padding: 4px 12px; background: var(--primary-color); color: #fff; border-radius: 20px; font-size: 0.8rem; margin-bottom: 12px; font-weight: bold; width: fit-content; }

        /* Quiz Layout */
        .quiz-container { padding: 40px; max-width: 800px; margin: 0 auto; }
        .question-item { margin-bottom: 25px; padding-bottom: 20px; border-bottom: 1px solid var(--glass-border); }
        .question-text { font-size: 1.1rem; margin-bottom: 15px; font-weight: 500; }
        .radio-group { display: flex; justify-content: space-between; max-width: 500px; margin-top: 10px; }
        .radio-group label { display: flex; flex-direction: column; align-items: center; cursor: pointer; font-size: 0.9rem; gap: 5px; }
        .radio-group input { transform: scale(1.3); accent-color: var(--primary-color); cursor: pointer; }

        /* Result Dashboard */
        .result-card { padding: 30px; margin-bottom: 25px; }
        .top-1 { border: 2px solid var(--primary-color); box-shadow: 0 0 25px rgba(2, 132, 199, 0.3); }
        .progress-bar-bg { width: 100%; height: 14px; background: rgba(0,0,0,0.1); border-radius: 10px; margin-top: 12px; overflow: hidden; }
        .progress-bar-fill { height: 100%; background: linear-gradient(90deg, var(--primary-color), var(--accent-color)); width: 0%; transition: width 1.5s ease-out; }

        /* Compare Selectors */
        .compare-selectors { display: flex; gap: 15px; margin-bottom: 20px; align-items: center; flex-wrap: wrap; }
        .compare-selectors select { flex: 1; min-width: 200px; margin-bottom: 0; }

        /* Tables (Admin & Compare) */
        table { width: 100%; border-collapse: collapse; margin-top: 15px; overflow: hidden; border-radius: 8px; }
        th, td { padding: 14px; text-align: left; border-bottom: 1px solid var(--glass-border); }
        th { background: rgba(0,0,0,0.06); font-weight: bold; color: var(--primary-color); }
        [data-theme="dark"] th { background: rgba(255,255,255,0.05); }

        /* Accordion (FAQ) */
        .faq-item { margin-top: 15px; border-bottom: 1px solid var(--glass-border); padding-bottom: 12px; }
        .faq-question { cursor: pointer; color: var(--primary-color); font-weight: 600; display: flex; align-items: center; gap: 8px; }
        .faq-answer { display: none; margin-top: 10px; font-size: 0.95rem; line-height: 1.5; padding-left: 20px; opacity: 0.9; }

        /* Modal Popup */
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.4); z-index: 1000; justify-content: center; align-items: center; backdrop-filter: blur(4px); }
        .modal-content { width: 90%; max-width: 650px; padding: 35px; position: relative; animation: slide-up 0.4s ease; max-height: 85vh; overflow-y: auto; }
        .close-btn { position: absolute; top: 15px; right: 20px; font-size: 1.8rem; cursor: pointer; opacity: 0.7; }
        .close-btn:hover { opacity: 1; }

        /* Animations */
        .fade-in { animation: fade-in 0.7s ease forwards; }
        @keyframes fade-in { from { opacity: 0; transform: translateY(15px); } to { opacity: 1; transform: translateY(0); } }
        @keyframes slide-up { from { opacity: 0; transform: translateY(40px); } to { opacity: 1; transform: translateY(0); } }

        /* =========================================
           6. Responsive Web Design (Mobile/Tablet)
           ========================================= */
        @media (max-width: 768px) {
            .nav-bar { flex-direction: column; gap: 15px; text-align: center; padding: 15px; }
            .nav-links { flex-wrap: wrap; justify-content: center; gap: 5px; }
            .nav-btn { font-size: 0.9rem; padding: 6px 10px; }
            .hero h2 { font-size: 1.8rem; }
            .hero p { font-size: 1rem; }
            .quiz-container { padding: 20px; }
            .radio-group { width: 100%; }
            th, td { padding: 10px; font-size: 0.9rem; }
        }
    </style>
</head>
<body>

    <div class="blob blob-1"></div>
    <div class="blob blob-2"></div>
    <div class="blob blob-3"></div>

    <header class="glass nav-bar">
        <div class="logo-container">
            <h1 class="school-name">โรงเรียนศรียาภัย</h1>
            <span class="system-name">ระบบแนะนำห้องเรียนตามความถนัด</span>
        </div>
        <nav aria-label="Main Navigation">
            <ul class="nav-links">
                <li><button class="nav-btn active" data-target="home" aria-label="หน้าแรก">หน้าแรก</button></li>
                <li><button class="nav-btn" data-target="classes" aria-label="ข้อมูลห้องเรียน">ข้อมูลห้องเรียน</button></li>
                <li><button class="nav-btn" data-target="compare" aria-label="เปรียบเทียบห้องเรียน">เปรียบเทียบ</button></li>
                <li><button class="nav-btn" data-target="admin" aria-label="แผงควบคุมแอดมิน">Admin Dashboard</button></li>
            </ul>
        </nav>
        <button id="theme-toggle" class="glass-btn" aria-label="เปลี่ยนโหมดธีม">🌙 Dark Mode</button>
    </header>

    <main class="container">
        
        <section id="home" class="section active">
            <div class="hero glass fade-in">
                <h2>ค้นหา "ห้องเรียน" ที่ใช่ สำหรับอนาคตของคุณ</h2>
                <p>ระบบ AI วิเคราะห์อัจฉริยะประมวลผลจากความชอบ ทักษะ และเป้าหมายของนักเรียน เพื่อแนะนำแผนการเรียนที่เหมาะสมที่สุดในโรงเรียนศรียาภัย</p>
                <div class="action-buttons">
                    <button class="glass-btn primary" onclick="navigate('quiz')">🚀 เริ่มต้นทำแบบทดสอบ</button>
                    <button class="glass-btn" onclick="navigate('classes')">📚 ดูข้อมูลหลักสูตรห้องเรียน</button>
                </div>
            </div>
            
            <div class="features-grid fade-in">
                <div class="feature-card glass">
                    <h3>💡 AI Rule-Based วิเคราะห์ตรงจุด</h3>
                    <p>ประเมินผลลัพธ์ผ่านตัวชี้วัดทักษะ 15 ข้ออย่างมีระบบ</p>
                </div>
                <div class="feature-card glass">
                    <h3>📊 Dashboard ประมวลผลเลเยอร์</h3>
                    <p>แสดงผลความเหมาะสม Top 3 ลำดับ พร้อมกราฟเปอร์เซ็นต์ความเหมาะสม</p>
                </div>
                <div class="feature-card glass">
                    <h3>🎯 ค้นพบเส้นทางอนาคต</h3>
                    <p>เจาะลึกรายวิชาที่ต้องเจอ แนะนำแนวทางศึกษาต่อ และสายอาชีพยืดหยุ่น</p>
                </div>
            </div>

            <div class="faq-container glass fade-in" style="margin-top: 2rem; padding: 25px;">
                <h3>❓ คำถามที่พบบ่อย (FAQ)</h3>
                <div id="faq-content"></div>
            </div>
        </section>

        <section id="classes" class="section">
            <h2 class="section-title glass">ข้อมูลห้องเรียนและหลักสูตรวิชา</h2>
            <div class="controls glass" style="margin-bottom: 1.5rem; padding: 15px;">
                <input type="text" id="search-class" placeholder="🔍 ค้นหาชื่อห้องเรียน แผนการเรียน หรือสายอาชีพที่เกี่ยวข้อง..." class="glass-input" style="margin-bottom:0;">
            </div>
            <div id="class-cards-container" class="cards-grid fade-in">
                </div>
        </section>

        <section id="quiz" class="section">
            <div class="quiz-container glass fade-in">
                <h2 style="text-align: center; color: var(--primary-color);">แบบประเมินความถนัดและความสนใจ (15 ข้อ)</h2>
                <p style="text-align: center; margin-bottom: 25px; opacity: 0.8;">โปรดเลือกคะแนนที่ตรงกับระดับความชอบหรือตัวตนของคุณมากที่สุด (1: น้อยที่สุด -> 5: มากที่สุด)</p>
                <form id="quiz-form">
                    <div id="questions-container">
                        </div>
                    <button type="submit" class="glass-btn primary full-width">🔮 ส่งคำตอบเพื่อวิเคราะห์ผลลัพธ์ด้วย AI</button>
                </form>
            </div>
        </section>

        <section id="result" class="section">
            <h2 class="section-title glass">📊 ผลการวิเคราะห์อันดับแผนการเรียนที่เหมาะสม</h2>
            <div id="result-content" class="fade-in">
                </div>
            <div style="text-align: center; margin-top: 2.5rem; display: flex; justify-content: center; gap: 15px;">
                <button class="glass-btn" onclick="navigate('home')">🏠 กลับหน้าแรก</button>
                <button class="glass-btn primary" onclick="navigate('quiz')">🔄 ทำแบบทดสอบอีกครั้ง</button>
            </div>
        </section>

        <section id="compare" class="section">
            <div class="compare-container glass fade-in" style="padding: 30px;">
                <h2>🔄 เปรียบเทียบแผนการเรียนแบบเชิงลึก</h2>
                <p style="margin-bottom: 20px; opacity:0.8;">เลือกห้องเรียนที่ต้องการจับคู่เพื่อดูข้อแตกต่างในวิชาเน้นและจุดเด่น</p>
                <div class="compare-selectors">
                    <select id="compare-1" class="glass-input"></select>
                    <span style="font-weight: bold;">VS</span>
                    <select id="compare-2" class="glass-input"></select>
                    <button class="glass-btn primary" onclick="compareClasses()">เริ่มเปรียบเทียบ</button>
                </div>
                <div id="compare-result" style="margin-top: 2rem;"></div>
            </div>
        </section>

        <section id="admin" class="section">
            <div class="admin-container glass fade-in" style="padding: 30px;">
                <h2>⚙️ ระบบหลังบ้านจัดการข้อมูลห้องเรียน (Admin Simulation)</h2>
                <p style="margin-bottom: 15px; opacity: 0.8;">ทดสอบฟีเจอร์เพิ่ม/ลบ ข้อมูลจำลองในอาเรย์ชั่วคราว (CRUD Simulation)</p>
                <div id="admin-table-container" style="overflow-x: auto;">
                    </div>
                <button class="glass-btn primary" onclick="addMockClass()" style="margin-top: 1.5rem;">➕ เพิ่มแผนการเรียนจำลอง (Test Create)</button>
            </div>
        </section>

    </main>

    <div id="class-modal" class="modal" aria-hidden="true">
        <div class="modal-content glass">
            <span class="close-btn" onclick="closeModal()" tabindex="0" role="button" aria-label="ปิดหน้าต่าง">&times;</span>
            <div id="modal-body"></div>
        </div>
    </div>

    <script>
        // ==========================================
        // 1. คลังข้อมูลหลักสูตรโรงเรียนศรียาภัย (Data Store)
        // ==========================================
        let classData = [
            { 
                id: 'smte', 
                name: 'SMTE (Science Mathematics and Technology Education)', 
                level: 'มัธยมศึกษาตอนต้น / ตอนปลาย', 
                focus: 'เน้นวิทยาศาสตร์, คณิตศาสตร์, เทคโนโลยี และทักษะการวิจัย/นวัตกรรมเข้มข้น', 
                subjects: 'ฟิสิกส์ขั้นสูง, เคมีขั้นสูง, ชีววิทยา, วิทยาการคำนวณ, โครงงานวิทยาศาสตร์ศึกษา', 
                careers: 'แพทย์, ทันตแพทย์, วิศวกรวิจัย, นักวิเคราะห์ข้อมูล (Data Analyst), นักวิจัยนวัตกรรม', 
                desc: 'โครงการห้องเรียนพิเศษวิทยาศาสตร์ คณิตศาสตร์ เทคโนโลยี และสิ่งแวดล้อม มุ่งเน้นการบ่มเพาะนักวิจัยและนวัตกรชั้นนำ รองรับการแข่งขันระดับชาติ' 
            },
            { 
                id: 'smt', 
                name: 'SMT (Science Mathematics and Technology Program)', 
                level: 'มักยมศึกษาตอนต้น / ตอนปลาย', 
                focus: 'เน้นพื้นฐานวิทย์-คณิต พัฒนากระบวนการคิดวิเคราะห์เชิงตรรกะ', 
                subjects: 'คณิตศาสตร์เพิ่มเติม, วิทยาศาสตร์ประยุกต์, เทคโนโลยีสารสนเทศเพื่อการศึกษา', 
                careers: 'วิศวกรซอฟต์แวร์, โปรแกรมเมอร์, ครู/อาจารย์สายวิทยาศาสตร์, นักวิทยาศาสตร์ปฏิบัติการ', 
                desc: 'หลักสูตรที่เน้นความรู้พื้นฐานด้านวิทยาศาสตร์และคณิตศาสตร์อย่างเป็นระบบ ฝึกการแก้โจทย์ปัญหาและการคิดอย่างมีวิจารณญาณ' 
            },
            { 
                id: 'iep', 
                name: 'IEP (Intensive English Program)', 
                level: 'มัธยมศึกษาตอนต้น / ตอนปลาย', 
                focus: 'เรียนวิชาแกนหลักเป็นภาษาอังกฤษเพื่อการสื่อสารและการทำงานระดับสากล', 
                subjects: 'Intensive English, Mathematics in English, Science for Communication', 
                careers: 'ล่าม/นักแปล, นักการทูต, พนักงานต้อนรับสายการบิน, ผู้เชี่ยวชาญในบริษัทข้ามชาติ', 
                desc: 'มุ่งเน้นการพัฒนาทักษะภาษาอังกฤษเพื่อความเชี่ยวชาญขั้นสูง เรียนรู้กับเจ้าของภาษาโดยตรงในวิชาหลัก มุ่งสู่สากล' 
            },
            { 
                id: 'eis', 
                name: 'EIS (English for Integrated Studies)', 
                level: 'มัธยมศึกษาตอนต้น', 
                focus: 'ใช้ภาษาอังกฤษเป็นสื่อกลางการสอนควบคู่กับวิชาหลักแบบบูรณาการ', 
                subjects: 'English Listening & Speaking, วิทย์-คณิตในรูปแบบภาษาอังกฤษพื้นฐาน, สังคมศึกษาบูรณาการ', 
                careers: 'นักธุรกิจรุ่นใหม่, นักแปลภาษา, นักการตลาดข้ามชาติ, ครูผู้สอนสองภาษา', 
                desc: 'การผสมผสานตำราและการสอนภาษาอังกฤษเข้ากับวิชาวิทยาศาสตร์และคณิตศาสตร์พื้นฐาน ช่วยให้เรียนรู้ศัพท์เทคนิคอย่างเป็นธรรมชาติ' 
            },
            { 
                id: 'smep', 
                name: 'SMEP (Science Mathematics and English Program)', 
                level: 'มัธยมศึกษาตอนปลาย', 
                focus: 'ผสมผสานหลักสูตร STEM (วิทย์-คณิต) ควบคู่ทักษะภาษาอังกฤษระดับสูง', 
                subjects: 'Advanced STEM Project, วิทยาศาสตร์ภาคภาษาอังกฤษ, การเขียนโปรแกรมคอมพิวเตอร์, Academic English', 
                careers: 'แพทย์หลักสูตรอินเตอร์, วิศวกรระบบสากล, โปรแกรมเมอร์, นักธุรกิจระหว่างประเทศ', 
                desc: 'แผนการเรียนระดับมัธยมปลายที่ตอบโจทย์ยุคดิจิทัลอย่างสมบูรณ์แบบ ได้ทั้งความเข้มข้นทางวิทย์-คณิต และความคล่องแคล่วทางภาษาอังกฤษ' 
            }
        ];

        // ชุดคำถามประเมินผล 15 ข้อ พร้อมค่าน้ำหนัก (Weighting Core) เพื่อคำนวณตามอัลกอริทึม
        const quizQuestions = [
            { text: "1. ฉันชื่นชอบและมีความสุขกับการได้ทดลองวิทยาศาสตร์และหาคำตอบตามสมมติฐาน", weights: { smte: 5, smep: 4, smt: 4, eis: 1, iep: 0 } },
            { text: "2. การคิดคำนวณ แก้โจทย์คณิตศาสตร์ที่ซับซ้อน หรือการคิดเชิงตรรกะทำให้ฉันรู้สึกท้าทาย", weights: { smte: 4, smep: 3, smt: 5, eis: 1, iep: 0 } },
            { text: "3. ฉันมีความสุขกับการใช้ภาษาอังกฤษ ไม่ว่าจะเป็นการฟัง พูด อ่าน หรือเขียนในชีวิตประจำวัน", weights: { iep: 5, eis: 4, smep: 4, smt: 1, smte: 1 } },
            { text: "4. ฉันสนใจนวัตกรรมทางไอที เทคโนโลยีใหม่ๆ หรือการเขียนโค้ดคอมพิวเตอร์", weights: { smte: 5, smt: 4, smep: 3, eis: 1, iep: 0 } },
            { text: "5. ฉันชอบกระบวนการทำโครงงาน การสืบค้นข้อมูลเชิงลึก และทำวิจัยเรื่องที่ตนเองสนใจ", weights: { smte: 5, smep: 4, smt: 2, eis: 1, iep: 0 } },
            { text: "6. ฉันมีเป้าหมายที่อยากไปศึกษาต่อต่างประเทศหรือทำงานในบริษัทข้ามชาติระดับสากล", weights: { iep: 5, smep: 5, eis: 4, smt: 1, smte: 2 } },
            { text: "7. ฉันชื่นชอบวิชาที่เน้นการทำความเข้าใจบริบทสังคม ภาษา วัฒนธรรมมากกว่าการท่องจำกฎเกณฑ์", weights: { eis: 4, iep: 3, smep: 1, smt: 0, smte: 0 } },
            { text: "8. ฉันชอบรับชมสื่อต่างประเทศ ฟังเพลงสากล เพื่อฝึกฝนภาษาอังกฤษด้วยตนเองเป็นประจำ", weights: { iep: 5, eis: 5, smep: 4, smt: 0, smte: 0 } },
            { text: "9. ฉันตื่นเต้นและสนใจติดตามข่าวสารความก้าวหน้าด้านการแพทย์ พันธุวิศวกรรม หรือสิ่งประดิษฐ์ไฮเทค", weights: { smte: 5, smep: 5, smt: 3, eis: 0, iep: 0 } },
            { text: "10. อาชีพในฝันของฉันคือกลุ่มบุคลากรทางการแพทย์ (หมอ, เภสัช) หรือวิศวกรเฉพาะทาง", weights: { smte: 5, smep: 5, smt: 4, eis: 0, iep: 0 } },
            { text: "11. อาชีพในฝันของฉันคือ นักการทูต, ล่าม, ไกด์นำเที่ยว หรือตำแหน่งงานที่ติดต่อประสานงานระหว่างประเทศ", weights: { iep: 5, eis: 4, smt: 0, smte: 0, smep: 0 } },
            { text: "12. เวลาเจอปัญหา ฉันมักวิเคราะห์ปัญหาอย่างรอบคอบและคิดแก้ไขด้วยเหตุและผลอย่างเป็นระบบ", weights: { smte: 4, smt: 5, smep: 4, eis: 1, iep: 1 } },
            { text: "13. ฉันรู้สึกว่าการนำทักษะภาษาอังกฤษไปใช้ร่วมกับการเรียนวิชาอื่น (เช่น วิทย์, คณิต) เป็นเรื่องน่าสนุก", weights: { eis: 5, smep: 5, iep: 4, smte: 1, smt: 1 } },
            { text: "14. ฉันเรียนรู้ได้ดีที่สุดเมื่อได้ลงมือปฏิบัติจริง (Hands-on) มากกว่าการนั่งฟังทฤษฎีในห้องเรียน", weights: { smte: 5, smt: 4, smep: 3, eis: 2, iep: 1 } },
            { text: "15. ฉันต้องการพัฒนาตนเองให้รอบด้าน ทั้งทักษะการคำนวณเชิงวิชาการควบคู่กับการพูดสื่อสารสากล", weights: { smep: 5, smte: 2, eis: 2, iep: 1, smt: 1 } }
        ];

        const faqData = [
            { q: "ระบบ AI วิเคราะห์ตัวนี้ประมวลผลอย่างไร?", a: "ระบบทำงานในรูปแบบ Rule-based AI Algorithm โดยคำนวณและกระจายน้ำหนักคะแนนจากคำตอบทั้ง 15 ข้อ ลงไปยังฐานข้อมูลหลักสูตรจริงของโรงเรียนศรียาภัย เพื่อหาค่าร้อยละ (%) ความแมทช์ที่แม่นยำที่สุด" },
            { q: "หากผลลัพธ์ออกมาไม่ตรงกับสายการเรียนที่อยากเรียนทำอย่างไร?", a: "ผลลัพธ์จากระบบเป็นเพียงตัวช่วยแนะแนวทางเบื้องต้นประกอบการตัดสินใจ นักเรียนสามารถเลือกสมัครเรียนตามความมุ่งมั่นและเป้าหมายแท้จริงของตนเองได้" },
            { q: "ข้อมูลห้องเรียนอัปเดตเป็นปัจจุบันหรือไม่?", a: "เป็นข้อมูลโครงสร้างวิชาหลักเบื้องต้นของแต่ละแผนการเรียน (SMTE, SMT, IEP, EIS, SMEP) หากมีการเปลี่ยนแปลงระเบียบการรับสมัคร แนะนำให้ติดต่อฝ่ายวิชาการโรงเรียนศรียาภัยเพิ่มเติม" }
        ];

        // ==========================================
        // 2. SPA Navigation & Theme Switcher (UI ลอจิก)
        // ==========================================
        function navigate(targetId) {
            document.querySelectorAll('.section').forEach(sec => sec.classList.remove('active'));
            document.querySelectorAll('.nav-btn').forEach(btn => btn.classList.remove('active'));
            
            document.getElementById(targetId).classList.add('active');
            const activeBtn = document.querySelector(`.nav-btn[data-target="${targetId}"]`);
            if(activeBtn) activeBtn.classList.add('active');
            window.scrollTo(0, 0);
        }

        document.querySelectorAll('.nav-btn').forEach(btn => {
            btn.addEventListener('click', (e) => navigate(e.target.dataset.target));
        });

        // สลับธีม Dark / Light Mode
        const themeToggleBtn = document.getElementById('theme-toggle');
        themeToggleBtn.addEventListener('click', () => {
            const isDark = document.body.getAttribute('data-theme') === 'dark';
            document.body.setAttribute('data-theme', isDark ? 'light' : 'dark');
            themeToggleBtn.innerHTML = isDark ? '🌙 Dark Mode' : '☀️ Light Mode';
        });

        // ==========================================
        // 3. Render Engine (แสดงผลข้อมูลทางหน้าจอ)
        // ==========================================
        document.addEventListener('DOMContentLoaded', () => {
            renderClasses();
            renderQuiz();
            renderFAQ();
            populateCompareSelects();
            renderAdminTable();
        });

        // แสดงการ์ดห้องเรียนพร้อมระบบกรองค้นหา (Search)
        function renderClasses(filter = "") {
            const container = document.getElementById('class-cards-container');
            container.innerHTML = '';
            
            const filteredData = classData.filter(c => 
                c.name.toLowerCase().includes(filter.toLowerCase()) || 
                c.focus.toLowerCase().includes(filter.toLowerCase()) ||
                c.careers.toLowerCase().includes(filter.toLowerCase())
            );

            if(filteredData.length === 0) {
                container.innerHTML = `<p style='text-align:center; grid-column: 1/-1; padding: 20px;'>❌ ไม่พบข้อมูลห้องเรียนที่ตรงกับเงื่อนไขการค้นหา</p>`;
                return;
            }

            filteredData.forEach(c => {
                const card = document.createElement('div');
                card.className = 'class-card glass fade-in';
                card.innerHTML = `
                    <div>
                        <span class="tag">${c.level}</span>
                        <h3 style="color: var(--primary-color); margin-bottom: 12px;">${c.id.toUpperCase()}</h3>
                        <p style="margin-bottom: 8px;"><strong>จุดเน้น:</strong> ${c.focus}</p>
                        <p style="font-size: 0.9rem; opacity: 0.85;"><strong>💼 อาชีพแนะนำ:</strong> ${c.careers}</p>
                    </div>
                    <button class="glass-btn primary" style="margin-top: 20px; width: 100%;" onclick="openModal('${c.id}')">🔎 ดูเจาะลึกวิชาเรียน</button>
                `;
                container.appendChild(card);
            });
        }

        document.getElementById('search-class').addEventListener('input', (e) => renderClasses(e.target.value));

        // เปิด Modal แสดงข้อมูลเชิงลึก
        function openModal(classId) {
            const cls = classData.find(c => c.id === classId);
            if(!cls) return;
            const modalBody = document.getElementById('modal-body');
            modalBody.innerHTML = `
                <span class="tag" style="background: var(--accent-color); color:#000;">${cls.level}</span>
                <h2 style="color: var(--primary-color); margin: 10px 0 15px 0; font-size: 1.6rem;">${cls.name}</h2>
                <p style="margin: 15px 0; line-height: 1.6;"><strong>คำอธิบายหลักสูตร:</strong> ${cls.desc}</p>
                <p style="margin-bottom: 10px; padding: 10px; background: rgba(0,0,0,0.03); border-radius:8px;"><strong>📚 รายวิชาเด่นที่เน้นหนัก:</strong> ${cls.subjects}</p>
                <p style="margin-bottom: 10px;"><strong>🎯 แนวทางสายอาชีพในอนาคต:</strong> ${cls.careers}</p>
            `;
            const modal = document.getElementById('class-modal');
            modal.style.display = 'flex';
            modal.setAttribute('aria-hidden', 'false');
        }

        function closeModal() {
            const modal = document.getElementById('class-modal');
            modal.style.display = 'none';
            modal.setAttribute('aria-hidden', 'true');
        }

        window.onclick = function(event) {
            const modal = document.getElementById('class-modal');
            if (event.target === modal) closeModal();
        }

        // Render ส่วนของ FAQ Accordion
        function renderFAQ() {
            const container = document.getElementById('faq-content');
            faqData.forEach((item, index) => {
                container.innerHTML += `
                    <div class="faq-item">
                        <div class="faq-question" onclick="toggleFAQ(${index})">🔹 ${item.q}</div>
                        <div id="faq-ans-${index}" class="faq-answer">${item.a}</div>
                    </div>
                `;
            });
        }
        
        window.toggleFAQ = function(index) {
            const ans = document.getElementById(`faq-ans-${index}`);
            const isHidden = window.getComputedStyle(ans).display === 'none';
            ans.style.display = isHidden ? 'block' : 'none';
        }

        // ==========================================
        // 4. AI Analysis Engine (Rule-based Weighting)
        // ==========================================
        function renderQuiz() {
            const container = document.getElementById('questions-container');
            container.innerHTML = '';
            quizQuestions.forEach((q, index) => {
                container.innerHTML += `
                    <div class="question-item">
                        <p class="question-text">${q.text}</p>
                        <div class="radio-group">
                            <label><input type="radio" name="q${index}" value="1" required> 1 (น้อยสุด)</label>
                            <label><input type="radio" name="q${index}" value="2"> 2</label>
                            <label><input type="radio" name="q${index}" value="3"> 3 (ปานกลาง)</label>
                            <label><input type="radio" name="q${index}" value="4"> 4</label>
                            <label><input type="radio" name="q${index}" value="5"> 5 (มากสุด)</label>
                        </div>
                    </div>
                `;
            });
        }

        document.getElementById('quiz-form').addEventListener('submit', function(e) {
            e.preventDefault();
            
            // 1. คำนวณหาคะแนนเต็มสูงสุดที่เป็นไปได้ของแต่ละห้องเรียน (ตามฐานข้อสอบที่มีค่าน้ำหนัก)
            let maxScores = { smte: 0, smt: 0, iep: 0, eis: 0, smep: 0 };
            quizQuestions.forEach(q => {
                for(let key in q.weights) { 
                    maxScores[key] += (q.weights[key] * 5); // คะแนนตอบข้อละ 5 คือสูงสุด
                }
            });

            // 2. รวบรวมคะแนนดิบที่ผู้ใช้งานกดยืนยันเลือกจริง
            let userScores = { smte: 0, smt: 0, iep: 0, eis: 0, smep: 0 };
            for(let i = 0; i < quizQuestions.length; i++) {
                const selected = document.querySelector(`input[name="q${i}"]:checked`);
                if(!selected) return alert('โปรดตอบแบบประเมินให้ครบถ้วนทุกข้อครับเพื่อความแม่นยำ!');
                
                const scoreValue = parseInt(selected.value);
                const weightTable = quizQuestions[i].weights;
                for(let key in weightTable) { 
                    userScores[key] += (weightTable[key] * scoreValue); 
                }
            }

            // 3. แปลงผลลัพธ์เป็นเปอร์เซ็นต์ร้อยละ (%) แล้วจัดลำดับ Top 3
            let finalRankings = [];
            for(let key in userScores) {
                let percentage = ((userScores[key] / maxScores[key]) * 100).toFixed(1);
                finalRankings.push({ id: key, percent: parseFloat(percentage) });
            }
            finalRankings.sort((a, b) => b.percent - a.percent);

            renderRecommendationDashboard(finalRankings);
            navigate('result');
        });

        // แสดงแดชบอร์ดลัพธ์การแนะนำผ่าน Dynamic Element
        function renderRecommendationDashboard(rankings) {
            const container = document.getElementById('result-content');
            container.innerHTML = '';
            
            // ดึงเฉพาะ 3 อันดับแรกที่มีเปอร์เซ็นต์สูงสุดออกมาแสดงผล
            for(let i = 0; i < 3; i++) {
                const cls = classData.find(c => c.id === rankings[i].id);
                const isWinner = i === 0; // อันดับ 1
                
                container.innerHTML += `
                    <div class="glass result-card ${isWinner ? 'top-1' : ''}">
                        <div style="display:flex; justify-content:space-between; align-items:center; flex-wrap:wrap;">
                            <h3 style="font-size:1.3rem;">🎖️ อันดับที่ ${i+1}: ${cls.name}</h3>
                            <span class="tag" style="margin-bottom:0;">ความเหมาะสม ${rankings[i].percent}%</span>
                        </div>
                        
                        <div class="progress-bar-bg">
                            <div class="progress-bar-fill" style="width: ${rankings[i].percent}%"></div>
                        </div>
                        
                        ${isWinner ? `
                            <div style="margin-top: 20px; padding-top: 15px; border-top: 1px dashed var(--glass-border); font-size: 0.95rem;">
                                <p style="margin-bottom:6px;"><strong>💡 เหตุผลที่แนะนำ:</strong> รูปแบบความชอบของคุณสอดคล้องอย่างมีนัยสำคัญกับแนวทาง: <span style="color:var(--primary-color); font-weight:bold;">${cls.focus}</span></p>
                                <p style="margin-bottom:6px;"><strong>📚 รายวิชาหลักที่ต้องพบเจอ:</strong> ${cls.subjects}</p>
                                <p><strong>🎯 แนะนำเป้าหมายสายอาชีพ:</strong> ${cls.careers}</p>
                            </div>
                        ` : ''}
                    </div>
                `;
            }
        }

        // ==========================================
        // 5. ระบบเปรียบเทียบแผนการเรียน (Compare Modules)
        // ==========================================
        function populateCompareSelects() {
            const s1 = document.getElementById('compare-1');
            const s2 = document.getElementById('compare-2');
            s1.innerHTML = ''; s2.innerHTML = '';
            
            classData.forEach(c => {
                s1.innerHTML += `<option value="${c.id}">${c.id.toUpperCase()} Program</option>`;
                s2.innerHTML += `<option value="${c.id}">${c.id.toUpperCase()} Program</option>`;
            });
            if(s2.options.length > 1) s2.selectedIndex = 1; // ให้ตัวที่สองเลือกรายการลำดับถัดไปเป็นค่าเริ่มต้น
        }

        window.compareClasses = function() {
            const id1 = document.getElementById('compare-1').value;
            const id2 = document.getElementById('compare-2').value;
            const c1 = classData.find(c => c.id === id1);
            const c2 = classData.find(c => c.id === id2);
            
            document.getElementById('compare-result').innerHTML = `
                <table class="glass fade-in">
                    <thead>
                        <tr>
                            <th style="width:20%">เกณฑ์เทียบเคียง</th>
                            <th style="width:40%; color: var(--primary-color);">${c1.id.toUpperCase()} Section</th>
                            <th style="width:40%; color: var(--accent-color);">${c2.id.toUpperCase()} Section</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr><td><strong>ระดับช่วงชั้นที่เปิด</strong></td><td>${c1.level}</td><td>${c2.level}</td></tr>
                        <tr><td><strong>จุดเด่นกระบวนการ</strong></td><td>${c1.focus}</td><td>${c2.focus}</td></tr>
                        <tr><td><strong>โครงสร้างวิชาเด่น</strong></td><td>${c1.subjects}</td><td>${c2.subjects}</td></tr>
                        <tr><td><strong>ความพร้อมสายอาชีพ</strong></td><td>${c1.careers}</td><td>${c2.careers}</td></tr>
                    </tbody>
                </table>
            `;
        }

        // ==========================================
        // 6. แผงควบคุมระบบจำลองหลังบ้าน (Mock CRUD Data Admin)
        // ==========================================
        function renderAdminTable() {
            let tableHTML = `
                <table>
                    <thead>
                        <tr>
                            <th>รหัสไอดี</th>
                            <th>โครงสร้างชื่อห้องเรียน</th>
                            <th>การดำเนินการหลังบ้าน</th>
                        </tr>
                    </thead>
                    <tbody>`;
            
            classData.forEach((c, index) => {
                tableHTML += `
                    <tr>
                        <td><strong>${c.id.toUpperCase()}</strong></td>
                        <td>${c.name}</td>
                        <td>
                            <button class="glass-btn" style="padding: 5px 12px; color: #ef4444; font-size:0.85rem;" onclick="deleteClass(${index})">🗑️ ลบข้อมูล</button>
                        </td>
                    </tr>`;
            });
            tableHTML += `</tbody></table>`;
            document.getElementById('admin-table-container').innerHTML = tableHTML;
        }

        window.deleteClass = function(index) {
            if(confirm(`⚠️ คุณแน่ใจใช่หรือไม่ที่จะลบรายชื่อห้องเรียน "${classData[index].id.toUpperCase()}" ออกจากตารางระบบจำลองชั่วคราว?`)) {
                classData.splice(index, 1);
                renderAdminTable();
                renderClasses();          // อัปเดตการแสดงผลหน้าแสดงข้อมูลห้องเรียนทันที
                populateCompareSelects(); // อัปเดตตัวเลือกเปรียบเทียบทันที
            }
        }

        window.addMockClass = function() {
            const timestampID = 'mock' + Date.now().toString().slice(-3);
            const newMockPayload = {
                id: timestampID,
                name: `ห้องเรียนเสริมทักษะอนาคตพิเศษ (${timestampID.toUpperCase()})`,
                level: 'มัธยมศึกษาตอนปลาย',
                focus: 'เน้นศิลปะประยุกต์ ดนตรีสากล และนวัตกรรมการสร้างคอนเทนต์สื่อดิจิทัล',
                subjects: 'Creative Arts, Digital Content Media, Music Production',
                careers: 'Creative Director, Content Creator, ศิลปินนักดนตรี, ผู้ผลิตสื่อบันเทิง',
                desc: 'หลักสูตรรองรับเทรนด์เศรษฐกิจสร้างสรรค์ (Creative Economy) เน้นพัฒนาศักยภาพเฉพาะบุคคลขั้นสูงสุด'
            };
            
            classData.push(newMockPayload);
            renderAdminTable();
            renderClasses();
            populateCompareSelects();
            alert('➕ เพิ่มข้อมูลหลักสูตรจำลองสำเร็จ! (ข้อมูลนี้ถูกเก็บในหน่วยความจำ RAM ชั่วคราว จะคืนค่าเดิมเมื่อคุณกด Refresh หน้าเว็บ)');
        }
    </script>
</body>
</html>
