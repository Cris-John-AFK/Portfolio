<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { ExternalLink, Code2, Loader2, ChevronLeft, ChevronRight, X, MessageSquare, Briefcase } from 'lucide-vue-next'
import { contactStore } from '../store'

const projects = ref([])
const loading = ref(true)
const githubUsername = 'Cris-John-AFK'

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

// Custom image mapping remain same
const customImages = {
  'This-Portfolio': ['/portfolio-preview.png', '/portfolio-preview2.png'],
  'My-Portfolio': ['/portfolio-preview.png', '/portfolio-preview2.png'],
  'Portfolio': ['/portfolio-preview.png', '/portfolio-preview2.png'],
  'OJT_Manager_APP': ['/ojt-preview.png', '/ojt-preview2.png', '/ojt-preview3.png', '/ojt-preview4.png'],
  'HR-Leave-Monitoring': ['/hr-preview.png', '/hr-preview2.png', '/hr-preview3.png', '/hr-preview4.png'],
  'Event-Attendance-with-wheel-of-names-and-slot-machine-system': [
    '/event-preview.png', 
    '/event-preview2.png', 
    '/event-preview3.png', 
    '/event-preview4.png', 
    '/event-preview5.png', 
    '/event-preview6.png'
  ]
}

const fetchProjects = async () => {
  try {
    loading.value = true
    const res = await fetch(`https://api.github.com/users/${githubUsername}/repos?sort=updated&per_page=6`)
    if (res.ok) {
      const data = await res.json()
      
      projects.value = data
        .filter(repo => !repo.fork)
        .map(repo => {
          let imgs = []
          if (customImages[repo.name]) {
            imgs = Array.isArray(customImages[repo.name]) ? customImages[repo.name] : [customImages[repo.name]]
          } else {
            imgs = [`https://opengraph.githubassets.com/1/${repo.owner.login}/${repo.name}`]
          }
          
          return {
            id: repo.id,
            name: repo.name.replace(/-/g, ' '),
            originalName: repo.name,
            ownerLogin: repo.owner.login,
            description: repo.description || 'A web development project built with modern technologies.',
            url: repo.html_url,
            homepage: repo.homepage,
            language: repo.language,
            topics: repo.topics || [],
            images: imgs,
            activeImgIndex: 0
          }
        })
    }
  } catch (err) {
    console.error('Failed to fetch projects', err)
  } finally {
    loading.value = false
  }
}

// Carousel Navigation Methods
const nextImage = (project) => {
  project.activeImgIndex = (project.activeImgIndex + 1) % project.images.length
}

const prevImage = (project) => {
  project.activeImgIndex = (project.activeImgIndex - 1 + project.images.length) % project.images.length
}

let autoSlideInterval = null

onMounted(() => {
  fetchProjects()
  
  autoSlideInterval = setInterval(() => {
    // Only auto-slide background projects, not the one in the modal
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
  <section id="projects" class="py-20 lg:py-32 bg-gray-50 dark:bg-gray-900">
    <div class="container mx-auto px-4 md:px-6 lg:px-8 max-w-7xl">
      <div class="mb-16 md:text-center" v-motion-fade-visible-once>
        <h2 class="text-3xl md:text-5xl font-bold tracking-tight text-gray-900 dark:text-white mb-4 border-b-2 border-blue-500 pb-2 inline-block">
          Featured Projects
        </h2>
        <p class="text-lg text-gray-600 dark:text-gray-400 max-w-2xl mx-auto mt-6">
          A showcase of my recent work fetched dynamically from GitHub. Click any project to view details.
        </p>
      </div>

      <div v-if="loading" class="flex justify-center items-center py-20">
        <Loader2 class="w-10 h-10 animate-spin text-blue-500" />
      </div>

      <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
        <div 
          v-for="(project, index) in projects" 
          :key="project.id"
          v-motion-slide-visible-bottom="{ delay: index * 100 }"
          class="group bg-white dark:bg-gray-800 rounded-2xl overflow-hidden shadow-sm hover:shadow-xl transition-all duration-300 border border-gray-100 dark:border-gray-700 flex flex-col h-full cursor-pointer"
          @click="openModal(project)"
        >
          <!-- Dynamic Project Preview Image / Carousel -->
          <div class="h-48 relative overflow-hidden flex items-center justify-center bg-gray-100 dark:bg-gray-900 group/carousel">
            <img 
              :src="project.images[project.activeImgIndex]" 
              :alt="project.name"
              class="w-full h-full object-cover object-top group-hover:scale-105 transition-transform duration-500"
              loading="lazy"
            />
            <div class="absolute inset-0 bg-blue-600/0 group-hover:bg-blue-600/10 transition-colors duration-300"></div>

            <div v-if="project.images.length > 1" class="absolute inset-0 flex items-center justify-between px-2 opacity-0 group-hover/carousel:opacity-100 transition-opacity duration-300">
              <button @click.stop="prevImage(project)" class="p-1.5 rounded-full bg-black/40 text-white hover:bg-black/70 backdrop-blur-md transition-colors cursor-pointer z-10">
                <ChevronLeft class="w-5 h-5" />
              </button>
              <button @click.stop="nextImage(project)" class="p-1.5 rounded-full bg-black/40 text-white hover:bg-black/70 backdrop-blur-md transition-colors cursor-pointer z-10">
                <ChevronRight class="w-5 h-5" />
              </button>
            </div>
          </div>
          
          <div class="p-6 flex flex-col flex-grow">
            <h3 class="text-xl font-bold text-gray-900 dark:text-white mb-2 capitalize">{{ project.name }}</h3>
            <p class="text-gray-600 dark:text-gray-400 mb-6 text-sm flex-grow line-clamp-3">
              {{ project.description }}
            </p>
            
            <div class="flex flex-wrap gap-2 mb-6">
              <span v-if="project.language" class="px-2.5 py-1 text-xs font-medium bg-blue-50 text-blue-600 dark:bg-blue-900/30 dark:text-blue-400 rounded-md">
                {{ project.language }}
              </span>
            </div>
            
            <div class="mt-auto flex gap-3 pt-4 border-t border-gray-100 dark:border-gray-700">
              <span class="text-sm font-semibold text-blue-600 dark:text-blue-400 group-hover:underline">View Details</span>
              <Code2 class="w-4 h-4 ml-auto text-gray-400" />
            </div>
          </div>
        </div>
      </div>
      
    </div>

    <!-- Project Detail Modal -->
    <Teleport to="body">
      <div v-if="selectedProject" 
           class="fixed inset-0 z-[100] flex items-center justify-center p-4 md:p-8 overflow-hidden"
           v-motion-fade>
        
        <!-- Backdrop -->
        <div class="absolute inset-0 bg-gray-950/80 backdrop-blur-sm" @click="closeModal"></div>
        
        <!-- Modal Content -->
        <div class="relative bg-white dark:bg-gray-900 w-full max-w-6xl max-h-full rounded-3xl overflow-hidden shadow-2xl flex flex-col lg:flex-row shadow-blue-500/10 border border-gray-200 dark:border-gray-800"
             v-motion-slide-bottom>
          
          <!-- Close Button Mobile -->
          <button @click="closeModal" class="absolute top-4 right-4 z-50 p-2 rounded-full bg-gray-100/80 dark:bg-gray-800/80 text-gray-900 dark:text-white lg:hidden">
            <X class="w-6 h-6" />
          </button>

          <!-- Left: Big Slider -->
          <div class="lg:w-3/5 h-[300px] md:h-[450px] lg:h-full relative bg-gray-100 dark:bg-gray-950 flex items-center justify-center group/modal-slider">
            <img 
              :src="selectedProject.images[selectedProject.activeImgIndex]" 
              :alt="selectedProject.name"
              class="w-full h-full object-contain p-4 md:p-8"
            />
            
            <div v-if="selectedProject.images.length > 1" class="absolute inset-0 flex items-center justify-between px-4 md:px-6">
              <button @click="prevImage(selectedProject)" class="p-3 rounded-full bg-white/20 hover:bg-white/40 text-white backdrop-blur-md transition-all shadow-lg">
                <ChevronLeft class="w-6 h-6" />
              </button>
              <button @click="nextImage(selectedProject)" class="p-3 rounded-full bg-white/20 hover:bg-white/40 text-white backdrop-blur-md transition-all shadow-lg">
                <ChevronRight class="w-6 h-6" />
              </button>
            </div>

            <!-- Dots -->
            <div v-if="selectedProject.images.length > 1" class="absolute bottom-6 left-0 right-0 flex justify-center gap-2">
              <button v-for="(_, idx) in selectedProject.images" :key="idx"
                      @click="selectedProject.activeImgIndex = idx"
                      :class="['w-2 h-2 rounded-full transition-all', selectedProject.activeImgIndex === idx ? 'bg-blue-600 w-6' : 'bg-gray-400']">
              </button>
            </div>
          </div>

          <!-- Right: Info -->
          <div class="lg:w-2/5 p-8 md:p-12 overflow-y-auto bg-white dark:bg-gray-900 border-l border-gray-100 dark:border-gray-800 flex flex-col">
            <div class="flex items-center justify-between mb-6">
              <div class="flex items-center space-x-2 text-blue-600 dark:text-blue-400">
                <Briefcase class="w-5 h-5" />
                <span class="text-xs font-bold uppercase tracking-widest">Featured Project</span>
              </div>
              <button @click="closeModal" class="hidden lg:block p-2 rounded-full hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors">
                <X class="w-6 h-6 text-gray-500" />
              </button>
            </div>

            <h3 class="text-3xl md:text-4xl font-extrabold text-gray-900 dark:text-white capitalize mb-4 leading-tight">
              {{ selectedProject.name }}
            </h3>
            
            <div class="flex flex-wrap gap-2 mb-8">
              <span v-if="selectedProject.language" class="px-3 py-1 text-xs font-bold bg-blue-100 text-blue-700 dark:bg-blue-900/30 dark:text-blue-400 rounded-full">
                {{ selectedProject.language }}
              </span>
              <span v-for="topic in selectedProject.topics" :key="topic" class="px-3 py-1 text-xs font-semibold bg-gray-100 text-gray-600 dark:bg-gray-800 dark:text-gray-400 rounded-full">
                {{ topic }}
              </span>
            </div>

            <p class="text-lg text-gray-600 dark:text-gray-400 mb-10 leading-relaxed font-medium">
              {{ selectedProject.description }}
            </p>

            <div class="flex flex-col space-y-4 mt-auto">
              <button 
                @click="handleInquiry"
                class="flex items-center justify-center px-8 py-4 bg-blue-600 hover:bg-blue-700 text-white rounded-2xl font-bold transition-all shadow-lg shadow-blue-500/25 hover:-translate-y-1"
              >
                <MessageSquare class="w-5 h-5 mr-3" />
                Inquire About This Project
              </button>
              
              <div class="grid grid-cols-2 gap-4">
                <a 
                  v-if="selectedProject.homepage" 
                  :href="selectedProject.homepage" 
                  target="_blank"
                  class="flex items-center justify-center p-4 border border-gray-200 dark:border-gray-700 rounded-2xl text-gray-700 dark:text-gray-300 hover:bg-gray-50 dark:hover:bg-gray-800 font-bold transition-all"
                >
                  <ExternalLink class="w-5 h-5 mr-2" /> 
                  Live Demo
                </a>
                <!-- Only show Codebase if it's the public portfolio or OJT manager -->
                <a 
                  v-if="selectedProject.name.toLowerCase().includes('portfolio') || selectedProject.name.toLowerCase().includes('ojt')" 
                  :href="selectedProject.url" 
                  target="_blank"
                  class="flex items-center justify-center p-4 border border-gray-200 dark:border-gray-700 rounded-2xl text-gray-700 dark:text-gray-300 hover:bg-gray-50 dark:hover:bg-gray-800 font-bold transition-all"
                >
                  <Code2 class="w-5 h-5 mr-2" /> 
                  Codebase
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </Teleport>
  </section>
</template>
