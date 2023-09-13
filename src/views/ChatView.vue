<script setup>
import { onMounted,ref } from 'vue';
import {
    app,
    auth,
    database,
    ref as refDb,
    set,
    push
} from '../firebaseConfig'
let chat = ref("");
const studentId = "6314110001"
const onSend = () => {
    push (refDb(database,'all_chat/group_1') ,{
        "user": studentId,
        "message": chat.value,
        "dateTime": new Date().toLocaleDateString('th-TH')
    });
    chat.value = "";
}

// onMounted(() => {
//     push(refDb(database,'test'), {
//     "6314110001": "Hello World"
//   });
// });


</script>


<template>
    <div class="flex">
        <div class="overflow-scroll bg-white h-[90vh] w-[30%]">
            <div class="w-full bg-[#BCB2B0] h-36 border-orange-500 border-4" v-for="i in 100">

            </div>
        </div>
        <div class="bg-gray h-[90vh] w-[70%]">
            <div class="overflow-y-scroll h-[90%] p-5">
                <!-- <p v-for="i in 100">test</p> -->
                <div class="chat chat-start">
                    <div class="chat-bubble chat-bubble-warning">It's over Anakin, <br />I have the high ground.</div>
                </div>
                <div class="chat chat-end">
                    <div class="chat-bubble chat-bubble-warning">You underestimate my power!</div>
                </div>
            </div>
            <div class="flex h-[10%]">
                <input v-on:keyup.enter="onSend"  v-model="chat" type="text" placeholder="Type here" class="input input-bordered w-[80%] h-full">
                <button @click="onSend" class="w-[20%] btn h-full">send</button>
            </div>
        </div>
    </div>
</template>
<style scoped>

</style>
