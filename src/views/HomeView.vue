<script>
import axios from 'axios';

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!!",
      contacts: [],
      contactInfo: {}
    };
  },
  created: function () {
    this.indexContacts();
  },
  methods: {
    indexContacts: function () {
      console.log('loading all products');
      axios.get('/contacts').then(response => {
        console.log(response.data);
        this.contacts = response.data
      });
    },
    showContact: function (thisContact) {
      console.log('showing product');
      axios.get(`/contacts/1`).then(response => {
        console.log(response.data);
        this.contactInfo = thisContact;

      });
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      {{ contact.first_name }} {{ contact.last_name }}
      <button
        v-on:click="showContact(contact)"
      >Show contact info</button>
    </div>
  </div>
</template>

<style></style>