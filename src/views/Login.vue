<template>
    <HomeNavBar />
    <div>
        <!-- Error Handling -->
        <div v-if="errorMsg" class="mb-10 p-4 rounded-md bg-nav shadow-md m-4 text-center">
            <p class="text-red-500 font-semibold text-center">{{ errorMsg }}</p>
        </div>

        <!-- Login -->
        <form @submit.prevent="signInWithEmail" class="bg-gray-100 shadow-md rounded px-16 pt-6 pb-8 m-4">
            <h2 class="text-2xl font-bold text-trale mb-4">Login</h2>
            <div class="mb-6">
                <label class="block text-gray-700 text-sm font-bold mb-2 font-[Nunito]" for="email">
                    Dirección de correo electrónico
                </label>
                <input
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
                    id="email" type="email" required v-model="email">

            </div>
            <div class="mb-6">
                <label class="block text-gray-700 text-sm font-bold mb-2 font-[Nunito]" for="password">
                    Contraseña
                </label>
                <input
                    class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"
                    id="password" type="password" v-model="password" required>
            </div>
            <div class="flex items-center justify-between mb-6 md:px-64">
                <button
                    class="bg-trale text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline hover:bg-blue-700 hover:border-gray-100 hover:text-gray-100 duration-200 border-solid border-2 border-transparent font-[Nunito]"
                    type="submit">
                    Iniciar sesión
                </button>
                <router-link
                    class="inline-block align-baseline text-center text-l wordmark-color font-bold hover:text-resalta duration-500 cursor-pointer font-[Nunito]"
                    :to="{ name: 'Register' }">¿No tienes una cuenta?<br><span class="text-resalta underline hover:wordmark-color">  Regístrate aquí</span></router-link>
                </div>
        </form>

    </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import {supabase} from '../supabase/supabase';
import { useRouter } from 'vue-router';
import HomeNavBar from '../components/HomeNavBar.vue';

//Create data/vars
const router = useRouter();
const email = ref("");
const password = ref("");
const errorMsg = ref(null);

//Login function
const signInWithEmail = async () => {
    try {
        const {data, error} = await supabase.auth.signInWithPassword({
            email: email.value,
            password: password.value,
        });
        if (error) throw error;
        router.push({name: "TheTrale"});
    }
    catch(error){
        errorMsg.value = `Error: ${error.message}`;
        setTimeout(() => {
        errorMsg.value = null;
        }, 5000);
    }
}

//reseteando valores........ Pregunta para IronHack
// const resetForm = () => {
//     email.value ="";
//     password.value ="";
// }

// onMounted(async () =>{
//     resetForm()
// });

</script>

<style scoped>
.bg-trale {
    background-color: #445D73;
}

.text-resalta {
    color: #869FB1;
};

.text-trale{
    color: #445D73;
}

.wordmark-color {
    color: #445D73;
}

.bg-nav {
    background-color: #F3F3F3;
}

</style>
