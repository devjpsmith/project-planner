<template>
  <div class="home">
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <single-project :project="project" :apiUrl="apiUrl" @onDelete="onDelete" />
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import SingleProject from '../components/SingleProject.vue'
export default {
  name: 'Home',
  components: {
    SingleProject
  },
  data() {
    return {
      projects: [],
      apiUrl: 'http://localhost:3000/projects/'
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
  }
}
</script>
