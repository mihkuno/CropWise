<script>
    import { Sprout, MapPin, ChartLine, Umbrella, Leaf, Sun, CloudRain, Droplet, Wind, Clover, Calendar, Pickaxe, BrainCircuit, Globe, History } from '@lucide/svelte';
    import { goto } from "$app/navigation";

    // State variables
    let language = 'English'; // Initial language
    let selectedFarm = null;
    let showSoilWeather = false;
    let showCrops = false;
    let selectedCrop = null;
    let showSuggestions = false;

    // Translation dictionaries
    const translations = {
        'English': {
            welcome: 'Welcome, Juan Carlos Santos',
            logout: 'Logout',
            history: 'History',
            myFarms: 'My Farms',
            selectFarmDetails: 'Select your farm and we\'ll show you details.',
            selectFarmButton: 'Select Farm',
            soilHealth: 'Soil Health',
            summary: 'Summary',
            lowSalinity: 'Low salinity is ideal for most vegetable crops. Good moisture retention suitable for current season planting. Consider adding compost or organic fertilizers to improve soil structure.',
            weatherForecast: 'Weather Forecast',
            todayIs: 'Today is August 27, 2025 • Wet Season (Southwest Monsoon)',
            day: 'Day',
            night: 'Night',
            partlyCloudy: 'Partly Cloudy',
            humidity: 'Humidity',
            precipitation: 'Precipitation',
            windSpeed: 'Wind Speed',
            sunny: 'Sunny',
            showers: 'Showers',
            cloudy: 'Cloudy',
            heavyRain: 'Heavy Rain',
            southwestMonsoon: 'Southwest monsoon pattern typical for August. High humidity Thursday may require disease monitoring. Overall weather supports current season crop establishment.',
            generateAIPlan: 'Generate AI Plan',
            recommendedCrops: 'Recommended Crops',
            selectStartingCrop: 'Select your starting crop and we\'ll plan your crop rotation.',
            selectCropButton: 'Select Crop',
            multiCroppingSuggestions: 'Multi-Cropping Suggestions',
            goodCompanions: 'Good Companions (Plant Together)',
            badNeighbors: 'Bad Neighbors (Avoid Planting Together)',
            afterHarvest: 'After Harvest Succession',
            afterHarvestDesc: 'These crops will benefit from leftover nutrients after tomato harvest:',
            multiCropTimeline: 'Multi-Crop Planting Timeline',
            landPreparation: 'Week 1-2 (Land Preparation)',
            soilPrepDetails: 'Till soil, add compost (2 tons/hectare). Apply base fertilizer: 14-14-14 NPK (200kg/hectare).',
            plantCarrotTomato: 'Plant: Carrot seeds directly, start tomato seedlings in nursery',
            transplanting: 'Week 3-4 (Transplanting)',
            transplantTomatoesBasil: 'Transplant 3-week-old tomato seedlings (60cm spacing). Plant basil seedlings around tomatoes.',
            careDailyWatering: 'Care: Daily watering, install stakes for tomatoes',
            firstSideDress: 'Week 6-8 (First Side Dress)',
            sideDressingMaintenance: 'Apply first side dress fertilizer: Urea (50kg/hectare). Harvest baby carrots for thinning.',
            pruneTomatoSuckers: 'Care: Prune tomato suckers, tie to stakes, weed management',
            floweringStage: 'Week 10-12 (Flowering Stage)',
            floweringSecondFertilizer: 'Tomatoes flowering, apply phosphorus-rich fertilizer (16-20-0). Harvest mature carrots.',
            mainHarvestPeriod: 'Week 14-18 (Fruiting & Harvest)',
            peakTomatoHarvest: 'Peak tomato harvest begins. Apply final fertilizer: high-potassium (0-0-50).',
            harvestContinuous: 'Harvest: Continuous tomato picking, weekly basil harvest, prepare succession beds',
            successionPlanting: 'Week 20+ (Succession Planting)',
            endCyclePlantNext: 'Final tomato harvest, clear beds. Plant leafy greens or beans for succession crop.',
            prepSoilTest: 'Prep: Soil test, add organic matter, plan next rotation cycle',
            startFarming: 'Start Farming',
            multiCropSuccess: '🎉 Multi-crop suggestion successful! Your farming plan has been created and saved. You can now begin implementing your optimized crop rotation schedule.',

            // Farm specific details
            kalinawanDetails: '12.5 hectares • Clay-loam soil',
            malasagDetails: '8.2 hectares • Sandy-loam soil',
            kalinawanLocation: 'Cagayan De Oro, Misamis Oriental',
            malasagLocation: 'Malaybalay, Bukidnon',

            // Crop specific descriptions
            tomatoDescription: 'Thrives in clay-loam soil with good drainage. High potassium supports fruit development. Current weather pattern ideal for transplanting.',
            eggplantDescription: 'Excellent nitrogen uptake matches your soil profile. Heat-tolerant variety perfect for current season. Good rotation crop after rice.',
            okraDescription: 'Low phosphorus requirement suits your soil. Drought-resistant backup for dry spells. Fast-growing summer crop.',

            // Companion planting
            basilName: 'Basil',
            basilDesc: 'Repels pests, improves tomato flavor. Plant 30cm away from tomato base.',
            basilTags: ['Pest Control', 'Flavor Enhancer', 'Space Efficient'],
            carrotsName: 'Carrots',
            carrotsDesc: 'Aerates soil, different root depth. Harvest before tomatoes need more space.',
            carrotsTags: ['Soil Improvement', 'Early Harvest', 'Root Diversity'],
            lettuceName: 'Lettuce',
            lettuceDesc: 'Ground cover reduces weeds, cool-season crop. Plant between tomato rows.',
            lettuceTags: ['Ground Cover', 'Quick Growing', 'Weed Control'],
            onionsName: 'Onions',
            onionsDesc: 'Natural pest deterrent, different nutrient needs. Border planting recommended.',
            onionsTags: ['Pest Deterrent', 'Sulfur Benefits', 'Perimeter Plant'],
            
            // Bad neighbors
            potatoesName: 'Potatoes',
            potatoesDesc: 'Both are nightshades, compete for nutrients. Share same diseases and pests.',
            potatoesTags: ['Disease Risk', 'Nutrient Competition', 'Same Family'],
            cornName: 'Corn',
            cornDesc: 'Heavy nitrogen user, creates too much shade. Attracts tomato fruitworm.',
            cornTags: ['Heavy Feeder', 'Shading Issues', 'Pest Attraction'],

            // After harvest
            leafyGreensName: 'Leafy Greens (Pechay, Kangkong)',
            leafyGreensDesc: 'Will use remaining nitrogen efficiently. Quick 30-45 day harvest cycle.',
            leafyGreensTags: ['Nitrogen Utilization', 'Fast Growing', 'Cool Season'],
            beansName: 'Beans',
            beansDesc: 'Nitrogen-fixing legume restores soil nutrients. Improves soil for next crop cycle.',
            beansTags: ['Soil Restoration', 'Nitrogen Fixing', 'Protein Crop'],
        },
        'Filipino': {
            welcome: 'Maligayang Pagdating, Juan Carlos Santos',
            logout: 'Mag-logout',
            history: 'Kasaysayan',
            myFarms: 'Aking mga Bukirin',
            selectFarmDetails: 'Pumili ng iyong bukirin at ipapakita namin ang mga detalye.',
            selectFarmButton: 'Pumili ng Bukirin',
            soilHealth: 'Kalusugan ng Lupa',
            summary: 'Buod',
            lowSalinity: 'Ang mababang kaasinan ay mainam para sa karamihan ng mga gulay. Maganda ang pagpapanatili ng kahalumigmigan na angkop para sa pagtatanim ngayong panahon. Isaalang-alang ang pagdaragdag ng compost o organikong pataba upang mapabuti ang istruktura ng lupa.',
            weatherForecast: 'Pagtataya ng Panahon',
            todayIs: 'Ngayon ay Agosto 27, 2025 • Tag-ulan (Habagat)',
            day: 'Araw',
            night: 'Gabi',
            partlyCloudy: 'Bahagyang Maulap',
            humidity: 'Halumigmig',
            precipitation: 'Pag-ulan',
            windSpeed: 'Bilis ng Hangin',
            sunny: 'Maaraw',
            showers: 'Pag-ulan',
            cloudy: 'Maulap',
            heavyRain: 'Malakas na Ulan',
            southwestMonsoon: 'Karaniwang pattern ng habagat para sa Agosto. Ang mataas na halumigmig sa Huwebes ay maaaring mangailangan ng pagsubaybay laban sa sakit. Sa pangkalahatan, ang panahon ay nakatutulong sa pagtatanim at pagtatatag ng pananim ngayong panahon.',
            generateAIPlan: 'Bumuo ng Plano mula sa AI',
            recommendedCrops: 'Mga Inirerekomendang Pananim',
            selectStartingCrop: 'Pumili ng panimulang pananim at gagawa kami ng plano para sa iyong crop rotation.',
            selectCropButton: 'Pumili ng Pananim',
            multiCroppingSuggestions: 'Mga Mungkahi sa Sabayang Pagtatanim',
            goodCompanions: 'Magandang Kasama (Itanim nang Magkasama)',
            badNeighbors: 'Hindi Magandang Katabi (Iwasang Itanim nang Magkasama)',
            afterHarvest: 'Pagkatapos ng Ani (Sunod na Itatanim)',
            afterHarvestDesc: 'Ang mga pananim na ito ay makikinabang mula sa natitirang sustansya matapos ang pag-ani ng kamatis:',
            multiCropTimeline: 'Timeline ng Sabayang Pagtatanim',
            landPreparation: 'Linggo 1-2 (Paghahanda ng Lupa)',
            soilPrepDetails: 'Araruhin ang lupa, magdagdag ng compost (2 tonelada/ektarya). Maglagay ng panimulang pataba: 14-14-14 NPK (200kg/ektarya).',
            plantCarrotTomato: 'Itanim: Direktang buto ng karot, at simulan ang punla ng kamatis sa punlaan',
            transplanting: 'Linggo 3-4 (Paglipat-tanim)',
            transplantTomatoesBasil: 'Ilipat ang 3-linggong gulang na punla ng kamatis (60cm pagitan). Itanim ang punla ng basil sa paligid ng kamatis.',
            careDailyWatering: 'Pangangalaga: Araw-araw na pagdidilig, maglagay ng tukod para sa mga kamatis',
            firstSideDress: 'Linggo 6-8 (Unang Side Dress)',
            sideDressingMaintenance: 'Maglagay ng unang side dress na pataba: Urea (50kg/ektarya). Anihin ang mga batang karot para sa pagbabawas.',
            pruneTomatoSuckers: 'Pangangalaga: Pagtanggal ng suwi ng kamatis, itali sa tukod, at kontrolin ang damo',
            floweringStage: 'Linggo 10-12 (Yugto ng Pamumulaklak)',
            floweringSecondFertilizer: 'Namumulaklak ang mga kamatis, maglagay ng patabang mayaman sa posporus (16-20-0). Anihin ang mga hinog na karot.',
            mainHarvestPeriod: 'Linggo 14-18 (Pamumunga at Pag-ani)',
            peakTomatoHarvest: 'Nagsisimula ang rurok ng anihan ng kamatis. Maglagay ng huling pataba: may mataas na potassium (0-0-50).',
            harvestContinuous: 'Pag-ani: Tuloy-tuloy na pagpitas ng kamatis, lingguhang pag-ani ng basil, at paghahanda ng mga susunod na taniman',
            successionPlanting: 'Linggo 20+ (Sunod na Pagtatanim)',
            endCyclePlantNext: 'Huling anihan ng kamatis, linisin ang mga taniman. Magtanim ng dahon gulay o beans bilang kasunod na pananim.',
            prepSoilTest: 'Paghahanda: Magsagawa ng pagsusuri ng lupa, magdagdag ng organikong materyal, at planuhin ang susunod na ikot ng pagtatanim',
            startFarming: 'Simulan ang Pagsasaka',
            multiCropSuccess: '🎉 Matagumpay ang mungkahi sa sabayang pagtatanim! Nalikha at na-save ang iyong plano sa pagsasaka. Maaari mo nang simulan ang pagpapatupad ng na-optimize na iskedyul ng crop rotation.',

            // Farm specific details
            kalinawanDetails: '12.5 ektarya • Lupa na clay-loam',
            malasagDetails: '8.2 ektarya • Lupa na sandy-loam',
            kalinawanLocation: 'Cagayan de Oro, Misamis Oriental',
            malasagLocation: 'Malaybalay, Bukidnon',

            // Crop specific descriptions
            tomatoDescription: 'Umuunlad sa clay-loam na lupa na may mahusay na paagusan. Ang mataas na potassium ay sumusuporta sa pagbuo ng bunga. Ang kasalukuyang pattern ng panahon ay mainam para sa paglipat-tanim.',
            eggplantDescription: 'Ang mahusay na pagsipsip ng nitrogen ay tumutugma sa iyong uri ng lupa. Ang uri na matibay sa init ay angkop ngayong panahon. Magandang kapalit na pananim pagkatapos ng palay.',
            okraDescription: 'Mababa ang pangangailangan sa posporus kaya akma sa iyong lupa. Matibay sa tagtuyot at mabilis lumaki, kaya angkop bilang backup na pananim sa tag-init.',

            // Companion planting
            basilName: 'Basil',
            basilDesc: 'Nagtataboy ng mga peste at nagpapabuti ng lasa ng kamatis. Itanim nang 30cm ang layo mula sa puno ng kamatis.',
            basilTags: ['Panlaban sa Peste', 'Pampahusay ng Lasa', 'Epektibong Espasyo'],
            carrotsName: 'Karot',
            carrotsDesc: 'Nagpapahangin sa lupa, may ibang lalim ng ugat. Anihin bago kailanganin ng kamatis ang mas maraming espasyo.',
            carrotsTags: ['Pagpapabuti ng Lupa', 'Maagang Pag-ani', 'Pagkakaiba ng Ugat'],
            lettuceName: 'Litsugas',
            lettuceDesc: 'Takip sa lupa na nakababawas ng damo, pananim sa malamig na panahon. Itanim sa pagitan ng mga hanay ng kamatis.',
            lettuceTags: ['Takip sa Lupa', 'Mabilis Lumaki', 'Kontrol sa Damo'],
            onionsName: 'Sibuyas',
            onionsDesc: 'Natural na panlaban sa peste, may ibang pangangailangan sa sustansya. Inirerekomenda ang pagtatanim sa gilid.',
            onionsTags: ['Panlaban sa Peste', 'Benepisyo ng Sulfur', 'Hangganang Pananim'],
            
            // Bad neighbors
            potatoesName: 'Patatas',
            potatoesDesc: 'Parehong nightshade, kaya nagkakumpitensya sa sustansya. Pareho ring madaling tamaan ng parehong peste at sakit.',
            potatoesTags: ['Panganib ng Sakit', 'Kumpetisyon sa Sustansya', 'Parehong Pamilya'],
            cornName: 'Mais',
            cornDesc: 'Mabigat sa nitrogen, nagbibigay ng sobrang lilim. Umaakit din ng tomato fruitworm.',
            cornTags: ['Malakas Gumamit ng Sustansya', 'Problema sa Lilim', 'Umaakit ng Peste'],

            // After harvest
            leafyGreensName: 'Dahon na Gulay (Pechay, Kangkong)',
            leafyGreensDesc: 'Epektibong gagamitin ang natitirang nitrogen. Mabilis anihin sa loob ng 30-45 araw.',
            leafyGreensTags: ['Paggamit ng Nitrogen', 'Mabilis Lumaki', 'Pang-Malamig na Panahon'],
            beansName: 'Munggo / Beans',
            beansDesc: 'Legume na nagbabalik ng nitrogen sa lupa. Pinapabuti ang lupa para sa susunod na pagtatanim.',
            beansTags: ['Pagpapanumbalik ng Lupa', 'Nag-aayos ng Nitrogen', 'Pananim na May Protina'],
        }
    };

    // Farms data
    const farms = [
        {
            id: 'kalinawan',
            name: 'Kalinawan Farm',
            icon: '🌾',
            get details() { return translations[language].kalinawanDetails; },
            get location() { return translations[language].kalinawanLocation; }
        },
        {
            id: 'malasag',
            name: 'Malasag Organic Farm',
            icon: '🥕',
            get details() { return translations[language].malasagDetails; },
            get location() { return translations[language].malasagLocation; }
        }
    ];

    // Crops data
    const crops = [
        {
            id: 'tomato',
            name: 'Tomato',
            icon: '🍅',
            category: 'Fruit Vegetable',
            get description() { return translations[language].tomatoDescription; },
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
            get description() { return translations[language].eggplantDescription; },
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
            get description() { return translations[language].okraDescription; },
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
        alert(translations[language].multiCropSuccess);
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
                <p class="text-sm font-semibold text-white">{translations[language].welcome}</p>
            </div>
        </div>
        <button
            type="button"
            onclick={handleLogout}
            class="ml-10 bg-white/20 text-white font-semibold px-4 py-2 rounded-lg hover:bg-white/30 cursor-pointer">
            {translations[language].logout}</button>
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
            <p class="text-sm">{translations[language].history}</p>
        </button>
    </div>

    <div class="flex flex-col gap-4 p-6 bg-white/50 rounded-xl shadow-2xl w-full">
        <div class="flex items-center gap-2">
            <MapPin size="30" color="#84c059" />
            <p class="font-bold text-lg">{translations[language].myFarms}</p>
        </div>

        <div class="flex flex-col gap-4">
            <p>{translations[language].selectFarmDetails}</p>

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
            <p class="text-lg font-semibold text-white">{translations[language].selectFarmButton}</p>
        </button>
    {/if}

    {#if showSoilWeather}
        <hr class="mt-4 w-full h-[2px] border-0 bg-gradient-to-r from-transparent via-[#e8e3d9] to-transparent" />

        <!-- soil health -->
        <div class="flex flex-col gap-4 p-6 bg-white/50 rounded-xl shadow-2xl w-full">
            <div class="flex items-center gap-2">
                <ChartLine size="30" color="#84c059" />
                <p class="font-bold text-lg">{translations[language].soilHealth}</p>
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

                {@render healthCard('Nitrogen', '𝐍', 75, translations[language].good, 'bg-green-100 text-green-700', '75', '%', '100')}
                {@render healthCard('Phosphorus', '𝐏', 65, translations[language].moderate, 'bg-yellow-100 text-yellow-700', '65', '%', '100')}
                {@render healthCard('Potassium', '𝐊', 85, translations[language].excellent, 'bg-emerald-100 text-emerald-700', '85', '%', '100')}
                {@render healthCard('Salinity', '🧂', 18, translations[language].low, 'bg-blue-100 text-blue-700', '1.2', ' dS/m', '6.5')}
                {@render healthCard('Acidity', '🧪', 69, translations[language].optimal, 'bg-green-100 text-green-700', '6.8', ' pH', '10')}
                {@render healthCard('Moisture', '💧', 80, translations[language].good, 'bg-cyan-100 text-cyan-700', '80', '%', '100')}
                {@render healthCard('Organic Matter', '🍂', 30, translations[language].low, 'bg-orange-100 text-orange-700', '3.2', '%', '10')}

            </div>

            <!-- Overall Assessment -->
            <div class="flex flex-col gap-3 px-6 py-6 rounded-xl bg-gradient-to-r from-[#f3eee6] to-[#e8f5e8] border-l-4 border-[#84c059]">
                <div class="flex items-center gap-2">
                    <span class="text-2xl">💡</span>
                    <p class="font-semibold text-lg">{translations[language].summary}</p>
                </div>
                <p class="text-sm leading-relaxed">{translations[language].lowSalinity}</p>
            </div>
        </div>

        <div class="flex flex-col gap-4 p-6 bg-white/70 backdrop-blur-md rounded-2xl shadow-xl w-full">
            <!-- Header -->
            <div class="flex items-center gap-2">
                <Umbrella size="28" color="#84c059" />
                <p class="font-bold text-xl">{translations[language].weatherForecast}</p>
            </div>

            <p class="text-sm font-medium">{translations[language].todayIs}</p>

            <!-- Today's Weather -->
            <div class="flex flex-col items-center p-6 rounded-2xl bg-gradient-to-br bg-[#f3eee6] gap-3">
                <span class="text-6xl mb-2">⛅</span>
                <div class="text-center">
                    <div class="flex items-center justify-center gap-4 text-lg font-semibold">
                        <div class="flex items-center gap-1">
                            <span class="text-sm">☀️</span>
                            <span>32°C</span>
                            <span class="text-xs text-gray-600 font-medium">{translations[language].day}</span>
                        </div>
                        <span class="text-gray-400">/</span>
                        <div class="flex items-center gap-1">
                            <span class="text-sm">🌙</span>
                            <span>24°C</span>
                            <span class="text-xs text-gray-600 font-medium">{translations[language].night}</span>
                        </div>
                    </div>
                    <p class="text-sm text-gray-600 font-medium mt-1">{translations[language].partlyCloudy}</p>
                </div>

                <div class="grid grid-cols-3 gap-2 mt-2 w-full">
                    <div class="flex flex-col items-center p-4 bg-white/50 rounded-lg">
                        <Droplet class="w-6 h-6 mb-1 text-blue-400" />
                        <span class="text-xs font-medium text-gray-700">{translations[language].humidity}</span>
                        <span class="text-sm font-semibold">68%</span>
                    </div>
                    <div class="flex flex-col items-center p-4 bg-white/50 rounded-lg">
                        <CloudRain class="w-6 h-6 mb-1 text-blue-500" />
                        <span class="text-xs font-medium text-gray-700">{translations[language].precipitation}</span>
                        <span class="text-sm font-semibold">0 mm</span>
                    </div>
                    <div class="flex flex-col items-center p-4 bg-white/50 rounded-lg">
                        <Wind class="w-6 h-6 mb-1 text-gray-600 dark:text-gray-500" />
                        <span class="text-xs font-medium text-gray-700">{translations[language].windSpeed}</span>
                        <span class="text-sm font-semibold">12 km/h SW</span>
                    </div>
                </div>
            </div>

            <!-- Next days -->
            <div class="grid grid-cols-2 gap-3 mt-2">
                <div class="p-4 bg-[#f3eee6] rounded-xl flex items-center gap-3">
                    <span class="text-3xl">☀️</span>
                    <div class="flex flex-col">
                        <p class="font-semibold text-sm">{translations[language].tuesday}</p>
                        <div class="flex items-center gap-2 text-xs">
                            <span class="flex items-center gap-1">☀️ 33°C</span>
                            <span>/</span>
                            <span class="flex items-center gap-1">🌙 25°C</span>
                        </div>
                        <p class="text-xs text-gray-600">{translations[language].sunny}</p>
                    </div>
                </div>
                <div class="p-4 bg-[#f3eee6] rounded-xl flex items-center gap-3">
                    <span class="text-3xl">🌦️</span>
                    <div class="flex flex-col">
                        <p class="font-semibold text-sm">{translations[language].wednesday}</p>
                        <div class="flex items-center gap-2 text-xs">
                            <span class="flex items-center gap-1">☀️ 30°C</span>
                            <span>/</span>
                            <span class="flex items-center gap-1">🌙 23°C</span>
                        </div>
                        <p class="text-xs text-gray-600">{translations[language].showers}</p>
                    </div>
                </div>
                <div class="p-4 bg-[#f3eee6] rounded-xl flex items-center gap-3">
                    <span class="text-3xl">⛅</span>
                    <div class="flex flex-col">
                        <p class="font-semibold text-sm">{translations[language].thursday}</p>
                        <div class="flex items-center gap-2 text-xs">
                            <span class="flex items-center gap-1">☀️ 31°C</span>
                            <span>/</span>
                            <span class="flex items-center gap-1">� 24°C</span>
                        </div>
                        <p class="text-xs text-gray-600">{translations[language].cloudy}</p>
                    </div>
                </div>
                <div class="p-4 bg-[#f3eee6] rounded-xl flex items-center gap-3">
                    <span class="text-3xl">🌧️</span>
                    <div class="flex flex-col">
                        <p class="font-semibold text-sm">{translations[language].friday}</p>
                        <div class="flex items-center gap-2 text-xs">
                            <span class="flex items-center gap-1">☀️ 29°C</span>
                            <span>/</span>
                            <span class="flex items-center gap-1">🌙 22°C</span>
                        </div>
                        <p class="text-xs text-gray-600">{translations[language].heavyRain}</p>
                    </div>
                </div>
            </div>

            <!-- weather summary -->
            <div class="flex flex-col gap-3 px-6 py-6 rounded-xl bg-gradient-to-r from-[#f3eee6] to-[#e8f5e8] border-l-4 border-[#84c059]">
                <div class="flex items-center gap-2">
                    <span class="text-2xl">💡</span>
                    <p class="font-semibold text-lg">{translations[language].summary}</p>
                </div>
                <p class="text-sm leading-relaxed">{translations[language].southwestMonsoon}</p>
            </div>
        </div>

        {#if !showCrops}
            <button
                onclick={generateAIPlan}
                class="flex items-center justify-center p-4 w-full bg-[#84c059] rounded-3xl shadow-2xl gap-2 cursor-pointer hover:opacity-90">
                <BrainCircuit size="28" color="#ffffff" />
                <p class="text-lg font-semibold text-white">{translations[language].generateAIPlan}</p>
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
                <p class="font-bold text-xl">{translations[language].recommendedCrops}</p>
            </div>

            <p>{translations[language].selectStartingCrop}</p>

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
                <p class="text-lg font-semibold text-white">{translations[language].selectCropButton}</p>
            </button>
        {/if}
    {/if}

    {#if showSuggestions}
        <hr class="mt-4 w-full h-[2px] border-0 bg-gradient-to-r from-transparent via-[#e8e3d9] to-transparent" />

        <div class="flex flex-col gap-6 p-6 bg-white/70 backdrop-blur-md rounded-2xl shadow-xl w-full max-w-4xl mx-auto">
            <!-- Header -->
            <div class="flex items-center gap-3">
                <Clover size="28" color="#84c059" />
                <h2 class="font-bold text-xl">{translations[language].multiCroppingSuggestions}</h2>
            </div>

            <!-- Good Companions Section -->
            <div class="flex flex-col gap-4">
                <h3 class="font-semibold text-lg text-gray-700 flex items-center gap-2">
                🤝 <span>{translations[language].goodCompanions}</span>
                </h3>

                <div class="grid gap-4">
                <!-- Basil -->
                <div class="flex items-center gap-4 p-4 bg-green-50/80 rounded-xl border-2 border-green-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🌿</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">{translations[language].basilName}</p>
                    <p class="text-gray-600 text-sm mb-2">{translations[language].basilDesc}</p>
                    <div class="flex flex-wrap gap-2">
                        {#each translations[language].basilTags as tag}
                        <span class="px-2 py-1 bg-green-100 text-green-700 rounded-lg text-xs">{tag}</span>
                        {/each}
                    </div>
                    </div>
                </div>

                <!-- Carrots -->
                <div class="flex items-center gap-4 p-4 bg-orange-50/80 rounded-xl border-2 border-orange-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🥕</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">{translations[language].carrotsName}</p>
                    <p class="text-gray-600 text-sm mb-2">{translations[language].carrotsDesc}</p>
                    <div class="flex flex-wrap gap-2">
                        {#each translations[language].carrotsTags as tag}
                        <span class="px-2 py-1 bg-orange-100 text-orange-700 rounded-lg text-xs">{tag}</span>
                        {/each}
                    </div>
                    </div>
                </div>

                <!-- Lettuce -->
                <div class="flex items-center gap-4 p-4 bg-emerald-50/80 rounded-xl border-2 border-emerald-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🌱</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">{translations[language].lettuceName}</p>
                    <p class="text-gray-600 text-sm mb-2">{translations[language].lettuceDesc}</p>
                    <div class="flex flex-wrap gap-2">
                        {#each translations[language].lettuceTags as tag}
                        <span class="px-2 py-1 bg-emerald-100 text-emerald-700 rounded-lg text-xs">{tag}</span>
                        {/each}
                    </div>
                    </div>
                </div>

                <!-- Onions -->
                <div class="flex items-center gap-4 p-4 bg-purple-50/80 rounded-xl border-2 border-purple-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🧅</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">{translations[language].onionsName}</p>
                    <p class="text-gray-600 text-sm mb-2">{translations[language].onionsDesc}</p>
                    <div class="flex flex-wrap gap-2">
                        {#each translations[language].onionsTags as tag}
                        <span class="px-2 py-1 bg-purple-100 text-purple-700 rounded-lg text-xs">{tag}</span>
                        {/each}
                    </div>
                    </div>
                </div>
                </div>
            </div>

            <!-- Bad Neighbors Section -->
            <div class="flex flex-col gap-4">
                <h3 class="font-semibold text-lg text-gray-700 flex items-center gap-2">
                ❌ <span>{translations[language].badNeighbors}</span>
                </h3>

                <div class="grid gap-4">
                <!-- Potatoes -->
                <div class="flex items-center gap-4 p-4 bg-red-50/80 rounded-xl border-2 border-red-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🥔</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">{translations[language].potatoesName}</p>
                    <p class="text-gray-600 text-sm mb-2">{translations[language].potatoesDesc}</p>
                    <div class="flex flex-wrap gap-2">
                        {#each translations[language].potatoesTags as tag}
                        <span class="px-2 py-1 bg-red-100 text-red-700 rounded-lg text-xs">{tag}</span>
                        {/each}
                    </div>
                    </div>
                </div>

                <!-- Corn -->
                <div class="flex items-center gap-4 p-4 bg-yellow-50/80 rounded-xl border-2 border-yellow-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🌽</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">{translations[language].cornName}</p>
                    <p class="text-gray-600 text-sm mb-2">{translations[language].cornDesc}</p>
                    <div class="flex flex-wrap gap-2">
                        {#each translations[language].cornTags as tag}
                        <span class="px-2 py-1 bg-yellow-100 text-yellow-700 rounded-lg text-xs">{tag}</span>
                        {/each}
                    </div>
                    </div>
                </div>
                </div>
            </div>

            <!-- After Harvest Section -->
            <div class="flex flex-col gap-4">
                <h3 class="font-semibold text-lg text-gray-700 flex items-center gap-2">
                🔄 <span>{translations[language].afterHarvest}</span>
                </h3>
                <p class="text-gray-600 text-sm">{translations[language].afterHarvestDesc}</p>

                <div class="grid gap-4">
                <!-- Leafy Greens -->
                <div class="flex items-center gap-4 p-4 bg-teal-50/80 rounded-xl border-2 border-teal-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🥬</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">{translations[language].leafyGreensName}</p>
                    <p class="text-gray-600 text-sm mb-2">{translations[language].leafyGreensDesc}</p>
                    <div class="flex flex-wrap gap-2">
                        {#each translations[language].leafyGreensTags as tag}
                        <span class="px-2 py-1 bg-teal-100 text-teal-700 rounded-lg text-xs">{tag}</span>
                        {/each}
                    </div>
                    </div>
                </div>

                <!-- Beans -->
                <div class="flex items-center gap-4 p-4 bg-lime-50/80 rounded-xl border-2 border-lime-400/50 backdrop-blur-sm">
                    <span class="text-5xl">🫘</span>
                    <div class="flex-1">
                    <p class="font-semibold text-gray-800 mb-1">{translations[language].beansName}</p>
                    <p class="text-gray-600 text-sm mb-2">{translations[language].beansDesc}</p>
                    <div class="flex flex-wrap gap-2">
                        {#each translations[language].beansTags as tag}
                        <span class="px-2 py-1 bg-lime-100 text-lime-700 rounded-lg text-xs">{tag}</span>
                        {/each}
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
        <h2 class="font-bold text-xl">{translations[language].multiCropTimeline}</h2>
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
                <h3 class="font-bold text-lg text-gray-800">{translations[language].landPreparation}</h3>
              </div>
              <div class="mb-4">
                <h4 class="font-semibold text-gray-700 mb-2">🚜 {translations[language].soilPrepDetails}</h4>
                <p class="text-gray-600 text-sm mb-3">{translations[language].soilPrepDetails}</p>
                <div class="flex flex-wrap gap-2 mb-3">
                  <span class="px-3 py-1 bg-green-200 text-green-800 rounded-lg text-sm font-medium">{translations[language].plantCarrotTomato}</span>
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
                <h3 class="font-bold text-lg text-gray-800">{translations[language].transplanting}</h3>
              </div>
              <div class="mb-4">
                <h4 class="font-semibold text-gray-700 mb-2">🌱 {translations[language].transplantTomatoesBasil}</h4>
                <p class="text-gray-600 text-sm mb-3">{translations[language].transplantTomatoesBasil}</p>
                <div class="bg-blue-100/60 rounded-lg p-3">
                  <span class="text-sm font-medium text-blue-800">{translations[language].careDailyWatering}</span>
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
                <h3 class="font-bold text-lg text-gray-800">{translations[language].firstSideDress}</h3>
              </div>
              <div class="mb-4">
                <h4 class="font-semibold text-gray-700 mb-2">🌿 {translations[language].sideDressingMaintenance}</h4>
                <p class="text-gray-600 text-sm mb-3">{translations[language].sideDressingMaintenance}</p>
                <div class="bg-purple-100/60 rounded-lg p-3">
                  <span class="text-sm font-medium text-purple-800">{translations[language].pruneTomatoSuckers}</span>
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
                <h3 class="font-bold text-lg text-gray-800">{translations[language].floweringStage}</h3>
              </div>
              <div class="mb-4">
                <h4 class="font-semibold text-gray-700 mb-2">🌸 {translations[language].floweringSecondFertilizer}</h4>
                <p class="text-gray-600 text-sm mb-3">{translations[language].floweringSecondFertilizer}</p>
                <div class="bg-pink-100/60 rounded-lg p-3">
                  <span class="text-sm font-medium text-pink-800">{translations[language].pruneTomatoSuckers}</span>
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
                <h3 class="font-bold text-lg text-gray-800">{translations[language].mainHarvestPeriod}</h3>
              </div>
              <div class="mb-4">
                <h4 class="font-semibold text-gray-700 mb-2">🍅 {translations[language].peakTomatoHarvest}</h4>
                <p class="text-gray-600 text-sm mb-3">{translations[language].peakTomatoHarvest}</p>
                <div class="bg-orange-100/60 rounded-lg p-3">
                  <span class="text-sm font-medium text-orange-800">{translations[language].harvestContinuous}</span>
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
                <h3 class="font-bold text-lg text-gray-800">{translations[language].successionPlanting}</h3>
              </div>
              <div class="mb-4">
                <h4 class="font-semibold text-gray-700 mb-2">🔄 {translations[language].endCyclePlantNext}</h4>
                <p class="text-gray-600 text-sm mb-3">{translations[language].endCyclePlantNext}</p>
                <div class="bg-teal-100/60 rounded-lg p-3">
                  <span class="text-sm font-medium text-teal-800">{translations[language].prepSoilTest}</span>
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
            <p class="text-lg font-semibold text-white">{translations[language].startFarming}</p>
        </button>
    {/if}

</div>
