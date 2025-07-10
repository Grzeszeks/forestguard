<!DOCTYPE html>
<html lang="pl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>ForestGuard AI</title>
    
    <!-- Ustawienia dla Progresywnej Aplikacji Internetowej (PWA) -->
    <meta name="theme-color" content="#1a202c"/>
    <link rel="apple-touch-icon" href="https://placehold.co/192x192/1a202c/e2e8f0?text=FGAI">
    <link rel="manifest" href="data:application/manifest+json;base64,ewogICJuYW1lIjogIkZvcmVzdEd1YXJkIEFJIiwKICAic2hvcnRfbmFtZSI6ICJGb3Jlc3RHdWFyZCIsCiAgImRlc2NyaXB0aW9uIjogIkludGVsaWdlbnRueSBNb25pdG9yaW5nIExlxZtueSIsCiAgInN0YXJ0X3VybCI6ICIuIiwKICAiZGlzcGxheSI6ICJzdGFuZGFsb25lIiwKICAiYmFja2dyb3VuZF9jb2xvciI6ICIjMWEyMDJjIiwKICAidGhlbWVfY29sb3IiOiAiIzFhMjAyYyIsCiAgImljb25zIjogWwogICAgewogICAgICAic3JjIjogImh0dHBzOi8vcGxhY2Vob2xkLmNvLzE5MngxOTIvMWEyMDJjL2UyZThmMD90ZXh0PUZHQUkiLAogICAgICAic2l6ZXMiOiAiMTkyeDE5MiIsCiAgICAgICJ0eXBlIjogImltYWdlL3BuZyIKICAgIH0sCiAgICB7CiAgICAgICJzcmMiOiAiaHR0cHM6Ly9wbGFjZWhvbGQuY28vNTEyeDUxMi8xYTIwMmMvZTJlOGYwP3RleHQ9RkdBSSIsCiAgICAgICJzaXplcyI6ICI1MTJ4NTEyIiwKICAgICAgInR5cGUiOiAiaW1hZ2UvcG5nIgogICAgfQogIF0KfQo=">

    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script src="https://unpkg.com/lucide@latest"></script>
    
    <!-- Zależności dla interaktywnej mapy Leaflet.js -->
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
    
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }
        .screen {
            display: none;
        }
        .screen.active {
            display: flex;
            flex-direction: column;
            height: 100%;
        }
        .no-scrollbar::-webkit-scrollbar {
            display: none;
        }
        .no-scrollbar {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
        #map-container {
            flex-grow: 1;
            background-color: #1a202c;
            cursor: default;
        }
        #map-container.add-mode {
            cursor: crosshair;
        }
        .leaflet-marker-icon.leaflet-draggable {
            cursor: move;
        }
        .leaflet-control-attribution a {
            color: #9ca3af;
        }
        /* Style dla popupu na mapie */
        .custom-popup .leaflet-popup-content-wrapper {
            background: rgba(31, 41, 55, 0.9); /* Ciemniejszy, bardziej spójny kolor */
            backdrop-filter: blur(8px);
            -webkit-backdrop-filter: blur(8px);
            color: #e2e8f0;
            border-radius: 12px;
            border: 1px solid #4a5568;
            box-shadow: 0 8px 24px rgba(0,0,0,0.5);
            padding: 12px;
        }
        .custom-popup .leaflet-popup-tip {
            background: rgba(31, 41, 55, 0.9);
        }
        .custom-popup .leaflet-popup-close-button {
            color: #e2e8f0;
            top: 8px;
            right: 8px;
        }
        .custom-popup .leaflet-popup-close-button:hover {
            color: #fff;
        }
        .custom-popup table {
            font-size: 0.8rem;
            border-collapse: collapse;
        }
        .custom-popup th, .custom-popup td {
            padding: 6px 4px;
            text-align: center;
        }
        .custom-popup th {
            color: #9ca3af;
        }
        .custom-popup td:first-child {
            text-align: left;
            font-weight: 500;
        }
        .nav-item > * {
            pointer-events: none;
        }
        .delete-marker-btn {
            background-color: #ef4444;
            color: white;
            padding: 4px 8px;
            border-radius: 6px;
            border: none;
            font-size: 0.75rem;
            margin-top: 8px;
            cursor: pointer;
            width: 100%;
        }
        .delete-marker-btn:hover {
            background-color: #dc2626;
        }
        .map-controls {
            position: absolute;
            bottom: 20px;
            right: 16px;
            z-index: 1000;
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        .map-controls button {
            box-shadow: 0 4px 12px rgba(0,0,0,0.4);
        }
    </style>
</head>
<body class="bg-gray-900 flex items-center justify-center min-h-screen p-4">

    <div class="w-full max-w-sm h-[800px] bg-black rounded-[40px] border-[10px] border-gray-700 shadow-2xl overflow-hidden flex flex-col relative">
        <!-- ZNAK WODNY -->
        <div class="absolute top-3 right-5 text-xs text-gray-600 font-semibold z-50">by Grzeszeks</div>

        <!-- Tło z teksturą -->
        <div class="absolute inset-0 bg-[url('https://www.transparenttextures.com/patterns/dark-wood.png')] opacity-5"></div>

        <!-- Kontener na ekrany -->
        <main class="flex-grow overflow-y-auto no-scrollbar relative pb-24">

            <!-- EKRAN ŁADOWANIA -->
            <div id="screen-loading" class="screen active h-full flex flex-col items-center justify-center text-white bg-gray-900">
                <div class="w-20 h-20 text-green-400">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" class="w-full h-full">
                        <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z" />
                        <path d="M12 15v-3" />
                        <path d="M12 12l2.5 2.5" />
                        <path d="M12 12l-2.5 2.5" />
                        <path d="M12 9l2.5 2.5" />
                        <path d="M12 9l-2.5 2.5" />
                    </svg>
                </div>
                <h1 class="text-2xl font-bold text-white mt-4">ForestGuard AI</h1>
                <p class="text-green-400">Inteligentny Monitoring Leśny</p>
                <i data-lucide="loader" class="animate-spin text-green-500 mt-8 w-10 h-10"></i>
                <p id="loading-status" class="text-gray-400 mt-4 text-sm">Inicjalizacja...</p>
            </div>
            
            <div id="app-content" class="hidden h-full">
                <header class="p-6 pb-2 flex items-center justify-between">
                    <div class="flex items-center space-x-3">
                        <div class="w-12 h-12 text-green-400">
                           <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" class="w-full h-full">
                                <path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z" />
                                <path d="M12 15v-3" />
                                <path d="M12 12l2.5 2.5" />
                                <path d="M12 12l-2.5 2.5" />
                                <path d="M12 9l2.5 2.5" />
                                <path d="M12 9l-2.5 2.5" />
                            </svg>
                        </div>
                        <div>
                            <h1 class="text-xl font-bold text-white">ForestGuard AI</h1>
                            <p class="text-xs text-green-400 font-medium">Monitoring Leśny</p>
                        </div>
                    </div>
                    <div class="w-8 h-8 rounded-full bg-gray-700 flex items-center justify-center"><i data-lucide="wind" class="w-4 h-4 text-blue-300"></i></div>
                </header>

                <!-- EKRAN GŁÓWNY (HOME) -->
                <div id="screen-home" class="screen p-6 pt-4 text-white bg-transparent relative z-10">
                    <div class="mt-4 bg-gray-800/50 p-3 rounded-lg flex items-center space-x-2 text-sm">
                        <i data-lucide="map-pin" class="w-4 h-4 text-green-400"></i>
                        <span>Lokalizacja: <strong id="location-display">...</strong></span>
                    </div>
                    <div class="mt-6 space-y-3">
                        <div id="notification-fire-container" class="bg-gray-800/70 p-4 rounded-xl border-l-4 flex items-start space-x-4 shadow-md">
                            <i data-lucide="flame" class="w-6 h-6 mt-1"></i>
                            <div class="flex-1">
                                <p id="notification-fire" class="text-sm font-semibold text-gray-200">...</p>
                                <p id="notification-fire-details" class="text-xs text-gray-400 mt-1">...</p>
                            </div>
                        </div>
                        <div id="notification-wind-container" class="bg-gray-800/70 p-4 rounded-xl border-l-4 flex items-center space-x-4 shadow-md">
                            <i data-lucide="wind" class="w-5 h-5"></i>
                            <p id="notification-wind" class="text-sm text-gray-200 flex-1">...</p>
                        </div>
                    </div>
                    <div class="mt-8 rounded-2xl overflow-hidden border-2 border-green-500/30 shadow-lg cursor-pointer" data-nav-card="map">
                        <div class="relative">
                            <img src="https://images.pexels.com/photos/167699/pexels-photo-167699.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=2" alt="Widok z drona na deszczowy, mglisty las" class="w-full h-56 object-cover">
                            <div class="absolute inset-0 bg-gradient-to-t from-black/80 to-transparent"></div>
                            <div class="absolute bottom-4 left-4">
                                <h3 class="text-lg font-bold">Mapa Ryzyka</h3>
                                <p class="text-sm text-gray-300">Aktualizacja: <span id="map-update-time"></span></p>
                            </div>
                            <div class="absolute top-4 right-4 bg-white/20 p-2 rounded-full"><i data-lucide="chevron-right" class="w-5 h-5"></i></div>
                        </div>
                    </div>
                    <div class="mt-8">
                        <button id="report-threat-btn" class="w-full bg-gradient-to-r from-orange-500 to-red-600 text-white font-bold py-4 px-6 rounded-xl shadow-lg hover:shadow-xl hover:scale-105 transform transition-all duration-300 flex items-center justify-center space-x-3">
                            <i data-lucide="siren" class="w-6 h-6"></i>
                            <span>Zgłoś zagrożenie</span>
                        </button>
                    </div>
                </div>

                <!-- EKRAN MAPY -->
                <div id="screen-map" class="screen text-white relative">
                    <div class="p-6 pb-2">
                        <h2 class="text-2xl font-bold">Interaktywna Mapa</h2>
                        <p class="text-gray-400 text-sm">Kliknij, aby sprawdzić ryzyko lub dodać punkt.</p>
                    </div>
                    <div id="map-notification" class="hidden text-center p-2 bg-blue-600 text-white font-semibold transition-opacity duration-300"></div>
                    <div id="map-container" class="border-y-2 border-green-500/30"></div>
                    <div class="map-controls">
                        <button id="moderate-points-btn" class="bg-blue-600 hover:bg-blue-700 text-white font-bold p-3 rounded-full flex items-center justify-center transition-all">
                            <i data-lucide="shield" class="w-6 h-6"></i>
                        </button>
                        <button id="add-point-btn" class="bg-green-600 hover:bg-green-700 text-white font-bold p-3 rounded-full flex items-center justify-center transition-all">
                            <i data-lucide="plus" class="w-6 h-6"></i>
                        </button>
                    </div>
                </div>

                <!-- EKRAN PREDYKCJI AI -->
                <div id="screen-ai" class="screen p-6 pt-4 text-white">
                    <h2 class="text-2xl font-bold mt-4">Predykcje AI w Twojej okolicy</h2>
                    <p class="text-gray-400">Analiza zagrożeń na 48h. Ostatnia: <span id="ai-update-time"></span></p>
                    <div id="ai-predictions-container" class="mt-8 space-y-4"></div>
                </div>

                <!-- EKRAN ALERTÓW -->
                <div id="screen-alerts" class="screen p-6 pt-4 text-white">
                    <h2 class="text-2xl font-bold mt-4">Aktywne Alerty w Twojej okolicy</h2>
                    <p class="text-gray-400">Lista aktualnych ostrzeżeń i zdarzeń.</p>
                    <div id="alerts-container" class="mt-6 space-y-4"></div>
                </div>
            </div>
        </main>

        <!-- Dolna nawigacja -->
        <div id="navigation-bar-container" class="absolute bottom-4 left-0 right-0 px-4 z-40 hidden">
             <nav class="w-full bg-gray-900/80 backdrop-blur-sm border border-gray-700/50 flex justify-around items-center p-2 z-10 rounded-full shadow-lg">
                <button data-nav="home" class="nav-item flex flex-col items-center justify-center space-y-1 w-16 h-12 transition-colors duration-300 text-green-400 cursor-pointer">
                    <i data-lucide="home" class="w-7 h-7"></i><span class="text-xs font-medium">Home</span>
                </button>
                <button data-nav="map" class="nav-item flex flex-col items-center justify-center space-y-1 w-16 h-12 transition-colors duration-300 text-gray-400 hover:text-white cursor-pointer">
                    <i data-lucide="map" class="w-7 h-7"></i><span class="text-xs font-medium">Mapa</span>
                </button>
                <button data-nav="ai" class="nav-item flex flex-col items-center justify-center space-y-1 w-16 h-12 transition-colors duration-300 text-gray-400 hover:text-white cursor-pointer">
                    <i data-lucide="brain-circuit" class="w-7 h-7"></i><span class="text-xs font-medium">Predykcje</span>
                </button>
                <button data-nav="alerts" class="nav-item flex flex-col items-center justify-center space-y-1 w-16 h-12 transition-colors duration-300 text-gray-400 hover:text-white cursor-pointer">
                    <i data-lucide="bell" class="w-7 h-7"></i><span class="text-xs font-medium">Alerty</span>
                </button>
            </nav>
        </div>
        
        <!-- MODAL (OKNO DIALOGOWE) -->
        <div id="modal" class="hidden absolute inset-0 bg-black/60 z-50 flex items-center justify-center p-4">
            <div id="modal-content" class="bg-gray-800 text-white p-6 rounded-2xl w-full max-w-xs border border-gray-600 shadow-2xl">
                <!-- Treść będzie wstawiana przez JS -->
            </div>
        </div>
    </div>

<script>
document.addEventListener('DOMContentLoaded', () => {

    // === SELEKTORY DOM ===
    const selectors = {
        screens: document.querySelectorAll('.screen'),
        navItems: document.querySelectorAll('.nav-item'),
        clickableMapCard: document.querySelector('[data-nav-card="map"]'),
        modal: document.getElementById('modal'),
        modalContent: document.getElementById('modal-content'),
        loadingStatus: document.getElementById('loading-status'),
        appContent: document.getElementById('app-content'),
        navContainer: document.getElementById('navigation-bar-container'),
        screenLoading: document.getElementById('screen-loading'),
        locationDisplay: document.getElementById('location-display'),
        mapUpdateTime: document.getElementById('map-update-time'),
        aiUpdateTime: document.getElementById('ai-update-time'),
        notificationFire: document.getElementById('notification-fire'),
        notificationFireDetails: document.getElementById('notification-fire-details'),
        notificationFireContainer: document.getElementById('notification-fire-container'),
        notificationWind: document.getElementById('notification-wind'),
        notificationWindContainer: document.getElementById('notification-wind-container'),
        aiPredictionsContainer: document.getElementById('ai-predictions-container'),
        alertsContainer: document.getElementById('alerts-container'),
        addPointBtn: document.getElementById('add-point-btn'),
        mapContainer: document.getElementById('map-container'),
        reportThreatBtn: document.getElementById('report-threat-btn'),
        moderatePointsBtn: document.getElementById('moderate-points-btn'),
        mapNotification: document.getElementById('map-notification'),
    };

    // === STAN APLIKACJI ===
    let mapInstance = null;
    let genericPopup = null;
    let isAddMode = false;
    let isModerateMode = false;
    let userMarkers = {};
    let objectMarkers = [];
    let icons = {};
    const userCoords = [53.779, 20.494]; // Olsztyn
    
    const appState = {
        isDataReady: false,
        weatherData: null,
        predictions: [],
        alerts: []
    };

    // === POBIERANIE I WERYFIKACJA DANYCH ===
    /**
     * Oblicza zagrożenie pożarowe na podstawie danych pogodowych.
     * Metodologia jest uproszczonym modelem bazującym na czynnikach używanych przez IBL.
     * @param {object} weather - Obiekt z danymi pogodowymi { temp, humidity, wind, rain }.
     * @returns {number} - Poziom zagrożenia (0-3).
     */
    function calculateFireThreat(weather) {
        let score = 0;
        // Temperatura
        if (weather.temp > 20) score += 0.5;
        if (weather.temp > 25) score += 1;
        // Wilgotność
        if (weather.humidity < 60) score += 0.5;
        if (weather.humidity < 40) score += 1;
        // Wiatr
        if (weather.wind > 15) score += 0.5;
        // Opady (najważniejszy czynnik)
        if (weather.rain > 0.1) score = 0; // Deszcz zeruje ryzyko
        if (weather.rain > 0) score *= 0.5; // Lekki deszcz zmniejsza ryzyko

        return Math.min(3, Math.round(score)); // Ograniczenie do skali 0-3
    }
    
    /**
     * Pobiera zweryfikowane dane pogodowe z publicznego API IMGW.
     */
    async function fetchVerifiedWeatherData() {
        try {
            selectors.loadingStatus.textContent = "Weryfikacja danych w IMGW...";
            const response = await fetch('https://danepubliczne.imgw.pl/api/data/synop/station/olsztyn');
            if (!response.ok) {
                throw new Error(`Błąd sieci: ${response.status}`);
            }
            const olsztynData = await response.json();
            
            appState.weatherData = {
                timestamp: new Date(),
                stationName: "Olsztyn (IMGW)",
                temperature: parseFloat(olsztynData.temperatura),
                windSpeed: parseFloat(olsztynData.predkosc_wiatru), // w m/s
                humidity: parseFloat(olsztynData.wilgotnosc_wzgledna),
                rain: parseFloat(olsztynData.suma_opadu),
                pressure: parseFloat(olsztynData.cisnienie),
                description: olsztynData.stan_pogody || "Brak opisu"
            };

        } catch (error) {
            console.error("Nie udało się pobrać zweryfikowanych danych:", error);
            selectors.loadingStatus.textContent = "Błąd pobierania danych.";
            // Ustaw dane awaryjne w przypadku błędu
            appState.weatherData = {
                timestamp: new Date(), stationName: "Błąd Danych", temperature: 0, windSpeed: 0, humidity: 0, rain: 0, pressure: 0, description: "Brak połączenia"
            };
        }
    }

    /**
     * Generuje dynamiczne alerty i predykcje na podstawie aktualnych danych pogodowych.
     */
    function generateDynamicContent() {
        if (!appState.weatherData) return;
        
        selectors.loadingStatus.textContent = "Analiza ryzyka i generowanie predykcji...";
        const { temperature, windSpeed, humidity, rain, pressure } = appState.weatherData;
        const windSpeedKmh = Math.round(windSpeed * 3.6);
        const now = new Date();
        const month = now.getMonth(); // 0-11

        const fireThreat = calculateFireThreat({ temp: temperature, humidity: humidity, wind: windSpeedKmh, rain: rain });

        // --- BAZA POTENCJALNYCH ALERTÓW I PREDYKCJI ---
        const potentialAlerts = [
            // Alerty najwyższego priorytetu
            { id: 301, level: 3, icon: 'cloud-lightning', color: 'text-red-500', title: `Alert IMGW 2. st. - Gwałtowne burze`, details: (d) => `Możliwe burze z porywami wiatru do ${Math.round(d.windSpeed * 3.6) + 30} km/h i gradem.`, source: "IMGW-PIB", condition: (d) => d.temperature > 25 && d.humidity > 70 },
            { id: 302, level: 3, icon: 'thermometer-sun', color: 'text-red-500', title: `Alert IMGW 2. st. - Upał`, details: (d) => `Prognozowana temperatura maksymalna ${d.temperature}°C. Ogranicz przebywanie na słońcu.`, source: "IMGW-PIB", condition: (d) => d.temperature > 30 },
            
            // Alerty średniego priorytetu
            { id: 201, level: 2, icon: 'wind', color: 'text-yellow-400', title: `Alert IMGW 1. st. - Silny wiatr`, details: (d) => `Prognozowane porywy wiatru do ${Math.round(d.windSpeed * 3.6)} km/h. Zabezpiecz luźne przedmioty.`, source: "IMGW-PIB", condition: (d) => Math.round(d.windSpeed * 3.6) > 50 },
            { id: 202, level: 2, icon: 'snowflake', color: 'text-blue-400', title: `Alert IMGW 1. st. - Intensywne opady śniegu`, details: (d) => `Możliwy przyrost pokrywy śnieżnej o 10-15 cm w ciągu 12h.`, source: "IMGW-PIB", condition: (d) => d.rain > 5 && d.temperature < 1 && (month === 11 || month < 3) },
            { id: 203, level: 2, icon: 'cloud-rain', color: 'text-cyan-400', title: "Ostrzeżenie hydrologiczne", details: (d) => `Intensywne opady (${d.rain} mm) mogą powodować wezbrania rzek.`, source: "Wody Polskie", condition: (d) => d.rain > 15 },
            { id: 204, level: 2, icon: 'car', color: 'text-gray-400', title: "Ostrzeżenie drogowe - Gołoledź", details: (d) => `Temperatura bliska 0°C przy opadach deszczu stwarza ryzyko gołoledzi.`, source: "GDDKiA", condition: (d) => d.temperature < 2 && d.temperature > -2 && d.rain > 0 },

            // Alerty informacyjne
            { id: 101, level: 1, icon: 'leaf', color: 'text-green-400', title: "Wysokie stężenie pyłków", details: (d) => `Pylenie traw i/lub brzozy. Alergicy mogą odczuwać objawy.`, source: "ODOR.PL", condition: (d) => d.temperature > 15 && d.windSpeed > 2 && d.rain === 0 && (month >= 3 && month <= 6) },
            { id: 102, level: 1, icon: 'bug', color: 'text-lime-400', title: "Zwiększona aktywność kleszczy", details: (d) => `Ciepła i wilgotna pogoda sprzyja aktywności kleszczy.`, source: "PSSE Olsztyn", condition: (d) => d.temperature > 10 && d.humidity > 60 },
            { id: 103, level: 1, icon: 'sun', color: 'text-amber-400', title: "Wysoki indeks UV", details: (d) => `Długotrwała ekspozycja na słońce bez ochrony jest niewskazana.`, source: "System ForestGuard", condition: (d) => (month >= 4 && month <= 8) && !d.description.toLowerCase().includes('pochmurno') && !d.description.toLowerCase().includes('deszcz') },
            { id: 104, level: 1, icon: 'cloud-fog', color: 'text-gray-500', title: "Gęsta mgła", details: (d) => `Widoczność ograniczona do 100-200m. Zachowaj ostrożność.`, source: "System ForestGuard", condition: (d) => d.humidity > 95 && d.windSpeed < 2 },
        ];

        const potentialPredictions = [
            { id: 1, title: "Ryzyko wiatrołomów", content: (d) => `Silny wiatr (do ${Math.round(d.windSpeed * 3.6)} km/h) w połączeniu z namokniętą glebą po opadach (${d.rain} mm) zwiększa ryzyko łamania się drzew.`, riskLevel: "Wysokie", riskColor: "text-orange-400", recommendation: () => "Unikaj wchodzenia do lasu i parkowania pod drzewami.", condition: (d) => Math.round(d.windSpeed * 3.6) > 40 && d.rain > 1 },
            { id: 2, title: "Wzrost zagrożenia pożarowego", content: (d) => `Brak opadów przez ostatnie 24h, niska wilgotność ściółki (${d.humidity}%) i wysoka temperatura (${d.temperature}°C) znacząco podnoszą ryzyko pożaru.`, riskLevel: "Wysokie", riskColor: "text-orange-400", recommendation: () => "Zachowaj szczególną ostrożność w lasach, nie używaj otwartego ognia.", condition: (d) => fireThreat >= 2 },
            { id: 3, title: "Dobre warunki do wypoczynku", content: (d) => `Stabilne ciśnienie (${d.pressure} hPa) i przyjemna temperatura (${d.temperature}°C) tworzą idealne warunki do aktywności na zewnątrz.`, riskLevel: "Niskie", riskColor: "text-green-400", recommendation: () => "Skorzystaj z pogody i spędź czas na świeżym powietrzu.", condition: (d) => d.pressure > 1015 && d.temperature > 15 && d.temperature < 25 && d.windSpeed < 5 },
            { id: 4, title: "Niekorzystny biomet", content: (d) => `Gwałtowne zmiany ciśnienia lub wysoka wilgotność (${d.humidity}%) mogą powodować bóle głowy i ogólne osłabienie.`, riskLevel: "Umiarkowane", riskColor: "text-yellow-400", recommendation: () => "Zadbaj o nawodnienie i unikaj nadmiernego wysiłku.", condition: (d) => d.pressure < 995 || d.humidity > 85 && d.temperature > 20 },
            { id: 5, title: "Trudne warunki drogowe", content: (d) => `Intensywne opady deszczu (${d.rain} mm) mogą powodować ograniczenie widoczności i ryzyko aquaplaningu.`, riskLevel: "Wysokie", riskColor: "text-orange-400", recommendation: () => "Dostosuj prędkość do warunków, zachowaj większy odstęp.", condition: (d) => d.rain > 5 },
            { id: 6, title: "Możliwe lokalne podtopienia", content: (d) => `Po intensywnych opadach możliwe jest wystąpienie lokalnych podtopień na terenach zurbanizowanych i w obniżeniach terenu.`, riskLevel: "Umiarkowane", riskColor: "text-yellow-400", recommendation: () => "Sprawdź drożność rynien i kratek ściekowych. Unikaj przejeżdżania przez duże kałuże.", condition: (d) => d.rain > 10 },
            { id: 7, title: "Zwiększona aktywność zwierząt leśnych", content: (d) => `Nagłe zmiany pogody, jak nadchodząca burza, mogą powodować wzmożoną i nieprzewidywalną aktywność dzikich zwierząt.`, riskLevel: "Umiarkowane", riskColor: "text-yellow-400", recommendation: () => "Zachowaj szczególną ostrożność na drogach przebiegających przez tereny leśne.", condition: (d) => d.pressure < 1000 && d.windSpeed > 10 },
            { id: 8, title: "Ryzyko przymrozków przygruntowych", content: (d) => `Spadek temperatury poniżej 2°C przy bezchmurnym niebie może spowodować przymrozki, groźne dla upraw.`, riskLevel: "Wysokie", riskColor: "text-orange-400", recommendation: () => "Zabezpiecz wrażliwe rośliny agrowłókniną.", condition: (d) => d.temperature < 2 && (month >= 3 && month <= 5) && !d.description.toLowerCase().includes('pochmurno') },
            { id: 9, title: "Komfort termiczny", content: (d) => `Temperatura odczuwalna jest w strefie komfortu, co sprzyja dobremu samopoczuciu.`, riskLevel: "Niskie", riskColor: "text-green-400", recommendation: () => "Warunki są optymalne, nie jest wymagany specjalny ubiór.", condition: (d) => d.temperature >= 18 && d.temperature <= 24 && d.humidity >= 40 && d.humidity <= 60 },
            { id: 10, title: "Ryzyko udaru cieplnego", content: (d) => `Wysoka temperatura (${d.temperature}°C) w połączeniu z dużą wilgotnością (${d.humidity}%) znacznie obciąża organizm.`, riskLevel: "Wysokie", riskColor: "text-orange-400", recommendation: () => "Pij dużo wody, unikaj słońca w południe, szukaj cienia.", condition: (d) => d.temperature > 28 && d.humidity > 60 },
            { id: 11, title: "Dobra widoczność i przejrzystość powietrza", content: (d) => `Suche powietrze i stabilna atmosfera zapewniają doskonałą widoczność, idealną do fotografii krajobrazowej i obserwacji.`, riskLevel: "Niskie", riskColor: "text-green-400", recommendation: () => "Idealny moment na wycieczkę do punktu widokowego.", condition: (d) => d.humidity < 50 && d.windSpeed < 5 && d.rain === 0 },
            { id: 12, title: "Potencjalne burze pyłowe", content: (d) => `Długi okres bezdeszczowy w połączeniu z silnym wiatrem może powodować powstawanie lokalnych burz pyłowych na otwartych terenach rolniczych.`, riskLevel: "Umiarkowane", riskColor: "text-yellow-400", recommendation: () => "Kierowcy powinni być przygotowani na nagłe ograniczenie widoczności.", condition: (d) => fireThreat >= 2 && d.windSpeedKmh > 40 },
        ];

        appState.alerts = potentialAlerts
            .filter(a => a.condition(appState.weatherData))
            .sort((a, b) => b.level - a.level);
            
        appState.predictions = potentialPredictions.filter(p => p.condition(appState.weatherData));
        
        // --- Aktualizacja stanu globalnego ---
        appState.threats = {
            fire: fireThreat,
            wind: windSpeedKmh > 50 ? 2 : (windSpeedKmh > 25 ? 1 : 0),
            storm: (temperature > 20 && humidity > 70) ? 2 : (temperature > 18 && humidity > 60 ? 1 : 0),
        };
    }


    // === BAZY DANYCH ===
    const mapObjects = [
        // ... (reszta bez zmian)
        { type: 'park', name: 'Las Miejski', coords: [53.79, 20.50] },
        { type: 'beach', name: 'Plaża Miejska (J. Ukiel)', coords: [53.774, 20.456] },
        { type: 'foresters-lodge', name: 'Leśniczówka "Kudypy"', coords: [53.768, 20.378] },
        { type: 'park', name: 'Arboretum w Kudypach', coords: [53.766, 20.373] },
        { type: 'foresters-lodge', name: 'Leśniczówka "Stawiguda"', coords: [53.68, 20.41] },
        { type: 'park', name: 'Mazury Golf & Country Club', coords: [53.75, 20.40] },
        { type: 'park', name: 'Skansen w Olsztynku', coords: [53.585, 20.285] },
        { type: 'foresters-lodge', name: 'Leśniczówka "Mierki"', coords: [53.61, 20.33] },
        { type: 'beach', name: 'Plaża w Zielonowie (J. Pluszne)', coords: [53.63, 20.39] },
        { type: 'park', name: 'Zamek w Nidzicy', coords: [53.358, 20.426] },
        { type: 'foresters-lodge', name: 'Nadleśnictwo Nidzica', coords: [53.36, 20.415] },
        { type: 'park', name: 'Rezerwat "Źródła Rzeki Łyny"', coords: [53.48, 20.35] },
        { type: 'watchtower', name: 'Wieża ppoż. "Koniuszyn"', coords: [53.453, 20.697] },
        { type: 'beach', name: 'Plaża w Jabłonce (J. Omulew)', coords: [53.473, 20.730] },
        { type: 'foresters-lodge', name: 'Leśniczówka "Zimna Woda"', coords: [53.323, 20.548] },
        { type: 'foresters-lodge', name: 'Nadleśnictwo Jedwabno', coords: [53.50, 21.01] },
        { type: 'beach', name: 'Plaża w Nartach (J. Świętajno)', coords: [53.58, 21.11] },
        { type: 'scout-camp', name: 'Pole biwakowe "Sasek Wielki"', coords: [53.60, 21.05] },
        { type: 'park', name: 'Rezerwat "Galwica"', coords: [53.56, 20.88] },
        { type: 'foresters-lodge', name: 'Leśniczówka "Małszewo"', coords: [53.54, 21.15] },
        { type: 'beach', name: 'Plaża miejska w Pasymiu', coords: [53.65, 20.785] },
        { type: 'foresters-lodge', name: 'Leśniczówka "Leleszki"', coords: [53.68, 20.76] },
        { type: 'park', name: 'Ruiny zamku w Pasymiu', coords: [53.652, 20.791] },
        { type: 'beach', name: 'Plaża miejska w Szczytnie', coords: [53.56, 20.98] },
        { type: 'park', name: 'Ruiny Zamku Krzyżackiego', coords: [53.565, 20.988] },
        { type: 'watchtower', name: 'Wieża widokowa k. Szczytna', coords: [53.59, 20.96] },
        { type: 'foresters-lodge', name: 'Leśniczówka "Nowy Ramuk"', coords: [53.66, 20.60] },
        { type: 'park', name: 'Rezerwat "Las Warmiński"', coords: [53.64, 20.55] },
        { type: 'scout-camp', name: 'Pole biwakowe "Wikno"', coords: [53.49, 20.65] },
        { type: 'beach', name: 'Plaża w Wiknie (J. Omulew)', coords: [53.485, 20.66] },
        { type: 'foresters-lodge', name: 'Leśniczówka "Jabłonka"', coords: [53.46, 20.75] },
        { type: 'park', name: 'Rezerwat "Koniuszanka II"', coords: [53.44, 20.68] },
    ];

    // === NAWIGACJA ===
    function navigateTo(screenId) {
        selectors.screens.forEach(screen => screen.classList.remove('active'));
        document.getElementById(`screen-${screenId}`).classList.add('active');
        selectors.navItems.forEach(item => {
            const isActive = item.dataset.nav === screenId;
            item.classList.toggle('text-green-400', isActive);
            item.classList.toggle('text-gray-400', !isActive);
        });
        if (screenId === 'map' && mapInstance) {
            requestAnimationFrame(() => {
                mapInstance.invalidateSize();
            });
        }
    }

    // === RENDEROWANIE TREŚCI ===
    function getThreatLevelStyle(level) {
        const styles = {
            0: { text: 'Niski', color: 'text-green-400', abbr: 'N' },
            1: { text: 'Umiarkowany', color: 'text-yellow-400', abbr: 'U' },
            2: { text: 'Wysoki', color: 'text-orange-400', abbr: 'W' },
            3: { text: 'Ekstremalny', color: 'text-red-500', abbr: 'E' },
        };
        const roundedLevel = Math.min(3, Math.max(0, Math.round(level)));
        return styles[roundedLevel];
    }
    
    function haversineDistance(coords1, coords2) {
        function toRad(x) { return x * Math.PI / 180; }
        const R = 6371;
        const dLat = toRad(coords2[0] - coords1[0]);
        const dLon = toRad(coords2[1] - coords1[1]);
        const lat1 = toRad(coords1[0]);
        const lat2 = toRad(coords2[0]);
        const a = Math.sin(dLat / 2) * Math.sin(dLat / 2) + Math.sin(dLon / 2) * Math.sin(dLon / 2) * Math.cos(lat1) * Math.cos(lat2);
        const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
        return R * c;
    }

    function getInterpolatedData(targetCoords, timeOffset = 0) {
        if (!appState.isDataReady) return null;
        
        // Na potrzeby demonstracji, interpolacja jest wyłączona,
        // ponieważ mamy tylko jedną, ale za to zweryfikowaną stację.
        // W przyszłości można dodać więcej stacji i przywrócić interpolację.
        return {
            weather: {
                temperature: appState.weatherData.temperature,
                windSpeed: Math.round(appState.weatherData.windSpeed * 3.6),
                description: appState.weatherData.description
            },
            threats: {
                fire: appState.threats.fire,
                wind: appState.threats.wind,
                storm: appState.threats.storm,
            }
        };
    }

    function createPopupContent(object, coords) {
        if (!appState.isDataReady) return `<div class="p-2 text-sm">Ładowanie danych...</div>`;
        
        const dataNow = getInterpolatedData(coords, 0);

        const title = object.name;
        const subtitle = object.type === 'user-point' ? `<h4 class="font-normal text-xs text-gray-400 -mt-1 mb-2">Lat: ${coords.lat.toFixed(3)}, Lon: ${coords.lng.toFixed(3)}</h4>` : '';
        const deleteButton = object.type === 'user-point' ? `<button class="delete-marker-btn" data-marker-id="${object.markerId}">Usuń punkt</button>` : '';
        
        const popupHtml = `
            <div class="text-white w-[240px]">
                <h3 class="font-bold text-base mb-1">${title}</h3>
                ${subtitle}
                <div class="p-2 rounded-lg bg-black/20 border border-gray-600 mb-3">
                    <div class="flex justify-between items-center text-sm">
                        <span><i data-lucide="thermometer" class="w-4 h-4 inline-block -mt-1 mr-1"></i> ${dataNow.weather.temperature}°C</span>
                        <span class="text-right">${dataNow.weather.description}</span>
                    </div>
                </div>

                <p class="text-xs text-gray-400 mb-1 font-semibold">Aktualne zagrożenia:</p>
                <table class="w-full text-xs">
                    <thead>
                        <tr class="border-b border-gray-600">
                            <th class="pb-1 font-semibold text-left">Zagrożenie</th>
                            <th class="pb-1 font-semibold text-center">Poziom</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr class="border-b border-gray-700">
                            <td class="py-1.5 flex items-center"><i data-lucide="wind" class="w-4 h-4 mr-2 text-blue-300"></i>Wiatr</td>
                            <td class="py-1.5 text-center font-bold ${getThreatLevelStyle(dataNow.threats.wind).color}">${getThreatLevelStyle(dataNow.threats.wind).text}</td>
                        </tr>
                        <tr class="border-b border-gray-700">
                            <td class="py-1.5 flex items-center"><i data-lucide="cloud-lightning" class="w-4 h-4 mr-2 text-yellow-300"></i>Burza</td>
                            <td class="py-1.5 text-center font-bold ${getThreatLevelStyle(dataNow.threats.storm).color}">${getThreatLevelStyle(dataNow.threats.storm).text}</td>
                        </tr>
                        <tr>
                            <td class="py-1.5 flex items-center"><i data-lucide="flame" class="w-4 h-4 mr-2 text-orange-400"></i>Pożar</td>
                            <td class="py-1.5 text-center font-bold ${getThreatLevelStyle(dataNow.threats.fire).color}">${getThreatLevelStyle(dataNow.threats.fire).text}</td>
                        </tr>
                    </tbody>
                </table>
                 ${deleteButton}
            </div>
        `;
        
        const tempDiv = document.createElement('div');
        tempDiv.innerHTML = popupHtml;
        lucide.createIcons({
            nodes: tempDiv.querySelectorAll('[data-lucide]')
        });
        return tempDiv.innerHTML;
    }

    function initializeMap() {
        if (mapInstance) return;
        
        mapInstance = L.map('map-container', { zoomControl: false }).setView(userCoords, 10);
        L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', { attribution: '© OpenStreetMap © CARTO', maxZoom: 20 }).addTo(mapInstance);

        icons = {
            'user': L.divIcon({ html: `<i data-lucide="map-pin" class="text-blue-400 w-10 h-10 drop-shadow-lg"></i>`, className: '', iconSize: [40, 40], iconAnchor: [20, 40] }),
            'scout-camp': L.divIcon({ html: `<div class="bg-yellow-600 p-1 rounded-full border-2 border-white"><i data-lucide="tent" class="w-5 h-5 text-white"></i></div>`, className: '', iconSize: [32, 32] }),
            'watchtower': L.divIcon({ html: `<div class="bg-gray-600 p-1 rounded-full border-2 border-white"><i data-lucide="binoculars" class="w-5 h-5 text-white"></i></div>`, className: '', iconSize: [32, 32] }),
            'foresters-lodge': L.divIcon({ html: `<div class="bg-green-700 p-1 rounded-full border-2 border-white"><i data-lucide="home" class="w-5 h-5 text-white"></i></div>`, className: '', iconSize: [32, 32] }),
            'beach': L.divIcon({ html: `<div class="bg-blue-500 p-1 rounded-full border-2 border-white"><i data-lucide="umbrella" class="w-5 h-5 text-white"></i></div>`, className: '', iconSize: [32, 32] }),
            'park': L.divIcon({ html: `<div class="bg-emerald-600 p-1 rounded-full border-2 border-white"><i data-lucide="sprout" class="w-5 h-5 text-white"></i></div>`, className: '', iconSize: [32, 32] }),
            'user-point': L.divIcon({ html: `<div class="bg-purple-600 p-1 rounded-full border-2 border-white"><i data-lucide="map-pin" class="w-5 h-5 text-white"></i></div>`, className: '', iconSize: [32, 32] }),
            'reported-threat': L.divIcon({ html: `<div class="bg-red-600 p-1 rounded-full border-2 border-white animate-pulse"><i data-lucide="siren" class="w-5 h-5 text-white"></i></div>`, className: '', iconSize: [32, 32] }),
        };
        
        const userMarker = L.marker(userCoords, { icon: icons.user }).addTo(mapInstance);
        userMarker.bindPopup(() => createPopupContent({name: 'Twoja lokalizacja', type: 'user-location'}, {lat: userCoords[0], lng: userCoords[1]}), { className: 'custom-popup' });
        
        objectMarkers = []; // Wyczyść tablicę przed ponownym wypełnieniem
        mapObjects.forEach((obj, index) => {
            const marker = L.marker(obj.coords, { 
                icon: icons[obj.type],
                draggable: false, // Domyślnie przeciąganie wyłączone
                objectIndex: index // Przechowaj oryginalny indeks obiektu
            }).addTo(mapInstance);

            marker.bindPopup(() => createPopupContent(mapObjects[marker.options.objectIndex], marker.getLatLng()), { className: 'custom-popup' });
            
            marker.on('dragend', function(event) {
                const marker = event.target;
                const position = marker.getLatLng();
                const objectIndex = marker.options.objectIndex;

                mapObjects[objectIndex].coords = [position.lat, position.lng];
                
                marker.unbindPopup();
                marker.bindPopup(() => createPopupContent(mapObjects[objectIndex], position), { className: 'custom-popup' });
                
                showMapNotification(`Zaktualizowano pozycję dla: ${mapObjects[objectIndex].name}`, 2500);
            });

            objectMarkers.push(marker);
        });
        
        mapInstance.on('click', (e) => {
            if (isAddMode) {
                addUserPoint(e.latlng);
            } else {
                if (genericPopup) mapInstance.removeLayer(genericPopup);
                genericPopup = L.popup({ className: 'custom-popup' })
                    .setLatLng(e.latlng)
                    .setContent(createPopupContent({ name: 'Analiza Punktu', type: 'generic' }, e.latlng))
                    .openOn(mapInstance);
            }
        });

        mapInstance.on('popupopen', (e) => {
            const popupContent = e.popup.getElement().querySelector('.leaflet-popup-content');
            if (!popupContent) return;
            lucide.createIcons({
                nodes: popupContent.querySelectorAll('[data-lucide]')
            });
             const deleteBtn = popupContent.querySelector('.delete-marker-btn');
            if (deleteBtn) {
                deleteBtn.addEventListener('click', () => {
                    const markerId = deleteBtn.dataset.markerId;
                    if (userMarkers[markerId]) {
                        mapInstance.removeLayer(userMarkers[markerId]);
                        delete userMarkers[markerId];
                    }
                });
            }
        });

        function addUserPoint(latlng) {
            const marker = L.marker(latlng, { icon: icons['user-point'] }).addTo(mapInstance);
            const markerId = L.Util.stamp(marker);
            userMarkers[markerId] = marker;

            const pointData = {
                name: 'Mój Punkt',
                type: 'user-point',
                markerId: markerId
            };

            marker.bindPopup(() => createPopupContent(pointData, latlng), { className: 'custom-popup' }).openPopup();
            toggleAddMode();
        }
        
        function toggleAddMode() {
            isAddMode = !isAddMode;
            if (isAddMode && isModerateMode) toggleModerateMode(); // Wyłącz tryb moderacji
            selectors.addPointBtn.classList.toggle('bg-red-600', isAddMode);
            selectors.addPointBtn.classList.toggle('hover:bg-red-700', isAddMode);
            selectors.addPointBtn.classList.toggle('bg-green-600', !isAddMode);
            selectors.addPointBtn.classList.toggle('hover:bg-green-700', !isAddMode);
            selectors.mapContainer.classList.toggle('add-mode', isAddMode);
            const icon = selectors.addPointBtn.querySelector('i');
            icon.setAttribute('data-lucide', isAddMode ? 'x' : 'plus');
            lucide.createIcons();
            showMapNotification(isAddMode ? 'Tryb dodawania: Kliknij na mapę, aby dodać punkt.' : 'Zakończono tryb dodawania.', 2500);
        }
        
        selectors.addPointBtn.addEventListener('click', toggleAddMode);
    }

    function updateUI() {
        if (!appState.weatherData) return;
        appState.isDataReady = true;

        selectors.locationDisplay.textContent = appState.weatherData.stationName;
        const lastUpdate = appState.weatherData.timestamp.toLocaleTimeString('pl-PL', { hour: '2-digit', minute: '2-digit' });
        selectors.mapUpdateTime.textContent = lastUpdate;
        selectors.aiUpdateTime.textContent = lastUpdate;

        // Enhanced Fire Notification
        const fireLevel = appState.threats.fire;
        const fireNotif = selectors.notificationFireContainer;
        let fireMessage, fireDetails, fireColor, fireIconColor;

        if (fireLevel < 1) {
            fireMessage = "Zagrożenie pożarowe: Niskie";
            fireDetails = "Wilgotność ściółki w normie. Małe ryzyko.";
            fireColor = 'border-green-500/50';
            fireIconColor = 'text-green-400';
        } else if (fireLevel < 2) {
            fireMessage = "Zagrożenie pożarowe: Umiarkowane";
            fireDetails = "Niska wilgotność ściółki. Zachowaj ostrożność.";
            fireColor = 'border-yellow-500/50';
            fireIconColor = 'text-yellow-400';
        } else if (fireLevel < 3) {
            fireMessage = "Zagrożenie pożarowe: Wysokie";
            fireDetails = "Bardzo sucha ściółka i wiatr. Możliwy zakaz wstępu do lasu.";
            fireColor = 'border-orange-500/50';
            fireIconColor = 'text-orange-400';
        } else {
            fireMessage = "Zagrożenie pożarowe: Ekstremalne";
            fireDetails = "Bezwzględny zakaz używania ognia. Natychmiastowe ryzyko.";
            fireColor = 'border-red-500/50';
            fireIconColor = 'text-red-400';
        }
        
        fireNotif.className = `bg-gray-800/70 p-4 rounded-xl border-l-4 flex items-start space-x-4 shadow-md ${fireColor}`;
        fireNotif.querySelector('i').className = `w-6 h-6 mt-1 ${fireIconColor}`;
        selectors.notificationFire.textContent = fireMessage;
        selectors.notificationFireDetails.textContent = fireDetails;


        const windLevel = appState.threats.wind;
        const windSpeedKmh = Math.round(appState.weatherData.windSpeed * 3.6);
        selectors.notificationWindContainer.className = `bg-gray-800/70 p-4 rounded-xl border-l-4 flex items-center space-x-4 shadow-md ${windLevel > 0 ? 'border-blue-400/50' : 'border-gray-500/50'}`;
        selectors.notificationWindContainer.querySelector('i').className = `w-5 h-5 ${windLevel > 0 ? 'text-blue-300' : 'text-gray-400'}`;
        selectors.notificationWind.textContent = `Wiatr: ${windSpeedKmh} km/h. ${windLevel > 0 ? 'Możliwe silne porywy.' : 'Słaby wiatr.'}`;
        
        renderLists();
        lucide.createIcons();
    }
    
    function renderLists() {
        const renderItems = (container, items, template) => {
            container.innerHTML = items.length ? items.map(template).join('') : `<p class="text-gray-400 text-center mt-10">Brak aktywnych alertów i predykcji.</p>`;
        };

        renderItems(selectors.aiPredictionsContainer, appState.predictions, pred => `
            <div class="bg-gray-800/70 p-5 rounded-xl border border-gray-700/50 shadow-lg cursor-pointer hover:bg-gray-700/70 transition-colors" data-id="${pred.id}" data-type="prediction">
                <div class="flex justify-between items-start gap-4">
                    <div class="flex-1 min-w-0"><h3 class="text-lg font-semibold text-gray-100">${pred.title}</h3></div>
                    <span class="flex-shrink-0 font-bold text-sm px-2 py-1 rounded-md bg-gray-900 ${pred.riskColor}">${pred.riskLevel}</span>
                </div>
                <p class="mt-2 text-gray-300 text-sm">${pred.content(appState.weatherData)}</p>
            </div>`);

        renderItems(selectors.alertsContainer, appState.alerts, alert => `
            <div class="flex items-start space-x-4 bg-gray-800/70 p-4 rounded-xl border-l-4 border-gray-600 cursor-pointer hover:bg-gray-700/70 transition-colors" data-id="${alert.id}" data-type="alert">
                <div class="flex-shrink-0 w-10 h-10 bg-gray-700 rounded-full flex items-center justify-center">
                    <i data-lucide="${alert.icon}" class="w-5 h-5 ${alert.color}"></i>
                </div>
                <div class="flex-1 min-w-0">
                    <h4 class="font-bold text-gray-100">${alert.title}</h4>
                    <p class="text-sm text-gray-400 mt-1 truncate">${alert.details(appState.weatherData)}</p>
                </div>
            </div>`);
        
        lucide.createIcons();
    }

    function showModal(content) {
        selectors.modalContent.innerHTML = content;
        selectors.modal.classList.remove('hidden');
        selectors.modal.classList.add('flex');
        lucide.createIcons();
    }
    
    function showMapNotification(message, duration = 3000) {
        const el = selectors.mapNotification;
        el.textContent = message;
        el.classList.remove('hidden');
        el.style.opacity = 1;

        if (duration > 0) {
            setTimeout(() => {
                el.style.opacity = 0;
                setTimeout(() => el.classList.add('hidden'), 300);
            }, duration);
        }
    }

    function hideMapNotification() {
        const el = selectors.mapNotification;
        el.style.opacity = 0;
        setTimeout(() => el.classList.add('hidden'), 300);
    }

    // === FUNKCJE ZGŁASZANIA I MODERACJI ===
    function initiateThreatReport() {
        if (!mapInstance) return;

        mapInstance.flyTo(userCoords, 14);

        const latlng = L.latLng(userCoords);
        const threatIcon = icons['reported-threat'];
        const marker = L.marker(latlng, { icon: threatIcon }).addTo(mapInstance);

        const createSavedPopup = (description) => `
            <div class="text-white w-[240px]">
                <h3 class="font-bold text-base mb-1 text-red-400">Zgłoszone Zagrożenie</h3>
                <p class="text-sm text-gray-300 mb-2">${description}</p>
                <p class="text-xs text-gray-500">Zgłoszono: ${new Date().toLocaleString('pl-PL')}</p>
            </div>
        `;

        const popupContainer = document.createElement('div');
        popupContainer.className = 'text-white w-[240px]';
        popupContainer.innerHTML = `
            <h3 class="text-xl font-bold mb-2">Opisz zagrożenie</h3>
            <p class="text-sm text-gray-400 mb-4">Zdarzenie zostanie zgłoszone w Twojej lokalizacji.</p>
            <textarea class="w-full bg-gray-700 border border-gray-600 rounded-lg p-2 h-20 text-white focus:outline-none focus:ring-2 focus:ring-green-500" placeholder="np. Dym, powalone drzewo..."></textarea>
            <div class="flex justify-end space-x-2 mt-3">
                <button class="cancel-btn bg-gray-600 hover:bg-gray-500 text-white font-bold py-1 px-3 rounded-lg text-sm">Anuluj</button>
                <button class="save-btn bg-green-600 hover:bg-green-700 text-white font-bold py-1 px-3 rounded-lg text-sm">Zapisz</button>
            </div>
        `;
        
        const textarea = popupContainer.querySelector('textarea');
        const saveBtn = popupContainer.querySelector('.save-btn');
        const cancelBtn = popupContainer.querySelector('.cancel-btn');

        saveBtn.addEventListener('click', () => {
            const description = textarea.value;
            if (description.trim()) {
                marker.unbindPopup();
                marker.bindPopup(createSavedPopup(description), { className: 'custom-popup' }).openPopup();
            } else {
                textarea.classList.add('border-red-500');
                textarea.focus();
            }
        });

        cancelBtn.addEventListener('click', () => mapInstance.removeLayer(marker));
        marker.on('popupclose', () => {
            if (!marker.getPopup().getContent().includes('Zgłoszone Zagrożenie')) {
                 mapInstance.removeLayer(marker);
            }
        });

        marker.bindPopup(popupContainer, { className: 'custom-popup', closeButton: false, minWidth: 260 }).openPopup();
    }
    
    function toggleModerateMode() {
        isModerateMode = !isModerateMode;
        if (isModerateMode && isAddMode) toggleAddMode(); // Wyłącz tryb dodawania

        objectMarkers.forEach(marker => {
            if (isModerateMode) {
                marker.dragging.enable();
            } else {
                marker.dragging.disable();
            }
        });

        const btn = selectors.moderatePointsBtn;
        btn.classList.toggle('bg-red-600', isModerateMode);
        btn.classList.toggle('hover:bg-red-700', isModerateMode);
        btn.classList.toggle('bg-blue-600', !isModerateMode);
        btn.classList.toggle('hover:bg-blue-700', !isModerateMode);

        const icon = btn.querySelector('i');
        icon.setAttribute('data-lucide', isModerateMode ? 'x-circle' : 'shield');
        lucide.createIcons({nodes: [icon]});
        
        if (isModerateMode) {
            showMapNotification('Tryb moderacji: Przesuń punkty, aby zaktualizować ich pozycję.', 0);
        } else {
            hideMapNotification();
        }
    }

    // === GŁÓWNA FUNKCJA URUCHOMIENIOWA ===
    async function updateAllData() {
        await fetchVerifiedWeatherData();
        generateDynamicContent();
        updateUI();
    }

    async function main() {
        setupEventListeners();
        selectors.loadingStatus.textContent = "Inicjalizacja interfejsu...";
        await new Promise(resolve => setTimeout(resolve, 100));
        
        await updateAllData();
        
        selectors.loadingStatus.textContent = "Renderowanie mapy...";
        initializeMap();
        
        selectors.appContent.classList.remove('hidden');
        selectors.navContainer.classList.remove('hidden');
        selectors.screenLoading.classList.remove('active');
        navigateTo('home');
        
        setInterval(updateAllData, 600000); // Aktualizacja co 10 minut
    }

    // === OBSŁUGA ZDARZEŃ ===
    function setupEventListeners() {
        selectors.navItems.forEach(item => item.addEventListener('click', () => navigateTo(item.dataset.nav)));
        selectors.clickableMapCard.addEventListener('click', () => navigateTo('map'));
        selectors.reportThreatBtn.addEventListener('click', () => {
            navigateTo('map');
            setTimeout(initiateThreatReport, 200);
        });
        selectors.moderatePointsBtn.addEventListener('click', toggleModerateMode);

        const handleListClick = (e, type, collection) => {
            const el = e.target.closest(`[data-type="${type}"]`);
            if (!el) return;
            const item = collection.find(i => i.id == el.dataset.id);
            if (item) {
                const details = typeof item.details === 'function' ? item.details(appState.weatherData) : item.details;
                const contentText = typeof item.content === 'function' ? item.content(appState.weatherData) : item.content;
                const recommendationText = typeof item.recommendation === 'function' ? item.recommendation(appState.weatherData) : item.recommendation;

                const modalHTML = type === 'prediction' ? `
                    <h3 class="text-xl font-bold mb-2">${item.title}</h3>
                    <p class="text-gray-300 mb-2">${contentText}</p>
                    <p class="text-green-400 font-semibold border-t border-gray-600 pt-2 mt-2">${recommendationText}</p>` : `
                    <h3 class="text-xl font-bold mb-2">${item.title}</h3>
                    <p class="text-gray-300 mb-4">${details}</p>
                    <p class="text-xs font-semibold text-gray-500">Źródło: ${item.source}</p>`;
                showModal(`${modalHTML}<button id="modal-close-btn" class="mt-6 w-full bg-green-600 text-white font-bold py-2 rounded-lg">Zamknij</button>`);
            }
        };

        selectors.aiPredictionsContainer.addEventListener('click', (e) => handleListClick(e, 'prediction', appState.predictions));
        selectors.alertsContainer.addEventListener('click', (e) => handleListClick(e, 'alert', appState.alerts));

        selectors.modal.addEventListener('click', (e) => {
            if (e.target.id === 'modal' || e.target.closest('#modal-close-btn')) {
                selectors.modal.classList.add('hidden');
                selectors.modal.classList.remove('flex');
            }
        });
    }

    // --- Rejestracja Service Workera dla PWA ---
    if ('serviceWorker' in navigator) {
        const swContent = `
            const CACHE_NAME = 'forestguard-ai-cache-v1';
            const urlsToCache = [
                '/',
                'https://cdn.tailwindcss.com',
                'https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap',
                'https://unpkg.com/lucide@latest',
                'https://unpkg.com/leaflet@1.9.4/dist/leaflet.css',
                'https://unpkg.com/leaflet@1.9.4/dist/leaflet.js',
                'https://images.pexels.com/photos/167699/pexels-photo-167699.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=2'
            ];

            self.addEventListener('install', event => {
                event.waitUntil(
                    caches.open(CACHE_NAME)
                        .then(cache => {
                            console.log('Opened cache');
                            return cache.addAll(urlsToCache);
                        })
                );
            });

            self.addEventListener('fetch', event => {
                event.respondWith(
                    caches.match(event.request)
                        .then(response => {
                            if (response) {
                                return response;
                            }
                            return fetch(event.request);
                        }
                    )
                );
            });
        `;
        const swBlob = new Blob([swContent], {type: 'application/javascript'});
        const swUrl = URL.createObjectURL(swBlob);

        window.addEventListener('load', () => {
            navigator.serviceWorker.register(swUrl)
                .then(registration => {
                    console.log('ServiceWorker registration successful with scope: ', registration.scope);
                })
                .catch(error => {
                    console.log('ServiceWorker registration failed: ', error);
                });
        });
    }


    // Uruchomienie aplikacji
    main().catch(error => {
        console.error("Błąd krytyczny podczas inicjalizacji aplikacji:", error);
        selectors.loadingStatus.innerHTML = '<p class="text-red-500">Wystąpił błąd krytyczny. Odśwież stronę.</p>';
    });
});
</script>
</body>
</html>
