<template>
  <div class="chat-container">
    <div class="chatbox" ref="chatbox">
      <div v-for="(message, index) in messages" :key="index" :class="message.sender">
        <strong>{{ message.sender }}:</strong> {{ message.text }}
      </div>
    </div>
    <input
      v-model="userMessage"
      @keyup.enter="sendMessage"
      placeholder="Tapez votre message ici..."
    />
    <button @click="sendMessage">Envoyer</button>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      userMessage: '',
      messages: []
    };
  },
  methods: {
    addMessage(sender, text) {
      this.messages.push({ sender, text });
      this.$nextTick(() => {
        this.$refs.chatbox.scrollTop = this.$refs.chatbox.scrollHeight;
      });
    },
    async sendMessage() {
      if (this.userMessage.trim() === '') return;

      // Ajouter le message de l'utilisateur
      const message = this.userMessage;
      this.addMessage('Utilisateur', message);
      this.userMessage = '';

      try {
        const response = await axios.post(
          'https://api.openai.com/v1/chat/completions',
          {
            model: 'gpt-3.5-turbo', // Remplacez par 'gpt-4' si vous y avez accès
            messages: [{ role: 'user', content: message }],
            max_tokens: 100
          },
          {
            headers: {
              'Authorization': `Bearer VOTRE_CLE_API_OPENAI`,
              'Content-Type': 'application/json'
            }
          }
        );

        const botReply = response.data.choices[0].message.content;
        this.addMessage('Chatbot', botReply);
      } catch (error) {
        console.error('Erreur :', error);
        this.addMessage('Chatbot', 'Désolé, une erreur est survenue.');
      }
    }
  }
};
</script>

<style>
.chat-container {
  width: 400px;
  margin: 0 auto;
  padding-top: 50px;
}

.chatbox {
  border: 1px solid #ccc;
  padding: 10px;
  height: 400px;
  overflow-y: auto;
  margin-bottom: 10px;
}

input {
  width: 70%;
  padding: 10px;
}

button {
  width: 25%;
  padding: 10px;
}

.Utilisateur {
  text-align: left;
  color: blue;
}

.Chatbot {
  text-align: left;
  color: green;
}
</style>
//commentaire 