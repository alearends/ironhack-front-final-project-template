<template>
    <div class="max-w-screen-sm mx-auto px-4 py-10">
        <!-- Error Handling -->
        <div v-if="errorMsg" class="mb-10 p-4 rounded-md bg-nav shadow-lg">
            <p class="text-red-500">{{ errorMsg }}</p>
        </div>

        <!-- Login -->
        <form @submit.prevent="login" class="bg-gray-100 shadow-md rounded px-16 pt-6 pb-8 m-4">
            <div class="mb-6">
                <label class="block text-gray-700 text-sm font-bold mb-2 font-[Nunito]" for="email">
                    Dirección de correo electrónico
                    <input
                        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                        id="email" name="email" type="email" required v-model="email" placeholder="e-mail">
                </label>
            </div>
            <div class="mb-6">
                <label class="block text-gray-700 text-sm font-bold mb-2 font-[Nunito]" for="password">
                    Contraseña
                    <input
                        class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"
                        id="password" type="password" v-model="password" required>
                </label>
            </div>
            <div class="flex items-center justify-between mb-6 md:px-64">
                <button
                    class="bg-trale text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline hover:bg-blue-700 hover:border-gray-100 hover:text-gray-100 duration-200 border-solid border-2 border-transparent font-[Nunito]"
                    type="submit">
                    Iniciar sesión
                </button>
                <a class="inline-block align-baseline font-bold text-sm text-trale"
                    href="#">
                    ¿Olvidaste tu contraseña?
                </a>
            </div>
            <div class="text-center">
                <!-- <router-link
                    class="inline-block mx-auto my-6 md:my-0 text-l align-baseline text-center wordmark-color font-bold hover:text-trale duration-500 cursor-pointer font-[Nunito]"
                    :to="{ name: 'Register' }">¿No tienes una cuenta?<span class="text-resalta font-bold hover:wordmark-color">Regístrate aquí</span></router-link> -->
            </div>
        </form>

    </div>
</template>

<script setup>
import { ref } from "vue";
import { supabase } from '../supabase/supabase';
import { useRouter } from 'vue-router';
import { useUserStore } from "../store/user";
import { useBoardStore } from "../store/board";

//Create data/vars
const router = useRouter();
const username = ref('');
const email = ref('');
const password = ref('');
const errorMsg = ref(null);
const userStore = useUserStore();


//Login function
const login = async () => {
    try {
        userStore.sigIn(email.value, password.value)
        router.push({ name: "Home" });
    }
    catch (error) {
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
            errorMsg.value = null;
        }, 5000);
    }
}



</script>

<style scoped>
.text-resalta {
    color: #869FB1;
};

.bg-trale {
    background-color: #445D73;
}

.text-trale {
    color: #445D73;
}

.wordmark-color {
    color: #445D73;
}

.bg-nav {
    background-color: #F3F3F3;
}
</style>
