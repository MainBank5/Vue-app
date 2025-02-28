<script setup>
import { ref, computed } from "vue";
import { 
  ChevronDown, 
  ChevronRight, 
  ChevronLeft,
  ChevronUp,
  LayoutDashboard,
  BarChart,
  Calendar, 
  Settings as SettingsIcon, 
  Package,
  FileText,
  File,
  Truck,
  Wrench,
  Plus,
  Building,
  Menu
} from "lucide-vue-next";
import { Button } from "@/components/ui/button";
import { Sheet, SheetContent, SheetTrigger } from "@/components/ui/sheet";
import { Collapsible, CollapsibleContent, CollapsibleTrigger } from "@/components/ui/collapsible";
import { Popover, PopoverContent, PopoverTrigger } from "@/components/ui/popover";
import { Tooltip, TooltipContent, TooltipProvider, TooltipTrigger } from "@/components/ui/tooltip";
import mainMail from "./mail/mainMail.vue";

// Track collapsed state
const collapsed = ref(false);
const expandedItems = ref({
  Overview: true,
  Dashboard: false,
  Calendar: false,
  FieldService: false,
  PickupB2C: false,
  Reports: false,
  Documents: false,
  Settings: false
});

const sidebarWidth = computed(() => collapsed.value ? 'w-16' : 'w-64');

// Toggle expanded state for each menu item
const toggleExpand = (item) => {
  if (collapsed.value) return;
  expandedItems.value[item] = !expandedItems.value[item];
};

// User data
const user = {
  name: "Username",
  email: "m@example.com"
};

// Current page
const currentPage = ref("Analytics");

// Track team dropdown open state
const teamDropdownOpen = ref(false);

// Teams data
const teams = [
  { id: 1, name: "Acme Inc", type: "Enterprise", icon: Package, shortcode: "#1" },
  { id: 2, name: "Acme Corp.", type: "Business", icon: BarChart, shortcode: "#2" },
  { id: 3, name: "Evil Corp.", type: "Enterprise", icon: Building, shortcode: "#3" },
];

const activeTeam = ref(teams[0]);

// Menu items structure with icons and subitems
const menuItems = [
  {
    id: 'Overview',
    label: 'Overview',
    icon: LayoutDashboard,
    subitems: [
      { id: 'sub1', label: 'Summary', icon: BarChart },
      { id: 'sub2', label: 'Analytics', icon: BarChart },
      { id: 'sub3', label: 'Projects', icon: File }
    ]
  },
  {
    id: 'Dashboard',
    label: 'Dashboard',
    icon: BarChart,
    subitems: [
      { id: 'sub1', label: 'Performance', icon: BarChart },
      { id: 'sub2', label: 'Metrics', icon: BarChart }
    ]
  },
  {
    id: 'Calendar',
    label: 'Calendar',
    icon: Calendar,
    subitems: [
      { id: 'sub1', label: 'Schedule', icon: Calendar },
      { id: 'sub2', label: 'Events', icon: Calendar }
    ]
  },
  {
    id: 'FieldService',
    label: 'Field Service',
    icon: Wrench,
    subitems: [
      { id: 'sub1', label: 'Assignments', icon: Wrench },
      { id: 'sub2', label: 'Tickets', icon: FileText }
    ]
  },
  {
    id: 'PickupB2C',
    label: 'Pickup B2C',
    icon: Truck,
    subitems: [
      { id: 'sub1', label: 'Orders', icon: FileText },
      { id: 'sub2', label: 'Logistics', icon: Truck }
    ]
  },
  {
    id: 'Reports',
    label: 'Reports',
    icon: FileText,
    subitems: [
      { id: 'sub1', label: 'Weekly', icon: FileText },
      { id: 'sub2', label: 'Monthly', icon: FileText },
      { id: 'sub3', label: 'Annual', icon: FileText }
    ]
  },
  {
    id: 'Documents',
    label: 'Documents',
    icon: File,
    subitems: [
      { id: 'sub1', label: 'Contracts', icon: File },
      { id: 'sub2', label: 'Invoices', icon: File }
    ]
  },
  {
    id: 'Settings',
    label: 'Settings',
    icon: SettingsIcon,
    subitems: [
      { id: 'sub1', label: 'Account', icon: SettingsIcon },
      { id: 'sub2', label: 'Preferences', icon: SettingsIcon },
      { id: 'sub3', label: 'Security', icon: SettingsIcon }
    ]
  }
];

const selectTeam = (team) => {
  activeTeam.value = team;
  teamDropdownOpen.value = false;
};

// Toggle sidebar collapse state
const toggleSidebar = () => {
  collapsed.value = !collapsed.value;
};

// Toggle team dropdown
const toggleTeamDropdown = () => {
  teamDropdownOpen.value = !teamDropdownOpen.value;
};
</script>

<template>
  <div class="flex h-screen">
    <!-- Main sidebar -->
    <aside 
      :class="[
        'border-r transition-all duration-300 flex flex-col h-screen bg-gray-100/95 dark:bg-gray-900 relative',
        sidebarWidth
      ]"
    >
      <!-- Top section with logo and company info or just logo when collapsed -->
      <div class="border-b relative"> 
        <!-- Use Popover for the team selector when not collapsed -->
        <Popover v-if="!collapsed">
          <PopoverTrigger class="w-full">
            <div class="p-4 flex items-center cursor-pointer hover:bg-gray-50 dark:hover:bg-gray-800 w-full">
              <div class="bg-black text-white p-2 rounded-md flex items-center justify-center">
                <Package class="w-5 h-5" />
              </div>
              <div class="p-2 text-left">
                <h3 class="font-semibold text-base text-gray-600">{{ activeTeam.name }}</h3>
                <p class="text-muted-foreground text-xs">{{ activeTeam.type }}</p>
              </div>
              <div class="ml-auto flex flex-col -space-y-1">
                <ChevronUp class="h-3 w-3 text-gray-500" />
                <ChevronDown class="h-3 w-3 text-gray-500" />
              </div>
            </div>
          </PopoverTrigger>
          <PopoverContent side="right" class="p-0 w-64 mt-2">
            <div class="p-2">
              <h3 class="text-sm font-medium px-3 py-2">Teams</h3>
              <div class="space-y-1">
                <Button 
                  v-for="team in teams" 
                  :key="team.id" 
                  variant="ghost" 
                  class="w-full justify-between px-3" 
                  :class="{ 'bg-accent': team.id === activeTeam.id }"
                  @click="selectTeam(team)"
                >
                  <div class="flex items-center">
                    <div class="bg-black text-white p-1 rounded-md flex items-center justify-center mr-2">
                      <component :is="team.icon" class="w-4 h-4" />
                    </div>
                    <span>{{ team.name }}</span>
                  </div>
                  <span class="text-xs text-muted-foreground">{{ team.shortcode }}</span>
                </Button>
                
                <Button variant="ghost" class="w-full justify-start px-3">
                  <div class="flex items-center">
                    <div class="border border-gray-200 dark:border-gray-700 p-1 rounded-md flex items-center justify-center mr-2">
                      <Plus class="w-4 h-4" />
                    </div>
                    <span>Add team</span>
                  </div>
                </Button>
              </div>
            </div>
          </PopoverContent>
        </Popover>

        <!-- Collapsed state - just logo -->
        <div v-if="collapsed" class="p-4 flex justify-center items-center w-full">
          <div class="bg-black text-white p-2 rounded-md flex items-center justify-center">
            <Package class="w-5 h-5" />
          </div>
        </div>
      </div>

      <!-- Sidebar toggle button - always visible, positioned at the edge -->
      <Button 
        variant="ghost" 
        size="icon"
        class="absolute -right-3 top-14 h-6 w-6 rounded-full border shadow-sm bg-white z-10"
        @click="toggleSidebar"
      >
        <ChevronLeft v-if="!collapsed" class="h-3 w-3" />
        <ChevronRight v-else class="h-3 w-3" />
      </Button>

      <!-- Navigation section -->
      <div class="flex-1 overflow-y-auto">
        <div class="p-2">
          <div class="px-2 py-1.5">
            <p class="text-xs text-muted-foreground uppercase" v-if="!collapsed">Platform</p>
            <p class="text-xs text-center text-muted-foreground" v-else>•••</p>
          </div>

          <!-- Menu items with dropdowns or tooltips when collapsed -->
          <div v-for="item in menuItems" :key="item.id">
            <template v-if="collapsed">
              <TooltipProvider>
                <Tooltip>
                  <TooltipTrigger asChild>
                    <Button 
                      variant="ghost" 
                      class="w-full justify-center mb-1 px-0 h-10"
                    >
                      <component :is="item.icon" class="h-5 w-5" />
                    </Button>
                  </TooltipTrigger>
                  <TooltipContent side="right" class="bg-gray-800 text-white">
                    {{ item.label }}
                  </TooltipContent>
                </Tooltip>
              </TooltipProvider>
            </template>
            
            <Collapsible v-else :open="expandedItems[item.id]" class="w-full">
              <CollapsibleTrigger asChild>
                <Button 
                  variant="ghost" 
                  class="w-full justify-between mb-1 px-3"
                  @click="toggleExpand(item.id)"
                >
                  <div class="flex items-center">
                    <component :is="item.icon" class="h-5 w-5 mr-2" />
                    <span>{{ item.label }}</span>
                  </div>
                  <!-- Changed from ChevronDown to conditionally show ChevronRight or ChevronDown -->
                  <ChevronDown v-if="expandedItems[item.id]" class="h-4 w-4" />
                  <ChevronRight v-else class="h-4 w-4" />
                </Button>
              </CollapsibleTrigger>
              <CollapsibleContent class="pl-9 space-y-1 relative before:absolute before:left-4 before:top-0 before:h-full before:w-px before:bg-gray-200 dark:before:bg-gray-700">
                <Button 
                  v-for="subitem in item.subitems" 
                  :key="subitem.id" 
                  variant="ghost" 
                  class="w-full justify-start h-8" 
                  size="sm"
                >
                  <component :is="subitem.icon" class="h-4 w-4 mr-2" />
                  <span>{{ subitem.label }}</span>
                </Button>
              </CollapsibleContent>
            </Collapsible>
          </div>
        </div>
      </div>

      <!-- User profile section - simplified in collapsed state -->
      <div class="border-t p-4">
        <div class="flex items-center gap-3">
          <div class="w-8 h-8 rounded-md bg-purple-500 flex items-center justify-center text-white">
            {{ user.name.charAt(0).toUpperCase() }}
          </div>
          <div v-if="!collapsed">
            <p class="text-sm font-medium">{{ user.name }}</p>
            <p class="text-xs text-muted-foreground">{{ user.email }}</p>
          </div>
          <Button v-if="!collapsed" variant="ghost" size="icon" class="ml-auto">
            <div class="git  flex flex-col ">
                <ChevronUp class="h-3 w-3 text-gray-500" />
                <ChevronDown class="h-3 w-3 text-gray-500" />
              </div>
          </Button>
        </div>
      </div>
    </aside>


    <!-- Mobile sidebar (Sheet component) -->
    <Sheet>
      <SheetTrigger asChild>
        <Button variant="outline" class="lg:hidden fixed bottom-4 right-4">
          <Menu class="w-5 h-5" />
        </Button>
      </SheetTrigger>
      <SheetContent side="left" class="p-0">
        <div class="h-full">
          <!-- Mobile team selector with Popover -->
          <Popover>
            <PopoverTrigger class="w-full">
              <div class="p-4 flex items-center cursor-pointer hover:bg-gray-50 dark:hover:bg-gray-800 w-full">
                <div class="bg-black text-white p-2 rounded-md flex items-center justify-center mr-2">
                  <Package class="w-5 h-5" />
                </div>
                <div class="flex-1">
                  <h3 class="font-semibold text-base">{{ activeTeam.name }}</h3>
                  <p class="text-muted-foreground text-xs">{{ activeTeam.type }}</p>
                </div>
                <div class="ml-auto flex flex-col -space-y-1">
                  <ChevronUp class="h-3 w-3 text-gray-500" />
                  <ChevronDown class="h-3 w-3 text-gray-500" />
                </div>
              </div>
            </PopoverTrigger>
            <PopoverContent class="p-0 w-full mt-2">
              <div class="p-2">
                <h3 class="text-sm font-medium px-3 py-2">Teams</h3>
                <div class="space-y-1">
                  <Button 
                    v-for="team in teams" 
                    :key="team.id" 
                    variant="ghost" 
                    class="w-full justify-between px-3" 
                    :class="{ 'bg-accent': team.id === activeTeam.id }"
                    @click="selectTeam(team)"
                  >
                    <div class="flex items-center">
                      <div class="bg-black text-white p-1 rounded-md flex items-center justify-center mr-2">
                        <component :is="team.icon" class="w-4 h-4" />
                      </div>
                      <span>{{ team.name }}</span>
                    </div>
                    <span class="text-xs text-muted-foreground">{{ team.shortcode }}</span>
                  </Button>
                  
                  <Button variant="ghost" class="w-full justify-start px-3">
                    <div class="flex items-center">
                      <div class="border border-gray-200 dark:border-gray-700 p-1 rounded-md flex items-center justify-center mr-2">
                        <Plus class="w-4 h-4" />
                      </div>
                      <span>Add team</span>
                    </div>
                  </Button>
                </div>
              </div>
            </PopoverContent>
          </Popover>

          <div class="p-2 overflow-y-auto">
            <div class="px-2 py-1.5">
              <p class="text-xs text-muted-foreground uppercase">Platform</p>
            </div>

            <!-- Mobile menu items with updated chevrons -->
            <div v-for="item in menuItems" :key="item.id">
              <Collapsible :open="expandedItems[item.id]" class="w-full">
                <CollapsibleTrigger asChild>
                  <Button 
                    variant="ghost" 
                    class="w-full justify-between mb-1 px-3"
                    @click="toggleExpand(item.id)"
                  >
                    <div class="flex items-center">
                      <component :is="item.icon" class="h-5 w-5 mr-2" />
                      <span>{{ item.label }}</span>
                    </div>
                    <!-- Updated chevrons in mobile view too -->
                    <ChevronDown v-if="expandedItems[item.id]" class="h-4 w-4" />
                    <ChevronRight v-else class="h-4 w-4" />
                  </Button>
                </CollapsibleTrigger>
                <CollapsibleContent class="pl-9 space-y-1 relative before:absolute before:left-4 before:top-0 before:h-full before:w-px before:bg-gray-200 dark:before:bg-gray-700">
                  <Button 
                    v-for="subitem in item.subitems" 
                    :key="subitem.id" 
                    variant="ghost" 
                    class="w-full justify-start h-8" 
                    size="sm"
                  >
                    <component :is="subitem.icon" class="h-4 w-4 mr-2" />
                    <span>{{ subitem.label }}</span>
                  </Button>
                </CollapsibleContent>
              </Collapsible>
            </div>
          </div>

          <!-- Mobile user profile -->
          <div class="border-t p-4 mt-auto">
            <div class="flex items-center gap-3">
              <div class="w-8 h-8 rounded-md bg-purple-500 flex items-center justify-center text-white">
                {{ user.name.charAt(0).toUpperCase() }}
              </div>
              <div>
                <p class="text-sm font-medium">{{ user.name }}</p>
                <p class="text-xs text-muted-foreground">{{ user.email }}</p>
              </div>
              <Button variant="ghost" size="icon" class="ml-auto">
                <ChevronUp class="h-4 w-4" />
                <ChevronDown class="h-4 w-4" />
                
              </Button>
            </div>
          </div>
        </div>
      </SheetContent>
    </Sheet>
  </div>
</template>