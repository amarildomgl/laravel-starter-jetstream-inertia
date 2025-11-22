<template>
    <AppLayout title="Dashboard">
        <template #header>
            <div
                class="flex flex-col gap-4 md:flex-row md:items-center md:justify-between"
            >
                <h2 class="text-xl font-semibold leading-tight">Dashboard</h2>

                <Button
                    external
                    target="_blank"
                    class="items-center gap-2 max-w-xs bg-gradient-to-r from-purple-500 via-pink-500 to-orange-500 hover:from-purple-600 hover:via-pink-600 hover:to-orange-600 text-white border-0"
                    v-slot="{ iconSizeClasses }"
                    href="https://www.instagram.com/iami.ao"
                >
                    <InstagramIcon
                        aria-hidden="true"
                        :class="iconSizeClasses"
                    />
                    <span>Siga a iami no instagram</span>
                </Button>
            </div>
        </template>

        <div
            class="p-6 flex flex-col gap-6 overflow-hidden bg-white rounded-md shadow-md lg:flex-row md:justify-between dark:bg-dark-eval-1"
        >
            <div>You're logged in!</div>

            <div></div>
        </div>
    </AppLayout>
</template>

<script setup>
import AppLayout from "@/Layouts/AppLayout.vue";
import Button from "@/Components/Button.vue";
import { InstagramIcon } from "@/Components/Icons/Brands";
</script>
