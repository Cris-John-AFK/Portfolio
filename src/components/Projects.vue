<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { ExternalLink, Code2, Loader2, ChevronLeft, ChevronRight, X, MessageSquare, Briefcase, BookOpen, FileText } from 'lucide-vue-next'
import { contactStore } from '../store'

const loading = ref(false) // Set to false since we are using hardcoded data now

// Selected project for the enlargement modal
const selectedProject = ref(null)

const openModal = (project) => {
  selectedProject.value = JSON.parse(JSON.stringify(project)) // Clone to avoid direct mutation
  document.body.style.overflow = 'hidden' // Prevent scroll
}

const closeModal = () => {
  selectedProject.value = null
  document.body.style.overflow = '' // Restore scroll
}

const handleInquiry = () => {
  if (selectedProject.value) {
    contactStore.setInquiry(selectedProject.value.name)
    closeModal()
  }
}

// Static featured projects: High-end curated versions
const projects = ref([
  {
    id: 0,
    number: '01',
    category: 'RESEARCH PAPER',
    accentColor: '#A855F7', // Purple
    name: 'LAMMS Research Project',
    originalName: 'LAMMS-Attendance-System',
    description: 'A study on Learner\'s Attendance Monitoring using QR technology. Features real-time tracking and automated SF2 report generation.',
    url: '', 
    researchUrl: 'https://www.researchgate.net/publication/401703874_LAMMS_A_Learner\'s_Attendance_Monitoring_and_Management_System_for_Naawan_Central_School_Misamis_Oriental_Philippines',
    homepage: '',
    language: 'PHP & JavaScript',
    topics: ['QR Technology', 'Academic Research', 'Automation'],
    images: ['/dashboard.jpg', '/report.jpg'],
    activeImgIndex: 0
  },
  {
    id: 1,
    number: '02',
    category: 'WEB APPLICATION',
    accentColor: '#3B82F6', // Blue
    name: 'My Portfolio V2',
    originalName: 'My-Portfolio',
    description: 'State-of-the-art portfolio featuring glassmorphism, dynamic project modals, and automated GitHub integrations.',
    url: 'https://github.com/Cris-John-AFK/My-Portfolio',
    homepage: 'https://crisjohn-portfolio.vercel.app',
    language: 'Vue 3',
    topics: ['Tailwind v4', 'Vite', 'Frontend'],
    images: ['/portfolio-preview.png', '/portfolio-preview2.png'],
    activeImgIndex: 0
  },
  {
    id: 2,
    number: '03',
    category: 'DESKTOP APP',
    accentColor: '#F97316', // Orange
    name: 'OJT Manager App',
    originalName: 'OJT_Manager_APP',
    description: 'High-performance DTR tracking system built for speed and reliability in institutional environments.',
    url: 'https://github.com/Cris-John-AFK/OJT_Manager_APP',
    homepage: '',
    language: 'JavaScript',
    topics: ['SQLite', 'DTR', 'Management'],
    images: ['/ojt-preview.png', '/ojt-preview2.png', '/ojt-preview3.png', '/ojt-preview4.png'],
    activeImgIndex: 0
  },
  {
    id: 3,
    number: '04',
    category: 'ENTERPRISE SYSTEM',
    accentColor: '#10B981', // Emerald
    name: 'HR Leave Monitoring',
    originalName: 'HR-Leave-Monitoring',
    description: 'Enterprise resource planning for employee leave tracking and payroll-ready attendance logs.',
    url: 'https://github.com/Cris-John-AFK/HR-Leave-Monitoring',
    homepage: '',
    language: 'Laravel & Vue',
    topics: ['PostgreSQL', 'HRIS', 'Admin'],
    images: ['/hr-preview.png', '/hr-preview2.png', '/hr-preview3.png', '/hr-preview4.png'],
    activeImgIndex: 0
  },
  {
    id: 4,
    number: '05',
    category: 'AUTOMATION SYSTEM',
    accentColor: '#F43F5E', // Rose
    name: 'QR Event Attendance',
    originalName: 'Event-Attendance-System',
    description: 'Real-time event check-in system with built-in gamification (Wheel of Names) for huge gatherings.',
    url: 'https://github.com/Cris-John-AFK/Event-Attendance-System',
    homepage: '',
    language: 'Vue & Firebase',
    topics: ['QR-Code', 'Automation', 'Real-time'],
    images: ['/event-preview.png', '/event-preview2.png', '/event-preview3.png', '/event-preview4.png', '/event-preview5.png', '/event-preview6.png'],
    activeImgIndex: 0
  }
])

// Carousel Navigation Methods
const nextImage = (project) => {
  project.activeImgIndex = (project.activeImgIndex + 1) % project.images.length
}

const prevImage = (project) => {
  project.activeImgIndex = (project.activeImgIndex - 1 + project.images.length) % project.images.length
}

let autoSlideInterval = null

onMounted(() => {
  loading.value = false 
  
  autoSlideInterval = setInterval(() => {
    projects.value.forEach(p => {
      if (!selectedProject.value || selectedProject.value.id !== p.id) {
        if (p.images && p.images.length > 1) {
          nextImage(p)
        }
      }
    })
  }, 3500)
})

onUnmounted(() => {
  if (autoSlideInterval) clearInterval(autoSlideInterval)
})
</script>

<template>
  <section id="projects" class="py-24 lg:py-36 bg-[#030712] relative overflow-hidden">
    <!-- Dot Pattern Background -->
    <div class="absolute inset-0 dot-pattern opacity-10"></div>
    
    <div class="container mx-auto px-4 md:px-6 lg:px-8 max-w-7xl relative z-10">
      <!-- Section Header -->
      <div class="mb-24 text-center" v-motion-fade-visible-once>
        <span class="text-blue-500 font-bold tracking-widest text-xs uppercase mb-4 block">Portfolio</span>
        <h2 class="text-4xl md:text-7xl font-extrabold text-white mb-6 tracking-tight">
          Featured <span class="bg-gradient-to-r from-purple-500 to-pink-500 bg-clip-text text-transparent italic">Projects</span>
        </h2>
        <p class="text-lg text-gray-400 max-w-xl mx-auto">
          A curated selection of projects that made me confident in building software.
        </p>
      </div>

      <div v-if="loading" class="flex justify-center items-center py-20">
        <Loader2 class="w-10 h-10 animate-spin text-blue-500" />
      </div>

      <!-- Project Grid -->
      <div v-else class="grid grid-cols-1 lg:grid-cols-2 gap-x-12 gap-y-24">
        <div 
          v-for="(project, index) in projects" 
          :key="project.id"
          class="flex flex-col group cursor-pointer"
          v-motion-slide-visible-bottom="{ delay: index * 100 }"
          @click="openModal(project)"
        >
          <!-- Metadata Bar -->
          <div class="flex items-center space-x-4 mb-6 text-xs font-bold tracking-tighter text-gray-500 uppercase">
            <span>{{ project.number }}</span>
            <div class="h-[1px] w-8 bg-gray-800"></div>
            <span>{{ project.category }}</span>
          </div>

          <!-- Project Title -->
          <h3 class="text-3xl md:text-5xl font-black text-white mb-8 group-hover:translate-x-2 transition-transform duration-300">
            {{ project.name }}
          </h3>

          <!-- Window Card -->
          <div 
            class="relative rounded-[2.5rem] p-8 md:p-12 overflow-hidden transition-all duration-500 group-hover:-translate-y-2"
            :style="{ backgroundColor: project.accentColor + '10' }" 
          >
            <!-- Background Accent Glow -->
            <div class="absolute -top-24 -right-24 w-64 h-64 blur-[120px] opacity-20 transition-all duration-700 group-hover:scale-150"
                 :style="{ backgroundColor: project.accentColor }"></div>

            <p class="text-gray-300 text-lg md:text-xl font-medium mb-12 max-w-md leading-relaxed">
              {{ project.description }}
            </p>

            <!-- Browser Mockup -->
            <div class="relative bg-gray-900 border border-white/10 rounded-2xl shadow-2xl overflow-hidden aspect-video group/window">
              <!-- Window Bar -->
              <div class="h-8 bg-gray-800/50 backdrop-blur-md border-b border-white/5 px-4 flex items-center space-x-2">
                <div class="w-2.5 h-2.5 rounded-full bg-red-500/50"></div>
                <div class="w-2.5 h-2.5 rounded-full bg-yellow-500/50"></div>
                <div class="w-2.5 h-2.5 rounded-full bg-green-500/50"></div>
              </div>
              
              <!-- Content -->
              <div class="relative w-full h-full bg-black/20">
                <img 
                  :src="project.images[project.activeImgIndex]" 
                  :alt="project.name"
                  class="w-full h-full object-cover object-top transition-transform duration-1000 group-hover/window:scale-105"
                />
                
                <!-- Overlay Gradient -->
                <div class="absolute inset-0 bg-gradient-to-t from-gray-950/40 to-transparent"></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Immersive Project Detail Modal -->
    <Teleport to="body">
      <div v-if="selectedProject" 
           class="fixed inset-0 z-[100] flex items-center justify-center p-4 md:p-8 overflow-hidden"
           v-motion-fade>
        
        <!-- Backdrop -->
        <div class="absolute inset-0 bg-gray-950/90 backdrop-blur-xl" @click="closeModal"></div>
        
        <!-- Modal Content -->
        <div class="relative bg-gray-900 w-full max-w-6xl max-h-full rounded-[3rem] overflow-hidden shadow-2xl flex flex-col lg:flex-row border border-white/10"
             v-motion-slide-bottom>
          
          <button @click="closeModal" class="absolute top-6 right-6 z-50 p-2 rounded-full bg-white/5 hover:bg-white/10 text-white transition-colors">
            <X class="w-6 h-6" />
          </button>

          <!-- Modal Left Side (Visuals) -->
          <div class="lg:w-7/12 relative bg-gray-950 flex items-center justify-center border-b lg:border-b-0 lg:border-r border-white/5 min-h-[300px] md:min-h-[500px]">
            <img 
              :src="selectedProject.images[selectedProject.activeImgIndex]" 
              :alt="selectedProject.name"
              class="w-full h-full object-contain p-4 md:p-8 lg:p-12"
            />
            
            <div v-if="selectedProject.images.length > 1" class="absolute inset-0 flex items-center justify-between px-6">
              <button @click="prevImage(selectedProject)" class="p-4 rounded-full bg-white/5 hover:bg-white/15 text-white backdrop-blur-md transition-all">
                <ChevronLeft class="w-6 h-6" />
              </button>
              <button @click="nextImage(selectedProject)" class="p-4 rounded-full bg-white/5 hover:bg-white/15 text-white backdrop-blur-md transition-all">
                <ChevronRight class="w-6 h-6" />
              </button>
            </div>
          </div>

          <!-- Modal Right Side (Content) -->
          <div class="lg:w-5/12 p-8 md:p-14 overflow-y-auto bg-gray-900 flex flex-col justify-center">
            <div class="flex items-center space-x-3 mb-10">
              <div class="w-12 h-12 rounded-2xl flex items-center justify-center" 
                   :style="{ backgroundColor: selectedProject.accentColor + '20' }">
                <Briefcase class="w-6 h-6" :style="{ color: selectedProject.accentColor }" />
              </div>
              <div>
                <span class="block text-[10px] font-black uppercase tracking-tighter text-gray-500">Project Highlights</span>
                <span class="text-white font-bold text-sm">{{ selectedProject.category }}</span>
              </div>
            </div>

            <h3 class="text-4xl md:text-5xl font-black text-white mb-6 leading-[0.9]">
              {{ selectedProject.name }}
            </h3>
            
            <div class="flex flex-wrap gap-2 mb-10">
              <span v-for="topic in selectedProject.topics" :key="topic" class="px-3 py-1 text-[10px] font-black uppercase tracking-widest bg-white/5 text-gray-400 rounded-full border border-white/5">
                {{ topic }}
              </span>
            </div>

            <p class="text-xl text-gray-400 mb-12 leading-relaxed">
              {{ selectedProject.description }}
            </p>

            <div class="flex flex-col space-y-4 mt-auto">
              <a 
                v-if="selectedProject.researchUrl" 
                :href="selectedProject.researchUrl" 
                target="_blank"
                class="flex items-center justify-center px-8 py-5 bg-gradient-to-r from-purple-600 to-pink-600 text-white rounded-[2rem] font-bold transition-all shadow-xl shadow-purple-500/20 hover:-translate-y-1"
              >
                <FileText class="w-5 h-5 mr-3" />
                Read Publication
              </a>

              <button 
                @click="handleInquiry"
                class="flex items-center justify-center px-8 py-5 bg-white text-gray-900 rounded-[2rem] font-bold transition-all hover:bg-gray-100 hover:-translate-y-1"
              >
                <MessageSquare class="w-5 h-5 mr-3" />
                Discuss Project
              </button>
              
              <div class="grid grid-cols-2 gap-4">
                <a 
                  v-if="selectedProject.homepage" 
                  :href="selectedProject.homepage" 
                  target="_blank"
                  class="flex items-center justify-center p-5 bg-gray-800 text-white rounded-[2rem] font-bold transition-all hover:bg-gray-750"
                >
                  <ExternalLink class="w-5 h-5 mr-2" /> 
                  Demo
                </a>
                <a 
                  v-if="selectedProject.url && (selectedProject.name.toLowerCase().includes('portfolio') || selectedProject.name.toLowerCase().includes('ojt'))" 
                  :href="selectedProject.url" 
                  target="_blank"
                  class="flex items-center justify-center p-5 bg-gray-800 text-white rounded-[2rem] font-bold transition-all hover:bg-gray-750"
                >
                  <Code2 class="w-5 h-5 mr-2" /> 
                  Code
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </Teleport>
  </section>
</template>

<style scoped>
.dot-pattern {
  background-image: radial-gradient(rgba(255, 255, 255, 0.15) 1px, transparent 1px);
  background-size: 40px 40px;
}
</style>
