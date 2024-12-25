<template>
  <div>
    <table class="min-w-full bg-white border border-gray-200 rounded-lg shadow-md">
      <thead class="bg-gray-50">
        <tr>
          <th class="p-2 text-left">Order ID</th>
          <th class="p-2 text-left">User</th>
          <th class="p-2 text-left">Items</th>
          <th class="p-2 text-left">Status</th>
          <th class="p-2 text-left">Notes</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="order in orders" :key="order.id">
          <td class="p-2">{{ order.id }}</td>
          <td class="p-2">{{ order.user }}</td>
          <td class="p-2">{{ order.items.join(', ') }}</td>
          <td class="p-2">
            <span class="text-green-500">{{ order.status }}</span>
          </td>
          <td class="p-2">{{ order.notes }}</td>
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
