<template>
    <div class="container">

        <div class="crud-buttons">
            <button class="btn-add" @click="addItem">
                <i class="fa-solid fa-plus"></i>
                <span>Añadir</span>
            </button>
            <button class="btn-add" @click="editItem">
                <i class="fa-solid fa-plus"></i>
                <span>Editar</span>
            </button>
            <button class="btn-add" @click="deleteItem">
                <i class="fa-solid fa-plus"></i>
                <span>Eliminar</span>
            </button>
        </div>

        <div class="navigation-buttons">
            <button>&lt;&lt;</button>
            <button>&lt;</button>
            <button>&gt;</button>
            <button>&gt;&gt;</button>
        </div>
    </div>

</template>

<style scoped>
.container {
    background-color: red;
    display: flex;
    padding: 1em;
    justify-content: space-between;
}
</style>