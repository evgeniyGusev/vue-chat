<template>
  <div class="app">
    <app-contacts
      :is-load-error="isLoadContactsError"
      :contacts="contacts"
    />
    <app-chat />
  </div>
</template>

<script>
import AppContacts from '@/components/AppContacts.vue';
import AppChat from '@/components/AppChat.vue';

export default {
  name: 'App',

  components: { AppChat, AppContacts },

  data() {
    return {
      contacts: [],
      isLoadContactsError: true,
    };
  },

  async created() {
    await this.fetchContacts();
  },

  methods: {
    async fetchContacts() {
      this.isLoadContactsError = false;

      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');

        if (response.ok) {
          const contacts = await response.json();

          this.contacts = contacts
            .map(({ id, name, username }) => ({
              id,
              name,
              username,
              avatar: 'https://placekitten.com/g/30/30',
              messages: new Array(5)
                .fill(undefined)
                .map(() => ({
                  inbox: true,
                  value: 'Lorem ipsum dolor sit amet',
                })),
            }));
        } else {
          this.isLoadContactsError = true;
        }
      } catch {
        this.isLoadContactsError = true;
      }
    },
  },
};
</script>

<style scoped lang="scss">
.app {
  display: flex;
  align-items: flex-start;
  max-width: 1140px;
  max-height: 70vh;
  width: 1140px;
  height: 70vh;
  border-radius: 10px;
  box-shadow: 0 0 10px #444;
}
</style>
