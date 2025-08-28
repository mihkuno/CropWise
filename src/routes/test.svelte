<script>
    import { Sprout, User, Lock, Eye, EyeOff } from "@lucide/svelte";
    
    let userType = 'farmer'; // 'farmer' or 'admin'
    let username = '';
    let password = '';
    let showPassword = false;
    
    function handleLogin() {
        console.log('Login attempted:', { userType, username, password });
        // Add your login logic here
    }
    
    function toggleUserType() {
        userType = userType === 'farmer' ? 'admin' : 'farmer';
    }
    
    function togglePasswordVisibility() {
        showPassword = !showPassword;
    }
</script>

<div class="min-h-screen flex items-center justify-center px-4" style="background: linear-gradient(135deg, hsl(30 25% 85%), hsl(42 100% 98%));">
    <div class="max-w-md w-full bg-white/90 backdrop-blur-sm rounded-xl shadow-2xl border border-gray-200/50 overflow-hidden">
        <!-- Header with crop gradient background -->
        <div class="p-8 text-center relative overflow-hidden" style="background: linear-gradient(135deg, hsl(95 45% 55%), hsl(120 35% 25%));">
            <!-- Decorative elements -->
            <div class="absolute top-0 right-0 w-32 h-32 bg-white/10 rounded-full -mr-16 -mt-16"></div>
            <div class="absolute bottom-0 left-0 w-24 h-24 bg-white/10 rounded-full -ml-12 -mb-12"></div>
            
            <div class="relative z-10">
                <div class="flex items-center justify-center mb-4">
                    <div class="bg-white/20 rounded-full p-3 mr-3">
                        <Sprout size="32" color="white" />
                    </div>
                    <div class="text-left">
                        <h1 class="text-3xl font-bold text-white">CropWise</h1>
                        <p class="text-white/90 text-sm font-medium">Multi-Cropping Assistant</p>
                    </div>
                </div>
                <p class="text-white/80 text-sm">Your intelligent partner for sustainable agriculture</p>
            </div>
        </div>

        <!-- Form content -->
        <div class="p-8">
            <!-- User type toggle -->
            <div class="mb-6">
                <div class="relative rounded-full p-1 flex" style="background-color: hsl(42 20% 92%);">
                    <button
                        on:click={toggleUserType}
                        class="flex-1 py-2 px-4 rounded-full text-sm font-medium transition-all duration-300 {userType === 'farmer' ? 'text-white shadow-lg' : 'hover:text-gray-800'}"
                        style={userType === 'farmer' ? 'background: linear-gradient(135deg, hsl(95 45% 55%), hsl(120 35% 25%));' : 'color: hsl(25 15% 45%);'}
                    >
                        Farmer
                    </button>
                    <button
                        on:click={toggleUserType}
                        class="flex-1 py-2 px-4 rounded-full text-sm font-medium transition-all duration-300 {userType === 'admin' ? 'text-white shadow-lg' : 'hover:text-gray-800'}"
                        style={userType === 'admin' ? 'background: linear-gradient(135deg, hsl(95 45% 55%), hsl(120 35% 25%));' : 'color: hsl(25 15% 45%);'}
                    >
                        Admin
                    </button>
                </div>
            </div>

            <!-- Username input -->
            <div class="mb-4">
                <label for="username" class="block text-sm font-medium mb-2" style="color: hsl(25 20% 15%);">
                    {userType === 'farmer' ? 'Farmer ID' : 'Admin Username'}
                </label>
                <div class="relative">
                    <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                        <User size="18" style="color: hsl(25 15% 45%);" />
                    </div>
                    <input
                        id="username"
                        type="text"
                        bind:value={username}
                        placeholder={userType === 'farmer' ? 'Enter your farmer ID' : 'Enter admin username'}
                        class="w-full pl-10 pr-4 py-3 border rounded-xl focus:outline-none focus:ring-2 transition-all duration-200"
                        style="border-color: hsl(42 25% 88%); background-color: hsl(0 0% 100%); color: hsl(25 20% 15%); --tw-ring-color: hsl(120 35% 25%);"
                    />
                </div>
            </div>

            <!-- Password input -->
            <div class="mb-6">
                <label for="password" class="block text-sm font-medium mb-2" style="color: hsl(25 20% 15%);">Password</label>
                <div class="relative">
                    <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
                        <Lock size="18" style="color: hsl(25 15% 45%);" />
                    </div>
                    <input
                        id="password"
                        type={showPassword ? 'text' : 'password'}
                        bind:value={password}
                        placeholder="Enter your password"
                        class="w-full pl-10 pr-12 py-3 border rounded-xl focus:outline-none focus:ring-2 transition-all duration-200"
                        style="border-color: hsl(42 25% 88%); background-color: hsl(0 0% 100%); color: hsl(25 20% 15%); --tw-ring-color: hsl(120 35% 25%);"
                    />
                    <button
                        type="button"
                        on:click={togglePasswordVisibility}
                        class="absolute inset-y-0 right-0 pr-3 flex items-center hover:opacity-70 transition-opacity"
                    >
                        {#if showPassword}
                            <EyeOff size="18" style="color: hsl(25 15% 45%);" />
                        {:else}
                            <Eye size="18" style="color: hsl(25 15% 45%);" />
                        {/if}
                    </button>
                </div>
            </div>

            <!-- Login button with glow effect -->
            <button
                on:click={handleLogin}
                class="btn-glow w-full text-white font-semibold py-3 px-6 rounded-xl shadow-lg hover:shadow-xl transform hover:scale-[1.02] transition-all duration-300 active:scale-[0.98] relative overflow-hidden"
                style="background: linear-gradient(135deg, hsl(95 45% 55%), hsl(120 35% 25%));"
            >
                <span class="relative z-10">
                    Sign In as {userType === 'farmer' ? 'Farmer' : 'Administrator'}
                </span>
            </button>

            <!-- Additional options -->
            <div class="mt-6 text-center">
                <a href="#" class="text-sm font-medium hover:opacity-80 transition-opacity" style="color: hsl(95 45% 55%);">
                    Forgot your password?
                </a>
            </div>

            <div class="mt-4 text-center">
                <span class="text-sm" style="color: hsl(25 15% 45%);">New to CropWise? </span>
                <a href="#" class="text-sm font-medium hover:opacity-80 transition-opacity" style="color: hsl(95 45% 55%);">
                    Create an account
                </a>
            </div>
        </div>
    </div>
</div>

<style>
    /* Button glow effect using your color scheme */
    .btn-glow::before {
        content: '';
        position: absolute;
        inset: 0;
        background: linear-gradient(to right, 
            hsla(95, 45%, 65%, 0), 
            hsla(95, 45%, 65%, 0.2), 
            hsla(95, 45%, 65%, 0)
        );
        transform: translateX(-100%);
        transition: transform 0.7s ease;
    }

    .btn-glow:hover::before {
        transform: translateX(100%);
    }

    /* Focus ring color override */
    input:focus {
        --tw-ring-color: hsl(120 35% 25%) !important;
    }
</style>