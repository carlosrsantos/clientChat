<template>
  <div id="chat-container">
    
    <input
    type="text"
    v-model="username"
    placeholder="User"
    ref="tbUser"
    class="input-name"
    />
    
    <input
    type="text"
    v-model="message"
    @keyup.enter="send"
    placeholder="Type a message"
    ref="tbMessage"
    class="message-input"
    />
    <button @click="send" ref="btnSend" class="send-button">Send</button>

    <hr />

    <div id="divMessages" ref="divMessages" class="message-container">
      <!-- Renderiza as mensagens no chat -->
      <div v-for="(msg, index) in messages" :key="index" class="message">
        <div class="message-author">{{ msg.username }}</div>
        <div class="message-text">{{ msg.message }}</div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted } from "vue";
import * as signalR from "@microsoft/signalr";

export default defineComponent({
  name: "ChatComponent",
  setup() {
    const divMessages = ref<HTMLDivElement | null>(null);
    const tbUser = ref<HTMLInputElement | null>(null);
    const tbMessage = ref<HTMLInputElement | null>(null);
    const message = ref<string>(""); // Armazena a mensagem atual do input
    const messages = ref<{ username: string; message: string }[]>([]); // Lista de mensagens do chat
    const username = ref(""); // Identificador de usuário único

    // Inicializa a conexão com o SignalR Hub
    const connection = new signalR.HubConnectionBuilder()
      .withUrl("http://localhost:5034/chat")
      .build();

    // Lida com mensagens recebidas
    connection.on("ReceiveMessage", (user: string, msg: string) => {
      messages.value.push({ username: user, message: msg });
      scrollToBottom();
    });

    // Função para rolar para o final da div de mensagens
    const scrollToBottom = () => {
      if (divMessages.value) {
        divMessages.value.scrollTop = divMessages.value.scrollHeight;
      }
    };

    // Envia a mensagem
    const send = () => {
      let date = new Date();
      if (message.value.trim()) {
        connection.invoke("SendMessage", username.value + ' - ' + date.toLocaleString(), message.value).then(() => {
          message.value = ""; // Limpa o campo de input após o envio
        });
      }
    };

    // Inicia a conexão quando o componente é montado
    onMounted(() => {
      connection
        .start()
        .catch((err) => console.error("Erro ao conectar ao Hub:", err));
    });

    return {
      divMessages,
      tbUser,
      tbMessage,
      message,
      messages,
      username,
      send,
    };
  },
});
</script>

<style scoped>
#chat-container {
  
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 50%;
  margin: 10px;
}

.message-container {
  width: 50%;
  max-height: 300px;
  overflow-y: auto;
  border: 1px solid #ccc;
  padding: 10px;
  margin-bottom: 10px;
}

.message {
  margin-bottom: 8px;
}

.message-author {
  font-weight: bold;
  color: #333;
}

.message-text {
  margin-left: 5px;
  color: #555;
}

.input-name {
  margin: 10px 0 10px 0;
  width: 50%;
  padding: 8px;
  margin-right: 5px;
}
.message-input {
  width: 50%;
  padding: 8px;
  margin-right: 5px;
}

.send-button {
  padding: 8px;
  margin: 10px 0 10px 0;
}
</style>
