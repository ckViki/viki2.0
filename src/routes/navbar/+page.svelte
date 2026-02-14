<script>
    // @ts-nocheck
    import { goto } from "$app/navigation";
    import { page } from "$app/stores";

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

<div class="fixed top-6 left-1/2 -translate-x-1/2 z-50 w-fit max-w-4xl transition-all duration-300">
    <div class="bg-gray-950/60 backdrop-blur-md border border-gray-800/50 rounded-full px-4 py-2 md:px-6 md:py-3 shadow-2xl flex items-center justify-center gap-4">
        
        <!-- Navigation Links -->
        <div class="flex items-center gap-1 md:gap-4 justify-center">
            <button 
                on:click={()=>goto('/')} 
                class="px-4 py-1.5 rounded-full text-sm md:text-base transition-all duration-200 {$page.url.pathname === '/' ? 'bg-gradient-to-r from-blue-600/20 to-purple-600/20 text-blue-400 border border-blue-500/30' : 'text-gray-400 hover:text-white hover:bg-white/5 border border-transparent'}"
            >
                Home
            </button>
            
            <button 
                on:click={()=>goto('/work')} 
                class="px-4 py-1.5 rounded-full text-sm md:text-base transition-all duration-200 {$page.url.pathname === '/work' ? 'bg-gradient-to-r from-blue-600/20 to-purple-600/20 text-blue-400 border border-blue-500/30' : 'text-gray-400 hover:text-white hover:bg-white/5 border border-transparent'}"
            >
                Work
            </button>

            <button 
                on:click={()=>goto('/skills')} 
                class="px-4 py-1.5 rounded-full text-sm md:text-base transition-all duration-200 {$page.url.pathname === '/skills' ? 'bg-gradient-to-r from-blue-600/20 to-purple-600/20 text-blue-400 border border-blue-500/30' : 'text-gray-400 hover:text-white hover:bg-white/5 border border-transparent'}"
            >
                Skills
            </button>

            <button 
                on:click={()=>goto('/about')}  
                class="px-4 py-1.5 rounded-full text-sm md:text-base transition-all duration-200 {$page.url.pathname === '/about' ? 'bg-gradient-to-r from-blue-600/20 to-purple-600/20 text-blue-400 border border-blue-500/30' : 'text-gray-400 hover:text-white hover:bg-white/5 border border-transparent'}"
            >
                About
            </button>
        </div>

    </div>
</div>