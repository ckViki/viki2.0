<script>
    // @ts-nocheck
    
        // Data for the pie charts, including icon URLs
        const skills = [
          { label: 'HTML', value: 90, color: '#4F46E5', icon: '/skill/html-5.png' },
          { label: 'CSS', value: 85, color: '#3B82F6', icon: '/skill/css-3.png' },
          { label: 'JavaScript', value: 75, color: '#F59E0B', icon: '/skill/javascript.png' },
          { label: 'Bootstrap', value: 80, color: '#EC4899', icon: '/skill/bootstrap.png' },
          { label: 'Tailwind CSS', value: 85, color: '#10B981', icon: '/skill/Tailwind CSS.png' },
          { label: 'Angular', value: 70, color: '#F43F5E', icon: '/skill/angular-logo 2.png' },
          { label: 'Svelte', value: 80, color: '#6366F1', icon: '/skill/Svelte.png' },
        ];
    
        // Function to calculate the arc path for each slice
        function calculateArcPath(startAngle, endAngle, radius) {
          const x1 = Math.cos(startAngle) * radius;
          const y1 = Math.sin(startAngle) * radius;
          const x2 = Math.cos(endAngle) * radius;
          const y2 = Math.sin(endAngle) * radius;
      
          return `M 0 0 L ${x1} ${y1} A ${radius} ${radius} 0 ${endAngle - startAngle > Math.PI ? 1 : 0} 1 ${x2} ${y2} Z`;
        }
      
        // Calculate total value for the pie chart
        const total = 100; // Since each pie chart represents a skill percentage out of 100
      
        // Calculate the angle for each slice
        skills.forEach(skill => {
          const startAngle = 0;
          skill.startAngle = startAngle;
          skill.endAngle = (skill.value / total) * (2 * Math.PI);
        });
    </script>
    
    <div class="flex flex-wrap gap-6 justify-center">
      {#each skills as { label, startAngle, endAngle, value, color, icon }}
        <div class="relative flex flex-col items-center">
          <svg width="150" height="150" viewBox="-100 -100 200 200" class="bg-gray-100 rounded-full">
            <!-- Create the pie slice -->
            <path d={calculateArcPath(startAngle, endAngle, 100)}
                  fill={color}
                  class="transition-transform duration-300 ease-in-out hover:scale-105" />
            <!-- Add a white circle to cover the center -->
            <circle cx="0" cy="0" r="80" fill="white" class="hover:fill-gray-100 transition-colors duration-300 ease-in-out" />
          </svg>
          <!-- Icon in the center -->
          <img src={icon} alt={label} class="absolute w-14 h-14 top-12 object-contain" />
          <!-- <div class="mt-2 text-center">
            <span class="text-gray-800 font-medium">{label}: {value}%</span>
          </div> -->
        </div>
      {/each}
    </div>
    