<template>
  <div class="container">
    <div class="users-container">
      <div class="user" v-if="user1">
        <img :src="user1.picture.medium" :alt="user1.name.first">
        <h3>{{ user1.name.first }} {{ user1.name.last }}</h3>
        <div class="message-input">
          <input 
            v-model="message1" 
            placeholder="Escribe un mensaje..."
            @keyup.enter="sendMessage(1)"
          >
          <input 
            type="color" 
            v-model="color1"
          >
          <button @click="sendMessage(1)">Enviar</button>
        </div>
      </div>

      <ChatWindow 
        :messages="messages" 
        :user1="user1" 
        :user2="user2"
      />

      <div class="user" v-if="user2">
        <img :src="user2.picture.medium" :alt="user2.name.first">
        <h3>{{ user2.name.first }} {{ user2.name.last }}</h3>
        <div class="message-input">
          <input 
            v-model="message2" 
            placeholder="Escribe un mensaje..."
            @keyup.enter="sendMessage(2)"
          >
          <input 
            type="color" 
            v-model="color2"
          >
          <button @click="sendMessage(2)">Enviar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import ChatWindow from './components/ChatWindow.vue'

const user1 = ref(null)
const user2 = ref(null)
const message1 = ref('')
const message2 = ref('')
const color1 = ref('#e6f3ff')
const color2 = ref('#fff3e6')
const messages = ref([])

const fetchUsers = async () => {
  try {
    const response = await axios.get('https://randomuser.me/api/?results=2')
    const users = response.data.results
    user1.value = users[0]
    user2.value = users[1]
  } catch (error) {
    console.error('Error al obtener usuarios:', error)
  }
}

const sendMessage = (userNumber) => {
  const message = userNumber === 1 ? message1.value : message2.value
  const user = userNumber === 1 ? user1.value : user2.value
  const color = userNumber === 1 ? color1.value : color2.value

  if (message.trim()) {
    messages.value.push({
      text: message,
      user: `${user.name.first} ${user.name.last}`,
      color: color,
      userNumber
    })
    
    if (userNumber === 1) {
      message1.value = ''
    } else {
      message2.value = ''
    }
  }
}

onMounted(() => {
  fetchUsers()
})
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.users-container {
  display: grid;
  grid-template-columns: 250px 1fr 250px;
  gap: 20px;
  align-items: start;
}

.user {
  padding: 15px;
  border-radius: 8px;
  background-color: #f5f5f5;
  text-align: center;
}

.user img {
  border-radius: 50%;
  margin-bottom: 10px;
}

.message-input {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-top: 15px;
}

input[type="text"] {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

button {
  padding: 8px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}
</style>