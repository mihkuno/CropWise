

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

<div class="max-w-lg m-auto h-screen flex items-center justify-center">
    <div class="w-full rounded-xl shadow-2xl backdrop-blur-sm overflow-hidden">

        <div class="flex items-center justify-center bg-[#84c059]">
            <div class="bg-white/10 rounded-full">
                <Sprout size="60" color="#ffffff" />
            </div>
            <div class="px-4 py-8">
                <p class="text-3xl font-bold text-white">CropWise</p>
                <p class="text-sm font-semibold text-white">Multi-Cropping Assistant</p>
            </div>
        </div>

        <form class="p-8 flex flex-col gap-8 bg-white/50">
            
            <div class="flex flex-col items-center justify-center text-center">
                <p class="text-xl font-bold text-[#2e251f]">Welcome to CropWise</p>
                <p class="text-md font-semibold text-[#847062]">Your partner for sustainable multicropping</p>    
            </div>

            <div class="flex flex-col gap-4">
                <div class="flex flex-col gap-2 ">
                    <label for="id" class="text-[#2e251f] text-md font-bold">User ID</label>
                    <div class="border-2 border-[#e8e3d9] px-5 py-3 rounded-lg focus-within:border-[#84c059]">
                        <input 
                            type="text" 
                            placeholder="Enter your identification" 
                            bind:value={id_input}
                            class="placeholder:text-[#847062] placeholder:text-md placeholder:font-medium focus:outline-none"/>
                    </div>
                </div>
                <div class="flex flex-col gap-2">
                    <label for="password" class="text-[#2e251f] text-md font-bold">Password</label>
                    <div class="flex justify-between items-center border-2 border-[#e8e3d9] px-5 py-3 rounded-lg focus-within:border-[#84c059]">
                        <input 
                            type={show_pw ? "text" : "password"} 
                            placeholder="Enter your password" 
                            bind:value={pw_input}
                            class="placeholder:text-[#847062] placeholder:text-md placeholder:font-medium focus:outline-none"/>
                            
                        {#if show_pw}
                            <Eye 
                                class="text-[#847062] text-md cursor-pointer hover:opacity-80" 
                                size="20"
                                onclick={() => show_pw = false}/>
                        {:else}
                            <EyeOff
                                class="text-[#847062] text-md cursor-pointer hover:opacity-80" 
                                size="20"
                                onclick={() => show_pw = true}/>
                        {/if}
                    </div>
                </div>
            </div>

            <button 
                type="button"
                onclick={handleLogin}
                class="w-full bg-[#84c059] p-3 font-bold text-white rounded-lg cursor-pointer hover:opacity-90">Continue</button>

        </form>
    </div>
</div>