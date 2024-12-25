<template>
  <div class="flex h-screen bg-gray-100">
    <!-- Sidebar -->
    <aside class="w-1/5 bg-blue-100 p-4">
      <h1 class="text-2xl font-bold text-gray-800 mb-6">ASR Kitchen</h1>
      <nav>
        <ul>
          <li class="mb-4">
            <button
              class="flex items-center text-gray-700 hover:text-blue-500 w-full text-left"
              @click="activeSection = 'OrderList'"
            >
              <span class="mr-2">📋</span> Order List
            </button>
          </li>
          <li class="mb-4">
            <button
              class="flex items-center text-gray-700 hover:text-blue-500 w-full text-left"
              @click="activeSection = 'History'"
            >
              <span class="mr-2">⏰</span> History
            </button>
          </li>
          <li>
            <button
              class="flex items-center text-gray-700 hover:text-blue-500 w-full text-left"
            >
              <span class="mr-2">⚙️</span> Setting
            </button>
          </li>
        </ul>
      </nav>
    </aside>

    <!-- Main Content -->
    <main class="flex-1 p-6 bg-white">
      <!-- Header -->
      <header class="mb-6">
        <h2 class="text-2xl font-bold text-gray-800">Order List</h2>
        <p class="text-sm text-gray-500">{{ currentDate }}</p>
      </header>

      <!-- Order List Section -->
      <section v-if="activeSection === 'OrderList'">
        <!-- Tabs -->
        <div class="flex space-x-4 mb-4">
          <button
            v-for="tab in tabs"
            :key="tab"
            @click="activeTab = tab"
            :class="{
              'bg-blue-500 text-white': activeTab === tab,
              'bg-gray-100 text-gray-700': activeTab !== tab,
            }"
            class="py-2 px-4 rounded shadow"
          >
            {{ tab }}
          </button>
        </div>

        <!-- Order Cards -->
        <div class="grid grid-cols-2 gap-4">
          <div
            v-for="order in filteredOrders"
            :key="order.id"
            class="border rounded p-4 bg-white shadow-sm hover:shadow-lg transition-shadow"
          >
            <div class="flex justify-between items-center mb-2">
              <h3 class="text-lg font-bold">Table {{ order.table }}</h3>
              <span
                :class="{
                  'text-blue-500': order.status === 'New Order',
                  'text-yellow-500': order.status === 'On Cook',
                  'text-green-500': order.status === 'Complete',
                }"
              >
                {{ order.status }}
              </span>
            </div>
            <p class="text-sm text-gray-500 mb-2">
              {{ order.items.length }} items
            </p>
            <ul class="text-sm text-gray-600 mb-4">
              <li
                v-for="item in order.items"
                :key="item"
                class="list-disc ml-4"
              >
                {{ item }}
              </li>
            </ul>
            <button
              v-if="order.status === 'New Order'"
              class="w-full py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
            >
              Accept
            </button>
            <button
              v-if="order.status === 'On Cook'"
              class="w-full py-2 bg-green-500 text-white rounded hover:bg-green-600"
            >
              Complete
            </button>
            <button
              v-if="order.status === 'Complete'"
              class="w-full py-2 bg-gray-300 text-gray-700 rounded hover:bg-gray-400"
            >
              Confirm
            </button>
          </div>
        </div>
      </section>


      <!-- History Section -->
      <section v-else>
        <table class="w-full bg-white shadow border rounded">
          <thead>
            <tr class="bg-gray-100">
              <th class="p-2 text-left">Order ID</th>
              <th class="p-2 text-left">Table</th>
              <th class="p-2 text-left">Time</th>
              <th class="p-2 text-left">Status</th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="history in histories"
              :key="history.id"
              class="hover:bg-gray-50"
            >
              <td class="p-2">{{ history.id }}</td>
              <td class="p-2">{{ history.table }}</td>
              <td class="p-2">{{ history.time }}</td>
              <td class="p-2 text-sm" :class="statusColor(history.status)">
                {{ history.status }}
              </td>
            </tr>
          </tbody>
        </table>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentDate: new Date().toLocaleString(),
      activeSection: "OrderList",
      tabs: ["All", "New Order", "On Cook", "Complete"],
      activeTab: "All",
      orders: [
        {
          id: 1,
          table: "T01",
          status: "New Order",
          items: ["Hamburger x6", "Pizza x2", "Ice Cream x2"],
        },
        {
          id: 2,
          table: "T02",
          status: "On Cook",
          items: ["Pasta x3", "Salad x2"],
        },
        {
          id: 3,
          table: "T03",
          status: "Complete",
          items: ["Soup x1", "Steak x1"],
        },
      ],
      histories: [
        { id: "H01", table: "T01", time: "7:00 AM", status: "Complete" },
        { id: "H02", table: "T02", time: "8:15 AM", status: "Complete" },
      ],
    };
  },
  computed: {
    filteredOrders() {
      if (this.activeTab === "All") return this.orders;
      return this.orders.filter((order) => order.status === this.activeTab);
    },
  },
  methods: {
    statusColor(status) {
      switch (status) {
        case "Complete":
          return "text-green-500";
        case "On Cook":
          return "text-yellow-500";
        default:
          return "text-blue-500";
      }
    },
  },
};
</script>

<style>
@import "./assets/main.css";

</style>
