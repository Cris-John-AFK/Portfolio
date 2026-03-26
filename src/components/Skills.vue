<script setup>
import { computed } from 'vue'
import { Monitor, Server, Terminal, Cpu } from 'lucide-vue-next'

const skillCategoriesRaw = [
  {
    name: 'Frontend',
    icon: Monitor,
    rotation: -15,
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
    rotation: 5,
    skills: [
      { name: 'Node.js', level: 85 },
      { name: 'Laravel', level: 80 },
      { name: 'Firebase', level: 90 },
      { name: 'SQL', level: 85 }
    ]
  },
  {
    name: 'DevOps',
    icon: Terminal,
    rotation: 25,
    skills: [
      { name: 'Git/GitHub', level: 95 },
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
  <section id="skills" class="py-24 lg:py-48 bg-gray-50 dark:bg-[#030712] relative overflow-hidden transition-colors duration-500 flex flex-col items-center">
    <!-- Section Header -->
    <div class="mb-24 text-center px-4" v-motion-fade-visible-once>
      <span class="text-blue-500 font-bold tracking-widest text-[10px] uppercase mb-4 block">Expertise</span>
      <h2 class="text-4xl md:text-6xl font-black text-gray-900 dark:text-white mb-6">
        Technical <span class="bg-gradient-to-r from-blue-500 to-indigo-500 bg-clip-text text-transparent italic">Skills</span>
      </h2>
    </div>

    <!-- Uiverse Stacking Glass Container -->
    <div class="glass-stack-container" v-motion-fade-visible-once>
      <div 
        v-for="(category, index) in skillCategories" 
        :key="category.name"
        :data-text="category.name"
        :style="{ '--r': category.rotation }"
        class="glass-card"
      >
        <!-- Card Content -->
        <div class="card-inner">
          <div class="flex items-center justify-between mb-10">
            <component :is="category.icon" class="w-10 h-10 text-blue-500" />
            <span class="text-xs font-black uppercase text-gray-400 dark:text-gray-500 tracking-tighter">
               0{{ index + 1 }}
            </span>
          </div>
          
          <div class="space-y-8">
            <div v-for="skill in category.skills" :key="skill.name" class="space-y-3">
              <div class="flex justify-between text-xs md:text-sm font-bold">
                <span class="text-gray-800 dark:text-gray-200 tracking-tight">{{ skill.name }}</span>
                <span class="text-blue-500">{{ skill.level }}%</span>
              </div>
              <div class="w-full bg-gray-300/30 dark:bg-white/5 rounded-full h-1.5 overflow-hidden">
                <div 
                  class="h-full bg-blue-500 transition-all duration-1000 ease-out shadow-[0_0_15px_rgba(59,130,246,0.5)]"
                  :style="{ width: `${skill.level}%` }"
                ></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
.glass-stack-container {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 120px 0;
  width: 100%;
}

.glass-card {
  position: relative;
  width: 360px; /* Scaled up from 280px */
  height: 520px; /* Scaled up from 340px */
  background: linear-gradient(rgba(255, 255, 255, 0.1), transparent);
  border: 1px solid rgba(255, 255, 255, 0.1);
  box-shadow: 0 40px 80px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
  padding: 45px;
  transition: 0.6s cubic-bezier(0.23, 1, 0.32, 1);
  border-radius: 36px;
  margin: 0 -140px; /* Adjusted overlap for larger cards */
  backdrop-filter: blur(20px);
  transform: rotate(calc(var(--r) * 1deg));
  z-index: 1;
}

/* Light mode adjustments */
:global(.light) .glass-card {
  background: linear-gradient(rgba(0, 0, 0, 0.05), transparent);
  border: 1px solid rgba(0, 0, 0, 0.05);
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
}

.glass-stack-container:hover .glass-card {
  transform: rotate(0deg) translateY(-20px);
  margin: 0 15px;
  z-index: 10;
}

.glass-card::before {
  content: attr(data-text);
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 50px;
  background: rgba(255, 255, 255, 0.05);
  display: flex;
  justify-content: center;
  align-items: center;
  color: #fff;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 2px;
  font-size: 12px;
  border-bottom-left-radius: 24px;
  border-bottom-right-radius: 24px;
  opacity: 0.8;
}

:global(.light) .glass-card::before {
  background: rgba(0, 0, 0, 0.05);
  color: #1f2937;
}

.card-inner {
  width: 100%;
  height: calc(100% - 40px);
}

.fill-glow {
  box-shadow: 0 0 10px rgba(59, 130, 246, 0.5);
}

/* Mobile responsive */
@media (max-width: 768px) {
  .glass-stack-container {
    flex-direction: column;
    margin-top: 50px;
  }
  .glass-card {
    margin: -80px 0;
    width: 320px;
  }
  .glass-stack-container:hover .glass-card {
    transform: rotate(0deg) translateY(0);
    margin: 10px 0;
  }
}
</style>
