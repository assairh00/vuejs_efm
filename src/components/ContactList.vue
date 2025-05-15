<template>
  <div class="table-container">
    <h2>Liste des Contacts</h2>
    <table>
      <thead>
        <tr>
          <th>Nom</th>
          <th>Email</th>
          <th>Téléphone</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="contact in contacts" :key="contact.id">
          <td v-if="!contact.editing">{{ contact.name }}</td>
          <td v-else><input v-model="contact.name" /></td>

          <td v-if="!contact.editing">{{ contact.email }}</td>
          <td v-else><input v-model="contact.email" /></td>

          <td v-if="!contact.editing">{{ contact.phone }}</td>
          <td v-else><input v-model="contact.phone" /></td>

          <td>
            <button v-if="!contact.editing" @click="editContact(contact)">Modifier</button>
            <button v-else @click="saveContact(contact)">Enregistrer</button>
            <button class="delete" @click="deleteContact(contact.id)">Supprimer</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  props: ['contacts'],
  methods: {
    editContact(contact) {
      this.contacts.forEach(c => c.editing = false);
      contact.editing = true;
    },
    async saveContact(contact) {
      await axios.put(`http://localhost:3000/contacts/${contact.id}`, {
        name: contact.name,
        email: contact.email,
        phone: contact.phone
      });
      contact.editing = false;
      this.$emit('contactUpdated');
    },
    async deleteContact(id) {
      if (confirm('Êtes-vous sûr de vouloir supprimer ce contact ?')) {
        await axios.delete(`http://localhost:3000/contacts/${id}`);
        this.$emit('contactDeleted');
      }
    }
  }
};
</script>

<style scoped>
.table-container {
  margin: 40px auto;
  max-width: 800px;
  background-color: #fff;
  padding: 25px;
  border-radius: 12px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

h2 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

table {
  width: 100%;
  border-collapse: collapse;
  font-size: 14px;
}

thead {
  background-color: #42b983;
  color: white;
}

th, td {
  padding: 12px;
  border: 1px solid #ddd;
  text-align: left;
  vertical-align: middle;
}

tr:nth-child(even) {
  background-color: #f9f9f9;
}

input {
  width: 100%;
  padding: 6px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 6px 10px;
  margin-right: 5px;
  border: none;
  border-radius: 4px;
  background-color: #42b983;
  color: white;
  cursor: pointer;
  font-size: 13px;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #36976b;
}

button.delete {
  background-color: #e74c3c;
}

button.delete:hover {
  background-color: #c0392b;
}
</style>
