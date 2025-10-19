<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chúc Mừng Ngày Phụ Nữ!</title>
    
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Great+Vibes&display=swap" rel="stylesheet">

    <style>
        :root {
            --bg-start: linear-gradient(135deg, #ff9a9e 0%, #fad0c4 100%);
            --bg-main: #fbe0e6; /* Màu hồng nhạt */
            --bg-dark: #2c3e50; /* Màu nền tối lúc mở thiệp */
            --envelope-color: #ffb6c1;
            --envelope-flap-color: #ff9a9e;
            --card-color: #ffffff;
            --font-cursive: 'Great Vibes', cursive;
        }

        body {
            font-family: Arial, sans-serif;
            margin: 0;
            overflow: hidden;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            transition: background 1s ease;
            background: var(--bg-start);
        }

        .hidden {
            display: none !important;
        }

        /* 0. Màn hình Loading */
        #loading-screen {
            color: white;
            font-size: 1.2rem;
            text-align: center;
        }
        #loading-screen p {
            opacity: 0;
            animation: fadeIn 1s forwards;
        }

        /* 1. Màn hình Bắt đầu */
        #start-screen {
            text-align: center;
            color: white;
            animation: fadeIn 0.5s;
        }

        #start-screen h1 {
            font-family: var(--font-cursive); /* Áp dụng font chữ */
            font-size: 5rem; /* Tăng kích cỡ font */
            font-weight: normal;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
            margin-bottom: 0;
        }

        #start-button {
            padding: 10px 20px;
            font-size: 1.2rem;
            border: none;
            border-radius: 25px;
            background-color: white;
            color: var(--envelope-flap-color);
            cursor: pointer;
            transition: transform 0.2s;
        }
        #start-button:hover {
            transform: scale(1.1);
        }

        /* 2. Màn hình chính (có bao thư) */
        #main-content {
            position: relative;
            width: 100%;
            height: 100%;
            display: flex;
            justify-content: center;
            align-items: center;
        }

        /* Nút điều khiển nhạc */
        #music-controls {
            position: absolute;
            top: 20px;
            right: 20px;
            z-index: 10;
        }
        #music-controls button {
            background: rgba(255, 255, 255, 0.5);
            border: none;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            font-size: 1.2rem;
            cursor: pointer;
            margin-left: 10px;
        }

        /* --- HIỆU ỨNG HẠT --- */
        .particle-container {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            overflow: hidden;
            z-index: 1;
        }

        /* Hạt loại 1: Trái tim/Hình tròn (Bay lên) */
        .particle {
            position: absolute;
            background-color: white;
            border-radius: 50%;
            opacity: 0.7;
            animation: float-up 20s infinite linear;
        }
        .particle.heart {
            background-color: transparent;
            font-size: 20px; /* Kích cỡ trái tim */
        }
        .particle.heart::before {
            content: '❤️';
            color: #ff4757;
        }
        .particle.circle {
            background-color: #ff9a9e;
            opacity: 0.5;
        }

        @keyframes float-up {
            0% {
                transform: translateY(100vh) scale(1);
                opacity: 0.7;
            }
            100% {
                transform: translateY(-10vh) scale(0.8);
                opacity: 0;
            }
        }
        
        /* Hạt loại 2: Đốm sáng (Lấp lánh) */
        .star {
            position: absolute;
            width: 2px;
            height: 2px;
            background: white;
            border-radius: 50%;
            box-shadow: 0 0 5px white;
            animation: twinkle 1.5s infinite alternate;
        }

        @keyframes twinkle {
            0% { opacity: 0.1; }
            100% { opacity: 1; }
        }
        /* --- KẾT THÚC HIỆU ỨNG HẠT --- */


        /* Bao thư */
        #envelope-wrapper {
            position: relative;
            z-index: 5;
            cursor: pointer;
        }

        #envelope {
            position: relative;
            width: 220px;
            height: 150px;
            background-color: var(--envelope-color);
            border-radius: 5px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
            transition: transform 0.5s;
        }
        #envelope-wrapper:hover #envelope {
            transform: scale(1.1);
        }

        .flap {
            position: absolute;
            width: 0;
            height: 0;
            border-left: 110px solid transparent;
            border-right: 110px solid transparent;
            border-top: 80px solid var(--envelope-flap-color);
            top: 0;
            left: 0;
            z-index: 3;
            transform-origin: top center;
            transition: transform 0.5s ease-in-out;
        }

        .letter {
            position: absolute;
            background-color: white;
            width: 200px;
            height: 130px;
            top: 10px;
            left: 10px;
            border-radius: 5px;
            z-index: 1;
            transition: transform 0.5s ease-in-out;
        }

        /* Hiệu ứng mở thư */
        #envelope-wrapper.open .flap {
            transform: rotateX(180deg);
            z-index: 0;
        }
        #envelope-wrapper.open .letter {
            transform: translateY(-80px);
        }

        /* 3. Thiệp (Modal) */
        #card-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            /* Nền tối được xử lý bởi body */
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 10; /* Phải thấp hơn z-index của thiệp */
            opacity: 0;
            transition: opacity 0.5s;
        }

        .card-content {
            background-color: var(--card-color);
            padding: 20px 30px;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
            width: 80%;
            max-width: 500px;
            text-align: center;
            position: relative;
            z-index: 101; /* Phải cao nhất */
            transform: scale(0.5);
            transition: transform 0.5s;
        }
        
        /* Hiệu ứng hiện thiệp */
        #card-modal:not(.hidden) {
            opacity: 1;
        }
        #card-modal:not(.hidden) .card-content {
            transform: scale(1);
        }

        #close-button {
            position: absolute;
            top: 10px;
            right: 15px;
            font-size: 2rem;
            color: #aaa;
            cursor: pointer;
            transition: color 0.2s;
        }
        #close-button:hover {
            color: #ff0000;
        }

        .card-content h2 {
            font-family: var(--font-cursive); /* Áp dụng font chữ */
            font-size: 3rem; /* Tăng kích cỡ font */
            font-weight: normal;
            color: #ff4757;
            margin-top: 10px;
            margin-bottom: 15px;
        }

        /* Hiệu ứng gõ chữ */
        #typing-text {
            font-size: 1.1rem;
            color: #333;
            min-height: 120px;
            border-right: 3px solid #333;
            animation: blinkCursor 0.7s infinite;
            white-space: pre-wrap;
            text-align: left;
            margin-bottom: 20px;
        }
        #typing-text.done {
            border-right: none;
            animation: none;
        }

        @keyframes blinkCursor {
            from { border-right-color: #333; }
            to { border-right-color: transparent; }
        }

        /* Danh sách lời chúc */
        .wishes-list {
            list-style: none;
            padding: 0;
            text-align: left;
            opacity: 0;
            transition: opacity 1s;
        }
        .wishes-list.visible {
            opacity: 1;
        }
        .wishes-list li {
            background-color: #f9f9f9;
            padding: 10px;
            border-radius: 5px;
            margin: 8px 0;
            display: flex;
            align-items: center;
        }
        .wishes-list li::before {
            content: '';
            font-size: 1.2rem;
            margin-right: 10px;
        }
        .wishes-list li:nth-child(1)::before { content: '🌸'; }
        .wishes-list li:nth-child(2)::before { content: '🌟'; }
        .wishes-list li:nth-child(3)::before { content: '💖'; }
        .wishes-list li:nth-child(4)::before { content: '☀️'; }

        .signature {
            text-align: right;
            margin-top: 20px;
            font-style: italic;
            color: #555;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

    </style>
</head>
<body>

    <div class="particle-container hidden" id="stars-container"></div>

    <div class="particle-container hidden" id="hearts-container"></div>

    <div id="loading-screen">
        <p>Đang chuẩn bị điều bất ngờ...</p>
    </div>

    <div id="start-screen" class="hidden">
        <h1>Happy Women's Day</h1>
        <p>Chạm để mở món quà đặc biệt</p>
        <button id="start-button">Bắt đầu</button>
    </div>

    <div id="main-content" class="hidden">
        <div id="music-controls">
            <button id="play-pause-btn">▶️</button>
            <button id="mute-btn">🔊</button>
        </div>
        <div id="envelope-wrapper">
            <div id="envelope">
                <div class="letter"></div>
                <div class="flap"></div>
            </div>
        </div>
    </div>

    <div id="card-modal" class="hidden">
        <div class="card-content">
            <span id="close-button">&times;</span>
            <h2>Happy Women's Day 2025! ❤️</h2>
            
            <p id="typing-text"></p>
            
            <ul class="wishes-list" id="wishes-list">
                <li>Chúc bạn luôn xinh đẹp, rạng ngời như những đóa hoa tươi thắm</li>
                <li>Chúc bạn thành công rực rỡ trên con đường sự nghiệp</li>
                <li>Chúc bạn tìm được hạnh phúc trọn vẹn trong tình yêu</li>
                <li>Chúc bạn luôn vui vẻ, tràn đầy năng lượng mỗi ngày</li>
            </ul>

            <p class="signature">
                With love and appreciation,<br>
                TIEDNEV
            </p>
        </div>
    </div>

    <audio id="bg-music" src="music.mp3" loop></audio>


    <script>
        // Lấy các phần tử
        const body = document.body;
        const loadingScreen = document.getElementById('loading-screen');
        const startScreen = document.getElementById('start-screen');
        const startButton = document.getElementById('start-button');
        const mainContent = document.getElementById('main-content');
        const envelopeWrapper = document.getElementById('envelope-wrapper');
        const cardModal = document.getElementById('card-modal');
        const closeButton = document.getElementById('close-button');
        const typingText = document.getElementById('typing-text');
        const wishesList = document.getElementById('wishes-list');
        const bgMusic = document.getElementById('bg-music');
        const playPauseBtn = document.getElementById('play-pause-btn');
        const muteBtn = document.getElementById('mute-btn');
        const heartsContainer = document.getElementById('hearts-container');
        const starsContainer = document.getElementById('stars-container');

        // Nội dung bức thư
        const letterContent = "Thân gửi người phụ nữ tuyệt vời,\n\nNhân dịp ngày Quốc tế Phụ nữ 20/10, tôi xin gửi đến bạn những lời chúc tốt đẹp nhất! Mỗi người phụ nữ là một bông hoa tuyệt đẹp, tô điểm cho cuộc sống này thêm rực rỡ.";
        let i = 0;
        let speed = 50; // Tốc độ gõ chữ

        // 1. Logic màn hình Loading
        setTimeout(() => {
            loadingScreen.classList.add('hidden');
            startScreen.classList.remove('hidden');
        }, 2000); // Hiện trong 2 giây

        // 2. Logic nút Bắt đầu
        startButton.addEventListener('click', () => {
            startScreen.classList.add('hidden');
            mainContent.classList.remove('hidden');
            body.style.background = 'var(--bg-main)'; // Đổi nền hồng nhạt
            
            // Bật nhạc
            bgMusic.play().catch(e => console.log(e));
            playPauseBtn.textContent = '⏸️';
            
            // Hiện hạt trái tim
            heartsContainer.classList.remove('hidden');
            if (heartsContainer.children.length === 0) {
                 createHeartParticles();
            }
        });

        // 3. Logic mở thư
        envelopeWrapper.addEventListener('click', () => {
            if (envelopeWrapper.classList.contains('open')) return;
            
            envelopeWrapper.classList.add('open');
            body.style.background = 'var(--bg-dark)'; // Đổi nền tối
            
            // Ẩn hạt trái tim, hiện hạt lấp lánh
            heartsContainer.classList.add('hidden');
            starsContainer.classList.remove('hidden');
            if (starsContainer.children.length === 0) {
                createStarParticles();
            }

            setTimeout(() => {
                cardModal.classList.remove('hidden');
                
                // Bắt đầu gõ chữ
                typingText.innerHTML = '';
                wishesList.classList.remove('visible');
                typingText.classList.remove('done');
                i = 0;
                typeWriter();
            }, 1000);
        });
        
        // 4. Logic gõ chữ
        function typeWriter() {
            if (i < letterContent.length) {
                typingText.innerHTML += letterContent.charAt(i);
                i++;
                setTimeout(typeWriter, speed);
            } else {
                typingText.classList.add('done');
                wishesList.classList.add('visible');
            }
        }

        // 5. Logic đóng thiệp
        closeButton.addEventListener('click', () => {
            cardModal.classList.add('hidden');
            envelopeWrapper.classList.remove('open');
            body.style.background = 'var(--bg-main)'; // Trở về nền hồng nhạt
            
            // Ẩn hạt lấp lánh, hiện lại hạt trái tim
            starsContainer.classList.add('hidden');
            heartsContainer.classList.remove('hidden');
        });

        // 6. Logic điều khiển nhạc
        playPauseBtn.addEventListener('click', () => {
            if (bgMusic.paused) {
                bgMusic.play();
                playPauseBtn.textContent = '⏸️';
            } else {
                bgMusic.pause();
                playPauseBtn.textContent = '▶️';
            }
        });

        muteBtn.addEventListener('click', () => {
            bgMusic.muted = !bgMusic.muted;
            muteBtn.textContent = bgMusic.muted ? '🔇' : '🔊';
        });
        
        // 7. Logic tạo hạt bay (Tim/Tròn)
        function createHeartParticles() {
            for (let i = 0; i < 30; i++) {
                const particle = document.createElement('div');
                particle.classList.add('particle');
                
                const type = Math.random();
                if (type > 0.66) {
                    particle.classList.add('heart');
                } else if (type > 0.33) {
                     particle.classList.add('circle');
                     const size = Math.random() * 10 + 5;
                     particle.style.width = `${size}px`;
                     particle.style.height = `${size}px`;
                } else {
                     // Hạt tròn trắng nhỏ hơn
                     const size = Math.random() * 5 + 2;
                     particle.style.width = `${size}px`;
                     particle.style.height = `${size}px`;
                }
                
                particle.style.left = `${Math.random() * 100}vw`;
                particle.style.animationDuration = `${Math.random() * 10 + 10}s`;
                particle.style.animationDelay = `${Math.random() * 10}s`;
                
                heartsContainer.appendChild(particle);
            }
        }

        // 8. Logic tạo hạt (Lấp lánh)
        function createStarParticles() {
            for (let i = 0; i < 50; i++) {
                const star = document.createElement('div');
                star.classList.add('star');
                star.style.left = `${Math.random() * 100}vw`;
                star.style.top = `${Math.random() * 100}vh`;
                star.style.animationDelay = `${Math.random() * 1.5}s`;
                starsContainer.appendChild(star);
            }
        }

    </script>
</body>
</html>
