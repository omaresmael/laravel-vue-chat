<template>
    <div class="input-group">
        <input id="btn-input" type="text" name="message" class="form-control input-sm" placeholder="Type your message here..." v-model="newMessage" @keyup.enter="sendMessage" @keyup="typing" @blur="ignoring">

        <span class="input-group-btn" style="margin-top: 4px; margin-left: 4px">
            <button class="btn btn-primary btn-sm" id="btn-chat" @click="sendMessage">
                Send
            </button>
        </span>
    </div>
</template>

<script>
    import {eventBus} from '../app';
    export default {

        props: ['user'],

        data() {
            return {
                newMessage: ''
            }
        },

        methods: {
            sendMessage() {
                this.$emit('messagesent', {
                    user: this.user,
                    message: this.newMessage
                });

                this.newMessage = ''
            },
           /* typing(){
              if (this.newMessage !== ''){

                  eventBus.$emit('messageTyped', {

                      user: this.user,
                  });
              }

            },*/
            typing() {
                let channel = Echo.private('chat');
                channel.whisper('typing', {
                    user: Laravel.user,
                });

            },
            /*ignoring(){
                eventBus.$emit('messageIgnored');
            }*/
            ignoring(){
                let channel = Echo.private('chat');
                channel.whisper('ignoring', {
                    user: Laravel.user,
                });
            }

        }
    }
</script>

<style scoped>

</style>
