<template>
    <div class="chat-window">
      <div class="messages">
        <div 
          v-for="(message, index) in messages" 
          :key="index"
          class="message"
          :class="{ 'message-right': message.userNumber === 2 }"
          :style="{ backgroundColor: message.color }"
        >
          <strong>{{ message.user }}:</strong>
          <p>{{ message.text }}</p>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  defineProps({
    messages: {
      type: Array,
      required: true
    },
    user1: {
      type: Object,
      required: true
    },
    user2: {
      type: Object,
      required: true
    }
  })
  </script>
  
  <style scoped>
  .chat-window {
    height: 600px;
    border: 1px solid #ddd;
    border-radius: 8px;
    padding: 20px;
    background-color: white;
  }
  
  .messages {
    height: 100%;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  
  .message {
    max-width: 70%;
    padding: 10px;
    border-radius: 8px;
    margin-bottom: 10px;
  }
  
  .message-right {
    margin-left: auto;
  }
  
  .message strong {
    display: block;
    margin-bottom: 5px;
  }
  
  .message p {
    margin: 0;
  }
  </style>