<template>
  <div class="flex">
    <div>
      <img src="@/assets/logo.png" class="logo-header">
    </div>

    <div class="flex flex-items-center">
      <nuxt-link v-if="!isSignedIn" to="/" class="nav-link">Sign In</nuxt-link>
      <nuxt-link v-if="!isSignedIn" to="/signup" class="nav-link">Sign Up</nuxt-link>
      <nuxt-link v-if="isSignedIn" to="/my_characters" class="nav-link">My Characters</nuxt-link>
      <nuxt-link v-if="isSignedIn && currentUser.user.officer" to="/rosters" class="nav-link">Rosters</nuxt-link>
      <nuxt-link v-if="isSignedIn && currentUser.user.officer" to="/basic_compos" class="nav-link">Basic compos</nuxt-link>
      <nuxt-link v-if="isSignedIn && currentUser.user.officer" to="/characters" class="nav-link">All Characters</nuxt-link>
      <a v-if="isSignedIn" href="javascript:void(0);" @click.prevent="doSignout()" class="nav-link">Sign out</a>

      <div v-if="currentUser.csrf" class="nav-link">
        Connected as: {{currentUser.user.name}}
      </div>
    </div>

  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex'

export default {
  name: 'Header',
  computed: {
    ...mapGetters({
      currentUser: 'currentUser'
    }),
    isSignedIn () {
      return this.currentUser.csrf || localStorage.csrf
    }
  },
  methods: {
    ...mapActions({
      signout: 'signout',
    }),
    setError (error, text) {
      this.error = (error.response && error.response.data && error.response.data.error) || text
    },
    doSignout () {
      this.signout().then(() => {
        delete localStorage.csrf
        delete localStorage.signedIn
        this.$router.push('/')
      })
    }
  }
}
</script>

<style scoped>
.logo-header {
  padding: 10px;
  width: 100px;
}
</style>
