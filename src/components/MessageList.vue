<template>
    <div class="chat">
        <div v-for="(message, index) in messages" :key="index"
            :class="['message-container', message.userIndex === 0 ? 'left' : 'right']">
            <p>{{ message.user.name.first }} {{ message.user.name.last }}:</p>
            <div class="message" :style="{ backgroundColor: message.color }">
                <div>{{ message.text }}</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'MessageList',
    props: {
        messages: {
            type: Array,
            required: true,
        },
    },
};
</script>

<style scoped>
.chat {
    max-height: 450px;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
}

.message-container {
    display: flex;
    flex-direction: column;
    margin: 10px;
}

.left {
    align-items: flex-start;
}

.right {
    align-items: flex-end;
}

.message {
    padding: 10px;
    border-radius: 10px;
    max-width: 70%;
    font-size: 16px;
    line-height: 1.5;
    word-wrap: break-word;
    color: black;
    margin-top: -10px;
}
</style>
