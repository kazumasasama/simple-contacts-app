<template>
  <div class="home">
    <div v-for="error in errors" :key="error">
      <p class="error-message">{{ error }}</p>
    </div>
    <div>
      <h3>Create Contact</h3>
      <p>
        First name
        <input type="text" v-model="contact.first_name" />
      </p>
      <p>
        Last name
        <input type="text" v-model="contact.last_name" />
      </p>
      <p>
        Email
        <input type="text" v-model="contact.email" />
      </p>
      <p>
        Phone
        <input type="text" v-model="contact.phone_number" />
      </p>
      <p>
        Image URL
        <input type="text" v-model="contact.image" />
      </p>
      <button @click="createContact(contact)">add contact</button>
    </div>
    <div v-for="contact in contacts" :key="contact.id">
      <h3>{{ contact.id }}: {{ contact.first_name }} {{ contact.last_name }}</h3>
      <button @click="showContact(contact)">More info</button>
      <p>{{ contact.email }}</p>
      <p>{{ contact.phone_number }}</p>
      <img class="avator" :src="contact.image" :alt="contact.first_name" />
    </div>

    <dialog id="contact-dialog">
      <form method="dialog">
        <h4>Contact info</h4>
        <p>
          <input type="number" v-model="contact.id" />
        </p>
        <p>
          <input type="text" v-model="contact.first_name" />
        </p>
        <p>
          <input type="text" v-model="contact.last_name" />
        </p>
        <p>
          <input type="text" v-model="contact.email" />
        </p>
        <p>
          <input type="text" v-model="contact.phone_number" />
        </p>
        <p>
          <input type="text" v-model="contact.image" />
        </p>

        <button @click="updateContact(contact)">update</button>
        <button class="delete-btn" @click="destroyContact(contact)">delete</button>
        <button>close</button>
      </form>
    </dialog>
  </div>
</template>


<script>
import axios from "axios"
export default {
  name: 'HomeView',
  data() {
    return {
      contacts: [],
      contact: {},
      errors: [],
    }
  },
  created() {
    this.indexContacts()
  },
  methods: {
    showContact(contact) {
      document.querySelector("#contact-dialog").showModal();
      this.contact = contact;
    },
    indexContacts() {
      axios.get("contacts.json")
        .then((res) => {
          this.contacts = res.data;
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        })
    },
    createContact(contact) {
      axios.post("contacts.json", contact)
        .then((res) => {
          this.contacts.push(res.data);
          this.contact = {};
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        })
    },
    updateContact(contact) {
      axios.patch(`contacts/${contact.id}.json`, contact)
        .then(() => {
          this.contact = {};
          location.reload();
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        })
    },
    destroyContact(contact) {
      axios.delete(`contacts/${contact.id}.json`)
        .then(() => {
          var i = this.contacts.indexOf(contact);
          this.contacts.splice(i, 1);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        })
    }
  }
}
</script>

<style>
.avator {
  max-width: 250px;
}
.delete-btn {
  color: red;
}
.error-message {
  color: red;
}
</style>
