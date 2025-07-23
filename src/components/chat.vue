<!-- src/components/Chat.vue -->
<template>
  <div class="modernchat-root" role="main" aria-label="Chatbereich">
    <div
      class="modernchat-chat-box"
      ref="chatBox"
      tabindex="0"
      aria-live="polite"
    >
      <div
        v-for="(msg, index) in messages"
        :key="index"
        :class="['modernchat-message', msg.sender]"
      >
        <div
          class="modernchat-text"
          :aria-label="
            msg.sender === 'user' ? 'Ihre Nachricht' : 'Antwort des Chatbots'
          "
        >
          {{ msg.text }}
        </div>
      </div>
    </div>
    <form
      @submit.prevent="sendMessage"
      class="modernchat-form"
      aria-label="Nachricht senden"
    >
      <label for="modernchat-input" class="sr-only">Nachricht eingeben</label>
      <input
        id="modernchat-input"
        type="text"
        v-model="userInput"
        placeholder="Schreibe eine Nachricht..."
        autocomplete="off"
        required
        :aria-label="'Nachricht eingeben'"
        @keydown.enter.exact.prevent="sendMessage"
      />
      <button
        type="submit"
        :disabled="userInput.trim() === ''"
        aria-label="Senden"
        title="Senden"
      >
        <svg
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
          aria-hidden="true"
          focusable="false"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path d="M2 21L23 12L2 3V10L17 12L2 14V21Z" fill="currentColor" />
        </svg>
      </button>
    </form>
    <div class="modernchat-counter-container" aria-live="polite">
      <p>Ausgeführte Prompts in dieser Session: {{ counter }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "ModernChat",
  data() {
    return {
      userInput: "",
      messages: [],
      counter: 0,
    };
  },
  methods: {
    async sendMessage() {
      const message = this.userInput.trim();
      if (message === "") return;
      this.messages.push({ sender: "user", text: message });
      this.userInput = "";
      this.counter++;
      this.$nextTick(() => {
        this.scrollToBottom();
      });
      try {
        const response = await axios.post("http://127.0.0.1:5000/api/chat", {
          message: message,
        });
        if (response.data.response) {
          this.messages.push({ sender: "bot", text: response.data.response });
        } else {
          this.messages.push({
            sender: "bot",
            text: "Entschuldigung, ich konnte keine Antwort generieren.",
          });
        }
      } catch (error) {
        console.error("Error:", error);
        this.messages.push({
          sender: "bot",
          text: "Ein Fehler ist aufgetreten.",
        });
      } finally {
        this.$nextTick(() => {
          this.scrollToBottom();
        });
      }
    },
    scrollToBottom() {
      const chatBox = this.$refs.chatBox;
      chatBox.scrollTop = chatBox.scrollHeight;
    },
  },
};
</script>

<style scoped>
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  border: 0;
}

.modernchat-root {
  min-height: 100vh;
  min-width: 100vw;
  background: linear-gradient(135deg, #181c24 0%, #23272f 100%);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-end;
  padding: 0;
  box-sizing: border-box;
  font-family: "Segoe UI", "Arial", sans-serif;
  color: #f3f6fa;
  height: 100vh;
  width: 100vw;
  overflow: hidden;
}

.modernchat-chat-box {
  width: 100vw;
  max-width: 1100px;
  flex: 1 1 auto;
  padding: 24px 0 90px 0; /* extra bottom padding für Abstand zur Eingabe */
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 18px;
  box-sizing: border-box;
  outline: none;
  height: calc(100vh - 110px - 48px);
  scrollbar-width: none; /* Firefox */
  -ms-overflow-style: none; /* IE 10+ */
}
.modernchat-chat-box::-webkit-scrollbar {
  display: none;
}

.modernchat-message {
  display: flex;
  width: 100%;
  margin-bottom: 30px;
}

.modernchat-message.user .modernchat-text {
  margin-left: auto;
  background: linear-gradient(90deg, #2e8b57 0%, #3cb371 100%);
  color: #fff;
  padding: 16px 22px;
  border-radius: 18px 18px 4px 18px;
  max-width: 70%;
  font-size: 1.08rem;
  box-shadow: 0 2px 8px rgba(46, 139, 87, 0.1);
  word-break: break-word;
  outline: none;
}

.modernchat-message.bot .modernchat-text {
  margin-right: auto;
  background: #23272f;
  color: #f3f6fa;
  padding: 16px 22px;
  border-radius: 18px 18px 18px 4px;
  max-width: 70%;
  font-size: 1.08rem;
  box-shadow: 0 2px 8px rgba(44, 46, 84, 0.1);
  border: 1px solid #23272f;
  word-break: break-word;
  outline: none;
}

.modernchat-text:focus {
  outline: 2px solid #2e8b57;
}

.modernchat-form {
  position: absolute;
  bottom: 48px;

  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  max-width: 700px;
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #181c24;
  padding: 16px 12px 16px 12px;
  box-sizing: border-box;
  z-index: 10;
  border-top: 1px solid #23272f;
  box-shadow: 0 -2px 8px rgba(44, 46, 84, 0.1);
  height: 80px;
  border-top-right-radius: 10px;
  border-top-left-radius: 10px;
}

.modernchat-form input {
  flex: 1;
  padding: 14px;
  border: 2px solid #23272f;
  border-radius: 12px;
  background: #23272f;
  color: #f3f6fa;
  font-size: 1.08rem;
  margin-right: 10px;
  outline: none;
  box-shadow: 0 2px 8px rgba(38, 50, 56, 0.1);
  transition: border 0.2s, box-shadow 0.2s;
}

.modernchat-form input:focus {
  border: 2px solid #2e8b57;
  box-shadow: 0 0 0 2px #3cb37155;
}

.modernchat-form input::placeholder {
  color: #b0b8c1;
  opacity: 1;
}

.modernchat-form button {
  background: #2e8b57;
  color: #fff;
  border: none;
  border-radius: 12px;
  padding: 0 18px;
  height: 44px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.2s, box-shadow 0.2s;
  font-size: 1.15rem;
  box-shadow: 0 2px 8px rgba(46, 139, 87, 0.1);
  outline: none;
}

.modernchat-form button:focus {
  outline: 2px solid #2e8b57;
  box-shadow: 0 0 0 2px #3cb37155;
}

.modernchat-form button:disabled {
  background: #444a55;
  cursor: not-allowed;
}

.modernchat-form button:hover:not(:disabled) {
  background: #3cb371;
}

.modernchat-counter-container {
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  max-width: 700px;
  margin: 0;
  text-align: center;
  color: #b0b8c1;
  font-size: 0.98rem;
  padding: 0 20px 8px 0;
  background: #181c24;
  height: 48px;
  display: flex;
  align-items: center;
  box-sizing: border-box;
}

.modernchat-counter-container p {
  text-align: center;
  margin: 0 auto;
  width: 100%;
}

@media (max-width: 1200px) {
  .modernchat-chat-box,
  .modernchat-form,
  .modernchat-counter-container {
    max-width: 100vw;
  }
}
@media (max-width: 800px) {
  .modernchat-chat-box,
  .modernchat-form,
  .modernchat-counter-container {
    max-width: 100vw;
    padding-left: 0;
    padding-right: 0;
  }
}

@media (max-width: 600px) {
  .modernchat-chat-box {
    padding: 10px 0 0 0;
    height: calc(100vh - 110px - 48px);
  }
  .modernchat-form {
    height: 56px;
    padding: 10px 4px 10px 4px;
  }
  .modernchat-form input {
    margin-right: 0;
    width: 100%;
  }
  .modernchat-form button {
    width: 48px;
    padding: 0;
    height: 40px;
  }
  .modernchat-counter-container {
    height: 40px;
    font-size: 0.92rem;
    padding: 0 8px 4px 0;
  }
}
</style>
