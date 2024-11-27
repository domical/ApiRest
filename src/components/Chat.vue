<template>
    <div class="chat-container container mt-5">
      <h1 class="text-center">Chat entre Desconocidos</h1>
      <div class="row">
        <div class="col-md-5">
          <div class="user border p-3 rounded text-center">
            <img :src="users[0]?.picture.large" alt="User 1 Photo" class="user-photo rounded-circle mb-2" />
            <h5>{{ users[0]?.name.first }}</h5>
          </div>
        </div>
        <div class="col-md-2">
          <div class="central-chat border p-3 mt-4 text-center">
            <h5>Conversación</h5>
            <div class="message-list" style="height: 300px; overflow-y: auto;">
              <MessageList :messages="centralMessages" />
            </div>
            <div class="input-group mb-3">
              <input v-model="newMessage" type="text" class="form-control" placeholder="Escribe un mensaje..." />
              <input type="color" v-model="messageColor" class="form-control-color" />
              <button class="btn btn-primary" @click="sendMessage">Enviar</button>
            </div>
          </div>
        </div>
        <div class="col-md-5">
          <div class="user border p-3 rounded text-center">
            <img :src="users[1]?.picture.large" alt="User 2 Photo" class="user-photo rounded-circle mb-2" />
            <h5>{{ users[1]?.name.first }}</h5>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import MessageList from './MessageList.vue';
  
  export default {
    name: 'Chat',
    components: {
      MessageList
    },
    data() {
      return {
        users: [],
        centralMessages: [],
        newMessage: '',
        messageColor: '#000000'
      };
    },
    created() {
      this.loadUsers();
    },
    methods: {
      async loadUsers() {
        try {
          const response = await axios.get('https://randomuser.me/api/?results=2');
          this.users = response.data.results;
        } catch (error) {
          console.error('Error al cargar los usuarios:', error);
        }
      },
      sendMessage() {
        if (this.newMessage.trim()) {
          const sender = Math.random() < 0.5 ? this.users[0].name.first : this.users[1].name.first; // Alternar entre usuarios
          this.centralMessages.push({
            text: this.newMessage,
            color: this.messageColor,
            user: sender,
          });
          this.newMessage = '';
        }
      }
    }
  }
  </script>
  
  <style scoped>
  .chat-container {
    max-width: 800px; /* Limitar el ancho para mantener el diseño centrado */
    margin: 0 auto; /* Centrar el contenedor */
  }
  .user-photo {
    width: 80px;
    height: 80px;
  }
  .central-chat {
    margin-top: 20px;
    display: flex;
    flex-direction: column;
    align-items: center; /* Centrar contenido dentro del chat central */
  }
  </style>