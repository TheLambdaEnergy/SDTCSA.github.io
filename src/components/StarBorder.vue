<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  as?: string;
  color?: string;
  speed?: string;
  thickness?: number;
  className?: string;
}

const props = withDefaults(defineProps<Props>(), {
  as: 'button',
  color: 'white',
  speed: '6s',
  thickness: 2,
  className: '',
});

const outerStyle = computed(() => ({
  padding: `${props.thickness}px`,
}));

const starStyle = computed(() => ({
  background: `radial-gradient(circle, ${props.color}, transparent 10%)`,
  animationDuration: props.speed,
}));
</script>

<template>
  <component
    :is="as"
    class="relative inline-block overflow-hidden rounded"
    :style="outerStyle"
  >
    <div
      class="absolute w-[300%] h-[50%] opacity-70 bottom-[-11px] right-[-250%] rounded-full animate-star-movement-bottom z-0"
      :style="starStyle"
    ></div>
    <div
      class="absolute w-[300%] h-[50%] opacity-70 top-[-10px] left-[-250%] rounded-full animate-star-movement-top z-0"
      :style="starStyle"
    ></div>
    <div 
      class="relative z-10" 
      :class="className"
    >
      <slot></slot>
    </div>
  </component>
</template>
