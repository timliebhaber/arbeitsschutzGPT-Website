<template>
  <div class="pw-root">
    <header v-if="!isAuthorized" class="pw-app-header">
      Arbeitsschutz GPT
    </header>
    <!-- Passwortabfrage -->
    <div
      v-if="!isAuthorized"
      class="pw-card"
      role="dialog"
      aria-modal="true"
      aria-label="Passwortabfrage"
    >
      <h2 class="pw-title">Zugang erforderlich</h2>
      <p class="pw-desc">Bitte geben Sie das Passwort ein, um fortzufahren.</p>
      <div class="pw-input-group">
        <label for="pw-input" class="sr-only">Passwort</label>
        <input
          id="pw-input"
          type="password"
          v-model="password"
          placeholder="Passwort eingeben"
          autocomplete="current-password"
          @keyup.enter="checkPassword"
        />
      </div>
      <button @click="checkPassword" class="pw-btn">Bestätigen</button>
      <p v-if="errorMessage" class="pw-error" role="alert">
        {{ errorMessage }}
      </p>
    </div>

    <!-- Geschützter Inhalt -->
    <div v-else class="pw-content">
      <h1 class="pw-header">Arbeitsschutz GPT</h1>
      <p class="pw-sub">
        Ihr persönlicher Assistent in allen Fragen zum Recht rund um den
        Arbeits- und Brandschutz.
      </p>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      password: "",
      isAuthorized: false,
      errorMessage: "",
      correctPassword: "miami123#",
    };
  },
  methods: {
    checkPassword() {
      if (this.password === this.correctPassword) {
        this.isAuthorized = true;
        this.errorMessage = "";
      } else {
        this.errorMessage = "Falsches Passwort. Bitte erneut versuchen.";
      }
    },
  },
};
</script>

<style scoped>
:global(html),
:global(body) {
  overflow-x: hidden;
}
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

.pw-root {
  min-height: 100vh;
  min-width: 100vw;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  background: none;
  height: 100vh;
  width: 100vw;
  overflow-y: auto;
  overflow-x: hidden;
}

.pw-app-header {
  width: 100vw;
  max-width: 100vw;
  background: none;
  color: #f3f6fa;
  font-size: 2.2rem;
  font-weight: 800;
  text-align: center;
  letter-spacing: 0.01em;
  margin: 32px 0 24px 0;
  padding: 0 0 0 0;
  line-height: 1.1;
  z-index: 2;
  user-select: none;
}

.pw-card {
  background: linear-gradient(135deg, #23272f 0%, #181c24 100%);
  color: #f3f6fa;
  border-radius: 18px;
  box-shadow: 0 4px 32px rgba(0, 0, 0, 0.25);
  padding: 40px 32px 32px 32px;
  max-width: 380px;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 18px;
  max-height: 96vh;
  overflow-y: auto;
}

.pw-title {
  font-size: 1.5rem;
  font-weight: 600;
  margin: 0 0 4px 0;
  text-align: center;
  letter-spacing: 0.01em;
}
.pw-desc {
  font-size: 1.05rem;
  color: #b0b8c1;
  margin: 0 0 10px 0;
  text-align: center;
}
.pw-input-group {
  width: 100%;
  margin: 0 0 8px 0;
}
.pw-input-group input {
  width: 100%;
  padding: 14px 12px;
  border-radius: 10px;
  border: 2px solid #23272f;
  background: #181c24;
  color: #f3f6fa;
  font-size: 1.08rem;
  outline: none;
  box-shadow: 0 2px 8px rgba(44, 46, 84, 0.1);
  transition: border 0.2s, box-shadow 0.2s;
}
.pw-input-group input:focus {
  border: 2px solid #2e8b57;
  box-shadow: 0 0 0 2px #3cb37155;
}
.pw-btn {
  width: 100%;
  padding: 14px 0;
  border-radius: 10px;
  border: none;
  background: linear-gradient(90deg, #2e8b57 0%, #3cb371 100%);
  color: #fff;
  font-size: 1.08rem;
  font-weight: 600;
  cursor: pointer;
  margin-top: 4px;
  transition: background 0.2s, box-shadow 0.2s;
  box-shadow: 0 2px 8px rgba(46, 139, 87, 0.1);
}
.pw-btn:focus {
  outline: 2px solid #2e8b57;
  box-shadow: 0 0 0 2px #3cb37155;
}
.pw-btn:hover {
  background: linear-gradient(90deg, #3cb371 0%, #2e8b57 100%);
}
.pw-error {
  color: #ff6b6b;
  font-size: 1rem;
  margin: 0;
  text-align: center;
}

.pw-content {
  width: 100vw;
  max-width: 1100px;
  margin: 0 auto;
  padding: 32px 0 0 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  background: none;
}
.pw-header {
  font-size: 2.1rem;
  font-weight: 700;
  margin: 0 0 8px 0;
  color: #f3f6fa;
  text-align: center;
  letter-spacing: 0.01em;
}
.pw-sub {
  font-size: 1.15rem;
  color: #b0b8c1;
  margin: 0 0 18px 0;
  text-align: center;
}
@media (max-width: 600px) {
  .pw-app-header {
    font-size: 1.3rem;
    margin: 18px 0 12px 0;
  }
  .pw-card {
    padding: 24px 8px 20px 8px;
    max-width: 98vw;
    max-height: 98vh;
  }
  .pw-content {
    padding: 18px 0 0 0;
  }
  .pw-header {
    font-size: 1.3rem;
  }
  .pw-sub {
    font-size: 1rem;
  }
}
</style>
