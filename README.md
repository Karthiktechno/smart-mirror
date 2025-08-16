<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <meta name="theme-color" content="#0c0c0c">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Smart Mirror">
    <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoiU21hcnQgTWlycm9yIiwic2hvcnRfbmFtZSI6Ik1pcnJvciIsInN0YXJ0X3VybCI6Ii8iLCJkaXNwbGF5Ijoic3RhbmRhbG9uZSIsImJhY2tncm91bmRfY29sb3IiOiIjMGMwYzBjIiwidGhlbWVfY29sb3IiOiIjMGMwYzBjIiwiaWNvbnMiOlt7InNyYyI6ImRhdGE6aW1hZ2Uvc3ZnK3htbDtiYXNlNjQsUEhOMlp5QjNhV1IwYUQwaU5USTRJaUJvWldsbmFIUTlJalV5T0NJZ2RtbGxkMEp2ZUQwaU1DQXdJRFV5T0NBMU1qZ2lJSGh0Ykc1elBTSm9kSFJ3T2k4dmQzZDNMbmN6TG05eVp5OHlNREF3TDNOMlp5SStQSEpsWTNRZ2QybGtkR2c5SWpVeU9DSWdhR1ZwWjJoMFBTSTFNamdpSUhKNFBTSXlOaUlnWm1sc2JEMGlJekJqTUdOakluOCtQSFJsZUhRZ2VEMGlNalkySWlCNVBTSXhOekFpSUdacGJHdzlJaU0zWm1abVppSWdkR1Y0ZEMxaGJtTm9iM0k5SW0xcFpHUnNaU0lnWm05dWRDMXphWHBsUFNJeU5DSWdabTl1ZEMxbVlXMXBiSGs5SW1GdWFXRnNMQ0J6WVc1ekxYTmxjbWxtSWo0NU5qUTFQQzkwWlhoMFBqd3ZjM1puUGc9PSIsInR5cGUiOiJpbWFnZS9zdmcreG1sIiwic2l6ZXMiOiI1MjggeDUyOCJ9XX0=">
    <link rel="apple-touch-icon" href="data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iNTI4IiBoZWlnaHQ9IjUyOCIgdmlld0JveD0iMCAwIDUyOCA1MjgiIHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZyI+PHJlY3Qgd2lkdGg9IjUyOCIgaGVpZ2h0PSI1MjgiIHJ4PSIyNiIgZmlsbD0iIzBjMGMwYyIvPjx0ZXh0IHg9IjI2NCIgeT0iMTcwIiBmaWxsPSIjN2ZmZmZmIiB0ZXh0LWFuY2hvcj0ibWlkZGxlIiBmb250LXNpemU9IjI0IiBmb250LWZhbWlseT0iYXJpYWwsIHNhbnMtc2VyaWYiPjk2NDU8L3RleHQ+PC9zdmc+">
    <title>Smart Mirror</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700&display=swap');
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: linear-gradient(135deg, #0c0c0c 0%, #1a1a1a 50%, #2d2d2d 100%);
            color: #ffffff;
            font-family: 'Inter', sans-serif;
            overflow-x: hidden;
            overflow-y: auto;
            height: 100vh;
            position: relative;
            scroll-behavior: smooth;
        }

        /* Subtle mirror effect overlay */
        body::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: 
                radial-gradient(circle at 20% 20%, rgba(255,255,255,0.1) 0%, transparent 50%),
                radial-gradient(circle at 80% 80%, rgba(255,255,255,0.05) 0%, transparent 50%);
            pointer-events: none;
            z-index: 1;
        }

        .mirror-container {
            position: relative;
            z-index: 2;
            min-height: 100vh;
            display: grid;
            grid-template-areas: 
                "datetime weather apps"
                "calendar news apps"
                "footer footer footer";
            grid-template-rows: 1fr 1fr auto;
            grid-template-columns: 1fr 1fr 300px;
            gap: 20px;
            padding: 30px;
        }

        .widget {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 15px;
            padding: 25px;
            box-shadow: 
                0 8px 32px rgba(0, 0, 0, 0.3),
                inset 0 1px 0 rgba(255, 255, 255, 0.1);
            transition: all 0.3s ease;
        }

        .widget:hover {
            background: rgba(255, 255, 255, 0.08);
            transform: translateY(-2px);
        }

        .datetime {
            grid-area: datetime;
            text-align: center;
            display: flex;
            flex-direction: column;
            justify-content: center;
        }

        .time {
            font-size: 4rem;
            font-weight: 300;
            letter-spacing: -2px;
            margin-bottom: 10px;
            background: linear-gradient(135deg, #ffffff 0%, #e0e0e0 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .date {
            font-size: 1.5rem;
            font-weight: 400;
            opacity: 0.8;
            margin-bottom: 10px;
        }

        .day {
            font-size: 1rem;
            opacity: 0.6;
            text-transform: uppercase;
            letter-spacing: 1px;
        }

        .weather {
            grid-area: weather;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }

        .weather-icon {
            font-size: 4rem;
            margin-bottom: 15px;
            filter: drop-shadow(0 4px 8px rgba(0,0,0,0.3));
        }

        .temperature {
            font-size: 3rem;
            font-weight: 600;
            margin-bottom: 10px;
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .weather-desc {
            font-size: 1.2rem;
            opacity: 0.8;
            margin-bottom: 15px;
            text-transform: capitalize;
        }

        .weather-details {
            display: flex;
            gap: 20px;
            font-size: 0.9rem;
            opacity: 0.7;
        }

        .calendar {
            grid-area: calendar;
        }

        .calendar h2 {
            font-size: 1.5rem;
            margin-bottom: 20px;
            color: #4facfe;
            font-weight: 600;
        }

        .calendar-event {
            background: rgba(79, 172, 254, 0.1);
            border-left: 3px solid #4facfe;
            padding: 12px 15px;
            margin-bottom: 12px;
            border-radius: 8px;
            transition: all 0.2s ease;
        }

        .calendar-event:hover {
            background: rgba(79, 172, 254, 0.2);
            transform: translateX(5px);
        }

        .event-time {
            font-size: 0.85rem;
            color: #4facfe;
            font-weight: 600;
        }

        .event-title {
            font-size: 1rem;
            margin-top: 2px;
            font-weight: 500;
        }

        .news {
            grid-area: news;
        }

        .news h2 {
            font-size: 1.5rem;
            margin-bottom: 20px;
            color: #ff6b6b;
            font-weight: 600;
        }

        .news-item {
            background: rgba(255, 107, 107, 0.1);
            border-left: 3px solid #ff6b6b;
            padding: 12px 15px;
            margin-bottom: 12px;
            border-radius: 8px;
            transition: all 0.2s ease;
            cursor: pointer;
        }

        .news-item:hover {
            background: rgba(255, 107, 107, 0.2);
            transform: translateX(5px);
        }

        .news-title {
            font-size: 0.95rem;
            font-weight: 500;
            line-height: 1.4;
            margin-bottom: 5px;
        }

        .news-source {
            font-size: 0.8rem;
            opacity: 0.6;
            text-transform: uppercase;
            letter-spacing: 0.5px;
        }

        .apps {
            grid-area: apps;
        }

        .apps h2 {
            font-size: 1.5rem;
            margin-bottom: 20px;
            color: #9c88ff;
            font-weight: 600;
            text-align: center;
        }

        .apps-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 15px;
            margin-bottom: 20px;
        }

        .app-icon {
            background: rgba(156, 136, 255, 0.1);
            border: 2px solid rgba(156, 136, 255, 0.2);
            border-radius: 20px;
            padding: 20px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s ease;
            aspect-ratio: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-decoration: none;
            color: inherit;
        }

        .app-icon:hover {
            background: rgba(156, 136, 255, 0.2);
            border-color: rgba(156, 136, 255, 0.5);
            transform: translateY(-5px) scale(1.05);
            box-shadow: 0 15px 30px rgba(156, 136, 255, 0.3);
        }

        .app-emoji {
            font-size: 2.5rem;
            margin-bottom: 8px;
            filter: drop-shadow(0 2px 4px rgba(0,0,0,0.3));
        }

        .app-name {
            font-size: 0.8rem;
            font-weight: 500;
            text-align: center;
            opacity: 0.9;
        }

        .quick-actions {
            margin-top: 15px;
        }

        .quick-actions h3 {
            font-size: 1.1rem;
            color: #9c88ff;
            margin-bottom: 10px;
            text-align: center;
            opacity: 0.8;
        }

        .action-button {
            background: linear-gradient(135deg, #9c88ff 0%, #7c4dff 100%);
            border: none;
            border-radius: 12px;
            padding: 12px 20px;
            color: white;
            font-size: 0.9rem;
            font-weight: 500;
            cursor: pointer;
            width: 100%;
            margin-bottom: 8px;
            transition: all 0.2s ease;
            box-shadow: 0 4px 15px rgba(156, 136, 255, 0.3);
        }

        .action-button:hover {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px rgba(156, 136, 255, 0.4);
        }

        .action-button:active {
            transform: translateY(0);
        }

        /* Scroll Navigation Controls */
        .scroll-controls {
            position: fixed;
            right: 20px;
            top: 50%;
            transform: translateY(-50%);
            z-index: 1000;
            display: flex;
            flex-direction: column;
            gap: 10px;
        }

        .scroll-btn {
            background: rgba(156, 136, 255, 0.9);
            border: none;
            border-radius: 50%;
            width: 50px;
            height: 50px;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
            backdrop-filter: blur(10px);
            border: 2px solid rgba(255, 255, 255, 0.1);
            box-shadow: 0 4px 15px rgba(156, 136, 255, 0.3);
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .scroll-btn:hover {
            background: rgba(156, 136, 255, 1);
            transform: scale(1.1);
            box-shadow: 0 8px 25px rgba(156, 136, 255, 0.4);
        }

        .scroll-btn:active {
            transform: scale(0.95);
        }

        /* Scroll indicators */
        .scroll-indicator {
            position: fixed;
            right: 30px;
            bottom: 30px;
            background: rgba(0, 0, 0, 0.5);
            padding: 8px 12px;
            border-radius: 20px;
            font-size: 0.8rem;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            opacity: 0;
            transition: opacity 0.3s ease;
            z-index: 1000;
        }

        .scroll-indicator.show {
            opacity: 1;
        }

        /* Section markers for smooth scrolling */
        .scroll-section {
            scroll-margin-top: 20px;
        }

        /* Enhanced mobile scrolling */
        .mobile-scroll-hint {
            position: fixed;
            bottom: 10px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(156, 136, 255, 0.9);
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 0.8rem;
            backdrop-filter: blur(10px);
            z-index: 1000;
            animation: bounce 2s infinite;
            display: none;
        }

        @keyframes bounce {
            0%, 20%, 50%, 80%, 100% {
                transform: translateX(-50%) translateY(0);
            }
            40% {
                transform: translateX(-50%) translateY(-10px);
            }
            60% {
                transform: translateX(-50%) translateY(-5px);
            }
        }

        .footer {
            grid-area: footer;
            text-align: center;
            padding: 20px 0;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            font-size: 0.9rem;
            opacity: 0.5;
        }

        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 2px solid rgba(255,255,255,0.3);
            border-radius: 50%;
            border-top-color: #4facfe;
            animation: spin 1s ease-in-out infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* Responsive design */
        @media (max-width: 1200px) {
            .mirror-container {
                grid-template-areas: 
                    "datetime weather"
                    "calendar news"
                    "apps apps"
                    "footer footer";
                grid-template-columns: 1fr 1fr;
                grid-template-rows: auto auto auto auto;
            }
            
            .apps-grid {
                grid-template-columns: repeat(6, 1fr);
            }

            .scroll-controls {
                right: 15px;
            }

            .scroll-btn {
                width: 45px;
                height: 45px;
                font-size: 1.3rem;
            }
        }

        @media (max-width: 768px) {
            .mirror-container {
                grid-template-areas: 
                    "datetime"
                    "weather"
                    "calendar"
                    "news"
                    "apps"
                    "footer";
                grid-template-columns: 1fr;
                grid-template-rows: auto;
                padding: 20px;
                gap: 15px;
                padding-bottom: 80px;
            }
            
            .time {
                font-size: 3rem;
            }
            
            .temperature {
                font-size: 2.5rem;
            }
            
            .apps-grid {
                grid-template-columns: repeat(4, 1fr);
            }

            .scroll-controls {
                right: 10px;
                bottom: 20px;
                top: auto;
                transform: none;
            }

            .scroll-btn {
                width: 40px;
                height: 40px;
                font-size: 1.2rem;
            }

            .mobile-scroll-hint {
                display: block;
            }

            .scroll-indicator {
                bottom: 90px;
                right: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="mirror-container">
        <div class="widget datetime scroll-section" id="datetime-section">
            <div class="time" id="time">--:--</div>
            <div class="date" id="date">Loading...</div>
            <div class="day" id="day">--</div>
        </div>

        <div class="widget weather scroll-section" id="weather-section">
            <div class="weather-icon" id="weatherIcon">🌤️</div>
            <div class="temperature" id="temperature">--°</div>
            <div class="weather-desc" id="weatherDesc">Loading weather...</div>
            <div class="weather-details">
                <span id="humidity">Humidity: --%</span>
                <span id="windSpeed">Wind: -- km/h</span>
            </div>
        </div>

        <div class="widget calendar scroll-section" id="calendar-section">
            <h2>📅 Today's Schedule</h2>
            <div id="calendarEvents">
                <div class="calendar-event">
                    <div class="event-time">9:00 AM</div>
                    <div class="event-title">Team Meeting</div>
                </div>
                <div class="calendar-event">
                    <div class="event-time">2:00 PM</div>
                    <div class="event-title">Project Review</div>
                </div>
                <div class="calendar-event">
                    <div class="event-time">4:30 PM</div>
                    <div class="event-title">Engineering Workshop</div>
                </div>
            </div>
        </div>

        <div class="widget news scroll-section" id="news-section">
            <h2>📰 Headlines</h2>
            <div id="newsContainer">
                <div class="news-item">
                    <div class="news-title">Loading latest news...</div>
                    <div class="news-source">News Source</div>
                </div>
            </div>
        </div>

        <div class="widget apps scroll-section" id="apps-section">
            <h2>📱 Apps</h2>
            <div class="apps-grid">
                <a href="https://netflix.com" target="_blank" class="app-icon" data-app="netflix">
                    <div class="app-emoji">🎬</div>
                    <div class="app-name">Netflix</div>
                </a>
                <a href="https://youtube.com" target="_blank" class="app-icon" data-app="youtube">
                    <div class="app-emoji">📺</div>
                    <div class="app-name">YouTube</div>
                </a>
                <a href="https://spotify.com" target="_blank" class="app-icon" data-app="spotify">
                    <div class="app-emoji">🎵</div>
                    <div class="app-name">Spotify</div>
                </a>
                <a href="https://amazon.com" target="_blank" class="app-icon" data-app="amazon">
                    <div class="app-emoji">🛒</div>
                    <div class="app-name">Amazon</div>
                </a>
                <a href="https://gmail.com" target="_blank" class="app-icon" data-app="gmail">
                    <div class="app-emoji">📧</div>
                    <div class="app-name">Gmail</div>
                </a>
                <a href="https://maps.google.com" target="_blank" class="app-icon" data-app="maps">
                    <div class="app-emoji">🗺️</div>
                    <div class="app-name">Maps</div>
                </a>
            </div>
            
            <div class="quick-actions">
                <h3>Quick Actions</h3>
                <button class="action-button" onclick="toggleLights()">💡 Toggle Lights</button>
                <button class="action-button" onclick="playMusic()">🎼 Play Music</button>
                <button class="action-button" onclick="checkSecurity()">🔒 Security</button>
            </div>
        </div>

        <div class="footer">
            <div>Smart Mirror v1.0 | Last Updated: <span id="lastUpdated">--:--</span></div>
        </div>
    </div>

    <!-- Scroll Controls -->
    <div class="scroll-controls">
        <button class="scroll-btn" onclick="scrollToSection('up')" title="Scroll Up">↑</button>
        <button class="scroll-btn" onclick="scrollToSection('down')" title="Scroll Down">↓</button>
    </div>

    <!-- Scroll Indicator -->
    <div class="scroll-indicator" id="scrollIndicator">Scrolling...</div>

    <!-- Mobile Scroll Hint -->
    <div class="mobile-scroll-hint" id="scrollHint">👆 Swipe up for more</div>

    <script>
        // Sample data and state
        let weatherData = {
            temperature: 22,
            description: "Partly Cloudy",
            humidity: 65,
            windSpeed: 12,
            icon: "🌤️"
        };

        let newsItems = [
            { title: "Revolutionary AI Breakthrough in Engineering", source: "Tech Today" },
            { title: "Sustainable Energy Solutions Show Promise", source: "Green Tech" },
            { title: "Smart City Infrastructure Advances", source: "Urban News" },
            { title: "Quantum Computing Reaches New Milestone", source: "Science Daily" }
        ];

        // Update date and time
        function updateDateTime() {
            const now = new Date();
            const timeOptions = { 
                hour: '2-digit', 
                minute: '2-digit',
                hour12: false 
            };
            const dateOptions = { 
                year: 'numeric', 
                month: 'long', 
                day: 'numeric' 
            };
            const dayOptions = { 
                weekday: 'long' 
            };

            document.getElementById('time').textContent = now.toLocaleTimeString('en-US', timeOptions);
            document.getElementById('date').textContent = now.toLocaleDateString('en-US', dateOptions);
            document.getElementById('day').textContent = now.toLocaleDateString('en-US', dayOptions);
            document.getElementById('lastUpdated').textContent = now.toLocaleTimeString('en-US', timeOptions);
        }

        // Update weather display
        function updateWeather() {
            // Simulate weather data changes
            const weatherIcons = ['☀️', '🌤️', '⛅', '🌧️', '🌨️'];
            const descriptions = ['Sunny', 'Partly Cloudy', 'Cloudy', 'Rainy', 'Snowy'];
            
            weatherData.temperature += Math.floor(Math.random() * 3) - 1; // Random temperature change
            weatherData.humidity = 50 + Math.floor(Math.random() * 40);
            weatherData.windSpeed = 5 + Math.floor(Math.random() * 20);
            
            const weatherIndex = Math.floor(Math.random() * weatherIcons.length);
            weatherData.icon = weatherIcons[weatherIndex];
            weatherData.description = descriptions[weatherIndex];

            document.getElementById('weatherIcon').textContent = weatherData.icon;
            document.getElementById('temperature').textContent = `${weatherData.temperature}°C`;
            document.getElementById('weatherDesc').textContent = weatherData.description;
            document.getElementById('humidity').textContent = `Humidity: ${weatherData.humidity}%`;
            document.getElementById('windSpeed').textContent = `Wind: ${weatherData.windSpeed} km/h`;
        }

        // Update news headlines
        function updateNews() {
            const newsContainer = document.getElementById('newsContainer');
            newsContainer.innerHTML = '';
            
            // Shuffle and display news items
            const shuffledNews = [...newsItems].sort(() => Math.random() - 0.5).slice(0, 3);
            
            shuffledNews.forEach(item => {
                const newsElement = document.createElement('div');
                newsElement.className = 'news-item';
                newsElement.innerHTML = `
                    <div class="news-title">${item.title}</div>
                    <div class="news-source">${item.source}</div>
                `;
                newsContainer.appendChild(newsElement);
            });
        }

        // Add smooth animations on load
        function animateWidgets() {
            const widgets = document.querySelectorAll('.widget');
            widgets.forEach((widget, index) => {
                widget.style.opacity = '0';
                widget.style.transform = 'translateY(20px)';
                
                setTimeout(() => {
                    widget.style.transition = 'all 0.6s ease';
                    widget.style.opacity = '1';
                    widget.style.transform = 'translateY(0)';
                }, index * 200);
            });
        }

        // Initialize the mirror
        function initSmartMirror() {
            updateDateTime();
            updateWeather();
            updateNews();
            animateWidgets();
            
            // Set up intervals for real-time updates
            setInterval(updateDateTime, 1000); // Update every second
            setInterval(updateWeather, 300000); // Update every 5 minutes
            setInterval(updateNews, 600000); // Update every 10 minutes
        }

        // Register service worker for PWA
        if ('serviceWorker' in navigator) {
            window.addEventListener('load', () => {
                navigator.serviceWorker.register('data:application/javascript;base64,c2VsZi5hZGRFdmVudExpc3RlbmVyKCdpbnN0YWxsJywgKGV2ZW50KSA9PiB7IGNvbnNvbGUubG9nKCdTVyBpbnN0YWxsZWQnKTsgfSk7IHNlbGYuYWRkRXZlbnRMaXN0ZW5lcignZmV0Y2gnLCAoZXZlbnQpID0+IHsgZXZlbnQucmVzcG9uZFdpdGgoZmV0Y2goZXZlbnQucmVxdWVzdCkpOyB9KTs=')
                    .then(() => console.log('Smart Mirror PWA ready!'))
                    .catch(() => console.log('PWA registration failed'));
            });
        }

        // Start the smart mirror when page loads
        window.addEventListener('load', initSmartMirror);

        // Scroll functionality
        let currentSection = 0;
        const sections = ['datetime-section', 'weather-section', 'calendar-section', 'news-section', 'apps-section'];
        
        function scrollToSection(direction) {
            const indicator = document.getElementById('scrollIndicator');
            
            if (direction === 'up') {
                if (currentSection > 0) {
                    currentSection--;
                } else {
                    currentSection = sections.length - 1; // Go to last section
                }
            } else {
                if (currentSection < sections.length - 1) {
                    currentSection++;
                } else {
                    currentSection = 0; // Go to first section
                }
            }
            
            const targetSection = document.getElementById(sections[currentSection]);
            targetSection.scrollIntoView({ behavior: 'smooth' });
            
            // Show scroll indicator
            indicator.textContent = `Section ${currentSection + 1}/${sections.length}`;
            indicator.classList.add('show');
            setTimeout(() => {
                indicator.classList.remove('show');
            }, 1500);
            
            // Haptic feedback
            if (navigator.vibrate) {
                navigator.vibrate(30);
            }
        }

        // Keyboard navigation
        document.addEventListener('keydown', (e) => {
            if (e.key === 'ArrowUp' || e.key === 'w' || e.key === 'W') {
                e.preventDefault();
                scrollToSection('up');
            } else if (e.key === 'ArrowDown' || e.key === 's' || e.key === 'S') {
                e.preventDefault();
                scrollToSection('down');
            }
        });

        // Touch/swipe support for mobile
        let touchStartY = 0;
        let touchEndY = 0;
        
        document.addEventListener('touchstart', (e) => {
            touchStartY = e.changedTouches[0].screenY;
        }, { passive: true });
        
        document.addEventListener('touchend', (e) => {
            touchEndY = e.changedTouches[0].screenY;
            handleSwipe();
        }, { passive: true });
        
        function handleSwipe() {
            const swipeThreshold = 50;
            const diff = touchStartY - touchEndY;
            
            if (Math.abs(diff) > swipeThreshold) {
                if (diff > 0) {
                    // Swipe up - scroll down
                    scrollToSection('down');
                } else {
                    // Swipe down - scroll up
                    scrollToSection('up');
                }
            }
        }

        // Auto-hide scroll hint after 5 seconds
        setTimeout(() => {
            const hint = document.getElementById('scrollHint');
            if (hint) {
                hint.style.display = 'none';
            }
        }, 5000);

        // Detect scroll position to update current section
        function updateCurrentSection() {
            const scrollPosition = window.scrollY + window.innerHeight / 2;
            
            sections.forEach((sectionId, index) => {
                const section = document.getElementById(sectionId);
                if (section) {
                    const rect = section.getBoundingClientRect();
                    const sectionTop = window.scrollY + rect.top;
                    const sectionBottom = sectionTop + rect.height;
                    
                    if (scrollPosition >= sectionTop && scrollPosition <= sectionBottom) {
                        currentSection = index;
                    }
                }
            });
        }

        // Listen for manual scrolling
        let scrollTimeout;
        window.addEventListener('scroll', () => {
            clearTimeout(scrollTimeout);
            scrollTimeout = setTimeout(updateCurrentSection, 100);
        }, { passive: true });

        // Add some interactivity
        document.addEventListener('click', (e) => {
            if (e.target.closest('.news-item')) {
                e.target.closest('.news-item').style.transform = 'scale(1.02)';
                setTimeout(() => {
                    e.target.closest('.news-item').style.transform = '';
                }, 200);
            }
        });

        // Smart home functions
        function toggleLights() {
            const button = event.target;
            const isOn = button.textContent.includes('💡');
            
            if (isOn) {
                button.textContent = '🔆 Lights ON';
                button.style.background = 'linear-gradient(135deg, #ffd700 0%, #ffb300 100%)';
                setTimeout(() => {
                    button.textContent = '💡 Toggle Lights';
                    button.style.background = 'linear-gradient(135deg, #9c88ff 0%, #7c4dff 100%)';
                }, 2000);
            }
            
            // Add haptic feedback
            if (navigator.vibrate) {
                navigator.vibrate(50);
            }
        }

        function playMusic() {
            const button = event.target;
            button.textContent = '🎵 Playing...';
            button.style.background = 'linear-gradient(135deg, #1db954 0%, #1ed760 100%)';
            
            setTimeout(() => {
                button.textContent = '🎼 Play Music';
                button.style.background = 'linear-gradient(135deg, #9c88ff 0%, #7c4dff 100%)';
            }, 3000);
        }

        function checkSecurity() {
            const button = event.target;
            button.textContent = '🔍 Checking...';
            button.style.background = 'linear-gradient(135deg, #ff6b6b 0%, #ff5252 100%)';
            
            setTimeout(() => {
                button.textContent = '✅ All Secure';
                button.style.background = 'linear-gradient(135deg, #4caf50 0%, #45a049 100%)';
                setTimeout(() => {
                    button.textContent = '🔒 Security';
                    button.style.background = 'linear-gradient(135deg, #9c88ff 0%, #7c4dff 100%)';
                }, 2000);
            }, 1500);
        }

        // Add app launch tracking
        document.addEventListener('click', (e) => {
            if (e.target.closest('.app-icon')) {
                const appName = e.target.closest('.app-icon').getAttribute('data-app');
                console.log(`Launching ${appName}...`);
                
                // Add visual feedback
                e.target.closest('.app-icon').style.transform = 'scale(0.95)';
                setTimeout(() => {
                    e.target.closest('.app-icon').style.transform = '';
                }, 150);
            }
        });
    </script>
</body>
</html>
