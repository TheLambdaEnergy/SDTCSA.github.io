<template>
  <Transition
    enter-active-class="transition duration-700 ease-out-back"
    enter-from-class="opacity-0 scale-50 rotate-6 translate-y-10"
    enter-to-class="opacity-100 scale-100 rotate-0 translate-y-0"
    leave-active-class="transition duration-500 ease-in"
    leave-from-class="opacity-100 scale-100 translate-y-0"
    :leave-to-class="isNavigating ? 'opacity-0 scale-0 translate-y-[50vh] rotate-12 blur-sm' : 'opacity-0 scale-90 -translate-y-10'"
  >
    <div v-if="isOpen" class="fixed inset-0 z-[100] flex items-center justify-center p-4 sm:p-6 perspective-1000">
      <!-- Backdrop with magical blur -->
      <div class="absolute inset-0 bg-slate-950/80 backdrop-blur-md transition-opacity duration-500" @click="close"></div>

      <!-- Magical Glow Behind -->
      <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-full max-w-lg aspect-[3/4] bg-gradient-to-tr from-purple-500/30 via-sky-500/30 to-pink-500/30 blur-[100px] animate-pulse-slow pointer-events-none"></div>

      <!-- Modal Content -->
      <div class="relative w-full max-w-2xl overflow-hidden rounded-2xl bg-transparent shadow-2xl transform-style-3d group">
        
        <!-- Magical Border Effect -->
        <div class="absolute inset-0 rounded-2xl border border-transparent bg-gradient-to-br from-sky-400/50 via-purple-400/50 to-pink-400/50 [mask:linear-gradient(#fff_0_0)_content-box,linear-gradient(#fff_0_0)] [-webkit-mask-composite:xor] [mask-composite:exclude] opacity-50 group-hover:opacity-100 transition-opacity duration-500 pointer-events-none z-40"></div>

        <!-- Close Button -->
        <button 
          @click="close"
          class="absolute right-3 top-3 z-50 rounded-full bg-black/40 p-2 text-white/80 backdrop-blur-md transition hover:bg-red-500/80 hover:text-white hover:rotate-90 duration-300"
        >
          <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M18 6 6 18"/><path d="m6 6 12 12"/></svg>
        </button>

        <!-- Poster Container -->
        <div class="relative w-full h-auto bg-transparent">
           <!-- Main Image (Natural size) -->
           <img :src="resolveImage('/posters/qlhj.webp')" alt="齐鲁幻聚海报" class="block w-full h-auto object-contain" />
           
           <!-- Floating Particles (CSS) -->
           <div class="absolute inset-0 z-20 pointer-events-none overflow-hidden">
              <div class="particle p1"></div>
              <div class="particle p2"></div>
              <div class="particle p3"></div>
              <div class="particle p4"></div>
              <div class="particle p5"></div>
           </div>

          <!-- Overlay Content -->
          <div class="absolute inset-x-0 bottom-0 z-30 bg-gradient-to-t from-slate-950 via-slate-950/80 to-transparent pt-20 pb-6 px-6 text-center">
            <h3 class="text-2xl font-bold mb-2 text-transparent bg-clip-text bg-gradient-to-r from-sky-300 via-white to-purple-300 animate-gradient-x drop-shadow-sm">12月例会售票中！</h3>
            <p class="text-sm text-slate-300 mb-4 font-medium tracking-wide">山东东方高校联合例会01-齐鲁幻聚</p>
            
            <button 
              @click="navigateToEvent"
              class="relative w-full max-w-xs mx-auto overflow-hidden rounded-xl bg-gradient-to-r from-sky-600 to-purple-600 px-6 py-3 text-base font-bold text-white shadow-lg shadow-sky-500/30 transition-all hover:scale-[1.02] hover:shadow-sky-500/50 active:scale-95 group/btn"
            >
              <span class="relative z-10 flex items-center justify-center gap-2">
                <span class="animate-bounce">✨</span> 立即查看详情 <span class="animate-bounce delay-100">✨</span>
              </span>
              <!-- Button Shine Effect -->
              <div class="absolute inset-0 -translate-x-full group-hover/btn:animate-shine bg-gradient-to-r from-transparent via-white/30 to-transparent z-0"></div>
            </button>
          </div>
        </div>
      </div>
    </div>
  </Transition>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { resolveImage } from '@/utils/image';

const isOpen = ref(false);
const isNavigating = ref(false);

const emit = defineEmits<{
  (e: 'navigate'): void;
}>();

const close = () => {
  isOpen.value = false;
};

const navigateToEvent = () => {
  isNavigating.value = true;
  isOpen.value = false;
  // Wait for close animation to finish slightly before navigating
  setTimeout(() => {
    emit('navigate');
    // Reset flag after animation
    setTimeout(() => { isNavigating.value = false; }, 500);
  }, 300);
};

onMounted(() => {
  // Show modal after a short delay
  setTimeout(() => {
    // Check if already shown in this session (optional, but good UX)
    // For now, we show it every time as requested, or we can use sessionStorage
    const hasShown = sessionStorage.getItem('hasShownTicketModal');
    if (!hasShown) {
      isOpen.value = true;
      sessionStorage.setItem('hasShownTicketModal', 'true');
    }
  }, 1000);
});
</script>

<style scoped>
.ease-out-back {
  transition-timing-function: cubic-bezier(0.34, 1.56, 0.64, 1);
}

.perspective-1000 {
  perspective: 1000px;
}

.transform-style-3d {
  transform-style: preserve-3d;
}

.animate-pulse-slow {
  animation: pulse 4s cubic-bezier(0.4, 0, 0.6, 1) infinite;
}

.animate-gradient-x {
  background-size: 200% 200%;
  animation: gradient-x 3s ease infinite;
}

@keyframes gradient-x {
  0% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
  100% { background-position: 0% 50%; }
}

/* Simple Particle Animation */
.particle {
  position: absolute;
  border-radius: 50%;
  background: white;
  opacity: 0;
  animation: float-up 4s infinite linear;
  pointer-events: none;
}
.p1 { width: 4px; height: 4px; left: 10%; bottom: -10px; animation-delay: 0s; }
.p2 { width: 6px; height: 6px; left: 30%; bottom: -10px; animation-delay: 1.5s; }
.p3 { width: 3px; height: 3px; left: 50%; bottom: -10px; animation-delay: 0.5s; }
.p4 { width: 5px; height: 5px; left: 70%; bottom: -10px; animation-delay: 2.5s; }
.p5 { width: 4px; height: 4px; left: 90%; bottom: -10px; animation-delay: 1s; }

@keyframes float-up {
  0% { transform: translateY(0) scale(0); opacity: 0; }
  20% { opacity: 0.6; }
  80% { opacity: 0.6; }
  100% { transform: translateY(-300px) scale(1.5); opacity: 0; }
}

/* Button Shine Animation */
@keyframes shine {
  100% {
    transform: translateX(100%);
  }
}

.group-hover\/btn\:animate-shine:hover {
  animation: shine 0.8s;
}
</style>
