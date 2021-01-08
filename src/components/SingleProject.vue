<template>
  <div class="project" :class="{ complete: project.complete }">
    <div class="actions">
      <h3 @click="toggleDetails">{{ project.title }}</h3>
      <div class="icons">
        <router-link :to="{ name: 'EditProject', params: { id: project.id } }"
          ><span class="material-icons"> edit </span></router-link
        >
        <span class="material-icons" @click="deleteProject">
          delete
        </span>
        <span class="material-icons tick" @click="toggleComplete">
          done
        </span>
      </div>
    </div>
    <div class="details" v-if="showDetails">{{ project.details }}</div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: ['project'],
  emits: ['delete', 'complete'],
  data() {
    return {
      showDetails: false,
      url: 'http://localhost:3000/projects/' + this.project.id,
    };
  },
  methods: {
    toggleDetails() {
      this.showDetails = !this.showDetails;
    },
    async deleteProject() {
      try {
        await axios.delete(this.url);
        this.$emit('delete', this.project.id);
      } catch (err) {
        console.log(err);
      }
    },
    async toggleComplete() {
      try {
        await axios.patch(this.url, { complete: !this.project.complete });
        this.$emit('complete', this.project.id);
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>

<style scoped>
.project {
  margin: 20px auto;
  background: white;
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

.project.complete {
  border-left: 4px solid #00ce89;
}

.project.complete .tick {
  color: #00ce89;
}
</style>
