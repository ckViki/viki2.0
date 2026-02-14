<script>
// @ts-nocheck

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
<div class="min-h-screen bg-black text-white px-4 md:px-8 py-12">
    <div class="max-w-6xl mx-auto grid md:grid-cols-2 gap-10 items-center">

        <!-- Left: Profile card -->
        <div class="flex items-center justify-center">
            <div class="profile-card relative w-full max-w-sm p-6 rounded-2xl bg-gradient-to-br from-white/3 to-white/6 border border-white/6 backdrop-blur-md">
                <div class="avatar-wrap w-full flex justify-center">
                    <img src="/working.png" alt="profile" class="w-40 h-40 rounded-full object-cover ring-4 ring-white/10 shadow-lg hover:scale-105 transition-transform duration-300" />
                </div>

                <div class="mt-4 text-center">
                    <h3 class="text-2xl font-bold">Vignesh Kumar C</h3>
                    <p class="text-sm text-gray-300 mt-1">Frontend Developer — UI/UX enthusiast</p>
                </div>

                <div class="mt-5 flex items-center justify-center gap-4">
                    <button aria-label="LinkedIn" on:click={() => gotoSocialMedia('linkedIn')} class="social-btn">
                        <img src="/socialmedia/linkedin.png" alt="LinkedIn" class="w-6 h-6" />
                    </button>
                    <button aria-label="WhatsApp" on:click={() => gotoSocialMedia('whatsup')} class="social-btn">
                        <img src="/socialmedia/whatsup.png" alt="WhatsApp" class="w-6 h-6" />
                    </button>
                    <button aria-label="Instagram" on:click={() => gotoSocialMedia('insta')} class="social-btn">
                        <img src="/socialmedia/insta.png" alt="Instagram" class="w-6 h-6" />
                    </button>
                </div>

                <div class="mt-6 flex justify-center">
                    <a href="mailto:youremail@example.com" class="cta inline-block px-5 py-2 rounded-md bg-blue-500 text-black font-semibold hover:bg-blue-400 transition">Contact Me</a>
                </div>
            </div>
        </div>

        <!-- Right: About content -->
        <div>
            <h2 class="text-4xl font-bold mb-4">About</h2>
            <p class="text-gray-300 mb-6">I build modern web interfaces with a focus on performance, accessibility and delightful UX. I enjoy solving problems, designing components and shipping polished products.</p>

            <div class="grid gap-4 md:grid-cols-2">
                <div class="info-card p-4 rounded-lg bg-neutral-900/40 border border-white/6">
                    <h4 class="font-semibold mb-2">What I do</h4>
                    <ul class="text-sm text-gray-300 list-disc list-inside space-y-1">
                        <li>Frontend development (Svelte, Tailwind, JS)</li>
                        <li>Design systems & UI components</li>
                        <li>Responsive & accessible interfaces</li>
                    </ul>
                </div>

                <div class="info-card p-4 rounded-lg bg-neutral-900/40 border border-white/6">
                    <h4 class="font-semibold mb-2">Highlights</h4>
                    <ul class="text-sm text-gray-300 list-disc list-inside space-y-1">
                        <li>Worked on Cleared 4 — pandemic safety tool</li>
                        <li>Built modular frontend components</li>
                        <li>Experience with performance and accessibility</li>
                    </ul>
                </div>
            </div>

            <p class="text-gray-400 mt-6">When I'm not coding I ride my motorcycle to discover new places and draw creative inspiration.</p>
        </div>

    </div>
</div>