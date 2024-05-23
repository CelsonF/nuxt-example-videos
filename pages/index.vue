<template>
    <div>
        <h1>Videos</h1>
        <div v-if="video">
            <h1>{{ video.code }}</h1>
            <iframe :src="video.video_url" width="560" height="315" frameborder="0" autoplay="true" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen ></iframe>
        </div>
        <div v-else>
            <p>Video not found.</p>
        </div>
    </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { useRoute } from 'vue-router';

const route = useRoute();
const video = ref<{ code: string, video_url: string } | null>(null);

const response = await axios.get('http://localhost:3000/videos');

onMounted(async () => {
    const codeParam = route.query.ref;
    if (!codeParam) return;

    try {
        const videos = response.data;
        video.value = videos.find((v: { code: string }) => v.code === codeParam) || null;

    } catch (error) {
        console.error('Error fetching videos:', error);
    }
});
</script>

<style scoped>
ul {
    list-style-type: none;
    padding: 0;
}

li {
    margin: 10px 0;
}
</style>