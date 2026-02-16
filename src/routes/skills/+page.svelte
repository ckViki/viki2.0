<script>
    // @ts-nocheck
    import { fade, fly } from "svelte/transition";
    import { quintOut } from "svelte/easing";

    const industryKnowledge = [
        { name: "HTML 5", icon: "/skill/html-5.png", percent: 90 },
        { name: "CSS 3", icon: "/skill/css-3.png", percent: 85 },
        { name: "JavaScript", icon: "/skill/javascript.png", percent: 75 },
        { name: "Bootstrap", icon: "/skill/bootstrap.png", percent: 90 },
        { name: "Tailwind CSS", icon: "/skill/Tailwind CSS.png", percent: 80 },
        { name: "Angular", icon: "/skill/angular-logo 2.png", percent: 60 },
        { name: "Svelte", icon: "/skill/Svelte.png", percent: 80 },
        { name: "React", icon: "https://upload.wikimedia.org/wikipedia/commons/thumb/a/a7/React-icon.svg/512px-React-icon.svg.png", percent: 25 },
    ];

    const tools = [
        { name: "GitHub", icon: "/skill/GitHub.png", percent: 70 },
        { name: "Adobe Photoshop", icon: "/skill/Adobe Photoshop.png", percent: 80 },
        { name: "Figma", icon: "/skill/Figma 2.png", percent: 50 },
        { name: "Firebase", icon: "/skill/Firebase.png", percent: 30 },
    ];

    function handleMouseMove(e, node) {
        const rect = node.getBoundingClientRect();
        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;
        const centerX = rect.width / 2;
        const centerY = rect.height / 2;
        const rotateX = (y - centerY) / 15;
        const rotateY = (centerX - x) / 15;
        node.style.transform = `perspective(1000px) rotateX(${rotateX}deg) rotateY(${rotateY}deg) scale3d(1.05, 1.05, 1.05)`;
    }

    function handleMouseLeave(node) {
        node.style.transform = `perspective(1000px) rotateX(0deg) rotateY(0deg) scale3d(1, 1, 1)`;
    }
</script>

<div class="container mx-auto px-4 py-20 min-h-screen">
    <!-- Header -->
    <div class="mb-20 text-center">
        <h1 class="text-6xl md:text-8xl font-black uppercase tracking-tighter text-white">
            Expertise
        </h1>
        <div class="h-1 w-20 bg-white mx-auto mt-4"></div>
    </div>

    <div class="flex flex-col gap-24">
        <!-- Industry Knowledge Section -->
        <section>
            <div class="flex items-center gap-4 mb-12">
                <h2 class="text-2xl font-bold uppercase tracking-[0.2em] text-zinc-500 italic">Industry <span class="text-white not-italic">Knowledge</span></h2>
                <div class="h-[1px] flex-1 bg-white/10"></div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8">
                {#each industryKnowledge as skill, i}
                    <div
                        in:fly={{ y: 30, duration: 1000, delay: i * 100, easing: quintOut }}
                        class="group relative perspective-1000"
                    >
                        <div
                            role="presentation"
                            class="bg-zinc-900/40 backdrop-blur-xl border border-white/5 rounded-3xl p-8 transition-all duration-300 ease-out preserve-3d group-hover:border-white/20 group-hover:bg-zinc-800/50"
                            on:mousemove={(e) => handleMouseMove(e, e.currentTarget)}
                            on:mouseleave={(e) => handleMouseLeave(e.currentTarget)}
                        >
                            <div class="flex flex-col items-center text-center">
                                <div class="w-16 h-16 mb-6 grayscale group-hover:grayscale-0 transition-all duration-500 transform group-hover:scale-110">
                                    <img class="w-full h-full object-contain" src={skill.icon} alt={skill.name} />
                                </div>
                                <h3 class="text-lg font-bold text-white mb-4 tracking-tight">{skill.name}</h3>
                                
                                <div class="w-full space-y-2">
                                    <div class="flex justify-between text-[10px] font-mono uppercase tracking-widest text-zinc-500">
                                        <span>Mastery</span>
                                        <span>{skill.percent}%</span>
                                    </div>
                                    <div class="h-1 w-full bg-white/5 rounded-full overflow-hidden">
                                        <div class="h-full bg-white transition-all duration-1000 ease-out" style="width: {skill.percent}%"></div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                {/each}
            </div>
        </section>

        <!-- Tools Section -->
        <section>
            <div class="flex items-center gap-4 mb-12">
                <h2 class="text-2xl font-bold uppercase tracking-[0.2em] text-zinc-500 italic">Key <span class="text-white not-italic">Stack</span></h2>
                <div class="h-[1px] flex-1 bg-white/10"></div>
            </div>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-8">
                {#each tools as tool, i}
                    <div
                        in:fly={{ y: 30, duration: 1000, delay: i * 100, easing: quintOut }}
                        class="group relative perspective-1000"
                    >
                        <div
                            role="presentation"
                            class="bg-zinc-900/40 backdrop-blur-xl border border-white/5 rounded-3xl p-8 transition-all duration-300 ease-out preserve-3d group-hover:border-white/20 group-hover:bg-zinc-800/50"
                            on:mousemove={(e) => handleMouseMove(e, e.currentTarget)}
                            on:mouseleave={(e) => handleMouseLeave(e.currentTarget)}
                        >
                            <div class="flex flex-col items-center text-center">
                                <div class="w-16 h-16 mb-6 grayscale group-hover:grayscale-0 transition-all duration-500 transform group-hover:scale-110">
                                    <img class="w-full h-full object-contain" src={tool.icon} alt={tool.name} />
                                </div>
                                <h3 class="text-lg font-bold text-white mb-4 tracking-tight">{tool.name}</h3>
                                
                                <div class="w-full space-y-2">
                                    <div class="flex justify-between text-[10px] font-mono uppercase tracking-widest text-zinc-500">
                                        <span>Effectiveness</span>
                                        <span>{tool.percent}%</span>
                                    </div>
                                    <div class="h-1 w-full bg-white/5 rounded-full overflow-hidden">
                                        <div class="h-full bg-white/40 transition-all duration-1000 ease-out" style="width: {tool.percent}%"></div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                {/each}
            </div>
        </section>
    </div>
</div>

<style>
    .preserve-3d {
        transform-style: preserve-3d;
        will-change: transform;
    }
</style>

