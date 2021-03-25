<template>
  <div v-if="currentPage === 'Landingpage'">
    <!-- signup page -->
    <div id="signup-page" v-if="!signupStatus">
      <div class="signup-box">
        <h5 style="margin-top: 20px">Create your Kanban Account</h5>
        <h6>to contiue to Kanban</h6>
        <b-form
          class="signup-form"
          style="margin-top: 20px"
          @submit.prevent="signup()"
        >
          <b-form-group
            id="input-group-1"
            label="Username:"
            label-for="signup-username"
            class="label"
          >
            <b-form-input
              id="signup-username"
              v-model="username"
              type="text"
              placeholder="Enter username"
              required
            ></b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-2"
            label="Email address:"
            label-for="signup-email"
            class="label"
          >
            <b-form-input
              id="signup-email"
              v-model="email"
              type="email"
              placeholder="Enter email"
              required
            ></b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-2"
            label="Password:"
            label-for="signup-password"
            class="label"
          >
            <b-form-input
              id="signup-password"
              v-model="password"
              type="password"
              placeholder="Enter Password"
              required
            ></b-form-input>
          </b-form-group>
          <div class="signup-ops">
            <a class="signin-btn" @click="signinPage">Sign in instead</a>
            <b-button type="submit" class="btn">Sign up</b-button>
          </div>
        </b-form>
      </div>
    </div>
    <!-- end -->

    <!-- signin page -->
    <div id="signin-page" v-if="signupStatus">
      <div class="signin-box">
        <h5 style="margin-top: 20px">Sign in</h5>
        <h6>to contiue to Kanban</h6>
        <b-form
          class="signin-form"
          style="margin-top: 20px"
          @submit.prevent="signin()"
        >
          <b-form-group
            id="input-group-2"
            label="Email address:"
            label-for="email"
            class="label"
          >
            <b-form-input
              id="email"
              v-model="email"
              type="email"
              placeholder="Enter email"
              required
            ></b-form-input>
          </b-form-group>

          <b-form-group
            id="input-group-2"
            label="Password:"
            label-for="password"
            class="label"
          >
            <b-form-input
              id="password"
              v-model="password"
              type="password"
              placeholder="Enter Password"
              required
            ></b-form-input>
          </b-form-group>
          <div class="signup-ops">
            <a class="signup-btn" @click="signupPage">Create account</a>
            <b-button type="submit" class="btn">Sign in</b-button>
          </div>
        </b-form>
      </div>
    </div>
    <!-- end -->
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Landingpage",
  props: ["currentPage"],
  data() {
    return {
      username: "",
      email: "",
      password: "",
      signupStatus: true,
    };
  },
  methods: {
    signupPage() {
      this.signupStatus = false;
    },
    signinPage() {
      this.signupStatus = true;
    },
    signup() {
      axios({
        method: "POST",
        url: "http://localhost:3001/signup",
        data: {
          username: this.username,
          email: this.email,
          password: this.password,
        },
      })
        .then(({ data }) => {
          console.log(data);
          localStorage.setItem("kanban-token", data.token);
          this.$emit("changePage", "Dashboard");
          this.signupStatus = true;
          this.username = "";
          this.email = "";
          this.password = "";
        })
        .catch((err) => {
          console.log(err);
        });
    },
    signin() {
      axios({
        method: "POST",
        url: "http://localhost:3001/signin",
        data: {
          email: this.email,
          password: this.password,
        },
      })
        .then(({ data }) => {
          console.log(data);
          localStorage.setItem("kanban-token", data.token);
          this.$emit("changePage", "Dashboard");
          this.email = "";
          this.password = "";
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style scoped>
#signup-page,
#signin-page {
  width: 100vw;
  height: 100vh;
  background-color: #252525;
  display: flex;
  justify-content: center;
  align-items: center;
}

.signup-box {
  width: 350px;
  height: 550px;
  padding: 20px;
  background-color: #2d2d2d;
  border-radius: 5px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.signin-box {
  width: 350px;
  height: 450px;
  padding: 20px;
  background-color: #2d2d2d;
  border-radius: 5px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

h5,
h6 {
  color: #fff;
}

.signup-form,
.signin-form {
  display: flex;
  flex-direction: column;
}

.label {
  margin-top: 13px;
  color: #cecece;
}

.signup-ops {
  margin-top: 45px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.signup-btn,
.signin-btn {
  color: #3ea6e1;
  cursor: pointer;
  padding: 7px;
}

.signup-btn:hover,
.signin-btn:hover {
  color: #3ea6e2;
  cursor: pointer;
  text-decoration: none;
  background-color: #414141;
  border-radius: 3px;
}

.btn {
  background: #3ea6e1 !important;
  padding: 7px;
}
</style>