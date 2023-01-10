<template>
  <div class="UserList">
    <v-chip
      class="ma-2"
      close
      @click="updateUser(dataUser)"
      @click:close="remove(dataUser)"
      >{{ dataUser.name }}</v-chip
    >
  </div>
</template>

<script>
export default {
  name: "UserList",
  props: ["dataUser"],
  data() {
    return {};
  },
  methods: {
    remove() {
      this.$emit("remove", this.dataUser);
    },

    updateUser(item) {
      this.$emit("updateUser", this.dataUser);
    },
  },
};
</script>
