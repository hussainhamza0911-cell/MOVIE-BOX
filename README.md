# MOVIE-BOX<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Movie Box - Hollywood Movies Streaming</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Poppins', sans-serif;
            line-height: 1.6;
            color: #fff;
            background: linear-gradient(135deg, #0c0c0c 0%, #1a1a2e 50%, #16213e 100%);
            overflow-x: hidden;
            min-height: 100vh;
        }

        /* Hollywood Classic Color Effects */
        :root {
            --primary-gold: #d4af37;
            --primary-red: #dc143c;
            --accent-gold: #f1c40f;
            --dark-bg: #0a0a0a;
            --card-bg: rgba(20, 20, 40, 0.9);
            --glass-bg: rgba(255, 255, 255, 0.1);
            --glow-gold: 0 0 20px var(--primary-gold);
            --glow-red: 0 0 20px var(--primary-red);
        }

        /* --- Custom Scrollbar --- */
        ::-webkit-scrollbar {
            width: 10px;
        }
        ::-webkit-scrollbar-track {
            background: var(--dark-bg);
        }
        ::-webkit-scrollbar-thumb {
            background: var(--primary-gold);
            border-radius: 5px;
        }

        /* --- Utility Classes --- */
        .hidden {
            display: none !important;
        }

        /* --- Owner Control Panel --- */
        .owner-panel {
            position: fixed;
            top: 0;
            right: -400px;
            width: 400px;
            height: 100vh;
            background: linear-gradient(145deg, #1a1a2e, #16213e);
            box-shadow: -10px 0 30px rgba(0,0,0,0.5);
            z-index: 10000;
            transition: right 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
            backdrop-filter: blur(10px);
            border-left: 2px solid var(--primary-gold);
        }

        .owner-panel.active {
            right: 0;
        }

        .owner-header {
            padding: 20px;
            border-bottom: 1px solid rgba(212, 175, 55, 0.3);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .owner-header h3 {
            color: var(--primary-gold);
            font-size: 1.4rem;
        }

        .close-btn {
            background: rgba(220, 20, 60, 0.2);
            border: 1px solid var(--primary-red);
            color: var(--primary-red);
            padding: 8px 12px;
            border-radius: 50%;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .close-btn:hover {
            background: var(--primary-red);
            color: white;
            box-shadow: var(--glow-red);
        }

        .owner-content {
            padding: 20px;
            overflow-y: auto;
            height: calc(100vh - 80px);
        }

        .control-section {
            margin-bottom: 25px;
            padding: 20px;
            background: var(--glass-bg);
            border-radius: 12px;
            border: 1px solid rgba(212, 175, 55, 0.2);
        }

        .control-section h4 {
            color: var(--primary-gold);
            margin-bottom: 15px;
            font-size: 1.1rem;
        }

        .control-section input, .control-section select {
            width: 100%;
            padding: 12px;
            margin-bottom: 15px;
            background: rgba(0,0,0,0.4);
            border: 1px solid rgba(212, 175, 55, 0.5);
            border-radius: 8px;
            color: white;
            font-size: 14px;
            outline: none;
            transition: border-color 0.3s ease;
        }

        .control-section input:focus, .control-section select:focus {
            border-color: var(--primary-gold);
            box-shadow: inset 0 0 5px var(--primary-gold);
        }

        .control-section input::placeholder {
            color: rgba(255,255,255,0.5);
        }

        .control-section select option {
            background: #1a1a2e;
            color: white;
        }

        .control-section button {
            width: 100%;
            padding: 12px;
            margin-bottom: 10px;
            background: linear-gradient(45deg, var(--primary-gold), #f39c12);
            border: none;
            border-radius: 8px;
            color: #000;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .control-section button:hover {
            transform: translateY(-2px);
            box-shadow: var(--glow-gold);
        }

        .owner-access-btn {
            position: fixed;
            bottom: 30px;
            right: 30px;
            width: 60px;
            height: 60px;
            background: linear-gradient(45deg, var(--primary-gold), #f39c12);
            border: none;
            border-radius: 50%;
            color: #000;
            font-size: 1.5rem;
            cursor: pointer;
            z-index: 9999;
            box-shadow: var(--glow-gold);
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0% { box-shadow: 0 0 0 0 rgba(212, 175, 55, 0.7); }
            70% { box-shadow: 0 0 0 20px rgba(212, 175, 55, 0); }
            100% { box-shadow: 0 0 0 0 rgba(212, 175, 55, 0); }
        }

        /* --- Maintenance Overlay --- */
        .maintenance-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.95);
            display: flex;
            justify-content: center;
            align-items: center;
            z-index: 9998;
            backdrop-filter: blur(10px);
        }

        .maintenance-content {
            text-align: center;
            padding: 40px;
            background: linear-gradient(145deg, var(--dark-bg), var(--card-bg));
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.8);
            border: 2px solid var(--primary-gold);
            max-width: 500px;
        }

        .maintenance-content h2 {
            color: var(--primary-gold);
            font-size: 2.5rem;
            margin-bottom: 20px;
        }

        /* --- Header --- */
        .header {
            position: fixed;
            top: 0;
            width: 100%;
            background: rgba(10, 10, 10, 0.85);
            backdrop-filter: blur(20px);
            z-index: 1000;
            padding: 15px 0;
            border-bottom: 1px solid rgba(212, 175, 55, 0.2);
            transition: all 0.3s ease;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 0 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .logo {
            display: flex;
            align-items: center;
            font-size: 1.8rem;
            font-weight: 700;
            color: var(--primary-gold);
            text-decoration: none;
            text-shadow: 0 2px 10px rgba(212,175,55,0.3);
        }

        .logo i {
            margin-right: 10px;
            font-size: 2rem;
        }

        .nav {
            display: flex;
            gap: 30px;
        }

        .nav-link {
            color: rgba(255,255,255,0.8);
            text-decoration: none;
            font-weight: 500;
            transition: all 0.3s ease;
            position: relative;
            padding-bottom: 5px;
        }

        .nav-link:hover,
        .nav-link.active {
            color: var(--primary-gold);
        }

        .nav-link::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 0;
            height: 2px;
            background: var(--primary-gold);
            transition: width 0.3s ease;
        }

        .nav-link:hover::after,
        .nav-link.active::after {
            width: 100%;
        }

        .search-box {
            position: relative;
            display: flex;
            align-items: center;
            background: rgba(255,255,255,0.05);
            border: 1px solid rgba(212, 175, 55, 0.3);
            border-radius: 25px;
            padding: 8px 20px;
            transition: all 0.3s ease;
        }

        .search-box:focus-within {
            border-color: var(--primary-gold);
            box-shadow: 0 0 10px rgba(212,175,55,0.2);
            background: rgba(255,255,255,0.1);
        }

        .search-box i {
            color: rgba(255,255,255,0.5);
            margin-right: 10px;
        }

        .search-box input {
            background: none;
            border: none;
            color: white;
            outline: none;
            width: 200px;
            transition: width 0.3s ease;
        }

        .search-box input:focus {
            width: 250px;
        }

        .search-box input::placeholder {
            color: rgba(255,255,255,0.5);
        }

        /* --- Hero Section --- */
        .hero {
            height: 100vh;
            background: linear-gradient(rgba(0,0,0,0.8), rgba(0,0,0,0.4), rgba(10,10,10,1)), 
                        url('https://images.unsplash.com/photo-1489599849927-2ee91cede3ba?q=80&w=2070&auto=format&fit=crop') center/cover no-repeat fixed;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            text-align: center;
            position: relative;
            padding-top: 80px;
        }

        .hero::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: radial-gradient(circle at 20% 80%, rgba(212, 175, 55, 0.15) 0%, transparent 50%),
                        radial-gradient(circle at 80% 20%, rgba(220, 20, 60, 0.15) 0%, transparent 50%);
            pointer-events: none;
        }

        .hero-content {
            z-index: 2;
            max-width: 800px;
            position: relative;
            padding: 0 20px;
        }

        .hero h1 {
            font-size: 4.5rem;
            font-weight: 700;
            margin-bottom: 20px;
            background: linear-gradient(45deg, var(--primary-gold), #fff, var(--primary-gold));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.5);
            letter-spacing: 2px;
        }

        .hero p {
            font-size: 1.4rem;
            margin-bottom: 40px;
            color: #ddd;
            text-shadow: 1px 1px 2px #000;
        }

        .cta-btn {
            padding: 18px 45px;
            font-size: 1.2rem;
            font-weight: 600;
            background: linear-gradient(45deg, var(--primary-gold), #f39c12);
            border: none;
            border-radius: 50px;
            color: #000;
            cursor: pointer;
            transition: all 0.3s ease;
            box-shadow: 0 10px 30px rgba(212, 175, 55, 0.4);
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .cta-btn:hover {
            transform: translateY(-5px) scale(1.05);
            box-shadow: 0 15px 40px rgba(212, 175, 55, 0.6);
        }

        .hero-features {
            display: flex;
            gap: 50px;
            margin-top: 60px;
            position: relative;
            z-index: 2;
        }

        .feature {
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 15px;
            font-weight: 500;
            color: #ccc;
        }

        .feature i {
            font-size: 2.5rem;
            color: var(--primary-gold);
            background: rgba(212, 175, 55, 0.1);
            width: 80px;
            height: 80px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            border: 1px solid rgba(212,175,55,0.3);
            transition: all 0.3s ease;
        }
        
        .feature:hover i {
            background: rgba(212, 175, 55, 0.3);
            transform: scale(1.1);
            box-shadow: var(--glow-gold);
        }

        /* --- Movies Section & Grid --- */
        .movies-section {
            padding: 100px 0;
            background: var(--dark-bg);
            position: relative;
        }

        .section-title {
            text-align: center;
            font-size: 3rem;
            margin-bottom: 60px;
            background: linear-gradient(45deg, var(--primary-gold), white);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            position: relative;
            display: inline-block;
            left: 50%;
            transform: translateX(-50%);
        }

        .section-title::after {
            content: '';
            position: absolute;
            bottom: -10px;
            left: 25%;
            width: 50%;
            height: 3px;
            background: linear-gradient(90deg, transparent, var(--primary-gold), transparent);
        }

        .movies-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
            gap: 30px;
            padding: 20px 0;
        }

        /* --- Movie Cards --- */
        .movie-card {
            background: var(--card-bg);
            border-radius: 15px;
            overflow: hidden;
            transition: all 0.4s ease;
            position: relative;
            cursor: pointer;
            border: 1px solid rgba(255,255,255,0.05);
            box-shadow: 0 10px 20px rgba(0,0,0,0.5);
        }

        .movie-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 15px 30px rgba(212, 175, 55, 0.2);
            border-color: rgba(212,175,55,0.5);
        }

        .poster-container {
            position: relative;
            width: 100%;
            padding-top: 150%; /* 2:3 Aspect Ratio */
            overflow: hidden;
        }

        .movie-card img {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.5s ease;
        }

        .movie-card:hover img {
            transform: scale(1.1);
        }

        .play-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.6);
            display: flex;
            justify-content: center;
            align-items: center;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .movie-card:hover .play-overlay {
            opacity: 1;
        }

        .play-overlay i {
            font-size: 4rem;
            color: var(--primary-gold);
            text-shadow: 0 0 20px rgba(212,175,55,0.8);
            transform: scale(0.8);
            transition: transform 0.3s ease;
        }

        .movie-card:hover .play-overlay i {
            transform: scale(1);
        }

        .movie-info-card {
            padding: 20px;
            background: linear-gradient(to top, #111 50%, transparent);
            position: absolute;
            bottom: 0;
            width: 100%;
            z-index: 2;
        }

        .movie-info-card h3 {
            font-size: 1.2rem;
            margin-bottom: 5px;
            color: #fff;
            text-shadow: 1px 1px 3px #000;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .movie-meta-card {
            display: flex;
            justify-content: space-between;
            color: var(--primary-gold);
            font-size: 0.85rem;
            font-weight: 600;
        }

        /* --- Movie Modal Player --- */
        .movie-modal {
            position: fixed;
            top: 0; 
            left: 0; 
            width: 100%; 
            height: 100%;
            background: rgba(0,0,0,0.95);
            z-index: 10001;
            display: flex;
            justify-content: center;
            align-items: center;
            backdrop-filter: blur(10px);
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
        }

        .movie-modal:not(.hidden) {
            opacity: 1;
            visibility: visible;
        }

        .modal-content {
            background: var(--dark-bg);
            width: 90%;
            max-width: 1000px;
            border-radius: 15px;
            border: 1px solid rgba(212,175,55,0.3);
            overflow: hidden;
            box-shadow: 0 0 40px rgba(0,0,0,0.8), 0 0 20px rgba(212,175,55,0.1);
            transform: scale(0.9);
            transition: transform 0.3s ease;
        }

        .movie-modal:not(.hidden) .modal-content {
            transform: scale(1);
        }

        .modal-header {
            padding: 20px 25px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: #111;
            border-bottom: 1px solid rgba(255,255,255,0.05);
        }

        .modal-header h3 {
            color: var(--primary-gold);
            font-size: 1.4rem;
            margin: 0;
        }

        .close-modal {
            background: transparent;
            border: none;
            color: #aaa;
            font-size: 1.5rem;
            cursor: pointer;
            transition: color 0.3s;
        }

        .close-modal:hover {
            color: var(--primary-red);
        }

        .player-container {
            width: 100%;
            background: #000;
        }

        .player-container video {
            width: 100%;
            max-height: 60vh;
            display: block;
        }

        .player-controls {
            padding: 15px 25px;
            background: #151515;
            display: flex;
            gap: 20px;
            border-bottom: 1px solid rgba(255,255,255,0.05);
        }

        .player-controls select {
            background: #222;
            color: white;
            border: 1px solid rgba(255,255,255,0.1);
            padding: 8px 15px;
            border-radius: 5px;
            outline: none;
            cursor: pointer;
        }

        .modal-movie-info {
            padding: 25px;
        }

        .modal-movie-meta {
            display: flex;
            gap: 20px;
            margin-bottom: 15px;
            color: var(--primary-gold);
            font-weight: 500;
            font-size: 0.95rem;
        }

        .modal-movie-meta span {
            background: rgba(212,175,55,0.1);
            padding: 4px 10px;
            border-radius: 4px;
        }

        .movie-description {
            color: #ccc;
            line-height: 1.8;
            font-size: 1rem;
        }

        /* --- Footer --- */
        .footer {
            background: #050505;
            padding: 60px 0 20px;
            border-top: 1px solid rgba(212,175,55,0.2);
            margin-top: 50px;
        }

        .footer-content {
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
            gap: 40px;
            margin-bottom: 40px;
        }

        .footer-section h3 {
            color: var(--primary-gold);
            font-size: 1.8rem;
            margin-bottom: 15px;
        }
        
        .footer-section h4 {
            color: #fff;
            margin-bottom: 15px;
            font-size: 1.2rem;
        }

        .footer-section p {
            color: #888;
            max-width: 300px;
        }

        .footer-section a {
            display: block;
            color: #888;
            text-decoration: none;
            margin-bottom: 10px;
            transition: color 0.3s;
        }

        .footer-section a:hover {
            color: var(--primary-gold);
            padding-left: 5px;
        }

        .footer-bottom {
            text-align: center;
            padding-top: 20px;
            border-top: 1px solid rgba(255,255,255,0.05);
            color: #666;
            font-size: 0.9rem;
        }

        /* --- Responsive Design --- */
        @media (max-width: 768px) {
            .hero h1 { font-size: 3rem; }
            .hero p { font-size: 1.1rem; }
            .hero-features { flex-direction: column; gap: 30px; }
            .nav { display: none; /* In a real app, add a hamburger menu */ }
            .search-box input { width: 120px; }
            .owner-panel { width: 300px; right: -300px; }
            .footer-content { flex-direction: column; }
        }
    </style>
</head>
<body>
    <!-- Owner Control Panel (Hidden by default) -->
    <div id="ownerPanel" class="owner-panel">
        <div class="owner-header">
            <h3><i class="fas fa-crown"></i> Owner Panel</h3>
            <button id="toggleOwnerPanel" class="close-btn"><i class="fas fa-times"></i></button>
        </div>
        <div class="owner-content">
            <div class="control-section">
                <h4><i class="fas fa-film"></i> Add New Movie</h4>
                <input type="text" id="movieTitle" placeholder="Movie Title">
                <input type="text" id="movieUrl" placeholder="Video URL (mp4)">
                <input type="text" id="moviePoster" placeholder="Poster Image URL">
                <input type="text" id="movieGenre" placeholder="Genre (e.g. Action)">
                <input type="text" id="movieYear" placeholder="Release Year">
                <button id="addMovie"><i class="fas fa-plus"></i> Publish Movie</button>
            </div>
            <div class="control-section">
                <h4><i class="fas fa-cogs"></i> Site Settings</h4>
                <button id="maintenanceToggle"><i class="fas fa-tools"></i> Toggle Maintenance</button>
                <select id="themeSelect">
                    <option value="classic">Classic Hollywood (Gold)</option>
                    <option value="dark">Cinematic Dark (Monochrome)</option>
                    <option value="neon">Cyberpunk (Neon)</option>
                </select>
                <button id="applyTheme"><i class="fas fa-paint-brush"></i> Apply Theme</button>
            </div>
        </div>
    </div>

    <!-- Owner Access Button -->
    <button id="ownerAccess" class="owner-access-btn" title="Open Owner Panel">
        <i class="fas fa-crown"></i>
    </button>

    <!-- Maintenance Overlay -->
    <div id="maintenanceOverlay" class="maintenance-overlay hidden">
        <div class="maintenance-content">
            <h2><i class="fas fa-tools"></i> Under Maintenance</h2>
            <p>We're currently upgrading our servers to bring you better streaming quality. We'll be back online shortly!</p>
        </div>
    </div>

    <!-- Header -->
    <header class="header" id="header">
        <div class="container">
            <a href="#" class="logo">
                <i class="fas fa-film"></i>
                <span>Movie Box</span>
            </a>
            <nav class="nav">
                <a href="#home" class="nav-link active">Home</a>
                <a href="#movies" class="nav-link">Movies</a>
                <a href="#" class="nav-link">Series</a>
                <a href="#" class="nav-link">Trending</a>
            </nav>
            <div class="header-actions">
                <div class="search-box">
                    <i class="fas fa-search"></i>
                    <input type="text" id="searchInput" placeholder="Search movies...">
                </div>
            </div>
        </div>
    </header>

    <!-- Hero Section -->
    <section id="home" class="hero">
        <div class="hero-content">
            <h1>Unlimited Movies & Shows</h1>
            <p>Experience cinematic magic from the comfort of your home. High quality streaming, zero interruptions.</p>
            <a href="#movies"><button class="cta-btn"><i class="fas fa-play"></i> Start Watching</button></a>
        </div>
        <div class="hero-features">
            <div class="feature">
                <i class="fas fa-desktop"></i>
                <span>4K Ultra HD</span>
            </div>
            <div class="feature">
                <i class="fas fa-language"></i>
                <span>Multi-Language</span>
            </div>
            <div class="feature">
                <i class="fas fa-mobile-alt"></i>
                <span>Any Device</span>
            </div>
        </div>
    </section>

    <!-- Movies Grid -->
    <section id="movies" class="movies-section">
        <div class="container">
            <h2 class="section-title">Latest Releases</h2>
            <div class="movies-grid" id="moviesGrid">
                <!-- Movies will be loaded here via JavaScript -->
            </div>
        </div>
    </section>

    <!-- Movie Player Modal -->
    <div id="movieModal" class="movie-modal hidden">
        <div class="modal-content">
            <div class="modal-header">
                <h3 id="modalTitle">Movie Title</h3>
                <button id="closeModal" class="close-modal"><i class="fas fa-times"></i></button>
            </div>
            <div class="player-container">
                <!-- Using HTML5 Video element. Real streaming sites would use custom players like Video.js or HLS.js -->
                <video id="moviePlayer" controls preload="metadata" controlsList="nodownload">
                    Your browser does not support the video tag.
                </video>
                <div class="player-controls">
                    <div class="quality-selector">
                        <select id="qualitySelect">
                            <option value="1080p">1080p HD</option>
                            <option value="720p">720p HD</option>
                            <option value="480p">480p SD</option>
                        </select>
                    </div>
                    <div class="subtitle-selector">
                        <select id="subtitleSelect">
                            <option value="">No Subtitles</option>
                            <option value="en">English (CC)</option>
                            <option value="es">Spanish</option>
                        </select>
                    </div>
                </div>
            </div>
            <div class="modal-movie-info">
                <div class="modal-movie-meta">
                    <span id="modalYear"><i class="far fa-calendar-alt"></i> 2024</span>
                    <span id="modalGenre"><i class="fas fa-film"></i> Action</span>
                    <span id="modalDuration"><i class="far fa-clock"></i> 2h 15m</span>
                </div>
                <div class="movie-description" id="modalDescription">
                    Loading description...
                </div>
            </div>
        </div>
    </div>

    <!-- Footer -->
    <footer class="footer">
        <div class="container">
            <div class="footer-content">
                <div class="footer-section">
                    <h3><i class="fas fa-film"></i> Movie Box</h3>
                    <p>Your ultimate destination for premium Hollywood entertainment. Stream thousands of movies and TV shows securely and in top quality.</p>
                </div>
                <div class="footer-section">
                    <h4>Quick Links</h4>
                    <a href="#home">Home</a>
                    <a href="#movies">Top Movies</a>
                    <a href="#">TV Series</a>
                    <a href="#">My List</a>
                </div>
                <div class="footer-section">
                    <h4>Support</h4>
                    <a href="#">FAQ</a>
                    <a href="#">Contact Us</a>
                    <a href="#">Terms of Service</a>
                    <a href="#">Privacy Policy</a>
                </div>
            </div>
            <div class="footer-bottom">
                <p>&copy; 2024 Movie Box. All rights reserved.</p>
            </div>
        </div>
    </footer>

    <script>
        // --- Mock Data Database ---
        // Using open source placeholder videos for demonstration purposes
        let movieDatabase = [
            {
                id: 1,
                title: "Dune: Echoes",
                year: "2024",
                genre: "Sci-Fi",
                duration: "2h 30m",
                poster: "https://images.unsplash.com/photo-1534447677768-be436bb09401?q=80&w=800&auto=format&fit=crop",
                url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4",
                description: "A breathtaking journey across the stars. When a distant colony loses communication, a specialized team is sent to uncover a mystery that threatens the entire galaxy."
            },
            {
                id: 2,
                title: "Neon Streets",
                year: "2023",
                genre: "Action",
                duration: "1h 55m",
                poster: "https://images.unsplash.com/photo-1605806616949-1e87b487cb2a?q=80&w=800&auto=format&fit=crop",
                url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ElephantsDream.mp4",
                description: "In a cyberpunk metropolis, a rogue detective must navigate a web of corruption to find a missing heiress before a corporate war breaks out."
            },
            {
                id: 3,
                title: "The Silent Forest",
                year: "2024",
                genre: "Thriller",
                duration: "2h 05m",
                poster: "https://images.unsplash.com/photo-1448375240586-882707db888b?q=80&w=800&auto=format&fit=crop",
                url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/TearsOfSteel.mp4",
                description: "A group of friends venture into an uncharted forest, only to realize that the ancient trees hold a dark secret that won't let them leave."
            },
            {
                id: 4,
                title: "Ocean's Edge",
                year: "2022",
                genre: "Drama",
                duration: "2h 10m",
                poster: "https://images.unsplash.com/photo-1505118380757-91f5f5632de0?q=80&w=800&auto=format&fit=crop",
                url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/Sintel.mp4",
                description: "An aging lighthouse keeper discovers a shipwrecked survivor who brings news of an impending storm that will change both their lives forever."
            },
            {
                id: 5,
                title: "Cyber Heist",
                year: "2024",
                genre: "Action",
                duration: "1h 45m",
                poster: "https://images.unsplash.com/photo-1526304640581-d334cdbbf45e?q=80&w=800&auto=format&fit=crop",
                url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerBlazes.mp4",
                description: "The world's greatest hackers are recruited for an impossible mission: infiltrate the central bank's impenetrable mainframe."
            },
            {
                id: 6,
                title: "Midnight Drive",
                year: "2023",
                genre: "Crime",
                duration: "1h 50m",
                poster: "https://images.unsplash.com/photo-1494976388531-d1058494cdd8?q=80&w=800&auto=format&fit=crop",
                url: "https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerJoyrides.mp4",
                description: "A getaway driver finds himself hunted by his former employers after a heist goes terribly wrong on the streets of Los Angeles."
            }
        ];

        // --- DOM Elements ---
        const moviesGrid = document.getElementById('moviesGrid');
        const searchInput = document.getElementById('searchInput');
        
        // Modal Elements
        const movieModal = document.getElementById('movieModal');
        const closeModalBtn = document.getElementById('closeModal');
        const moviePlayer = document.getElementById('moviePlayer');
        const modalTitle = document.getElementById('modalTitle');
        const modalYear = document.getElementById('modalYear');
        const modalGenre = document.getElementById('modalGenre');
        const modalDuration = document.getElementById('modalDuration');
        const modalDescription = document.getElementById('modalDescription');

        // Owner Panel Elements
        const ownerAccessBtn = document.getElementById('ownerAccess');
        const ownerPanel = document.getElementById('ownerPanel');
        const toggleOwnerPanelBtn = document.getElementById('toggleOwnerPanel');
        const maintenanceToggle = document.getElementById('maintenanceToggle');
        const maintenanceOverlay = document.getElementById('maintenanceOverlay');
        const applyThemeBtn = document.getElementById('applyTheme');
        const addMovieBtn = document.getElementById('addMovie');

        // Header scroll effect
        window.addEventListener('scroll', () => {
            const header = document.getElementById('header');
            if (window.scrollY > 50) {
                header.style.background = 'rgba(10, 10, 10, 0.98)';
                header.style.boxShadow = '0 5px 20px rgba(0,0,0,0.5)';
            } else {
                header.style.background = 'rgba(10, 10, 10, 0.85)';
                header.style.boxShadow = 'none';
            }
        });

        // --- Core Functions ---

        // 1. Render Movies
        function renderMovies(moviesToRender = movieDatabase) {
            moviesGrid.innerHTML = ''; // Clear grid
            
            if (moviesToRender.length === 0) {
                moviesGrid.innerHTML = '<p style="text-align:center; grid-column: 1/-1; color: #888;">No movies found.</p>';
                return;
            }

            moviesToRender.forEach(movie => {
                const card = document.createElement('div');
                card.className = 'movie-card';
                card.innerHTML = `
                    <div class="poster-container">
                        <img src="${movie.poster}" alt="${movie.title} Poster" onerror="this.src='https://via.placeholder.com/400x600?text=No+Poster'">
                        <div class="play-overlay">
                            <i class="fas fa-play-circle"></i>
                        </div>
                    </div>
                    <div class="movie-info-card">
                        <h3>${movie.title}</h3>
                        <div class="movie-meta-card">
                            <span>${movie.year}</span>
                            <span>${movie.genre}</span>
                        </div>
                    </div>
                `;
                
                // Add click listener to open modal
                card.addEventListener('click', () => openMovieModal(movie));
                
                moviesGrid.appendChild(card);
            });
        }

        // 2. Open Movie Modal & Play
        function openMovieModal(movie) {
            modalTitle.textContent = movie.title;
            modalYear.innerHTML = `<i class="far fa-calendar-alt"></i> ${movie.year}`;
            modalGenre.innerHTML = `<i class="fas fa-film"></i> ${movie.genre}`;
            modalDuration.innerHTML = `<i class="far fa-clock"></i> ${movie.duration}`;
            modalDescription.textContent = movie.description;
            
            moviePlayer.src = movie.url;
            
            movieModal.classList.remove('hidden');
            // Auto play the video (browsers usually allow this if user interacted)
            moviePlayer.play().catch(e => console.log("Autoplay prevented:", e));
        }

        // 3. Close Modal & Stop Video
        function closeMovieModal() {
            movieModal.classList.add('hidden');
            moviePlayer.pause();
            moviePlayer.src = ''; // Clear source to stop buffering
        }

        // --- Event Listeners ---

        // Search functionality
        searchInput.addEventListener('input', (e) => {
            const searchTerm = e.target.value.toLowerCase();
            const filteredMovies = movieDatabase.filter(movie => 
                movie.title.toLowerCase().includes(searchTerm) || 
                movie.genre.toLowerCase().includes(searchTerm)
            );
            renderMovies(filteredMovies);
        });

        // Close modal on button click or outside click
        closeModalBtn.addEventListener('click', closeMovieModal);
        movieModal.addEventListener('click', (e) => {
            if (e.target === movieModal) {
                closeMovieModal();
            }
        });

        // --- Owner Control Panel Functionality ---

        // Toggle Panel
        ownerAccessBtn.addEventListener('click', () => {
            ownerPanel.classList.add('active');
        });

        toggleOwnerPanelBtn.addEventListener('click', () => {
            ownerPanel.classList.remove('active');
        });

        // Add Movie
        addMovieBtn.addEventListener('click', () => {
            const titleInput = document.getElementById('movieTitle');
            const urlInput = document.getElementById('movieUrl');
            const posterInput = document.getElementById('moviePoster');
            const genreInput = document.getElementById('movieGenre');
            const yearInput = document.getElementById('movieYear');

            if (!titleInput.value || !urlInput.value) {
                alert('Title and Video URL are required!');
                return;
            }

            const newMovie = {
                id: Date.now(),
                title: titleInput.value,
                url: urlInput.value,
                poster: posterInput.value || 'https://images.unsplash.com/photo-1485846234645-a62644f84728?q=80&w=800&auto=format&fit=crop', // default poster
                genre: genreInput.value || 'Unknown',
                year: yearInput.value || new Date().getFullYear().toString(),
                duration: "TBA",
                description: "Newly added movie to the database."
            };

            // Add to beginning of array
            movieDatabase.unshift(newMovie);
            renderMovies();

            // Clear inputs and give feedback
            titleInput.value = ''; urlInput.value = ''; posterInput.value = ''; genreInput.value = ''; yearInput.value = '';
            
            // Temporary button text feedback
            const originalText = addMovieBtn.innerHTML;
            addMovieBtn.innerHTML = '<i class="fas fa-check"></i> Added Successfully!';
            addMovieBtn.style.background = '#2ecc71';
            setTimeout(() => {
                addMovieBtn.innerHTML = originalText;
                addMovieBtn.style.background = ''; // reset to css default
            }, 2000);
        });

        // Toggle Maintenance
        maintenanceToggle.addEventListener('click', () => {
            maintenanceOverlay.classList.toggle('hidden');
            const isMaintenance = !maintenanceOverlay.classList.contains('hidden');
            
            if(isMaintenance) {
                maintenanceToggle.innerHTML = '<i class="fas fa-tools"></i> Disable Maintenance';
                maintenanceToggle.style.background = '#e74c3c';
            } else {
                maintenanceToggle.innerHTML = '<i class="fas fa-tools"></i> Toggle Maintenance';
                maintenanceToggle.style.background = '';
            }
        });

        // Theme Switcher
        applyThemeBtn.addEventListener('click', () => {
            const theme = document.getElementById('themeSelect').value;
            const root = document.documentElement;

            if (theme === 'dark') {
                root.style.setProperty('--primary-gold', '#ffffff');
                root.style.setProperty('--primary-red', '#333333');
                root.style.setProperty('--card-bg', 'rgba(10, 10, 10, 0.9)');
            } else if (theme === 'neon') {
                root.style.setProperty('--primary-gold', '#00ffcc');
                root.style.setProperty('--primary-red', '#ff00ff');
                root.style.setProperty('--card-bg', 'rgba(10, 0, 20, 0.9)');
            } else {
                // Classic Hollywood
                root.style.setProperty('--primary-gold', '#d4af37');
                root.style.setProperty('--primary-red', '#dc143c');
                root.style.setProperty('--card-bg', 'rgba(20, 20, 40, 0.9)');
            }
            
            // Close panel after applying theme
            ownerPanel.classList.remove('active');
        });

        // --- Initialize App ---
        renderMovies();

    </script>
</body>
</html>
