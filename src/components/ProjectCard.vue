<template>
  <div class="project-card">

    

    <div class="card-content">
    <div class="card-image">
      <img :src="project.image" :alt="project.alt" />
    </div>

      <h2>{{ project.title }}</h2>

      <p>{{ project.description }}</p>

      <a v-if="project.link" :href="project.link" target="_blank">
        <button class="project-btn">
          View Project
        </button>
      </a>

      <button v-else class="project-btn" disabled>
        No Link Available
      </button>

    </div>

  </div>
</template>

<script setup>
defineProps({
  project: Object
});
</script>