<template>
  <div class="user-profile">
      <div class="user-profile__user-panel">
        <h1 class="user-profile__username">    @{{ user.username }} </h1>
        <div class="user-profile__admin-badge" v-if="user.isAdmin"> 
          admin 
        </div>
        <div class="user-profile__follower-count">
          <strong>Followers: </strong> {{ followers }}
        </div>
        <form class="user-profile__create-twoot" @submit.prevent="createNewTwoot" :class="{ '--exceeded': newTwootCharacterCount > 180 }">
          <label for="newTwoot"><strong>New Twoot: </strong>({{newTwootCharacterCount}}/180)</label>
          <textarea id="newTwoot" rows="4" v-model="newTwootContent"/>

          <div class="user-profile__create-twoot-type">
            <label for="newTwootType"><strong>Type:</strong></label>
            <select id="newTwootType" v-model="selectedTwootType">
              <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
                {{ option.name }}
              </option>
            </select>
          </div>

          <button>
            Twoot!
          </button>
        </form>
      </div>
      <div class="user-profile__twoots-wrapper">
        <Twoot 
            v-for="twoot in user.twoots" 
            :key="twoot.id" 
            :username="user.username" 
            :twoot="twoot" 
            @favourite="toggleFavourite"
        />
      </div>
  </div>  
</template>
<script>
import Twoot from "./Twoot";


export default {
  name: 'UserProfile',
  components: { Twoot },
  data() {
    return {
      newTwootContent: '',
      selectedTwootType: 'instant',
      twootTypes: [
        { value: 'draft', name: 'Draft' },
        { value: 'instant', name: 'Instant Twoot'}
      ],
      followers: 0,
      user: {
        id: 1,
        username: "EdWoods",
        firstName: "Edward",
        lastName: "Woods",
        email: "ejvdhout@xs4all.nl",
        isAdmin: true,
        twoots: [
          { id: 1, content: 'Twotter is Amazing!' },
          { id: 2, content: "Don't forget to subscribe to my Twotter!"},
          { id: 3, content: "I'm a mastertwooter"},
        ],
      }
    }
  },
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower!`)
      }
    }
  },
  computed: {
    newTwootCharacterCount() {
      return this.newTwootContent.length;
    },
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`
    }
  },
  methods: {
    followUser() {
      this.followers++
    },
    toggleFavourite(id) {
      console.log(`Favourited Tweet ${id}`)
    },
    createNewTwoot() {
      if (this.newTwootContent && this.selectedTwootType !== 'draft') {
        this.user.twoots.unshift( {
          id: this.user.twoots.length + 1,
          content: this.newTwootContent,
        })
        this.newTwootContent = ""
      }
    }
  },
  mounted() {
    this.followUser();
  }
}
</script>

<style lang="scss" scoped>
* {
  margin: 0;
}

.user-profile {
display: grid;
grid-template-columns: 1fr 3fr;
width: 90%;
padding: 30px 5%;

  .user-profile__user-panel {
      display: flex;
      flex-direction: column;
      margin-right: 50px;
      padding: 10px;
      background-color: white;
      border-radius: 5px;
      border: 1px solid #dfe3e8;
      align-self: flex-start;
      min-width: 300px;

    .user-profile__admin-badge {
    background: rebeccapurple;
    color: white;
    border-radius: 5px;
    margin: 3px 0;
    margin-right: auto;
    padding: 0 10px;
    margin-bottom: 5px;
    }
    h1 {
    margin: 0;
    }
  }
  .user-profile__create-twoot {
  display: flex;
  flex-direction: column;
  padding-top: 20px;

    &.--exceeded {
      color: red;
      border-color: red;

      button {
        background-color: red;
        border-color: red;
        color: white;
      }
    }
  }

  .user-profile__create-twoot-type {
    padding-top: 5px;
    display: flex;
    flex-direction: row;
  }

  select#newTwootType {
    margin-left: 5px;
    height: 20px;
    margin-bottom: 5px;
  }
}









</style>
