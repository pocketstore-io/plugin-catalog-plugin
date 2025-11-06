<script setup lang="ts">
import {usePocketBase} from "@/util/pocketbase";

const props = defineProps({
  identifier: {type: String, required: true}
});
const plugin = ref({});

const pb = usePocketBase();
const load = async () => {
  plugin.value = await pb.collection('plugins').getOne(props.identifier);
}

onMounted(()=>{
  load()
});
</script>

<template>
  <div class="card bg-gray-300">
    <figure>
      <img
          :src="'https://picsum.photos/600/600?random='+plugin.id"
          alt="Shoes"/>
    </figure>
    <div class="card-body">
      <h2 class="card-title">{{plugin.name}}</h2>
      <p>{{plugin.description}}</p>
      <div class="card-actions justify-end">
        <a :href="'/de/plugin/'+plugin.name+'.html'" class="btn btn-primary">
          View
        </a>
      </div>
    </div>
  </div>
</template>