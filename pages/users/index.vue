<template>
  <section>
    <h1>{{ pageTitle }}</h1>
    <ul>
      <li v-for="user of users" :key="user.id">
        <a href="#" @click.prevent="goToUser(user)">
          {{ user.name }} - {{ user.email }}
        </a>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  middleware: ["auth"],
  async fetch({ store, error }) {
    try {
      if (store.getters["users/users"].length === 0) {
        await store.dispatch("users/fetchUsers");
      }
    } catch (e) {
      error(e);
    }
  },
  data: () => ({
    pageTitle: "Users page"
  }),
  computed: {
    users() {
      return this.$store.getters["users/users"];
    }
  },
  methods: {
    goToUser(user) {
      this.$router.push(`/users/${user.id}`);
    }
  }
};
</script>
