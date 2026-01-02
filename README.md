[test6.html](https://github.com/user-attachments/files/24408920/test6.html)
<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>$DUNKBALL - Solana Basketball Memecoin</title>
    <!-- Fonty -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800;900&family=Rubik+Wet+Paint&display=swap" rel="stylesheet">
    <!-- Ikony -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- DexScreener Embed Script -->
    <script src="https://dexscreener.com/dex-widget/embed/script.js"></script>
    <style>
        /* Reset i podstawy */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', sans-serif;
        }
        
        :root {
            --primary: #00FFA3;
            --secondary: #14F195;
            --accent: #9945FF;
            --dark: #0A0A0A;
            --light: #FFFFFF;
            --gray: #1A1A1A;
            --orange: #FF6B00;
            --blue: #03E1FF;
        }
        
        body {
            background-color: var(--dark);
            color: var(--light);
            overflow-x: hidden;
            cursor: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 32 32"><circle cx="16" cy="16" r="14" fill="%2300FFA3" opacity="0.7"/><circle cx="16" cy="16" r="8" fill="%239945FF"/></svg>') 16 16, auto;
            /* MEMOWE TŁO */
            background-image: url('https://i.imgur.com/0JzGqQZ.png'), url('https://i.imgur.com/VfKPQf5.png');
            background-size: 300px, 200px;
            background-position: left top, right bottom;
            background-repeat: no-repeat, no-repeat;
            background-attachment: fixed;
            position: relative;
        }
        
        /* Overlay dla czytelności */
        body::before {
            content: '';
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, rgba(10, 10, 10, 0.9) 0%, rgba(10, 10, 10, 0.85) 100%);
            z-index: -2;
        }
        
        /* Pływające memy w tle */
        .floating-meme {
            position: fixed;
            z-index: -1;
            opacity: 0.15;
            pointer-events: none;
        }
        
        .meme-1 {
            top: 10%;
            left: 5%;
            width: 150px;
            animation: float 25s infinite linear;
        }
        
        .meme-2 {
            top: 60%;
            right: 8%;
            width: 120px;
            animation: float 30s infinite linear reverse;
        }
        
        .meme-3 {
            bottom: 15%;
            left: 10%;
            width: 100px;
            animation: float 35s infinite linear;
        }
        
        .meme-4 {
            top: 20%;
            right: 15%;
            width: 180px;
            animation: float 40s infinite linear reverse;
        }
        
        @keyframes float {
            0% { transform: translateY(0px) rotate(0deg); }
            25% { transform: translateY(-20px) rotate(90deg); }
            50% { transform: translateY(0px) rotate(180deg); }
            75% { transform: translateY(20px) rotate(270deg); }
            100% { transform: translateY(0px) rotate(360deg); }
        }
        
        /* Efekt śladu kursora */
        .cursor-trail {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            pointer-events: none;
            z-index: 9998;
        }
        
        .trail-dot {
            position: absolute;
            width: 6px;
            height: 6px;
            border-radius: 50%;
            background: var(--dot-color);
            animation: trailFade 0.8s forwards;
        }
        
        @keyframes trailFade {
            0% {
                opacity: 1;
                transform: scale(1) translateY(0);
            }
            100% {
                opacity: 0;
                transform: scale(0.5) translateY(-20px);
            }
        }
        
        /* Kontrolki dźwięku */
        .audio-controls {
            position: fixed;
            bottom: 20px;
            left: 20px;
            z-index: 9999;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }
        
        .audio-btn {
            width: 50px;
            height: 50px;
            border-radius: 50%;
            background: var(--primary);
            color: var(--dark);
            border: none;
            cursor: pointer;
            font-size: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
            box-shadow: 0 4px 15px rgba(0, 255, 163, 0.3);
        }
        
        .audio-btn:hover {
            transform: scale(1.1);
            box-shadow: 0 6px 20px rgba(0, 255, 163, 0.5);
        }
        
        .audio-btn.active {
            background: var(--accent);
        }
        
        .volume-slider-container {
            background: rgba(0, 0, 0, 0.8);
            padding: 15px;
            border-radius: 25px;
            display: flex;
            align-items: center;
            gap: 10px;
            transform: translateX(-10px);
            opacity: 0;
            transition: all 0.3s;
            pointer-events: none;
        }
        
        .volume-slider-container.show {
            opacity: 1;
            transform: translateX(0);
            pointer-events: all;
        }
        
        .volume-slider {
            width: 100px;
            height: 6px;
            -webkit-appearance: none;
            appearance: none;
            background: var(--gray);
            border-radius: 3px;
            outline: none;
        }
        
        .volume-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 20px;
            height: 20px;
            border-radius: 50%;
            background: var(--primary);
            cursor: pointer;
        }
        
        /* Kontener główny */
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 20px;
        }
        
        /* Nagłówek */
        header {
            padding: 20px 0;
            border-bottom: 2px solid var(--gray);
            background: rgba(10, 10, 10, 0.9);
            backdrop-filter: blur(10px);
            position: sticky;
            top: 0;
            z-index: 1000;
        }
        
        .nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 15px;
        }
        
        .logo-icon {
            width: 50px;
            height: 50px;
            background: linear-gradient(45deg, var(--primary), var(--accent));
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            font-weight: bold;
            color: var(--dark);
            animation: bounce 2s infinite;
        }
        
        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-10px); }
        }
        
        .logo-text h1 {
            font-family: 'Rubik Wet Paint', cursive;
            font-size: 32px;
            background: linear-gradient(45deg, var(--primary), var(--accent));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 30px rgba(0, 255, 163, 0.3);
        }
        
        .logo-text p {
            font-size: 14px;
            color: #aaa;
        }
        
        .nav-links {
            display: flex;
            gap: 30px;
            align-items: center;
        }
        
        .nav-links a {
            color: var(--light);
            text-decoration: none;
            font-weight: 500;
            transition: color 0.3s;
            position: relative;
        }
        
        .nav-links a:hover {
            color: var(--primary);
        }
        
        .nav-links a::after {
            content: '';
            position: absolute;
            bottom: -5px;
            left: 0;
            width: 0;
            height: 2px;
            background: var(--primary);
            transition: width 0.3s;
        }
        
        .nav-links a:hover::after {
            width: 100%;
        }
        
        .nav-buttons {
            display: flex;
            gap: 15px;
        }
        
        .btn {
            padding: 12px 24px;
            border-radius: 50px;
            font-weight: 600;
            text-decoration: none;
            transition: all 0.3s;
            border: none;
            cursor: pointer;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
        }
        
        .btn-primary {
            background: linear-gradient(45deg, var(--primary), var(--secondary));
            color: var(--dark);
        }
        
        .btn-primary:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 20px rgba(0, 255, 163, 0.3);
        }
        
        .btn-secondary {
            background: transparent;
            color: var(--primary);
            border: 2px solid var(--primary);
        }
        
        .btn-secondary:hover {
            background: var(--primary);
            color: var(--dark);
        }
        
        /* Sekcja hero */
        .hero {
            padding: 100px 0;
            text-align: center;
            position: relative;
            overflow: hidden;
        }
        
        .hero-bg {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at 30% 20%, rgba(20, 241, 149, 0.1) 0%, transparent 50%),
                        radial-gradient(circle at 70% 80%, rgba(153, 69, 255, 0.1) 0%, transparent 50%);
            z-index: -1;
        }
        
        .hero h2 {
            font-size: 72px;
            font-family: 'Rubik Wet Paint', cursive;
            margin-bottom: 20px;
            background: linear-gradient(45deg, var(--primary), var(--accent), var(--blue));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 50px rgba(0, 255, 163, 0.5);
        }
        
        .hero p {
            font-size: 24px;
            max-width: 800px;
            margin: 0 auto 40px;
            color: #ccc;
            line-height: 1.6;
        }
        
        .hero-stats {
            display: flex;
            justify-content: center;
            gap: 50px;
            margin-top: 60px;
            flex-wrap: wrap;
        }
        
        .stat {
            text-align: center;
        }
        
        .stat-value {
            font-size: 42px;
            font-weight: 800;
            color: var(--primary);
            display: block;
        }
        
        .stat-label {
            font-size: 16px;
            color: #aaa;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        /* Sekcja tokenomics */
        .section {
            padding: 80px 0;
        }
        
        .section-title {
            font-size: 48px;
            text-align: center;
            margin-bottom: 40px;
            font-family: 'Rubik Wet Paint', cursive;
            color: var(--light);
            text-shadow: 0 0 20px rgba(0, 255, 163, 0.3);
        }
        
        .section-title span {
            color: var(--primary);
        }
        
        .tokenomics-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 30px;
        }
        
        .token-card {
            background: rgba(26, 26, 26, 0.8);
            border-radius: 20px;
            padding: 30px;
            border: 2px solid transparent;
            transition: all 0.3s;
            backdrop-filter: blur(10px);
        }
        
        .token-card:hover {
            border-color: var(--primary);
            transform: translateY(-10px);
            box-shadow: 0 15px 30px rgba(0, 255, 163, 0.2);
        }
        
        .token-icon {
            width: 70px;
            height: 70px;
            background: linear-gradient(45deg, var(--primary), var(--accent));
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 30px;
            margin-bottom: 20px;
        }
        
        .token-title {
            font-size: 24px;
            margin-bottom: 15px;
            color: var(--light);
        }
        
        .token-value {
            font-size: 36px;
            font-weight: 800;
            color: var(--primary);
            margin-bottom: 10px;
        }
        
        .token-desc {
            color: #ccc;
            line-height: 1.6;
        }
        
        /* Kontrakt */
        .contract-section {
            background: linear-gradient(135deg, rgba(20, 241, 149, 0.1), rgba(153, 69, 255, 0.1));
            border-radius: 20px;
            padding: 40px;
            margin: 50px 0;
            text-align: center;
            backdrop-filter: blur(10px);
        }
        
        .contract-title {
            font-size: 24px;
            margin-bottom: 20px;
            color: var(--light);
        }
        
        .contract-address {
            background: rgba(0, 0, 0, 0.5);
            padding: 15px 25px;
            border-radius: 50px;
            font-family: monospace;
            font-size: 18px;
            color: var(--primary);
            border: 2px solid var(--primary);
            display: inline-block;
            margin-bottom: 20px;
            word-break: break-all;
        }
        
        .copy-btn {
            background: var(--primary);
            color: var(--dark);
            border: none;
            padding: 10px 20px;
            border-radius: 50px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .copy-btn:hover {
            transform: scale(1.05);
        }
        
        /* Wykres */
        .chart-section {
            padding: 40px 0;
        }
        
        .chart-container {
            background: rgba(26, 26, 26, 0.8);
            border-radius: 20px;
            padding: 20px;
            margin: 30px 0;
            border: 2px solid var(--primary);
            overflow: hidden;
            backdrop-filter: blur(10px);
        }
        
        .chart-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            flex-wrap: wrap;
            gap: 15px;
        }
        
        .chart-title {
            font-size: 28px;
            color: var(--light);
            font-weight: 700;
        }
        
        .chart-title span {
            color: var(--primary);
        }
        
        .interval-selector {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }
        
        .interval-btn {
            padding: 8px 16px;
            background: rgba(255, 255, 255, 0.1);
            color: var(--light);
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 500;
        }
        
        .interval-btn:hover {
            background: rgba(0, 255, 163, 0.2);
        }
        
        .interval-btn.active {
            background: var(--primary);
            color: var(--dark);
            font-weight: 700;
        }
        
        /* Kontener dla widgetu DexScreener */
        #dex-widget {
            width: 100%;
            height: 600px;
            border-radius: 12px;
            overflow: hidden;
            margin: 0 auto;
        }
        
        .widget-container {
            position: relative;
            width: 100%;
            height: 100%;
        }
        
        /* Informacje o parze */
        .pair-info {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: rgba(0, 0, 0, 0.3);
            padding: 15px 20px;
            border-radius: 12px;
            margin-top: 20px;
            flex-wrap: wrap;
            gap: 15px;
            backdrop-filter: blur(10px);
        }
        
        .pair-detail {
            text-align: center;
        }
        
        .pair-label {
            font-size: 14px;
            color: #aaa;
            margin-bottom: 5px;
        }
        
        .pair-value {
            font-size: 20px;
            font-weight: 700;
            color: var(--primary);
        }
        
        .pair-value.positive {
            color: #00FFA3;
        }
        
        .pair-value.negative {
            color: #FF4757;
        }
        
        /* Social */
        .social-section {
            text-align: center;
            padding: 50px 0;
        }
        
        .social-links {
            display: flex;
            justify-content: center;
            gap: 20px;
            margin-top: 30px;
            flex-wrap: wrap;
        }
        
        .social-link {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            background: var(--gray);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            color: var(--light);
            text-decoration: none;
            transition: all 0.3s;
        }
        
        .social-link:hover {
            transform: translateY(-5px);
            background: var(--primary);
            color: var(--dark);
        }
        
        /* Stopka */
        footer {
            padding: 50px 0;
            border-top: 2px solid var(--gray);
            text-align: center;
            background: rgba(10, 10, 10, 0.9);
            backdrop-filter: blur(10px);
        }
        
        .footer-logo {
            font-size: 32px;
            font-family: 'Rubik Wet Paint', cursive;
            margin-bottom: 20px;
            color: var(--primary);
        }
        
        .disclaimer {
            max-width: 800px;
            margin: 30px auto;
            padding: 20px;
            background: rgba(255, 0, 0, 0.1);
            border-radius: 10px;
            border-left: 4px solid red;
            color: #ff9999;
            font-size: 14px;
        }
        
        /* Responsywność */
        @media (max-width: 768px) {
            .nav {
                flex-direction: column;
                gap: 20px;
            }
            
            .hero h2 {
                font-size: 48px;
            }
            
            .hero p {
                font-size: 18px;
            }
            
            .section-title {
                font-size: 36px;
            }
            
            #dex-widget {
                height: 500px;
            }
            
            .chart-header {
                flex-direction: column;
                align-items: flex-start;
            }
            
            .audio-controls {
                bottom: 10px;
                left: 10px;
            }
            
            .floating-meme {
                display: none; /* Ukryj pływające memy na mobile */
            }
        }
        
        @media (max-width: 480px) {
            #dex-widget {
                height: 400px;
            }
            
            .hero h2 {
                font-size: 36px;
            }
            
            .stat-value {
                font-size: 32px;
            }
        }
    </style>
</head>
<body>
    <!-- Pływające memy w tle -->
    <img src="https://i.imgur.com/4wXpZ9C.png" class="floating-meme meme-1" alt="Meme">
    <img src="https://i.imgur.com/9cM9j0N.png" class="floating-meme meme-2" alt="Meme">
    <img src="https://i.imgur.com/7GqBk0v.png" class="floating-meme meme-3" alt="Meme">
    <img src="https://i.imgur.com/t0BpZ8W.png" class="floating-meme meme-4" alt="Meme">
    
    <!-- Kontrolki audio -->
    <div class="audio-controls">
        <button class="audio-btn" id="musicToggle" title="Włącz/Wyłącz muzykę">
            <i class="fas fa-music"></i>
        </button>
        
        <button class="audio-btn" id="sfxToggle" title="Włącz/Wyłącz efekty dźwiękowe">
            <i class="fas fa-volume-up"></i>
        </button>
        
        <button class="audio-btn" id="volumeControl" title="Głośność">
            <i class="fas fa-sliders-h"></i>
        </button>
        
        <div class="volume-slider-container" id="volumeSliderContainer">
            <i class="fas fa-volume-down" style="color: var(--primary);"></i>
            <input type="range" min="0" max="100" value="50" class="volume-slider" id="volumeSlider">
            <i class="fas fa-volume-up" style="color: var(--primary);"></i>
        </div>
    </div>
    
    <!-- Elementy audio -->
    <audio id="backgroundMusic" loop>
        <source src="https://assets.mixkit.co/music/preview/mixkit-tech-house-vibes-130.mp3" type="audio/mpeg">
        Twoja przeglądarka nie obsługuje elementu audio.
    </audio>
    
    <audio id="clickSound">
        <source src="https://assets.mixkit.co/sfx/preview/mixkit-select-click-1109.mp3" type="audio/mpeg">
    </audio>
    
    <audio id="hoverSound">
        <source src="https://assets.mixkit.co/sfx/preview/mixkit-plastic-bubble-click-1124.mp3" type="audio/mpeg">
    </audio>
    
    <audio id="successSound">
        <source src="https://assets.mixkit.co/sfx/preview/mixkit-achievement-bell-600.mp3" type="audio/mpeg">
    </audio>
    
    <!-- Efekt kursora -->
    <div class="cursor-trail" id="cursorTrail"></div>
    
    <!-- Nagłówek -->
    <header>
        <div class="container">
            <nav class="nav">
                <div class="logo">
                    <div class="logo-icon">🏀</div>
                    <div class="logo-text">
                        <h1>$DUNKBALL</h1>
                        <p>SOLANA MEMECOIN | NO LIMITS, ONLY NETS</p>
                    </div>
                </div>
                <div class="nav-links">
                    <a href="#home">HOME</a>
                    <a href="#tokenomics">TOKENOMICS</a>
                    <a href="#chart">LIVE CHART</a>
                    <a href="#buy">HOW TO BUY</a>
                </div>
                <div class="nav-buttons">
                    <a href="#buy" class="btn btn-primary"><i class="fas fa-bolt"></i> BUY NOW</a>
                    <a href="#chart" class="btn btn-secondary"><i class="fas fa-chart-line"></i> CHART</a>
                </div>
            </nav>
        </div>
    </header>
    
    <!-- Sekcja główna -->
    <section class="hero" id="home">
        <div class="hero-bg"></div>
        <div class="container">
            <h2>FROM HALF COURT TO FULL SEND</h2>
            <p>The only memecoin that's always scoring. No team taxes, no rug pulls - just pure, unadulterated BALLING on the Solana chain. This isn't just a token, it's a lifestyle.</p>
            
            <div class="hero-stats">
                <div class="stat">
                    <span class="stat-value" id="marketCap">$12.4M</span>
                    <span class="stat-label">MARKET CAP</span>
                </div>
                <div class="stat">
                    <span class="stat-value" id="holders">8,421</span>
                    <span class="stat-label">HOLDERS</span>
                </div>
                <div class="stat">
                    <span class="stat-value" id="price">$0.00124</span>
                    <span class="stat-label">PRICE</span>
                </div>
                <div class="stat">
                    <span class="stat-value" id="liquidity">$1.8M</span>
                    <span class="stat-label">LIQUIDITY</span>
                </div>
            </div>
        </div>
    </section>
    
    <!-- Wykres LIVE -->
    <section class="section chart-section" id="chart">
        <div class="container">
            <h2 class="section-title">LIVE <span>CHART</span></h2>
            
            <div class="chart-container">
                <div class="chart-header">
                    <h3 class="chart-title">$DUNKBALL / <span>USDC</span></h3>
                    <div class="interval-selector" id="intervalSelector">
                        <button class="interval-btn" data-interval="1">1m</button>
                        <button class="interval-btn" data-interval="5">5m</button>
                        <button class="interval-btn active" data-interval="15">15m</button>
                        <button class="interval-btn" data-interval="60">1h</button>
                        <button class="interval-btn" data-interval="240">4h</button>
                        <button class="interval-btn" data-interval="1D">1D</button>
                    </div>
                </div>
                
                <!-- Widget DexScreener -->
                <div id="dex-widget">
                    <div class="widget-container" id="widgetContainer">
                        <!-- Widget zostanie załadowany przez JavaScript -->
                    </div>
                </div>
                
                <!-- Informacje o parze -->
                <div class="pair-info">
                    <div class="pair-detail">
                        <div class="pair-label">24h Volume</div>
                        <div class="pair-value" id="volume24h">$1,245,892</div>
                    </div>
                    <div class="pair-detail">
                        <div class="pair-label">24h Change</div>
                        <div class="pair-value positive" id="change24h">+12.45%</div>
                    </div>
                    <div class="pair-detail">
                        <div class="pair-label">Liquidity</div>
                        <div class="pair-value" id="pairLiquidity">$489,230</div>
                    </div>
                    <div class="pair-detail">
                        <div class="pair-label">FDV</div>
                        <div class="pair-value" id="fdv">$14.2M</div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    
    <!-- Tokenomics -->
    <section class="section" id="tokenomics">
        <div class="container">
            <h2 class="section-title">TOKEN<span>OMICS</span></h2>
            
            <div class="tokenomics-grid">
                <div class="token-card">
                    <div class="token-icon">🔥</div>
                    <h3 class="token-title">TOTAL SUPPLY</h3>
                    <div class="token-value">1 BILLION</div>
                    <p class="token-desc">That's right - 1,000,000,000 $DUNK. No infinite mint, no funny business.</p>
                </div>
                
                <div class="token-card">
                    <div class="token-icon">🏀</div>
                    <h3 class="token-title">LIQUIDITY</h3>
                    <div class="token-value">100% LOCKED</div>
                    <p class="token-desc">LP tokens burned, contract renounced. This is as safe as it gets.</p>
                </div>
                
                <div class="token-card">
                    <div class="token-icon">👥</div>
                    <h3 class="token-title">TEAM ALLOCATION</h3>
                    <div class="token-value">5%</div>
                    <p class="token-desc">For marketing and development. Vested for 12 months.</p>
                </div>
                
                <div class="token-card">
                    <div class="token-icon">💰</div>
                    <h3 class="token-title">TAX</h3>
                    <div class="token-value">0/0</div>
                    <p class="token-desc">No buy tax, no sell tax. Trade freely like a true baller.</p>
                </div>
            </div>
            
            <!-- Adres kontraktu -->
            <div class="contract-section">
                <h3 class="contract-title">CONTRACT ADDRESS</h3>
                <div class="contract-address" id="contractAddress">
                    6wCYEZEBFQC7CHndo7p7KejyM4oGgi5E1Ya1e9eQpump
                </div>
                <button class="copy-btn" onclick="copyContract()">
                    <i class="fas fa-copy"></i> COPY ADDRESS
                </button>
                <div style="margin-top: 15px; color: #aaa; font-size: 14px;">
                    <i class="fas fa-exclamation-circle"></i> Verify this address on Solscan before trading
                </div>
            </div>
        </div>
    </section>
    
    <!-- Jak kupić -->
    <section class="section" id="buy">
        <div class="container">
            <h2 class="section-title">HOW TO <span>BUY</span></h2>
            
            <div class="tokenomics-grid">
                <div class="token-card">
                    <div class="token-icon">1</div>
                    <h3 class="token-title">GET SOLANA</h3>
                    <p class="token-desc">Buy SOL on any major exchange (Binance, Coinbase, Kraken) and send to your Solana wallet (Phantom, Solflare, Backpack).</p>
                </div>
                
                <div class="token-card">
                    <div class="token-icon">2</div>
                    <h3 class="token-title">SWAP TO $DUNK</h3>
                    <p class="token-desc">Go to Jupiter, Raydium, or Pump.fun and swap SOL for $DUNKBALL using the contract address above.</p>
                </div>
                
                <div class="token-card">
                    <div class="token-icon">3</div>
                    <h3 class="token-title">HOLD & BALL</h3>
                    <p class="token-desc">Add to your wallet and watch the gains roll in. This is a marathon, not a sprint (but we're pretty fast).</p>
                </div>
            </div>
        </div>
    </section>
    
    <!-- Social media -->
    <section class="social-section">
        <div class="container">
            <h2 class="section-title">JOIN THE <span>TEAM</span></h2>
            <p>Follow us for updates, memes, and the occasional halftime show</p>
            
            <div class="social-links">
                <a href="https://twitter.com" target="_blank" class="social-link">
                    <i class="fab fa-twitter"></i>
                </a>
                <a href="https://t.me" target="_blank" class="social-link">
                    <i class="fab fa-telegram"></i>
                </a>
                <a href="https://discord.com" target="_blank" class="social-link">
                    <i class="fab fa-discord"></i>
                </a>
                <a href="https://dexscreener.com/solana/26m5m3nwgake4zavkd3zetys5hjwdxe6xbwpdtslhy1o" target="_blank" class="social-link">
                    <i class="fas fa-chart-line"></i>
                </a>
                <a href="https://birdeye.so" target="_blank" class="social-link">
                    <i class="fas fa-eye"></i>
                </a>
            </div>
        </div>
    </section>
    
    <!-- Stopka -->
    <footer>
        <div class="container">
            <div class="footer-logo">$DUNKBALL</div>
            <p>THE OFFICIAL BASKETBALL MEMECOIN OF SOLANA</p>
            
            <div class="disclaimer">
                <strong>DISCLAIMER:</strong> $DUNKBALL is a meme coin with no intrinsic value or expectation of financial return. This is a community-driven experiment in decentralized entertainment. The coin is completely useless and for entertainment purposes only. Please do not spend money you cannot afford to lose. Not affiliated with the NBA or any basketball organization.
            </div>
            
            <p style="color: #666; margin-top: 30px; font-size: 14px;">
                © 2024 $DUNKBALL. All memes reserved. Contract renounced, LP burned.
            </p>
        </div>
    </footer>
    
    <script>
        // ====================
        // SYSTEM DŹWIĘKU
        // ====================
        let musicEnabled = false;
        let sfxEnabled = true;
        let volume = 0.5;
        
        const backgroundMusic = document.getElementById('backgroundMusic');
        const clickSound = document.getElementById('clickSound');
        const hoverSound = document.getElementById('hoverSound');
        const successSound = document.getElementById('successSound');
        
        // Ustaw głośność początkową
        backgroundMusic.volume = volume;
        clickSound.volume = volume;
        hoverSound.volume = volume * 0.5; // Efekty cichsze
        successSound.volume = volume;
        
        // Elementy interfejsu
        const musicToggle = document.getElementById('musicToggle');
        const sfxToggle = document.getElementById('sfxToggle');
        const volumeControl = document.getElementById('volumeControl');
        const volumeSlider = document.getElementById('volumeSlider');
        const volumeSliderContainer = document.getElementById('volumeSliderContainer');
        
        // Obsługa muzyki tła
        musicToggle.addEventListener('click', function() {
            musicEnabled = !musicEnabled;
            
            if (musicEnabled) {
                backgroundMusic.play().catch(e => {
                    console.log("Autoplay blocked. User needs to interact first.");
                    musicEnabled = false;
                    this.classList.remove('active');
                    alert("Kliknij gdziekolwiek na stronie, aby odblokować dźwięk, a następnie włącz muzykę ponownie.");
                });
                this.classList.add('active');
                this.innerHTML = '<i class="fas fa-pause"></i>';
                playSound(successSound);
            } else {
                backgroundMusic.pause();
                this.classList.remove('active');
                this.innerHTML = '<i class="fas fa-music"></i>';
                playSound(clickSound);
            }
        });
        
        // Obsługa efektów dźwiękowych
        sfxToggle.addEventListener('click', function() {
            sfxEnabled = !sfxEnabled;
            if (sfxEnabled) {
                this.classList.add('active');
                this.innerHTML = '<i class="fas fa-volume-up"></i>';
                playSound(successSound);
            } else {
                this.classList.remove('active');
                this.innerHTML = '<i class="fas fa-volume-mute"></i>';
                playSound(clickSound);
            }
        });
        
        // Pokazuj/ukryj suwak głośności
        volumeControl.addEventListener('click', function() {
            volumeSliderContainer.classList.toggle('show');
            playSound(clickSound);
        });
        
        // Zmiana głośności
        volumeSlider.addEventListener('input', function() {
            volume = this.value / 100;
            backgroundMusic.volume = volume;
            clickSound.volume = volume;
            hoverSound.volume = volume * 0.5;
            successSound.volume = volume;
        });
        
        // Funkcja odtwarzania dźwięku
        function playSound(soundElement) {
            if (!sfxEnabled) return;
            
            soundElement.currentTime = 0;
            soundElement.play().catch(e => {
                console.log("Sound play failed:", e);
            });
        }
        
        // Dodaj efekty dźwiękowe do interakcji
        document.querySelectorAll('.btn, .interval-btn, .copy-btn, .social-link, .nav-links a').forEach(element => {
            element.addEventListener('click', () => {
                playSound(clickSound);
            });
            
            element.addEventListener('mouseenter', () => {
                playSound(hoverSound);
            });
        });
        
        // Autoplay po interakcji użytkownika (wymagane przez przeglądarki)
        document.addEventListener('click', function initAudio() {
            // Tylko jedna inicjalizacja
            document.removeEventListener('click', initAudio);
            
            // Uruchom wszystkie elementy audio (ale wyciszone)
            backgroundMusic.volume = 0;
            backgroundMusic.play().then(() => {
                backgroundMusic.pause();
                backgroundMusic.volume = volume;
            }).catch(e => console.log("Autoplay initialization"));
        });
        
        // ====================
        // EFEKT ŚLADU KURSORA
        // ====================
        const cursorTrail = document.getElementById('cursorTrail');
        let lastTime = 0;
        const minTimeDiff = 16;
        
        const colorPalettes = ['#00FFA3', '#14F195', '#9945FF', '#03E1FF', '#FF6B00'];
        
        document.addEventListener('mousemove', (e) => {
            const currentTime = Date.now();
            if (currentTime - lastTime < minTimeDiff) return;
            lastTime = currentTime;
            
            for (let i = 0; i < 3; i++) {
                setTimeout(() => {
                    createTrailDot(e.clientX, e.clientY);
                }, i * 50);
            }
        });
        
        function createTrailDot(x, y) {
            const dot = document.createElement('div');
            dot.className = 'trail-dot';
            
            const randomColor = colorPalettes[Math.floor(Math.random() * colorPalettes.length)];
            const offsetX = (Math.random() - 0.5) * 15;
            const offsetY = (Math.random() - 0.5) * 15;
            
            dot.style.left = (x + offsetX) + 'px';
            dot.style.top = (y + offsetY) + 'px';
            dot.style.background = randomColor;
            dot.style.setProperty('--dot-color', randomColor);
            
            cursorTrail.appendChild(dot);
            
            setTimeout(() => {
                if (dot.parentNode === cursorTrail) {
                    cursorTrail.removeChild(dot);
                }
            }, 800);
        }
        
        // ====================
        // WIDGET DEXSCREENER
        // ====================
        let currentInterval = '15';
        
        function loadDexWidget(interval = '15') {
            const widgetContainer = document.getElementById('widgetContainer');
            widgetContainer.innerHTML = '';
            
            // Tworzymy widget DexScreener z odpowiednimi parametrami
            const pairAddress = '26m5m3nwgake4zavkd3zetys5hjwdxe6xbwpdtslhy1o';
            
            // Tworzymy iframe z DexScreener (oficjalny embed)
            const iframe = document.createElement('iframe');
            iframe.src = `https://dexscreener.com/solana/${pairAddress}?embed=1&theme=dark&trades=0&info=0&chartOnly=0&interval=${interval}`;
            iframe.style.width = '100%';
            iframe.style.height = '100%';
            iframe.style.border = 'none';
            iframe.style.borderRadius = '12px';
            iframe.allow = 'clipboard-write';
            iframe.title = 'DexScreener Chart';
            
            widgetContainer.appendChild(iframe);
        }
        
        // Inicjalizacja widgetu z domyślnym interwałem 15m
        window.addEventListener('load', () => {
            loadDexWidget('15');
            
            // Aktualizuj dane co 30 sekund (symulacja)
            updatePairData();
            setInterval(updatePairData, 30000);
        });
        
        // Obsługa zmiany interwałów
        document.querySelectorAll('.interval-btn').forEach(btn => {
            btn.addEventListener('click', function() {
                // Usuń aktywną klasę ze wszystkich przycisków
                document.querySelectorAll('.interval-btn').forEach(b => {
                    b.classList.remove('active');
                });
                
                // Dodaj aktywną klasę do klikniętego przycisku
                this.classList.add('active');
                
                // Pobierz nowy interwał
                const newInterval = this.getAttribute('data-interval');
                currentInterval = newInterval;
                
                // Załaduj widget z nowym interwałem
                loadDexWidget(newInterval);
                
                // Efekt dźwiękowy
                playSound(clickSound);
            });
        });
        
        // ====================
        // AKTUALIZACJA DANYCH PARY
        // ====================
        function updatePairData() {
            // Symulacja zmieniających się danych
            // W rzeczywistości pobierałbyś te dane z API DexScreener
            
            const change24h = document.getElementById('change24h');
            const volume24h = document.getElementById('volume24h');
            const pairLiquidity = document.getElementById('pairLiquidity');
            const fdv = document.getElementById('fdv');
            
            // Losowe zmiany dla demonstracji
            const randomChange = (Math.random() - 0.5) * 20;
            const randomVolume = 800000 + Math.random() * 1000000;
            const randomLiquidity = 400000 + Math.random() * 200000;
            const randomFDV = 10000000 + Math.random() * 10000000;
            
            // Formatowanie liczb
            const formatCurrency = (num) => {
                if (num >= 1000000) {
                    return '$' + (num / 1000000).toFixed(2) + 'M';
                }
                if (num >= 1000) {
                    return '$' + (num / 1000).toFixed(1) + 'K';
                }
                return '$' + num.toFixed(0);
            };
            
            // Aktualizuj wartości
            const changeValue = randomChange.toFixed(2);
            change24h.textContent = (randomChange >= 0 ? '+' : '') + changeValue + '%';
            change24h.className = randomChange >= 0 ? 'pair-value positive' : 'pair-value negative';
            
            volume24h.textContent = formatCurrency(randomVolume);
            pairLiquidity.textContent = formatCurrency(randomLiquidity);
            fdv.textContent = formatCurrency(randomFDV);
        }
        
        // ====================
        // ANIMOWANE LICZBY
        // ====================
        function animateValue(element, start, end, duration) {
            let startTimestamp = null;
            const step = (timestamp) => {
                if (!startTimestamp) startTimestamp = timestamp;
                const progress = Math.min((timestamp - startTimestamp) / duration, 1);
                const value = Math.floor(progress * (end - start) + start);
                element.textContent = formatNumber(value);
                if (progress < 1) {
                    window.requestAnimationFrame(step);
                }
            };
            window.requestAnimationFrame(step);
        }
        
        function formatNumber(num) {
            if (num >= 1000000) {
                return '$' + (num / 1000000).toFixed(1) + 'M';
            }
            if (num >= 1000) {
                return (num / 1000).toFixed(0) + 'K';
            }
            return num.toString();
        }
        
        // Start animacji po załadowaniu strony
        window.addEventListener('load', () => {
            // Symulacja zmiennych cen
            setInterval(() => {
                const change = (Math.random() - 0.5) * 0.0001;
                const priceElement = document.getElementById('price');
                const currentPrice = parseFloat(priceElement.textContent.replace('$', ''));
                const newPrice = Math.max(0.0005, currentPrice + change);
                priceElement.textContent = '$' + newPrice.toFixed(5);
                
                // Aktualizuj market cap
                const marketCapElement = document.getElementById('marketCap');
                const currentMC = parseFloat(marketCapElement.textContent.replace('$', '').replace('M', ''));
                const mcChange = (Math.random() - 0.5) * 0.5;
                const newMC = Math.max(5, currentMC + mcChange);
                marketCapElement.textContent = '$' + newMC.toFixed(1) + 'M';
            }, 3000);
        });
        
        // ====================
        // KOPIOWANIE ADRESU KONTRAKTU
        // ====================
        function copyContract() {
            const contractText = document.getElementById('contractAddress').textContent;
            navigator.clipboard.writeText(contractText.trim())
                .then(() => {
                    const btn = document.querySelector('.copy-btn');
                    const originalText = btn.innerHTML;
                    btn.innerHTML = '<i class="fas fa-check"></i> COPIED!';
                    btn.style.background = '#14F195';
                    
                    // Efekt dźwiękowy
                    playSound(successSound);
                    
                    setTimeout(() => {
                        btn.innerHTML = originalText;
                        btn.style.background = '';
                    }, 2000);
                })
                .catch(err => {
                    console.error('Failed to copy: ', err);
                });
        }
        
        // ====================
        // PŁYNNE PRZEWIJANIE
        // ====================
        document.querySelectorAll('a[href^="#"]').forEach(anchor => {
            anchor.addEventListener('click', function (e) {
                e.preventDefault();
                
                const targetId = this.getAttribute('href');
                if (targetId === '#') return;
                
                const targetElement = document.querySelector(targetId);
                if (targetElement) {
                    window.scrollTo({
                        top: targetElement.offsetTop - 80,
                        behavior: 'smooth'
                    });
                }
            });
        });
        
        // ====================
        // EFEKT PARALLAX
        // ====================
        window.addEventListener('scroll', () => {
            const scrolled = window.pageYOffset;
            const heroBg = document.querySelector('.hero-bg');
            
            if (heroBg) {
                heroBg.style.transform = `translateY(${scrolled * 0.5}px)`;
            }
        });
    </script>
</body>
</html>
