<script setup>
import { ref, onMounted } from 'vue'
import { ExternalLink, Code2, Loader2 } from 'lucide-vue-next'

const projects = ref([])
const loading = ref(true)
const githubUsername = 'Cris-John-AFK' // Updated to your true GitHub username

const fetchProjects = async () => {
  try {
    loading.value = true
    const res = await fetch(`https://api.github.com/users/${githubUsername}/repos?sort=updated&per_page=6`)
    if (res.ok) {
      const data = await res.json()
      // Filter out forks and map to our project format
      projects.value = data
        .filter(repo => !repo.fork)
        .map(repo => ({
          id: repo.id,
          name: repo.name.replace(/-/g, ' '),
          originalName: repo.name,
          ownerLogin: repo.owner.login,
          description: repo.description || 'A web development project built with modern technologies.',
          url: repo.html_url,
          homepage: repo.homepage,
          language: repo.language,
          topics: repo.topics || []
        }))
    }
  } catch (err) {
    console.error('Failed to fetch projects', err)
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchProjects()
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
          A showcase of my recent work fetched dynamically from GitHub.
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
          class="group bg-white dark:bg-gray-800 rounded-2xl overflow-hidden shadow-sm hover:shadow-xl transition-all duration-300 border border-gray-100 dark:border-gray-700 flex flex-col h-full"
        >
          <!-- Dynamic GitHub Social Preview / OpenGraph Image -->
          <div class="h-48 relative overflow-hidden flex items-center justify-center bg-gray-100 dark:bg-gray-900">
            <img 
              :src="`https://opengraph.githubassets.com/1/${project.ownerLogin}/${project.originalName}`" 
              :alt="project.name"
              class="w-full h-full object-cover object-top group-hover:scale-105 transition-transform duration-500"
              loading="lazy"
            />
            <div class="absolute inset-0 bg-blue-600/0 group-hover:bg-blue-600/10 transition-colors duration-300"></div>
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
              <span v-for="topic in project.topics.slice(0, 3)" :key="topic" class="px-2.5 py-1 text-xs font-medium bg-gray-100 text-gray-600 dark:bg-gray-700 dark:text-gray-300 rounded-md">
                {{ topic }}
              </span>
            </div>
            
            <div class="mt-auto flex gap-3 pt-4 border-t border-gray-100 dark:border-gray-700">
              <a 
                v-if="project.homepage" 
                :href="project.homepage" 
                target="_blank"
                class="flex items-center text-sm font-medium text-blue-600 dark:text-blue-400 hover:text-blue-800 dark:hover:text-blue-300 transition-colors"
                aria-label="Live Demo"
              >
                <ExternalLink class="w-4 h-4 mr-1.5" /> Live Demo
              </a>
              <a 
                :href="project.url" 
                target="_blank"
                class="flex items-center text-sm font-medium text-gray-700 dark:text-gray-300 hover:text-gray-900 dark:hover:text-white transition-colors ml-auto"
                aria-label="View Code"
              >
                <Code2 class="w-4 h-4 mr-1.5" /> Source
              </a>
            </div>
          </div>
        </div>
      </div>
      
    </div>
  </section>
</template>
