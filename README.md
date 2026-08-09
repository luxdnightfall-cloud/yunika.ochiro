<!DOCTYPE html>

<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hysilens - Abyssal Reverie</title>

    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;600&family=Cormorant+Garamond:ital,wght@0,500;1,400&family=Inter:wght@300;400&display=swap" rel="stylesheet">

    <style>
        :root {
            --deep-violet: #6d5a96;
            --mid-violet: #8a79b7;
            --text-soft: #5c4e7d;
            --sea-shadow: #7ca7b7;
        }

        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
        body, html {
            margin: 0; padding: 0; height: 100%;
            display: flex; justify-content: center; align-items: center;
            background: radial-gradient(circle at top, #e8f6ff, #f0ecfa 45%, #e7f5f7 100%);
            overflow: hidden; font-family: 'Inter', sans-serif;
        }

        .card-wrapper {
            position: relative;
            width: 900px;
            height: 500px;
            background: linear-gradient(135deg, #fbf9ff 0%, #f1ecfa 45%, #eaf7f8 100%);
            border: 1px solid rgba(126, 109, 167, 0.14);
            box-shadow: 0 25px 60px rgba(86, 71, 126, 0.1);
        }

        .bg-name {
            position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);
            font-family: 'Cormorant Garamond', serif; font-size: 190px; font-weight: 600;
            color: #d8dbf3; opacity: 0.3; white-space: nowrap; letter-spacing: -4px; z-index: 1;
        }

        .character-overlay {
            position: absolute; height: 130%; z-index: 10; bottom: -12%; left: 50%;
            transform: translateX(-50%); pointer-events: none;
            filter: drop-shadow(0 20px 38px rgba(88, 66, 130, 0.15));
            animation: floatCharacter 6s ease-in-out infinite;
        }

        /* --- gelembung musik --- */
        .bubble {
            position: absolute; border-radius: 50%;
            background: linear-gradient(135deg, rgba(255,255,255,0.7), rgba(194,229,233,0.2));
            backdrop-filter: blur(2px);
            border: 1px solid rgba(255,255,255,0.5);
            z-index: 5;
            animation: bubbleFloat 8s ease-in-out infinite;
            transition: background 0.5s ease, box-shadow 0.5s ease, transform 0.3s ease;
        }

        /* reaksi terhadap musik */
        .card-wrapper.playing .bubble {
            background: linear-gradient(135deg, rgba(138, 121, 183, 0.4), rgba(109, 90, 150, 0.2));
            box-shadow: 0 0 15px rgba(138, 121, 183, 0.3);
            animation: bubbleFloat 4s ease-in-out infinite, bubblePulse 2s ease-in-out infinite;
        }

        @keyframes bubblePulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.1); }
        }

        .intro-text {
            position: absolute; 
            right: 40px; 
            bottom: 70px; 
            z-index: 15;
            display: inline-block;
            width: auto; 
            padding: 12px 25px;
            background: rgba(255, 255, 255, 0.2);
            backdrop-filter: blur(20px) saturate(120%);
            -webkit-backdrop-filter: blur(20px) saturate(120%);
            border-radius: 40px 40px 0px 40px;
            border: 1px solid rgba(255, 255, 255, 0.4);
            box-shadow: 0 10px 30px rgba(109, 90, 150, 0.1);
            font-family: 'Cormorant Garamond', serif;
            font-size: 16px;
            line-height: 1.4;
            font-style: italic;
            color: #5A4678 !important;
            text-shadow: 0 1px 4px rgba(255, 255, 255, 0.25);
            user-select: none;
            white-space: nowrap;
        }

/* TIGA TOMBOL POP-UP DI SISI KIRI   */
.popup-buttons {
    position: absolute;
    left: 25px;
    top: 50%;
    transform: translateY(-50%);

    display: flex;
    flex-direction: column;
    gap: 12px;

    z-index: 20;
}

/* Tombol */
.popup-button {
    width: 100px;
    padding: 9px 10px;

    border: 1px solid rgba(109, 90, 150, 0.3);
    border-radius: 20px;

    background: rgba(255, 255, 255, 0.25);
    backdrop-filter: blur(10px);
    -webkit-backdrop-filter: blur(10px);

    color: var(--deep-violet);
    font-family: 'Cinzel', serif;
    font-size: 10px;
    letter-spacing: 1px;

    cursor: pointer;
    transition: 0.3s ease;
}

.popup-button:hover {
    background: rgba(255, 255, 255, 0.6);
    transform: translateX(5px);
}

  /* overlay popup tengah */    
.popup-overlay {
    position: absolute;
    inset: 0;

    display: none;
    justify-content: center;
    align-items: center;

    background: rgba(40, 30, 60, 0.2);

    backdrop-filter: blur(8px);
    -webkit-backdrop-filter: blur(8px);

    z-index: 100;
}

.popup-overlay.active {
    display: flex;
}



/*  KOTAK ISI POP-UP   */
.popup-box {
    width: 330px;
    max-width: 75%;

    padding: 25px;

    background: rgba(255, 255, 255, 0.45);

    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);

    border: 1px solid rgba(255, 255, 255, 0.6);
    border-radius: 20px;

    box-shadow: 0 20px 50px rgba(80, 60, 120, 0.2);

    text-align: center;
    color: #000080;
}

.popup-box h2 {
    margin: 0 0 12px;

    font-family: 'Cinzel', serif;
    font-size: 20px;
}

.popup-box p {
    margin: 0;

    font-family: 'Cormorant Garamond', serif;
    font-size: 16px;
    line-height: 1.3;
}



/* Tombol CLOSE */
.popup-close {
    margin-top: 18px;

    padding: 7px 20px;

    border: 1px solid rgba(109, 90, 150, 0.3);
    border-radius: 20px;

    background: rgba(255, 255, 255, 0.35);

    color: var(--deep-violet);

    cursor: pointer;
    font-family: 'Cinzel', serif;
}

.popup-close:hover {
    background: rgba(255, 255, 255, 0.7);
}

        .top-label {
            position: absolute; top: 25px; left: 30px;
            font-family: 'Cinzel', serif; font-size: 11px;
            color: var(--sea-shadow); letter-spacing: 4px;
            text-transform: uppercase; z-index: 7;
        }

        .main-title {
            position: absolute; top: 15%; width: 100%;
            text-align: center; font-family: 'Cinzel', serif;
            font-size: 12px; letter-spacing: 12px;
            color: var(--deep-violet); z-index: 7;
        }

        .footer-note {
            position: absolute; bottom: 20px; right: 40px;
            font-family: 'Cormorant Garamond', serif;
            font-size: 13px; font-style: italic;
            color: var(--sea-shadow); opacity: 0.8; z-index: 7;
        }

        .abyssal-player-zone {
            position: absolute; bottom: 15px; left: 25px; z-index: 20;
            display: flex; align-items: center; gap: 8px; padding: 10px;
        }

        .play-trigger {
            font-size: 16px; color: var(--mid-violet); cursor: pointer;
        }

        .track-name-display {
            font-family: 'Cormorant Garamond', serif;
            font-size: 15px; color: var(--deep-violet);
        }

        @keyframes floatCharacter {
            0%, 100% { transform: translateX(-50%) translateY(0); }
            50% { transform: translateX(-50%) translateY(-15px); }
        }

        @keyframes bubbleFloat {
            0%, 100% { transform: translate(0, 0); }
            50% { transform: translate(10px, -20px); }
        }

        audio { display: none; }
    </style>
</head>
<body>

    <div class="card-wrapper" id="main-card">


<!-- popup kiri -->
<div class="popup-buttons">

    <button class="popup-button" onclick="openPopup('info')">
        INFO
    </button>

    <button class="popup-button" onclick="openPopup('story')">
        STORY
    </button>

    <button class="popup-button" onclick="openPopup('data')">
        catatan
    </button>

</div>

      
<!-- POP-UP INFO   -->
<div class="popup-overlay" id="info-popup">

    <div class="popup-box">

        <h2>INFO</h2>

        <p>
            perkenalkan saya adalah aethelr, saya seorang KOMANDAN dari kapal induk ENTERPRISE dari kelas Yorktown
        </p>

        <button class="popup-close" onclick="closePopup()">
            CLOSE
        </button>

    </div>

</div>


<!-- POP-UP STORY     -->
<div class="popup-overlay" id="story-popup">

    <div class="popup-box">

        <h2>STORY</h2>

        <p>
            1945 merupakan tahun terberat ku...., mungki aku akan dimarahi karena mengeluh, tapi percayalah... tahun ini sangat berat untukku. Sepertinya kita harus mundur ke 4 tahun yang lalu, dimana perintah itu muncul...  (data tidak ditemukan)
        </p>

        <button class="popup-close" onclick="closePopup()">
            CLOSE
        </button>

    </div>

</div>


<!-- POP-UP DATA  -->
<div class="popup-overlay" id="data-popup">

    <div class="popup-box">

        <h2>CATATAN</h2>

        <p>
            sepertinya aneh menaruh cerita seperti itu diawal, tapi ya sudahlah. Pertama-tama, perkenalkan saya krishna yang sebagian besar membuat proyek ini. ini merupakan website pertamaku dengan desain ke 3, jika kalia bertanya kenapa ini desain ketiga... jawabannya itu karena desain 1 dan 2 itu sebagai bahan eksperimenku, seharusnya itu menjawabkan?. baiklah sampai sini dulu karena waktuku tidak banyak, salam hangat _krishna_ 
        </p>

        <button class="popup-close" onclick="closePopup()">
            CLOSE
        </button>

    </div>

</div>
      
        <div class="bg-name">ENTERPRISE</div>
        <div class="top-label">ENTERPRISE</div>
        <div class="main-title">THE SAME · ELEGANT · FIRM</div>

        <img src="https://i.ibb.co.com/BHBcj9Kt/Enterprise.png" class="character-overlay">

        <div class="bubble" style="width:40px; height:40px; top:15%; left:10%; animation-delay: 0s;"></div>
        <div class="bubble" style="width:60px; height:60px; top:70%; left:8%; opacity: 0.3; animation-delay: 2s;"></div>
        <div class="bubble" style="width:30px; height:30px; top:20%; right:15%; animation-delay: 1s;"></div>
        <div class="bubble" style="width:20px; height:20px; top:45%; left:25%; animation-delay: 3s;"></div>
        <div class="bubble" style="width:50px; height:50px; bottom:15%; left:40%; animation-delay: 5s; opacity: 0.2;"></div>
        <div class="bubble" style="width:35px; height:35px; top:10%; right:35%; animation-delay: 4s;"></div>
        <div class="bubble" style="width:15px; height:15px; bottom:30%; right:10%; animation-delay: 1.5s;"></div>
        <div class="bubble" style="width:45px; height:45px; top:60%; right:25%; animation-delay: 6s; opacity: 0.4;"></div>

        <div class="intro-text">
           KAPAL INDUK KELAS YORKTOWN<br>
        </div>

        <div class="footer-note">by OCHIRO YUNIKA</div>

        <div class="abyssal-player-zone">
            <div class="play-trigger" id="master-btn" onclick="togglePlay()">▶</div>
            <span class="track-name-display">play - song</span>
        </div>

        <audio id="audio-core">
            <source src="https://www.image2url.com/r2/default/audio/1786122495602-df72d485-5cfa-4f2b-b34d-4389724e7634.mp3" type="audio/mpeg">
        </audio>
    </div>

    <script>
        const audio = document.getElementById('audio-core');
        const btn = document.getElementById('master-btn');
        const card = document.getElementById('main-card');

    
// SISTEM TIGA POP-UP
function openPopup(type) {
    document.querySelectorAll('.popup-overlay')
        .forEach(popup => {
            popup.classList.remove('active');
        });

    const popup = document.getElementById(type + '-popup');

    if (popup) {
        popup.classList.add('active');
    }
}

function closePopup() {
    document.querySelectorAll('.popup-overlay')
        .forEach(popup => {
            popup.classList.remove('active');
        });
}

        function togglePlay() {
            if (audio.paused) {
                audio.play();
                btn.innerHTML = "||";
                card.classList.add('playing'); // mengaktifkan efek gelembung dan musik
            } else {
                audio.pause();
                btn.innerHTML = "▶";
                card.classList.remove('playing'); // lagu mati
            }
        }

        // efek saat lagu selesai
        audio.onended = () => {
            btn.innerHTML = "▶";
            card.classList.remove('playing');
        };
    </script>
</body>
</html>

