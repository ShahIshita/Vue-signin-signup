<template>
  <div>
    <base-dialog :show="!!error" title="An error occurred" @close="handleError">
      <p>{{ error }}</p>
    </base-dialog>
    <base-card>
      <h2>Registration</h2>
      <form @submit.prevent="submitForm">
        <div class="form-control">
          <label for="userName">Name</label>
          <input typ="text" id="userName" v-model.trim="userName" required />
         
        </div>
        <div class="form-control">
          <label for="email">E-Mail</label>
          <input type="email" id="email" v-model.trim="email" />
        </div>
        <div class="form-control">
          <label for="password">Password</label>
          <input type="password" id="password" v-model.trim="password" />
          <p v-if="!!invalidPassword">{{ invalidPassword }}</p>
        
        </div>
        <div class="form-control">
          <label for="confirmpassword">Confirm Password</label>
          <input type="password" id="confirmpassword" v-model.trim="confirmPassword" />
        </div><br>
        <base-button>SignUp</base-button>
      </form>
    </base-card>
  </div>
</template>

<script>

import BaseButton from './ui/BaseButton.vue';
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
      userName: "",
      email: "",
      password: "",
      confirmPassword: "",
      invalidPassword: "",
      formIsValid: true,
      error: null,
    };
  },

  methods: {
    submitForm() {
      this.formIsValid = true;
     // console.log('hello');

      if( this.password.length< 6 ){
        this.invalidPassword = 'Length must be 6 letters';
      }
      if(this.userName === "" || !this.email.includes("@") || this.password.length< 6 || !this.confirmPasswordValidate)
      {
        this.formIsValid = false;
        return;
     }

      this.$store.dispatch('registration', {
                userName: this.userName,
                email: this.email,
                password: this.password,
            });
     
      this.userName = "";
      this.email = "";
      this.password = "";
      this.confirmpassword = "";
      this.error = "go to login";
    this.$router.replace('/login');
    
    },
    handleError() {
      this.error = null;
    },
  },
  computed: {
  
    confirmPasswordValidate() {
      if (this.password != this.confirmPassword) 
      {
        return false;
      }
      return true;
    },
  },
};
</script>