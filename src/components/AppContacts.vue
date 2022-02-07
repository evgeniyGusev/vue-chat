<template>
  <div class="app-contacts">
    <h3 class="title">
      Контакты
    </h3>

    <div v-if="isLoadError" class="load-error">
      Ошибка загрузки контактов

      <button class="reload-contacts">
        повторить
      </button>
    </div>

    <ul v-else class="contacts-list">
      <li
        v-for="contact in contacts"
        :key="contact.id"
        class="contacts-item"
      >
        <img :src="contact.avatar" :alt="contact.name" class="avatar">
        <div class="contact-data">
          <div class="name">
            {{ contact.name }}
          </div>
          <div class="last-message">
            {{ contact.messages.at(-1).value }}
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'AppContacts',

  props: {
    isLoadError: {
      type: Boolean,
      default: false,
    },
    contacts: {
      type: Array,
      default: () => [],
    },
  },
};
</script>

<style lang="scss" scoped>
.app-contacts {
  overflow-y: auto;
  flex-basis: 30%;
  height: 100%;
  padding: 10px;
  border-radius: 10px 0 0 10px;
  box-shadow: 1px 0 3px #444;
  border-right: 1px solid #f6f6f6;

  .title {
    margin-bottom: 10px;
    padding-bottom: 10px;
    border-bottom: 1px solid #999;
  }

  .contacts-list {
    .contacts-item {
      display: flex;
      align-items: flex-start;

      &:not(:last-child) {
        margin-bottom: 20px;
      }
    }
  }
}
</style>
