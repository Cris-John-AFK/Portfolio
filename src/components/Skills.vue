<script setup>
import { computed, ref, onMounted } from 'vue'
import { Monitor, Server, Terminal, Hammer, Cpu, Layers } from 'lucide-vue-next'

const skillCategoriesRaw = [
  {
    name: 'Frontend',
    icon: Monitor,
    skills: [
      { name: 'Vue.js', level: 95 },
      { name: 'React', level: 85 },
      { name: 'HTML & CSS', level: 95 },
      { name: 'TailwindCSS', level: 95 },
      { name: 'JavaScript / ES6+', level: 90 }
    ]
  },
  {
    name: 'Backend & APIs',
    icon: Server,
    skills: [
      { name: 'Node.js', level: 85 },
      { name: 'Laravel', level: 80 },
      { name: 'REST APIs', level: 90 },
      { name: 'Firebase', level: 90 },
      { name: 'SQLite/SQL', level: 85 }
    ]
  },
  {
    name: 'Tools & DevOps',
    icon: Terminal,
    skills: [
      { name: 'Git & GitHub', level: 95 },
      { name: 'VS Code', level: 95 },
      { name: 'Vite / Webpack', level: 85 },
      { name: 'Vercel / Netlify', level: 90 },
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

// Interactive mouse effect for cards
const cardRefs = ref([])
const mousePositions = ref([])

onMounted(() => {
  mousePositions.value = skillCategories.value.map(() => ({ x: 0, y: 0 }))
})

const handleMouseMove = (index, e) => {
  const card = cardRefs.value[index]
  if (!card) return
  
  const rect = card.getBoundingClientRect()
  const x = (e.clientX - rect.left) / rect.width - 0.5
  const y = (e.clientY - rect.top) / rect.height - 0.5
  
  mousePositions.value[index] = { x, y }
}

const resetMouse = (index) => {
  mousePositions.value[index] = { x: 0, y: 0 }
}

const getCardStyle = (index) => {
  const pos = mousePositions.value[index] || { x: 0, y: 0 }
  return {
    transform: `perspective(1000px) rotateY(${pos.x * 12}deg) rotateX(${pos.y * -12}deg) scale3d(1.02, 1.02, 1.02)`,
    transition: 'transform 0.15s ease-out'
  }
}
</script>

<template>
  <section id="skills" class="py-24 lg:py-36 bg-gray-50 dark:bg-[#030712] relative overflow-hidden transition-colors duration-500">
    <!-- Animated Background Blobs -->
    <div class="absolute top-0 left-1/4 w-96 h-96 bg-blue-500/10 blur-[120px] rounded-full animate-blob"></div>
    <div class="absolute bottom-0 right-1/4 w-96 h-96 bg-purple-500/10 blur-[120px] rounded-full animate-blob animation-delay-2000"></div>

    <div class="container mx-auto px-4 md:px-6 lg:px-8 max-w-7xl relative z-10">
      <!-- Section Header -->
      <div class="mb-24 text-center" v-motion-fade-visible-once>
        <span class="text-blue-500 font-bold tracking-widest text-xs uppercase mb-4 block">Expertise</span>
        <h2 class="text-4xl md:text-7xl font-extrabold text-gray-900 dark:text-white mb-6 tracking-tight">
          Technical <span class="bg-gradient-to-r from-blue-500 to-indigo-500 bg-clip-text text-transparent italic">Skills</span>
        </h2>
        <p class="text-xl text-gray-600 dark:text-gray-400 max-w-xl mx-auto leading-relaxed">
          Mastering modern technologies to build high-performance, forensic-level applications.
        </p>
      </div>

      <div class="grid grid-cols-1 lg:grid-cols-3 gap-8 lg:gap-12">
        <div 
          v-for="(category, index) in skillCategories" 
          :key="category.name"
          ref="cardRefs"
          v-motion-slide-visible-bottom="{ delay: index * 100 }"
          @mousemove="handleMouseMove(index, $event)"
          @mouseleave="resetMouse(index)"
          class="group relative bg-white dark:bg-gray-900/50 backdrop-blur-xl rounded-[2.5rem] p-10 border border-gray-200 dark:border-white/5 shadow-2xl transition-all duration-300"
          :style="getCardStyle(index)"
        >
          <!-- Category Header -->
          <div class="flex items-center gap-5 mb-12">
            <div class="w-14 h-14 rounded-2xl flex items-center justify-center bg-blue-500/10 dark:bg-blue-500/20 text-blue-500 shadow-inner">
              <component :is="category.icon" class="w-7 h-7" />
            </div>
            <h3 class="text-2xl font-black text-gray-900 dark:text-white tracking-tight">{{ category.name }}</h3>
          </div>
          
          <!-- Skill List -->
          <div class="space-y-8">
            <div v-for="skill in category.skills" :key="skill.name" class="space-y-3 group/skill">
              <div class="flex justify-between items-end">
                <span class="text-base font-bold text-gray-700 dark:text-gray-200 group-hover/skill:text-blue-500 transition-colors">
                  {{ skill.name }}
                </span>
                <span class="text-[10px] font-black text-gray-400 dark:text-gray-500 uppercase tracking-widest">
                  {{ skill.level }}%
                </span>
              </div>
              
              <!-- Progress Bar Wrapper -->
              <div class="relative w-full bg-gray-200/50 dark:bg-white/5 rounded-full h-2.5 overflow-hidden">
                <!-- Glowing Fill -->
                <div 
                  class="h-full rounded-full bg-gradient-to-r from-blue-600 via-indigo-500 to-purple-600 relative overflow-hidden transition-all duration-1000 ease-out fill-animation"
                  :style="{ width: `${skill.level}%` }"
                  v-motion-slide-left
                >
                  <!-- Light Shine -->
                  <div class="absolute inset-0 bg-gradient-to-r from-transparent via-white/20 to-transparent skew-x-[-20deg] animate-shine"></div>
                </div>
              </div>
            </div>
          </div>

          <!-- Bottom Decorative Element -->
          <div class="absolute bottom-6 right-10 opacity-0 group-hover:opacity-100 transition-opacity duration-500">
             <Cpu class="w-8 h-8 text-blue-500/20 dark:text-blue-400/10" />
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
@keyframes blob {
  0% { transform: translate(0px, 0px) scale(1); }
  33% { transform: translate(30px, -50px) scale(1.1); }
  66% { transform: translate(-20px, 20px) scale(0.9); }
  100% { transform: translate(0px, 0px) scale(1); }
}

.animate-blob {
  animation: blob 7s infinite alternate ease-in-out;
}

.animation-delay-2000 {
  animation-delay: 2s;
}

@keyframes shine {
  0% { transform: translateX(-100%) skewX(-20deg); }
  50% { transform: translateX(100%) skewX(-20deg); }
  100% { transform: translateX(100%) skewX(-20deg); }
}

.animate-shine {
  animation: shine 3s infinite ease-in-out;
}

.fill-animation {
  box-shadow: 0 0 15px rgba(59, 130, 246, 0.4);
}
</style>
