<template>
    <div>
      <div 
        v-for="(message, index) in messages" 
        :key="index" 
        :style="{ backgroundColor: message.color }" 
        class="message p-2 mb-2 rounded"
      >
        <strong>{{ message.user }}:</strong> {{ message.text }}
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'MessageList',
    props: {
      messages: {
        type: Array,
        required: true
      }
    }
  }
  </script>
  
  <style scoped>
  .message {
    border: 1px solid #ccc;
  }
  </style>