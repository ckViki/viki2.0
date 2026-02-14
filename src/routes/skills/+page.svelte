<script>
    import { fade, fly } from "svelte/transition";
    import { quintOut } from "svelte/easing";

    const industryKnowledge = [
        {
            name: "HTML 5",
            icon: "/skill/html-5.png",
            percent: 90,
            color: "text-orange-500",
        },
        {
            name: "CSS 3",
            icon: "/skill/css-3.png",
            percent: 85,
            color: "text-blue-500",
        },
        {
            name: "JavaScript",
            icon: "/skill/javascript.png",
            percent: 75,
            color: "text-yellow-400",
        },
        {
            name: "Bootstrap",
            icon: "/skill/bootstrap.png",
            percent: 90,
            color: "text-purple-600",
        },
        {
            name: "Tailwind CSS",
            icon: "/skill/Tailwind CSS.png",
            percent: 80,
            color: "text-cyan-400",
        },
        {
            name: "Angular",
            icon: "/skill/angular-logo 2.png",
            percent: 60,
            color: "text-red-500",
        },
        {
            name: "Svelte",
            icon: "/skill/Svelte.png",
            percent: 80,
            color: "text-orange-600",
        },
        {
            name: "React",
            icon: "https://upload.wikimedia.org/wikipedia/commons/thumb/a/a7/React-icon.svg/512px-React-icon.svg.png",
            percent: 25,
            color: "text-cyan-400",
        },
    ];

    const tools = [
        { name: "GitHub", icon: "/skill/GitHub.png", percent: 70 },
        {
            name: "Adobe Photoshop",
            icon: "/skill/Adobe Photoshop.png",
            percent: 80,
        },
        { name: "Figma", icon: "/skill/Figma 2.png", percent: 50 },
        { name: "Firebase", icon: "/skill/Firebase.png", percent: 30 },
    ];

    const languages = [
        { name: "Tamil", percent: 100 },
        { name: "English", percent: 90 },
        { name: "Hindi", percent: 10 },
    ];
</script>

<div class="container mx-auto px-4 py-10 min-h-screen">
    <!-- Header -->
    <div class="mb-12 text-center md:text-left">
        <h1 class="text-4xl md:text-5xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-blue-400 to-purple-600 inline-block">
            Skills & Expertise
        </h1>
    </div>

    <div class="flex flex-col gap-16">
        <!-- Industry Knowledge Section -->
        <section>
            <h2 class="text-2xl font-semibold text-white mb-8 flex items-center gap-3">
                <span class="w-10 h-1 bg-blue-500 rounded-full"></span>
                Industry Knowledge
            </h2>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                {#each industryKnowledge as skill, i}
                    <div
                        in:fly={{
                            y: 20,
                            duration: 800,
                            delay: i * 100,
                            easing: quintOut,
                        }}
                        class="group relative bg-gray-900/50 backdrop-blur-sm border border-gray-800 rounded-xl p-6 hover:border-blue-500/50 hover:bg-gray-800/50 transition-all duration-300 hover:shadow-lg hover:shadow-blue-500/10 hover:-translate-y-1"
                    >
                        <div class="flex items-center gap-4 mb-4">
                            <div class="bg-gray-800 rounded-lg p-3 group-hover:bg-gray-700 transition-colors">
                                <img class="w-10 h-10 object-contain" src={skill.icon} alt={skill.name} />
                            </div>
                            <h3 class="text-xl font-bold text-gray-100">
                                {skill.name}
                            </h3>
                        </div>

                        <div class="space-y-2">
                            <div class="flex justify-between text-sm">
                                <span class="text-gray-400">Proficiency</span>
                                <span class="text-blue-400 font-mono">{skill.percent}%</span>
                            </div>
                            <div class="h-2 w-full bg-gray-700 rounded-full overflow-hidden">
                                <div class="h-full bg-gradient-to-r from-blue-600 to-purple-600 rounded-full group-hover:from-blue-500 group-hover:to-purple-500 transition-all duration-1000 ease-out" style="width: {skill.percent}%"></div>
                            </div>
                        </div>
                    </div>
                {/each}
            </div>
        </section>

        <!-- Tools Section -->
        <section>
            <h2 class="text-2xl font-semibold text-white mb-8 flex items-center gap-3">
                <span class="w-10 h-1 bg-purple-500 rounded-full"></span>
                Tools & Technologies
            </h2>

            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                {#each tools as tool, i}
                    <div
                        in:fly={{
                            y: 20,
                            duration: 800,
                            delay: (industryKnowledge.length + i) * 100,
                            easing: quintOut,
                        }}
                        class="group relative bg-gray-900/50 backdrop-blur-sm border border-gray-800 rounded-xl p-6 hover:border-purple-500/50 hover:bg-gray-800/50 transition-all duration-300 hover:shadow-lg hover:shadow-purple-500/10 hover:-translate-y-1"
                    >
                        <div class="flex items-center gap-4 mb-4">
                            <div
                                class="bg-gray-800 rounded-lg p-3 group-hover:bg-gray-700 transition-colors"
                            >
                                <img
                                    class="w-10 h-10 object-contain"
                                    src={tool.icon}
                                    alt={tool.name}
                                />
                            </div>
                            <h3 class="text-xl font-bold text-gray-100">
                                {tool.name}
                            </h3>
                        </div>

                        <div class="space-y-2">
                            <div class="flex justify-between text-sm">
                                <span class="text-gray-400">Proficiency</span>
                                <span class="text-purple-400 font-mono">{tool.percent}%</span>
                            </div>
                            <div class="h-2 w-full bg-gray-700 rounded-full overflow-hidden">
                                <div class="h-full bg-gradient-to-r from-purple-600 to-pink-600 rounded-full group-hover:from-purple-500 group-hover:to-pink-500 transition-all duration-1000 ease-out" style="width: {tool.percent}%"></div>
                            </div>
                        </div>
                    </div>
                {/each}
            </div>
        </section>

        <!-- Languages Section (Optional / Hidden in original but modernized here) -->
        {#if languages.length > 0}
            <section class="hidden">
                <h2
                    class="text-2xl font-semibold text-white mb-8 flex items-center gap-3"
                >
                    <span class="w-10 h-1 bg-green-500 rounded-full"></span>
                    Languages
                </h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    {#each languages as lang, i}
                        <div
                            in:fly={{
                                y: 20,
                                duration: 800,
                                delay:
                                    (industryKnowledge.length +
                                        tools.length +
                                        i) *
                                    100,
                                easing: quintOut,
                            }}
                            class="bg-gray-900/30 border border-gray-800 rounded-lg p-5 flex items-center justify-between hover:border-green-500/30 transition-colors"
                        >
                            <span class="font-medium text-lg text-gray-200"
                                >{lang.name}</span
                            >
                            <div class="flex items-center gap-3">
                                <div
                                    class="w-24 h-1.5 bg-gray-700 rounded-full overflow-hidden"
                                >
                                    <div
                                        class="h-full bg-green-500 rounded-full"
                                        style="width: {lang.percent}%"
                                    ></div>
                                </div>
                                <span
                                    class="text-sm text-gray-400 w-8 text-right"
                                    >{lang.percent}%</span
                                >
                            </div>
                        </div>
                    {/each}
                </div>
            </section>
        {/if}
    </div>
</div>

<style>
    /* Custom scrollbar or specific overrides if needed */
</style>
