<template>
    <div class="p-2 border-b border-gray-200">
      <Select v-model="selectedAccount">
        <SelectTrigger class="w-full">
          <div class="flex items-center">
            <img 
              :src="getIconForAccount(selectedAccount)" 
              :alt="getIconAltText(selectedAccount)" 
              class="h-4 w-4 mr-2" 
            />
            <span>{{ selectedAccount }}</span>
          </div>
        </SelectTrigger>
        <SelectContent>
          <SelectGroup>
            <SelectItem value="Alicia Koch">
              <div class="flex items-center">
                <img src="@/assets/icon-triangle.png" alt="Triangle icon" class="h-4 w-4 mr-2" />
                <span>Alicia Koch</span>
              </div>
            </SelectItem>
            <SelectItem value="alicia@example.com">
              <div class="flex items-center">
                <img src="@/assets/icon-triangle.png" alt="Triangle icon" class="h-4 w-4 mr-2" />
                <span>alicia@example.com</span>
              </div>
            </SelectItem>
            <SelectItem value="alicia@gmail.com">
              <div class="flex items-center">
                <img src="@/assets/icon-G.png" alt="Google icon" class="h-4 w-4 mr-2" />
                <span>alicia@gmail.com</span>
              </div>
            </SelectItem>
            <SelectItem value="alicia@me.com">
              <div class="flex items-center">
                <img src="@/assets/icon-m.png" alt="Apple icon" class="h-4 w-4 mr-2" />
                <span>alicia@me.com</span>
              </div>
            </SelectItem>
          </SelectGroup>
        </SelectContent>
      </Select>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import { 
    Select,
    SelectContent,
    SelectGroup,
    SelectItem,
    SelectTrigger,
  } from '@/components/ui/select';
  
  const selectedAccount = ref('alicia@example.com');
  
  // Function to get the appropriate icon based on the account
  const getIconForAccount = (account) => {
    if (account.includes('@gmail.com')) {
      return new URL('@/assets/icon-G.png', import.meta.url).href;
    } else if (account.includes('@me.com')) {
      return new URL('@/assets/icon-m.png', import.meta.url).href;
    } else {
      return new URL('@/assets/icon-triangle.png', import.meta.url).href;
    }
  };
  
  // Function to get the appropriate alt text for accessibility
  const getIconAltText = (account) => {
    if (account.includes('@gmail.com')) {
      return 'Google icon';
    } else if (account.includes('@me.com')) {
      return 'Apple icon';
    } else {
      return 'Triangle icon';
    }
  };
  </script>
  
  <style scoped>
  /* Make sure the selected indicator from shadcn is visible */
  :deep(.select-item[data-state="checked"]) {
    /* Consider adding styles here if needed */
  }
  
  /* Add any additional styling as needed */
  </style>