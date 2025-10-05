<template>
  <div class="min-h-screen bg-background text-foreground transition-colors">
    <nav :class="['fixed top-0 left-0 right-0 z-50 backdrop-blur-sm border-b', scrolled ? 'bg-background/90 shadow-sm' : 'bg-background/80']">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex items-center justify-between h-16">
          <div class="flex items-center gap-4">
            <div class="text-lg font-semibold">{{ name }}</div>
            <p class="hidden sm:block text-sm text-muted-foreground">{{ title }}</p>
          </div>

          <!-- Desktop nav -->
          <div class="hidden md:flex items-center gap-6">
            <button
              v-for="section in sections"
              :key="section.id"
              @click="scrollToSection(section.id)"
              :class="['text-sm px-2 py-1 rounded-md transition-colors', activeSection === section.id ? 'text-primary font-semibold' : 'text-muted-foreground hover:text-primary']"
            >
              {{ section.label }}
            </button>

            <button
              @click="downloadCV"
              class="ml-4 px-4 py-2 bg-primary text-primary-foreground rounded-md text-sm hover:brightness-95 transition"
            >
              <i class="fa-solid fa-file-arrow-down mr-2"></i>Télécharger CV
            </button>
          </div>

          <!-- Mobile controls -->
          <div class="md:hidden flex items-center gap-3">
            <button @click="downloadCV" class="p-2 rounded-md text-muted-foreground hover:text-primary">
              <i class="fa-solid fa-file-arrow-down"></i>
            </button>
            <button @click="mobileOpen = !mobileOpen" class="p-2 rounded-md text-muted-foreground hover:text-primary">
              <i :class="mobileOpen ? 'fa-solid fa-xmark' : 'fa-solid fa-bars'"></i>
            </button>
          </div>
        </div>
      </div>

      <!-- Mobile menu -->
      <transition name="fade">
        <div v-if="mobileOpen" class="md:hidden border-t border-border bg-background/95">
          <div class="px-4 py-3 space-y-2">
            <button
              v-for="section in sections"
              :key="section.id"
              @click="onMobileNav(section.id)"
              class="w-full text-left px-3 py-2 rounded-md text-sm text-muted-foreground hover:bg-accent hover:text-primary transition"
            >
              {{ section.label }}
            </button>
          </div>
        </div>
      </transition>
    </nav>
    <main class="pt-20">
      <section class="pt-12 pb-20 px-6">
        <div class="max-w-4xl mx-auto">
          <p class="text-sm text-muted-foreground mb-4">{{ greeting }}</p>
          <h2 class="text-3xl sm:text-4xl font-extrabold mb-4 tracking-tight text-foreground">
            {{ name }} — <span class="text-primary">{{ title }}</span>
          </h2>
          <p class="text-lg leading-relaxed text-muted-foreground bg-gradient-to-r from-muted/0 via-muted/20 to-muted/0 p-4 rounded-lg">
            {{ bio }}
          </p>
        </div>
      </section>

      <section id="projets" class="py-16 px-6 bg-muted/30">
        <div class="max-w-7xl mx-auto">
          <h3 class="text-sm font-medium text-muted-foreground mb-8">Projets sélectionnés</h3>
          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8">
            <article v-for="project in projects" :key="project.id" class="group bg-white/5 rounded-lg overflow-hidden shadow-sm hover:shadow-md transition">
              <div class="relative">
                <img :src="project.image" :alt="project.title" class="w-full h-48 object-cover group-hover:scale-105 transition-transform duration-300" />
                <div class="absolute inset-0 flex items-end p-3 pointer-events-none">
                  <div class="bg-gradient-to-t from-black/60 to-transparent w-full p-3 rounded-t-md pointer-events-auto opacity-0 group-hover:opacity-100 transition-opacity">
                    <div class="flex items-center justify-between">
                      <h4 class="text-sm font-semibold text-white">{{ project.title }}</h4>
                      <div class="flex gap-2">
                        <a v-if="project.github" :href="project.github" target="_blank" rel="noopener noreferrer" class="text-white bg-primary/90 p-2 rounded-md hover:scale-105 transition">
                          <i class="fa-brands fa-github text-2xl"></i>
                        </a>
                        <a v-if="project.demo" :href="project.demo" target="_blank" rel="noopener noreferrer" class="text-white bg-accent p-2 rounded-md hover:scale-105 transition">
                          <i class="fa-solid fa-arrow-up-right-from-square text-black"></i>
                        </a>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="p-4">
                <p class="text-sm text-muted-foreground mb-3 leading-relaxed">{{ project.description }}</p>
                <div class="flex flex-wrap gap-2 mb-3">
                  <span v-for="tech in project.technologies" :key="tech" class="text-xs px-2 py-1 bg-muted rounded text-muted-foreground ">
                    {{ tech }}
                  </span>
                </div>
                <div class="flex gap-3 text-sm">
                  <a v-if="project.github" :href="project.github" target="_blank" rel="noopener noreferrer" class="text-primary hover:underline">
                    GitHub
                  </a>
                  <a v-if="project.demo" :href="project.demo" target="_blank" rel="noopener noreferrer" class="text-primary hover:underline">
                    Démo
                  </a>
                </div>
              </div>
            </article>
          </div>
        </div>
      </section>

      <section id="experience" class="py-20 px-6">
        <div class="max-w-4xl mx-auto">
          <h2 class="text-sm font-medium text-muted-foreground mb-12">Expérience</h2>
          <div class="space-y-12">
            <div
              v-for="exp in experiences"
              :key="exp.id"
              class="border-l-2 border-border pl-6"
            >
              <div class="flex items-start gap-4 mb-2">
                <img
                  :src="exp.image"
                  :alt="exp.company"
                  class="w-20 h-20 object-contain rounded-md bg-muted p-2 flex-shrink-0"
                  loading="lazy"
                />
                <div class="flex-1">
                  <div class="flex items-start justify-between">
                    <div>
                      <h3 class="font-medium">{{ exp.position }}</h3>
                      <p class="text-muted-foreground">{{ exp.company }}</p>
                    </div>
                    <span class="text-sm text-muted-foreground whitespace-nowrap">
                      {{ exp.period }}
                    </span>
                  </div>
                  <p class="text-sm leading-relaxed text-muted-foreground mt-3">
                    {{ exp.description }}
                  </p>
                  <div class="flex flex-wrap gap-2 mt-4">
                    <span
                      v-for="tech in exp.technologies"
                      :key="tech"
                      class="text-xs px-2 py-1 bg-muted rounded"
                    >
                      {{ tech }}
                    </span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
         
      <section id="formation" class="py-20 px-6">
        <div class="max-w-4xl mx-auto">
          <h2 class="text-sm font-medium text-muted-foreground mb-12">Formation</h2>
          <div class="space-y-8">
            <div
              v-for="edu in education"
              :key="edu.id"
              class="border-l-2 border-border pl-6"
            >
              <div class="flex items-start gap-4 mb-2">
                <img
                  :src="edu.image"
                  :alt="edu.school"
                  class="w-20 h-20 object-contain rounded-md bg-muted p-2 flex-shrink-0"
                  loading="lazy"
                />
                <div class="flex-1">
                  <div class="flex items-start justify-between">
                    <div>
                      <h3 class="font-medium">{{ edu.degree }}</h3>
                      <p class="text-muted-foreground">{{ edu.school }}</p>
                    </div>
                    <span class="text-sm text-muted-foreground whitespace-nowrap">
                      {{ edu.year }}
                    </span>
                  </div>
                  <p class="text-sm leading-relaxed text-muted-foreground mt-3">
                    {{ edu.description }}
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>

      <section id="competences" class="py-20 px-6 bg-muted/30">
        <div class="max-w-4xl mx-auto">
          <h2 class="text-sm font-medium text-muted-foreground mb-12">Compétences</h2>
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
            <div v-for="category in skills" :key="category.category">
              <h3 class="font-medium mb-4">{{ category.category }}</h3>
              <ul class="space-y-2">
                <li
                  v-for="skill in category.items"
                  :key="skill.name"
                  class="text-sm text-muted-foreground hover:bg-gray-100 flex items-center gap-2"
                >
                  <i :class="skill.icon" class="text-primary"></i>
                  {{ skill.name }}
                </li>
              </ul>
            </div>
          </div>
        </div>
      </section>

      <section id="contact" class="py-20 px-6 bg-muted/30">
        <div class="max-w-4xl mx-auto text-center">
          <p class="text-lg mb-8 text-balance">
            Si vous souhaitez discuter d'un projet ou simplement dire bonjour, je suis toujours disponible pour échanger.
          </p>
          <a
            href="mailto:votre.email@example.com"
            class="inline-block px-6 py-3 bg-primary text-primary-foreground rounded-md hover:bg-primary/90 transition-colors"
          >
            Me contacter
          </a>
        </div>
      </section>
    </main>

    <footer class="py-12 px-6 border-t border-border">
      <div class="max-w-7xl mx-auto">
        <div class="flex flex-col md:flex-row items-center justify-between gap-4">
          <p class="text-sm text-muted-foreground">
            © {{ currentYear }} {{ name }}. Tous droits réservés.
          </p>
          <div class="flex gap-6">
            <a
              v-for="social in socials"
              :key="social.name"
              :href="social.url"
              target="_blank"
              rel="noopener noreferrer"
              class="text-sm text-muted-foreground hover:text-foreground transition-colors"
            >
              {{ social.name }}
            </a>
          </div>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'

const mobileOpen = ref(false)
const activeSection = ref(null)
const scrolled = ref(false)

// Personal Information
const name = ref('Ayoub FETTI')
const title = ref('Développeur Full Stack')
const greeting = ref('Bonjour')
const bio = ref("Je suis un développeur passionné par la création d'expériences web modernes et performantes. Mon expertise se situe à l'intersection du design et du développement, créant des interfaces qui non seulement sont visuellement attrayantes mais aussi méticuleusement construites pour la performance et l'accessibilité.")

const currentYear = computed(() => new Date().getFullYear())

// Navigation
const sections = ref([
  { id: 'projets', label: 'Projets' },
  { id: 'experience', label: 'Expérience' },
  { id: 'competences', label: 'Compétences' },
  { id: 'formation', label: 'Formation' },
  { id: 'contact', label: 'Contact' }
])

// Projects Data
const projects = ref([
  {
    id: 1,
    title: 'AtlasVolunteers',
    description: 'AtlasVolunteers est une application web conçue pour faciliter la connexion entre bénévoles et organisations. Son objectif est de simplifier la recherche, la gestion et la participation aux actions de service communautaire, tout en favorisant l`engagement citoyen et l`impact social.',
    image: '../public/images/atlas.png',
    technologies: ['Laravel','Blade', 'Tailwind', 'UML'],
    github: 'https://github.com/Ayoub-fetti/AtlasVolunteer.git',
    demo: '#'
  },
  {
    id: 2,
    title: 'Snapshop',
    description: 'SnapShop est une application e-commerce moderne développée avec Vue.js pour le front-end, Tailwind CSS pour le design réactif et élégant, et Firebase pour la gestion de l’authentification, de la base de données et de l’hébergement.',
    image: '../public/images/snap.png',
    technologies: ['Vue.js', 'Tailwind', 'Firebase'],
    github: 'https://github.com/Ayoub-fetti/SnapShop',
    demo: 'https://shopysnap.netlify.app/'
  },
  {
    id: 3,
    title: 'QuizCraft',
    description: 'QuizCraft est une application légère et pédagogique permettant de créer, jouer et gérer des quiz directement dans le navigateur. Développée avec HTML, CSS (design responsive) et JavaScript (logique et persistance locale), l’application vise à offrir une expérience simple pour enseignants, étudiants ou développeurs qui veulent s’exercer ou partager des questionnaires interactifs.',
    image: '../public/images/quiz2.png',
    technologies: ['CSS', 'Javascript', 'JSON'],
    github: 'https://github.com/Ayoub-fetti/JSQuizStarter',
    demo: 'https://ayoub-fetti.github.io/JSQuizStarter/'
  },
])

// Experience Data
const experiences = ref([
  {
    id: 1,
    position: 'Développeur Full Stack (Stage)',
    company: 'VMS',
    period: '3 mois',
    description: "Développement et maintenance de composants critiques utilisés dans l'ensemble du produit. Collaboration étroite avec des équipes interfonctionnelles pour implémenter les meilleures pratiques en matière d'accessibilité web et de performance.",
    image: '../public/images/vms.png',
    technologies: ['Vue.js', 'Laravel', 'PostgreSQL', 'Docker']
  }
])

// Skills Data
const skills = ref([
  {
    category: 'Frontend',
    items: [
      { name: 'Vue.js', icon: 'fa-brands fa-vuejs' },
      { name: 'React', icon: 'fa-brands fa-react' },
      { name: 'Tailwind CSS', icon: 'fa-brands fa-css3-alt' },
      { name: 'HTML5/CSS3', icon: 'fa-brands fa-html5' }
    ]
  },
  {
    category: 'Backend',
    items: [
      { name: 'Node.js', icon: 'fa-brands fa-node-js' },
      { name: 'Express', icon: 'fa-solid fa-server' },
      { name: 'Laravel', icon: 'fa-brands fa-laravel' },
      { name: 'REST API', icon: 'fa-solid fa-code' },
      { name: 'UML', icon: 'fa-solid fa-diagram-project' }
    ]
  },
  {
    category: 'Base de données',
    items: [
      { name: 'PostgreSQL', icon: 'fa-solid fa-database' },
      { name: 'MongoDB', icon: 'fa-solid fa-database' },
      { name: 'Mysql', icon: 'fa-solid fa-database' },
      { name: 'Firebase', icon: 'fa-solid fa-fire' }
    ]
  },
  {
    category: 'Outils',
    items: [
      { name: 'Git', icon: 'fa-brands fa-git-alt' },
      { name: 'Docker', icon: 'fa-brands fa-docker' },
      { name: 'Vite', icon: 'fa-solid fa-bolt' },
      { name: 'Postman', icon: 'fa-brands fa-get-pocket' },
      { name: 'Swager', icon: 'fa-solid fa-file-lines' }
    ]
  },
  {
    category: 'Design',
    items: [
      { name: 'Figma', icon: 'fa-brands fa-figma' },
      { name: 'Responsive Design', icon: 'fa-solid fa-mobile-screen-button' },
      { name: 'UI/UX', icon: 'fa-solid fa-paint-brush' }
    ]
  },
  {
    category: 'Autres',
    items: [
      { name: 'Agile/Scrum', icon: 'fa-solid fa-people-roof' },
      { name: 'CI/CD', icon: 'fa-solid fa-gear' },
      { name: 'SEO', icon: 'fa-solid fa-chart-line' },
      { name: 'Accessibilité', icon: 'fa-solid fa-universal-access' }
    ]
  }
])

// Education Data
const education = ref([
  {
    id: 1,
    degree: 'Développement Full-Stack',
    school: 'YouCode , Fondation OCP / UM6P | Nador',
    year: '2024-2026',
    description: 'Spécialisation en développement web et ingénierie logicielle. Projet de fin d\'études sur les applications web progressives.',
    image: '../public/images/youcode.png'
  },
  {
    id: 2,
    degree: 'Licence professionnelle en Gestion Informatisée des Organisations',
    school: 'Faculté des sciences juridiques, économiques et sociales | Meknès',
    year: '2023',
    description: 'Formation générale en informatique et gestion des entreprises',
    image: '../public/images/umi.png'
  }
])

// Social Links
const socials = ref([
  { name: 'GitHub', url: 'https://github.com/Ayoub-fetti' },
  { name: 'LinkedIn', url: 'https://www.linkedin.com/in/ayoub-fetti-09925a239/' },
  { name: 'X', url: 'https://x.com/ayoub1_fetti' },
  { name: 'Email', url: 'mailto:ayoubfetti.dev@gmail.com' }
])

// Methods
const scrollToSection = (sectionId) => {
  const element = document.getElementById(sectionId)
  if (element) {
    const offset = 80
    const elementPosition = element.getBoundingClientRect().top
    const offsetPosition = elementPosition + window.pageYOffset - offset

    window.scrollTo({
      top: offsetPosition,
      behavior: 'smooth'
    })
  }
  activeSection.value = sectionId
}

// helper for mobile nav
const onMobileNav = (id) => {
  mobileOpen.value = false
  scrollToSection(id)
}

// download CV: use public root path
const downloadCV = () => {
  const link = document.createElement('a')
  link.href = '/cv.pdf'
  link.download = 'CV_Ayoub_FETTI.pdf'
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)
}

// observe sections to update active nav
let io = null
onMounted(() => {
  // detect scroll for nav background
  const onScroll = () => (scrolled.value = window.scrollY > 10)
  window.addEventListener('scroll', onScroll)
  onScroll()

  const sectionEls = sections.map(s => document.getElementById(s.id)).filter(Boolean)
  if (sectionEls.length) {
    io = new IntersectionObserver((entries) => {
      entries.forEach(e => {
        if (e.isIntersecting) activeSection.value = e.target.id
      })
    }, { root: null, rootMargin: '-40% 0px -40% 0px', threshold: 0 })
    sectionEls.forEach(el => io.observe(el))
  }

  onBeforeUnmount(() => {
    window.removeEventListener('scroll', onScroll)
    if (io) io.disconnect()
  })
})
</script>

<style>
/* small local transitions */
.fade-enter-active, .fade-leave-active { transition: opacity .2s }
.fade-enter-from, .fade-leave-to { opacity: 0 }
</style>
