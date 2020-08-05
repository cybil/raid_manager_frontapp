<template>
  <div>
    <div v-if="error" class="alert alert-danger col-md-11 mx-auto">{{ error }}</div>
    <div v-if="compo">
      <h3>Compo {{ compo.name }}</h3>

      <div class="flex flex-align-center flex-items-center col-md-11 mx-auto">
        <div v-for="grpNum in (Object.values(temp).length / 5)" :key="grpNum">
          <CompoSlot
            v-for="(currentSlot, slotIndex) in Object.values(temp).slice((grpNum - 1) * 5, ((grpNum - 1) * 5) + 5)" :key="slotIndex"
            :theSlot="currentSlot"
            :compoId="compo.id"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CompoSlot from '../../components/CompoSlot.vue'
import { mapGetters } from 'vuex'

export default {
  name: 'BasicCompo',
  components: { CompoSlot },
  data () {
    return {
      error: ''
    }
  },
  computed: {
    ...mapGetters([
      'basicCompos'
    ]),
    temp () {
      if (this.compo === '') {
        return []
      }
      return Object.values(this.compo.template)
    },
    compo() {
      return this.basicCompos.find(compo => compo.id.toString() === this.$route.params.id.toString())
    }
  },
  created () {
    if (!localStorage.signedIn) {
      this.$router.replace('/')
    }
  },
  methods: {
    setError (error, text) {
      this.error = (error.response && error.response.data) || error.message || text
    }
  }
}
</script>
