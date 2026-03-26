<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { Menu, X, Moon, Sun, Monitor } from 'lucide-vue-next'

const props = defineProps(['isDark'])
const emit = defineEmits(['toggle-dark'])

const isScrolled = ref(false)
const isMobileMenuOpen = ref(false)

const navLinks = [
  { name: 'Home', href: '#home' },
  { name: 'About', href: '#about' },
  { name: 'Projects', href: '#projects' },
  { name: 'Skills', href: '#skills' },
  { name: 'Contact', href: '#contact' },
]

const handleScroll = () => {
  isScrolled.value = window.scrollY > 20
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
})

const scrollTo = (href) => {
  isMobileMenuOpen.value = false
  const element = document.querySelector(href)
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' })
  }
}
</script>

<template>
  <header 
    :class="[
      'fixed top-0 w-full z-50 transition-all duration-300',
      isScrolled ? 'glass-nav shadow-sm py-3' : 'bg-transparent py-5'
    ]"
  >
    <div class="container mx-auto px-4 md:px-6 lg:px-8 max-w-7xl flex items-center justify-between">
      <a href="#home" class="flex items-center text-blue-600 dark:text-blue-400" @click.prevent="scrollTo('#home')">
      </a>

      <!-- Desktop Nav -->
      <nav class="hidden md:flex items-center space-x-8">
        <a 
          v-for="link in navLinks" 
          :key="link.name" 
          :href="link.href"
          @click.prevent="scrollTo(link.href)"
          class="text-sm font-medium text-gray-700 hover:text-blue-600 dark:text-gray-300 dark:hover:text-blue-400 transition-colors"
        >
          {{ link.name }}
        </a>
        
        <button 
          @click="emit('toggle-dark')" 
          class="p-2 rounded-full hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors focus:outline-none"
          aria-label="Toggle Dark Mode"
        >
          <Sun v-if="isDark" class="w-5 h-5 text-yellow-400" />
          <Moon v-else class="w-5 h-5 text-gray-700" />
        </button>
      </nav>

      <!-- Mobile Menu Toggle -->
      <div class="md:hidden flex items-center space-x-4">
        <button 
          @click="emit('toggle-dark')" 
          class="p-2 rounded-full hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors focus:outline-none"
        >
          <Sun v-if="isDark" class="w-5 h-5 text-yellow-400" />
          <Moon v-else class="w-5 h-5 text-gray-700" />
        </button>
        <button 
          @click="isMobileMenuOpen = !isMobileMenuOpen"
          class="p-2 text-gray-700 dark:text-gray-300 focus:outline-none"
        >
          <Menu v-if="!isMobileMenuOpen" class="w-6 h-6" />
          <X v-else class="w-6 h-6" />
        </button>
      </div>
    </div>

    <!-- Mobile Menu -->
    <div 
      v-if="isMobileMenuOpen" 
      class="md:hidden absolute top-full left-0 w-full bg-white dark:bg-gray-900 border-b border-gray-200 dark:border-gray-800 shadow-lg py-4 px-4 flex flex-col space-y-4"
    >
      <a 
        v-for="link in navLinks" 
        :key="link.name" 
        :href="link.href"
        @click.prevent="scrollTo(link.href)"
        class="text-base font-medium text-gray-800 dark:text-gray-200 hover:text-blue-600 dark:hover:text-blue-400 px-2 flex py-2"
      >
        {{ link.name }}
      </a>
    </div>
  </header>
</template>
