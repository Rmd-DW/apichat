<template>
  <div class="contenedor_chat">
    <div class="user1">
      <div class="card_user" v-if="user.length" :key="user[0].name.first">
        <img :src="user[0].picture.large" alt="">
        <h2>{{ user[0].name.first }} {{ user[0].name.last }}</h2>
        <h3>{{ user[0].email }}</h3>
        <textarea v-model="contenidoChatUser1"></textarea>
        <button @click="sendMessageUser1">Enviar</button>
      </div>
    </div>
    <div class="chat_users">
      <div v-for="(chat, index) in chatPrincipal" :key="index">
        <br>
        <span class="contenidouser1" v-if="chat.user === 1">{{ chat.message }}</span>
        <br>
        <span class="contenidouser2" v-if="chat.user === 2">{{ chat.message }}</span>
        <br>
      </div>
    </div>
    <div class="user2">
      <div class="card_user" v-if="user.length" :key="user[1].name.first">
        <img :src="user[1].picture.large" alt="">
        <h2>{{ user[1].name.first }} {{ user[1].name.last }}</h2>
        <h3>{{ user[1].email }}</h3>
        <textarea v-model="contenidoChatUser2"></textarea>
        <button @click="sendMessageUser2">Enviar</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "App",
  data() {
    return {
      user: [], 
      chatPrincipal: [], // Stores chat messages
      contenidoChatUser1: "",
      contenidoChatUser2: "",
      showchats: false, 
    };
  },
  methods: {
    async getUsers() {
      try {
        const url = 'https://randomuser.me/api/?results=2';
        const response = await axios.get(url);
        return response.data.results;
      } catch (error) {
        console.log(error);
        return [];
      }
    },
    async setUser() {
      this.user = await this.getUsers();
    },
    sendMessageUser1() {
      this.showchats = true;
      this.chatPrincipal.push({ user: 1, message: this.contenidoChatUser1 });
      this.contenidoChatUser1 = ""; 
      console.log(this.chatPrincipal);
    },
    sendMessageUser2() {
      this.showchats = true;
      this.chatPrincipal.push({ user: 2, message: this.contenidoChatUser2 });
      this.contenidoChatUser2 = ""; // Clear the input after sending the message
      console.log(this.chatPrincipal);
    }
  },
  mounted() {
    this.setUser();
  }
};
</script>

<style>
.contenedor_chat {
  display: flex;
  margin: 1rem;
  padding: 1rem;
  background-color: rgb(163, 163, 163);
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

.user1, .user2 {
  margin: 1rem;
  padding: 1rem;
  flex: 1;
}

.user1 {
  background-color: rgb(126, 211, 126);
}

.user2 {
  background-color: rgb(177, 126, 211);
}

.chat_users {
  margin: 1rem;
  padding: 1rem;
  border-radius: 5%;
  background-color: rgb(255, 255, 255);
  flex: 2;
}

.card_user {
  display: flex;
  flex-direction: column;
  padding: 1rem;
  margin: 1rem;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.contenidouser1 {
  background-color: rgb(126, 211, 126);
  border-radius: 5%;
  padding: 0.5rem;
  margin: 1rem;
}

.contenidouser2 {
  background-color: rgb(177, 126, 211);
  border-radius: 5%;
  padding: 0.5rem;
  margin: 1rem;
}
</style>
