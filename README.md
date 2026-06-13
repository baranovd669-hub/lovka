#<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Для Ирочки ❤️</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
            -webkit-tap-highlight-color: transparent;
        }

        html, body {
            margin: 0;
            padding: 0;
            height: 100%;
            width: 100%;
        }

        body {
            background: linear-gradient(145deg, #ff9a9e 0%, #fad0c4 50%, #fad0c4 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            font-family: 'Segoe UI', 'Quicksand', system-ui, -apple-system, cursive;
            position: relative;
            overflow-x: hidden;
            padding: 20px;
        }

        /* Контейнер для конверта — ВЕСЬ кликабельный */
        .envelope-container {
            perspective: 1200px;
            z-index: 10;
            cursor: pointer;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }

        .envelope {
            width: 320px;
            height: 220px;
            background-color: #fff5e6;
            border-radius: 12px;
            position: relative;
            box-shadow: 0 20px 35px rgba(0,0,0,0.2), 0 0 0 1px rgba(255,215,175,0.5);
            transition: transform 0.2s ease;
            transform-style: preserve-3d;
            cursor: pointer;
        }

        /* Клапан конверта */
        .envelope-flap {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: #ffddb0;
            clip-path: polygon(0% 0%, 100% 0%, 50% 55%);
            border-radius: 12px 12px 0 0;
            transition: transform 0.7s cubic-bezier(0.23, 1, 0.32, 1);
            transform-origin: top;
            z-index: 3;
            box-shadow: 0 2px 8px rgba(0,0,0,0.05);
            cursor: pointer;
        }

        .envelope:before, .envelope:after {
            content: "";
            position: absolute;
            bottom: 0;
            width: 0;
            height: 0;
            border-style: solid;
            z-index: 2;
        }

        .envelope:before {
            left: 0;
            border-width: 0 0 60px 60px;
            border-color: transparent transparent #e6c8a8 transparent;
        }

        .envelope:after {
            right: 0;
            border-width: 0 60px 60px 0;
            border-color: transparent transparent #e6c8a8 transparent;
        }

        /* Сердечко на конверте */
        .heart-on-envelope {
            position: absolute;
            top: 55%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 48px;
            z-index: 10;
            pointer-events: none;
            text-shadow: 0 0 8px rgba(255,80,80,0.5);
            animation: heartbeat 1.4s ease infinite;
        }

        @keyframes heartbeat {
            0%, 100% { transform: translate(-50%, -50%) scale(1); opacity: 0.9; }
            50% { transform: translate(-50%, -50%) scale(1.2); opacity: 1; text-shadow: 0 0 18px #ff3366; }
        }

        /* Письмо внутри */
        .letter {
            position: absolute;
            bottom: 18px;
            left: 18px;
            right: 18px;
            background: #fffef7;
            border-radius: 8px;
            padding: 15px 12px;
            font-size: 18px;
            text-align: center;
            color: #b34e4e;
            font-weight: bold;
            font-family: 'Segoe UI', 'Quicksand', cursive;
            transition: all 0.5s ease;
            opacity: 0;
            transform: translateY(30px);
            z-index: 1;
            box-shadow: 0 6px 12px rgba(0,0,0,0.1);
            pointer-events: none;
        }

        .letter p {
            font-size: 22px;
            letter-spacing: 1px;
        }

        .letter small {
            font-size: 14px;
            color: #e08e8e;
            display: block;
            margin-top: 8px;
        }

        /* Открытый конверт */
        .envelope.open .envelope-flap {
            transform: rotateX(180deg);
        }

        .envelope.open .letter {
            opacity: 1;
            transform: translateY(0);
            transition-delay: 0.3s;
        }

        .envelope.open .heart-on-envelope {
            opacity: 0;
            transition: opacity 0.2s;
            animation: none;
        }

        .hint {
            text-align: center;
            margin-top: 40px;
            color: #9b4b4b;
            font-weight: 500;
            background: rgba(255,245,235,0.7);
            padding: 8px 20px;
            border-radius: 60px;
            backdrop-filter: blur(4px);
            font-size: 16px;
            letter-spacing: 0.5px;
            cursor: pointer;
        }

        /* Стили для падающих сердечек */
        .heart-fall {
            position: fixed;
            top: -20px;
            font-size: 24px;
            pointer-events: none;
            z-index: 9999;
            animation: fall linear forwards;
            opacity: 0.9;
        }

        @keyframes fall {
            0% {
                transform: translateY(0) rotate(0deg);
                opacity: 1;
            }
            100% {
                transform: translateY(100vh) rotate(360deg);
                opacity: 0;
            }
        }

        @media (max-width: 480px) {
            .envelope { width: 280px; height: 190px; }
            .heart-on-envelope { font-size: 42px; }
            .letter p { font-size: 18px; }
            .heart-fall { font-size: 20px; }
        }
    </style>
</head>
<body>

<div class="envelope-container" id="envelopeContainer">
    <div class="envelope" id="loveEnvelope">
        <div class="envelope-flap"></div>
        <div class="heart-on-envelope">❤️</div>
        <div class="letter">
            <p>✨ Я ЛЮБЛЮ ТЕБЯ, ИРОЧКА ✨</p>
            <small>💌 просто так, от всего сердца 💌</small>
        </div>
    </div>
    <div class="hint" id="hintText">
        💖 Нажми на конверт — и пойдёт сердцепад 💖
    </div>
</div>

<script>
    const envelope = document.getElementById('loveEnvelope');
    const container = document.getElementById('envelopeContainer');
    let isOpened = false;

    // Функция для создания падающего сердечка
    function createFallingHeart() {
        const heart = document.createElement('div');
        heart.className = 'heart-fall';
        
        const heartsArray = ['❤️', '💖', '💗', '💓', '💕', '💞', '❣️', '🧡', '💛', '💚', '💙', '💜'];
        heart.innerText = heartsArray[Math.floor(Math.random() * heartsArray.length)];
        
        const size = 18 + Math.random() * 24;
        heart.style.fontSize = size + 'px';
        heart.style.left = Math.random() * 100 + '%';
        
        const duration = 2 + Math.random() * 3;
        heart.style.animationDuration = duration + 's';
        heart.style.animationDelay = Math.random() * 0.5 + 's';
        
        document.body.appendChild(heart);
        
        setTimeout(() => {
            if (heart.parentNode) heart.remove();
        }, duration * 1000);
    }

    // Функция открытия конверта
    function openEnvelope(event) {
        event.stopPropagation();
        
        if (!envelope.classList.contains('open')) {
            envelope.classList.add('open');
            isOpened = true;
            
            if (navigator.vibrate) navigator.vibrate(50);
            
            // Запуск сердечек
            for (let i = 0; i < 50; i++) {
                setTimeout(() => {
                    createFallingHeart();
                }, i * 80);
            }
            
            setTimeout(() => {
                let extraHearts = 0;
                const extraInterval = setInterval(() => {
                    if (extraHearts < 20) {
                        createFallingHeart();
                        extraHearts++;
                    } else {
                        clearInterval(extraInterval);
                    }
                }, 200);
            }, 4000);
        }
    }

    // ДЛЯ ТЕЛЕФОНОВ: слушаем и touch, и click
    envelope.addEventListener('click', openEnvelope);
    envelope.addEventListener('touchstart', openEnvelope, { passive: false });
    container.addEventListener('click', function(e) {
        // Если кликнули на контейнер, но не на конверт — всё равно открываем
        if (!envelope.classList.contains('open')) {
            openEnvelope(e);
        }
    });
    
    // Для подсказки тоже
    const hint = document.getElementById('hintText');
    hint.addEventListener('click', openEnvelope);
    hint.addEventListener('touchstart', openEnvelope, { passive: false });
</script>

</body>
</html>