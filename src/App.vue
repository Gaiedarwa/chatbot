<template>
  <div id="app">
    <h1>Chatbot Vue.js</h1>
    <ChatBot />
  </div>
</template>

<script>
import ChatBot from './components/ChatBot.vue';

export default {
  name: 'App',
  components: {
    ChatBot
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
