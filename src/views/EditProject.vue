<template>
  <form @submit.prevent="updateProject">
    <label>Title:</label>
    <input type="text" v-model="title" required />
    <label for="">Details:</label>
    <textarea v-model="details" required></textarea>
    <button>Update Project</button>
  </form>
</template>

<script>
import axios from 'axios';

export default {
  props: ['id'],
  data() {
    return {
      title: '',
      details: '',
      url: 'http://localhost:3000/projects/' + this.id,
    };
  },
  methods: {
    async updateProject() {
      const project = {
        title: this.title,
        details: this.details,
      };

      try {
        await axios.patch(this.url, project);
        this.$router.push('/');
      } catch (error) {
        console.log(error);
      }
    },
  },
  async created() {
    try {
      const res = await axios.get(this.url);
      const project = res.data;

      this.title = project.title;
      this.details = project.details;
    } catch (error) {
      console.log(error);
    }
  },
};
</script>

<style scoped>
form {
  background: white;
  padding: 20px;
  border-radius: 10px;
}
label {
  display: block;
  color: #bbb;
  text-transform: uppercase;
  font-size: 14px;
  font-weight: bold;
  letter-spacing: 1px;
  margin: 20px 0 10px 0;
}
input {
  padding: 10px;
  border: 0;
  border-bottom: 1px solid #ddd;
  width: 100%;
  box-sizing: border-box;
}
textarea {
  border: 1px solid #ddd;
  padding: 10px;
  width: 100%;
  box-sizing: border-box;
  height: 100px;
}
form button {
  display: block;
  margin: 20px auto 0;
  background: #00ce89;
  color: white;
  padding: 10px;
  border: 0;
  border-radius: 6px;
  font-size: 16px;
}
</style>
