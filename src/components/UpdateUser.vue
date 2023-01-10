<template>
  <div class="UpdateUser">
    <v-text-field name="rename" v-model="renameUserName.name" outlined>
    </v-text-field>
    <v-btn color="primary" elevation="2" @click="saveNewName()">
      Сохранить</v-btn
    >
  </div>
</template>

<script>
export default {
  name: "UpdateUser",
  props: ["renameUserName"],
  data() {
    return {};
  },
  methods: {
    saveNewName() {
      this.$emit("saveNewName", this.renameUserName);
    },
  },
};
</script>
