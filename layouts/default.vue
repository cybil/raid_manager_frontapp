<template>
  <div id="app" class="global-bg">
    <Header />
    <div v-if="globalError" class="alert alert-danger col-md-11 mx-auto">{{ globalError }}</div>
    <Nuxt />
  </div>
</template>

<script>
import Header from '@/components/Header.vue'
import { mapGetters, mapActions } from 'vuex'

export default {
  name: 'App',
  components: {
    Header
  },
  created() {
    let that = this
    this.initializeStore()
    .then(() => {
      if (that.currentUser !== undefined && that.currentUser !== null) {
        that.getUserCharacters()
        if (that.currentUser.user.officer) {
          that.getAllCharacters()
          that.getRosters()
          that.getBasicCompos()
        }
      }
    })
  },
  computed: {
    ...mapGetters([
      'globalError',
      'currentUser'
    ])
  },
  methods: {
    ...mapActions([
      'initializeStore',
      'getUserCharacters',
      'getAllCharacters',
      'getRosters',
      'getBasicCompos'
    ])
  }
}
</script>

<style scoped>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
