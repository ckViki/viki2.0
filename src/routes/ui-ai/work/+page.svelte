<script>
// @ts-nocheck

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

<div class="bg-black text-white min-h-screen px-4 md:px-8 py-8">
    <div class="max-w-5xl mx-auto">
        <h1 class="text-3xl md:text-4xl font-bold mb-6">Work Experience</h1>

        <div class="space-y-8">
            {#each experiences as exp, i}
                <article class="md:flex md:items-start md:gap-6">
                    <div class="md:w-1/4">
                        <div class="text-sm text-gray-400">{exp.dates}</div>
                        <div class="text-lg font-semibold mt-2">{exp.role}</div>
                        <div class="text-sm text-gray-300 mt-1">{exp.company}</div>
                    </div>

                    <div class="md:flex-1 bg-neutral-900/60 p-4 rounded-lg border border-white/10">
                        <p class="text-sm text-gray-200 mb-3">{exp.summary}</p>
                        <ul class="list-disc list-inside text-sm text-gray-300 space-y-1">
                            {#each exp.bullets as b}
                                <li>{b}</li>
                            {/each}
                        </ul>
                    </div>
                </article>
            {/each}
        </div>
    </div>
</div>

