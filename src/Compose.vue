<template>
  <div>
    <a href="#composeModal" data-bs-toggle="modal" class="btn btn-compose">Compose</a>

    <div aria-hidden="true" role="dialog" tabindex="-1" id="composeModal" class="modal fade" style="display: none;">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h4 class="modal-title">New Message</h4>
            <button aria-hidden="true" data-bs-dismiss="modal" class="btn btn-close" type="button"></button>
          </div>

          <div class="modal-body">
            <form @submit.prevent="sendMessage" role="form" class="form">
              <div class="mb-3">
                <label class="col-lg-2 form-label" for="subject">Subject</label>
                <input type="text" v-model="message.subject" id="subject" class="form-control">
              </div>

              <div class="mb-3">
                <label class="col-lg-2 form-label" for="message">Message</label>
                <textarea v-model="message.content" rows="10" cols="30" class="form-control" id="message"></textarea>
              </div>

              <div class="mb-3 mt-3">
                <div class="col-lg-offset-2 col-lg-10">
                  <button class="btn btn-send" type="submit">Send</button>
                </div>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import moment from 'moment';
  import { eventBus } from './main';

  export default {
    data() {
      return {
        message: {
          subject: '',
          content: ''
        }
      };
    },
    methods: {
      sendMessage() {
        eventBus.$emit('sentMessage', {
          message: {
            subject: this.message.subject,
            content: this.message.content,
            isDeleted: false,
            type: 'outgoing',
            date: moment(),
            from: {
              name: 'Ann Kriviruchko',
              email: 'samplename@gmail.com'
            },
            attachments: []
          }
        });
      }
    }
  }
</script>
