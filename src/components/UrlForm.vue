<!-- TODO: Validate input -->
<template>
    <form @submit.prevent="onSubmit">
        <input type="text" v-model="input" />
        <button type="submit" class="btn btn-primary">Submit</button>
    </form>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const input = ref('');
const emit = defineEmits(['onSubmit'])

const onSubmit = async () => {
    const API_URL = import.meta.env.VITE_API_URL

    try {
        const res = await fetch(`${API_URL}api/v1/shorten`, {
            method: 'POST',
            credentials: 'include',
            mode: 'cors',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({
                url: input.value
            })
        })

        const data = await res.json()

        if (res.ok) {
            emit('onSubmit', data)
            input.value = ''
        } else {
            // TODO: Handle redirect to login
        }
    }
    catch (error) {
        console.log(error)
    }
}
</script>

<style scoped>
form {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 0.5rem;
    width: 100%;
    margin-top: 1rem;
}

input {
    width: 100%;
    padding: 0.5rem;
    color: white;
    background: transparent;
    border: 1px solid rgba(255, 255, 255, 0.5);
    border-radius: 10px;
    font-size: 1rem;
    transition: border 0.1s;
}

input:focus {
    outline: none;
    border: 1px solid white;
}
</style>