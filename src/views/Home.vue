<template>
  <div class="home">
    <filter-nav @filterChanged="filter = $event" :current="filter" />
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <single-project :project="project" :apiUrl="apiUrl" @onDelete="onDelete" />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue'
export default {
  name: 'Home',
  components: {
    SingleProject,
    FilterNav
  },
  data() {
    return {
      projects: [],
      allProjects: [],
      apiUrl: 'http://localhost:3000/projects/',
      filter: 'all'
    }
  },
  mounted () {
    this.fetchData();
  },
  methods: {
    onDelete(id) {
      this.projects = this.projects.filter(p => p.id !== id);
    },
    onComplete(id) {
      const project = this.projects.find(p => p.id === id);
      if (project)
        project.complete = !project.complete;
    },
    fetchData() {
      fetch(this.apiUrl)
      .then(res => res.json())
      .then(data => this.projects = data)
      .catch(err => console.error(err));
    }
  },
  computed: {
    filteredProjects() {
      switch (this.filter)
      {
        case 'complete': return this.projects.filter(el => el.complete);
        case 'ongoing': return this.projects.filter(el => !el.complete);
        default: return this.projects;
      }
    }
  },
}
</script>
