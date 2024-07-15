<template>
  <div>
    <input v-model="url" placeholder="Enter company careers page URL" />
    <button @click="searchJobs">Search</button>
    <div v-if="loading">Loading...</div>
    <ul v-else>
      <li v-for="job in jobs" :key="job.title">
        {{ job.title }} - <a :href="job.link">Apply</a>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import axios from "axios";

export default defineComponent({
  name: "JobSearch",
  setup() {
    const url = ref("");
    const jobs = ref<Array<{ title: string; link: string }>>([]);
    const loading = ref(false);

    const searchJobs = async () => {
      loading.value = true;
      try {
        const response = await axios.post("http://localhost:3000/search", {
          url: url.value,
        });
        jobs.value = response.data;
      } catch (error) {
        console.error("Error searching jobs:", error);
      }
      loading.value = false;
    };

    return { url, jobs, loading, searchJobs };
  },
});
</script>
