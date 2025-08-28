<script>
    import { Sprout, MapPin, ChartLine, Umbrella, Leaf, Sun, CloudRain, Droplet, Wind, Clover, Calendar, Pickaxe, BrainCircuit, Globe, History } from '@lucide/svelte';
    import { goto } from "$app/navigation";

    // State variables
    let language = 'English';
    let selectedFarm = null;
    let showSoilWeather = false;
    let showCrops = false;
    let selectedCrop = null;
    let showSuggestions = false;

    // Farms data
    const farms = [
        {
            id: 'kalinawan',
            name: 'Kalinawan Farm',
            icon: '🌾',
            details: '12.5 hectares • Clay-loam soil',
            location: 'Cagayan De Oro, Misamis Oriental'
        },
        {
            id: 'malasag',
            name: 'Malasag Organic Farm',
            icon: '🥕',
            details: '8.2 hectares • Sandy-loam soil',
            location: 'Malaybalay, Bukidnon'
        }
    ];

    // Crops data
    const crops = [
        {
            id: 'tomato',
            name: 'Tomato',
            icon: '🍅',
            category: 'Fruit Vegetable',
            description: 'Thrives in clay-loam soil with good drainage. High potassium supports fruit development. Current weather pattern ideal for transplanting.',
            details: {
                growth: '75-85 days',
                water: 'Moderate',
                spacing: '60cm apart',
                season: 'Year-round',
                ph: '6.0-7.0',
                yield: '2-3 kg/plant'
            }
        },
        {
            id: 'eggplant',
            name: 'Eggplant',
            icon: '🍆',
            category: 'Fruit Vegetable',
            description: 'Excellent nitrogen uptake matches your soil profile. Heat-tolerant variety perfect for current season. Good rotation crop after rice.',
            details: {
                growth: '70-80 days',
                water: 'Moderate - High',
                spacing: '45cm apart',
                season: 'Dry-Wet Transition',
                ph: '6.5-7.5',
                yield: '1.5-2 kg/plant'
            }
        },
        {
            id: 'okra',
            name: 'Okra',
            icon: '🌱',
            category: 'Pod Vegetable',
            description: 'Low phosphorus requirement suits your soil. Drought-resistant backup for dry spells. Fast-growing summer crop.',
            details: {
                growth: '50-65 days',
                water: 'Low - Moderate',
                spacing: '30cm apart',
                season: 'Hot/Dry Season',
                ph: '6.0-8.0',
                yield: '0.8-1.2 kg/plant'
            }
        }
    ];

    function handleLogout() {
        goto('/');
    }

    function toggleLanguage() {
        language = language === 'English' ? 'Filipino' : 'English';
    }

    function goToHistory() {
        goto('/history');
    }

    function selectFarm(farmId) {
        if (selectedFarm === farmId) {
            selectedFarm = null;
            showSoilWeather = false;
            showCrops = false;
            selectedCrop = null;
            showSuggestions = false;
        } else {
            selectedFarm = farmId;
        }
    }

    function confirmFarmSelection() {
        if (selectedFarm) {
            showSoilWeather = true;
        }
    }

    function generateAIPlan() {
        showCrops = true;
    }

    function selectCrop(cropId) {
        if (selectedCrop === cropId) {
            selectedCrop = null;
            showSuggestions = false;
        } else {
            selectedCrop = cropId;
        }
    }

    function confirmCropSelection() {
        if (selectedCrop) {
            showSuggestions = true;
        }
    }

    function startFarming() {
        alert('🎉 Multi-crop suggestion successful! Your farming plan has been created and saved. You can now begin implementing your optimized crop rotation schedule.');
    }
</script>

<div class="max-w-lg m-auto flex flex-col gap-8 items-center justify-center py-14">

    <div class="flex items-center bg-[#84c059] rounded-xl shadow-2xl justify-between py-6 px-8 w-full">
        <div class="flex items-center gap-4">
            <div class="bg-white/10 rounded-full">
                <Sprout size="60" color="#ffffff" />
            </div>
            <div>
                <p class="text-3xl font-bold text-white">CropWise</p>
                <p class="text-sm font-semibold text-white">Welcome, Juan Carlos Santos</p>
            </div>
        </div>
        <button 
            type="button"
            onclick={handleLogout}
            class="ml-10 bg-white/20 text-white font-semibold px-4 py-2 rounded-lg hover:bg-white/30 cursor-pointer">
            Logout</button>
    </div>

    <!-- pills for history and language -->
    <div class="flex gap-2 w-full">
        <button 
            onclick={toggleLanguage}
            class="flex items-center gap-2 px-4.5 py-2.5 rounded- font-semibold bg-white rounded-2xl shadow-2xl cursor-pointer hover:opacity-90">
            <Globe size="20" />
            <p class="text-sm">{language}</p>
        </button>
        <button 
            onclick={goToHistory}
            class="flex items-center gap-2 px-4.5 py-2.5 rounded- font-semibold bg-white rounded-2xl shadow-2xl cursor-pointer hover:opacity-90">
            <History size="20" />
            <p class="text-sm">History</p>
        </button>
    </div>

    <div class="flex flex-col gap-4 p-6 bg-white/50 rounded-xl shadow-2xl w-full">
        <div class="flex items-center gap-2">
            <MapPin size="30" color="#84c059" />
            <p class="font-bold text-lg">My Farms</p>
        </div>

        <div class="flex flex-col gap-4">
            <p>Select your farm and we'll show you details.</p>
            
            {#each farms as farm}
                <button 
                    onclick={() => selectFarm(farm.id)}
                    class="flex items-center p-4 bg-[#f3eee6] rounded-xl border-2 cursor-pointer transition-all duration-200 {selectedFarm === farm.id ? 'border-[#84c059] bg-[#e8f5e8] shadow-md' : 'border-[#e8e3d9] hover:border-[#d0cab5]'}">
                    <span class="text-5xl mr-3.5">{farm.icon}</span>
                    <div class="text-left">
                        <p class="font-semibold mb-1">{farm.name}</p>
                        <p>{farm.details}</p>
                        <p>{farm.location}</p>
                    </div>
                </button>
            {/each}
        </div>
    </div>

    {#if selectedFarm && !showSoilWeather}
        <button 
            onclick={confirmFarmSelection}
            class="flex items-center justify-center p-4 w-full bg-[#84c059] rounded-3xl shadow-2xl gap-2 cursor-pointer hover:opacity-90">
            <MapPin size="28" color="#ffffff" />
            <p class="text-lg font-semibold text-white">Select Farm</p>
        </button>
    {/if}

    {#if showSoilWeather}
        <hr class="mt-4 w-full h-[2px] border-0 bg-gradient-to-r from-transparent via-[#e8e3d9] to-transparent" />

        <!-- soil health -->
        <div class="flex flex-col gap-4 p-6 bg-white/50 rounded-xl shadow-2xl w-full">
            <div class="flex items-center gap-2">
                <ChartLine size="30" color="#84c059" />
                <p class="font-bold text-lg">Soil Health</p>
            </div>

            <div class="grid grid-cols-2 gap-3 mt-4">
                
                {#snippet healthCard(element, symbol, percent, status, statusColor, actualValue, unit, maxValue)}
                    <div class="flex flex-col p-4 bg-gradient-to-br from-white to-gray-50 rounded-xl border border-gray-200 shadow-sm">
                        <div class="flex items-center justify-between mb-3">
                            <div class="flex items-center gap-2">
                                <span class="text-lg">{symbol}</span>
                                <span class="text-sm font-semibold text-gray-800">{element}</span>
                            </div>
                            <span class="text-xs px-2 py-1 rounded-full {statusColor} font-medium">{status}</span>
                        </div>
                        
                        <div class="w-full bg-gray-200 rounded-full h-2.5 overflow-hidden mb-2">
                            <div class="bg-gradient-to-r from-[#84c059] to-[#6fa045] h-2.5 rounded-full transition-all duration-300" style={`width: ${percent}%`}></div>
                        </div>
                        
                        <div class="flex justify-between items-center text-xs text-gray-600">
                            <span>0</span>
                            <span class="font-semibold text-[#84c059]">{actualValue}{unit}</span>
                            <span>{maxValue}{unit}</span>
                        </div>
                    </div>
                {/snippet}

                {@render healthCard('Nitrogen', '𝐍', 75, 'Good', 'bg-green-100 text-green-700', '75', '%', '100')}
                {@render healthCard('Phosphorus', '𝐏', 65, 'Moderate', 'bg-yellow-100 text-yellow-700', '65', '%', '100')}
                {@render healthCard('Potassium', '𝐊', 85, 'Excellent', 'bg-emerald-100 text-emerald-700', '85', '%', '100')}
                {@render healthCard('Salinity', '🧂', 18, 'Low', 'bg-blue-100 text-blue-700', '1.2', ' dS/m', '6.5')}
                {@render healthCard('Acidity', '🧪', 69, 'Optimal', 'bg-green-100 text-green-700', '6.8', ' pH', '10')}
                {@render healthCard('Moisture', '💧', 80, 'Good', 'bg-cyan-100 text-cyan-700', '80', '%', '100')}
                {@render healthCard('Organic Matter', '🍂', 30, 'Low', 'bg-orange-100 text-orange-700', '3.2', '%', '10')}
                
            </div>

            <!-- Overall Assessment -->
            <div class="flex flex-col gap-3 px-6 py-6 rounded-xl bg-gradient-to-r from-[#f3eee6] to-[#e8f5e8] border-l-4 border-[#84c059]">
                <div class="flex items-center gap-2">
                    <span class="text-2xl">💡</span>
                    <p class="font-semibold text-lg">Summary</p>
                </div>
                <p class="text-sm leading-relaxed">Low salinity is ideal for most vegetable crops. Good moisture retention suitable for current season planting. Consider adding compost or organic fertilizers to improve soil structure. </p>
            </div>
        </div>

        <div class="flex flex-col gap-4 p-6 bg-white/70 backdrop-blur-md rounded-2xl shadow-xl w-full">
            <!-- Header -->
            <div class="flex items-center gap-2">
                <Umbrella size="28" color="#84c059" />
                <p class="font-bold text-xl">Weather Forecast</p>
            </div>

            <p class="text-sm font-medium">Today is August 27, 2025 • Wet Season (Southwest Monsoon)</p>

            <!-- Today's Weather -->
            <div class="flex flex-col items-center p-6 rounded-2xl bg-gradient-to-br bg-[#f3eee6] gap-3">
                <span class="text-6xl mb-2">⛅</span>
                <div class="text-center">
                    <div class="flex items-center justify-center gap-4 text-lg font-semibold">
                        <div class="flex items-center gap-1">
                            <span class="text-sm">☀️</span>
                            <span>32°C</span>
                            <span class="text-xs text-gray-600 font-medium">Day</span>
                        </div>
                        <span class="text-gray-400">/</span>
                        <div class="flex items-center gap-1">
                            <span class="text-sm">🌙</span>
                            <span>24°C</span>
                            <span class="text-xs text-gray-600 font-medium">Night</span>
                        </div>
                    </div>
                    <p class="text-sm text-gray-600 font-medium mt-1">Partly Cloudy</p>
                </div>
                
                <div class="grid grid-cols-3 gap-2 mt-2 w-full">
                    <div class="flex flex-col items-center p-4 bg-white/50 rounded-lg">
                        <Droplet class="w-6 h-6 mb-1 text-blue-400" />
                        <span class="text-xs font-medium text-gray-700">Humidity</span>
                        <span class="text-sm font-semibold">68%</span>
                    </div>
                    <div class="flex flex-col items-center p-4 bg-white/50 rounded-lg">
                        <CloudRain class="w-6 h-6 mb-1 text-blue-500" />
                        <span class="text-xs font-medium text-gray-700">Precipitation</span>
                        <span class="text-sm font-semibold">0 mm</span>
                    </div>
                    <div class="flex flex-col items-center p-4 bg-white/50 rounded-lg">
                        <Wind class="w-6 h-6 mb-1 text-gray-600 dark:text-gray-500" />
                        <span class="text-xs font-medium text-gray-700">Wind Speed</span>
                        <span class="text-sm font-semibold">12 km/h SW</span>
                    </div>
                </div>
            </div>

            <!-- Next days -->
            <div class="grid grid-cols-2 gap-3 mt-2">
                <div class="p-4 bg-[#f3eee6] rounded-xl flex items-center gap-3">
                    <span class="text-3xl">☀️</span>
                    <div class="flex flex-col">
                        <p class="font-semibold text-sm">Tuesday</p>
                        <div class="flex items-center gap-2 text-xs">
                            <span class="flex items-center gap-1">☀️ 33°C</span>
                            <span>/</span>
                            <span class="flex items-center gap-1">🌙 25°C</span>
                        </div>
                        <p class="text-xs text-gray-600">Sunny</p>
                    </div>
                </div>
                <div class="p-4 bg-[#f3eee6] rounded-xl flex items-center gap-3">
                    <span class="text-3xl">🌦️</span>
                    <div class="flex flex-col">
                        <p class="font-semibold text-sm">Wednesday</p>
                        <div class="flex items-center gap-2 text-xs">
                            <span class="flex items-center gap-1">☀️ 30°C</span>
                            <span>/</span>
                            <span class="flex items-center gap-1">🌙 23°C</span>
                        </div>
                        <p class="text-xs text-gray-600">Showers</p>
                    </div>
                </div>
                <div class="p-4 bg-[#f3eee6] rounded-xl flex items-center gap-3">
                    <span class="text-3xl">⛅</span>
                    <div class="flex flex-col">
                        <p class="font-semibold text-sm">Thursday</p>
                        <div class="flex items-center gap-2 text-xs">
                            <span class="flex items-center gap-1">☀️ 31°C</span>
                            <span>/</span>
                            <span class="flex items-center gap-1">🌙 24°C</span>
                        </div>
                        <p class="text-xs text-gray-600">Cloudy</p>
                    </div>
                </div>
                <div class="p-4 bg-[#f3eee6] rounded-xl flex items-center gap-3">
                    <span class="text-3xl">🌧️</span>
                    <div class="flex flex-col">
                        <p class="font-semibold text-sm">Friday</p>
                        <div class="flex items-center gap-2 text-xs">
                            <span class="flex items-center gap-1">☀️ 29°C</span>
                            <span>/</span>
                            <span class="flex items-center gap-1">🌙 22°C</span>
                        </div>
                        <p class="text-xs text-gray-600">Heavy Rain</p>
                    </div>
                </div>
            </div>

            <!-- weather summary -->
            <div class="flex flex-col gap-3 px-6 py-6 rounded-xl bg-gradient-to-r from-[#f3eee6] to-[#e8f5e8] border-l-4 border-[#84c059]">
                <div class="flex items-center gap-2">
                    <span class="text-2xl">💡</span>
                    <p class="font-semibold text-lg">Summary</p>
                </div>
                <p class="text-sm leading-relaxed">Southwest monsoon pattern typical for August. High humidity Thursday may require disease monitoring. Overall weather supports current season crop establishment. </p>
            </div>
        </div>

        {#if !showCrops}
            <button 
                onclick={generateAIPlan}
                class="flex items-center justify-center p-4 w-full bg-[#84c059] rounded-3xl shadow-2xl gap-2 cursor-pointer hover:opacity-90">
                <BrainCircuit size="28" color="#ffffff" />
                <p class="text-lg font-semibold text-white">Generate AI Plan</p>
            </button>
        {/if}
    {/if}

    {#if showCrops}
        <hr class="mt-4 w-full h-[2px] border-0 bg-gradient-to-r from-transparent via-[#e8e3d9] to-transparent" />

        <!-- recommended crops -->
        <div class="flex flex-col gap-4 p-6 bg-white/70 backdrop-blur-md rounded-2xl shadow-xl w-full">
            <!-- Header -->
            <div class="flex items-center gap-2">
                <Leaf size="28" color="#84c059" />
                <p class="font-bold text-xl">Recommended Crops</p>
            </div>

            <p>Select your starting crop and we'll plan your crop rotation.</p>

            {#each crops as crop}
                <button 
                    onclick={() => selectCrop(crop.id)}
                    class="flex flex-col border-2 rounded-xl p-4 gap-4 cursor-pointer transition-all duration-200 {selectedCrop === crop.id ? 'border-[#84c059] bg-[#e8f5e8] shadow-md' : 'border-[#e8e3d9] bg-[#f3eee6] hover:border-[#d0cab5]'}">
                    <!-- Left: Icon + Name -->
                    <div class="flex items-center gap-3">
                        <span class="text-6xl">{crop.icon}</span>
                        <div class="flex justify-between items-center w-full">
                            <p class="font-semibold text-xl text-left">{crop.name}</p>
                            <span class="text-xs bg-black text-white px-2 py-1.5 rounded-lg w-fit">{crop.category}</span>
                        </div>
                    </div>

                    <!-- Right: Details -->
                    <div class="flex flex-col gap-3">
                        <!-- Description -->
                        <p class="text-sm text-gray-700 leading-snug text-left">
                            {crop.description}
                        </p>

                        <!-- Quick Facts (grid instead of stacked) -->
                        <div class="grid grid-cols-2 gap-y-1 gap-x-4 text-sm text-left">
                            <p><span class="font-semibold">🌱 Growth:</span> {crop.details.growth}</p>
                            <p><span class="font-semibold">💧 Water:</span> {crop.details.water}</p>
                            <p><span class="font-semibold">📏 Spacing:</span> {crop.details.spacing}</p>
                            <p><span class="font-semibold">☀️ Season:</span> {crop.details.season}</p>
                            <p><span class="font-semibold">🧪 pH:</span> {crop.details.ph}</p>
                            <p><span class="font-semibold">🥕 Yield:</span> {crop.details.yield}</p>
                        </div>
                    </div>
                </button>
            {/each}
        </div>

        {#if selectedCrop && !showSuggestions}
            <button 
                onclick={confirmCropSelection}
                class="flex items-center justify-center p-4 w-full bg-[#84c059] rounded-3xl shadow-2xl gap-2 cursor-pointer hover:opacity-90">
                <Clover size="28" color="#ffffff" />
                <p class="text-lg font-semibold text-white">Select Crop</p>
            </button>
        {/if}
    {/if}

    {#if showSuggestions}
        <hr class="mt-4 w-full h-[2px] border-0 bg-gradient-to-r from-transparent via-[#e8e3d9] to-transparent" />

        <div class="flex flex-col gap-6 p-6 bg-white/70 backdrop-blur-md rounded-2xl shadow-xl w-full max-w-4xl mx-auto">
            <!-- Header -->
            <div class="flex items-center gap-3">
                <Clover size="28" color="#84c059" />
                <h2 class="font-bold text-xl">Multi-Cropping Suggestions</h2>
            </div>

            <!-- Good Companions Section -->
            <div class="flex flex-col gap-4">
                <h3 class="font-semibold text-lg text-gray-700 flex items-center gap-2">
                🤝 <span>Good Companions (Plant Together)</span>
                </h3>
                
                <div class="grid gap-4">
                <!-- Basil -->
                <div class="flex items-center gap-4 p-4 bg-green-50/80 rounded-xl border-2 border-green-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🌿</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">Basil</p>
                    <p class="text-gray-600 text-sm mb-2">Repels pests, improves tomato flavor. Plant 30cm away from tomato base.</p>
                    <div class="flex flex-wrap gap-2">
                        <span class="px-2 py-1 bg-green-100 text-green-700 rounded-lg text-xs">Pest Control</span>
                        <span class="px-2 py-1 bg-green-100 text-green-700 rounded-lg text-xs">Flavor Enhancer</span>
                        <span class="px-2 py-1 bg-green-100 text-green-700 rounded-lg text-xs">Space Efficient</span>
                    </div>
                    </div>
                </div>

                <!-- Carrots -->
                <div class="flex items-center gap-4 p-4 bg-orange-50/80 rounded-xl border-2 border-orange-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🥕</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">Carrots</p>
                    <p class="text-gray-600 text-sm mb-2">Aerates soil, different root depth. Harvest before tomatoes need more space.</p>
                    <div class="flex flex-wrap gap-2">
                        <span class="px-2 py-1 bg-orange-100 text-orange-700 rounded-lg text-xs">Soil Improvement</span>
                        <span class="px-2 py-1 bg-orange-100 text-orange-700 rounded-lg text-xs">Early Harvest</span>
                        <span class="px-2 py-1 bg-orange-100 text-orange-700 rounded-lg text-xs">Root Diversity</span>
                    </div>
                    </div>
                </div>

                <!-- Lettuce -->
                <div class="flex items-center gap-4 p-4 bg-emerald-50/80 rounded-xl border-2 border-emerald-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🌱</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">Lettuce</p>
                    <p class="text-gray-600 text-sm mb-2">Ground cover reduces weeds, cool-season crop. Plant between tomato rows.</p>
                    <div class="flex flex-wrap gap-2">
                        <span class="px-2 py-1 bg-emerald-100 text-emerald-700 rounded-lg text-xs">Ground Cover</span>
                        <span class="px-2 py-1 bg-emerald-100 text-emerald-700 rounded-lg text-xs">Quick Growing</span>
                        <span class="px-2 py-1 bg-emerald-100 text-emerald-700 rounded-lg text-xs">Weed Control</span>
                    </div>
                    </div>
                </div>

                <!-- Onions -->
                <div class="flex items-center gap-4 p-4 bg-purple-50/80 rounded-xl border-2 border-purple-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🧅</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">Onions</p>
                    <p class="text-gray-600 text-sm mb-2">Natural pest deterrent, different nutrient needs. Border planting recommended.</p>
                    <div class="flex flex-wrap gap-2">
                        <span class="px-2 py-1 bg-purple-100 text-purple-700 rounded-lg text-xs">Pest Deterrent</span>
                        <span class="px-2 py-1 bg-purple-100 text-purple-700 rounded-lg text-xs">Sulfur Benefits</span>
                        <span class="px-2 py-1 bg-purple-100 text-purple-700 rounded-lg text-xs">Perimeter Plant</span>
                    </div>
                    </div>
                </div>
                </div>
            </div>

            <!-- Bad Neighbors Section -->
            <div class="flex flex-col gap-4">
                <h3 class="font-semibold text-lg text-gray-700 flex items-center gap-2">
                ❌ <span>Bad Neighbors (Avoid Planting Together)</span>
                </h3>
                
                <div class="grid gap-4">
                <!-- Potatoes -->
                <div class="flex items-center gap-4 p-4 bg-red-50/80 rounded-xl border-2 border-red-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🥔</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">Potatoes</p>
                    <p class="text-gray-600 text-sm mb-2">Both are nightshades, compete for nutrients. Share same diseases and pests.</p>
                    <div class="flex flex-wrap gap-2">
                        <span class="px-2 py-1 bg-red-100 text-red-700 rounded-lg text-xs">Disease Risk</span>
                        <span class="px-2 py-1 bg-red-100 text-red-700 rounded-lg text-xs">Nutrient Competition</span>
                        <span class="px-2 py-1 bg-red-100 text-red-700 rounded-lg text-xs">Same Family</span>
                    </div>
                    </div>
                </div>

                <!-- Corn -->
                <div class="flex items-center gap-4 p-4 bg-yellow-50/80 rounded-xl border-2 border-yellow-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🌽</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">Corn</p>
                    <p class="text-gray-600 text-sm mb-2">Heavy nitrogen user, creates too much shade. Attracts tomato fruitworm.</p>
                    <div class="flex flex-wrap gap-2">
                        <span class="px-2 py-1 bg-yellow-100 text-yellow-700 rounded-lg text-xs">Heavy Feeder</span>
                        <span class="px-2 py-1 bg-yellow-100 text-yellow-700 rounded-lg text-xs">Shading Issues</span>
                        <span class="px-2 py-1 bg-yellow-100 text-yellow-700 rounded-lg text-xs">Pest Attraction</span>
                    </div>
                    </div>
                </div>
                </div>
            </div>

            <!-- After Harvest Section -->
            <div class="flex flex-col gap-4">
                <h3 class="font-semibold text-lg text-gray-700 flex items-center gap-2">
                🔄 <span>After Harvest Succession</span>
                </h3>
                <p class="text-gray-600 text-sm">These crops will benefit from leftover nutrients after tomato harvest:</p>
                
                <div class="grid gap-4">
                <!-- Leafy Greens -->
                <div class="flex items-center gap-4 p-4 bg-teal-50/80 rounded-xl border-2 border-teal-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🥬</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">Leafy Greens (Pechay, Kangkong)</p>
                    <p class="text-gray-600 text-sm mb-2">Will use remaining nitrogen efficiently. Quick 30-45 day harvest cycle.</p>
                    <div class="flex flex-wrap gap-2">
                        <span class="px-2 py-1 bg-teal-100 text-teal-700 rounded-lg text-xs">Nitrogen Utilization</span>
                        <span class="px-2 py-1 bg-teal-100 text-teal-700 rounded-lg text-xs">Fast Growing</span>
                        <span class="px-2 py-1 bg-teal-100 text-teal-700 rounded-lg text-xs">Cool Season</span>
                    </div>
                    </div>
                </div>

                <!-- Beans -->
                <div class="flex items-center gap-4 p-4 bg-lime-50/80 rounded-xl border-2 border-lime-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🫘</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">Beans</p>
                    <p class="text-gray-600 text-sm mb-2">Nitrogen-fixing legume restores soil nutrients. Improves soil for next crop cycle.</p>
                    <div class="flex flex-wrap gap-2">
                        <span class="px-2 py-1 bg-lime-100 text-lime-700 rounded-lg text-xs">Soil Restoration</span>
                        <span class="px-2 py-1 bg-lime-100 text-lime-700 rounded-lg text-xs">Nitrogen Fixing</span>
                        <span class="px-2 py-1 bg-lime-100 text-lime-700 rounded-lg text-xs">Protein Crop</span>
                    </div>
                    </div>
                </div>
                </div>
            </div>
        </div>


        <div class="flex flex-col gap-6 p-6 bg-white/70 backdrop-blur-md rounded-2xl shadow-xl w-full max-w-5xl mx-auto">
      <!-- Header -->
      <div class="flex items-center gap-3">
        <Calendar size="28" color="#84c059" />
        <h2 class="font-bold text-xl">Multi-Crop Planting Timeline</h2>
      </div>

      <!-- Timeline Container -->
      <div class="relative">
        <!-- Timeline Line -->
        <div class="absolute left-8 top-0 bottom-0 w-1 bg-gradient-to-b from-green-400 to-emerald-600 rounded-full"></div>
        
        <div class="flex flex-col gap-6">
          <!-- Week 1-2 -->
          <div class="relative flex gap-6 items-start">
            <div class="flex-shrink-0 w-16 h-16 bg-green-100/80 backdrop-blur-sm border-4 border-green-400 rounded-full flex items-center justify-center z-10">
              <span class="text-2xl">🚜</span>
            </div>
            <div class="flex-1 bg-green-50/80 backdrop-blur-sm rounded-xl border-2 border-green-400/50 p-5">
              <div class="flex items-center gap-2 mb-3">
                <h3 class="font-bold text-lg text-gray-800">Week 1-2 (Land Preparation)</h3>
              </div>
              <div class="mb-4">
                <h4 class="font-semibold text-gray-700 mb-2">🚜 Soil Preparation & Base Fertilizer</h4>
                <p class="text-gray-600 text-sm mb-3">Till soil, add compost (2 tons/hectare). Apply base fertilizer: 14-14-14 NPK (200kg/hectare).</p>
                <div class="flex flex-wrap gap-2 mb-3">
                  <span class="px-3 py-1 bg-green-200 text-green-800 rounded-lg text-sm font-medium">Plant: Carrot seeds directly, start tomato seedlings in nursery</span>
                </div>
              </div>
            </div>
          </div>

          <!-- Week 3-4 -->
          <div class="relative flex gap-6 items-start">
            <div class="flex-shrink-0 w-16 h-16 bg-blue-100/80 backdrop-blur-sm border-4 border-blue-400 rounded-full flex items-center justify-center z-10">
              <span class="text-2xl">🌱</span>
            </div>
            <div class="flex-1 bg-blue-50/80 backdrop-blur-sm rounded-xl border-2 border-blue-400/50 p-5">
              <div class="flex items-center gap-2 mb-3">
                <h3 class="font-bold text-lg text-gray-800">Week 3-4 (Transplanting)</h3>
              </div>
              <div class="mb-4">
                <h4 class="font-semibold text-gray-700 mb-2">🌱 Transplant Tomatoes & Plant Basil</h4>
                <p class="text-gray-600 text-sm mb-3">Transplant 3-week-old tomato seedlings (60cm spacing). Plant basil seedlings around tomatoes.</p>
                <div class="bg-blue-100/60 rounded-lg p-3">
                  <span class="text-sm font-medium text-blue-800">Care: Daily watering, install stakes for tomatoes</span>
                </div>
              </div>
            </div>
          </div>

          <!-- Week 6-8 -->
          <div class="relative flex gap-6 items-start">
            <div class="flex-shrink-0 w-16 h-16 bg-purple-100/80 backdrop-blur-sm border-4 border-purple-400 rounded-full flex items-center justify-center z-10">
              <span class="text-2xl">🌿</span>
            </div>
            <div class="flex-1 bg-purple-50/80 backdrop-blur-sm rounded-xl border-2 border-purple-400/50 p-5">
              <div class="flex items-center gap-2 mb-3">
                <h3 class="font-bold text-lg text-gray-800">Week 6-8 (First Side Dress)</h3>
              </div>
              <div class="mb-4">
                <h4 class="font-semibold text-gray-700 mb-2">🌿 Side Dressing & Maintenance</h4>
                <p class="text-gray-600 text-sm mb-3">Apply first side dress fertilizer: Urea (50kg/hectare). Harvest baby carrots for thinning.</p>
                <div class="bg-purple-100/60 rounded-lg p-3">
                  <span class="text-sm font-medium text-purple-800">Care: Prune tomato suckers, tie to stakes, weed management</span>
                </div>
              </div>
            </div>
          </div>

          <!-- Week 10-12 -->
          <div class="relative flex gap-6 items-start">
            <div class="flex-shrink-0 w-16 h-16 bg-pink-100/80 backdrop-blur-sm border-4 border-pink-400 rounded-full flex items-center justify-center z-10">
              <span class="text-2xl">🌸</span>
            </div>
            <div class="flex-1 bg-pink-50/80 backdrop-blur-sm rounded-xl border-2 border-pink-400/50 p-5">
              <div class="flex items-center gap-2 mb-3">
                <h3 class="font-bold text-lg text-gray-800">Week 10-12 (Flowering Stage)</h3>
              </div>
              <div class="mb-4">
                <h4 class="font-semibold text-gray-700 mb-2">🌸 Flowering & Second Fertilizer</h4>
                <p class="text-gray-600 text-sm mb-3">Tomatoes flowering, apply phosphorus-rich fertilizer (16-20-0). Harvest mature carrots.</p>
                <div class="bg-pink-100/60 rounded-lg p-3">
                  <span class="text-sm font-medium text-pink-800">Care: Prune tomato suckers, tie to stakes, weed management</span>
                </div>
              </div>
            </div>
          </div>

          <!-- Week 14-18 -->
          <div class="relative flex gap-6 items-start">
            <div class="flex-shrink-0 w-16 h-16 bg-orange-100/80 backdrop-blur-sm border-4 border-orange-400 rounded-full flex items-center justify-center z-10">
              <span class="text-2xl">🍅</span>
            </div>
            <div class="flex-1 bg-orange-50/80 backdrop-blur-sm rounded-xl border-2 border-orange-400/50 p-5">
              <div class="flex items-center gap-2 mb-3">
                <h3 class="font-bold text-lg text-gray-800">Week 14-18 (Fruiting & Harvest)</h3>
              </div>
              <div class="mb-4">
                <h4 class="font-semibold text-gray-700 mb-2">🍅 Main Harvest Period</h4>
                <p class="text-gray-600 text-sm mb-3">Peak tomato harvest begins. Apply final fertilizer: high-potassium (0-0-50).</p>
                <div class="bg-orange-100/60 rounded-lg p-3">
                  <span class="text-sm font-medium text-orange-800">Harvest: Continuous tomato picking, weekly basil harvest, prepare succession beds</span>
                </div>
              </div>
            </div>
          </div>

          <!-- Week 20+ -->
          <div class="relative flex gap-6 items-start">
            <div class="flex-shrink-0 w-16 h-16 bg-teal-100/80 backdrop-blur-sm border-4 border-teal-400 rounded-full flex items-center justify-center z-10">
              <span class="text-2xl">🔄</span>
            </div>
            <div class="flex-1 bg-teal-50/80 backdrop-blur-sm rounded-xl border-2 border-teal-400/50 p-5">
              <div class="flex items-center gap-2 mb-3">
                <h3 class="font-bold text-lg text-gray-800">Week 20+ (Succession Planting)</h3>
              </div>
              <div class="mb-4">
                <h4 class="font-semibold text-gray-700 mb-2">🔄 End Cycle & Plant Next Crop</h4>
                <p class="text-gray-600 text-sm mb-3">Final tomato harvest, clear beds. Plant leafy greens or beans for succession crop.</p>
                <div class="bg-teal-100/60 rounded-lg p-3">
                  <span class="text-sm font-medium text-teal-800">Prep: Soil test, add organic matter, plan next rotation cycle</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

        <button 
            onclick={startFarming}
            class="flex items-center justify-center p-4 w-full bg-[#84c059] rounded-3xl shadow-2xl gap-2 cursor-pointer hover:opacity-90">
            <Pickaxe size="28" color="#ffffff" />
            <p class="text-lg font-semibold text-white">Start Farming</p>
        </button>
    {/if}

</div>