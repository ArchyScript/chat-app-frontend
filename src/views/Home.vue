<script setup lang="ts">
import { ref, reactive, onMounted, onBeforeUnmount } from 'vue'
import { io, type Socket } from "socket.io-client"
import { useRouter } from 'vue-router';
type Message = {
    text: string
    name?: string
    timeStamp: string
    sender: "user" | "friend"
}

// 
const socket = ref<Socket>()
const router = useRouter()
const rooms = ['vue installation', 'vue guide', 'vue api', 'vue examples']
const state = reactive({
    username: '',
    room: rooms[0]
})
const onSubmit = () => {
    console.log("SUBMIT")
    router.push(`/chats?username=${state.username}&room=${state.room}`)
}
const newMessage = ref("")
const messages = ref<Message[]>([
    {
        text: "Hey! How are you?",
        sender: "friend",
        name: "Script",
        timeStamp: "10:02",
    },
])

const sendMessage = () => {

    console.log(newMessage.value)
    // if (newMessage.value.trim() !== "") {
    //   messages.value.push({
    //     text: newMessage.value,
    //     sender: "user",
    //     timeStamp: "just now",
    //   })
    //   newMessage.value = ""
    //   // Simulate a friend reply
    //   setTimeout(() => {
    //     messages.value.push({
    //       text: "That sounds great!",
    //       sender: "friend",
    //       timeStamp: "just now",
    //       name: "TXO Operations",
    //     })
    //   }, 1000)
    // }
}


onMounted(() => {
    socket.value = io('http://localhost:5000')
    console.log('socket.value', socket.value)
})

onBeforeUnmount(() => {
    console.log('DISCONNECTED')
    socket.value?.disconnect()

})
</script>

<template>
    <div class="min-h-[100vh] py-6 w-96 mx-auto ">
        <div class="relative w-full h-[580px] rounded-2xl mx-auto bg-white border overflow-hidden">
            <div
                class="absolute top-0 left-0 w-full py-4 z-1 px-[22px] bg-[#F5FDFF] rounded-t-2xl flex items-center justify-between">
                <div class="space-y-1">
                    <h6 class="text-sm font-medium text-complimentary leading-5">
                        Login page
                    </h6>
                    <p class="flex items-center space-x-1">
                        <span class="h-1.5 w-1.5 rounded-full bg-warning inline-block">
                        </span>
                        <span class="text-[10px] font-medium text-[#626262] leading-[13px]">
                            Seen 3 hours ago
                        </span>
                    </p>
                </div>

             </div>

        
            <!-- Input Section -->
            <div class="absolute bottom-0 left-0 w-full h-[88px] mt-4 bg-white z-1 border-t pt-3.5 px-6 rounded-b-2xl">
                <div class="flex items-center space-x-2 bg-white border p-2 pl-3 rounded-xl">
           
                    <input v-model="state.username" type="text" placeholder="Enter username"
                        class="flex-grow px-4 py-2 border-none focus:outline-none focus:border-none" />


                    <!-- send btn -->
                    <button @click="onSubmit" class="px-4 py-1 text-sm rounded-lg font-medium text-white bg-blue-500"
                        :class="!state.username && 'cursor-not-allowed !bg-blue-300'" :disabled="!state.username">
                        submit
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped></style>
