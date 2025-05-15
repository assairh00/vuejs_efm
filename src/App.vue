<template>
  <div class="container">
    <h1>Gestionnaire de Contacts</h1>
    <ContactForm @contactAdded="fetchContacts" />
    <ContactList
      :contacts="contacts"
      @contactDeleted="fetchContacts"
      @contactUpdated="fetchContacts"
    />
  </div>
</template>

<script>
import ContactForm from './components/ContactForm.vue';
import ContactList from './components/ContactList.vue';
import axios from 'axios';

export default {
  components: { ContactForm, ContactList },
  data() {
    return {
      contacts: []
    };
  },
  methods: {
    async fetchContacts() {
      const res = await axios.get('http://localhost:3000/contacts');
      this.contacts = res.data;
    }
  },
  mounted() {
    this.fetchContacts();
  }
};
</script>

<style scoped>
.container {
  max-width: 900px;
  margin: 40px auto;
  padding: 30px;
  background-color: #ffffff;
  border-radius: 12px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
}

h1 {
  text-align: center;
  font-size: 28px;
  color: #2c3e50;
  margin-bottom: 30px;
  font-weight: 600;
}
</style>
