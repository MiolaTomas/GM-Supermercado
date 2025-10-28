<template>
    <aside class="sidebar">
        <!-- Logo -->
        <a href="/" class="sidebar-logo">
            <span class="logo-main">GM</span>
            <span class="logo-sub">Supermercado</span>
        </a>

        <!-- Menú principal -->
        <ul>
            <li v-for="item in menuItems" :key="item.name">
                <div :class="{ active: activeItem === item.name }" @click="handleItemClick(item.name)"
                    class="menu-item">
                    <i :class="item.icon"></i>
                    <span>{{ item.label }}</span>
                    <i v-if="item.subItems"
                        :class="isStockOpen ? 'fa-solid fa-chevron-down' : 'fa-solid fa-chevron-right'"
                        class="chevron"></i>
                </div>

                <!-- Submenu para Stock -->
                <ul v-if="item.subItems && isStockOpen" class="submenu">
                    <li v-for="subItem in item.subItems" :key="subItem.name"
                        :class="{ active: activeItem === subItem.name }" @click.stop="handleSubItemClick(subItem.name)"
                        class="submenu-item">
                        <i :class="subItem.icon"></i>
                        <span>{{ subItem.label }}</span>
                    </li>
                </ul>
            </li>
        </ul>

        <!-- Bottom fixed items -->
        <div class="sidebar-bottom">
            <div :class="{ active: activeItem === 'Configuracion' }" class="menu-item"
                @click="activeItem = 'Configuracion'">
                <i class="fa-solid fa-gear"></i>
                <span>Configuración</span>
            </div>
            <div :class="{ active: activeItem === 'Usuario' }" class="menu-item" @click="activeItem = 'Usuario'">
                <i class="fa-solid fa-user"></i>
                <span>Usuario</span>
            </div>
        </div>
    </aside>
</template>

<script setup>
import { ref } from 'vue'

const activeItem = ref('Productos')
const isStockOpen = ref(false)

const menuItems = [
    { name: 'Productos', label: 'Productos', icon: 'fa-solid fa-box' },
    {
        name: 'Stock',
        label: 'Stock',
        icon: 'fa-solid fa-cubes',
        subItems: [
            { name: 'Existencias', label: 'Existencias', icon: 'fa-solid fa-list' },
            { name: 'GestionarDespachos', label: 'Gestionar despachos', icon: 'fa-solid fa-truck' }
        ]
    },
    { name: 'Proveedores', label: 'Proveedores', icon: 'fa-solid fa-truck' },
    { name: 'Compras', label: 'Compras', icon: 'fa-solid fa-cart-shopping' },
    { name: 'Informe', label: 'Informe', icon: 'fa-solid fa-chart-line' },
]

function handleItemClick(name) {
    if (name === 'Stock') {
        isStockOpen.value = !isStockOpen.value
    }
    activeItem.value = name
}

function handleSubItemClick(name) {
    activeItem.value = name
}
</script>

<style scoped>
.sidebar {
    width: 200px;
    height: 100vh;
    display: flex;
    flex-direction: column;
    background: var(--sidebar-bg);
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
    list-style: none;
}

.menu-item {
    display: flex;
    align-items: center;
    gap: 1em;
    padding: 12px 20px;
    cursor: pointer;
    transition: background 0.3s, color 0.3s;
}

.menu-item i {
    color: var(--sidebar-base-text);
    font-size: 1.2rem;
}

.menu-item span {
    color: var(--sidebar-base-text);
    font-weight: 500;
}

.menu-item .chevron {
    margin-left: auto;
    font-size: 0.8rem;
}

.menu-item:hover {
    background-color: rgba(231, 76, 60, 0.1);
}

.menu-item.active {
    background-color: #e74c3c;
}

.menu-item.active i,
.menu-item.active span {
    color: white;
}

.submenu {
    background-color: #f5f5f5;
    padding: 0;
    margin: 0;
}

.submenu-item {
    display: flex;
    align-items: center;
    gap: 0.8em;
    padding: 10px 20px 10px 50px;
    cursor: pointer;
    transition: background 0.3s, color 0.3s;
}

.submenu-item i {
    color: var(--sidebar-base-text);
    font-size: 1rem;
}

.submenu-item span {
    color: var(--sidebar-base-text);
    font-size: 0.9rem;
    font-weight: 500;
}

.submenu-item:hover {
    background-color: rgba(231, 76, 60, 0.1);
}

.submenu-item.active {
    background-color: #e74c3c;
}

.submenu-item.active i,
.submenu-item.active span {
    color: white;
}

.sidebar-bottom {
    margin-top: auto;
    /* empuja hacia abajo */
    display: flex;
    flex-direction: column;
}

.sidebar-bottom .menu-item {
    padding: 12px 20px;
}
</style>
