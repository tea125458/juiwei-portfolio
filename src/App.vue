<script setup>
import { ref, onMounted, onUnmounted } from 'vue';

const currentImageIndex = ref(0);
const projectImages = [
  '/demo-1.png',
  '/demo-2.png',
  '/demo-3.png',
  '/demo-4.png',
  '/demo-5.png',
  '/demo-6.png'
];

const nextImage = () => {
  currentImageIndex.value = (currentImageIndex.value + 1) % projectImages.length;
};

const prevImage = () => {
  currentImageIndex.value = (currentImageIndex.value - 1 + projectImages.length) % projectImages.length;
};

let autoPlayTimer;
let observer;

onMounted(() => {
  autoPlayTimer = setInterval(nextImage, 4000);
  
  observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        entry.target.classList.add('visible');
      }
    });
  }, { threshold: 0.15, rootMargin: '0px 0px -50px 0px' });
  
  // Need to wait for DOM to be ready
  setTimeout(() => {
    document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
  }, 100);
});

onUnmounted(() => {
  clearInterval(autoPlayTimer);
  if (observer) observer.disconnect();
});

const printResume = () => {
  window.print();
};
</script>

<template>
  <div class="app-wrapper">
    <nav class="navbar print-hide">
      <div class="container nav-content">
        <a href="#" class="logo">JUI WEI.</a>
        <ul class="nav-links">
          <li><a href="#about">關於我</a></li>
          <li><a href="#skills">技術能力</a></li>
          <li><a href="#projects">實戰專案</a></li>
          <li><a href="#experience">經歷與學歷</a></li>
          <li><a href="#contact">聯絡資訊</a></li>
        </ul>
      </div>
    </nav>

    <main>
      <section id="hero" class="hero section">
        <div class="container hero-content">
          <p class="hero-subtitle print-hide">Hello, I'm</p>
          <h1 class="hero-title">徐蕊薇</h1>
          <h2 class="hero-role">Java 全端工程師</h2>
          <p class="hero-description print-hide">
            熱愛以邏輯解決問題，專注於打造直覺且穩定的系統架構。
          </p>
          <div class="hero-actions print-hide">
            <a href="#projects" class="btn btn-primary">觀看作品</a>
            <button @click="printResume" class="btn btn-outline">下載履歷 (PDF)</button>
            <a href="#contact" class="btn btn-outline">聯絡我</a>
          </div>
        </div>
      </section>

      <section id="about" class="about section reveal">
        <div class="container">
          <h2 class="section-title">關於我</h2>
          <div class="about-content">
            <div class="about-text">
              <p>我是徐蕊薇，一名具備跨域溝通思維的 Java 軟體工程師。</p>
              <p>畢業於中興大學中文系，過去在醫療與空軍後勤單位的行政經驗，培養了我對流程管理的極度嚴謹。因為深刻體會到「好系統對提升工作效率的重要性」，我決定主動跨出舒適圈，投入程式開發領域。</p>
              <p>為了將需求分析能力轉化為實際技術，我完成了高強度的軟體工程師培訓，並具備以下核心優勢：</p>
              <p><strong>🛠️ 扎實的全端開發實力：</strong><br>熟練運用 Java、Spring Boot 進行 RESTful API 開發與 SQL Server 資料庫架構設計，並能結合 Vue.js 獨立完成流暢的前端互動介面。</p>
              <p><strong>💡 具備使用者思維的需求轉譯：</strong><br>文學訓練與過往的跨部門協作經驗，讓我能精準理解複雜的商業邏輯。擅長從使用者痛點出發，將抽象的業務需求轉化為清晰的程式邏輯。</p>
              <p><strong>🚀 追求高紀律與系統穩定度：</strong><br>曾在高準確度要求的環境中工作，讓我更重視程式碼的品質與防呆機制，致力於為團隊打造穩定、高效的資訊系統。</p>
            </div>
            <div class="about-image">
              <div class="image-box">
                <img src="/profile.jpg" alt="徐蕊薇 Jui Wei" class="profile-img" />
              </div>
            </div>
          </div>
        </div>
      </section>

      <section id="skills" class="skills section bg-alt reveal">
        <div class="container">
          <h2 class="section-title">技術能力</h2>
          <div class="skills-grid">
            <div class="skill-category">
              <h3>前端開發 (Frontend)</h3>
              <div class="skill-tags">
                <span class="tag">Vue 3</span>
                <span class="tag">Pinia</span>
                <span class="tag">Vue Router</span>
                <span class="tag">JavaScript (ES6+)</span>
                <span class="tag">HTML5 / CSS3</span>
                <span class="tag">Bootstrap 5</span>
                <span class="tag">Axios</span>
              </div>
            </div>
            <div class="skill-category">
              <h3>後端開發 (Backend)</h3>
              <div class="skill-tags">
                <span class="tag">Java 17</span>
                <span class="tag">Spring Boot 3</span>
                <span class="tag">Spring MVC</span>
                <span class="tag">Spring Security</span>
                <span class="tag">Spring Data JPA / Hibernate</span>
                <span class="tag">RESTful API</span>
              </div>
            </div>
            <div class="skill-category">
              <h3>資料庫與工具 (DB & Tools)</h3>
              <div class="skill-tags">
                <span class="tag">MS SQL Server</span>
                <span class="tag">MySQL</span>
                <span class="tag">Git / GitHub</span>
                <span class="tag">Postman</span>
                <span class="tag">Maven</span>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section id="projects" class="projects section reveal">
        <div class="container">
          <h2 class="section-title">實戰專案</h2>
          <div class="project-card">
            <div class="project-image">
              <div class="carousel">
                <img :src="projectImages[currentImageIndex]" alt="羽球館預約系統展示" class="carousel-img" />
                <button class="carousel-btn prev print-hide" @click="prevImage">❮</button>
                <button class="carousel-btn next print-hide" @click="nextImage">❯</button>
                <div class="carousel-indicators print-hide">
                  <span 
                    v-for="(_, index) in projectImages" 
                    :key="index"
                    class="indicator"
                    :class="{ active: currentImageIndex === index }"
                    @click="currentImageIndex = index"
                  ></span>
                </div>
              </div>
            </div>
            <div class="project-info">
              <h3 class="project-title">羽球館預約管理系統 🏸</h3>
              <p class="project-role">核心負責模組：臨打揪團 (全端開發)</p>
              <p class="project-desc">
                以 5 人團隊協作打造的一站式羽球平台。我獨立負責「臨打揪團模組」，包含資料庫設計、Spring Boot RESTful API 開發，以及 Vue 3 前端互動實作。
              </p>
              <div class="project-features-grid">
                <div class="feature-card">
                  <h4>即時防呆機制</h4>
                  <p>實作時間衝突與資格檢查</p>
                </div>
                <div class="feature-card">
                  <h4>模組化 API 架構</h4>
                  <p>Spring Boot RESTful 設計</p>
                </div>
                <div class="feature-card">
                  <h4>流暢非同步互動</h4>
                  <p>Vue 3 狀態管理與動態篩選</p>
                </div>
                <div class="feature-card">
                  <h4>資料庫關聯設計</h4>
                  <p>SQL Server 實體關聯與交易一致性</p>
                </div>
              </div>
              <div class="project-tech print-hide">
                <span class="tech-tag">Vue 3</span>
                <span class="tech-tag">Pinia</span>
                <span class="tech-tag">Spring Boot</span>
                <span class="tech-tag">SQL Server</span>
              </div>
              <div class="project-links print-hide">
                <a href="https://github.com/tea125458/badminton-vue" target="_blank" class="btn btn-primary">查看 GitHub</a>
                <a href="https://youtu.be/t314dZDJq7s?t=3655" target="_blank" class="btn btn-outline">Demo 影片</a>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section id="experience" class="experience section bg-alt reveal">
        <div class="container">
          <h2 class="section-title">經歷與學歷</h2>
          <div class="timeline-container">
            <div class="timeline">
              <div class="timeline-item">
                <div class="timeline-dot print-hide"></div>
                <div class="timeline-content">
                  <span class="timeline-date">2025.12 - 2026.05</span>
                  <div class="timeline-header">
                    <h4 class="timeline-role">跨域 Java 軟體工程師養成班</h4>
                    <p class="timeline-org">資展國際 (iSpan)</p>
                  </div>
                  <p class="timeline-desc">540 小時密集實戰培訓，完成一站式羽球館預約系統之全端開發與資料庫設計。</p>
                </div>
              </div>
              <div class="timeline-item">
                <div class="timeline-dot print-hide"></div>
                <div class="timeline-content">
                  <span class="timeline-date">過去經歷</span>
                  <div class="timeline-header">
                    <h4 class="timeline-role">後勤與行政支援</h4>
                    <p class="timeline-org">空軍後勤單位 / 南投醫院與連鎖藥局</p>
                  </div>
                  <p class="timeline-desc">負責高紀律要求之流程執行與物資管理，確保帳務資訊準確度，培養對複雜系統防呆機制的敏感度。</p>
                </div>
              </div>
              <div class="timeline-item">
                <div class="timeline-dot print-hide"></div>
                <div class="timeline-content">
                  <span class="timeline-date">2018.09 - 2022.06</span>
                  <div class="timeline-header">
                    <h4 class="timeline-role">中國文學系 學士</h4>
                    <p class="timeline-org">國立中興大學</p>
                  </div>
                  <p class="timeline-desc">培養強大的文本理解、需求分析與溝通能力，能精準捕捉使用者痛點並轉化為系統規格。</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section id="contact" class="contact section reveal">
        <div class="container">
          <div class="contact-header print-hide">
            <h2 class="section-title">聯絡資訊</h2>
            <p class="contact-text">我目前正在尋找 <strong>Java 後端 / 全端工程師</strong> 的職缺。<br>如果您對我的履歷與作品感興趣，歡迎隨時與我聯繫！</p>
          </div>
          <div class="contact-content">
            <div class="contact-cards">
              <a href="mailto:tea125458@gmail.com" class="contact-card">
                <div class="icon-box print-hide">📧</div>
                <h4 class="print-hide">Email</h4>
                <p>tea125458@gmail.com</p>
              </a>
              <a href="https://github.com/tea125458" target="_blank" class="contact-card">
                <div class="icon-box print-hide">💻</div>
                <h4 class="print-hide">GitHub</h4>
                <p>github.com/tea125458</p>
              </a>
              <div class="contact-card">
                <div class="icon-box print-hide">📱</div>
                <h4 class="print-hide">Phone</h4>
                <p>0910-069-213</p>
              </div>
            </div>
          </div>
        </div>
      </section>
    </main>

    <footer class="footer print-hide">
      <p>&copy; 2026 徐蕊薇 Jui Wei. All rights reserved.</p>
    </footer>
  </div>
</template>

<style scoped>
/* =========== 網頁排版  =========== */
.app-wrapper { min-height: 100vh; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif; }
.navbar { position: fixed; top: 0; left: 0; right: 0; background-color: rgba(255, 255, 255, 0.9); backdrop-filter: blur(8px); border-bottom: 1px solid #eee; z-index: 100; padding: 1rem 0; }
.nav-content { display: flex; justify-content: space-between; align-items: center; max-width: 1200px; margin: 0 auto; padding: 0 2rem;}
.logo { font-size: 1.5rem; font-weight: 700; color: #00b4b4; text-decoration: none;}
.nav-links { display: flex; gap: 2rem; list-style: none; margin: 0; padding: 0;}
.nav-links a { color: #334155; font-weight: 500; text-decoration: none;}
.nav-links a:hover { color: #00b4b4; }

.container { max-width: 1200px; margin: 0 auto; padding: 0 2rem; }
.section { padding: 5rem 0; }
.section-title { font-size: 2rem; font-weight: 700; margin-bottom: 3rem; text-align: center; color: #0f172a; }
.bg-alt { background-color: #f8fafc; }

.hero { min-height: 90vh; display: flex; align-items: center; padding-top: 5rem; justify-content: center;}
.hero-content { text-align: center; max-width: 800px; }
.hero-subtitle { color: #00b4b4; font-size: 1.25rem; font-weight: 600; margin-bottom: 0.5rem; }
.hero-title { font-size: 4rem; font-weight: 800; margin-bottom: 0.5rem; color: #0f172a; }
.hero-role { font-size: 2rem; font-weight: 600; color: #475569; margin-bottom: 1.5rem; }
.hero-description { font-size: 1.125rem; color: #64748b; margin-bottom: 2.5rem; }
.hero-actions { display: flex; gap: 1rem; justify-content: center; }
.btn { padding: 0.75rem 1.5rem; border-radius: 8px; font-weight: 600; cursor: pointer; text-decoration: none; border: none; font-size: 1rem;}
.btn-primary { background-color: #00b4b4; color: white; }
.btn-outline { background-color: transparent; border: 1px solid #00b4b4; color: #00b4b4; }

.contact-header { text-align: center; margin-bottom: 2rem; }
.contact-text { font-size: 1.125rem; color: #475569; margin-bottom: 3rem; line-height: 1.6;}
.contact-cards { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 2rem; max-width: 800px; margin: 0 auto;}
.contact-card { background: white; padding: 2rem; border-radius: 12px; box-shadow: 0 4px 6px rgba(0,0,0,0.05); display: flex; flex-direction: column; align-items: center; text-decoration: none; color: #334155; transition: transform 0.3s ease, box-shadow 0.3s ease;}
.contact-card:hover { transform: translateY(-5px); box-shadow: 0 10px 25px rgba(0,0,0,0.1); }
.icon-box { font-size: 2.5rem; margin-bottom: 1rem; }

.about-content { display: grid; grid-template-columns: 3fr 2fr; gap: 4rem; align-items: center; }
.about-text p { margin-bottom: 1.5rem; font-size: 1.125rem; color: #334155; line-height: 1.7; text-align: justify;}
.image-box { border-radius: 12px; overflow: hidden; box-shadow: 0 10px 15px rgba(0,0,0,0.1); }
.profile-img { width: 100%; height: auto; display: block; }

.skills-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 2rem; }
.skill-category { background: white; padding: 2.5rem; border-radius: 12px; box-shadow: 0 4px 6px rgba(0,0,0,0.05); transition: transform 0.3s ease, box-shadow 0.3s ease;}
.skill-category:hover { transform: translateY(-5px); box-shadow: 0 10px 25px rgba(0,0,0,0.1); }
.skill-category h3 { color: #00b4b4; margin-bottom: 1.5rem; font-size: 1.25rem; }
.skill-tags { display: flex; flex-wrap: wrap; gap: 0.75rem; }
.tag { background-color: #f0fdfa; color: #334155; padding: 0.5rem 1rem; border-radius: 99px; font-size: 0.875rem; }

.project-card { background: white; border-radius: 12px; overflow: hidden; box-shadow: 0 10px 15px rgba(0,0,0,0.05); transition: transform 0.3s ease, box-shadow 0.3s ease;}
.project-card:hover { transform: translateY(-5px); box-shadow: 0 15px 30px rgba(0,0,0,0.12); }
.project-image { width: 100%; background-color: #f8fafc; position: relative; }
.carousel { position: relative; width: 100%; display: flex; align-items: center; justify-content: center; }
.carousel-img { width: 100%; height: auto; display: block; }
.carousel-btn { position: absolute; top: 50%; transform: translateY(-50%); background: rgba(0,0,0,0.3); color: white; border: none; width: 40px; height: 40px; border-radius: 50%; cursor: pointer; z-index: 10;}
.carousel-btn.prev { left: 1rem; }
.carousel-btn.next { right: 1rem; }
.carousel-indicators { position: absolute; bottom: 1rem; left: 50%; transform: translateX(-50%); display: flex; gap: 0.5rem; }
.indicator { width: 10px; height: 10px; border-radius: 50%; background: rgba(255,255,255,0.5); cursor: pointer; }
.indicator.active { background: white; transform: scale(1.2); }
.project-info { padding: 3rem; }
.project-title { font-size: 1.75rem; font-weight: 700; margin-bottom: 0.5rem; color: #0f172a;}
.project-role { color: #00b4b4; font-weight: 600; margin-bottom: 1.5rem; }
.project-desc { color: #475569; margin-bottom: 1.5rem; line-height: 1.7; }

/* Feature Grid */
.project-features-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 1rem; margin-bottom: 2rem; }
.feature-card { background: #f8fafc; padding: 1.25rem; border-radius: 8px; border-left: 4px solid #00b4b4; }
.feature-card h4 { color: #0f172a; margin-bottom: 0.5rem; font-size: 1rem; }
.feature-card p { color: #475569; font-size: 0.9rem; margin: 0; }
@media (max-width: 640px) {
  .project-features-grid { grid-template-columns: 1fr; }
}

.project-tech { display: flex; flex-wrap: wrap; gap: 0.5rem; margin-bottom: 2rem; }
.tech-tag { background-color: #f0fdfa; padding: 0.25rem 0.75rem; border-radius: 4px; font-size: 0.875rem; color: #334155; }
.project-links { display: flex; gap: 1rem; }

.timeline-container { max-width: 800px; margin: 0 auto; }
.timeline { position: relative; padding-left: 2rem; border-left: 2px solid #e2e8f0; }
.timeline-item { position: relative; padding-bottom: 3rem; }
.timeline-dot { position: absolute; left: -2.4rem; top: 0.25rem; width: 16px; height: 16px; border-radius: 50%; background-color: #00b4b4; border: 4px solid white; }
.timeline-content { background: white; padding: 2rem; border-radius: 12px; box-shadow: 0 4px 6px rgba(0,0,0,0.05); transition: transform 0.3s ease, box-shadow 0.3s ease;}
.timeline-content:hover { transform: translateY(-5px); box-shadow: 0 10px 25px rgba(0,0,0,0.1); }
.timeline-date { display: inline-block; color: #00b4b4; font-weight: 600; font-size: 0.875rem; margin-bottom: 1rem; background: #f0fdfa; padding: 0.25rem 0.75rem; border-radius: 99px; }

.timeline-header { margin-bottom: 0.5rem; }
.timeline-role { font-size: 1.25rem; font-weight: 700; margin-bottom: 0.2rem; color: #0f172a;}
.timeline-org { color: #64748b; font-weight: 500; font-size: 1rem;}
.timeline-desc { color: #475569; line-height: 1.6; margin: 0;}

.footer { text-align: center; padding: 2rem; color: #94a3b8; border-top: 1px solid #eee;}

/* =======================================================
   RWD 手機版響應式設定
   ======================================================= */
@media (max-width: 768px) {
  /* Navbar */
  .nav-links { display: none; }
  .nav-content { justify-content: center; }
  
  /* Container & Padding */
  .container { padding: 0 1.5rem; }
  .section { padding: 3.5rem 0; }
  .section-title { font-size: 1.75rem; margin-bottom: 2rem; }

  /* Hero */
  .hero { padding-top: 6rem; min-height: auto; padding-bottom: 2rem; }
  .hero-title { font-size: 2.5rem; }
  .hero-role { font-size: 1.5rem; margin-bottom: 1rem; }
  .hero-description { font-size: 1rem; margin-bottom: 2rem; padding: 0 1rem; }
  .hero-actions { flex-direction: column; gap: 1rem; padding: 0 1rem; }
  .hero-actions .btn { width: 100%; text-align: center; }

  /* About */
  .about-content { grid-template-columns: 1fr; gap: 2.5rem; }
  .image-box { max-width: 200px; margin: 0 auto; order: -1; } /* 照片放到上方 */
  .about-text p { font-size: 1rem; text-align: justify; }

  /* Skills */
  .skill-category { padding: 1.5rem; }
  .skill-category h3 { font-size: 1.15rem; margin-bottom: 1rem; }
  .skill-tags { gap: 0.5rem; }
  .tag { font-size: 0.8rem; padding: 0.4rem 0.8rem; }

  /* Projects */
  .project-info { padding: 1.5rem; }
  .project-title { font-size: 1.4rem; }
  .project-links { flex-direction: column; }
  .project-links .btn { text-align: center; width: 100%; }

  /* Timeline */
  .timeline { padding-left: 1.25rem; border-left-width: 2px; margin-left: 0.5rem; }
  .timeline-dot { left: -1.75rem; width: 14px; height: 14px; border-width: 3px; }
  .timeline-content { padding: 1.25rem; }
  .timeline-date { font-size: 0.8rem; padding: 0.2rem 0.6rem; }
  .timeline-role { font-size: 1.1rem; }
  
  /* Contact */
  .contact-cards { grid-template-columns: 1fr; gap: 1rem; }
  .contact-card { padding: 1.5rem; }
}

/* Scroll Reveal Animation */
.reveal {
  opacity: 0;
  transform: translateY(30px);
  transition: all 0.8s cubic-bezier(0.5, 0, 0, 1);
}
.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}

/* =======================================================
   PDF 列印專用 CSS 
   ======================================================= */
@media print {
  * {
    -webkit-print-color-adjust: exact !important;
    print-color-adjust: exact !important;
  }

  @page { margin: 12mm 15mm; }
  
  body {
    background-color: white !important;
    font-size: 10pt !important;
    line-height: 1.5 !important;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif !important;
    color: #1e293b !important;
  }

  .print-hide, .navbar, .carousel-btn, .carousel-indicators, .bg-alt, .contact-text, .contact-header h2 {
    display: none !important;
  }
  
  .section { padding: 0 !important; margin-bottom: 1.5rem !important; display: block !important; }
  .container { padding: 0 !important; max-width: 100% !important; margin: 0 !important; display: block !important; }

  .section-title {
    font-size: 12pt !important;
    border-bottom: 1.5px solid #1e293b !important;
    margin-bottom: 12px !important;
    padding-bottom: 4px !important;
    text-align: left !important;
    color: #0f172a !important;
    font-weight: 700 !important;
    page-break-after: avoid !important;
  }

  /* --- Reorder sections for print --- */
  main { display: flex !important; flex-direction: column !important; }
  .hero { order: 1 !important; padding: 0 !important; margin-bottom: 4px !important; display: block !important; min-height: auto !important; }
  .contact { order: 2 !important; margin-bottom: 1.5rem !important; padding: 0 !important; }
  .about { order: 3 !important; }
  .skills { order: 4 !important; }
  .projects { order: 5 !important; page-break-inside: avoid !important; }
  .experience { order: 6 !important; }

  /* --- 3. Hero & Contact --- */
  .hero-content { text-align: left !important; margin: 0 !important; }
  .hero-title { font-size: 22pt !important; font-weight: 800 !important; margin: 0 !important; color: #0f172a !important; letter-spacing: 1px !important;}
  .hero-role { font-size: 11.5pt !important; margin: 4px 0 0 0 !important; color: #00b4b4 !important; font-weight: 600 !important; display: inline-block !important; }
  
  .contact-content { text-align: left !important; }
  .contact-cards { display: block !important; margin: 0 !important; }
  .contact-card { display: inline-block !important; padding: 0 !important; margin-right: 18px !important; border: none !important; box-shadow: none !important; }
  .contact-card p { font-size: 9.5pt !important; display: inline !important; color: #475569 !important; font-weight: 500 !important;}

  /* --- 4. About (70% text, 30% photo) --- */
  .about-content { display: flex !important; flex-direction: row !important; gap: 2rem !important; align-items: stretch !important; }
  .about-text { flex: 7 !important; }
  .about-text p { margin-bottom: 0.5rem !important; text-align: justify !important; color: #334155 !important; font-size: 10pt !important;}
  .about-image { flex: 3 !important; display: flex !important; justify-content: flex-end !important; margin: 0 !important; width: 100% !important; }
  .image-box { border: none !important; box-shadow: none !important; background: transparent !important; padding: 0 !important; height: 100% !important; display: flex !important; align-items: flex-start !important; }
  .profile-img { width: 100% !important; max-width: 140px !important; height: auto !important; object-fit: cover !important; border-radius: 6px !important; border: 1px solid #e2e8f0 !important; }

  /* --- 5. Skills (Clean commas) --- */
  .skills-grid { display: block !important; }
  .skill-category { display: block !important; margin-bottom: 8px !important; padding: 0 !important; border: none !important; box-shadow: none !important; page-break-inside: avoid !important; }
  .skill-category h3 { display: inline-block !important; font-size: 10.5pt !important; margin: 0 8px 0 0 !important; color: #0f172a !important; font-weight: 700 !important; vertical-align: baseline !important; }
  .skill-category h3::after { content: "："; }
  .skill-tags { display: inline !important; }
  .tag { display: inline !important; border: none !important; padding: 0 !important; font-size: 10pt !important; background: transparent !important; color: #334155 !important;}
  .tag::after { content: ", "; color: #334155 !important; }
  .tag:last-child::after { content: ""; }

  /* --- 6. Projects (Bullet points, No image) --- */
  .project-card { display: block !important; border: none !important; box-shadow: none !important; page-break-inside: avoid !important; }
  .project-info { padding: 0 !important; }
  .project-image { display: none !important; }
  
  .project-title { font-size: 12pt !important; margin-bottom: 4px !important; color: #0f172a !important; font-weight: 700 !important; display: inline-block !important;}
  .project-role { font-size: 10.5pt !important; margin-bottom: 8px !important; color: #475569 !important; font-weight: 600 !important; display: inline-block !important; margin-left: 8px !important; }
  .project-desc { margin-bottom: 8px !important; font-size: 10pt !important; color: #334155 !important;}
  
  .project-features-grid { display: block !important; margin-bottom: 0 !important; }
  .feature-card { display: block !important; background: transparent !important; padding: 0 0 0 12px !important; border-left: none !important; margin-bottom: 6px !important; position: relative !important;}
  .feature-card::before { content: "•" !important; position: absolute !important; left: 0 !important; color: #00b4b4 !important; font-weight: bold !important;}
  .feature-card h4 { display: inline-block !important; font-size: 10.5pt !important; margin: 0 !important; color: #0f172a !important; font-weight: 600 !important; }
  .feature-card h4::after { content: "："; }
  .feature-card p { display: inline !important; font-size: 10pt !important; color: #334155 !important; margin: 0 !important; }
  .project-tech { display: none !important; } 

  /* --- 7. Experience (150px left column) --- */
  .timeline { border-left: none !important; padding-left: 0 !important; }
  .timeline-container { margin-top: 0 !important; }
  .timeline-item { padding-bottom: 0 !important; margin-bottom: 1.2rem !important; border-bottom: none !important; page-break-inside: avoid !important; }
  .timeline-dot { display: none !important; }
  
  .timeline-content { 
    display: grid !important;
    grid-template-columns: 150px 1fr !important;
    gap: 4px 0 !important;
    padding: 0 !important; border: none !important; box-shadow: none !important;
  }
  
  .timeline-date { width: auto !important; font-size: 10pt !important; background: transparent !important; color: #0f172a !important; font-weight: 600 !important; padding: 0 !important; margin: 0 !important; grid-column: 1 !important; grid-row: 1 / 3 !important; }
  .timeline-header { margin: 0 !important; display: block !important; grid-column: 2 !important; grid-row: 1 !important; }
  .timeline-role { display: inline-block !important; font-size: 11pt !important; font-weight: 700 !important; color: #0f172a !important; margin: 0 !important;}
  .timeline-org { display: inline-block !important; font-size: 10pt !important; color: #475569 !important; margin: 0 0 0 8px !important;}
  .timeline-desc { width: auto !important; font-size: 10pt !important; color: #334155 !important; margin: 0 !important; grid-column: 2 !important; grid-row: 2 !important; }

  /* --- 8. Disable Scroll Reveal for Print --- */
  .reveal { opacity: 1 !important; transform: none !important; transition: none !important; }
}
</style>