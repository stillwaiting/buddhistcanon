<script setup>
import BreezeButton from '@/Components/Button.vue';
import BreezeGuestLayout from '@/Layouts/AuthLayout.vue';
import BreezeInput from '@/Components/Input.vue';
import BreezeInputError from '@/Components/InputError.vue';
import BreezeLabel from '@/Components/Label.vue';
import {Head, useForm} from '@inertiajs/vue3';

defineProps({
    status: String,
});

const form = useForm({
    email: '',
});

const submit = () => {
    form.post("/forgot-password");
};
</script>

<template>
    <BreezeGuestLayout>
        <Head title="Forgot Password"/>

        <div class="mb-4 text-sm text-gray-600">
            Забыли пароль? Ничего страшного! Просто укажите свой адрес электронной почты, и мы отправим вам ссылку для
            сброса пароля.
        </div>

        <div v-if="status" class="mb-4 font-medium text-sm text-green-600">
            {{ status }}
        </div>

        <form @submit.prevent="submit">
            <div>
                <BreezeLabel for="email" value="Email"/>
                <BreezeInput id="email" type="email" class="mt-1 block w-full" v-model="form.email" required autofocus
                             autocomplete="username"/>
                <BreezeInputError class="mt-2" :message="form.errors.email"/>
            </div>

            <div class="flex items-center justify-end mt-4">
                <BreezeButton :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                    Сбросить пароль
                </BreezeButton>
            </div>
        </form>
    </BreezeGuestLayout>
</template>
