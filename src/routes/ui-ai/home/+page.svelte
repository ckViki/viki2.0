<section class="min-h-[92.5vh] flex items-center bg-black text-white px-6 md:px-12">
    <div class="max-w-6xl mx-auto w-full grid md:grid-cols-2 gap-8 items-center">

        <!-- Left: Headline + CTA -->
        <div class="space-y-6">
            <h1 class="text-[10vw] md:text-[6rem] leading-none font-extrabold tracking-tight gradient-text content animate-fade-down animate-once animate-delay-200">DEVELOPER</h1>

            <div class="text-lg md:text-xl text-gray-300 max-w-xl animate-fade-up animate-once animate-delay-300">
                UI/UX Designer & Frontend Engineer. I build fast, accessible, and delightful user interfaces using modern web
                technologies — turning ideas into polished digital experiences.
            </div>

            <div class="flex items-center gap-4 mt-4">
                <a href="/work" class="inline-block px-6 py-3 bg-blue-500 text-black font-semibold rounded-md hover:bg-blue-400 transition">See my work</a>
                <a href="/about" class="inline-block px-4 py-3 border border-white/10 rounded-md text-sm hover:bg-white/5 transition">About me</a>
            </div>

            <div class="flex items-center gap-3 mt-6 md:mt-8">
                <img src="/socialmedia/linkedin.png" alt="linkedin" class="w-8 h-8 opacity-90 hover:scale-110 transition" />
                <img src="/socialmedia/insta.png" alt="insta" class="w-8 h-8 opacity-90 hover:scale-110 transition" />
                <img src="/socialmedia/whatsup.png" alt="whatsapp" class="w-8 h-8 opacity-90 hover:scale-110 transition" />
            </div>
        </div>

        <!-- Right: Image with mask and subtle motion (preserve existing effects) -->
        <div class="relative flex items-center justify-center">
            <img src="/image/white 2.png" alt="hero" class="imageFade w-[80%] md:w-full max-h-[60vh] object-contain animate-fade-up animate-once animate-delay-200" />

            <!-- decorative blurred shapes for a modern feel -->
            <div class="pointer-events-none absolute -right-10 -top-10 w-40 h-40 rounded-full bg-blue-500/10 blur-3xl"></div>
            <div class="pointer-events-none absolute left-6 bottom-4 w-36 h-36 rounded-full bg-pink-500/6 blur-2xl"></div>
        </div>

    </div>
</section>

<style>
.imageFade{
    mask-image: linear-gradient(180deg, #000 40%, #0000);
}

.gradient-text {
    background: linear-gradient(90deg,#fff 0%,#9ad1ff 50%,#fff 100%);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
}

.content {
    -webkit-animation: fadeInLeft 2s ease forwards;
    animation: fadeInLeft 2s ease forwards;
}

@media (min-width: 768px) {
    h1.gradient-text { letter-spacing: -6px; }
}
</style>