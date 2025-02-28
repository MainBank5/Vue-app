<script setup>
import { ref } from 'vue';
import AccountSwitcher from './accountSwitcher.vue';
import { 
  Inbox, 
  File, 
  Send, 
  Trash, 
  Archive, 
  Users, 
  Clock, 
  MessageSquare,
  ShoppingCart,
  Tag 
} from 'lucide-vue-next';

const primaryItems = ref([
  { label: 'Inbox', icon: Inbox, count: 128, active: true },
  { label: 'Drafts', icon: File, count: 9, active: false },
  { label: 'Sent', icon: Send, count: null, active: false },
  { label: 'Junk', icon: Tag, count: 23, active: false },
  { label: 'Trash', icon: Trash, count: null, active: false },
  { label: 'Archive', icon: Archive, count: null, active: false },
]);

const secondaryItems = ref([
  { label: 'Social', icon: Users, count: 972, active: false },
  { label: 'Updates', icon: Clock, count: 342, active: false },
  { label: 'Forums', icon: MessageSquare, count: 128, active: false },
  { label: 'Shopping', icon: ShoppingCart, count: 8, active: false },
  { label: 'Promotions', icon: Tag, count: 21, active: false },
]);
</script>

<template>
  <div class="w-64 h-screen border-r border-gray-200 bg-white flex flex-col">
    <!-- Account Switcher -->
    <AccountSwitcher />
    
    <!-- Navigation Menu -->
    <nav class="flex-1 overflow-y-auto">
      <ul class="p-2">
        <!-- Primary navigation items -->
        <li v-for="item in primaryItems" :key="item.label">
          <button 
            class="w-full flex items-center px-3 py-2 text-sm rounded-md my-1"
            :class="[
              item.active 
                ? 'bg-black text-white font-medium' 
                : 'text-gray-700 hover:text-gray-900 hover:bg-gray-50'
            ]"
          >
            <component 
              :is="item.icon" 
              class="h-5 w-5 mr-2"
              :class="[
                item.active ? 'text-white' : 'text-gray-500'
              ]"
            />
            <span>{{ item.label }}</span>
            <span 
              v-if="item.count" 
              class="ml-auto text-sm font-medium"
              :class="[
                item.active ? 'text-white' : 'text-gray-600'
              ]"
            >
              {{ item.count }}
            </span>
          </button>
        </li>
        
        <!-- Divider -->
        <li class="border-t border-gray-200 my-2"></li>
        
        <!-- Secondary navigation items -->
        <li v-for="item in secondaryItems" :key="item.label">
          <button 
            class="w-full flex items-center px-3 py-2 text-sm rounded-md my-1"
            :class="[
              item.active 
                ? 'bg-black text-white font-medium' 
                : 'text-gray-700 hover:text-gray-900 hover:bg-gray-50'
            ]"
          >
            <component 
              :is="item.icon" 
              class="h-5 w-5 mr-2"
              :class="[
                item.active ? 'text-white' : 'text-gray-500'
              ]"
            />
            <span>{{ item.label }}</span>
            <span 
              v-if="item.count" 
              class="ml-auto text-sm font-medium"
              :class="[
                item.active ? 'text-white' : 'text-gray-600'
              ]"
            >
              {{ item.count }}
            </span>
          </button>
        </li>
      </ul>
    </nav>
  </div>
</template>