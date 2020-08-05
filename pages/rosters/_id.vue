<template>
  <div>
    <div v-if="roster">
      <h3>Roster {{roster.name}}</h3>

      <div class="flex flex-align-center flex-items-center col-md-11 mx-auto">
        <div v-for="grpNum in (Object.values(compo).length / 5)" :key="grpNum">
          <SlotSelector
            v-for="(currentSlot, slotIndex) in Object.values(compo).slice((grpNum - 1) * 5, ((grpNum - 1) * 5) + 5)" :key="slotIndex"
            :theSlot="currentSlot"
            :rosterId="roster.id"
          />
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import SlotSelector from '../../components/SlotSelector.vue'
import { mapActions, mapGetters } from 'vuex'

export default {
  name: 'roster',
  components: { SlotSelector },
  data () {
    return {
      // roster: '',
      selectedCharacter: [],
      error: ''
    }
  },
  created () {
    if (!localStorage.signedIn) {
      this.$router.replace('/')
    }
  },
  computed: {
    ...mapGetters(['rosters', 'allCharacters']),
    compo () {
      if (this.roster === '') {
        return []
      }
      return Object.values(this.roster.template)
    },
    roster () {
      return this.rosters.find(roster => roster.id.toString() === this.$route.params.id.toString())
    }
  },
  methods: {
    setError (error, text) {
      this.error = (error.response && error.response.data && error.response.data.error) || text
    }
  }
}
</script>
