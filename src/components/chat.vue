<!-- src/components/Chat.vue -->
<template>
  <div class="chat-container">
    <div class="chat-box" ref="chatBox">
      <div v-for="(msg, index) in messages" :key="index" :class="['message', msg.sender]">
        <div class="text">{{ msg.text }}</div>
      </div>
    </div>
    <form @submit.prevent="sendMessage" class="chat-form">
      <input
        type="text"
        v-model="userInput"
        placeholder="Schreibe eine Nachricht..."
        autocomplete="off"
        required
      />
      <button type="submit">Senden</button>
    </form>
  </div>
  <div class="counter-container">
      <p>Es wurden in dieser Session {{ counter }} Prompts ausgeführt.</p>
    </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Chat',
  data() {
    return {
      userInput: '',
      messages: [],
      counter: 0
    };
  },
  methods: {
    async sendMessage() {
      const message = this.userInput.trim();
      if (message === '') return;

      // Füge die Benutzernachricht hinzu
      this.messages.push({ sender: 'user', text: message });
      this.userInput = '';
      
      //Counter für Variable
      this.counter++;

      // Scrollt zum Ende des Chat-Fensters
      this.$nextTick(() => {
        this.scrollToBottom();
      });

      try {
        const response = await axios.post('http://127.0.0.1:5000/api/chat', {
          message: message
        });

        if (response.data.response) {
          this.messages.push({ sender: 'bot', text: response.data.response });
        } else {
          this.messages.push({ sender: 'bot', text: 'Entschuldigung, ich konnte keine Antwort generieren.' });
        }
      } catch (error) {
        console.error('Error:', error);
        this.messages.push({ sender: 'bot', text: 'Ein Fehler ist aufgetreten.' });
      } finally {
        this.$nextTick(() => {
          this.scrollToBottom();
        });
      }
    },
    scrollToBottom() {
      const chatBox = this.$refs.chatBox;
      chatBox.scrollTop = chatBox.scrollHeight;
    }
  }
};
</script>

<style scoped>
/* Dunkles Farbschema */
body {
  background-color: #121212;
  color: #e0e0e0;
}

.chat-container {
  width: 500px;
  margin: 50px auto;
  border: 1px solid #333;
  border-radius: 10px;
  background-color: #1e1e1e;
  display: flex;
  flex-direction: column;
  height: 600px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.5);
}

.counter-container {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 50px; /* Optional: kannst du anpassen, um die Höhe festzulegen */
}

.chat-header {
  padding: 15px;
  background-color: #272727;
  border-bottom: 1px solid #333;
  text-align: center;
}

.chat-header h2 {
  margin: 0;
  color: #ffffff;
}

.chat-box {
  flex: 1;
  padding: 20px;
  overflow-y: auto;
  background-color: #1e1e1e;
}

.chat-form {
  display: flex;
  padding: 10px;
  background-color: #272727;
  border-top: 1px solid #333;
}

.chat-form input {
  flex: 1;
  padding: 10px;
  border: 1px solid #333;
  border-radius: 5px;
  background-color: #2c2c2c;
  color: #e0e0e0;
  font-size: 16px;
}

.chat-form input::placeholder {
  color: #a0a0a0;
}

.chat-form button {
  margin-left: 10px;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  background-color: #3a3a3a;
  color: #ffffff;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.chat-form button:hover {
  background-color: #505050;
}

.message {
  margin-bottom: 15px;
  display: flex;
}

.message.user .text {
  margin-left: auto;
  background-color: #007bff;
  color: #ffffff;
  padding: 10px 15px;
  border-radius: 15px;
  max-width: 80%;
}

.message.bot .text {
  margin-right: auto;
  background-color: #333333;
  color: #e0e0e0;
  padding: 10px 15px;
  border-radius: 15px;
  max-width: 80%;
}

.text {
  word-wrap: break-word;
}
</style>
