<script setup>
import { computed } from 'vue'
import { Monitor, Server, Terminal, Cpu } from 'lucide-vue-next'

const skillCategoriesRaw = [
  {
    name: 'Frontend',
    icon: Monitor,
    skills: [
      { name: 'Vue.js', level: 95 },
      { name: 'React', level: 85 },
      { name: 'TailwindCSS', level: 95 },
      { name: 'JavaScript', level: 90 }
    ]
  },
  {
    name: 'Backend',
    icon: Server,
    skills: [
      { name: 'Node.js', level: 85 },
      { name: 'Laravel', level: 80 },
      { name: 'Firebase', level: 90 },
      { name: 'SQL/SQLite', level: 85 }
    ]
  },
  {
    name: 'DevOps',
    icon: Terminal,
    skills: [
      { name: 'Git & GitHub', level: 95 },
      { name: 'VS Code', level: 95 },
      { name: 'Vercel', level: 90 },
      { name: 'Docker', level: 70 }
    ]
  }
]

const skillCategories = computed(() => {
  return skillCategoriesRaw.map(category => ({
    ...category,
    skills: [...category.skills].sort((a, b) => b.level - a.level)
  }))
})
</script>

<template>
  <section id="skills" class="py-24 lg:py-36 bg-gray-50 dark:bg-[#030712] relative overflow-hidden transition-colors duration-500">
    <div class="container mx-auto px-4 md:px-6 lg:px-8 max-w-7xl relative z-10">
      <!-- Section Header -->
      <div class="mb-24 text-center" v-motion-fade-visible-once>
        <span class="text-blue-500 font-bold tracking-widest text-xs uppercase mb-4 block">Expertise</span>
        <h2 class="text-4xl md:text-7xl font-extrabold text-gray-900 dark:text-white mb-6 tracking-tight">
          Technical <span class="bg-gradient-to-r from-[#e81cff] to-[#40c9ff] bg-clip-text text-transparent italic">Skills</span>
        </h2>
        <p class="text-xl text-gray-600 dark:text-gray-400 max-w-xl mx-auto leading-relaxed">
          Mastering modern technologies to build high-performance, forensic-level applications.
        </p>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-16 lg:gap-12 justify-items-center">
        <div 
          v-for="(category, index) in skillCategories" 
          :key="category.name"
          v-motion-slide-visible-bottom="{ delay: index * 100 }"
          class="neon-card group"
        >
          <!-- Category Header Top -->
          <div class="flex items-center justify-between mb-8 w-full">
             <h3 class="text-2xl font-black text-white dark:text-white uppercase tracking-tighter">{{ category.name }}</h3>
             <component :is="category.icon" class="w-8 h-8 text-[#40c9ff]" />
          </div>

          <!-- Skill List -->
          <div class="space-y-8 w-full">
            <div v-for="skill in category.skills" :key="skill.name" class="space-y-3 group/skill">
              <div class="flex justify-between items-end">
                <span class="text-sm font-bold text-gray-300 dark:text-gray-200 group-hover/skill:text-[#e81cff] transition-colors">
                  {{ skill.name }}
                </span>
                <span class="text-[10px] font-black text-gray-400 dark:text-gray-500 uppercase tracking-widest">
                  {{ skill.level }}%
                </span>
              </div>
              
              <div class="relative w-full bg-white/5 rounded-full h-1.5 overflow-hidden">
                <div 
                  class="h-full rounded-full bg-gradient-to-r from-[#e81cff] to-[#40c9ff] transition-all duration-1000 ease-out fill-glow"
                  :style="{ width: `${skill.level}%` }"
                ></div>
              </div>
            </div>
          </div>

          <!-- Bottom Branding -->
          <div class="mt-auto pt-8 w-full flex justify-between items-center border-t border-white/10">
             <span class="text-[10px] font-black text-[#e81cff] tracking-[0.2em] uppercase">Tech Stack 0{{ index + 1 }}</span>
             <Cpu class="w-5 h-5 text-gray-600" />
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.neon-card {
  position: relative;
  width: 100%;
  max-width: 400px;
  height: 580px;
  background-color: #000;
  display: flex;
  flex-direction: column;
  padding: 40px;
  border-radius: 12px;
  cursor: pointer;
  color: white;
  transition: all 0.3s ease;
}

/* Light mode override for card background */
:global(.light) .neon-card {
  background-color: #111827; /* Keeping it dark for the neon pop, or we can make it lighter if preferred */
}

.neon-card::before {
  content: '';
  position: absolute;
  inset: -4px; /* Border thickness */
  margin: auto;
  border-radius: 14px;
  background: linear-gradient(-45deg, #e81cff 0%, #40c9ff 100% );
  z-index: -10;
  pointer-events: none;
  transition: all 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275);
}

.neon-card::after {
  content: "";
  z-index: -1;
  position: absolute;
  inset: 0;
  background: linear-gradient(-45deg, #fc00ff 0%, #00dbde 100% );
  transform: translate3d(0, 0, 0) scale(0.95);
  filter: blur(20px);
  opacity: 0.5;
  transition: opacity 0.3s ease, filter 0.3s ease;
}

.neon-card:hover::after {
  filter: blur(40px);
  opacity: 0.8;
}

.neon-card:hover::before {
  transform: rotate(-90deg) scaleX(1.1) scaleY(0.85);
}

.fill-glow {
  box-shadow: 0 0 10px rgba(232, 28, 255, 0.3);
}

/* Mobile responsive */
@media (max-width: 768px) {
  .neon-card {
    height: 520px;
  }
}
</style>
