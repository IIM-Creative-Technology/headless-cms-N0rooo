<template>
  <div v-if="projects" class=" w-screen flex flex-col items-center">
      <h1 class="text-6xl">Portfolio </h1>
      <p class="text-blue-400 text-xl">- Thomas aubert </p>

    <div class="w-10 mb-8 mt-2 border-b-slate-300 border-b-2 border-t-0 border-x-0">
    </div>
    <div v-if="types" class="flex mb-2">
      <button class=" bg-green-400 hover:bg-green-300 mx-2 mb-6 text-white rounded px-4 py-1" @click="filterProjects('all')">reset</button>
      <button class=" bg-blue-400 hover:bg-blue-300 mx-2 mb-6 text-white rounded px-4 py-1" :key="type" @click="filterProjects(type)" v-for="type in types">{{ type }}</button>
    </div>
    <div class="flex flex-wrap mx-10 justify-between max-lg:justify-center max-w-6xl ">
      <nuxt-link :to="`/projects/${project.slug}`" v-for="(project, index) in filteredProjects" :key="index" class="mx-4 ">
        <img :src="project.image.url" class=" min-w-1/4 w-64 object-cover border-2 h-36" />
        <p class="mt-2 text-blue-300 ml-4 mb-7">{{ project.name }}</p>
      </nuxt-link>
    </div>

  </div>
</template>

<script setup>
const { find } = useStrapi();
const projects = ref();
const types = ref([]);
const activeFilter = ref('all');

const filterProjects = (type) => {
  activeFilter.value = type;
}

const filteredProjects = computed(() => {
  if (activeFilter.value === 'all') {
    return projects.value.data;
  }
  return projects.value.data.filter(project => project.type === activeFilter.value);
});

onMounted(async () => {
  projects.value = await find('projects', { populate: 'deep' });
  types.value = new Set(projects.value.data.map(project => {
  return project.type
  }));
})

</script>



