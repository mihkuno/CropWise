<script>
    import { MapPin, BarChart3, Users, Sprout, Settings, RefreshCw, Download, LogOut, TrendingUp, Thermometer, Droplet, Wind, Eye, Edit, Plus, Activity, Database, AlertCircle } from '@lucide/svelte';
    import { onMount } from 'svelte';
    import { goto } from "$app/navigation";

    let activeTab = 'overview';
    let map;
    let soilHealthData = [
        { name: 'Kalinawan Farm', nitrogen: 68, phosphorus: 61, potassium: 34, ph: 6.7 },
        { name: 'Malasag Organic Farm', nitrogen: 78, phosphorus: 36, potassium: 39, ph: 6.1 },
        { name: 'Golden Harvest Farm', nitrogen: 95, phosphorus: 33, potassium: 48, ph: 4.7 }
    ];

    let weatherData = [
        { day: 'Today', icon: '☀️', condition: 'Sunny', temp: '26°C', humidity: '60%' },
        { day: 'Tomorrow', icon: '🌧️', condition: 'Rainy', temp: '31°C', humidity: '52%' },
        { day: 'Wednesday', icon: '⛅', condition: 'Partly Cloudy', temp: '31°C', humidity: '45%' },
        { day: 'Thursday', icon: '⛅', condition: 'Partly Cloudy', temp: '27°C', humidity: '78%' },
        { day: 'Friday', icon: '☀️', condition: 'Sunny', temp: '34°C', humidity: '46%' }
    ];

    let farmers = [
        { 
            name: 'Juan Carlos Santos', 
            phone: '+63 917 123 4567', 
            farms: 2, 
            language: 'EN',
            farmDetails: ['Kalinawan Farm (2.5 hectares)', 'Malasag Organic Farm (1.8 hectares)']
        },
        { 
            name: 'Maria Luz Villanueva', 
            phone: '+63 928 765 4321', 
            farms: 1, 
            language: 'EN',
            farmDetails: ['Golden Harvest Farm (4.2 hectares)']
        }
    ];

    let cropRules = [
        { current: 'Rice', recommended: 'Beans', reason: 'Nitrogen fixation', confidence: 73, status: 'active' },
        { current: 'Tomatoes', recommended: 'Spinach', reason: 'Pest cycle break', confidence: 86, status: 'active' },
        { current: 'Corn', recommended: 'Beans', reason: 'Companion planting', confidence: 87, status: 'active' },
        { current: 'Cotton', recommended: 'Mustard', reason: 'Soil health improvement', confidence: 70, status: 'active' }
    ];

    let farmLocations = [
        { name: 'Green Valley Farm', lat: 8.4542, lng: 124.6319, color: '#84c059' },
        { name: 'Sunrise Agriculture', lat: 8.4712, lng: 124.6542, color: '#f59e0b' },
        { name: 'Golden Harvest', lat: 8.4328, lng: 124.6124, color: '#eab308' },
        { name: 'Peaceful Acres', lat: 8.4789, lng: 124.6701, color: '#06b6d4' },
        { name: 'Organic Oasis', lat: 8.4401, lng: 124.5987, color: '#8b5cf6' }
    ];

    function handleLogout() {
        goto('/');
    }

    onMount(async () => {
        if (activeTab === 'gis' && typeof window !== 'undefined') {
            // Dynamically import Leaflet
            const L = await import('leaflet');
            
            // Initialize map centered on Cagayan de Oro City
            map = L.map('map').setView([8.4542, 124.6319], 12);

            // Add OpenStreetMap tiles
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                attribution: '© OpenStreetMap contributors'
            }).addTo(map);

            // Add zone polygons for different barangays in Cagayan de Oro
            const zones = [
                {
                    name: 'Carmen',
                    coords: [[8.4700, 124.6400], [8.4750, 124.6500], [8.4680, 124.6520], [8.4650, 124.6420]],
                    color: '#84c059'
                },
                {
                    name: 'Lapasan',
                    coords: [[8.4500, 124.6200], [8.4600, 124.6300], [8.4550, 124.6350], [8.4450, 124.6250]],
                    color: '#f59e0b'
                },
                {
                    name: 'Nazareth',
                    coords: [[8.4350, 124.6100], [8.4400, 124.6180], [8.4320, 124.6200], [8.4280, 124.6120]],
                    color: '#eab308'
                },
                {
                    name: 'Macasandig',
                    coords: [[8.4800, 124.6600], [8.4850, 124.6720], [8.4780, 124.6750], [8.4730, 124.6650]],
                    color: '#06b6d4'
                },
                {
                    name: 'Balubal',
                    coords: [[8.4380, 124.5950], [8.4420, 124.6020], [8.4360, 124.6050], [8.4320, 124.5980]],
                    color: '#8b5cf6'
                }
            ];

            // Add zone polygons to map
            zones.forEach(zone => {
                L.polygon(zone.coords, {
                    color: zone.color,
                    fillColor: zone.color,
                    fillOpacity: 0.3,
                    weight: 2
                }).addTo(map).bindPopup(`<strong>${zone.name}</strong><br>Agricultural Zone`);
            });

            // Add farm location markers
            farmLocations.forEach(farm => {
                L.circleMarker([farm.lat, farm.lng], {
                    radius: 8,
                    fillColor: farm.color,
                    color: '#fff',
                    weight: 2,
                    opacity: 1,
                    fillOpacity: 0.8
                }).addTo(map).bindPopup(`<strong>${farm.name}</strong><br>Lat: ${farm.lat}<br>Lng: ${farm.lng}`);
            });
        }
    });

    function setActiveTab(tab) {
        activeTab = tab;
        if (tab === 'gis') {
            setTimeout(async () => {
                if (typeof window !== 'undefined') {
                    const L = await import('leaflet');
                    
                    map = L.map('map').setView([8.4542, 124.6319], 12);

                    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                        attribution: '© OpenStreetMap contributors'
                    }).addTo(map);

                    const zones = [
                        {
                            name: 'Carmen',
                            coords: [[8.4700, 124.6400], [8.4750, 124.6500], [8.4680, 124.6520], [8.4650, 124.6420]],
                            color: '#84c059'
                        },
                        {
                            name: 'Lapasan',
                            coords: [[8.4500, 124.6200], [8.4600, 124.6300], [8.4550, 124.6350], [8.4450, 124.6250]],
                            color: '#f59e0b'
                        },
                        {
                            name: 'Nazareth',
                            coords: [[8.4350, 124.6100], [8.4400, 124.6180], [8.4320, 124.6200], [8.4280, 124.6120]],
                            color: '#eab308'
                        },
                        {
                            name: 'Macasandig',
                            coords: [[8.4800, 124.6600], [8.4850, 124.6720], [8.4780, 124.6750], [8.4730, 124.6650]],
                            color: '#06b6d4'
                        },
                        {
                            name: 'Balubal',
                            coords: [[8.4380, 124.5950], [8.4420, 124.6020], [8.4360, 124.6050], [8.4320, 124.5980]],
                            color: '#8b5cf6'
                        }
                    ];

                    zones.forEach(zone => {
                        L.polygon(zone.coords, {
                            color: zone.color,
                            fillColor: zone.color,
                            fillOpacity: 0.3,
                            weight: 2
                        }).addTo(map).bindPopup(`<strong>${zone.name}</strong><br>Agricultural Zone`);
                    });

                    farmLocations.forEach(farm => {
                        L.circleMarker([farm.lat, farm.lng], {
                            radius: 8,
                            fillColor: farm.color,
                            color: '#fff',
                            weight: 2,
                            opacity: 1,
                            fillOpacity: 0.8
                        }).addTo(map).bindPopup(`<strong>${farm.name}</strong><br>Lat: ${farm.lat}<br>Lng: ${farm.lng}`);
                    });
                }
            }, 100);
        }
    }
</script>

<svelte:head>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.7.1/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.7.1/dist/leaflet.js"></script>
</svelte:head>

<div class="max-w-6xl mx-auto min-h-screen p-6">
    
    <!-- Header -->
    <div class="flex items-center bg-[#84c059] shadow-2xl justify-between py-4 px-6 rounded-2xl shadow-2xl">
        <div class="flex items-center gap-4">
            <div class="bg-white/10 rounded-full p-2">
                <Sprout size="40" color="#ffffff" />
            </div>
            <div>
                <p class="text-2xl font-bold text-white">CropWise Admin</p>
                <p class="text-sm text-white/90">Welcome back, Dr. Ricardo Aguilar</p>
            </div>
        </div>
        <div class="flex items-center gap-2">
            <button class="bg-white/20 text-white font-medium px-3 py-2 rounded-lg hover:bg-white/30 flex items-center gap-2">
                <RefreshCw size="16" />
                Refresh Data
            </button>
            <button class="bg-white/20 text-white font-medium px-3 py-2 rounded-lg hover:bg-white/30 flex items-center gap-2">
                <Download size="16" />
                Export Data
            </button>
            <button 
                on:click={handleLogout}
                class="bg-red-500/20 text-white font-medium px-3 py-2 rounded-lg hover:bg-red-500/30 flex items-center gap-2">
                <LogOut size="16" />
                Logout
            </button>
        </div>
    </div>

    <!-- Stats Cards -->
    <div class="grid grid-cols-4 gap-4 p-6">
        <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-4">
            <div class="flex items-center justify-between">
                <div>
                    <p class="text-2xl font-bold text-[#84c059]">5</p>
                    <p class="text-sm font-medium text-gray-600">Total Farms</p>
                </div>
                <MapPin size="24" color="#84c059" />
            </div>
        </div>
        <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-4">
            <div class="flex items-center justify-between">
                <div>
                    <p class="text-2xl font-bold text-[#84c059]">2</p>
                    <p class="text-sm font-medium text-gray-600">Active Farmers</p>
                </div>
                <Users size="24" color="#84c059" />
            </div>
        </div>
        <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-4">
            <div class="flex items-center justify-between">
                <div>
                    <p class="text-2xl font-bold text-[#84c059]">4</p>
                    <p class="text-sm font-medium text-gray-600">Crop Rules</p>
                </div>
                <Settings size="24" color="#84c059" />
            </div>
        </div>
        <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-4">
            <div class="flex items-center justify-between">
                <div>
                    <p class="text-2xl font-bold text-[#84c059]">80%</p>
                    <p class="text-sm font-medium text-gray-600">Avg Soil Health</p>
                </div>
                <Activity size="24" color="#84c059" />
            </div>
        </div>
    </div>

    <!-- Navigation Tabs -->
    <div class="px-6">
        <div class="flex gap-2 bg-white/50 backdrop-blur-md rounded-xl p-2 shadow-lg">
            <button 
                class="px-4 py-2 rounded-lg font-medium transition-all {activeTab === 'overview' ? 'bg-[#84c059] text-white' : 'text-gray-600 hover:bg-white/50'}"
                on:click={() => setActiveTab('overview')}>
                Overview
            </button>
            <button 
                class="px-4 py-2 rounded-lg font-medium transition-all {activeTab === 'farms' ? 'bg-[#84c059] text-white' : 'text-gray-600 hover:bg-white/50'}"
                on:click={() => setActiveTab('farms')}>
                Farms
            </button>
            <button 
                class="px-4 py-2 rounded-lg font-medium transition-all {activeTab === 'gis' ? 'bg-[#84c059] text-white' : 'text-gray-600 hover:bg-white/50'}"
                on:click={() => setActiveTab('gis')}>
                GIS
            </button>
            <button 
                class="px-4 py-2 rounded-lg font-medium transition-all {activeTab === 'farmers' ? 'bg-[#84c059] text-white' : 'text-gray-600 hover:bg-white/50'}"
                on:click={() => setActiveTab('farmers')}>
                Farmers
            </button>
            <button 
                class="px-4 py-2 rounded-lg font-medium transition-all {activeTab === 'rules' ? 'bg-[#84c059] text-white' : 'text-gray-600 hover:bg-white/50'}"
                on:click={() => setActiveTab('rules')}>
                Crop Rules
            </button>
            <button 
                class="px-4 py-2 rounded-lg font-medium transition-all {activeTab === 'data' ? 'bg-[#84c059] text-white' : 'text-gray-600 hover:bg-white/50'}"
                on:click={() => setActiveTab('data')}>
                Data Management
            </button>
        </div>
    </div>

    <!-- Tab Content -->
    <div class="p-6">
        {#if activeTab === 'overview'}
            <div class="grid grid-cols-2 gap-6">
                <!-- Soil Health Chart -->
                <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-6">
                    <div class="flex items-center gap-2 mb-4">
                        <TrendingUp size="24" color="#84c059" />
                        <h3 class="font-bold text-lg">Soil Health Trends</h3>
                    </div>
                    <p class="text-sm text-gray-600 mb-4">Historical nutrient levels across all farms</p>
                    <div class="space-y-4">
                        {#each soilHealthData as farm}
                            <div class="space-y-2">
                                <p class="font-medium text-sm">{farm.name}</p>
                                <div class="grid grid-cols-4 gap-2 text-xs">
                                    <div>
                                        <div class="flex justify-between mb-1">
                                            <span>N</span>
                                            <span>{farm.nitrogen}%</span>
                                        </div>
                                        <div class="w-full bg-gray-200 rounded-full h-2">
                                            <div class="bg-blue-500 h-2 rounded-full" style="width: {farm.nitrogen}%"></div>
                                        </div>
                                    </div>
                                    <div>
                                        <div class="flex justify-between mb-1">
                                            <span>P</span>
                                            <span>{farm.phosphorus}%</span>
                                        </div>
                                        <div class="w-full bg-gray-200 rounded-full h-2">
                                            <div class="bg-orange-500 h-2 rounded-full" style="width: {farm.phosphorus}%"></div>
                                        </div>
                                    </div>
                                    <div>
                                        <div class="flex justify-between mb-1">
                                            <span>K</span>
                                            <span>{farm.potassium}%</span>
                                        </div>
                                        <div class="w-full bg-gray-200 rounded-full h-2">
                                            <div class="bg-green-500 h-2 rounded-full" style="width: {farm.potassium}%"></div>
                                        </div>
                                    </div>
                                    <div>
                                        <div class="flex justify-between mb-1">
                                            <span>pH</span>
                                            <span>{farm.ph}</span>
                                        </div>
                                        <div class="w-full bg-gray-200 rounded-full h-2">
                                            <div class="bg-purple-500 h-2 rounded-full" style="width: {(farm.ph / 14) * 100}%"></div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        {/each}
                    </div>
                </div>

                <!-- Soil Distribution -->
                <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-6">
                    <div class="flex items-center gap-2 mb-4">
                        <BarChart3 size="24" color="#84c059" />
                        <h3 class="font-bold text-lg">Farm Soil Distribution</h3>
                    </div>
                    <p class="text-sm text-gray-600 mb-4">Distribution of farms by soil type</p>
                    <div class="space-y-3">
                        <div class="flex items-center justify-between p-3 bg-[#f3eee6] rounded-lg">
                            <div class="flex items-center gap-2">
                                <div class="w-4 h-4 bg-[#84c059] rounded"></div>
                                <span class="text-sm font-medium">Clay-loam</span>
                            </div>
                            <span class="text-sm font-bold">60% (3 farms)</span>
                        </div>
                        <div class="flex items-center justify-between p-3 bg-[#f3eee6] rounded-lg">
                            <div class="flex items-center gap-2">
                                <div class="w-4 h-4 bg-[#f59e0b] rounded"></div>
                                <span class="text-sm font-medium">Sandy-loam</span>
                            </div>
                            <span class="text-sm font-bold">40% (2 farms)</span>
                        </div>
                    </div>
                </div>

                <!-- Weather Overview -->
                <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-6 col-span-2">
                    <div class="flex items-center gap-2 mb-4">
                        <Thermometer size="24" color="#84c059" />
                        <h3 class="font-bold text-lg">Weather Conditions</h3>
                    </div>
                    <div class="grid grid-cols-5 gap-4">
                        {#each weatherData as weather}
                            <div class="text-center p-3 bg-[#f3eee6] rounded-lg">
                                <div class="text-2xl mb-1">{weather.icon}</div>
                                <p class="font-medium text-sm">{weather.day}</p>
                                <p class="text-xs text-gray-600">{weather.condition}</p>
                                <p class="text-sm font-bold">{weather.temp}</p>
                                <p class="text-xs text-gray-600">{weather.humidity} humidity</p>
                            </div>
                        {/each}
                    </div>
                </div>
            </div>

        {:else if activeTab === 'gis'}
            <div class="grid grid-cols-3 gap-6">
                <!-- Map -->
                <div class="col-span-2 bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-6">
                    <div class="flex items-center gap-2 mb-4">
                        <MapPin size="24" color="#84c059" />
                        <h3 class="font-bold text-lg">Farm GIS Map</h3>
                    </div>
                    <div id="map" class="w-full h-96 rounded-lg border border-gray-200"></div>
                    
                    <!-- Legend -->
                    <div class="mt-4 p-4 bg-[#f3eee6] rounded-lg">
                        <p class="font-medium text-sm mb-2">Legend</p>
                        <div class="grid grid-cols-5 gap-2 text-xs">
                            {#each farmLocations as farm}
                                <div class="flex items-center gap-1">
                                    <div class="w-3 h-3 rounded-full" style="background-color: {farm.color}"></div>
                                    <span>{farm.name}</span>
                                </div>
                            {/each}
                        </div>
                    </div>
                </div>

                <!-- Farm Locations List -->
                <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-6">
                    <h3 class="font-bold text-lg mb-4">Farm Locations (List View)</h3>
                    <div class="space-y-3">
                        {#each farmLocations as farm}
                            <div class="p-3 bg-[#f3eee6] rounded-lg">
                                <div class="flex items-center gap-2 mb-1">
                                    <div class="w-3 h-3 rounded-full" style="background-color: {farm.color}"></div>
                                    <p class="font-medium text-sm">{farm.name}</p>
                                </div>
                                <p class="text-xs text-gray-600">{farm.lat}, {farm.lng}</p>
                            </div>
                        {/each}
                    </div>
                    
                    <div class="mt-6 p-4 bg-[#f3eee6] rounded-lg">
                        <h4 class="font-medium text-sm mb-2">Farm Overview</h4>
                        <div class="space-y-1 text-xs">
                            <p><span class="font-medium">Total Area:</span> 8.5 hectares</p>
                            <p><span class="font-medium">Active Farms:</span> 5</p>
                            <p><span class="font-medium">Avg Soil Health:</span> 80%</p>
                        </div>
                    </div>
                </div>
            </div>

        {:else if activeTab === 'farmers'}
            <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-6">
                <div class="flex items-center justify-between mb-6">
                    <div class="flex items-center gap-2">
                        <Users size="24" color="#84c059" />
                        <h3 class="font-bold text-lg">Farmer Management</h3>
                    </div>
                    <button class="bg-[#84c059] text-white px-4 py-2 rounded-lg font-medium hover:bg-[#6fa045] flex items-center gap-2">
                        <Plus size="16" />
                        Add Farmer
                    </button>
                </div>
                
                <div class="space-y-4">
                    {#each farmers as farmer}
                        <div class="p-4 bg-[#f3eee6] rounded-xl border border-[#e8e3d9]">
                            <div class="flex items-center justify-between mb-3">
                                <div>
                                    <p class="font-semibold text-lg">{farmer.name}</p>
                                    <p class="text-sm text-gray-600">{farmer.phone}</p>
                                </div>
                                <div class="flex items-center gap-4">
                                    <div class="text-center">
                                        <p class="text-xl font-bold text-[#84c059]">{farmer.farms}</p>
                                        <p class="text-xs text-gray-600">farms</p>
                                    </div>
                                    <div class="bg-gray-200 px-2 py-1 rounded text-xs font-medium">{farmer.language}</div>
                                    <div class="flex gap-2">
                                        <button class="p-2 bg-white rounded-lg hover:bg-gray-50">
                                            <Eye size="16" color="#84c059" />
                                        </button>
                                        <button class="p-2 bg-white rounded-lg hover:bg-gray-50">
                                            <Edit size="16" color="#84c059" />
                                        </button>
                                    </div>
                                </div>
                            </div>
                            <div>
                                <p class="text-sm font-medium mb-2">Assigned Farms:</p>
                                <div class="space-y-1">
                                    {#each farmer.farmDetails as farmDetail}
                                        <p class="text-sm text-gray-600">• {farmDetail}</p>
                                    {/each}
                                </div>
                            </div>
                        </div>
                    {/each}
                </div>
            </div>

        {:else if activeTab === 'rules'}
            <div class="space-y-6">
                <!-- AI Rules Engine -->
                <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-6">
                    <div class="flex items-center gap-2 mb-4">
                        <Settings size="24" color="#84c059" />
                        <h3 class="font-bold text-lg">AI Crop Rules Engine</h3>
                    </div>
                    <p class="text-sm text-gray-600 mb-6">Advanced AI-powered system for intelligent crop rotation and companion planting recommendations.</p>
                    
                    <div class="grid grid-cols-4 gap-4 mb-6">
                        <div class="p-4 bg-[#f3eee6] rounded-lg">
                            <p class="text-sm font-medium text-gray-600">AI Model</p>
                            <p class="text-lg font-bold">GPT-4 - High Accuracy</p>
                        </div>
                        <div class="p-4 bg-[#f3eee6] rounded-lg">
                            <p class="text-sm font-medium text-gray-600">Confidence Threshold</p>
                            <p class="text-lg font-bold">80%</p>
                        </div>
                        <div class="p-4 bg-[#f3eee6] rounded-lg">
                            <p class="text-sm font-medium text-gray-600">Auto-Update</p>
                            <p class="text-lg font-bold">Real-time</p>
                        </div>
                        <div class="p-4 bg-[#f3eee6] rounded-lg text-center">
                            <button class="bg-[#84c059] text-white px-4 py-2 rounded-lg font-medium hover:bg-[#6fa045] w-full">
                                Generate AI Rules
                            </button>
                        </div>
                    </div>

                    <div class="grid grid-cols-2 gap-6">
                        <div>
                            <h4 class="font-semibold mb-3">AI Capabilities</h4>
                            <ul class="space-y-2 text-sm">
                                <li class="flex items-center gap-2">
                                    <div class="w-2 h-2 bg-[#84c059] rounded-full"></div>
                                    Real-time soil analysis
                                </li>
                                <li class="flex items-center gap-2">
                                    <div class="w-2 h-2 bg-[#84c059] rounded-full"></div>
                                    Weather pattern integration
                                </li>
                                <li class="flex items-center gap-2">
                                    <div class="w-2 h-2 bg-[#84c059] rounded-full"></div>
                                    Advanced soil data processing
                                </li>
                                <li class="flex items-center gap-2">
                                    <div class="w-2 h-2 bg-[#84c059] rounded-full"></div>
                                    Predictive crop modeling
                                </li>
                                <li class="flex items-center gap-2">
                                    <div class="w-2 h-2 bg-[#84c059] rounded-full"></div>
                                    Multi-crop optimization
                                </li>
                            </ul>
                        </div>
                        <div>
                            <h4 class="font-semibold mb-3">AI-Generated Rules</h4>
                            <div class="bg-[#f3eee6] rounded-lg p-4">
                                <p class="text-xs text-gray-600 mb-2">Last Updated: 8/28/2025, 9:22:45 AM</p>
                                <div class="grid grid-cols-3 gap-4 mb-3">
                                    <div class="text-center">
                                        <p class="text-lg font-bold text-[#84c059]">4</p>
                                        <p class="text-xs text-gray-600">Active Rules</p>
                                    </div>
                                    <div class="text-center">
                                        <p class="text-lg font-bold text-[#84c059]">4</p>
                                        <p class="text-xs text-gray-600">Optimized Rules</p>
                                    </div>
                                    <div class="text-center">
                                        <p class="text-lg font-bold text-[#84c059]">94.2%</p>
                                        <p class="text-xs text-gray-600">Accuracy</p>
                                    </div>
                                </div>
                                <p class="text-xs text-gray-600">Processing Time: 1.2s</p>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Current Rules Table -->
                <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-6">
                    <div class="overflow-x-auto">
                        <table class="w-full">
                            <thead>
                                <tr class="border-b border-gray-200">
                                    <th class="text-left py-3 px-2 font-semibold">Current Crop</th>
                                    <th class="text-left py-3 px-2 font-semibold">Recommended Crop</th>
                                    <th class="text-left py-3 px-2 font-semibold">AI Reason</th>
                                    <th class="text-center py-3 px-2 font-semibold">Confidence</th>
                                    <th class="text-center py-3 px-2 font-semibold">Status</th>
                                    <th class="text-center py-3 px-2 font-semibold">Actions</th>
                                </tr>
                            </thead>
                            <tbody>
                                {#each cropRules as rule}
                                    <tr class="border-b border-gray-100">
                                        <td class="py-3 px-2 font-medium">{rule.current}</td>
                                        <td class="py-3 px-2">{rule.recommended}</td>
                                        <td class="py-3 px-2 text-sm text-gray-600">{rule.reason}</td>
                                        <td class="py-3 px-2 text-center">
                                            <div class="flex items-center justify-center gap-2">
                                                <div class="w-full bg-gray-200 rounded-full h-2 max-w-16">
                                                    <div class="bg-[#84c059] h-2 rounded-full" style="width: {rule.confidence}%"></div>
                                                </div>
                                                <span class="text-sm font-medium">{rule.confidence}%</span>
                                            </div>
                                        </td>
                                        <td class="py-3 px-2 text-center">
                                            <span class="px-2 py-1 bg-green-100 text-green-700 rounded-lg text-xs font-medium">
                                                {rule.status}
                                            </span>
                                        </td>
                                        <td class="py-3 px-2 text-center">
                                            <div class="flex items-center justify-center gap-1">
                                                <button class="p-1 bg-blue-100 text-blue-600 rounded hover:bg-blue-200">
                                                    <Eye size="14" />
                                                </button>
                                                <button class="p-1 bg-gray-100 text-gray-600 rounded hover:bg-gray-200">
                                                    <Edit size="14" />
                                                </button>
                                            </div>
                                        </td>
                                    </tr>
                                {/each}
                            </tbody>
                        </table>
                    </div>
                </div>

                <!-- Manual Rule Override -->
                <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-6">
                    <h3 class="font-bold text-lg mb-4">Manual Rule Override</h3>
                    <p class="text-sm text-gray-600 mb-4">Create custom rules to override AI recommendations when needed.</p>
                    
                    <div class="grid grid-cols-4 gap-4 mb-4">
                        <div>
                            <label class="block text-sm font-medium mb-1">Current Crop</label>
                            <input type="text" placeholder="e.g., Rice" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#84c059]">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Recommended Crop</label>
                            <input type="text" placeholder="e.g., Beans" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#84c059]">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Reason</label>
                            <input type="text" placeholder="e.g., Nitrogen fixation" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#84c059]">
                        </div>
                        <div>
                            <label class="block text-sm font-medium mb-1">Condition</label>
                            <input type="text" placeholder="e.g., nitrogen < 30" class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-[#84c059]">
                        </div>
                    </div>
                    <button class="bg-[#84c059] text-white px-6 py-2 rounded-lg font-medium hover:bg-[#6fa045] flex items-center gap-2">
                        <Plus size="16" />
                        Add Manual Rule
                    </button>
                </div>
            </div>

        {:else if activeTab === 'data'}
            <div class="space-y-6">
                <!-- Real-time Soil Data -->
                <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-6">
                    <div class="flex items-center gap-2 mb-4">
                        <Database size="24" color="#84c059" />
                        <h3 class="font-bold text-lg">Real-time Soil Data</h3>
                    </div>
                    
                    <div class="grid grid-cols-3 gap-4">
                        {#each soilHealthData as farm}
                            <div class="p-4 bg-[#f3eee6] rounded-xl border border-[#e8e3d9]">
                                <h4 class="font-semibold mb-2">{farm.name}</h4>
                                <div class="space-y-2 text-sm">
                                    <div class="flex justify-between">
                                        <span>N: {farm.nitrogen}%</span>
                                        <span>P: {farm.phosphorus}%</span>
                                    </div>
                                    <div class="flex justify-between">
                                        <span>K: {farm.potassium}%</span>
                                        <span>pH: {farm.ph}</span>
                                    </div>
                                    <div class="text-xs text-gray-600 pt-2">
                                        Updated: {new Date().toLocaleTimeString()}
                                    </div>
                                </div>
                            </div>
                        {/each}
                    </div>
                </div>

                <!-- System Information -->
                <div class="grid grid-cols-2 gap-6">
                    <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-6">
                        <div class="flex items-center gap-2 mb-4">
                            <Settings size="24" color="#84c059" />
                            <h3 class="font-bold text-lg">System Information</h3>
                        </div>
                        
                        <div class="space-y-4">
                            <div>
                                <h4 class="font-semibold mb-2">Data Sources</h4>
                                <ul class="space-y-1 text-sm text-gray-600">
                                    <li>• Soil Health: Simulated random values</li>
                                    <li>• Weather: Random forecast generator</li>
                                    <li>• Crop Pairs: Hardcoded knowledge base</li>
                                    <li>• Farm Boundaries: Static polygon data</li>
                                </ul>
                            </div>
                            
                            <div>
                                <h4 class="font-semibold mb-2">System Status</h4>
                                <div class="grid grid-cols-2 gap-2 text-sm">
                                    <div class="p-2 bg-[#f3eee6] rounded">
                                        <p class="font-medium">Data Refresh Rate</p>
                                        <p class="text-gray-600">Manual</p>
                                    </div>
                                    <div class="p-2 bg-[#f3eee6] rounded">
                                        <p class="font-medium">Storage</p>
                                        <p class="text-gray-600">In-Memory</p>
                                    </div>
                                    <div class="p-2 bg-[#f3eee6] rounded">
                                        <p class="font-medium">Last Export</p>
                                        <p class="text-gray-600">Never</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-6">
                        <div class="flex items-center gap-2 mb-4">
                            <AlertCircle size="24" color="#f59e0b" />
                            <h3 class="font-bold text-lg">Prototype Notice</h3>
                        </div>
                        
                        <div class="space-y-4">
                            <div class="p-4 bg-yellow-50 border border-yellow-200 rounded-lg">
                                <p class="text-sm text-yellow-800 font-medium mb-2">Development Environment</p>
                                <p class="text-sm text-yellow-700">
                                    This is a prototype using dummy data. All data resets on page refresh. 
                                    No real databases or APIs are connected.
                                </p>
                            </div>
                            
                            <div class="space-y-2">
                                <button class="w-full bg-[#84c059] text-white px-4 py-2 rounded-lg font-medium hover:bg-[#6fa045] flex items-center justify-center gap-2">
                                    <RefreshCw size="16" />
                                    Regenerate Sample Data
                                </button>
                                <button class="w-full bg-gray-500 text-white px-4 py-2 rounded-lg font-medium hover:bg-gray-600 flex items-center justify-center gap-2">
                                    <Download size="16" />
                                    Export System Logs
                                </button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

        {:else if activeTab === 'farms'}
            <div class="bg-white/70 backdrop-blur-md rounded-xl shadow-lg p-6">
                <div class="flex items-center justify-between mb-6">
                    <div class="flex items-center gap-2">
                        <MapPin size="24" color="#84c059" />
                        <h3 class="font-bold text-lg">Farm Management</h3>
                    </div>
                    <button class="bg-[#84c059] text-white px-4 py-2 rounded-lg font-medium hover:bg-[#6fa045] flex items-center gap-2">
                        <Plus size="16" />
                        Add Farm
                    </button>
                </div>
                
                <div class="grid grid-cols-1 gap-4">
                    {#each soilHealthData as farm, index}
                        <div class="p-4 bg-[#f3eee6] rounded-xl border border-[#e8e3d9]">
                            <div class="flex items-center justify-between mb-4">
                                <div class="flex items-center gap-3">
                                    <div class="w-4 h-4 rounded-full" style="background-color: {farmLocations[index]?.color || '#84c059'}"></div>
                                    <div>
                                        <p class="font-semibold text-lg">{farm.name}</p>
                                        <p class="text-sm text-gray-600">
                                            {farmLocations[index]?.lat}, {farmLocations[index]?.lng}
                                        </p>
                                    </div>
                                </div>
                                <div class="flex gap-2">
                                    <button class="p-2 bg-white rounded-lg hover:bg-gray-50">
                                        <Eye size="16" color="#84c059" />
                                    </button>
                                    <button class="p-2 bg-white rounded-lg hover:bg-gray-50">
                                        <Edit size="16" color="#84c059" />
                                    </button>
                                </div>
                            </div>
                            
                            <div class="grid grid-cols-4 gap-4">
                                <div class="text-center p-3 bg-white rounded-lg">
                                    <p class="text-xl font-bold text-blue-600">{farm.nitrogen}%</p>
                                    <p class="text-xs text-gray-600">Nitrogen</p>
                                </div>
                                <div class="text-center p-3 bg-white rounded-lg">
                                    <p class="text-xl font-bold text-orange-600">{farm.phosphorus}%</p>
                                    <p class="text-xs text-gray-600">Phosphorus</p>
                                </div>
                                <div class="text-center p-3 bg-white rounded-lg">
                                    <p class="text-xl font-bold text-green-600">{farm.potassium}%</p>
                                    <p class="text-xs text-gray-600">Potassium</p>
                                </div>
                                <div class="text-center p-3 bg-white rounded-lg">
                                    <p class="text-xl font-bold text-purple-600">{farm.ph}</p>
                                    <p class="text-xs text-gray-600">pH Level</p>
                                </div>
                            </div>
                        </div>
                    {/each}
                </div>
            </div>
        {/if}
    </div>
</div>