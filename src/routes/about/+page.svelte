<script>
    // @ts-nocheck
    import { fade, fly } from "svelte/transition";
    import { quintOut } from "svelte/easing";

    function gotoSocialMedia(socialmedia) {
        if(socialmedia == 'insta'){   
            window.open('https://www.instagram.com/c_k_viki/', '_blank');
        } else if (socialmedia == 'twitter') {
            // window.open('https://www.instagram.com/c_k_viki/', '_blank');
        } else if (socialmedia == 'whatsup') {
            window.open('https://wa.me/9629846371?text=Hi', '_blank');
        } else if (socialmedia == 'linkedIn') {
            window.open('https://www.linkedin.com/in/vignesh-kumar-c-02915b203', '_blank');
        }
    }
</script>

<div class="container mx-auto px-4 py-10 min-h-screen">
    <!-- Header -->
    <div class="mb-12 text-center md:text-left">
        <h1 class="text-4xl md:text-5xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-blue-400 to-purple-600 inline-block">
            About Me
        </h1>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-stretch">
        <!-- Text Content -->
        <div 
            in:fly={{ x: -20, duration: 800, delay: 200, easing: quintOut }}
            class="flex flex-col"
        >
            <div class="bg-gray-900/50 backdrop-blur-sm border border-gray-800 rounded-3xl p-8 lg:p-12 hover:border-blue-500/30 transition-all duration-500 shadow-xl shadow-black/20 h-full flex flex-col justify-center">
                <h2 class="text-3xl font-bold text-white mb-6">So, Who am I?</h2>
                <p class="text-gray-300 text-lg leading-relaxed mb-8">
                    As a web developer and designer who lives for the freedom of the online world, I extend that
                    freedom onto the roads, seeking new inspirations and perspectives on my trusty motorcycle.
                </p>

                <!-- Social Media Desktop & Mobile -->
                <div class="flex flex-wrap items-center gap-4 md:gap-6 pt-4">
                    <button 
                        on:click={()=>gotoSocialMedia('linkedIn')} 
                        class="p-3 bg-gray-800/50 rounded-xl hover:bg-blue-500/20 hover:scale-110 transition-all duration-300 border border-transparent hover:border-blue-500/50 group"
                        title="LinkedIn"
                    >
                        <img class="w-7 h-7 filter brightness-90 group-hover:brightness-100" src="/socialmedia/linkedin.png" alt="LinkedIn">
                    </button>
            
                    <button 
                        on:click={()=>gotoSocialMedia('whatsup')} 
                        class="p-3 bg-gray-800/50 rounded-xl hover:bg-green-500/20 hover:scale-110 transition-all duration-300 border border-transparent hover:border-green-500/50 group"
                        title="WhatsApp"
                    >
                        <img class="w-7 h-7 filter brightness-90 group-hover:brightness-100" src="/socialmedia/whatsup.png" alt="WhatsApp">
                    </button>
            
                    <button 
                        on:click={()=>gotoSocialMedia('insta')} 
                        class="p-3 bg-gray-800/50 rounded-xl hover:bg-pink-500/20 hover:scale-110 transition-all duration-300 border border-transparent hover:border-pink-500/50 group"
                        title="Instagram"
                    >
                        <img class="w-7 h-7 filter brightness-90 group-hover:brightness-100" src="/socialmedia/insta.png" alt="Instagram">
                    </button>
            
                    <button 
                        class="p-3 bg-gray-800/50 rounded-xl hover:bg-blue-400/20 hover:scale-110 transition-all duration-300 border border-transparent hover:border-blue-400/50 group"
                        title="Telegram"
                    >
                        <img class="w-7 h-7 filter brightness-90 group-hover:brightness-100" src="/socialmedia/telegram.png" alt="Telegram">
                    </button>
                </div>
            </div>
        </div>

        <!-- Image Content -->
        <div 
            in:fly={{ x: 20, duration: 800, delay: 400, easing: quintOut }}
            class="relative group h-full"
        >
            <div class="absolute -inset-1 bg-gradient-to-r from-blue-600 to-purple-600 rounded-3xl blur opacity-25 group-hover:opacity-40 transition duration-1000 group-hover:duration-200"></div>
            <div class="relative bg-gray-900 rounded-3xl overflow-hidden shadow-2xl h-full">
                <img src="/working.png" class="w-full h-full object-cover transform hover:scale-105 transition duration-700" alt="Working">
            </div>
        </div>
    </div>
</div>

<style>
    /* Add any custom animations if needed */
</style>