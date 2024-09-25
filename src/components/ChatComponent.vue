<template>
    <div class="container mt-4" v-if="users.length > 0">
        <div class="row">
            <div class="col-md-3 user-container text-center">
                <img :src="users[0].picture.large" :alt="users[0].name.first" class="user-image mb-2" />
                <h3>{{ users[0].name.first }} {{ users[0].name.last }}</h3>
                <input type="color" v-model="users[0].messageColor" class="form-control" />
                <textarea v-model="users[0].newMessage" placeholder="Escribe un mensaje..." class="textarea form-control mt-2"></textarea>
                <button @click="sendMessage(0)" class="btn btn-primary mt-2">Enviar</button>
            </div>

            <div class="col-md-5 chat-container">
                <MessageList :messages="messages" />
            </div>

            <div class="col-md-3 user-container text-center">
                <img :src="users[1].picture.large" :alt="users[1].name.first" class="user-image mb-2" />
                <h3>{{ users[1].name.first }} {{ users[1].name.last }}</h3>
                <input type="color" v-model="users[1].messageColor" class="form-control" />
                <textarea v-model="users[1].newMessage" placeholder="Escribe un mensaje..." class="textarea form-control mt-2"></textarea>
                <button @click="sendMessage(1)" class="btn btn-primary mt-2">Enviar</button>
            </div>
        </div>
    </div>
    <div v-else>
        <p>Cargando usuarios...</p>
    </div>
</template>

<script>
import axios from 'axios';
import MessageList from './MessageList.vue';

export default {
    name: 'ChatComponent',
    components: {
        MessageList,
    },
    data() {
        return {
            users: [],
            messages: [],
        };
    },
    async mounted() {
        try {
            const response = await axios.get('https://randomuser.me/api/?results=2');
            this.users = response.data.results.map(user => ({
                ...user,
                newMessage: '',
                messageColor: '#CCEBFD',
            }));
        } catch (error) {
            console.error("Error al obtener los usuarios:", error);
        }
    },
    methods: {
        sendMessage(userIndex) {
            const user = this.users[userIndex];
            if (user.newMessage.trim()) {
                this.messages.push({
                    text: user.newMessage,
                    user,
                    userIndex,
                    color: user.messageColor,
                });
                user.newMessage = '';
            } else {
                alert("Por favor escribe un mensaje antes de enviar.");
            }
        },
    },
};
</script>

<style scoped>
.user-container {
    background-color: #FFFFFF;
    padding: 15px;
    border-radius: 10px;
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
}

.user-image {
    object-fit: cover;
}

.chat-container {
    background-color: #EFE6DD;
    padding: 10px;
    border-radius: 10px;
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
}

.textarea{
    height: 200px;
}
</style>
