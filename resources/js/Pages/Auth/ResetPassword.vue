<script setup>
import BreezeButton from '@/Components/Button.vue';
import BreezeGuestLayout from '@/Layouts/AuthLayout.vue';
import BreezeInput from '@/Components/Input.vue';
import BreezeInputError from '@/Components/InputError.vue';
import BreezeLabel from '@/Components/Label.vue';
import {Head, useForm} from '@inertiajs/vue3';

const props = defineProps({
    email: String,
    token: String,
});

const form = useForm({
    token: props.token,
    email: props.email,
    password: '',
    password_confirmation: '',
});

const submit = () => {
    form.post("/reset-password", {
        onFinish: () => form.reset('password', 'password_confirmation'),
    });
};
</script>

<template>
    <BreezeGuestLayout>
        <Head title="Reset Password"/>

        <form @submit.prevent="submit">
            <div>
                <BreezeLabel for="email" value="Email"/>
                <BreezeInput id="email" type="email" class="mt-1 block w-full" v-model="form.email" required autofocus
                             autocomplete="username"/>
                <BreezeInputError class="mt-2" :message="form.errors.email"/>
            </div>

            <div class="mt-4">
                <BreezeLabel for="password" value="Пароль"/>
                <BreezeInput id="password" type="password" class="mt-1 block w-full" v-model="form.password" required
                             autocomplete="new-password"/>
                <BreezeInputError class="mt-2" :message="form.errors.password"/>
            </div>

            <div class="mt-4">
                <BreezeLabel for="password_confirmation" value="Подтвердите пароль"/>
                <BreezeInput id="password_confirmation" type="password" class="mt-1 block w-full"
                             v-model="form.password_confirmation" required autocomplete="new-password"/>
                <BreezeInputError class="mt-2" :message="form.errors.password_confirmation"/>
            </div>

            <div class="flex items-center justify-end mt-4">
                <BreezeButton :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                    Установить пароль
                </BreezeButton>
            </div>
        </form>
    </BreezeGuestLayout>
</template>
