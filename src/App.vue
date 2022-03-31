<template>
  <section class="app-wrapper">
    <header class="col-sm-12 roku-header">
      <img src="@/assets/images/roku.svg" alt="Roku logo" width="150"><span>FlashBack</span>
      <nav class="float-right">
      <ul v-if="authenticated">            
        <li><i class="fas fa-user-circle"></i></li>
        <li v-if="role === 1"><i class="fas fa-cog"></i></li>
        <li><i class="fas fa-power-off"></i></li>					
      </ul>
      </nav>
    </header>

    <router-view :key="$route.path" :loggedin="authenticated" :role="role" @setauth="setAuthenticated"></router-view>
  </section>
</template>

<script>
export default {
  name: "TheMainApp",

  data() {
    return {
      authenticated: false,
      role: 0
    }
  },

  methods: {
    setAuthenticated(status) {
      debugger;
      this.authenticated = status.status;
      this.role = status.role;
    }
  }
}
</script>

<style lang="scss">
  @import "@/assets/sass/main.scss";
</style>
