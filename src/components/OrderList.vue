<template>
  <div>
    <table class="min-w-full bg-white border border-gray-200 rounded-lg shadow-md">
      <thead class="bg-gray-50">
        <tr>
          <th class="p-2 text-left">Order ID</th>
          <th class="p-2 text-left">User</th>
          <th class="p-2 text-left">Items</th>
          <th class="p-2 text-left">Status</th>
          <th class="p-2 text-left">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="order in orders" :key="order.id">
          <td class="p-2">{{ order.id }}</td>
          <td class="p-2">{{ order.user }}</td>
          <td class="p-2">{{ order.items.join(', ') }}</td>
          <td class="p-2">
            <span
              :class="{
                'text-red-500': order.status === 'New',
                'text-yellow-500': order.status === 'On Cook',
                'text-green-500': order.status === 'Complete',
              }"
            >
              {{ order.status }}
            </span>
          </td>
          <td class="p-2">
            <button
              v-if="order.status === 'New'"
              @click="updateStatus(order.id, 'On Cook')"
              class="bg-yellow-500 text-white px-4 py-2 rounded-md"
            >
              Start Cooking
            </button>
            <button
              v-if="order.status === 'On Cook'"
              @click="updateStatus(order.id, 'Complete')"
              class="bg-green-500 text-white px-4 py-2 rounded-md"
            >
              Mark as Complete
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    orders: Array,
  },
  methods: {
    updateStatus(orderId, newStatus) {
      this.$emit('update-status', orderId, newStatus);
    },
  },
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
}
th, td {
  padding: 0.75rem;
}
</style>
