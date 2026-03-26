<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { ArrowRight, Code, Database, LayoutTemplate, BookOpen } from 'lucide-vue-next'

const heroProjects = ref([
  {
    name: 'This Portfolio',
    tech: 'VUE & TAILWIND',
    description: 'Dynamic portfolio with GitHub sync.',
    images: ['/portfolio-preview.png', '/portfolio-preview2.png'],
    activeImgIndex: 0,
    delay: '0s',
    position: 'top-[10%] -left-4 md:-left-28 w-48 md:w-64'
  },
  {
    name: 'OJT Manager',
    tech: 'JS & SQLITE',
    description: 'A premium DTR manager system.',
    images: ['/ojt-preview.png', '/ojt-preview2.png', '/ojt-preview3.png'], 
    activeImgIndex: 0,
    delay: '3.5s',
    position: 'bottom-[12%] -right-4 md:-right-24 w-48 md:w-64'
  }
])

let heroInterval = null

onMounted(() => {
  heroInterval = setInterval(() => {
    heroProjects.value.forEach(p => {
      if (p.images && p.images.length > 1) {
        p.activeImgIndex = (p.activeImgIndex + 1) % p.images.length
      }
    })
  }, 3500)
})

onUnmounted(() => {
  if (heroInterval) clearInterval(heroInterval)
})
</script>

<template>
  <section id="home" class="relative pt-24 pb-12 lg:pt-32 lg:pb-20 overflow-hidden flex items-center min-h-[90vh] lg:min-h-screen">
    <!-- Clean Background Elements -->
    <div class="absolute inset-0 z-0 bg-[linear-gradient(to_right,#80808012_1px,transparent_1px),linear-gradient(to_bottom,#80808012_1px,transparent_1px)] bg-[size:100px_100px] [mask-image:radial-gradient(ellipse_60%_50%_at_50%_0%,#000_70%,transparent_100%)]"></div>
    <div class="absolute inset-0 z-0">
      <div class="absolute top-20 left-10 w-96 h-96 bg-blue-500/10 dark:bg-blue-600/10 rounded-full blur-[100px] mix-blend-multiply dark:mix-blend-overlay animate-pulse"></div>
      <div class="absolute top-40 right-10 w-[40rem] h-[40rem] bg-purple-500/10 dark:bg-purple-600/10 rounded-full blur-[100px] mix-blend-multiply dark:mix-blend-overlay animate-pulse" style="animation-delay: 2s;"></div>
    </div>
    
    <div class="container mx-auto px-4 md:px-6 lg:px-8 max-w-7xl relative z-10">
      <div class="grid grid-cols-1 lg:grid-cols-2 gap-16 lg:gap-8 items-center">
        <!-- Left Side: Refined Typography & Clean Text -->
        <div class="flex flex-col items-start lg:pr-10" v-motion-slide-visible-left="{ delay: 200, duration: 800 }">
          
          <h1 class="text-5xl md:text-6xl lg:text-[5rem] font-extrabold tracking-tight text-gray-900 dark:text-white mb-4 leading-[1.1] drop-shadow-sm">
            Hi, I'm <br class="hidden sm:block" />
            <span class="text-transparent bg-clip-text bg-gradient-to-r from-blue-600 to-purple-600">Cris John</span>.
          </h1>
          
          <h2 class="text-2xl md:text-3xl font-bold text-gray-700 dark:text-gray-300 mb-8 border-l-4 border-blue-500 pl-5 py-1 tracking-wide">
            Full Stack Web Developer
          </h2>
          
          <p class="text-lg md:text-xl text-gray-600 dark:text-gray-400 mb-8 max-w-lg leading-relaxed">
            I build elegant, scalable, and user-friendly applications. Specializing in <strong class="text-gray-900 dark:text-gray-200 font-semibold">QR-based attendance systems</strong>, <strong class="text-gray-900 dark:text-gray-200 font-semibold">inventory management</strong>, <strong class="text-gray-900 dark:text-gray-200 font-semibold">POS solutions</strong>, <strong class="text-gray-900 dark:text-gray-200 font-semibold">portfolios</strong>, and <strong class="text-gray-900 dark:text-gray-200 font-semibold">DTR management apps</strong>.
          </p>
          
          <div class="flex flex-col sm:flex-row gap-5 mb-8 w-full">
            <a href="#projects" class="group inline-flex items-center justify-center px-8 py-4 text-base font-semibold rounded-xl text-white bg-blue-600 hover:bg-blue-700 shadow-lg shadow-blue-500/25 transition-all duration-300 hover:-translate-y-1">
              View Projects
              <ArrowRight class="ml-2 w-5 h-5 group-hover:translate-x-1 transition-transform" />
            </a>
            <a href="#contact" class="inline-flex items-center justify-center px-8 py-4 text-base font-semibold rounded-xl text-gray-800 dark:text-gray-200 bg-white/60 dark:bg-gray-800/60 backdrop-blur-md hover:bg-white dark:hover:bg-gray-800 border border-gray-200 dark:border-gray-700 shadow-sm transition-all duration-300 hover:-translate-y-1">
              Let's Connect
            </a>
          </div>

          <!-- Featured Research Highlight -->
          <a 
            href="https://www.researchgate.net/publication/401703874_LAMMS_A_Learner's_Attendance_Monitoring_and_Management_System_for_Naawan_Central_School_Misamis_Oriental_Philippines" 
            target="_blank" 
            class="group w-full max-w-lg flex items-center p-4 bg-white/40 dark:bg-blue-900/10 border border-blue-200 dark:border-blue-500/30 rounded-2xl hover:bg-white/70 dark:hover:bg-blue-900/30 transition-colors shadow-sm dark:shadow-none"
          >
            <div class="bg-blue-100 dark:bg-blue-600/20 p-3 rounded-xl mr-4">
              <BookOpen class="w-6 h-6 text-blue-600 dark:text-blue-400" />
            </div>
            <div>
              <p class="text-[10px] md:text-xs font-bold text-blue-600 dark:text-blue-400 uppercase tracking-widest mb-0.5">Featured Publication</p>
              <p class="text-sm md:text-base font-semibold text-gray-900 dark:text-gray-200 group-hover:text-blue-600 dark:group-hover:text-white transition-colors leading-tight">
                LAMMS: QR-Based Attendance Monitoring System
              </p>
            </div>
            <ArrowRight class="ml-auto w-5 h-5 text-gray-400 dark:text-gray-500 group-hover:text-blue-600 dark:group-hover:text-white group-hover:translate-x-1 transition-all" />
          </a>
        </div>

        <!-- Right Side: Ultra Premium Profile Image & Glass Projects -->
        <div class="relative w-full max-w-lg mx-auto mt-6 lg:mt-0" v-motion-fade-visible-once="{ delay: 500, duration: 1200 }">
          
          <!-- Speech Bubble -->
          <div class="absolute -top-10 left-1/2 -translate-x-1/2 z-30 pointer-events-none" v-motion-slide-visible-bottom="{ delay: 1500 }">
             <div class="relative flex items-center justify-center">
               <div class="bg-white dark:bg-gray-800 px-6 py-4 rounded-[2rem] shadow-2xl border border-gray-100 dark:border-gray-700/50 flex items-center gap-3 animate-bounce-slow">
                 <p class="text-sm md:text-base font-black text-gray-900 dark:text-white whitespace-nowrap tracking-tight">
                   "I'm ready for new opportunities!"
                 </p>
                 <!-- Speech Arrow -->
                 <div class="absolute -bottom-2 left-1/2 -translate-x-1/2 w-5 h-5 bg-white dark:bg-gray-800 border-r border-b border-gray-100 dark:border-gray-700/50 rotate-45 -z-10"></div>
               </div>
             </div>
          </div>

          <!-- Background Aura Behind Profile -->
          <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-[350px] h-[350px] rounded-full bg-gradient-to-tr from-blue-500/40 to-purple-500/40 blur-[80px] -z-10 absolute-glow"></div>
          
          <!-- Main Hero Image with Fading Mask at Bottom -->
          <div class="relative z-10 hero-image-wrapper">
            <img 
              src="/profile.png" 
              alt="Cris John" 
              class="w-full h-auto max-h-[600px] object-contain mx-auto transition-transform duration-700" 
              style="-webkit-mask-image: linear-gradient(to bottom, black 65%, transparent 100%); mask-image: linear-gradient(to bottom, black 65%, transparent 100%);"
            />
          </div>

          <!-- Floating Miniature Project Showcase Cards -->
          <div 
            v-for="(card, i) in heroProjects" 
            :key="i"
            :class="`absolute ${card.position} z-20 pointer-events-none hidden sm:flex`"
            :style="`animation: floatY 10s ease-in-out infinite; animation-delay: ${card.delay};`"
          >
            <div class="bg-white/80 dark:bg-gray-900/80 backdrop-blur-2xl border border-white dark:border-gray-700 rounded-xl overflow-hidden shadow-[0_20px_40px_rgba(0,0,0,0.15)] dark:shadow-[0_20px_40px_rgba(0,0,0,0.5)] flex flex-col transform transition-all duration-500 hover:scale-105 pointer-events-auto cursor-default">
              
              <!-- Auto-Sliding Image Area -->
              <div class="h-24 md:h-32 relative overflow-hidden bg-gray-200 dark:bg-gray-800">
                <img 
                  :src="card.images[card.activeImgIndex]" 
                  :alt="card.name"
                  class="w-full h-full object-cover object-top transition-transform duration-700 scale-100"
                />
                <div class="absolute inset-0 bg-blue-600/0 hover:bg-blue-600/10 transition-colors duration-300"></div>

                <!-- Tiny Dots Indicator -->
                <div v-if="card.images.length > 1" class="absolute bottom-1.5 left-0 right-0 flex justify-center gap-1">
                  <div v-for="(_, imgIdx) in card.images" :key="imgIdx" 
                       :class="['w-1 h-1 rounded-full transition-all duration-300', card.activeImgIndex === imgIdx ? 'bg-white scale-125 shadow-sm' : 'bg-white/50']">
                  </div>
                </div>
              </div>
              
              <!-- Project Details -->
              <div class="p-3 md:p-4">
                <p class="text-[13px] md:text-[15px] font-extrabold tracking-tight text-gray-900 dark:text-white leading-none mb-1">{{ card.name }}</p>
                <p class="text-[9px] md:text-[10px] font-bold text-blue-600 dark:text-blue-400 uppercase tracking-widest mb-1.5">{{ card.tech }}</p>
                <p class="text-[10px] md:text-[11px] text-gray-600 dark:text-gray-400 leading-snug line-clamp-2">
                  {{ card.description }}
                </p>
              </div>

            </div>
          </div>
          
        </div>
      </div>
    </div>
  </section>
</template>

<style scoped>
@keyframes floatY {
  0% { transform: translateY(0px) rotate(0deg); }
  50% { transform: translateY(-15px) rotate(2deg); }
  100% { transform: translateY(0px) rotate(0deg); }
}

.hero-image-wrapper {
  animation: floatY 6s ease-in-out infinite;
}

.absolute-glow {
  animation: pulse-glow 8s ease-in-out infinite;
}

@keyframes pulse-glow {
  0%, 100% { opacity: 0.5; transform: translate(-50%, -50%) scale(1); }
  50% { opacity: 0.8; transform: translate(-50%, -50%) scale(1.1); }
}

@keyframes bounce-slow {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

.animate-bounce-slow {
  animation: bounce-slow 3s ease-in-out infinite;
}
</style>
