<template>
  <div id="app">
    <div class="np-chat-container">
      <div class="np-chat-container--holder">
        <div
          v-for="(chat, chatIndex) in chatHistory"
          :key="chatIndex"
          style="position: relative"
        >
          <div
            v-bind:class="[
              chatIndex % 2 == 0
                ? 'np-chat--text np-chat--text_sent'
                : 'np-chat--text np-chat--text_received',
            ]"
          >
            <span class="np-chat--time"
              >{{ getChatTimeLocal(chat.time) }}:</span
            >
            {{ chat.message }}
          </div>
        </div>
      </div>
      <div class="np-input-text--value_holder">
        <input
          class="np-input-text--value"
          type="text"
          placeholder="Type and enter to send"
          v-model="originalUserInputValueObservable"
          @keyup.enter="insertIntoList()"
        />
        <div class="np-input-text--button" @click="insertIntoList()">Send</div>
      </div>
    </div>
  </div>
</template>

<script>
import moment from "moment";
export default {
  name: "App",
  data() {
    return {
      chatHistory: [
        {
          message: "Hi, this is Shruti from nightprogrammer.com!",
          time: new Date() ? new Date() : null,
        },
      ],
      originalUserInputValueObservable: null,
    };
  },
  beforeDestroy() {
    this.resetToInitialStates();
  },
  beforeCreate() {
    this.resetToInitialStates();
  },
  methods: {
    insertIntoList() {
      if (this.originalUserInputValueObservable) {
        this.chatHistory.push({
          message: this.originalUserInputValueObservable,
          time: new Date(),
        });
        this.originalUserInputValueObservable = null;
      }
    },
    resetToInitialStates() {
      this.chatHistory = [];
      this.originalUserInputValueObservable = null;
    },
    getChatTimeLocal(originalChatTime) {
      return moment.utc(originalChatTime).local().format("hh:mm:a");
    },
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
.np-chat-container {
  width: 300px;
  height: 500px;
  position: relative;
  border: 1px solid #000000;
  padding: 12px;
  background: #080b10;
}
.np-chat-container--holder {
  height: 500px;
  overflow-y: auto;
}
.np-chat--text {
  width: 200px;
  font-size: 12px;
  letter-spacing: 0.4px;
  background: #eee;
  word-break: keep-all;
  margin: 4px 6px;
  padding: 6px 10px;
  border-radius: 6px;
  color: #fff;
  line-height: 1.4;
}
.np-chat--time {
  opacity: 0.7;
}
.np-chat--text_received {
  background: #005d4b;
  float: right;
}
.np-chat--text_sent {
  background: #1f2c34;
  float: left;
}
.np-input-text--value_holder {
  position: absolute;
  background: #080b10;
  width: 300px;
  height: 50px;
  bottom: 0px;
}
.np-input-text--value {
  padding: 6px 4px;
  width: 220px;
  font-size: 14px;
  background: #1f2c34;
  border: 1px solid #1f2c34;
  border-radius: 30px;
  color: rgb(255, 255, 255);
  outline: none;
  transform: translateY(6px);
}
.np-input-text--button {
  color: #fff;
  position: absolute;
  right: 12px;
  top: 12px;
  cursor: pointer;
}
</style>