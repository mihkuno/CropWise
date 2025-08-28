<script>
    import { Sprout, Eye, EyeOff } from "@lucide/svelte";
	import { goto } from "$app/navigation";

    const demo_id_farmer = "farmer";
    const demo_pw_farmer= "farmer123";
    const demo_id_admin = "admin";
    const demo_pw_admin = "admin123";

    let id_input = $state("");
    let pw_input = $state("");
    let show_pw = $state(false);

    function handleLogin() {
        if (id_input === demo_id_farmer && pw_input === demo_pw_farmer) {
            alert("Login successful!");
            goto('/farmer');
        } 
        else if (id_input === demo_id_admin && pw_input === demo_pw_admin) {
            alert("Login successful!");
            goto('/admin');
        } 
        else {
            alert("Invalid ID or password. Please try again.");
        }
    }

</script>

<div class="min-h-screen flex items-center justify-center px-4 py-6 sm:px-6 lg:px-8">
    <div class="w-full max-w-sm sm:max-w-md lg:max-w-lg rounded-xl shadow-2xl backdrop-blur-sm overflow-hidden">

        <!-- Header Section -->
        <div class="flex flex-col sm:flex-row items-center justify-center bg-[#84c059] p-4 sm:p-6">
            <div class="bg-white/10 rounded-full mb-2 sm:mb-0 sm:mr-4">
                <Sprout size="40" color="#ffffff" class="sm:w-[60px] sm:h-[60px]" />
            </div>
            <div class="text-center sm:text-left">
                <p class="text-2xl sm:text-3xl font-bold text-white">CropWise</p>
                <p class="text-xs sm:text-sm font-semibold text-white">Multi-Cropping Assistant</p>
            </div>
        </div>

        <!-- Form Section -->
        <form class="p-4 sm:p-6 lg:p-8 flex flex-col gap-6 sm:gap-8 bg-white/50">
            
            <!-- Welcome Text -->
            <div class="flex flex-col items-center justify-center text-center">
                <p class="text-lg sm:text-xl font-bold text-[#2e251f]">Welcome to CropWise</p>
                <p class="text-sm sm:text-md font-semibold text-[#847062]">Your partner for sustainable multicropping</p>    
            </div>

            <!-- Input Fields -->
            <div class="flex flex-col gap-4">
                <!-- User ID Input -->
                <div class="flex flex-col gap-2">
                    <label for="id" class="text-[#2e251f] text-sm sm:text-md font-bold">User ID</label>
                    <div class="border-2 border-[#e8e3d9] px-3 sm:px-5 py-2 sm:py-3 rounded-lg focus-within:border-[#84c059]">
                        <input 
                            type="text" 
                            placeholder="Enter your identification" 
                            bind:value={id_input}
                            class="w-full placeholder:text-[#847062] placeholder:text-sm sm:placeholder:text-md placeholder:font-medium focus:outline-none bg-transparent"/>
                    </div>
                </div>
                
                <!-- Password Input -->
                <div class="flex flex-col gap-2">
                    <label for="password" class="text-[#2e251f] text-sm sm:text-md font-bold">Password</label>
                    <div class="flex justify-between items-center border-2 border-[#e8e3d9] px-3 sm:px-5 py-2 sm:py-3 rounded-lg focus-within:border-[#84c059]">
                        <input 
                            type={show_pw ? "text" : "password"} 
                            placeholder="Enter your password" 
                            bind:value={pw_input}
                            class="flex-1 placeholder:text-[#847062] placeholder:text-sm sm:placeholder:text-md placeholder:font-medium focus:outline-none bg-transparent"/>
                            
                        {#if show_pw}
                            <Eye 
                                class="text-[#847062] cursor-pointer hover:opacity-80 ml-2 flex-shrink-0" 
                                size="18"
                                onclick={() => show_pw = false}/>
                        {:else}
                            <EyeOff
                                class="text-[#847062] cursor-pointer hover:opacity-80 ml-2 flex-shrink-0" 
                                size="18"
                                onclick={() => show_pw = true}/>
                        {/if}
                    </div>
                </div>
            </div>

            <!-- Login Button -->
            <button 
                type="button"
                onclick={handleLogin}
                class="w-full bg-[#84c059] p-3 sm:p-3 text-sm sm:text-base font-bold text-white rounded-lg cursor-pointer hover:opacity-90 transition-opacity">
                Continue
            </button>

        </form>
    </div>
</div>