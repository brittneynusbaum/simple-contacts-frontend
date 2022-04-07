<script>
import axios from 'axios';

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!!",
      contacts: [],
      contactInfo: {},
      newContactParams: {},
      editContactParams: {},
      errors: []
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
    createContact: function () {
      console.log('creating product');
      var newContactParams = {
        first_name: this.newContactParams.first_name,
        last_name: this.newContactParams.last_name,
        email: this.newContactParams.email,
        phone_number: this.newContactParams.phone_number,
        image: this.newContactParams.image
      }
      axios.post(`/contacts`, newContactParams).then(response => {
        console.log(response.data);
        this.contacts.push(response.data)
        this.newContactParams = {}
        this.errors = []
      }).catch(error => {
        console.log('error loading')
        this.errors = error.response.data.errors
      })
    },
    showContact: function (currentContact) {
      console.log(currentContact);
      this.contactInfo = currentContact;
      this.editContactParams = currentContact;
      console.log('showing product');
      document.querySelector("#contact-details").showModal();
    },
    updateContact: function () {
      console.log('updating contact');
      axios.patch(`/contacts/${this.editContactParams.id}`, this.editContactParams).then(response => {
        console.log(response.data);
      });
    },
    destroyContact: function (contact) {
      console.log(contact.id);
      console.log('destroy contact');
      axios.delete(`/contacts/${this.contact.id}`).then(response => {
        var index = this.contacts.indexOf(contact);
        this.contacts.splice(index, 1);
        console.log(response.data);
      });
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p v-for="error in errors" v-bind:key="error">Errors: {{ errors }}</p>
    <h5>Add new contact</h5>
    <p>
      First name:
      <input type="text" v-model="newContactParams.first_name" />
    </p>
    <p>
      Last name:
      <input type="text" v-model="newContactParams.last_name" />
    </p>
    <p>
      Email:
      <input type="text" v-model="newContactParams.email" />
    </p>
    <p>
      Phone number:
      <input type="text" v-model="newContactParams.phone_number" />
    </p>
    <p>
      Image:
      <input type="text" v-model="newContactParams.image" />
    </p>

    <button v-on:click="createContact()">Create contact</button>
    <h5>Contacts</h5>
    <div v-for="contact in contacts" v-bind:key="contact.id">
      {{ contact.first_name }} {{ contact.last_name }}
      <img v-bind:src="contact.image" />
      <button v-on:click="showContact(contact)">Show contact info</button>
    </div>
    <dialog id="contact-details">
      <form method="dialog">
        <h5>Contact info</h5>
        <p>First Name: {{ contactInfo.first_name }}</p>
        <p>Last Name: {{ contactInfo.last_name }}</p>
        <p>Email: {{ contactInfo.email }}</p>
        <p>Phone number: {{ contactInfo.phone_number }}</p>
        <p>Image: {{ contactInfo.image }}</p>

        <h5>Update contact</h5>
        <p>
          First name:
          <input type="text" v-model="editContactParams.first_name" />
        </p>
        <p>
          Last name:
          <input type="text" v-model="editContactParams.last_name" />
        </p>
        <p>
          Email:
          <input type="text" v-model="editContactParams.email" />
        </p>
        <p>
          Phone number:
          <input type="text" v-model="editContactParams.phone_number" />
        </p>
        <p>
          Image:
          <input type="text" v-model="editContactParams.image" />
        </p>
        <button v-on:click="updateContact()">Update Contact</button>
        <button v-on:click="destroyContact(currentContact)">Delete Contact</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style>
img {
  width: 150px;
}
</style>