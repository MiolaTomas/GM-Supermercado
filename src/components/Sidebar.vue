<template>
    <aside class="sidebar">
        <a href="/" class="sidebar-logo">
            <span class="logo-main">GM</span>
            <span class="logo-sub">Supermercado</span>
        </a>

        <ul>
            <li v-for="item in menuItems" :key="item.name" :class="{ active: activeItem === item.name }"
                @click="setActive(item.name)">
                <i :class="item.icon"></i>
                <span>{{ item.label }}</span>
            </li>
        </ul>
    </aside>
</template>

<script setup>
import { ref } from 'vue'

const activeItem = ref('Productos')

const menuItems = [
    { name: 'Productos', label: 'Productos', icon: 'fa-solid fa-box' },
    { name: 'Stock', label: 'Stock', icon: 'fa-solid fa-cubes' },
    { name: 'Proveedores', label: 'Proveedores', icon: 'fa-solid fa-truck' },
]

function setActive(name) {
    activeItem.value = name
}
</script>

<style scoped>
.sidebar {
    width: 200px;
    height: 100vh;
    background: var(--sidebar-bg);
    /* white background */
    color: black;
    transition: transform 0.3s, width 0.3s;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.sidebar-logo {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-decoration: none;
    padding: 1em;
}

.sidebar-logo span:nth-child(1) {
    color: var(--color-primary);
    font-size: 3em;
    font-weight: bold;
}

.sidebar-logo span:nth-child(2) {
    color: black;
}

ul {
    padding: 0;
    margin: 0;
    list-style: none;
}

li {
    display: flex;
    align-items: center;
    gap: 1em;
    padding: 12px 20px;
    cursor: pointer;
    transition: background 0.3s, color 0.3s;
}

/* Default state */
li i {
    color: var(--sidebar-base-text);
    /* icon black */
    font-size: 1.2rem;
}

li span {
    color: var(--sidebar-base-text);
    /* text black */
    font-weight: 500;
}

/* Hover */
li:hover {
    background-color: rgba(231, 76, 60, 0.1);
}

/* Active state */
li.active {
    background-color: #e74c3c;
}

li.active i,
li.active span {
    color: white;
    /* both icon and text turn white */
}

/* Responsive */
@media (max-width: 768px) {
    .sidebar {
        width: 80px;
        display: flex;
        flex-direction: column;
        /* stack logo and menu vertically */
        align-items: center;
    }

    .sidebar-logo {
        padding: 0.5em;
    }

    .sidebar-logo span.logo-main {
        font-size: 2em;
        padding: .5em;
    }

    .sidebar-logo span.logo-sub {
        display: none;
        /* hide subtitle */
    }

    ul {
        width: 100%;
        display: flex;
        flex-direction: column;
        padding: 0;
        margin: 0;
    }

    li {
        justify-content: center;
        padding: 12px 0;
        gap: 0;
    }

    li span {
        display: none;
        /* hide menu text */
    }
}
</style>
