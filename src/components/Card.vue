<template>
  <div class="card">
    <div class="card-body">
      <h6>{{ task.task }}</h6>
      <a @click="deleteTask(task.id)"
        ><span class="material-icons"> clear </span></a
      >
    </div>
    <div class="card-footer">
      <a v-b-toggle="'collapse-' + task.id"
        ><span class="material-icons"> edit </span></a
      >
      <div class="move-ops">
        <div v-if="category !== 'Backlog'">
          <a @click="moveLeft(task.id)"
            ><span class="material-icons"> chevron_left </span></a
          >
        </div>
        <div v-if="category !== 'Done'">
          <a @click="moveRight(task.id)"
            ><span class="material-icons"> chevron_right </span></a
          >
        </div>
      </div>
    </div>
    <b-collapse :id="`collapse-${task.id}`" class="mt-2 collapse">
      <div class="collapse-card">
        <form @submit.prevent="editTask(task.id)">
          <input v-model="title" />
          <button type="submit" class="edit-btn">
            <i>Submit</i>
          </button>
        </form>
      </div>
    </b-collapse>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Card",
  props: ["task", "category"],
  data() {
    return {
      title: "",
    };
  },
  methods: {
    deleteTask(id) {
      axios({
        method: "DELETE",
        url: `http://localhost:3001/task/${id}`,
        headers: {
          token: localStorage.getItem("kanban-token"),
        },
      })
        .then((_) => {
          this.$emit("fetch");
        })
        .catch((err) => {
          console.log(err);
        });
    },
    moveLeft(id) {
      let data = {
        id,
        category: this.category,
      };
      this.$emit("moveLeft", data);
    },
    moveRight(id) {
      let data = {
        id,
        category: this.category,
      };
      this.$emit("moveRight", data);
    },
    editTask(id) {
      let task = this.title;
      axios({
        method: "PATCH",
        url: `http://localhost:3001/task/title/${id}`,
        headers: {
          token: localStorage.getItem("kanban-token"),
        },
        data: {
          task,
        },
      })
        .then((_) => {
          this.$emit("fetch");
          this.title = "";
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style scoped>
.card {
  width: 100%;
  min-height: 100px;
  margin-top: 16px;
  padding: 8px;
  border-radius: 4px;
  background-color: #414141;
}

.card-body {
  display: flex;
  padding: 10px 5px 5px 5px;
  justify-content: space-between;
}

h6 {
  color: #fff;
}

a {
  width: 20px;
  height: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 2px;
}

.material-icons {
  color: #fff;
  font-size: 15px;
}

a:hover {
  background-color: #909090;
}

.card-footer {
  background-color: #414141;
  border: none;
  padding: 5px 5px 10px 5px;
  display: flex;
  justify-content: space-between;
}

.move-ops {
  /* width: 55px; */
  display: flex;
  justify-content: space-between;
}

form {
  width: 100%;
  height: 47px;
  border: none;
  margin: 0;
  display: flex;
  justify-content: space-between;
}

input {
  width: 180px;
  height: 35px;
}

.edit-btn {
  width: 70px;
  height: 35px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 3px;
  color: #3ea6e1;
  background-color: #414141;
  border: none;
}

.edit-btn:hover {
  border: 1px solid #3ea6e1;
}
</style>