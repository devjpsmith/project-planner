<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 :class="{ complete: project.complete }" @click="toggleDetails">
        {{ project.title }}
      </h3>
      <div class="icons">
        <span class="material-icons">edit</span>
        <span class="material-icons" @click="deleteProject">delete</span>
        <span
          class="material-icons"
          :class="{ complete: project.complete }"
          @click="completeProject"
          >done</span
        >
      </div>
    </div>
    <div class="details" v-if="showDetails">
      <p>{{ project.details }}</p>
    </div>
  </div>
</template>

<script>
export default {
  props: ["project", "apiUrl"],
  data() {
    return {
      showDetails: false,
    };
  },
  methods: {
    toggleDetails() {
      this.showDetails = !this.showDetails;
    },
    deleteProject() {
      fetch(this.projectUrl, { method: "DELETE" })
        .then((res) => {
          if (res.status === 200) this.$emit("onDelete", this.project.id);
        })
        .catch((err) => console.error(err));
    },
    completeProject() {
      const project = Object.assign(this.project, { complete: !this.project.complete });
      fetch(this.projectUrl, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(project),
      })
        .then((res) => {
          console.log(res);
          if (res.status === 204) {
            this.$emit('onComplete', this.project.id);
          }
        })
        .catch((err) => console.error(err));
    },
  },
  computed: {
    projectUrl() {
      return `${this.apiUrl}${this.project.id}`;
    },
  },
};
</script>

<style>
    :root {
    --complete-color: #00ce89;
    }
    .project {
    margin: 20px auto;
    background: #fff;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
    border-left: 4px solid #e90074;
    }

    h3 {
    cursor: pointer;
    }

    .actions {
    display: flex;
    justify-content: space-between;
    align-items: center;
    }

    .material-icons {
    font-size: 24px;
    margin-left: 10px;
    color: #bbb;
    cursor: pointer;
    }

    .material-icons:hover {
    color: #777;
    }
    h3.complete {
    text-decoration: line-through;
    }
    .material-icons.complete {
    color: var(--complete-color);
    }
    div.complete {
    border-left: 4px solid var(--complete-color);
    }
</style>