<template>
  <div class="board">
    <h5>{{ category }}</h5>
    <div class="card-warper">
      <Card
        v-for="task in taskFilter"
        :key="task.id"
        :task="task"
        :category="category"
        @fetch="fetch"
        @moveLeft="moveLeft"
        @moveRight="moveRight"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Card from "./Card";

export default {
  name: "Board",
  props: ["tasks", "BoardCategory"],
  components: {
    Card,
  },
  data() {
    return {
      category: "",
    };
  },
  methods: {
    fetch() {
      this.$emit("fetch");
    },
    moveLeft(data) {
      let newCategory;
      if (data.category === "On Progress") {
        newCategory = "Backlog";
      } else if (data.category === "Need Review") {
        newCategory = "On Progress";
      } else if (data.category === "Done") {
        newCategory = "Need Review";
      }
      axios({
        method: "PATCH",
        url: `http://localhost:3001/task/${data.id}`,
        data: {
          category: newCategory,
        },
        headers: {
          token: localStorage.getItem("kanban-token"),
        },
      })
        .then((_) => {
          this.fetch();
        })
        .catch((err) => {
          console.log(err);
        });
    },
    moveRight(data) {
      let newCategory;
      console.log(data.category);
      if (data.category === "Backlog") {
        newCategory = "On Progress";
      } else if (data.category === "On Progress") {
        newCategory = "Need Review";
      } else if (data.category === "Need Review") {
        newCategory = "Done";
      }
      axios({
        method: "PATCH",
        url: `http://localhost:3001/task/${data.id}`,
        data: {
          category: newCategory,
        },
        headers: {
          token: localStorage.getItem("kanban-token"),
        },
      })
        .then((_) => {
          this.fetch();
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
  created() {
    this.fetch();
    if (this.BoardCategory === "Backlog") this.category = "Backlog";
    else if (this.BoardCategory === "On Progress")
      this.category = "On Progress";
    else if (this.BoardCategory === "Need Review")
      this.category = "Need Review";
    else if (this.BoardCategory === "Done") this.category = "Done";
  },
  computed: {
    taskFilter() {
      return this.tasks.filter((task) => task.category === this.category);
    },
  },
};
</script>

<style scoped>
.board {
  min-width: 300px;
  max-width: 300px;
  height: fit-content;
  max-height: 100%;
  margin: 16px;
  padding: 16px;
  border-radius: 5px;
  background-color: #2d2d2d;
}

h5 {
  color: #fff;
  margin: 0;
  background-color: #2d2d2d;
}

.card-warper {
  max-height: 80vh;
  padding-right: 5px;
  overflow: auto;
}
</style>