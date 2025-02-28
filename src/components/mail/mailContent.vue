<template>
    <div class="flex flex-col h-screen bg-white">
      <!-- Top bar -->
      <div class="flex items-center border-b border-gray-200 p-2">
        <h1 class="text-2xl font-semibold mr-6">Inbox</h1>
        
        <!-- Tabs -->
        <div class="flex space-x-1 bg-gray-100 p-1 rounded-lg mr-4">
          <button class="px-3 py-1 text-sm rounded-md bg-white shadow-sm">All mail</button>
          <button class="px-3 py-1 text-sm rounded-md text-gray-600 hover:text-gray-900">Unread</button>
        </div>
        
        <!-- First set of action buttons (immediately after tabs) -->
        <div class="flex items-center space-x-2 mr-4">
          <button class="p-2 text-gray-500 hover:text-gray-700 rounded-md">
            <Archive class="h-5 w-5" />
          </button>
          <button class="p-2 text-gray-500 hover:text-gray-700 rounded-md">
            <ArchiveX class="h-5 w-5" />
          </button>
          <button class="p-2 text-gray-500 hover:text-gray-700 rounded-md">
            <Trash2 class="h-5 w-5" />
          </button>
        </div>
        
        <!-- Separator -->
        <div class="border-l border-gray-200 h-6 mx-2"></div>
        
        <!-- Clock icon -->
        <button class="p-2 text-gray-500 hover:text-gray-700 rounded-md">
          <Clock class="h-5 w-5" />
        </button>
        
        <!-- Separator and navigation controls (pushed to the right) -->
        <div class="ml-auto flex items-center">
          <button class="p-2 text-gray-500 hover:text-gray-700 rounded-md">
            <CornerUpLeft class="h-5 w-5" />
          </button>
  
          <button class="p-2 text-gray-500 hover:text-gray-700 rounded-md">
            <ReplyAll class="h-5 w-5" />
          </button>
  
          <button class="p-2 text-gray-500 hover:text-gray-700 rounded-md">
            <CornerUpRightIcon class="h-5 w-5" />
          </button>
          
          <div class="border-l border-gray-200 h-6 mx-2"></div>
          
          <button class="p-2 text-gray-500 hover:text-gray-700 rounded-md">
            <MoreVertical class="h-5 w-5" />
          </button>
        </div>
      </div>
      
      <!-- Main content -->
      <div class="flex flex-1 overflow-hidden">
        <!-- Email list -->
        <div class="w-1/3 border-r border-gray-200 flex flex-col">
          <!-- Search bar -->
          <div class="p-4">
            <div class="relative">
              <Search class="h-5 w-5 absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400" />
              <input 
                type="text" 
                placeholder="Search" 
                class="w-full pl-10 pr-4 py-2 rounded-md border border-gray-200 focus:outline-none focus:ring-2 focus:ring-gray-200"
              />
            </div>
          </div>
          
          <!-- Email list with custom scrollbar -->
          <div class="flex-1 overflow-auto scrollbar-thin scrollbar-thumb-transparent hover:scrollbar-thumb-gray-300 scrollbar-track-transparent">
            <div 
              v-for="email in emails" 
              :key="email.id" 
              class="border-b border-gray-200 p-4 cursor-pointer hover:bg-gray-50"
              :class="{ 'bg-gray-50': selectedEmail && selectedEmail.id === email.id }"
              @click="selectEmail(email)"
            >
              <div class="flex items-start">
                <!-- Avatar -->
                <div class="w-8 h-8 rounded-full bg-gray-200 flex items-center justify-center text-gray-700 mr-3 flex-shrink-0 mt-1">
                  {{ email.sender.charAt(0).toUpperCase() }}
                </div>
                
                <!-- Email content -->
                <div class="flex-1 min-w-0">
                  <div class="flex items-center justify-between mb-1">
                    <h3 class="font-medium truncate">{{ email.sender }}</h3>
                    <span class="text-sm text-gray-500 ml-2 flex-shrink-0">{{ email.date }}</span>
                  </div>
                  <p class="font-medium text-sm mb-1">{{ email.subject }}</p>
                  <p class="text-sm text-gray-600 line-clamp-2">{{ email.preview }}</p>
                  
                  <!-- Tags -->
                  <div class="flex mt-2 space-x-2">
                    <span 
                      v-for="tag in email.tags" 
                      :key="tag"
                      class="px-2 py-1 text-xs rounded-md"
                      :class="{
                        'bg-gray-900 text-white': tag === 'work',
                        'bg-gray-100 text-gray-600': tag === 'important',
                        'bg-gray-100 text-gray-600': tag === 'meeting',
                        'bg-gray-100 text-gray-600': tag === 'personal',
                        'bg-gray-100 text-gray-600': tag === 'budget',
                      }"
                    >
                      {{ tag }}
                    </span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
        
        <!-- Email content -->
        <div class="flex-1 flex flex-col overflow-auto scrollbar-thin scrollbar-thumb-transparent hover:scrollbar-thumb-gray-300 scrollbar-track-transparent" v-if="selectedEmail">
          <!-- Email content -->
          <div class="flex-1 p-6">
            <div class="flex items-start mb-6">
              <!-- Avatar -->
              <div class="w-10 h-10 rounded-full bg-gray-200 flex items-center justify-center text-gray-700 mr-4 flex-shrink-0">
                {{ selectedEmail.initials || selectedEmail.sender.charAt(0).toUpperCase() }}
              </div>
              
              <!-- Email header info -->
              <div class="flex-1">
                <div class="flex items-start justify-between">
                  <div>
                    <h2 class="text-lg font-medium">{{ selectedEmail.sender }}</h2>
                    <p class="text-base">{{ selectedEmail.subject }}</p>
                    <p class="text-sm text-gray-600" v-if="selectedEmail.replyTo">
                      Reply-To: {{ selectedEmail.replyTo }}
                    </p>
                  </div>
                  <div class="text-right text-sm text-gray-500">
                    {{ selectedEmail.fullDate || selectedEmail.date }}
                  </div>
                </div>
              </div>
            </div>
            
            <!-- Email body -->
            <div class="prose max-w-none text-base">
              <p>{{ selectedEmail.body }}</p>
              <p v-if="selectedEmail.footer" class="mt-6">{{ selectedEmail.footer }}</p>
            </div>
          </div>
          
          <!-- Reply area using shadcn-vue components -->
          <div class="p-6 pt-0 border-t border-gray-100 mt-4">
            <div class="border border-gray-200 rounded-lg overflow-hidden">
              <Textarea 
                :placeholder="`Reply ${selectedEmail.sender.split(' ')[0]}...`"
                v-model="replyText"
                class="border-0 focus-visible:ring-0 resize-none min-h-[6rem]"
              />
              
              <div class="flex items-center justify-between bg-white px-4 py-2 border-t border-gray-200">
                <div class="flex items-center">
                  <label class="inline-flex items-center cursor-pointer">
                    <div class="relative">
                      <input type="checkbox" value="" class="sr-only peer" v-model="muteThread">
                      <div class="w-10 h-5 bg-gray-200 rounded-full peer peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:start-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-gray-600"></div>
                    </div>
                    <span class="ms-3 text-sm font-medium text-gray-600">Mute this thread</span>
                  </label>
                </div>
                
                <Button 
                  variant="default" 
  class="bg-black hover:bg-gray-800 text-white"
                >
                  Send
                </Button>
              </div>
            </div>
          </div>
        </div>
        
        <div class="flex-1 flex items-center justify-center text-gray-500" v-else>
          Select an email to view
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  import { 
    Archive, 
    ArchiveX,
    Trash2, 
    Clock, 
    CornerUpRightIcon,
    CornerUpLeft,
    ReplyAll,
    MoreVertical,
    Search 
  } from 'lucide-vue-next';
  import { Button } from '@/components/ui/button';
  import { Textarea } from '@/components/ui/textarea';
  
  // Sample emails data
  const emails = ref([
    {
      id: 1,
      sender: 'William Smith',
      initials: 'WS',
      subject: 'Meeting Tomorrow',
      preview: "Hi, let's have a meeting tomorrow to discuss the project. I've been reviewing the project details and have some ideas I'd like to share...",
      body: "Hi, let's have a meeting tomorrow to discuss the project. I've been reviewing the project details and have some ideas I'd like to share. It's crucial that we align on our next steps to ensure the project's success.\n\nPlease come prepared with any questions or insights you may have. Looking forward to our meeting!",
      footer: "Best regards, William",
      date: 'over 1 year ago',
      fullDate: 'Oct 22, 2023, 9:00:00 AM',
      replyTo: 'williamsmith@example.com',
      tags: ['meeting', 'work', 'important']
    },
    {
      id: 2,
      sender: 'Alice Smith',
      initials: 'A',
      subject: 'Re: Project Update',
      preview: "Thank you for the project update. It looks great! I've gone through the report, and the progress is impressive. The team has done a fantasti...",
      body: "Thank you for the project update. It looks great! I've gone through the report, and the progress is impressive. The team has done a fantastic job meeting all the milestones so far.",
      date: 'over 1 year ago',
      tags: ['work', 'important']
    },
    {
      id: 3,
      sender: 'Bob Johnson',
      subject: 'Weekend Plans',
      preview: "Any plans for the weekend? I was thinking of going hiking in the nearby mountains. It's been a while since we had some outdoor fun...",
      body: "Any plans for the weekend? I was thinking of going hiking in the nearby mountains. It's been a while since we had some outdoor fun. Let me know if you're interested in joining!",
      date: 'almost 2 years ago',
      tags: ['personal']
    },
    {
      id: 4,
      sender: 'Emily Davis',
      subject: 'Re: Question about Budget',
      preview: "I have a question about the budget for the upcoming project. It seems like we might need to allocate more resources for the design phase...",
      body: "I have a question about the budget for the upcoming project. It seems like we might need to allocate more resources for the design phase. Can we discuss this soon?",
      date: 'almost 2 years ago',
      tags: ['work', 'budget']
    },
    {
      id: 5,
      sender: 'Michael Wilson',
      subject: 'Important Announcement',
      preview: "I have an important announcement to make during our team meeting. It pertains to a strategic shift in our approach to the upcoming product launch...",
      body: "I have an important announcement to make during our team meeting. It pertains to a strategic shift in our approach to the upcoming product launch. We've received valuable feedback from our beta testers, and I think we need to pivot slightly to address their concerns.",
      date: 'almost 2 years ago',
      tags: ['meeting', 'work', 'important']
    },
    {
      id: 6,
      sender: 'Sarah Brown',
      subject: 'Re: Feedback on Proposal',
      preview: "Thank you for your feedback on the proposal. It looks great! I'm pleased to hear that you found it promising. The team worked hard on it...",
      body: "Thank you for your feedback on the proposal. It looks great! I'm pleased to hear that you found it promising. The team worked hard on it, and I'm glad it met your expectations.",
      date: 'about 2 years ago',
      tags: ['work']
    },
    {
      id: 7,
      sender: 'David Lee',
      subject: 'New Project Idea',
      preview: "I've been thinking about a new project that could help us streamline our operations. I'd like to discuss it with you when you have some time...",
      body: "I've been thinking about a new project that could help us streamline our operations. I'd like to discuss it with you when you have some time. I believe it could save us significant resources in the long run.",
      date: 'about 2 years ago',
      tags: ['work']
    }
  ]);
  
  // Selected email state - default to William Smith's email
  const selectedEmail = ref(emails.value.find(email => email.sender === 'William Smith'));
  
  // Reply functionality
  const replyText = ref('');
  const muteThread = ref(false);
  
  // Function to select an email
  const selectEmail = (email) => {
    selectedEmail.value = email;
    replyText.value = ''; // Clear reply text when switching emails
  };
  </script>
  
  <style>
  .line-clamp-2 {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
  }
  
  /* Custom scrollbar styling */
  .scrollbar-thin {
    scrollbar-width: thin;
  }
  
  .scrollbar-thumb-transparent {
    scrollbar-color: transparent transparent;
  }
  
  .hover\:scrollbar-thumb-gray-300:hover {
    scrollbar-color: #d1d5db transparent;
  }
  
  .scrollbar-track-transparent {
    scrollbar-track-color: transparent;
  }
  
  /* For Webkit browsers (Chrome, Safari) */
  .scrollbar-thin::-webkit-scrollbar {
    width: 4px;
    height: 4px;
  }
  
  .scrollbar-thumb-transparent::-webkit-scrollbar-thumb {
    background-color: transparent;
    border-radius: 20px;
  }
  
  .hover\:scrollbar-thumb-gray-300:hover::-webkit-scrollbar-thumb {
    background-color: #d1d5db;
  }
  
  .scrollbar-track-transparent::-webkit-scrollbar-track {
    background-color: transparent;
  }
  </style>