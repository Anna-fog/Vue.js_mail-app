<template>
  <aside class="lg-side">
    <div class="inbox-head">
      <h3>{{ currentView.title }}</h3>
    </div>

    <keep-alive>
      <component :is="currentView.tag" :data="currentView.data"></component>
    </keep-alive>

  </aside>
</template>

<script>
  import Inbox from "./Inbox";
  import Sent from "./Sent";
  import Important from "./Important";
  import Trash from "./Trash";
  import ViewMessage from "./ViewMessage";
  import { eventBus } from "./main";

  export default {
    props: {
      messages: {
        type: Array,
        required: true
      }
    },
    created() {
      eventBus.$on('changeView', (data) => {
        let temp = [{
          tag: data.tag,
          title: data.title,
          data: data.data || {}
        }];
        this.history = [...temp, ...this.history];
      });
    },
    data() {
      return {
        history: [
          {
            tag: 'app-inbox',
            title: 'Inbox',
            data: {
              messages: null
            }
          }
        ]
      };
    },
    computed: {
      currentView() {
        let current = this.history[0];
        this.$set(current.data, "messages", this.messages);
        return current;

      },
      previousView() {
        return typeof this.history[1] !== 'undefined' ? this.history[1] : null;
      }
    },
    components: {
      'app-inbox': Inbox,
      'app-sent': Sent,
      'app-important': Important,
      'app-trash': Trash,
      'app-view-message': ViewMessage
    }
  }
</script>
