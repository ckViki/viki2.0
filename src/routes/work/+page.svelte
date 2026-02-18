<script>
    // @ts-nocheck
    import { fade, fly } from "svelte/transition";
    import { quintOut } from "svelte/easing";

    const experiences = [
        {
            company: 'Mariyano Technologies',
            role: 'Frontend Developer',
            dates: 'Present',
            summary:
                'Focused on modern web interfaces, component-driven UIs, and seamless user experiences.',
            bullets: [
                'Implemented modular, reusable components',
                'Adopted modern tooling and best practices'
            ]
        },
        {
            company: 'Adaptive Media',
            role: 'Associate Software Engineer',
            dates: '2021 - 2023',
            summary:
                'Worked full-time focusing on frontend development and contributed to the Cleared 4 project.',
            bullets: [
                'Developed feature-rich frontend components',
                'Collaborated on accessibility and performance improvements'
            ]
        },
        {
            company: 'Adaptive Media',
            role: 'Intern',
            dates: '2021',
            summary:
                "In 2021, I started my web development career as an intern, igniting my passion for coding and web design.",
            bullets: [
                'Built responsive layouts and assisted with UI components',
                'Learned version control and basic frontend tooling'
            ]
        }
    ];

    let mouseX = 0;
    let mouseY = 0;

    function handleMouseMove(e, node) {
        const rect = node.getBoundingClientRect();
        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;
        const centerX = rect.width / 2;
        const centerY = rect.height / 2;
        
        const rotateX = (y - centerY) / 10;
        const rotateY = (centerX - x) / 10;

        node.style.transform = `perspective(1000px) rotateX(${rotateX}deg) rotateY(${rotateY}deg) scale3d(1.02, 1.02, 1.02)`;
    }

    function handleMouseLeave(node) {
        node.style.transform = `perspective(1000px) rotateX(0deg) rotateY(0deg) scale3d(1, 1, 1)`;
    }
</script>

<div class="container mx-auto px-4 py-20 min-h-screen">
    <!-- Header -->
    <div class="mb-20 text-center">
        <h1 class="text-6xl md:text-8xl font-black uppercase tracking-tighter text-white">
            Experience
        </h1>
        <div class="h-1 w-20 bg-white mx-auto mt-4"></div>
    </div>

    <div class="grid grid-cols-1 gap-12 max-w-5xl mx-auto">
        {#each experiences as exp, i}
            <div
                in:fly={{
                    y: 50,
                    duration: 1000,
                    delay: i * 200,
                    easing: quintOut,
                }}
                class="group relative perspective-1000"
            >
                <div 
                    role="presentation"
                    class="bg-zinc-900/30 backdrop-blur-xl border border-white/5 rounded-3xl p-8 md:p-12 transition-all duration-300 ease-out preserve-3d group-hover:border-white/20 group-hover:bg-zinc-800/40"
                    on:mousemove={(e) => handleMouseMove(e, e.currentTarget)}
                    on:mouseleave={(e) => handleMouseLeave(e.currentTarget)}
                >
                    <div class="flex flex-col md:flex-row gap-8 items-start">
                        <!-- Dates -->
                        <div class="md:w-1/4">
                            <span class="text-xs font-mono uppercase tracking-[0.3em] text-zinc-500 mb-2 block">{exp.dates}</span>
                            <h3 class="text-2xl font-bold text-white leading-tight">
                                {exp.role}
                            </h3>
                            <div class="text-lg text-zinc-400 font-light mt-1 italic">{exp.company}</div>
                        </div>

                        <!-- Content -->
                        <div class="md:flex-1 space-y-6">
                            <p class="text-xl text-zinc-300 font-light leading-relaxed">
                                {exp.summary}
                            </p>
                            
                            {#if exp.bullets && exp.bullets.length > 0}
                                <ul class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                    {#each exp.bullets as bullet}
                                        <li class="flex items-start gap-3 text-zinc-400">
                                            <div class="w-1.5 h-1.5 rounded-full bg-white mt-2 flex-shrink-0"></div>
                                            <span class="text-sm font-light leading-snug">{bullet}</span>
                                        </li>
                                    {/each}
                                </ul>
                            {/if}
                        </div>
                    </div>

                    <!-- 3D shadow effect -->
                    <div class="absolute inset-0 bg-white/5 blur-3xl opacity-0 group-hover:opacity-100 transition-opacity -z-10 rounded-3xl"></div>
                </div>
            </div>
        {/each}
    </div>
</div>

<style>
    .preserve-3d {
        transform-style: preserve-3d;
        will-change: transform;
    }
</style>


