<template>
  <div class="app">
    <app-contacts
      :is-load-error="isLoadContactsError"
      :contacts="contacts"
      :checked-contact-name="checkedContact ? checkedContact.name : ''"
      @set-contact="setCheckedContact"
      @reload="fetchContacts"
    />
    <app-chat
      :checked-contact="checkedContact"
      @send-message="pushMessage"
    />
  </div>
</template>

<script>
import AppContacts from '@/components/AppContacts.vue';
import AppChat from '@/components/AppChat.vue';
import mockMessages from '@/assets/messages';

export default {
  name: 'App',

  components: { AppChat, AppContacts },

  data() {
    return {
      contacts: [],
      checkedContact: null,
      isLoadContactsError: false,
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
              avatar: 'https://placekitten.com/g/35/35',
              isOnline: Math.random() >= 0.5,
              messages: new Array((Math.floor(Math.random() * 7)))
                .fill(undefined)
                .map(() => ({
                  inbox: Math.random() >= 0.3,
                  time: 'Вчера',
                  value: mockMessages[Math.floor(Math.random() * mockMessages.length)],
                })),
            }));
        } else {
          this.isLoadContactsError = true;
        }
      } catch {
        this.isLoadContactsError = true;
      }
    },
    setCheckedContact(contact) {
      this.checkedContact = contact;
    },
    pushMessage(message) {
      this.checkedContact.messages.push(message);
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
  width: 900px;
  height: 70vh;
  border-radius: 10px;
  box-shadow: 0 0 10px #444;
}
</style>
