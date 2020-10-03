<template>
  <div class="user-profile">
      <div class="user-profile__user-panel">
        <h1 class="user-profile__username">    @{{ user.username }} </h1>
        <div class="user-profile__admin-badge" v-if="user.isAdmin"> admin </div>
        <div class="user-profile__follower-count">
          <strong>Followers: </strong> {{ followers }}
        </div>
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
    }
  },
  mounted() {
    this.followUser();
  }
}
</script>

<style>
* {
  margin: 0;
}

.user-profile {
display: grid;
grid-template-columns: 1fr 3fr;
width: 70%;
padding: 30px 5%;
}

.user-profile__user-panel {
    display: flex;
    flex-direction: column;
    margin-right: 50px;
    padding: 10px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #dfe3e8;
    align-self: flex-start;
}

.user-profile__admin-badge {
    background: rebeccapurple;
    color: white;
    border-radius: 5px;
    margin: 3px 0;
    margin-right: auto;
    padding: 0 10px;
}

h1 {
    margin: 0;
}
</style>
