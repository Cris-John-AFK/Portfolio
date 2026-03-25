<script setup>
import { Github, Linkedin, Mail, Twitter } from 'lucide-vue-next'
</script>

<template>
  <footer class="bg-gray-900 text-white py-12 border-t border-gray-800">
    <div class="container mx-auto px-4 md:px-6 lg:px-8 max-w-7xl">
      <div class="grid grid-cols-1 md:grid-cols-3 gap-8 items-center">
        <!-- Brand -->
        <div class="text-center md:text-left">
          <a href="#home" class="text-2xl font-bold tracking-tight text-blue-400 mb-2 block">
            Web Dev Portfolio
          </a>
          <p class="text-gray-400 text-sm">
            Building robust web applications with a focus on UI/UX and performance.
          </p>
        </div>

        <div class="flex justify-center flex-wrap gap-6 pt-4">
          <a href="https://github.com/Cris-John-AFK" target="_blank" class="text-gray-400 hover:text-white transition-colors p-2 hover:bg-gray-800 rounded-full" aria-label="Github">
            <Github class="w-6 h-6" />
          </a>
          <a href="https://www.linkedin.com/in/cris-john-ca%C3%B1ales-06b407328" target="_blank" class="text-gray-400 hover:text-blue-500 transition-colors p-2 hover:bg-gray-800 rounded-full" aria-label="LinkedIn">
            <Linkedin class="w-6 h-6" />
          </a>
          <a href="mailto:crisjohn.canales@gmail.com" class="text-gray-400 hover:text-red-400 transition-colors p-2 hover:bg-gray-800 rounded-full" aria-label="Email">
            <Mail class="w-6 h-6" />
          </a>
        </div>

        <!-- Copyright -->
        <div class="text-center md:text-right text-gray-500 text-sm">
          <p>&copy; {{ new Date().getFullYear() }} Crisjohn. All rights reserved.</p>
          <p class="mt-1">Designed & Built with Vue 3 and TailwindCSS.</p>
        </div>
      </div>
    </div>
  </footer>
</template>
