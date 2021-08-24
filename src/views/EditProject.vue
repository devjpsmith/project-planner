<template>
  <form @submit.prevent="handleSave">
    <label>Title:</label>
    <input type="text" v-model="title" required />
    <label>Details:</label>
    <textarea v-model="details" required></textarea>
    <button>Save</button>
  </form>
</template>

<script>
export default {
    props: [ 'id' ],
 data() {
     return {
         title: '',
         details: '',
      apiUrl: 'http://localhost:3000/projects/'
     }
 },
 mounted () {
     fetch(`${this.apiUrl}${this.id}`)
        .then(res => res.json())
        .then(data => {
            this.title = data.title;
            this.details = data.details;
        })
        .catch(err => console.error(err));
 },
 methods: {
     handleSave() {
         const project = {
             title: this.title,
             details: this.details
         };
         fetch(`${this.apiUrl}${this.id}`, {
             method: 'PATCH',
             headers: { 'Content-Type': 'application/json'},
             body: JSON.stringify(project)
         })
            .then(() => this.$router.push('/'))
            .catch(err => console.error(err));
     }
 },
}
</script>

<style>

</style>