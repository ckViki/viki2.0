<script>
    // @ts-nocheck
    import { onMount } from 'svelte';
    import { fly, fade } from 'svelte/transition';
    import { quintOut } from 'svelte/easing';
    import { browser } from '$app/environment';

    let container;
    let title2, description;
    let textRow2;
    const words = ["DEVELOPER", "DESIGNER"];
    let currentWordIndex = 0;

    onMount(async () => {
        if (browser) {
            const gsapModule = await import('gsap');
            const gsap = gsapModule.default;
            
            const tl = gsap.timeline({ defaults: { ease: "power4.out", duration: 1.2 } });
            
            // Initial animation
            tl.from(title2, { y: 50, opacity: 0, delay: 0.5 })
              .from(".char", { 
                  y: 100, 
                  opacity: 0, 
                  stagger: 0.05, 
                  duration: 1, 
                  ease: "back.out(1.7)" 
              }, "-=0.8")
              .from(description, { y: 20, opacity: 0 }, "-=0.5");

            // Rotating animation for second row
            const rotateText = () => {
                const nextIndex = (currentWordIndex + 1) % words.length;
                const chars = textRow2.querySelectorAll('.char');
                
                const rotationTl = gsap.timeline();
                
                // Fly out
                rotationTl.to(chars, { 
                    y: -100, 
                    opacity: 0, 
                    stagger: 0.03, 
                    duration: 0.6, 
                    ease: "power4.in",
                    onComplete: () => {
                        currentWordIndex = nextIndex;
                        // Wait for Svelte to update DOM
                        setTimeout(() => {
                            const newChars = textRow2.querySelectorAll('.char');
                            gsap.set(newChars, { y: 100, opacity: 0 });
                            gsap.to(newChars, { 
                                y: 0, 
                                opacity: 1, 
                                stagger: 0.05, 
                                duration: 0.8, 
                                ease: "back.out(1.2)" 
                            });
                        }, 0);
                    }
                });
            };

            // Start rotation interval after initial animation
            setTimeout(() => {
                setInterval(rotateText, 3500);
            }, 3000);
        }
    });
</script>

<div bind:this={container} class="flex flex-col w-full min-h-[90vh] text-white relative items-center justify-center px-4 overflow-hidden">
    
    <div class="space-y-4 text-center z-10">
        <div class="overflow-hidden relative">
            <h1 bind:this={title2} class="text-zinc-500 font-medium tracking-[0.4em] uppercase text-sm md:text-2xl selection:bg-white selection:text-black mb-2 opacity-80">
                Web
            </h1>
        </div>

        <div bind:this={textRow2} class="overflow-hidden h-[15vw] lg:h-[14vw] flex items-center justify-center">
            <h1 class="font-black text-[15vw] lg:text-[14vw] leading-[0.8] tracking-tighter uppercase selection:bg-white selection:text-black transition-all duration-500 flex
                {words[currentWordIndex] === 'DESIGNER' ? 'text-transparent stroke-text' : 'text-white'}">
                {#each words[currentWordIndex].split('') as char}
                    <span class="char inline-block">{char}</span>
                {/each}
            </h1>
        </div>
    </div>

    <!-- Subtitle and Wide Description -->
    <div class="mt-12 text-center z-10">
        <!-- <h2 bind:this={subtitle} class="text-xl md:text-3xl font-light tracking-[0.3em] uppercase mb-8 text-zinc-400">
            UI/UX <span class="text-white font-bold italic">Designer</span>
        </h2> -->
        
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