<template>
  <section class="container">
    <div class="jumbotron">
        <h1>Welcome to Flashblack!</h1>
        <p class="lead">
        Before revisiting your favourite movies, tv shows or music from yesteryear, please log in with a valid username and password.
        </p>
    </div>

    <section class="log-in">
      <label class="sr-only" for="inlineFormInputName">Name</label>
      <input v-model="username" type="text" class="form-control" id="inlineFormInputName" placeholder="username" required>

      <label class="sr-only" for="inlineFormPassword">Name</label>
      <input v-model="password" type="password" class="form-control" id="inlineFormPassword" placeholder="password" required>
    </section>

    <button
        type="submit" 
        class="btn btn-primary login-submit"
        @click="login"
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
      password: ''
    }
  },

  methods: {
    login() {
      // hit the back end UMS with a user name and password

      //debugger;

      // mock up a succesful login
      // navigate to the next view
      // TODO => this only happens on successful login, so move it when ready
      this.$router.push({ name: 'UserSelect'});

      /*
      let formData = new FormData();

      formData.append("username", this.username);
      formData.append("password", this.password);

      let url = `someloginurl`; // todo - fix this when available

      fetch(url, {
        method: 'POST',
        body: formData
      })
      .then(res => res.json())
      .then(data => {
        console.log(data);
        // either pass or fail our login attempt
        // if it passes, then get the user data and navigate to the next view
        // if not, then flash an alert and ask to a login retry
      })
      .catch((error) => console.error(error)); */
    }
     
  }
}
</script>

<style lang="scss">
  @import "@/assets/sass/login.scss";
</style>
