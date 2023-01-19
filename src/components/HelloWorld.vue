<template>
  <v-container>
    <v-row style="width: 80%">
      <v-col>
        <v-btn color="primary" elevation="2" @click="postReqest()"
          >Создать данные для работы</v-btn
        ></v-col
      >
      <v-col>
        <v-btn color="primary" elevation="2" @click="getReqest()">
          Получить пользователей из API</v-btn
        ></v-col
      >
    </v-row>

    <v-row>
      <div v-for="user in arrayUsersAPI" :key="user.id">
        <user-list :dataUser="user" @remove="remove" @updateUser="updateUser">
        </user-list>
      </div>
    </v-row>

    <v-row>
      <v-col cols="8" sm="6" md="3" v-if="fieldVisibleRenameUser">
        <update-user :editingUser="editingUser" @saveNewName="saveNewName">
        </update-user
      ></v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";
import UserList from "./UserList.vue";
import UpdateUser from "./UpdateUser.vue";

export default {
  name: "HelloWorld",
  components: {
    UserList,
    UpdateUser,
  },

  data: () => ({
    url: "https://crudcrud.com/api/6188a7a1573b440f8c129a435c79f94e/unicorns/",
    fieldVisibleRenameUser: false,
    editingUser: "",
    arrayUsersAPI: [],
    FIRST_NAME: ["Николай", "Иван", "Федор", "Александр", "Михаил"],
    LAST_NAME: ["Гоголь", "Тургенев", "Блок", "Достоевкский", "Лермонтов"],
    arrayGeneratedUsers: [],
  }),
  methods: {
    generateArrayUsers() {
      this.arrayGeneratedUsers = [];
      while (this.arrayGeneratedUsers.length < 10) {
        this.arrayGeneratedUsers.push(
          this.FIRST_NAME[
            Math.floor(0 + Math.random() * (this.FIRST_NAME.length - 0))
          ] +
            " " +
            this.LAST_NAME[
              Math.floor(0 + Math.random() * (this.LAST_NAME.length - 0))
            ]
        );
      }
      console.log(this.arrayGeneratedUsers);
    },

    async postReqest() {
      this.generateArrayUsers();

      for (let i = 0; i < this.arrayGeneratedUsers.length; i++) {
        try {
          await axios({
            method: "POST",
            headers: {
              "Content-Type": "application/json",
            },
            url: "https://cors-anywhere.herokuapp.com/" + this.url,
            data: { name: this.arrayGeneratedUsers[i] },
          });
        } catch (error) {
          console.log(error.code);
        }
      }

      // this.arrayGeneratedUsers.forEach((element) => {
      //   axios({
      //     method: "POST",
      //     headers: {
      //       "Content-Type": "application/json",
      //     },
      //     url: "https://cors-anywhere.herokuapp.com/" + this.url,
      //     data: { name: element },
      //   });
      // });
    },

    async getReqest() {
      try {
        await axios({
          method: "get",
          url: this.url,
        }).then((response) => {
          this.arrayUsersAPI = response.data;
          console.log(this.arrayUsersAPI);
        });
      } catch (error) {
        console.log(error.code);
      }
    },

    async remove(item) {
      let deleteUserId = item._id;

      try {
        await axios
          .delete(
            "https://cors-anywhere.herokuapp.com/" + this.url + deleteUserId
          )
          .then((response) => console.log(response));
        this.arrayUsersAPI.splice(this.arrayUsersAPI.indexOf(item), 1);
        this.fieldVisibleRenameUser = false;
      } catch (error) {
        console.log(error.code);
      }
      console.log(deleteUserId);
      console.log(this.arrayUsersAPI);
    },

    updateUser(item) {
      this.editingUser = item;
      this.fieldVisibleRenameUser = true;
      console.log(this.editingUser);
    },

    saveNewName() {
      try {
        console.log(this.editingUser);
        axios.put(
          "https://cors-anywhere.herokuapp.com/" +
            this.url +
            this.editingUser._id,
          {
            name: this.editingUser.name,
          }
        );

        this.fieldVisibleRenameUser = false;
      } catch (error) {
        console.log(error.code);
      }
    },
  },
};
</script>
