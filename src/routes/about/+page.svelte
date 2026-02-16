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

    function handleMouseMove(e, node) {
        const rect = node.getBoundingClientRect();
        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;
        const centerX = rect.width / 2;
        const centerY = rect.height / 2;
        const rotateX = (y - centerY) / 20;
        const rotateY = (centerX - x) / 20;
        node.style.transform = `perspective(1000px) rotateX(${rotateX}deg) rotateY(${rotateY}deg)`;
    }

    function handleMouseLeave(node) {
        node.style.transform = `perspective(1000px) rotateX(0deg) rotateY(0deg)`;
    }
</script>

<div class="container mx-auto px-4 py-20 min-h-screen">
    <!-- Header -->
    <div class="mb-20 text-center">
        <h1 class="text-6xl md:text-8xl font-black uppercase tracking-tighter text-white">
            About
        </h1>
        <div class="h-1 w-20 bg-white mx-auto mt-4"></div>
    </div>

    <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-stretch max-w-6xl mx-auto">
        <!-- Text Content -->
        <div 
            in:fly={{ x: -50, duration: 1000, delay: 200, easing: quintOut }}
            class="group perspective-1000"
        >
            <div 
                role="presentation"
                class="bg-zinc-900/40 backdrop-blur-2xl border border-white/5 rounded-[2rem] p-10 md:p-16 h-full flex flex-col justify-center transition-all duration-300 preserve-3d group-hover:border-white/20"
                on:mousemove={(e) => handleMouseMove(e, e.currentTarget)}
                on:mouseleave={(e) => handleMouseLeave(e.currentTarget)}
            >
                <h2 class="text-3xl font-black text-white mb-8 uppercase tracking-tight">Identity</h2>
                <p class="text-zinc-400 text-xl font-light leading-relaxed mb-12">
                    As a web developer and designer who lives for the <span class="text-white italic">freedom</span> of the online world, I extend that
                    freedom onto the roads, seeking new inspirations and perspectives on my <span class="text-white font-medium border-b border-white/20">trusty motorcycle</span>.
                </p>

                <!-- Social Media -->
                <div class="flex flex-wrap items-center gap-6">
                    {#each [
                        { name: 'linkedIn', icon: '/socialmedia/linkedin.png' },
                        { name: 'whatsup', icon: '/socialmedia/whatsup.png' },
                        { name: 'insta', icon: '/socialmedia/insta.png' },
                        { name: 'Telegram', icon: '/socialmedia/telegram.png' }
                    ] as social}
                        <button 
                            on:click={() => social.name !== 'Telegram' ? gotoSocialMedia(social.name) : null} 
                            class="group/btn relative"
                            title={social.name}
                        >
                            <div class="absolute -inset-2 bg-white/10 rounded-2xl opacity-0 group-hover/btn:opacity-100 transition-opacity blur-sm"></div>
                            <div class="relative p-4 bg-zinc-800/50 rounded-2xl border border-white/5 transition-all duration-300 group-hover/btn:-translate-y-1 group-hover/btn:bg-white group-hover/btn:border-white">
                                <img class="w-6 h-6 grayscale group-hover/btn:invert transition-all duration-300" src={social.icon} alt={social.name}>
                            </div>
                        </button>
                    {/each}
                </div>
            </div>
        </div>

        <!-- Image Content -->
        <div 
            in:fly={{ x: 50, duration: 1000, delay: 400, easing: quintOut }}
            class="relative group"
        >
            <div class="absolute inset-0 bg-white/5 rounded-[2rem] blur-2xl opacity-0 group-hover:opacity-100 transition-opacity duration-700"></div>
            <div class="relative bg-zinc-900 rounded-[2rem] overflow-hidden border border-white/5 h-full min-h-[400px]">
                <img src="/working.png" class="w-full h-full object-cover grayscale transition duration-1000 group-hover:grayscale-0 group-hover:scale-110" alt="Working">
                <div class="absolute inset-0 bg-gradient-to-t from-black/80 via-transparent to-transparent opacity-60 group-hover:opacity-20 transition-opacity"></div>
            </div>
        </div>
    </div>
</div>

<style>
    .preserve-3d {
        transform-style: preserve-3d;
        will-change: transform;
    }
</style>