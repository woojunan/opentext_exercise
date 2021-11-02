<template>
  <div>
    <h1>User Directory</h1>
    <div class="search-bar-container">
      <div class="search-bar">
        <input id="search-box" type="text" v-model="searchQuery" placeholder="Search..." />
        <button @click="clearSearch" id="clear-button">Clear</button>
      </div>
    </div>

    <div class="results" v-for="user in filteredUsers" :key="user.id">
      <h2 class="user-name">{{ user.name }}</h2>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      users: [],
      searchQuery: "",
    };
  },

  methods: {
    async getUsers() {
      try {
        const response = await this.$http.get(
          "https://jsonplaceholder.typicode.com/users"
        );
        this.users = response.data;
      } catch (error) {
        console.log(error);
      }
    },

    clearSearch() {
      this.searchQuery = ''
    }
  },

  created() {
    this.getUsers();
  },

  computed: {
    filteredUsers() {
      return this.users.filter((user) => {
        return user.name.toLowerCase().match(this.searchQuery.toLowerCase());
      });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

::placeholder {
  color: #a6b0ba;
  opacity: 1;
}

.search-bar {
  margin-left: auto;
  margin-right: auto;
}

#search-box {
  height: 2em;
}

#clear-button {
  height: 2.45em;
}

.user-name {
  text-align: center;
}
</style>
