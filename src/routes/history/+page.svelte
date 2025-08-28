<script>
    import { Sprout, ArrowLeft, Calendar, MapPin, Leaf, TrendingUp, Clock, CheckCircle2, AlertTriangle, Eye } from '@lucide/svelte';
    import { goto } from "$app/navigation";

    // Sample history data
    const farmingHistory = [
        {
            id: 1,
            date: '2025-08-15',
            farmName: 'Kalinawan Farm',
            farmIcon: '🌾',
            crop: 'Tomato',
            cropIcon: '🍅',
            status: 'completed',
            duration: '85 days',
            yield: '2.3 kg/plant',
            revenue: '₱45,500',
            companions: ['Basil', 'Carrots', 'Lettuce'],
            notes: 'Excellent harvest despite heavy rains in week 16. Disease management was effective.'
        },
        {
            id: 2,
            date: '2025-07-01',
            farmName: 'Malasag Organic Farm',
            farmIcon: '🥕',
            crop: 'Eggplant',
            cropIcon: '🍆',
            status: 'in-progress',
            duration: '45/80 days',
            expectedYield: '1.8 kg/plant',
            expectedRevenue: '₱32,000',
            companions: ['Onions', 'Beans'],
            notes: 'Currently in flowering stage. Weather conditions favorable.'
        },
        {
            id: 3,
            date: '2025-06-10',
            farmName: 'Kalinawan Farm',
            farmIcon: '🌾',
            crop: 'Okra',
            cropIcon: '🌱',
            status: 'completed',
            duration: '62 days',
            yield: '1.1 kg/plant',
            revenue: '₱28,750',
            companions: ['Lettuce', 'Radish'],
            notes: 'Good drought resistance. Early harvest due to market demand.'
        },
        {
            id: 4,
            date: '2025-05-20',
            farmName: 'Malasag Organic Farm',
            farmIcon: '🥕',
            crop: 'Tomato',
            cropIcon: '🍅',
            status: 'failed',
            duration: '40 days (terminated)',
            loss: '₱15,200',
            companions: ['Basil'],
            notes: 'Bacterial wilt outbreak. Soil treatment needed before next planting.'
        }
    ];

    function goBack() {
        goto('/farmer'); // or wherever the main dashboard is
    }

    function getStatusColor(status) {
        switch(status) {
            case 'completed': return 'bg-green-100 text-green-700';
            case 'in-progress': return 'bg-blue-100 text-blue-700';
            case 'failed': return 'bg-red-100 text-red-700';
            default: return 'bg-gray-100 text-gray-700';
        }
    }

    function getStatusIcon(status) {
        switch(status) {
            case 'completed': return CheckCircle2;
            case 'in-progress': return Clock;
            case 'failed': return AlertTriangle;
            default: return Clock;
        }
    }

    function formatDate(dateString) {
        const date = new Date(dateString);
        return date.toLocaleDateString('en-US', { 
            year: 'numeric', 
            month: 'short', 
            day: 'numeric' 
        });
    }

    function viewDetails(planId) {
        // Navigate to detailed view
        // goto(`/history/${planId}`);
        alert('Fuctionality for this feature is not yet implemented.');
    }
</script>

<div class="max-w-lg m-auto flex flex-col gap-6 items-center justify-center py-8">

    <!-- Header -->
    <div class="flex items-center bg-[#84c059] rounded-xl shadow-2xl justify-between py-6 px-6 w-full">
        <div class="flex items-center gap-4">
            <button 
                onclick={goBack}
                class="bg-white/10 rounded-full p-2 hover:bg-white/20 transition-colors">
                <ArrowLeft size="24" color="#ffffff" />
            </button>
            <div class="flex items-center gap-3">
                <div class="bg-white/10 rounded-full p-2">
                    <Sprout size="40" color="#ffffff" />
                </div>
                <div>
                    <p class="text-2xl font-bold text-white">Farming History</p>
                    <p class="text-sm font-medium text-white/90">Track your agricultural journey</p>
                </div>
            </div>
        </div>
    </div>

    <!-- Summary Stats -->
    <div class="grid grid-cols-2 gap-4 w-full">
        <div class="flex flex-col items-center p-4 bg-white/70 backdrop-blur-md rounded-xl shadow-lg">
            <div class="flex items-center gap-2 mb-2">
                <TrendingUp size="24" color="#84c059" />
                <span class="text-lg font-bold text-gray-800">Success Rate</span>
            </div>
            <p class="text-2xl font-bold text-[#84c059]">75%</p>
            <p class="text-xs text-gray-600">3 of 4 completed</p>
        </div>
        
        <div class="flex flex-col items-center p-4 bg-white/70 backdrop-blur-md rounded-xl shadow-lg">
            <div class="flex items-center gap-2 mb-2">
                <Calendar size="24" color="#84c059" />
                <span class="text-lg font-bold text-gray-800">Total Plans</span>
            </div>
            <p class="text-2xl font-bold text-[#84c059]">{farmingHistory.length}</p>
            <p class="text-xs text-gray-600">Since May 2025</p>
        </div>
    </div>

    <!-- History List -->
    <div class="flex flex-col gap-4 w-full">
        <h2 class="text-lg font-bold text-gray-800 flex items-center gap-2">
            <Clock size="20" color="#84c059" />
            Recent Activities
        </h2>

        {#each farmingHistory as plan}
            <div class="flex flex-col gap-4 p-5 bg-white/70 backdrop-blur-md rounded-xl shadow-lg border border-gray-200">
                
                <!-- Header Row -->
                <div class="flex items-center justify-between">
                    <div class="flex items-center gap-3">
                        <span class="text-3xl">{plan.cropIcon}</span>
                        <div>
                            <p class="font-bold text-gray-800">{plan.crop} Cultivation</p>
                            <div class="flex items-center gap-2 text-sm text-gray-600">
                                <MapPin size="14" />
                                <span>{plan.farmName}</span>
                                <span class="text-gray-400">•</span>
                                <span>{formatDate(plan.date)}</span>
                            </div>
                        </div>
                    </div>
                    
                    <!-- Status Badge -->
                    <div class="flex items-center gap-2">
                        <span class="px-3 py-1 rounded-full text-xs font-medium {getStatusColor(plan.status)} flex items-center gap-1">
                            <svelte:component this={getStatusIcon(plan.status)} size="12" />
                            {plan.status === 'in-progress' ? 'Active' : plan.status.charAt(0).toUpperCase() + plan.status.slice(1)}
                        </span>
                    </div>
                </div>

                <!-- Metrics Row -->
                <div class="grid grid-cols-2 gap-4">
                    <div class="flex flex-col">
                        <span class="text-xs font-medium text-gray-500 uppercase">Duration</span>
                        <span class="text-sm font-semibold text-gray-800">{plan.duration}</span>
                    </div>
                    <div class="flex flex-col">
                        <span class="text-xs font-medium text-gray-500 uppercase">
                            {plan.status === 'failed' ? 'Loss' : plan.status === 'completed' ? 'Revenue' : 'Expected Revenue'}
                        </span>
                        <span class="text-sm font-semibold {plan.status === 'failed' ? 'text-red-600' : 'text-green-600'}">
                            {plan.revenue || plan.expectedRevenue || plan.loss}
                        </span>
                    </div>
                </div>

                {#if plan.yield || plan.expectedYield}
                    <div class="flex flex-col">
                        <span class="text-xs font-medium text-gray-500 uppercase">
                            {plan.status === 'completed' ? 'Yield Achieved' : 'Expected Yield'}
                        </span>
                        <span class="text-sm font-semibold text-gray-800">{plan.yield || plan.expectedYield}</span>
                    </div>
                {/if}

                <!-- Companions -->
                <div class="flex flex-col gap-2">
                    <span class="text-xs font-medium text-gray-500 uppercase">Companion Crops</span>
                    <div class="flex flex-wrap gap-1">
                        {#each plan.companions as companion}
                            <span class="px-2 py-1 bg-green-50 text-green-700 rounded-lg text-xs font-medium">
                                {companion}
                            </span>
                        {/each}
                    </div>
                </div>

                <!-- Notes -->
                <div class="flex flex-col gap-2">
                    <span class="text-xs font-medium text-gray-500 uppercase">Notes</span>
                    <p class="text-sm text-gray-700 leading-relaxed">{plan.notes}</p>
                </div>

                <!-- Action Button -->
                <button 
                    onclick={() => viewDetails(plan.id)}
                    class="flex items-center justify-center gap-2 py-2 px-4 bg-[#84c059] hover:bg-[#6fa045] text-white rounded-lg font-medium text-sm transition-colors">
                    <Eye size="16" />
                    View Details
                </button>
            </div>
        {/each}
    </div>

    <!-- Footer -->
    <div class="flex flex-col items-center gap-3 px-6 py-4 rounded-xl bg-gradient-to-r from-[#f3eee6] to-[#e8f5e8] border-l-4 border-[#84c059] w-full">
        <div class="flex items-center gap-2">
            <span class="text-xl">📊</span>
            <p class="font-semibold text-base text-gray-800">Agricultural Insights</p>
        </div>
        <p class="text-sm leading-relaxed text-center text-gray-700">
            Your farming performance shows consistent improvement. Consider rotating to nitrogen-fixing crops next season for soil health.
        </p>
    </div>

</div>