<template>
  <div id="projects" class="pt-5 pb-0">

    <div class="container mt-5 mb-5">

      <h1 class="text-center mb-5">PROJECTS</h1>

      <h3 class="section-desc">
        A showcase of my work in building scalable, functional, and user-friendly applications.
      </h3>

      <div class="row g-4 justify-content-center">

        <div
          v-for="project in projects"
          :key="project.id"
          class="col-12 col-md-6 col-lg-4"
        >
          <ProjectCard :project="project" />
        </div>

      </div>

    </div>

  </div>
</template>

<script setup>
import ProjectCard from './ProjectCard.vue'
import projects from '../data/projects.json'
</script>