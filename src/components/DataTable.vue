
<template>
    <div>
        <div class="controls">
            <label for="pageSize">Users per page:</label>
            <select id="pageSize" v-model="pageSize">
                <option value="5">5</option>
                <option value="10">10</option>
                <option value="15">15</option>
                <option value="20">All</option>
            </select>
        </div>
        <table>
            <thead>
                <tr>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Email</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="user in paginatedUsers" :key="user.id">
                    <td>{{ user.id }}</td>
                    <td>{{ user.name }}</td>
                    <td>{{ user.email }}</td>
                </tr>
            </tbody>
        </table>
        <div class="pagination">
            <button @click="prevPage" :disabled="currentPage <= 1">Prev</button>
            <span>Page {{ currentPage }} of {{ totalPages }}</span>
            <button @click="nextPage" :disabled="currentPage >= totalPages">Next</button>
        </div>
    </div>
</template>
<style>
/* Your styles here plus the below for the controls */
.controls {
    margin-bottom: 1rem;
}

.pagination {
    display: flex;
    justify-content: space-between;
    margin-top: 10px;
}

.pagination button:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}
</style>
  
<script setup>
import { ref, computed, watch } from 'vue';

const users = ref([
    { id: 1, name: 'Parsa Saeednia', email: 'parsa@example.com' },
    { id: 2, name: 'Jane Smith', email: 'jane.smith@example.com' },
    { id: 3, name: 'William Johnson', email: 'william.j@example.com' },
    { id: 4, name: 'Olivia Brown', email: 'olivia.b@example.com' },
    { id: 5, name: 'Michael Garcia', email: 'michael.g@example.com' },
    { id: 6, name: 'Emily Davis', email: 'emily.d@example.com' },
    { id: 7, name: 'Alexander Martinez', email: 'alexander.m@example.com' },
    { id: 8, name: 'Sophia Wilson', email: 'sophia.w@example.com' },
    { id: 9, name: 'Daniel Taylor', email: 'daniel.t@example.com' },
    { id: 10, name: 'Isabella Thomas', email: 'isabella.t@example.com' },
    { id: 11, name: 'Matthew Moore', email: 'matthew.m@example.com' },
    { id: 12, name: 'Amelia Anderson', email: 'amelia.a@example.com' },
    { id: 13, name: 'Lucas Jackson', email: 'lucas.j@example.com' },
    { id: 14, name: 'Emma Harris', email: 'emma.h@example.com' },
    { id: 15, name: 'Noah White', email: 'noah.w@example.com' },
    { id: 16, name: 'Avery Lee', email: 'avery.l@example.com' },
    { id: 17, name: 'Charlotte Martin', email: 'charlotte.m@example.com' },
    { id: 18, name: 'Logan Perez', email: 'logan.p@example.com' },
    { id: 19, name: 'Mia Thompson', email: 'mia.t@example.com' },
    { id: 20, name: 'James Hall', email: 'james.h@example.com' },
]);
const pageSize = ref(5); // Default page size
const currentPage = ref(1);

const totalPages = computed(() => {
    return pageSize.value === 'All'
        ? 1
        : Math.ceil(users.value.length / Number(pageSize.value));
});

watch(pageSize, (newSize, oldSize) => {
    // If 'All' isn't selected and the new page size divides into the current index
    // we're staying on the same page, otherwise reset to first page.
    if (newSize !== 'All') {
        const newIndex = Math.floor(
            (currentPage.value - 1) * oldSize / newSize) + 1;
        currentPage.value = Math.min(newIndex, totalPages.value);
    } else {
        currentPage.value = 1; // Reset to first page when 'All' is selected
    }
});

const paginatedUsers = computed(() => {
    if (pageSize.value === 'All') {
        return users.value; // Return all users if 'All' is selected
    }
    const startIndex = (currentPage.value - 1) * Number(pageSize.value);
    return users.value.slice(startIndex, startIndex + Number(pageSize.value));
});

const nextPage = () => {
    if (currentPage.value < totalPages.value) currentPage.value++;
};

const prevPage = () => {
    if (currentPage.value > 1) currentPage.value--;
};
</script>
  

  