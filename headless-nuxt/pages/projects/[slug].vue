<template>
    <div v-if="project" class="flex flex-col justify-center items-center">
        <h1 class="text-5xl max-w-lg text-center">{{ project.name }} </h1>
        <div v-if="project.technologies" class="flex flex-wrap justify-start ">
            <div class="flex mb-4 mt-5">
                <img v-for="(technology, index) in project.technologies" :key="index" :src="technology.image.url" alt="" class="w-10 h-auto object-contain">
            </div>

        </div>

        <div class="w-10 mb-8 border-b-slate-300 border-b-2 border-t-0 border-x-0">
        </div>
        <div class="flex flex-col items-center">
            <img :src="project.image.url" alt="" width="500" class="border-2">
            <p class="mt-8 max-w-2xl" v-if="project.description" v-html="$mdRenderer.render(project.description)"></p>
        </div>
        <nuxt-link v-if="project.link" :to="project.link" class="text-white ml-4 bg-blue-400 rounded px-4 py-1 hover:bg-blue-300 mt-4 ">GO</nuxt-link>
        <button v-else disabled class="text-white ml-4 bg-red-600 rounded px-4 py-1 cursor-not-allowed mt-4">Unavailable</button>
        <nuxt-link to="/" class="text-blue-400 ml-4 absolute top-8 left-8 ">Return </nuxt-link>
    </div>
</template>

<script setup>
const { findOne } = useStrapi()
const route = useRoute()
const project = ref()

onMounted(async () => {
    project.value = await findOne(`projects?filters[slug]=${route.params.slug}&populate=deep`);
    project.value = project.value.data[0]
})
</script>