<template>
  <div>
    <LandingPage
    :currentPage='currentPage'
    @changePage='changePage'
    ></LandingPage>
    <Dashboard
    :currentPage='currentPage'
    ></Dashboard>
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
    signinStatus: function () {
      let token = localStorage.getItem("kanban-token");
      if (token) {
        this.currentPage = "Dashboard";
      } else {
        this.currentPage = "Landingpage";
      }
    },
    changePage: function (page) {
      this.currentPage = page
    }
  },
  created() {
    this.signinStatus()
  }
};
</script>

<style>

</style>