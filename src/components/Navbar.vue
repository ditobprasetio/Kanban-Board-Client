<template>
  <div id="navbar">
    <h1>KANBAN.</h1>
    <a @click="signout" class="signout-btn"><span class="material-icons"> exit_to_app </span>Sign out</a>
  </div>
</template>

<script>
export default {
  name: "Navbar",
  methods: {
    signout () {
      this.$emit('signout')
    }
  }
};
</script>

<style scoped>
#navbar {
  width: 100%;
  height: 56px;
  background-color: #2f3133;
  padding: 8px 16px;
  font-family: "Anton", sans-serif;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  margin: 0;
  color: #cecece;
  background-color: #2f3133;
}

.signout-btn {
  height: 30px;
  padding: 0 5px;
  border-radius: 3px;
  display: flex;
  align-items: center;
  color: #cecece;
  text-decoration: none;
  cursor: pointer;
}

.signout-btn:hover {
  background-color: #414141;
}

.signout-btn .material-icons {
  color: #909090;
  margin-right: 10px;
  background-color: #2f3133;
}

span:hover {
  background-color: #2f3133 !important;
}
</style>