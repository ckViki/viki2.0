<script>
    import { fly, fade } from 'svelte/transition';
    import { quintOut } from 'svelte/easing';
</script>

<div class="flex w-full h-[90vh] text-white relative overflow-hidden">
    <!-- Restored Original Hero Image Layout -->
    <img in:fly={{ y: 40, duration: 1000, delay: 200, easing: quintOut }} src="/image/white 2.png" 
    class="imageFade absolute z-0 left-[25%] md:left-[40%] mt-10 md:mt-[2px] h-[50vh] md:h-[60vh] select-none pointer-events-none" alt="">

    <div class="m-auto  text-center z-10 px-4">
        <!-- Expanded Beach Sand & Water Wave Effect -->
        <div in:fly={{ y: -30, duration: 1000, delay: 400, easing: quintOut }} class="mt-5 relative inline-block transition-all duration-1000 ease-in-out md:hover:scale-105">
            <!-- Sand Layer -->
            <h1 class="font-bold text-[50px] lg:text-[11.5vw] leading-[0.85] text-white tracking-[0.05em] select-none uppercase">
                DEVELOPER
            </h1>
            
            <!-- Water Layer -->
            <h1 class="absolute inset-0 font-bold text-[50px] lg:text-[11.5vw] leading-[0.85] tracking-[0.05em] select-none bg-clip-text text-transparent bg-gradient-to-r from-blue-400 via-purple-500 to-blue-600 bg-[length:200%_auto] animate-gradient-slow wave-mask uppercase">
                DEVELOPER
            </h1>
        </div>
        
        <!-- Subtitle and Wide Description -->
        <div in:fly={{ y: 20, duration: 800, delay: 600, easing: quintOut }} class="space-y-6 mt-6 md:mt-4">
            <h2 class="text-2xl md:text-[3.5vw] font-bold tracking-[0.2em] uppercase">
                UI/UX <span class="bg-clip-text text-transparent bg-gradient-to-r from-blue-400 to-purple-400">Designer</span>
            </h2>
            
            <p class="max-w-5xl mx-auto text-sm md:text-xl text-gray-400 leading-relaxed font-medium px-4 md:px-0">
                Your friendly neighborhood <span class="text-blue-400">frontend developer</span>, UX architect, and JavaScript engineer. 
                I spend my days painting the <span class="text-white">Internet canvas</span> with lines of code, 
                turning zeroes and ones into <span class="text-purple-400">immersive experiences</span>.
            </p>
        </div>
    </div>
</div>

<style>
.imageFade {
    mask-image: linear-gradient(180deg, rgba(0,0,0,1) 40%, rgba(0,0,0,0) 100%);
}

@keyframes gradient {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
}

@keyframes wave {
    0% { clip-path: polygon(0% 45%, 15% 44%, 32% 50%, 54% 60%, 70% 61%, 84% 59%, 100% 52%, 100% 100%, 0% 100%); }
    25% { clip-path: polygon(0% 60%, 15% 65%, 34% 66%, 51% 62%, 67% 50%, 84% 45%, 100% 46%, 100% 100%, 0% 100%); }
    50% { clip-path: polygon(0% 55%, 18% 50%, 36% 45%, 52% 48%, 68% 55%, 85% 62%, 100% 65%, 100% 100%, 0% 100%); }
    75% { clip-path: polygon(0% 48%, 16% 55%, 33% 62%, 55% 65%, 72% 60%, 86% 50%, 100% 44%, 100% 100%, 0% 100%); }
    100% { clip-path: polygon(0% 45%, 15% 44%, 32% 50%, 54% 60%, 70% 61%, 84% 59%, 100% 52%, 100% 100%, 0% 100%); }
}

.animate-gradient-slow {
    animation: gradient 8s ease infinite;
}

.wave-mask {
    animation: wave 6s ease-in-out infinite;
}
</style>