<template>
  <div class="home">
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
    }
  },
  created() {

  },
  methods: {
  }
}
</script>

<style></style>


    // t.string "first_name"
    // t.string "last_name"
    // t.string "email"
    // t.string "phone_number"
    // t.string "image"