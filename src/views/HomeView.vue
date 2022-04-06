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
    showContact: function (currentContact) {
      console.log(currentContact);
      this.contactInfo = currentContact;
      console.log('showing product');
      document.querySelector("#contact-details").showModal();
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
      <dialog id="contact-details">
        <form method="dialog">
          <h1>Contact info</h1>
          <p>First Name: {{ contactInfo.first_name }}</p>
          <p>Last Name:: {{ contactInfo.last_name }}</p>
          <p>Email: {{ contactInfo.email }}</p>
          <p>Phone number: {{ contactInfo.phone_number }}</p>
          <button>Close</button>
        </form>
      </dialog>
    </div>
  </div>
</template>

<style></style>