<template>
  <q-page class="flex flex-center column">
    <!-- <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    /> -->

    <q-btn label="click me" @click="foo" class="q-ma-md" />

    <q-input v-model="user" label="user" outlined clearable class="q-ma-md" />

    <q-file filled bottom-slots v-model="someFile" label="Label" counter class="q-ma-md">
      <template v-slot:prepend>
        <q-icon name="cloud_upload" @click.stop.prevent />
      </template>
      <template v-slot:append>
        <q-icon name="close" @click.stop.prevent="someFile = null" class="cursor-pointer" />
      </template>
    </q-file>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const someFile = ref(null)
const user = ref('')

async function foo() {
  if (!someFile.value) {
    console.log('No file selected')
    return
  }
  try {
    let res = await axios.post(
      'http://localhost:3000/api/upload',
      { file: someFile.value, user: user.value },
      { headers: { 'Content-Type': 'multipart/form-data' } },
    )
    console.log(res.status)
    console.log(res.statusText)
    console.log(res.data)
  } catch (error) {
    console.error(error)
  }
}
</script>
