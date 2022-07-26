<template>
  <div class="w-100 px-5 d-flex">
    <button class="text-left btn bg-danger" @click="clearLocalStorage()">Clear Storage</button>
    <div class="vue-chat">
      <div v-if="chatIsFull">
        <h1>Sorry, chat is full</h1>
      </div>
      <div v-else>
        <chat-welcome @auth-user="authUser" />
        <chat-header>
          {{ interlocutor }}
        </chat-header>
        <chat-message-box :messages="messages" :userName="userName" />
        <chat-send-form @send-message="sendMessage" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, onBeforeMount, onMounted, ref } from "vue";
import chatWelcome from "./components/chatWelcome.vue";
import chatHeader from "./components/chatHeader.vue";
import chatMessageBox from "./components/chatMessageBox.vue";
import chatSendForm from "./components/chatSendForm.vue";
import Messages from "./services/messageProvider";

export default defineComponent({
  setup() {
    const Chat = new Messages();
    let messages = ref(Chat.getMessages());
    let userName = ref("");
    let interlocutor = ref("");
    let chatIsFull = ref(true);

    let sendMessage = (message: string) => {
      Chat.sendMessage({
        userName: userName.value,
        content: message,
        time: formatTime(new Date()),
      });

      updateMessage();
    };

    let updateMessage = () => {
      messages.value = Chat.getMessages();
    };

    let authUser = (name: string) => {
      Chat.setUserName(name);
      userName.value = name;
      getInterlocutorName();
    };
    
    let getInterlocutorName = () => {
      let users: Array<string>;
      users = Chat.getUsersNames();
      interlocutor.value = users.filter((user) => user !== userName.value)[0];
      console.log(interlocutor.value);
    };

    let formatTime = (time: Date) => {
      return `${time.getHours()}:${time.getMinutes()}:${time.getSeconds()}`;
    };

    let checkIfChatIsFull = () => {
      chatIsFull.value = Chat.getUsersNames().length === 2;
    };
    
    function clearLocalStorage(){
        localStorage.removeItem('vue-chat')
        localStorage.removeItem('vue-users')
    };

    onBeforeMount(() => {
      checkIfChatIsFull();

      window.addEventListener("storage", () => {
        updateMessage();
        getInterlocutorName();
      });

      window.addEventListener("beforeunload", function (event) {
        Chat.logOut(userName.value);
      });
    });

    return {
      messages,
      sendMessage,
      authUser,
      userName,
      interlocutor,
      chatIsFull,
      clearLocalStorage
    };
  },
  components: {
    chatWelcome,
    chatHeader,
    chatMessageBox,
    chatSendForm,
  },
});
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.vue-chat {
  position: fixed;
  bottom: 0;
  right: 10px;
  display: flex;
  flex-flow: column wrap;
  width: 400px;
  min-height: 300px;
  border-radius: 4px 4px 0 0;
  overflow: hidden;
  box-shadow: -1px 0 8px rgba(0, 0, 0, 0.34);
}
</style>
