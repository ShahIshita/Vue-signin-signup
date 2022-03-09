<template>
  <div>
    <base-dialog :show="!!error" title="An error occurred" @close="loginError">
      <p>{{ error }}</p>
    </base-dialog>
    <base-card>
      <h2>Login</h2>
      <form @submit.prevent="submitForm">
        <div class="form-control">
          <label for="email">E-Mail</label>
          <input type="email" id="email" v-model.trim="email" />
        </div>
        <div class="form-control">
          <label for="password">Password</label>
          <input type="password" id="password" v-model.trim="password" />
        </div><br>
        <p v-if="!formIsValid">
          Please enter a valid email and password (must be at least 6 characters
          long).
        </p>
        <base-button>Login</base-button>
      </form>
    </base-card>
  </div>
</template>

<script>
import BaseButton from "./ui/BaseButton.vue";
import BaseCard from "./ui/BaseCard.vue";
import BaseDialog from "./ui/BaseDialog.vue";
export default {
  components: {
    BaseButton,
    BaseCard,
    BaseDialog,
  },
  data() {
    return {
      email: "",
      password: "",
      formIsValid: true,
      mode: "login",
      error: null,
    };
  },
  methods: {
    submitForm() {
      this.formIsValid = true;
      if (
        this.email === "" ||
        !this.email.includes("@") ||
        this.password.length < 6
      ) {
        this.formIsValid = false;
        return;
      }
      this.$store.dispatch("login", {
        email: this.email,
        password: this.password,
      });
      this.email = "";
      this.password = "";
      this.isLogin = "go to Dashboard";

      this.$router.replace("/dashboard");
    },

    loginError() {
      this.error = null;
    },
  },
};
</script>