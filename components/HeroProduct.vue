<template>
  <div class="hero relative px-0 pt-12 pb-6">

    <img
        :src="'https://picsum.photos/1600/600?random='+props.index"
        alt="Hero background"
        class="absolute inset-0 h-full w-full object-cover"
    />
    <div class="absolute inset-0 bg-black/40"></div>
    <div v-if="props.plugin" class="hero-content text-center relative z-10 text-white">
      <div class="max-w-md">
        <h1 class="text-3xl font-bold mt-6">{{ props.plugin.name }}</h1>
        <p class="py-6">
          {{ props.plugin.description }}
        </p>
        <section class="actions space-x-3">
          <a :href="'/de/plugin/'+props.plugin.name+'.html'" class="btn btn-primary">
            {{$t('btn.view')}}
          </a>
          <a target="_blank" :href="'https://'+plugin.host+'/'+plugin.vendor+'/plugin-'+plugin.name" class="btn btn-primary">
            {{$t('btn.code')}}
          </a>
        </section>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
const props = defineProps({
  plugin: {type: Object, required: true},
  index: {type: Number, required: true},
});
</script>