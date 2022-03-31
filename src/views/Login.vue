<template>
  <section class="container">
    <div class="jumbotron">
        <h1>Welcome to Flashblack!</h1>
        <p class="lead">
        Before revisiting your favourite movies, tv shows or music from yesteryear, please log in with a valid username and password.
        </p>
    </div>

    <h2 class="login-flash" v-if="signup">{{ flash }}</h2>
    <h2 class="login-flash" v-if="errors">{{ flash }}</h2>

    <section class="log-in">
      <label class="sr-only" for="inlineFormInputName">Name</label>
      <input v-model="username" type="text" class="form-control" id="inlineFormInputName" placeholder="username" required>

      <label class="sr-only" for="inlineFormPassword">Name</label>
      <input v-model="password" type="password" class="form-control" id="inlineFormPassword" placeholder="password" required>
    </section>

    <button
        v-if="signup"
        type="submit" 
        class="btn btn-primary login-submit signup"
        @click="trySignUp"
      >Sign Up
    </button>

     <button
        type="submit" 
        class="btn btn-primary login-submit"
        @click="tryLogin"
      >Go!
    </button>
  </section>
</template>

<script>
export default {
  name: 'TheLogin',

  data() {
    return {
      username: '',
      password: '',
      url: 'users/getone',
      signup: false,
      errors: false,
      flash: ''
    }
  },

  methods: {
    tryLogin() { 
      this.url = 'users/getone';
      this.login();
    },

    trySignUp() {
      this.url = 'users/signup';
      this.login();
    },

    goToUsers(time, vm, role) {
      setTimeout(function() {
        vm.$emit('setauth', { status: true, role: role});
        vm.$router.push({ name: 'UserSelect'});
      }, time)
    },

    login() {
      // hit the back end UMS with a user name and password
      let formData = { username: this. username, password: this.password }

      let url = this.url;

      fetch(url, {
        method: 'POST',
        headers: {
          "Content-type" : "application/json"
        },
        body: JSON.stringify(formData)
      })
      .then(res => res.json())
      .then(data => {
        console.log(data);
        
        switch (data.action) {
          //  login failed -> user does not exist
          case 'add':
            this.signup = true;
            this.username = '';
            this.password = '';
            this.flash = `Hmmm... your username doesn't seem to exist. Do you want to sign up? Or you can try again.`;
            break;

          // successfully added a user
          case 'added':
            this.flash = 'Added you to Roku Flashback! Enjoy! ... redirecting ...';
            this.goToUsers(2500, this, data.role);
            break;

          // login failed b/c wrong username or password
          case 'retry':
            document.querySelector(`input[type=${data.field}]`).classList.add('error');
            console.log('retry');
            this.errors = true;
            this.flash = `Your login info is not correct. Please retry.`;
            break;

          // login successful -> user authenticated
          default:
            this.goToUsers(0, this, data.role);
        }
      })
      .catch((error) => console.error(error));
    }
     
  }
}
</script>

<style lang="scss">
  @import "@/assets/sass/login.scss";
</style>
