<template>
  <div>
    <h3>Add a new Character</h3>

    <form @submit.prevent="addCharacter" class="col-md-8 mx-auto">
      <div class="form-group">
        <input type="text" autofocus autocomplete class="form-control" placeholder="Your character's name" v-model="newCharacter.name">
      </div>

      <div class="form-group text-left">
        <label for="race">Race</label>
        <select name="race" v-model="newCharacter.race" class="form-control" required>
          <option value="orc">Orc</option>
          <option value="tauren">Tauren</option>
          <option value="troll">Troll</option>
          <option value="undead">Undead</option>
        </select>
      </div>

      <div class="form-group text-left">
        <label for="class">Class</label>
        <select name="class" v-model="newCharacter.ch_class" class="form-control" required>
          <option value="druid">Druid</option>
          <option value="hunter">Hunter</option>
          <option value="mage">Mage</option>
          <option value="priest">Priest</option>
          <option value="rogue">Rogue</option>
          <option value="shaman">Shaman</option>
          <option value="warrior">Warrior</option>
          <option value="warlock">Warlock</option>
        </select>
      </div>

      <div class="form-group text-left">
        <label for="class">Roles</label>
        <div class="flex flex-items-center flex-items-center">
          <div class="pr-4">
            <input type="checkbox" id="tank" value="tank" v-model="selectedRoles">
            <label for="tank" class="pointer">Tank</label>
          </div>
          <div class="pr-4">
            <input type="checkbox" id="heal" value="heal" v-model="selectedRoles">
            <label for="heal" class="pointer">Heal</label>
          </div>
          <div class="pr-4">
            <input type="checkbox" id="dps" value="dps" v-model="selectedRoles">
            <label for="dps" class="pointer">DPS</label>
          </div>
        </div>
      </div>

      <input type="submit" value="Validate" class="btn btn-success">
    </form>

    <hr>

    <div class="col-md-10 mx-auto">
      <table class="table table-hover">
        <thead>
          <th>Name</th>
          <th>Race</th>
          <th>Class</th>
          <th>Roles</th>
          <th></th>
        </thead>
        <tbody>
          <tr v-for="char in userCharacters" :key="char.id" :char="char">
            <td>{{char.name}}</td>
            <td>{{char.race.toUpperCase()}}</td>
            <td><b :class="colorForClass(char.ch_class)">{{char.ch_class.toUpperCase()}}</b></td>
            <td class="flex flex-items-center flex-align-between" style="max-width: 100px;">
              <img v-if="char.roles.includes('tank')" src="@/assets/tank.png" class="logo-role">
              <img v-if="char.roles.includes('heal')" src="@/assets/heal.png" class="logo-role">
              <img v-if="char.roles.includes('dps')" src="@/assets/dps.png" class="logo-role">
            </td>
            <td><button @click.prevent="removeCharacter(char)" class="btn btn-danger">X</button></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import { mapActions, mapState } from 'vuex'

export default {
  name: 'Characters',
  data () {
    return {
      newCharacter: [],
      selectedRoles: []
    }
  },
  created () {
    if (this.currentUser === {}) {
      this.$router.replace('/')
    } else {
      this.getUserCharacters()
    }
  },
  computed: {
    ...mapState(['currentUser', 'userCharacters'])
  },
  methods: {
    ...mapActions({
      getUserCharacters: 'getUserCharacters',
      createCharacter: 'createCharacter',
      deleteUserCharacter: 'deleteUserCharacter'
    }),
    colorForClass (chClass) {
      return `text-${chClass}`
    },
    setError (error, text) {
      this.error = (error.response && error.response.data && error.response.data.error) || text
    },
    addCharacter () {
      const value = this.newCharacter
      if (!value) {
        return
      }
      this.createCharacter({ char: this.newCharacter, roles: this.selectedRoles })
        .then(() => {
          this.newCharacter = []
          this.selectedRoles = []
        })
    },
    removeCharacter (character) {
      this.deleteUserCharacter(character)
    }
  }
}
</script>
