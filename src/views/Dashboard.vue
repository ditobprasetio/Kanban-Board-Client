<template>
  <div id="dashboard" v-if="currentPage === 'Dashboard'">
    <Navbar @signout="signout"></Navbar>
    <div id="head">
      <h6 style="margin: 0">My task</h6>
      <a class="add-btn" v-b-toggle.collapse-add
        ><span class="material-icons"> add </span>Add a card</a
      >
    </div>
    <b-collapse id="collapse-add" class="mt-2 collapse">
      <div class="collapse-card">
        <form @submit.prevent="addTask()">
          <input v-model="title" />
          <button type="submit" class="submit-btn">
            <i>Submit</i>
          </button>
        </form>
      </div>
    </b-collapse>
    <div id="main-content">
      <Board
        @fetch="fetch"
        :tasks="tasks"
        v-for="(category, index) in categories"
        :key="index"
        :BoardCategory="category"
      />
    </div>
  </div>
</template>

<script>
import Navbar from "../components/Navbar";
import Board from "../components/Board";
import axios from "axios";

export default {
  name: "Dashboard",
  components: {
    Navbar,
    Board,
  },
  props: ["currentPage"],
  data() {
    return {
      categories: ["Backlog", "On Progress", "Need Review", "Done"],
      tasks: [],
      title: "",
    };
  },
  methods: {
    signout() {
      this.$emit("signout");
    },
    fetch() {
      axios({
        methods: "GET",
        url: "http://localhost:3001/task",
        headers: {
          token: localStorage.getItem("kanban-token"),
        },
      })
        .then(({ data }) => {
          this.tasks = data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
    addTask() {
      console.log(this.tilte);
      axios({
        method: "POST",
        url: "http://localhost:3001/task",
        data: {
          task: this.title
        },
        headers: {
          token: localStorage.getItem("kanban-token")
        }
      })
        .then(task => {
          this.fetch()
          this.title = "";
        })
        .catch(err => {
          console.log(err);
        });
    },
  },
};
</script>

<style>
#dashboard {
  width: 100%;
  height: 100vh;
  background-color: #252525;
}

#head {
  width: 100%;
  margin-top: 12px;
  display: flex;
  color: #fff;
  padding: 0 16px;
  justify-content: space-between;
  align-items: center;
}

.add-btn {
  height: 30px;
  font-size: 1rem;
  padding: 0 5px;
  border-radius: 3px;
  display: flex;
  align-items: center;
  color: #fff;
  text-decoration: none;
  cursor: pointer;
}

.add-btn:hover {
  background-color: #414141;
  color: #fff;
  text-decoration: none;
}

.add-btn .material-icons {
  color: #909090;
  margin-right: 10px;
  background-color: #252525;
}

.add-btn .material-icons :hover {
  background: #252525 !important;
}

#collapse-add {
  padding: 0 16px;
}

form {
  width: 100%;
  height: 47px;
  border: none;
  margin: 0;
  display: flex;
}

input {
  width: 230px;
  height: 35px;
}

.submit-btn {
  width: 70px;
  height: 35px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 3px;
  color: #3ea6e1;
  background-color: #252525;
  border: none;
}

.submit-btn:hover {
  border: 1px solid #3ea6e1;
}

#main-content {
  width: 100%;
  display: flex;
  overflow-y: auto;
  background-color: #252525;
}
</style>