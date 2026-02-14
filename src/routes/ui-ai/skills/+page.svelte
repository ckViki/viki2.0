
<script>
// @ts-nocheck
const skills = [
  { name: 'HTML', icon: '/skill/html-5.png', level: 90 },
  { name: 'CSS', icon: '/skill/css-3.png', level: 85 },
  { name: 'JavaScript', icon: '/skill/javascript.png', level: 75 },
  { name: 'Bootstrap', icon: '/skill/bootstrap.png', level: 90 },
  { name: 'Tailwind', icon: '/skill/Tailwind CSS.png', level: 80 },
  { name: 'Angular', icon: '/skill/angular-logo 2.png', level: 60 },
  { name: 'Svelte', icon: '/skill/Svelte.png', level: 80 },
  { name: 'GitHub', icon: '/skill/GitHub.png', level: 70 },
  { name: 'Adobe Photoshop', icon: '/skill/Adobe Photoshop.png', level: 80 },
  { name: 'Figma', icon: '/skill/Figma 2.png', level: 50 },
  { name: 'Firebase', icon: '/skill/Firebase.png', level: 30 }
];

function pctOffset(p, r = 36) {
  const c = 2 * Math.PI * r;
  return c - (p / 100) * c;
}

function handleMove(e) {
  const el = e.currentTarget;
  const rect = el.getBoundingClientRect();
  const x = e.clientX - rect.left;
  const y = e.clientY - rect.top;
  const rx = ((y - rect.height / 2) / rect.height) * 12;
  const ry = -((x - rect.width / 2) / rect.width) * 12;
  // set inline transform directly for reliable updates
  el.style.willChange = 'transform';
  el.style.transform = `perspective(900px) rotateX(${rx}deg) rotateY(${ry}deg)`;
}

function resetMove(e) {
  const el = e.currentTarget;
  el.style.transform = 'perspective(900px) rotateX(0deg) rotateY(0deg)';
  el.style.willChange = 'auto';
}
</script>

<div class="min-h-screen bg-black text-white px-4 md:px-8 py-10">
  <div class="max-w-6xl mx-auto">
    <h2 class="text-3xl font-bold mb-6">Skills</h2>

    <p class="text-gray-400 mb-8">Interactive skills overview — hover a card for a subtle 3D tilt and see proficiency levels.</p>

    <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 gap-6">
      {#each skills as s}
        <button class="skill-card bg-neutral-900/40 backdrop-blur-sm border border-white/5 rounded-xl p-4 flex flex-col items-center gap-3 cursor-pointer"
          on:mousemove={(e) => handleMove(e)} on:mouseleave={(e) => resetMove(e)}>
          <svg class="w-20 h-20" viewBox="0 0 100 100">
            <defs></defs>
            <g transform="translate(50,50)">
              <circle r="36" cx="0" cy="0" fill="transparent" stroke="rgba(255,255,255,0.06)" stroke-width="8" />
              <circle r="36" cx="0" cy="0" fill="transparent" stroke="#60a5fa" stroke-width="8"
                stroke-linecap="round" stroke-dasharray={2 * Math.PI * 36} stroke-dashoffset={pctOffset(s.level)}
                transform="rotate(-90)"/>
              <text x="0" y="4" text-anchor="middle" font-size="16" fill="#e5e7eb">{s.level}%</text>
            </g>
          </svg>

          <img src={s.icon} alt={s.name} class="w-10 h-10" />
          <div class="text-center text-sm font-semibold">{s.name}</div>
        </button>
      {/each}
    </div>
  </div>
</div>

<style>
.skill-card {
  transform: perspective(900px) rotateX(var(--rx, 0deg)) rotateY(var(--ry, 0deg));
  transition: transform 250ms ease, box-shadow 250ms ease;
  box-shadow: 0 8px 20px rgba(0,0,0,0.6), 0 1px 0 rgba(255,255,255,0.02) inset;
}
.skill-card:hover {
  box-shadow: 0 18px 60px rgba(96,165,250,0.08), 0 6px 30px rgba(0,0,0,0.6);
}
svg circle[stroke="#60a5fa"] {
  transition: stroke-dashoffset 600ms cubic-bezier(.2,.8,.2,1);
}
</style>