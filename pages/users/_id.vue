<template>
  <div>
    <h1>{{ user.name }}</h1>
    <hr />
    <b>{{ user.email }}</b>
  </div>
</template>

<script>
export default {
  middleware: ["auth"],
  validate({ params }) {
    return /^\d+$/.test(params.id);
  },
  async asyncData({ params, error, store }) {
    try {
      const user = await store.dispatch("users/fetchUserById", params.id);
      console.log(user);
      return { user: user.data };
    } catch (e) {
      error(e);
    }
  }
};
</script>
