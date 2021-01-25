<template>
  <div>
                <h1>Laravue Santum Login</h1>

    <form @submit.prevent="login">
      <div>
        <label>Email：</label>
        <input type="text" v-model="email" />
        <span v-if="errors.email">
          {{ errors.email[0] }}
        </span>
      </div>
      <br />
      <div>
        <label>密碼：</label>
        <input type="password" v-model="password" />
        <span v-if="errors.password">
          {{ errors.password[0] }}
        </span>
      </div>
      <br />
      <button>登入</button>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: "",
      password: "",
      errors: [],
    };
  },
  methods: {
    login() {
      axios.get("/sanctum/csrf-cookie").then((response) => {
        axios
          .post("/api/login", {
            email: this.email,
            password: this.password,
          })
          .then((response) => {
            console.log(response);
            localStorage.setItem("auth", "ture");
            this.$router.push("/about");
          })
          .catch((error) => {
            this.errors = error.response.data.errors;
          });
      });
    },
  },
};
</script>