<template>
  <div class="home">
    <filter-nav @filter-change="filter = $event" :filter="filter"></filter-nav>
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <single-project
          :project="project"
          @delete="onDelete"
          @complete="onComplete"
        ></single-project>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import SingleProject from '../components/SingleProject';
import FilterNav from '../components/FilterNav';

export default {
  name: 'Home',
  components: { SingleProject, FilterNav },
  data() {
    return {
      projects: [],
      filter: 'all',
    };
  },
  computed: {
    filteredProjects() {
      if (this.filter === 'completed') {
        return this.projects.filter((project) => project.complete === true);
      } else if (this.filter === 'ongoing') {
        return this.projects.filter((project) => project.complete === false);
      } else {
        return this.projects;
      }
    },
  },
  methods: {
    onDelete(id) {
      this.projects = this.projects.filter((project) => project.id !== id);
    },
    onComplete(id) {
      const project = this.projects.find((project) => project.id === id);
      project.complete = !project.complete;
    },
  },
  async mounted() {
    try {
      const res = await axios.get('http://localhost:3000/projects');

      this.projects = res.data;
    } catch (err) {
      console.log(err);
    }
    console.log(process.env.BASE_URL);
  },
};
</script>
