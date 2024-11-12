<template>
  <div class="home-container">
    <!-- Left Panel with Welcome Message and Robot Logo -->
    <div class="overlay">
      <h1>Welcome to Chatbot ISI</h1>
    </div>

    <!-- Chatbot Interface on the Right -->
    <div class="right-panel">
      <div class="avatar" @click="toggleChat">
        💬
      </div>

      <div v-if="showChatbot" class="chat-container">
        <div class="chatbox" ref="chatbox">
          <div
            v-for="(message, index) in messages"
            :key="index"
            :class="['message', message.sender.toLowerCase()]"
          >
            <div class="sender">{{ message.sender }}</div>
            <div class="text">{{ message.text }}</div>
          </div>
        </div>
        <div class="input-container">
          <input
            v-model="userMessage"
            @keyup.enter="sendMessage"
            placeholder="Type your message here..."
          />
          <button @click="sendMessage">Send</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showChatbot: false,
      userMessage: '',
      messages: [
        { sender: 'Chatbot', text: 'Hello! How can I assist you today?' }
      ],
      faqData: [] // Will hold the FAQ data
    };
  },
  created() {
    // Example of loading static FAQ data directly:
    this.faqData = [
      { question: 'What is ISI?', answer: "ISI is the Institut Supérieur d'Informatique, a renowned educational institution specializing in computer science." },
      { question: 'How to apply to ISI?', answer: 'You can apply to ISI through their official website, where you’ll find all admission requirements and procedures.' },
      { question: 'What programs does ISI offer?', answer: 'ISI offers a range of programs in software engineering, data science, cybersecurity, and telecommunications.' }
    ];

    // If you want to fetch from an external JSON file or API:
    // fetch('/path/to/faqData.json')
    //   .then(response => response.json())
    //   .then(data => this.faqData = data);
  },
  methods: {
    toggleChat() {
      this.showChatbot = !this.showChatbot;
    },
    sendMessage() {
      if (this.userMessage.trim() !== '') {
        // Add user message to chat
        this.messages.push({
          sender: 'User',
          text: this.userMessage,
        });

        // Find matching FAQ response
        const response = this.faqData.find(faq =>
          faq.question.toLowerCase() === this.userMessage.toLowerCase()
        );

        // Respond with the matching answer or a default message
        setTimeout(() => {
          if (response) {
            this.messages.push({
              sender: 'Chatbot',
              text: response.answer
            });
          } else {
            this.messages.push({
              sender: 'Chatbot',
              text: 'Sorry, I do not have an answer to that question.'
            });
          }
        }, 1000);

        // Clear user input
        this.userMessage = '';
      }
    }
  }
};
</script>

<style scoped>
/* Main container styling */
.home-container {
  display: flex;
  height: 100vh;
  font-family: Arial, sans-serif;
  background-image: url('@/assets/test2.png'); /* Replace with actual image path */
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  position: relative;
}

/* Contrast overlay */
.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4); /* Dark overlay for contrast */
  display: flex;
  justify-content: center;
  align-items: center;
  color: #ffffff;
  font-size: 2rem;
  z-index: 1;
  text-align: center;
}

.right-panel, .avatar, .chat-container, .chatbox, .input-container, input, button {
  position: relative;
  z-index: 2;
}

/* Chat avatar */
.avatar {
  position: fixed;
  bottom: 20px;
  right: 20px;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  background-color: #3f51b5;
  color: #ffffff;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.2);
  font-size: 24px;
}

.chat-container {
  position: fixed;
  bottom: 100px;
  right: 20px;
  width: 450px;
  height: 500px;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 10px;
  box-shadow: 0px 4px 12px rgba(0, 0, 0, 0.1);
  font-family: Arial, sans-serif;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
}

/* Messages */
.message {
  display: flex;
  flex-direction: column;
  margin-bottom: 10px;
  max-width: 80%;
}

.user {
  align-self: flex-end;
  background-color: #e0f7fa;
  border-radius: 10px;
  padding: 10px;
  color: #00796b;
  margin-right: 5px;
}

.chatbot {
  align-self: flex-start;
  background-color: #e8eaf6;
  border-radius: 10px;
  padding: 10px;
  color: #3f51b5;
  margin-left: 5px;
}

.sender {
  font-weight: bold;
  font-size: 0.85em;
  color: #333;
  margin-bottom: 5px;
}

/* Input container */
.input-container {
  display: flex;
  width: 100%;
  margin-top: 10px;
}

input {
  flex: 1;
  padding: 10px;
  border-radius: 10px;
  border: none;
  font-size: 14px;
  margin-right: 5px;
}

button {
  padding: 0 15px;
  background-color: #3f51b5;
  color: #ffffff;
  border-radius: 10px;
  font-weight: bold;
}

button:hover {
  background-color: #303f9f;
}
</style>
