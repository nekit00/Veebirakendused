<script setup>
defineProps(['items', 'title']);

const labels = {3: 'Kõrge', 2: 'Keskmine', 1: 'Madal'};

// Kõrge = punane, keskmine = kollane, madal = roheline
function colorClass(importance) {
    if (importance >= 3) return 'is-danger';
    if (importance === 2) return 'is-warning';
    return 'is-success';
}
</script>
<template>
    <h1>{{ title }}</h1>
    <ul class="ml-0" style="list-style: none">
        <li
            v-for="item in items"
            :key="item.id"
            class="notification is-light py-2 px-4 mb-2"
            :class="colorClass(item.importance)"
        >
            <label class="checkbox">
                <input type="checkbox" v-model="item.isDone">
                {{ item.text }}
            </label>
            <span class="tag ml-2" :class="colorClass(item.importance)">{{ labels[item.importance] }}</span>
        </li>
    </ul>
</template>
