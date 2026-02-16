<script>
    // @ts-nocheck
    import { onMount } from 'svelte';
    import { fly, fade } from 'svelte/transition';
    import { quintOut } from 'svelte/easing';
    import { browser } from '$app/environment';

    let container;
    let title1, title2, subtitle, description;

    onMount(async () => {
        if (browser) {
            const gsapModule = await import('gsap');
            const gsap = gsapModule.default;
            
            const tl = gsap.timeline({ defaults: { ease: "power4.out", duration: 1.5 } });
            
            tl.from(title1, { y: 100, opacity: 0, delay: 0.5 })
              .from(title2, { y: 100, opacity: 0 }, "-=1.2")
              .from(subtitle, { x: -50, opacity: 0 }, "-=1")
              .from(description, { y: 20, opacity: 0 }, "-=0.8");
        }
    });
</script>

<div bind:this={container} class="flex flex-col w-full min-h-[90vh] text-white relative items-center justify-center px-4 overflow-hidden">
    
    <div class="space-y-2 text-center z-10">
        <div class="overflow-hidden relative">
            <h1 bind:this={title2} class="font-black text-[14vw] lg:text-[14vw] leading-[0.8] tracking-tighter uppercase text-transparent stroke-text selection:bg-white selection:text-black">
                Designer
            </h1>
            <div class="absolute inset-0 bg-gradient-to-t from-black to-transparent pointer-events-none h-1/2 bottom-0"></div>
        </div>

        <div class="overflow-hidden">
            <h1 bind:this={title1} class="font-black text-[15vw] lg:text-[14vw] leading-[0.8] tracking-tighter uppercase text-white selection:bg-white selection:text-black">
                DEVELOPER
            </h1>
        </div>
    </div>

    <!-- Subtitle and Wide Description -->
    <div class="mt-12 text-center z-10">
        <h2 bind:this={subtitle} class="text-xl md:text-3xl font-light tracking-[0.3em] uppercase mb-8 text-zinc-400">
            UI/UX <span class="text-white font-bold italic">Designer</span>
        </h2>
        
        <p bind:this={description} class="text-lg md:text-2xl text-zinc-500 leading-relaxed font-light">
            Your friendly neighborhood <span class="text-white border-b border-white/20">frontend developer</span>, UX architect, and JavaScript engineer. 
            I spend my days painting the <span class="text-white italic">Internet canvas</span> with lines of code, 
            turning zeroes and ones into <span class="text-white font-medium">immersive experiences</span>.
        </p>
    </div>

    <!-- Scroll Indicator -->
    <div class="absolute bottom-10 left-1/2 -translate-x-1/2 animate-bounce opacity-20 transition-opacity hover:opacity-100 cursor-pointer">
        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1" d="M19 14l-7 7-7-7m14-7l-7 7-7-7" />
        </svg>
    </div>
</div>

<style>
    .stroke-text {
        -webkit-text-stroke: 1px rgba(255, 255, 255, 0.3);
    }

    :global(::selection) {
        background: white;
        color: black;
    }
</style>