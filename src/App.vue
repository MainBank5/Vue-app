<script setup>
import Sidebar from "./components/sidebar.vue";
import mainMail from "./components/mail/mainMail.vue";
</script>

<template>
  <div class="flex h-screen">
    <Sidebar />
    <main class="flex-1 w-full py-5 px-2">
      <mainMail />
    </main>
  </div>
</template>