<template>
    <span v-if="present"  v-text="typing"></span>
</template>

<script>
    import {eventBus} from '../app';
    export default {
        name: "MessageTyping",
        props: [''],

        data() {
            return {
                typing: '',
                present: false
            }
        },
        created(){

            /* this function work only on the prowser but not as real time
            eventBus.$on('messageTyped', (user) => {
                this.present = true;
                this.typing = user.user.name + ' is typing...';

            });

            eventBus.$on('messageIgnored', (present) => {
                this.present = false;
                this.typing = '';
            });*/


            Echo.private('chat')
                .listenForWhisper('ignoring', (e) => {

                    this.present = false;

                })
                .listenForWhisper('typing', (e) => {
                    this.present = true;
                    this.user = e.user;

                    this.typing = this.user.name + ' is typing...';
                })



        },


        methods: {

        }

    }
</script>

<style scoped>

</style>
