<template>
  <v-app>
    <ChatBot />
  </v-app>
</template>

<script>
import ChatBot from "./components/ChatBot.vue";

export default {
  name: "App",
  components: {
    ChatBot,
  },
};
</script>
