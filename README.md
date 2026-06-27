<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="ระบบแนะนำห้องเรียนตามความถนัด โรงเรียนศรียาภัย (Sriyapai School) AI วิเคราะห์แผนการเรียนที่เหมาะสม">
    <title>ระบบแนะนำห้องเรียน | โรงเรียนศรียาภัย</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header class="glass-nav">
        <div class="nav-container">
            <div class="logo" aria-label="โลโก้โรงเรียนศรียาภัย">
                <h1>🔴🟡 ศรียาภัย</h1>
                <span>ระบบแนะนำห้องเรียน</span>
            </div>
            <nav aria-label="เมนูหลัก">
                <button class="nav-btn active" onclick="switchTab('home')">หน้าแรก</button>
                <button class="nav-btn" onclick="switchTab('classes')">ข้อมูลห้องเรียน</button>
                <button class="nav-btn" onclick="switchTab('quiz')">ทำแบบทดสอบ</button>
                <button class="nav-btn" onclick="switchTab('compare')">เปรียบเทียบ</button>
                <button class="nav-btn" onclick="switchTab('admin')">Admin</button>
                <button id="theme-toggle" class="theme-btn" aria-label="สลับโหมดมืดสว่าง">🌙</button>
            </nav>
        </div>
    </header>

    <main id="main-content">
        <section id="home" class="view-section active glass-panel">
            <div class="hero-content">
                <h2>ค้นหาห้องเรียนที่ใช่ สำหรับอนาคตของคุณ</h2>
                <p>ระบบ AI จะช่วยวิเคราะห์ความถนัดและความสนใจของคุณ เพื่อแนะนำแผนการเรียนที่เหมาะสมที่สุด ทั้งระดับ ม.ต้น และ ม.ปลาย</p>
                <div class="hero-buttons">
                    <button class="btn btn-primary" onclick="switchTab('quiz')">🚀 เริ่มทำแบบทดสอบ</button>
                    <button class="btn btn-secondary" onclick="switchTab('classes')">📚 ดูข้อมูลห้องเรียน</button>
                </div>
            </div>
            
            <div class="features-section">
                <h3>ข่าวประชาสัมพันธ์</h3>
                <ul class="news-list">
                    <li>📢 เปิดรับสมัครนักเรียนใหม่ ปีการศึกษา 2570</li>
                    <li>📢 กำหนดการสอบคัดเลือกห้องเรียนพิเศษ SMTE และ SMT</li>
                </ul>
                <h3>คำถามที่พบบ่อย (FAQ)</h3>
                <div class="accordion">
                    <button class="accordion-btn" onclick="toggleAccordion(this)">SMTE กับ SMT ต่างกันอย่างไร?</button>
                    <div class="accordion-content"><p>SMTE เน้นการทำวิจัยและนวัตกรรมเพิ่มเติมจากวิทย์-คณิตพื้นฐาน ส่วน SMT เน้นวิทย์-คณิต-เทคโนโลยีพื้นฐานที่เข้มข้น</p></div>
                </div>
            </div>
        </section>

        <section id="classes" class="view-section">
            <h2 class="section-title">ข้อมูลห้องเรียนทั้งหมด</h2>
            <div class="level-tabs">
                <button class="btn btn-outline active" onclick="filterClasses('ม.ต้น')">มัธยมศึกษาตอนต้น</button>
                <button class="btn btn-outline" onclick="filterClasses('ม.ปลาย')">มัธยมศึกษาตอนปลาย</button>
            </div>
            <div id="class-container" class="grid-container">
                </div>
        </section>

        <section id="quiz" class="view-section glass-panel">
            <h2 class="section-title">แบบทดสอบวิเคราะห์ความถนัด (15 ข้อ)</h2>
            <p class="quiz-desc">เลือกระดับที่ตรงกับตัวคุณมากที่สุด (1 = น้อยที่สุด, 5 = มากที่สุด)</p>
            <form id="quiz-form" onsubmit="submitQuiz(event)">
                <div id="quiz-container">
                    </div>
                <button type="submit" class="btn btn-primary full-width">📊 วิเคราะห์ผลลัพธ์</button>
            </form>
        </section>

        <section id="result" class="view-section glass-panel">
            <h2 class="section-title">🎯 ผลการวิเคราะห์ของคุณ</h2>
            <div id="result-dashboard">
                </div>
            <button class="btn btn-secondary" onclick="switchTab('quiz')">ทำแบบทดสอบอีกครั้ง</button>
        </section>

        <section id="compare" class="view-section glass-panel">
            <h2 class="section-title">⚖️ เปรียบเทียบห้องเรียน</h2>
            <div class="compare-selectors">
                <select id="compare-1" class="glass-input"></select>
                <select id="compare-2" class="glass-input"></select>
                <button class="btn btn-primary" onclick="compareClasses()">เปรียบเทียบ</button>
            </div>
            <div id="compare-result" class="compare-table-container"></div>
        </section>

        <section id="admin" class="view-section glass-panel">
            <h2 class="section-title">⚙️ Admin Dashboard (จำลอง)</h2>
            <div class="admin-controls">
                <input type="text" id="admin-name" placeholder="ชื่อห้องเรียน (เช่น SMT)" class="glass-input" required>
                <select id="admin-level" class="glass-input">
