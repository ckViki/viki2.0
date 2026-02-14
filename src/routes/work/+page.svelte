<script>
    // @ts-nocheck
    import { fade, fly } from "svelte/transition";
    import { quintOut } from "svelte/easing";

    const experiences = [
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
            company: 'Mariyano Technologies',
            role: 'Frontend Developer',
            dates: 'Present',
            summary:
                'Focused on modern web interfaces, component-driven UIs, and seamless user experiences.',
            bullets: [
                'Implemented modular, reusable components',
                'Adopted modern tooling and best practices'
            ]
        }
    ];
</script>

<div class="container mx-auto px-4 py-10 min-h-screen">
    <!-- Header -->
    <div class="mb-12 text-center md:text-left">
        <h1 class="text-4xl md:text-5xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-blue-400 to-purple-600 inline-block">
            Work Experience
        </h1>
    </div>

    <div class="flex flex-col gap-8">
        {#each experiences as exp, i}
            <div
                in:fly={{
                    y: 20,
                    duration: 800,
                    delay: i * 200,
                    easing: quintOut,
                }}
                class="group relative bg-gray-900/50 backdrop-blur-sm border border-gray-800 rounded-xl p-6 hover:border-blue-500/50 hover:bg-gray-800/50 transition-all duration-300 hover:shadow-lg hover:shadow-blue-500/10 hover:-translate-y-1"
            >
                <div class="flex flex-col md:flex-row md:items-start gap-4 md:gap-8">
                    <!-- Left Column: Dates & Company Info -->
                    <div class="md:w-1/4">
                        <div class="flex items-center gap-2 mb-2">
                            <span class="w-8 h-1 bg-blue-500 rounded-full"></span>
                            <span class="text-sm font-mono text-blue-400">{exp.dates}</span>
                        </div>
                        <h3 class="text-xl font-bold text-gray-100 group-hover:text-blue-400 transition-colors">
                            {exp.role}
                        </h3>
                        <div class="text-md text-gray-400 mt-1">{exp.company}</div>
                    </div>

                    <!-- Right Column: Content -->
                    <div class="md:flex-1">
                        <p class="text-gray-300 leading-relaxed mb-4">
                            {exp.summary}
                        </p>
                        
                        {#if exp.bullets && exp.bullets.length > 0}
                            <ul class="space-y-3">
                                {#each exp.bullets as bullet}
                                    <li class="flex items-start gap-3 text-sm text-gray-400 group-hover:text-gray-300 transition-colors">
                                        <svg class="w-5 h-5 text-blue-500/50 mt-0.5 flex-shrink-0" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
                                        </svg>
                                        <span>{bullet}</span>
                                    </li>
                                {/each}
                            </ul>
                        {/if}
                    </div>
                </div>
            </div>
        {/each}
    </div>
</div>

<style>
    /* Custom styles if needed */
</style>

