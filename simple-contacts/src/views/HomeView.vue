<script>
import axios from 'axios';

export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      contacts: [],
      newContact: {},
      currentContact: {},

    };
  },
  created: function () { },
  methods: {
    contactIndex: function () {
      console.log("Retrieving contacts!")
      axios.get("http://localhost:3000/contacts.json").then(response => {
        console.log(response.data)
        this.contacts = response.data
      })
    },
    contactCreate: function () {
      console.log('Creating contact!')
      console.log(this.newContact)
      axios.post("http://localhost:3000/contacts.json", this.newContact)
        .then(response => {
          console.log(response.data);
          this.contacts.push(this.newContact)
        })
    },
    contactShow: function (theContact) {
      console.log(`Showing more info...`)
      console.log(theContact)
      this.currentContact = theContact

      document.querySelector('#contact-details').showModal();
    },
    contactUpdate: function (theContact) {
      console.log(`Updating current Contact`)
      console.log(this.currentContact)
      axios.patch(`http://localhost:3000/contacts/${this.currentContact.id}`, this.currentContact)
        .then(response => {
          console.log(response.data)
        })
    },
    contactDestroy: function (theContact) {
      console.log(`Destroying the Friendship`)
      console.log(this.currentContact)
      let index = this.contacts.indexOf(this.currentContact)
      console.log(index)
      axios.delete(`http://localhost:3000/contacts/${this.currentContact.id}`)
        .then(response => {
          console.log(response.data)
          console.log(`Deleted friend`)
          this.contacts.splice(index, 1);
        })
    }
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
  </div>
  <div><button @click="contactIndex">Get Contacts</button>
    <p v-for="contact in contacts">{{ contact.first_name }} {{ contact.last_name }}
      <button @click="contactShow(contact)">Show More!</button>
    </p>

  </div>
  <div>
    <P>First Name:<input type="text" v-model="newContact.first_name"></P>
    <P>Last Name:<input type="text" v-model="newContact.last_name"></P>
    <P>Email:<input type="text" v-model="newContact.email"></P>
    <P>Phone Number:<input type="text" v-model="newContact.phone_number"></P>
  </div>
  <div><button @click="contactCreate">Create New Contact!</button></div>
  <dialog id="contact-details">
    <form method="dialog">
      <P><b>First Name: </b><input type="text" v-model="currentContact.first_name"></P>
      <p><b>Last Name: </b><input type="text" v-model="currentContact.last_name"></p>
      <p><b>Email: </b><input type="text" v-model="currentContact.email"></p>
      <p><b>Phone Number: </b><input type="text" v-model="currentContact.phone_number"></p>
      <button @click="contactUpdate">Update Contact</button>
      <button @click="contactDestroy">Delete Contact</button>
      <button>Close</button>
    </form>
  </dialog>
</template>

<style>
</style>