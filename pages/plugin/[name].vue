<template>
  <section class="bg-white mx-auto max-w-6xl px-3 py-3">
    <section class="container mx-auto px-6 py-12 grid grid-cols-1 md:grid-cols-2 gap-10">

      <!-- Product Image -->
      <div class="flex justify-center">
        <img src="https://picsum.photos/600/600"
             alt="Product Image"
             class="rounded-2xl w-full max-w-md object-cover shadow-md">
      </div>

      <!-- Product Details -->
      <div class="flex flex-col justify-center space-y-6">
        <h2 class="text-3xl md:text-4xl font-bold">
          Plugin {{ name }}
        </h2>
        <p class="text-gray-600 leading-relaxed">
          {{ plugin.description }}
        </p>
        <p class="versions">
          <label class="floating-label">
            <span>Version</span>
            <select class="select w-full">
              <option value="latest">Latest</option>
              <option v-for="version in plugin.versions" :value="version">{{ version }}</option>
            </select>
          </label>
        </p>

        <div v-if="plugin.price" class="flex items-center">
          <span class="text-3xl font-bold text-primary">Preis: {{ plugin.price?.toFixed(2) }} €</span>
          <span class="text-gray-400 line-through">vorher: {{ (plugin.price + 5).toFixed(2) }} €</span>
        </div>
        <div v-else class="alert alert-info block text-center">
          Das Plugin ist kostenlos verfügbar.
        </div>

        <div v-if="plugin.price" class="flex items-center space-x-4">
          <button class="btn btn-primary">
            <font-awesome-icon :icon="['fas', 'cart-plus']"/>
            <span class="ml-3">Add to Cart</span>
          </button>
          <button class="btn btn-secondary">
            <font-awesome-icon :icon="['fas', 'heart']"/>
            <span>
              Add to Wishlist
            </span>
          </button>
        </div>
        <div v-else class="space-y-3">
          <button class="btn btn-primary btn-block">
            <font-awesome-icon :icon="['fas', 'cloud-arrow-down']"/>
            <span>
              Download
            </span>
          </button>
          <button class="btn btn-secondary btn-block">
            Copy new Item for custom/plugins.json
          </button>
        </div>
      </div>
    </section>

    <!-- Product Description -->
    <section class="grid grid-cols-6 gap-3">
      <div v-for="block in blocks" class="col-span-6 md:col-span-3">
        <section class="container mx-auto px-6 py-12">
          <h2 class="text-2xl font-semibold mb-4">{{ block.name }}</h2>
          <div v-html="block.content"></div>
        </section>
      </div>
    </section>

    <!-- Related Products -->
    <section v-if="related.related?.length > 0" class="container mx-auto px-6 py-12 space-y-3">
      <h2 class="text-2xl font-semibold mb-6 text-center">Related Products</h2>
      <section class="grid grid-cols-6 gap-3">
        <PluginCard :identifier="plugin" v-for="plugin in related.related"
                    class="col-span-6 lg:col-span-2 md:col-span-3"/>
      </section>
    </section>

  </section>
</template>
<script setup lang="ts">
import {FontAwesomeIcon} from '@fortawesome/vue-fontawesome'
import {useRoute} from 'vue-router'
import {usePocketBase} from "@/util/pocketbase";
import PluginCard from "@/components/PluginCard.vue";

const route = useRoute();
const pb = usePocketBase();

const plugin = ref({});
const blocks = ref([]);
const related = ref([]);

const name = computed(()=>{
  return route.params.name.replace('.html', '');
});

const load = async () => {
  let response = await fetch('https://download.pocketstore.io/plugin/' + name.value);
  plugin.value = await response.json();

  blocks.value = (await pb.collection('plugin_blocks').getList(1, 25, {filter: 'plugin = "' + plugin.value.id + '"'})).items;
  related.value = await pb.collection('plugin_related').getFirstListItem('plugin = "' + plugin.value.id + '"');
}

onMounted(() => {
  load();
});
</script>