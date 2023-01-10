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
      <div v-for="user in arrayGetUser" :key="user.id">
        <user-list :dataUser="user" @remove="remove" @updateUser="updateUser">
        </user-list>
      </div>
    </v-row>

    <v-row>
      <v-col cols="8" sm="6" md="3" v-if="renameUser">
        <update-user
          :renameUserName="renameUserName"
          @saveNewName="saveNewName"
        >
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
    url: "https://crudcrud.com/api/586d38df7bf247cebd0d233be2fe6ba2/unicorns/",
    renameUser: false,
    renameUserName: "",
    arrayGetUser: [],
    arrayUser: [
      "Николай Гоголь",
      "Иван Тургенев",
      "Федор Блок",
      "Александр Достоевкский",
      "Иван Лермонтов",
      "Федор Лермонтов",
      "Михаил Лермонтов",
      "Иван Тургенев",
      "Николай Лермонтов",
    ],
  }),
  methods: {
    postReqest() {
      this.arrayUser.forEach((element) => {
        axios({
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          url: "https://cors-anywhere.herokuapp.com/" + this.url,
          data: { name: element },
        });
      });
    },

    async getReqest() {
      await axios({
        method: "get",
        url: this.url,
      }).then((response) => {
        this.arrayGetUser = response.data;
        console.log(this.arrayUser);
      });
    },

    async remove(item) {
      let deleteUserId = item._id;

      await axios.delete(
        "https://cors-anywhere.herokuapp.com/" + this.url + deleteUserId
      );
      this.arrayGetUser.splice(this.arrayGetUser.indexOf(item), 1);
      this.renameUser = false;
      console.log(deleteUserId);
    },

    updateUser(item) {
      this.renameUserName = item;
      this.renameUser = true;
      console.log(this.renameUserName);
    },

    saveNewName() {
      console.log(this.renameUserName);
      axios.put(
        "https://cors-anywhere.herokuapp.com/" +
          this.url +
          this.renameUserName._id,
        {
          name: this.renameUserName.name,
        }
      );

      this.renameUser = false;
    },
  },
};
</script>
