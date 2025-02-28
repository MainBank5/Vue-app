<script setup>
import MailSidebar from './mailSidebar.vue';
import mailContent from './mailContent.vue';
</script>
<template>
    <div class="flex h-screen bg-gray-100">
      <!-- Sidebar -->
      <MailSidebar />
      
      <!-- Main Content Area -->
      <div class="flex-1">
        <mailContent />
      </div>
    </div>
  </template>

