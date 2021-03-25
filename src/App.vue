<template>
  <div>
    <LandingPage
      :currentPage="currentPage"
      @changePage="changePage"
    ></LandingPage>
    <Dashboard :currentPage="currentPage" @signout="signout"></Dashboard>
  </div>
</template>

<script>
import Dashboard from "./views/Dashboard";
import LandingPage from "./views/LandingPage";

export default {
  name: "App",
  components: {
    LandingPage,
    Dashboard,
  },
  data() {
    return {
      currentPage: "Landingpage",
    };
  },
  methods: {
    signinStatus() {
      let token = localStorage.getItem("kanban-token");
      if (token) {
        this.currentPage = "Dashboard";
      } else {
        this.currentPage = "Landingpage";
      }
    },
    changePage(page) {
      this.currentPage = page;
    },
    signout() {
      this.currentPage = "Landingpage";
      localStorage.clear();
    },
  },
  created() {
    this.signinStatus();
  },
};
</script>

<style>

</style>