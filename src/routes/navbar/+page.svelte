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

<div class="fixed top-8 left-1/2 -translate-x-1/2 z-50 w-fit transition-all duration-500 hover:scale-[1.02]">
    <div class="bg-black/40 backdrop-blur-xl border border-white/10 rounded-2xl px-2 py-2 md:px-3 md:py-2.5 shadow-[0_0_30px_rgba(0,0,0,0.5)] flex items-center justify-center gap-1 md:gap-2">
        
        <!-- Navigation Links -->
        <div class="flex items-center gap-1 md:gap-2 justify-center">
            {#each [
                { name: 'Home', path: '/' },
                { name: 'Work', path: '/work' },
                { name: 'Skills', path: '/skills' },
                { name: 'About', path: '/about' }
            ] as link}
                <button 
                    on:click={() => goto(link.path)} 
                    class="px-5 py-2 rounded-xl text-sm md:text-base font-medium transition-all duration-300 transform
                    { $page.url.pathname === link.path 
                        ? 'bg-white text-black shadow-[0_0_15px_rgba(255,255,255,0.3)]' 
                        : 'text-gray-400 hover:text-white hover:bg-white/10' }"
                >
                    {link.name}
                </button>
            {/each}
        </div>

    </div>
</div>