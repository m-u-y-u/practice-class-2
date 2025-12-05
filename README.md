<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>我的个人介绍 | 计科2508班</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: "微软雅黑", sans-serif; line-height: 1.6; }
        
        /* 导航样式 */
        nav { background: #333; padding: 1rem; position: sticky; top: 0; z-index: 100; }
        nav ul { display: flex; list-style: none; justify-content: center; flex-wrap: wrap; }
        nav a { color: white; text-decoration: none; padding: 0.5rem 1.5rem; transition: background 0.3s; }
        nav a:hover { background: #555; border-radius: 4px; }
        nav a.active { background: #007bff; }
        
        /* 页面容器样式 */
        .page-container { display: none; }
        .page-container.active { display: block; }
        
        /* 首页样式 */
        .carousel { 
            width: 95%; 
            margin: 2rem auto; 
            overflow: hidden; 
            position: relative; 
            height: 500px; 
        }
        .carousel-inner { display: flex; transition: transform 0.5s ease; height: 100%; }
        .carousel-item { min-width: 100%; height: 100%; }
        .carousel-item img { width: 100%; height: 100%; object-fit: cover; }
        .carousel-btn { position: absolute; top: 50%; transform: translateY(-50%); background: rgba(0,0,0,0.5); color: white; border: none; padding: 1rem; cursor: pointer; border-radius: 50%; }
        .prev { left: 10px; }
        .next { right: 10px; }
        
        .intro { width: 80%; margin: 2rem auto; padding: 2rem; background: #f8f9fa; border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.1); }
        .intro h2 { color: #333; margin-bottom: 1rem; font-size: 1.8rem; }
        .intro p { color: #666; margin-bottom: 1rem; line-height: 1.8; }
        .intro .class-tag { display: inline-block; padding: 0.3rem 1rem; background: #007bff; color: white; border-radius: 20px; font-size: 0.9rem; margin-bottom: 1rem; }
        
        /* 关于我样式 */
        .about-container { width: 80%; margin: 2rem auto; }
        .about-card { display: flex; gap: 2rem; margin-bottom: 3rem; opacity: 0; transform: translateY(20px); transition: all 0.8s; }
        .about-card.visible { opacity: 1; transform: translateY(0); }
        .about-card img { width: 300px; height: 300px; border-radius: 50%; object-fit: cover; cursor: pointer; }
        .about-info { flex: 1; padding: 1rem; }
        .about-info h2 { color: #333; margin-bottom: 1rem; }
        .about-info p { color: #666; margin-bottom: 1rem; }
        
        /* 模态框样式 */
        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.8); z-index: 200; justify-content: center; align-items: center; }
        .modal img { max-width: 90%; max-height: 90%; }
        .close { position: absolute; top: 20px; right: 30px; color: white; font-size: 2rem; cursor: pointer; }
        
        /* 技能特长样式 */
        .skills-container { width: 70%; margin: 3rem auto; }
        .skill-card { margin-bottom: 2rem; }
        .skill-card h3 { color: #333; margin-bottom: 0.5rem; }
        .skill-bar { width: 100%; height: 25px; background: #eee; border-radius: 12px; overflow: hidden; }
        .skill-progress { height: 100%; background: #007bff; border-radius: 12px; width: 0; transition: width 1s ease; }
        .skill-tag { display: flex; flex-wrap: wrap; gap: 1rem; margin-top: 2rem; }
        .tag { padding: 0.5rem 1rem; background: #f0f0f0; border-radius: 20px; cursor: pointer; transition: all 0.3s; }
        .tag:hover { background: #007bff; color: white; transform: scale(1.05); }
        
        /* 经历样式 */
        .timeline { width: 80%; margin: 3rem auto; position: relative; }
        .timeline::before { content: ''; position: absolute; top: 0; left: 50%; width: 4px; height: 100%; background: #007bff; transform: translateX(-50%); }
        .timeline-item { margin-bottom: 3rem; position: relative; }
        .timeline-item:nth-child(odd) .timeline-content { left: 0; }
        .timeline-item:nth-child(even) .timeline-content { right: 0; }
        .timeline-content { width: 45%; position: relative; background: #f8f9fa; padding: 1.5rem; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); transition: transform 0.3s; }
        .timeline-content:hover { transform: scale(1.03); }
        .timeline-dot { position: absolute; top: 1rem; left: 50%; width: 20px; height: 20px; background: #007bff; border-radius: 50%; transform: translateX(-50%); z-index: 10; }
        .timeline-content h3 { color: #333; margin-bottom: 0.5rem; }
        .timeline-content .date { color: #007bff; font-weight: bold; margin-bottom: 1rem; }
        
        /* 联系我样式 */
        .contact-container { width: 80%; margin: 3rem auto; display: flex; gap: 3rem; }
        .contact-info { flex: 1; }
        .contact-form { flex: 1; }
        .contact-info h2, .contact-form h2 { color: #333; margin-bottom: 1.5rem; }
        .contact-info p { margin-bottom: 1rem; display: flex; align-items: center; }
        .contact-info p i { margin-right: 0.5rem; color: #007bff; font-size: 1.2rem; }
        .form-group { margin-bottom: 1.5rem; }
        .form-group label { display: block; margin-bottom: 0.5rem; color: #333; }
        .form-group input, .form-group textarea { width: 100%; padding: 0.8rem; border: 1px solid #ddd; border-radius: 4px; font-size: 1rem; }
        .form-group input:focus, .form-group textarea:focus { outline: none; border-color: #007bff; }
        .error { color: red; font-size: 0.8rem; margin-top: 0.3rem; display: none; }
        .submit-btn { padding: 0.8rem 2rem; background: #007bff; color: white; border: none; border-radius: 4px; cursor: pointer; transition: background 0.3s; }
        .submit-btn:hover { background: #0056b3; }
        .success { color: green; font-size: 1rem; margin-top: 1rem; display: none; text-align: center; }
        
        /* 通用页脚样式 */
        footer { text-align: center; padding: 1rem; background: #333; color: white; margin-top: 2rem; }
        
        /* 模拟图标样式 */
        .icon { display: inline-block; width: 20px; height: 20px; background: #007bff; border-radius: 50%; margin-right: 8px; }
        
        /* 响应式适配 */
        @media (max-width: 768px) {
            .carousel { 
                height: 350px;
                width: 98%;
            }
            .intro { width: 95%; padding: 1.5rem; }
            nav ul { gap: 0.5rem; }
            nav a { padding: 0.5rem 1rem; font-size: 0.9rem; }
            
            .about-card { flex-direction: column; align-items: center; }
            .about-card img { width: 200px; height: 200px; }
            
            .contact-container { flex-direction: column; width: 95%; }
            
            .timeline::before { left: 30px; }
            .timeline-content { width: calc(100% - 60px); left: 60px !important; right: auto !important; }
            .timeline-dot { left: 30px; }
        }
    </style>
</head>
<body>
    <!-- 导航栏 -->
    <nav>
        <ul>
            <li><a href="#home" class="nav-link active">首页</a></li>
            <li><a href="#about" class="nav-link">关于我</a></li>
            <li><a href="#skills" class="nav-link">技能特长</a></li>
            <li><a href="#experience" class="nav-link">经历</a></li>
            <li><a href="#contact" class="nav-link">联系我</a></li>
        </ul>
    </nav>

    <!-- 首页 -->
    <div id="home" class="page-container active">
        <!-- 轮播图 -->
        <div class="carousel">
            <div class="carousel-inner">
                <div class="carousel-item"><img src="campus.jpg" alt="大学校园风景"></div>
                <div class="carousel-item"><img src="games.jpg" alt="游戏"></div>
            </div>
            <button class="carousel-btn prev">&lt;</button>
            <button class="carousel-btn next">&gt;</button>
        </div>

        <!-- 个人简介 -->
        <div class="intro">
            <span class="class-tag">计算机科学与技术2508班</span>
            <h2>你好，我是平宇</h2>
            <p>一名刚踏入大学校园的计算机科学与技术专业新生，就读于2508班。怀揣着对计算机领域的热爱，开启了大学的专业学习之旅。</p>
            <p>作为计科2508班的一员，我希望在大学四年里夯实专业基础，提升实践能力，未来能在软件开发、人工智能等方向深耕发展。</p>
            <p>点击导航栏可查看我的学习进度、校园经历和练习作品等各项信息（当然很多都是空的😁，毕竟才大一），也欢迎通过「联系我」页面和我交流学习心得～</p>
        </div>

        <footer>
            <p>© 2025 计科2508班 平宇的个人主页 | 大学成长记录</p>
        </footer>
    </div>

    <!-- 关于我 -->
    <div id="about" class="page-container">
        <div class="about-container">
            <div class="about-card">
                <img src="avatar.jpg" alt="个人头像" id="avatar">
                <div class="about-info">
                    <h2>基本信息</h2>
                    <p>姓名：平宇</p>
                    <p>年龄：18岁</p>
                    <p>籍贯：湖北省荆州市</p>
                    <p>学历：华中科技大学 计算机科学与技术 本科</p>
                    <p>兴趣爱好：大概是没有的😢</p>
                    <p>性格特点：我难以评价我自己🤣</p>
                </div>
            </div>
        </div>

        <!-- 图片查看模态框 -->
        <div class="modal" id="imgModal">
            <span class="close" id="closeModal">&times;</span>
            <img src="" alt="大图查看" id="modalImg">
        </div>

        <footer>
            <p>© 2025 我的个人网站 | 所有权利保留</p>
        </footer>
    </div>

    <!-- 技能特长 -->
    <div id="skills" class="page-container">
        <div class="skills-container">
            <h2 style="text-align: center; margin-bottom: 2rem; color: #333;">我的技能特长（都好难😭）</h2>

            <!-- 专业技能进度条 -->
            <div class="skill-card">
                <h3>c语言</h3>
                <div class="skill-bar">
                    <div class="skill-progress" data-progress="0"></div>
                </div>
            </div>

            <div class="skill-card">
                <h3>微积分</h3>
                <div class="skill-bar">
                    <div class="skill-progress" data-progress="0"></div>
                </div>
            </div>

            <div class="skill-card">
                <h3>线性代数</h3>
                <div class="skill-bar">
                    <div class="skill-progress" data-progress="0"></div>
                </div>
            </div>

            <div class="skill-card">
                <h3>大学英语</h3>
                <div class="skill-bar">
                    <div class="skill-progress" data-progress="0"></div>
                </div>
            </div>

            <div class="skill-card">
                <h3>吃，喝，玩，睡</h3>
                <div class="skill-bar">
                    <div class="skill-progress" data-progress="100"></div>
                </div>
            </div>
        </div>

        <footer>
            <p>© 2025 我的个人网站 | 所有权利保留</p>
        </footer>
    </div>

    <!-- 经历 -->
    <div id="experience" class="page-container">
        <div class="timeline">
            <!-- 教育经历 -->
            <div class="timeline-item">
                <div class="timeline-dot"></div>
                <div class="timeline-content">
                    <div class="date">2025.09.01至今</div>
                    <h3>华中科技大学 - 计算机科学与技术</h3>
                </div>
            </div>

            <!-- 高中经历 -->
            <div class="timeline-item">
                <div class="timeline-dot"></div>
                <div class="timeline-content">
                    <div class="date">2022 - 2025</div>
                    <h3>华中师范大学第一附属中学学习，期间数学竞赛省赛获二等奖（一点用处也没有😭）</h3>
                </div>
            </div>

            <!-- 初中经历 -->
            <div class="timeline-item">
                <div class="timeline-dot"></div>
                <div class="timeline-content">
                    <div class="date">2019 - 2022</div>
                    <h3>就读于监利新教育实验中学</h3>
                </div>
            </div>
        </div>

        <footer>
            <p>© 2025 我的个人网站 | 所有权利保留</p>
        </footer>
    </div>

    <!-- 联系我 -->
    <div id="contact" class="page-container">
        <div class="contact-container">
            <!-- 联系信息 -->
            <div class="contact-info">
                <h2>联系方式</h2>
                <p><span class="icon"></span> 邮箱：3605331686@qq.com</p>
                <h2 style="margin-top: 2rem;">社交账号</h2>
                <p><span class="icon"></span>这个人很懒，什么都不想留（其实大概率没有😢）</p>
            </div>

            <!-- 联系表单 -->
            <div class="contact-form">
                <h2>给我留言</h2>
                <form id="contactForm">
                    <div class="form-group">
                        <label for="name">姓名</label>
                        <input type="text" id="name" name="name" placeholder="请输入您的姓名">
                        <div class="error" id="nameError">请输入有效姓名（2-10个字符）</div>
                    </div>

                    <div class="form-group">
                        <label for="email">邮箱</label>
                        <input type="email" id="email" name="email" placeholder="请输入您的邮箱">
                        <div class="error" id="emailError">请输入有效的邮箱地址</div>
                    </div>

                    <div class="form-group">
                        <label for="phone">电话（选填）</label>
                        <input type="tel" id="phone" name="phone" placeholder="请输入您的电话">
                        <div class="error" id="phoneError">请输入有效的手机号码</div>
                    </div>

                    <div class="form-group">
                        <label for="message">留言内容</label>
                        <textarea id="message" name="message" rows="5" placeholder="请输入您想对我说的话"></textarea>
                        <div class="error" id="messageError">留言内容不能为空（至少10个字符）</div>
                    </div>

                    <button type="submit" class="submit-btn">提交留言</button>
                    <div class="success" id="successMsg">留言提交成功！我会尽快回复您</div>
                </form>
            </div>
        </div>

        <footer>
            <p>© 2025 我的个人网站 | 所有权利保留</p>
        </footer>
    </div>

    <script>
        // 页面切换功能
        const navLinks = document.querySelectorAll('.nav-link');
        const pageContainers = document.querySelectorAll('.page-container');
        
        navLinks.forEach(link => {
            link.addEventListener('click', (e) => {
                e.preventDefault();
                
                // 移除所有活跃状态
                navLinks.forEach(l => l.classList.remove('active'));
                pageContainers.forEach(p => p.classList.remove('active'));
                
                // 添加当前活跃状态
                link.classList.add('active');
                const targetId = link.getAttribute('href').substring(1);
                document.getElementById(targetId).classList.add('active');
                
                // 触发相关页面的初始化
                if (targetId === 'skills') {
                    triggerSkillAnimation();
                } else if (targetId === 'about') {
                    triggerAboutAnimation();
                }
            });
        });
        
        // 首页轮播图功能
        const carousel = document.querySelector('.carousel-inner');
        const prevBtn = document.querySelector('.prev');
        const nextBtn = document.querySelector('.next');
        const items = document.querySelectorAll('.carousel-item');
        let currentIndex = 0;
        
        function getItemWidth() {
            return items[0].offsetWidth;
        }
        
        function goToSlide(index) {
            if (index < 0) index = items.length - 1;
            if (index >= items.length) index = 0;
            const itemWidth = getItemWidth();
            carousel.style.transform = `translateX(-${index * itemWidth}px)`;
            currentIndex = index;
        }
        
        prevBtn.addEventListener('click', () => goToSlide(currentIndex - 1));
        nextBtn.addEventListener('click', () => goToSlide(currentIndex + 1));
        
        // 自动轮播
        let autoPlay = setInterval(() => goToSlide(currentIndex + 1), 3000);
        
        // 鼠标悬停暂停轮播
        const carouselContainer = document.querySelector('.carousel');
        carouselContainer.addEventListener('mouseenter', () => clearInterval(autoPlay));
        carouselContainer.addEventListener('mouseleave', () => {
            autoPlay = setInterval(() => goToSlide(currentIndex + 1), 3000);
        });
        
        // 窗口大小变化时重新计算轮播宽度
        window.addEventListener('resize', () => goToSlide(currentIndex));
        
        // 关于我页面功能
        function triggerAboutAnimation() {
            const cards = document.querySelectorAll('.about-card');
            cards.forEach(card => {
                const cardTop = card.getBoundingClientRect().top;
                const windowHeight = window.innerHeight;
                if (cardTop < windowHeight * 0.8) {
                    card.classList.add('visible');
                }
            });
        }
        
        // 模态框交互
        const modal = document.getElementById('imgModal');
        const modalImg = document.getElementById('modalImg');
        const avatar = document.getElementById('avatar');
        const closeModal = document.getElementById('closeModal');
        
        function openModal(imgSrc) {
            modal.style.display = 'flex';
            modalImg.src = imgSrc;
        }
        
        if (avatar) {
            avatar.addEventListener('click', () => openModal(avatar.src));
        }
        
        closeModal.addEventListener('click', () => {
            modal.style.display = 'none';
        });
        
        modal.addEventListener('click', (e) => {
            if (e.target === modal) modal.style.display = 'none';
        });
        
        // 技能特长页面功能
        function triggerSkillAnimation() {
            const progressBars = document.querySelectorAll('.skill-progress');
            progressBars.forEach(bar => {
                const barTop = bar.getBoundingClientRect().top;
                const windowHeight = window.innerHeight;
                if (barTop < windowHeight * 0.8 && !bar.classList.contains('animated')) {
                    bar.classList.add('animated');
                    bar.style.width = bar.getAttribute('data-progress') + '%';
                }
            });
        }
        
        // 联系表单验证
        const form = document.getElementById('contactForm');
        if (form) {
            const nameInput = document.getElementById('name');
            const emailInput = document.getElementById('email');
            const phoneInput = document.getElementById('phone');
            const messageInput = document.getElementById('message');
            const successMsg = document.getElementById('successMsg');
            
            function validateName() {
                const error = document.getElementById('nameError');
                if (nameInput.value.trim().length < 2 || nameInput.value.trim().length > 10) {
                    error.style.display = 'block';
                    return false;
                }
                error.style.display = 'none';
                return true;
            }
            
            function validateEmail() {
                const error = document.getElementById('emailError');
                const reg = /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,6}$/;
                if (!reg.test(emailInput.value.trim())) {
                    error.style.display = 'block';
                    return false;
                }
                error.style.display = 'none';
                return true;
            }
            
            function validatePhone() {
                const error = document.getElementById('phoneError');
                if (phoneInput.value.trim() === '') return true; // 选填
                const reg = /^1[3-9]\d{9}$/;
                if (!reg.test(phoneInput.value.trim())) {
                    error.style.display = 'block';
                    return false;
                }
                error.style.display = 'none';
                return true;
            }
            
            function validateMessage() {
                const error = document.getElementById('messageError');
                if (messageInput.value.trim().length < 10) {
                    error.style.display = 'block';
                    return false;
                }
                error.style.display = 'none';
                return true;
            }
            
            // 实时验证
            nameInput.addEventListener('blur', validateName);
            emailInput.addEventListener('blur', validateEmail);
            phoneInput.addEventListener('blur', validatePhone);
            messageInput.addEventListener('blur', validateMessage);
            
            // 表单提交
            form.addEventListener('submit', (e) => {
                e.preventDefault();
                
                const isNameValid = validateName();
                const isEmailValid = validateEmail();
                const isPhoneValid = validatePhone();
                const isMessageValid = validateMessage();
                
                if (isNameValid && isEmailValid && isPhoneValid && isMessageValid) {
                    form.reset();
                    successMsg.style.display = 'block';
                    
                    setTimeout(() => {
                        successMsg.style.display = 'none';
                    }, 3000);
                }
            });
        }
        
        // 初始触发首页轮播和关于我动画
        window.addEventListener('load', () => {
            triggerAboutAnimation();
            triggerSkillAnimation();
        });
    </script>
</body>
</html>
