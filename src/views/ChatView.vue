<script setup>
import { onMounted, onUpdated, ref, watch } from 'vue';

import {
    app,
    auth,
    database,
    ref as refDb,
    set,
    push,
    onValue
} from '../firebaseConfig'
let chat = ref("");
let histories = ref([]);
let historyKey = ref("");
const bottomEl = ref(null);
const studentId = "6314110001"
let refChat;
const db = refDb(database, 'all_chat/');
const onSend = () => {
    if (chat.value != '') {
        push(refDb(database, `all_chat/${historyKey.value}`), {
            "user": studentId,
            "message": chat.value,
            "dateTime": new Date().toISOString()
        });
        chat.value = "";
    }
}


// const onSend = () => {
//     push (refDb(database,'all_chat/group_1') ,{
//         "user": studentId,
//         "message": chat.value,
//         "dateTime": new Date().toLocaleDateString('th-TH')
//     });
//     chat.value = "";
// }

onValue(db, (snapshot) => {
    const data = snapshot.val();
    histories.value = data;
})
// watch(histories, (newHistories, oldHistories) => {
//     bottomEl.value.scrollIntoView({ behavior: 'smooth' });
// });
onUpdated(() => {
    bottomEl.value.scrollIntoView({ behavior: 'smooth' });
});
const selectGroup = (key) => {
    historyKey.value = key;
}
// onMounted(() => {
//     push(refDb(database,'test'), {
//     "6314110001": "Hello World"
//   });
// });

</script>


<template>
    <div class="flex">
        <div class="overflow-scroll bg-black h-[90vh] w-[30%]">
            <div class="w-full bg-[#ffffff] h-26 border-b-4" style="cursor: pointer;" v-for="(group,index) in histories" :key="index" data-theme="cupcake" @click="selectGroup(index)">
                <div class="card-body">
                    <h2 class="card-title">{{ index }}</h2>
                    <p class="text-sm text-gray-500 px-4 py-2">พิมพ์ว่า: {{ group[Object.keys(group)[Object.keys(group).length - 1]].message  }}</p>
                </div>
            </div>
        </div>

        <div class="bg-gray h-[90vh] w-[70%]">
            <div class="overflow-y-scroll h-[90%] p-5">
                <div v-for="(history, index) in histories[historyKey]"
                    :class="`chat ${history.user == studentId ? 'chat-end' : 'chat-start'}`" :key="index">
                    <div class="chat-header">
                        {{ history.user }}
                        <time class="text-xs opacity-50">{{ history.dateTime }}</time>
                    </div>
                    <div class="chat-bubble">{{ history.message }}</div>
                </div>

                <div ref="bottomEl"></div>
            </div>
            <div class="flex h-[10%]">
                <input v-on:keyup.enter="onSend" v-model="chat" type="text" placeholder="Type here"
                    class="input input-bordered w-[80%] h-full">
                <button @click="onSend" class="w-[20%] btn h-full">send</button>
            </div>
        </div>
    </div>
</template>
<style scoped></style>
