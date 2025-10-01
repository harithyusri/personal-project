<script setup>
import Checkbox from '@/Components/Checkbox.vue';
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';
import { computed, ref } from 'vue'; // Import 'ref' for state management

// Define props for the status and password reset link visibility
// Simplifying to array/simple object syntax to ensure compilation works correctly.
defineProps(['canResetPassword', 'status']);

// --- New State for Password Visibility ---
const showPassword = ref(false);

// Computed property to dynamically set the password input type
// FIX: Changed to implicit return syntax to resolve JSX/return errors
const passwordInputType = computed(() =>
    showPassword.value ? 'text' : 'password'
);

// Method to toggle password visibility
const togglePasswordVisibility = () => {
    showPassword.value = !showPassword.value;
};
// ----------------------------------------

// Form state management using Inertia's useForm
const form = useForm({
    email: '',
    password: '',
    remember: false,
});

// Function to handle form submission
const submit = () => {
    form.post(route('login'), {
        onFinish: () => form.reset('password'),
    });
};

// Computed property for the title or brand text in the form
const appName = computed(() => {
    return 'Core Pulse'; // Replace with your actual app name if needed
});
</script>

<template>
    <Head title="Log in" />

    <!-- Full Screen Container with Gradient Background -->
    <div class="min-h-screen flex items-center justify-center p-4 sm:p-6 bg-gray-50 dark:bg-gray-900">
        
        <!-- Login Card Container -->
        <div class="w-full max-w-md bg-white dark:bg-gray-800 shadow-2xl rounded-xl p-8 space-y-6">
            
            <!-- Logo/Title Section -->
            <div class="text-center">
                <!-- Icon: Using a simple inline SVG for a modern look -->
                <svg class="mx-auto h-12 w-12 text-indigo-600 dark:text-indigo-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path>
                </svg>
                <h1 class="mt-4 text-3xl font-extrabold text-gray-900 dark:text-white">
                    Sign in to {{ appName }}
                </h1>
                <p class="mt-2 text-sm text-gray-500 dark:text-gray-400">
                    Enter your credentials to continue
                </p>
            </div>

            <!-- Session Status Message (if any) -->
            <div v-if="status" class="bg-green-100 border border-green-400 text-green-700 px-4 py-3 rounded relative text-sm font-medium">
                {{ status }}
            </div>

            <!-- Login Form -->
            <form @submit.prevent="submit" class="space-y-6">
                
                <!-- Email Input -->
                <div>
                    <InputLabel for="email" value="Email Address" class="block text-sm font-medium text-gray-700 dark:text-gray-300" />
                    <TextInput
                        id="email"
                        type="email"
                        class="mt-1 block w-full border-gray-300 dark:border-gray-600 dark:bg-gray-700 dark:text-white rounded-lg shadow-sm focus:ring-indigo-500 focus:border-indigo-500 transition duration-150"
                        v-model="form.email"
                        required
                        autofocus
                        autocomplete="username"
                        placeholder="you@example.com"
                    />
                    <InputError class="mt-2 text-red-500 text-xs" :message="form.errors.email" />
                </div>

                <!-- Password Input -->
                <div>
                    <InputLabel for="password" value="Password" class="block text-sm font-medium text-gray-700 dark:text-gray-300" />
                    
                    <!-- Wrapper for Input and Toggle Button -->
                    <div class="relative mt-1">
                        <TextInput
                            id="password"
                            :type="passwordInputType"
                            class="block w-full border-gray-300 dark:border-gray-600 dark:bg-gray-700 dark:text-white rounded-lg shadow-sm focus:ring-indigo-500 focus:border-indigo-500 transition duration-150 pr-10"
                            v-model="form.password"
                            required
                            autocomplete="current-password"
                            placeholder="••••••••"
                        />
                        
                        <!-- Password Visibility Toggle Button -->
                        <button 
                            type="button" 
                            @click="togglePasswordVisibility" 
                            class="absolute inset-y-0 right-0 pr-3 flex items-center text-gray-500 dark:text-gray-400 hover:text-gray-700 dark:hover:text-gray-200 focus:outline-none transition duration-150"
                            aria-label="Toggle password visibility"
                        >
                            <!-- Eye Icon (open/closed) using SVG -->
                            <svg v-if="!showPassword" class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z"></path>
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M2.458 12C3.732 7.943 7.523 5 12 5c4.477 0 8.268 2.943 9.542 7-1.274 4.057-5.065 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"></path>
                            </svg>
                            <svg v-else class="h-5 w-5" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13.875 18.825A10.05 10.05 0 0112 18c-4.477 0-8.268-2.943-9.542-7 1.274-4.057 5.065-7 9.542-7 2.408 0 4.697.837 6.643 2.25M21 12c-1.274 4.057-5.065 7-9.542 7M15 12a3 3 0 11-6 0 3 3 0 016 0z"></path>
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2"></path>
                            </svg>
                        </button>
                    </div>
                    <InputError class="mt-2 text-red-500 text-xs" :message="form.errors.password" />
                </div>
                
                <!-- Remember Me / Forgot Password Links -->
                <div class="flex items-center justify-between">
                    <!-- Remember Me Checkbox -->
                    <label class="flex items-center">
                        <Checkbox name="remember" v-model:checked="form.remember" class="text-indigo-600 focus:ring-indigo-500 dark:bg-gray-600 dark:checked:bg-indigo-500 dark:focus:ring-indigo-500" />
                        <span class="ms-2 text-sm text-gray-600 dark:text-gray-400">Remember me</span>
                    </label>

                    <!-- Forgot Password Link -->
                    <Link
                        v-if="canResetPassword"
                        :href="route('password.request')"
                        class="text-sm text-indigo-600 hover:text-indigo-500 dark:text-indigo-400 dark:hover:text-indigo-300 font-medium transition duration-150"
                    >
                        Forgot password?
                    </Link>
                </div>

                <!-- Login Button -->
                <PrimaryButton
                    type="submit"
                    class="w-full justify-center py-3 text-lg font-semibold tracking-wider bg-indigo-600 hover:bg-indigo-700 dark:bg-indigo-500 dark:hover:bg-indigo-600 text-white rounded-lg shadow-md transition duration-200"
                    :class="{ 'opacity-50 cursor-not-allowed': form.processing }"
                    :disabled="form.processing"
                >
                    Log in
                </PrimaryButton>
            </form>

            <!-- Register Link -->
             <div class="text-center pt-4 border-t dark:border-gray-700">
                <Link
                    :href="route('register')"
                    class="text-sm font-medium text-gray-500 hover:text-indigo-600 dark:text-gray-400 dark:hover:text-indigo-500 transition duration-150"
                >
                    Don't have an account? Register
                </Link>
            </div>

        </div>
    </div>
</template>
