<template>
  <div class="inbox-body">
    <div class="mail-option">
      <div class="btn-group">
        <a href="#" class="btn" @click="refresh">
          <i class="fa fa-refresh"></i>&nbsp; Refresh
        </a>
      </div>
      <div class="btn-group">
        <a href="#" class="btn" @click="deleteChecked">
          <i class="fa fa-trash-o"></i>&nbsp; Delete
        </a>
      </div>
    </div>

    <app-messages :messages="incomingMessages"></app-messages>
  </div>
</template>

<script>
  import Messages from "./Messages";
  import { eventBus } from "./main";

  export default {
    props: {
      data: {
        type: Object,
        required: true
      }
    },
    methods: {
      refresh() {
        eventBus.$emit('refreshMessages');
      },
      deleteChecked() {
        this.data.messages.forEach(m => {
          if (m.isChecked) {
            m.isDeleted = true;
            m.isChecked = false;
          }
        });

        this.data.messages = this.data.messages.filter(m => !m.isChecked);

      }
    },
    computed: {
      incomingMessages() {
        return this.data.messages.filter(function (message) {
          return (message.type === 'incoming' && !message.isDeleted);
        });
      }
    },
    components: {
      'app-messages': Messages
    }
  }
</script>
