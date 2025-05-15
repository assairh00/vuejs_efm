<template>
  <div class="form-container">
    <h2>Ajouter un Contact</h2>
    <form @submit.prevent="addContact">
      <input type="text" v-model="name" placeholder="Nom" required />
      <input type="email" v-model="email" placeholder="Email" required />
      <input type="text" v-model="phone" placeholder="Téléphone" required />
      <button type="submit">Ajouter</button>
    </form>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      name: '',
      email: '',
      phone: '',
    }
  },
  methods: {
    async addContact() {
      if (!this.name || !this.email || !this.phone) {
        alert('Tous les champs sont obligatoires.')
        return
      }

      await axios.post('http://localhost:3000/contacts', {
        name: this.name,
        email: this.email,
        phone: this.phone,
      })

      this.name = ''
      this.email = ''
      this.phone = ''

      this.$emit('contactAdded')
    },
  },
}
</script>

<style scoped>
.form-container {
  max-width: 400px;
  margin: 40px auto;
  padding: 20px 25px;
  background-color: #f9f9f9;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.form-container h2 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

input {
  display: block;
  width: 100%;
  margin-bottom: 15px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 14px;
}

input:focus {
  border-color: #42b983;
  outline: none;
}

button {
  width: 100%;
  background-color: #42b983;
  color: white;
  padding: 10px;
  border: none;
  border-radius: 6px;
  font-size: 16px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #36976b;
}
</style>
