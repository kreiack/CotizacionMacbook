<script setup>
import { ref, computed } from 'vue'
import heroImage from '../assets/hero.jpg'
import { 
  Cpu, 
  HardDrive, 
  CheckCircle2, 
  XCircle, 
  ArrowRight,
  AlertTriangle,
  Palette,
  BookOpen,
  Layers,
  Info
} from 'lucide-vue-next'

const selectedUse = ref('general') // 'general', 'design', 'student'
const showRamDetails = ref(false)

const models = {
  m1: {
    name: 'MacBook Air M1 (2020)',
    chip: 'Apple M1',
    ram: '8GB Unified Memory',
    storage: '256GB SSD',
    price: 'CLP $679.990',
    url: 'https://www.paris.cl/macbook-air-m1-8gb-ram-256gb-ssd-133-space-grey-mgn63bea-teclado-latino-744975999.html?utm_source=soicos&utm_term=2529552370&utm_medium=referral',
    pros: [
      'Excelente relación precio-calidad',
      'Batería de larga duración',
      'Rendimiento sólido para tareas diarias',
      'Diseño clásico y probado'
    ],
    cons: [
      'Diseño con bordes más gruesos',
      'Cámara web 720p (inferior)',
      '8GB de RAM es limitante para multitarea pesada'
    ],
    designScore: 6,
    portabilityScore: 9
  },
  m2: {
    name: 'MacBook Air M2 (2022)',
    chip: 'Apple M2',
    ram: '16GB Unified Memory',
    storage: '256GB SSD',
    price: 'Oferta CLP $899.990 (antes CLP $949.990, todo medio de pago)',
    url: 'https://www.tecnomas.cl/producto/apple-macbook-air-2022-apple-silicon-m2-8-core-gpu-16-gb-256-gb-ssd-midnight-mc7x4ci-a',
    pros: [
      '16GB RAM ideal para diseño y futuro',
      'Nuevo diseño moderno y ligero',
      'Pantalla Liquid Retina más brillante',
      'Cámara 1080p y MagSafe'
    ],
    cons: [
      'Precio considerablemente mayor',
      'SSD base puede ser más lento en escritura secuencial (menos relevante con 16GB RAM)'
    ],
    designScore: 9,
    portabilityScore: 10
  }
}

const recommendation = computed(() => {
  if (selectedUse.value === 'design') {
    return {
      model: 'm2',
      text: 'Para diseño, la memoria RAM es CRÍTICA. Los 8GB del M1 se llenarán rápidamente con Photoshop, Illustrator o Figma abiertos simultáneamente, causando lentitud (swap). El M2 con 16GB es la elección profesional obligatoria aquí.',
      highlight: 'ram'
    }
  }
  if (selectedUse.value === 'student') {
    return {
      model: 'm1',
      text: 'Para estudiar, escribir documentos y navegar, el M1 es una máquina increíble. No necesitas gastar extra en el M2 a menos que estudies carreras de diseño o arquitectura.',
      highlight: 'price'
    }
  }
  return {
    model: 'both',
    text: 'Ambos son excelentes equipos. El M1 es el rey del valor, mientras que el M2 es una inversión a largo plazo con mejor pantalla y diseño.',
    highlight: 'none'
  }
})

const setUse = (use) => {
  selectedUse.value = use
}
</script>

<template>
  <div class="min-h-screen bg-gray-50 font-sans text-slate-800">
    <!-- Header -->
    <header class="bg-white shadow-sm sticky top-0 z-50">
      <div class="max-w-7xl mx-auto px-4 py-4 flex items-center justify-between">
        <h1 class="text-2xl font-bold bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent">
          Comparativa MacBook Air
        </h1>
        <nav class="flex space-x-4">
          <button 
            @click="setUse('general')"
            :class="['px-4 py-2 rounded-full text-sm font-medium transition-all', selectedUse === 'general' ? 'bg-blue-100 text-blue-700' : 'text-gray-500 hover:bg-gray-100']"
          >
            General
          </button>
          <button 
            @click="setUse('design')"
            :class="['px-4 py-2 rounded-full text-sm font-medium transition-all', selectedUse === 'design' ? 'bg-purple-100 text-purple-700' : 'text-gray-500 hover:bg-gray-100']"
          >
            Diseño Gráfico
          </button>
          <button 
            @click="setUse('student')"
            :class="['px-4 py-2 rounded-full text-sm font-medium transition-all', selectedUse === 'student' ? 'bg-green-100 text-green-700' : 'text-gray-500 hover:bg-gray-100']"
          >
            Estudiante
          </button>
        </nav>
      </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 py-8 space-y-10">
      
      <!-- Dynamic Recommendation Banner -->
      <div class="p-6 rounded-xl border border-l-4 shadow-sm transition-all duration-500"
           :class="{
             'bg-purple-50 border-purple-500': selectedUse === 'design',
             'bg-green-50 border-green-500': selectedUse === 'student',
             'bg-blue-50 border-blue-500': selectedUse === 'general'
           }">
        <div class="flex flex-col md:flex-row md:items-center md:space-x-4 space-y-4 md:space-y-0">
          <div class="p-2 rounded-full bg-white/60 self-start">
            <Palette v-if="selectedUse === 'design'" class="w-6 h-6 text-purple-600" />
            <BookOpen v-else-if="selectedUse === 'student'" class="w-6 h-6 text-green-600" />
            <Layers v-else class="w-6 h-6 text-blue-600" />
          </div>
          <div class="flex-1">
            <h3 class="text-lg md:text-xl font-semibold mb-1">
              {{ selectedUse === 'design' ? 'Enfoque: Diseño y Creatividad' : selectedUse === 'student' ? 'Enfoque: Universidad y Oficina' : 'Visión General' }}
            </h3>
            <p class="text-gray-700 leading-relaxed text-sm md:text-base">
              {{ recommendation.text }}
            </p>
          </div>
        </div>
      </div>

      <!-- Comparison Section -->
      <section class="grid grid-cols-1 md:grid-cols-3 gap-6 items-stretch">
        
        <!-- M1 Card -->
        <div :class="['bg-white rounded-2xl p-5 md:p-6 shadow-lg border-2 transition-all duration-300 flex flex-col', recommendation.model === 'm1' ? 'border-green-500 ring-4 ring-green-500/20 scale-[1.02]' : 'border-transparent hover:border-gray-200']">
          <div class="mb-4 text-center space-y-1.5">
            <h2 class="text-xl md:text-2xl font-bold text-gray-900">{{ models.m1.name }}</h2>
            <div class="flex items-center justify-center space-x-2 text-sky-600">
              <p class="text-lg font-semibold">{{ models.m1.price }}</p>
              <div class="relative group inline-flex">
                <Info class="w-4 h-4 opacity-80" />
                <div class="absolute z-20 left-1/2 -translate-x-1/2 top-full mt-2 w-52 rounded-lg bg-slate-900 text-xs text-slate-50 px-3 py-2 shadow-lg opacity-0 translate-y-1 pointer-events-none group-hover:opacity-100 group-hover:translate-y-0">
                  Precio referencial mostrado en Paris.cl. Puede variar según promociones, stock o medio de pago.
                </div>
              </div>
            </div>
            <span class="inline-block px-3 py-1 bg-gray-100 text-gray-600 text-xs rounded-full">Opción Económica</span>
          </div>
          
          <div class="space-y-5 flex-grow">
            <!-- Specs -->
            <div class="space-y-3">
              <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
                <div class="flex items-center space-x-3">
                  <Cpu class="w-5 h-5 text-blue-500" />
                  <span class="font-medium text-sm">Chip</span>
                </div>
                <span class="text-gray-700 text-sm">{{ models.m1.chip }}</span>
              </div>
              
              <div :class="['flex items-center justify-between p-3 rounded-lg border text-sm transition-colors', selectedUse === 'design' ? 'bg-red-50 border-red-200' : 'bg-gray-50 border-transparent']">
                <div class="flex items-center space-x-3">
                  <Layers class="w-5 h-5 text-purple-500" />
                  <span class="font-medium">RAM</span>
                </div>
                <div class="flex items-center space-x-2 relative group">
                  <span class="text-gray-700 font-bold">{{ models.m1.ram }}</span>
                  <AlertTriangle v-if="selectedUse === 'design'" class="w-4 h-4 text-red-500" />
                  <div class="absolute z-20 right-0 top-full mt-2 w-60 rounded-lg bg-white text-xs text-slate-800 px-3 py-2 shadow-lg border border-red-100 opacity-0 translate-y-1 pointer-events-none group-hover:opacity-100 group-hover:translate-y-0">
                    8GB sirven para tareas diarias, pero en diseño con muchas capas o varias apps abiertas es fácil que se llene la memoria y el sistema use el SSD como swap, volviéndose más lento.
                  </div>
                </div>
              </div>

              <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg text-sm">
                <div class="flex items-center space-x-3">
                  <HardDrive class="w-5 h-5 text-gray-500" />
                  <span class="font-medium">Almacenamiento</span>
                </div>
                <span class="text-gray-700">{{ models.m1.storage }}</span>
              </div>
            </div>

            <!-- Pros & Cons -->
            <div class="grid gap-3">
              <div class="bg-green-50 p-3 rounded-lg">
                <h4 class="font-semibold text-green-800 mb-2 flex items-center text-sm"><CheckCircle2 class="w-4 h-4 mr-2"/> Ventajas</h4>
                <ul class="space-y-1.5">
                  <li v-for="pro in models.m1.pros" :key="pro" class="text-xs text-green-700 flex items-start">
                    <span class="mr-2 mt-[2px]">•</span> {{ pro }}
                  </li>
                </ul>
              </div>
              <div class="bg-red-50 p-3 rounded-lg">
                <h4 class="font-semibold text-red-800 mb-2 flex items-center text-sm"><XCircle class="w-4 h-4 mr-2"/> Limitaciones</h4>
                <ul class="space-y-1.5">
                  <li v-for="con in models.m1.cons" :key="con" class="text-xs text-red-700 flex items-start">
                    <span class="mr-2 mt-[2px]">•</span> {{ con }}
                  </li>
                </ul>
              </div>
            </div>
          </div>

          <div class="mt-6 pt-4 border-t">
            <a :href="models.m1.url" target="_blank" class="block w-full py-3 bg-gray-900 hover:bg-gray-800 text-white text-center rounded-xl font-semibold text-sm transition-colors flex items-center justify-center">
              Ver en Paris.cl <ArrowRight class="ml-2 w-4 h-4" />
            </a>
          </div>
        </div>

        <!-- Center Image / Summary -->
        <div class="relative flex flex-col items-center justify-center bg-gradient-to-b from-slate-900 via-slate-900 to-slate-800 rounded-2xl p-5 shadow-xl overflow-hidden">
          <div class="relative z-10 flex flex-col items-center text-center space-y-4 text-slate-50">
            <img
              :src="heroImage"
              alt="MacBook Air"
              class="max-h-40 md:max-h-48 w-auto object-contain drop-shadow-2xl"
            />
            <div class="flex flex-col items-center space-y-2">
              <span class="text-[11px] uppercase tracking-[0.25em] text-slate-200/80">
                Comparativa
              </span>
              <div class="flex items-center space-x-3">
                <span class="px-3 py-1 rounded-full bg-white/10 text-xs font-medium border border-white/20">M1</span>
                <div class="w-10 h-10 rounded-full bg-white text-slate-900 flex items-center justify-center font-bold shadow-md">
                  VS
                </div>
                <span class="px-3 py-1 rounded-full bg-white/10 text-xs font-medium border border-white/20">M2</span>
              </div>
            </div>
            <p class="text-xs leading-relaxed max-w-xs text-slate-100/90">
              Comparando MacBook Air M1 (2020) vs MacBook Air M2 (2022) para uso diario, diseño y estudios.
            </p>
          </div>
        </div>

        <!-- M2 Card -->
        <div :class="['bg-white rounded-2xl p-5 md:p-6 shadow-lg border-2 transition-all duration-300 flex flex-col', recommendation.model === 'm2' ? 'border-purple-500 ring-4 ring-purple-500/20 scale-[1.02]' : 'border-transparent hover:border-gray-200']">
          <div class="mb-4 text-center space-y-1.5">
            <h2 class="text-xl md:text-2xl font-bold text-gray-900">{{ models.m2.name }}</h2>
            <div class="flex items-center justify-center space-x-2 text-purple-600">
              <p class="text-lg font-semibold">{{ models.m2.price }}</p>
              <div class="relative group inline-flex">
                <Info class="w-4 h-4 opacity-80" />
                <div class="absolute z-20 left-1/2 -translate-x-1/2 top-full mt-2 w-60 rounded-lg bg-slate-900 text-xs text-slate-50 px-3 py-2 shadow-lg opacity-0 translate-y-1 pointer-events-none group-hover:opacity-100 group-hover:translate-y-0">
                  Oferta y precio de referencia de Tecnomas.cl. Comprueba siempre el valor final y la vigencia de la promoción antes de comprar.
                </div>
              </div>
            </div>
            <span class="inline-block px-3 py-1 bg-purple-100 text-purple-700 text-xs rounded-full">Recomendado para Pro</span>
          </div>
          
          <div class="space-y-5 flex-grow">
            <!-- Specs -->
            <div class="space-y-3">
              <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg">
                <div class="flex items-center space-x-3">
                  <Cpu class="w-5 h-5 text-blue-500" />
                  <span class="font-medium text-sm">Chip</span>
                </div>
                <span class="text-gray-700 text-sm">{{ models.m2.chip }}</span>
              </div>
              
              <div :class="['flex items-center justify-between p-3 rounded-lg border text-sm transition-colors', selectedUse === 'design' ? 'bg-green-50 border-green-200' : 'bg-gray-50 border-transparent']">
                <div class="flex items-center space-x-3">
                  <Layers class="w-5 h-5 text-purple-500" />
                  <span class="font-medium">RAM</span>
                </div>
                <div class="flex items-center space-x-2 relative group">
                  <span class="text-gray-900 font-bold">{{ models.m2.ram }}</span>
                  <CheckCircle2 v-if="selectedUse === 'design'" class="w-4 h-4 text-green-600" />
                  <div class="absolute z-20 right-0 top-full mt-2 w-60 rounded-lg bg-white text-xs text-slate-800 px-3 py-2 shadow-lg border border-emerald-100 opacity-0 translate-y-1 pointer-events-none group-hover:opacity-100 group-hover:translate-y-0">
                    16GB te permiten trabajar cómodo con Photoshop, Figma, navegador con muchas pestañas y otras apps abiertas sin saturar la RAM ni depender tanto del SSD.
                  </div>
                </div>
              </div>

              <div class="flex items-center justify-between p-3 bg-gray-50 rounded-lg text-sm">
                <div class="flex items-center space-x-3">
                  <HardDrive class="w-5 h-5 text-gray-500" />
                  <span class="font-medium">Almacenamiento</span>
                </div>
                <span class="text-gray-700">{{ models.m2.storage }}</span>
              </div>
            </div>

            <!-- Pros & Cons -->
            <div class="grid gap-3">
              <div class="bg-green-50 p-3 rounded-lg">
                <h4 class="font-semibold text-green-800 mb-2 flex items-center text-sm"><CheckCircle2 class="w-4 h-4 mr-2"/> Ventajas</h4>
                <ul class="space-y-1.5">
                  <li v-for="pro in models.m2.pros" :key="pro" class="text-xs text-green-700 flex items-start">
                    <span class="mr-2 mt-[2px]">•</span> {{ pro }}
                  </li>
                </ul>
              </div>
              <div class="bg-yellow-50 p-3 rounded-lg">
                <h4 class="font-semibold text-yellow-800 mb-2 flex items-center text-sm"><AlertTriangle class="w-4 h-4 mr-2"/> A Considerar</h4>
                <ul class="space-y-1.5">
                  <li v-for="con in models.m2.cons" :key="con" class="text-xs text-yellow-700 flex items-start">
                    <span class="mr-2 mt-[2px]">•</span> {{ con }}
                  </li>
                </ul>
              </div>
            </div>
          </div>

          <div class="mt-6 pt-4 border-t">
            <a :href="models.m2.url" target="_blank" class="block w-full py-3 bg-gray-900 hover:bg-gray-800 text-white text-center rounded-xl font-semibold text-sm transition-colors flex items-center justify-center">
              Ver en Tecnomas.cl <ArrowRight class="ml-2 w-4 h-4" />
            </a>
          </div>
        </div>

      </section>

      <!-- Deep Dive Section -->
      <div class="bg-white rounded-2xl p-6 md:p-8 shadow-sm border border-gray-100">
        <div class="flex items-center justify-between gap-4 mb-4">
          <h3 class="text-xl md:text-2xl font-bold text-gray-800">¿Por qué importan los 16GB de RAM?</h3>
          <button
            type="button"
            @click="showRamDetails = !showRamDetails"
            class="inline-flex items-center px-3 py-1.5 rounded-full text-xs font-medium border border-gray-200 text-gray-700 hover:bg-gray-50 transition-colors"
          >
            <span v-if="!showRamDetails">Ver explicación</span>
            <span v-else>Ocultar</span>
          </button>
        </div>
        <transition name="fade">
          <div v-if="showRamDetails" class="grid md:grid-cols-2 gap-8 items-center">
            <div>
              <p class="text-gray-600 mb-4 leading-relaxed">
                La memoria unificada de Apple es muy eficiente, por lo que 8GB en un Mac rinden más que 8GB en Windows. Sin embargo, <strong>para diseño gráfico y video</strong>, la RAM es el cuello de botella más común.
              </p>
              <p class="text-gray-600 leading-relaxed">
                Cuando los 8GB se llenan (ej: Photoshop + Chrome con pestañas), el sistema usa el SSD como memoria temporal ("Swap"). Esto desgasta el SSD y ralentiza el sistema. Los <strong>16GB</strong> del modelo M2 te permiten trabajar con múltiples capas, archivos grandes y varias apps abiertas sin comprometer la fluidez ni la vida útil de tu equipo.
              </p>
            </div>
            <div class="bg-gray-100 p-6 rounded-xl">
              <div class="space-y-4">
                <div>
                  <div class="flex justify-between text-sm mb-1">
                    <span>Uso de RAM (Diseño Básico)</span>
                    <span class="font-bold text-green-600">OK en 8GB</span>
                  </div>
                  <div class="w-full bg-gray-300 rounded-full h-2.5">
                    <div class="bg-green-500 h-2.5 rounded-full" style="width: 60%"></div>
                  </div>
                </div>
                <div>
                  <div class="flex justify-between text-sm mb-1">
                    <span>Uso de RAM (Diseño Pro / Multitask)</span>
                    <span class="font-bold text-red-600">Satura 8GB</span>
                  </div>
                  <div class="w-full bg-gray-300 rounded-full h-2.5 relative">
                    <div class="bg-red-500 h-2.5 rounded-full" style="width: 100%"></div>
                    <div class="absolute -right-2 -top-1 text-xs bg-red-100 text-red-600 px-1 rounded">Swap!</div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </transition>
      </div>

    </main>
  </div>
</template>

<style scoped>
/* Custom transitions if needed */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.25s ease, transform 0.25s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(-4px);
}
</style>
