<template>
  <div :class="['app-chat', !checkedContact && '_empty']">
    <section class="head">
      <img :src="checkedContact.avatar" :alt="checkedContact.name" class="avatar">
      <div class="contact-data">
        <div class="name">
          {{ checkedContact.name }}
        </div>
      </div>
    </section>

    <section class="body">
      <div v-if="!getIsListVisible" class="not-checked">
        {{ getEmptyMessage }}
      </div>

      <ul v-else class="messages">
        <li
          v-for="(message, i) in checkedContact.messages"
          :key="i"
          class="message"
        >
          <pre>{{ message }}</pre>
        </li>
      </ul>
    </section>

    <section class="footer">
      <form class="chat-form">
        <input v-model="message" type="text" placeholder="Введите сообщение">
        <button class="button">
          <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24"><path d="M6.48 6.794l.49 1.963a1 1 0 0 1-1.94.486l-1-4a1 1 0 0 1 1.393-1.15l15 7a1 1 0 0 1 0 1.813l-15 7a1 1 0 0 1-1.385-1.18l2-7A1 1 0 0 1 7 11h4a1 1 0 0 1 0 2H7.754l-1.19 4.167L17.635 12 6.48 6.794z" fill="#000" fill-rule="nonzero"/></svg>
        </button>
      </form>
    </section>
  </div>
</template>

<script>
export default {
  name: 'AppChat',

  props: {
    checkedContact: {
      type: Object,
      default: null,
    },
  },

  data() {
    return {
      message: '',
    };
  },

  computed: {
    getIsListVisible() {
      return this.checkedContact?.messages?.length;
    },
    getEmptyMessage() {
      return this.checkedContact && (!this.checkedContact.messages || !this.checkedContact.messages.length)
        ? 'Отправьте первое сообщение'
        : '⇽ Выберите, кому хотите написать';
    },
  },
};
</script>

<style lang="scss" scoped>
.app-chat {
  padding: 10px;
}
</style>
