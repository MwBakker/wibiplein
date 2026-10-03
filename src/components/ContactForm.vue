<script setup>
import { reactive, ref } from 'vue'

const form = reactive({
  name: '',
  email: '',
  subject: 'stalling',
  vehicleType: 'oldtimer',
  message: ''
})

const isSubmitted = ref(false)
const isLoading = ref(false)
const errorMessage = ref('')

const handleSubmit = async () => {
  isLoading.value = true
  errorMessage.value = ''

  try {
    const response = await fetch('contact.php', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(form)
    })

    const result = await response.json()

    if (result.success) {
      isSubmitted.value = true
    } else {
      errorMessage.value = result.message || 'Er is iets misgegaan. Probeer het later opnieuw.'
    }
  } catch (error) {
    errorMessage.value = 'Kan geen verbinding maken met de server.'
  } finally {
    isLoading.value = false
  }
}
</script>

<template>
  <div class="contact-container">
    <div class="contact-card">
      <h2>Neem contact op</h2>
      <p class="subtitle">Heb je een vraag over stalling of statiegeld? Vul het formulier in en we reageren snel!</p>

      <div v-if="isSubmitted" class="success-message">
        Bedankt voor je bericht! We nemen snel contact met je op.
      </div>

      <div v-if="errorMessage" class="error-message">
        {{ errorMessage }}
      </div>

      <form v-if="!isSubmitted" @submit.prevent="handleSubmit" class="contact-form">
        <div class="form-group">
          <label for="name">Naam</label>
          <input type="text" id="name" v-model="form.name" required placeholder="Je volledige naam">
        </div>

        <div class="form-group">
          <label for="email">E-mailadres</label>
          <input type="email" id="email" v-model="form.email" required placeholder="naam@voorbeeld.nl">
        </div>

        <div class="form-group">
          <label for="subject">Onderwerp</label>
          <select id="subject" v-model="form.subject">
            <option value="stalling">Stalling</option>
            <option value="statiegeld">Statiegeld</option>
            <option value="anders">Anders</option>
          </select>
        </div>

        <div class="form-group" v-if="form.subject === 'stalling'">
          <label for="vehicleType">Type voertuig</label>
          <select id="vehicleType" v-model="form.vehicleType">
            <option value="oldtimer">Oldtimer</option>
            <option value="camper">Camper</option>
            <option value="caravan">Caravan</option>
          </select>
        </div>

        <div class="form-group">
          <label for="message">Bericht</label>
          <textarea id="message" v-model="form.message" rows="5" required placeholder="Typ hier je bericht..."></textarea>
        </div>

        <button type="submit" class="submit-btn" :disabled="isLoading">
          {{ isLoading ? 'Bezig met verzenden...' : 'Verstuur bericht' }}
        </button>
      </form>
    </div>
  </div>
</template>

<style scoped lang="scss">
.contact-container {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 40px 20px;
}

.contact-card {
  background: #ffffff;
  padding: 40px;
  border-radius: 16px;
  border: 2px solid darkblue;
  box-shadow: 0 10px 30px rgba(27, 59, 91, 0.08);
  width: 65%;

  h2 {
    color: #1B3b5b;
    font-size: 28px;
    margin-bottom: 8px;
    font-weight: 700;
  }
}

.subtitle {
  color: #666;
  font-size: 16px;
  margin-bottom: 30px;
}

.contact-form {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.form-group {
  display: flex;
  flex-direction: column;
  gap: 8px;

  label {
    font-size: 14px;
    font-weight: 600;
    color: #1B3b5b;
  }

  input, select, textarea {
    padding: 12px 16px;
    border: 1px solid #d1d9e0;
    border-radius: 8px;
    font-size: 16px;
    color: #333;
    outline: none;
    transition: border-color 0.2s, box-shadow 0.2s;
    font-family: inherit;

    &:focus {
      border-color: #1B3b5b;
      box-shadow: 0 0 0 3px rgba(27, 59, 91, 0.1);
    }
  }

  textarea {
    resize: vertical;
  }
}

.submit-btn {
  background-color: #1B3b5b;
  color: #ffffff;
  padding: 14px;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  transition: background-color 0.2s, transform 0.1s;
  margin-top: 10px;

  &:hover:not(:disabled) {
    background-color: #264d75;
  }

  &:disabled {
    background-color: #a0aec0;
    cursor: not-allowed;
  }

  &:active:not(:disabled) {
    transform: scale(0.98);
  }
}

.success-message {
  background-color: #e6f4ea;
  color: #137333;
  padding: 20px;
  border-radius: 8px;
  font-weight: 600;
  text-align: center;
  font-size: 16px;
}

.error-message {
  background-color: #fce8e6;
  color: #c5221f;
  padding: 15px;
  border-radius: 8px;
  margin-bottom: 20px;
  font-size: 14px;
}
</style>