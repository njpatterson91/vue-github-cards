<template>
  <form class="search" v-on:submit.prevent="apiCall">
    <input
      type="text"
      name="search"
      placeholder="Search for a user"
      v-model="searchTerm"
      v-on:keydown.enter.prevent="addCategory"
    />
    <button>Search</button>
  </form>
  <div class="hello"></div>
  <div v-for="user in users" :key="user.id">
    <div class="followerCard">
      <h2>{{ user.login }}</h2>
      <div class="followerImages">
        <img :src="user.avatar_url" alt="" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  mounted() {
    this.apiCall();
  },
  data() {
    return {
      users: [],
      searchTerm: "",
    };
  },
  methods: {
    updateUsers(data) {
      return (this.users = data);
    },
    apiCall() {
      axios
        .get(`https://api.github.com/users/${this.searchTerm}/followers`)
        .then((res) => {
          console.log(res.data);
          this.updateUsers(res.data);
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.followerImages img {
  max-width: 100px;
}
.followerCard {
  display: flex;
  flex-flow: row wrap;
}
.search {
  text-align: center;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
