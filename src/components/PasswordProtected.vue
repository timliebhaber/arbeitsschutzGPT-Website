<template>
    <div>
      <!-- Passwortabfrage -->
      <div v-if="!isAuthorized">
        <h2>Geben Sie das Passwort ein, um fortzufahren</h2>
        <div class="input-group">
          <input type="password" v-model="password" placeholder="Passwort eingeben" />
        </div>
        <button @click="checkPassword">Bestätigen</button>
        <p v-if="errorMessage" class="error-message">{{ errorMessage }}</p>
      </div>
  
      <!-- Geschützter Inhalt -->
      <div v-else>
        <h1>Arbeitsschutz GPT</h1>
        <p>Ihr persönlicher Assistent in allen Fragen zum Recht rund um den Arbeits- und Brandschutz.</p>
        <slot></slot> <!-- Ermöglicht das Einfügen von Inhalt in diese Komponente -->
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
        correctPassword: "miami123#", // Hier das Passwort definieren
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
  /* CSS Styles */
  
  h1 {
  text-align: center;
}

  .input-group {
    margin: 10px 0;
  }
  button {
    padding: 10px 15px;
    font-size: 16px;
    cursor: pointer;
  }
  .error-message {
    color: red;
  }
  </style>
  