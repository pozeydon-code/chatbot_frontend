<template>
  <v-container>
    <v-row>
      <v-col>
        <v-text-field v-model="userName" label="Tu nombre" outlined />
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <v-text-field
          v-model="currentMessage"
          label="Escribe tu pregunta"
          outlined
          @keyup.enter="sendMessage"
        />
      </v-col>
      <v-col cols="2">
        <v-btn color="primary" @click="sendMessage"> Enviar </v-btn>
      </v-col>
    </v-row>

    <v-row>
      <v-col>
        <div v-for="(msg, index) in chatMessages" :key="index">
          <strong>{{ msg.sender }}:</strong> {{ msg.text }}
        </div>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  name: "ChatBot",
  data() {
    return {
      userName: "",
      currentMessage: "",
      chatMessages: [],
    };
  },
  methods: {
    async sendMessage() {
      if (!this.userName || !this.currentMessage) {
        alert("Por favor ingresa tu nombre y tu mensaje.");
        return;
      }

      // Agregamos el mensaje del usuario al chat
      this.chatMessages.push({
        sender: this.userName,
        text: this.currentMessage,
      });

      // Preparamos la petición al backend
      const payload = {
        userName: this.userName,
        message: this.currentMessage,
      };

      try {
        const response = await axios.post(
          "http://localhost:5283/api/Chat/",
          payload,
        );
        // Agregamos la respuesta del bot
        this.chatMessages.push({
          sender: "Bot",
          text: response.data.response,
        });
      } catch (error) {
        console.error(error);
        this.chatMessages.push({
          sender: "Bot",
          text: "Ha ocurrido un error en el servidor.",
        });
      }

      // Limpiamos el mensaje actual
      this.currentMessage = "";
    },
  },
};
</script>

<style scoped>
/* Aquí podrías añadir estilos o confiar en Vuetify */
</style>
