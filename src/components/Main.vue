<template>
    <div class="main-container">
        <!-- Barra de botones CRUD y navegación -->
        <div class="container">
            <div class="crud-buttons">
                <button class="btn-add btn-new" @click="addItem">
                    <i class="fa-solid fa-plus"></i>
                    <span>Nuevo</span>
                </button>
                <button class="btn-add btn-edit" @click="editItem">
                    <i class="fa-solid fa-pen"></i>
                    <span>Modificar</span>
                </button>
                <button class="btn-add btn-delete" @click="deleteItem">
                    <i class="fa-solid fa-trash"></i>
                    <span>Eliminar</span>
                </button>
            </div>

            <div class="navigation-buttons">
                <button class="nav-btn">&lt;&lt;</button>
                <button class="nav-btn">&lt;</button>
                <button class="nav-btn">&gt;</button>
                <button class="nav-btn">&gt;&gt;</button>
            </div>
        </div>

        <!-- Formulario -->
        <div class="form-container">
            <div class="form-row">
                <!-- Columna izquierda -->
                <div class="form-column">
                    <!-- Nombre del Producto -->
                    <div class="form-group">
                        <label>Nombre del Producto <span class="required">*</span></label>
                        <input type="text" placeholder="Ingrese nombre de producto" v-model="form.nombreProducto"
                            :disabled="!formEnabled" />
                    </div>

                    <!-- Precio Unitario -->
                    <div class="form-group">
                        <label>Precio Unitario <span class="required">*</span></label>
                        <input type="text" placeholder="Ingrese el precio de producto" v-model="form.precioUnitario"
                            :disabled="!formEnabled" />
                    </div>

                    <!-- Punto crítico de reposición -->
                    <div class="form-group">
                        <label>Punto crítico de reposición <span class="required">*</span></label>
                        <input type="text" placeholder="Ingrese el número de pun..." v-model="form.puntoCritico"
                            :disabled="!formEnabled" />
                    </div>

                    <!-- Existencias -->
                    <div class="form-group">
                        <label>Existencias</label>
                        <input type="text" placeholder="Ingrese la cantidad de existencias" v-model="form.existencias"
                            :disabled="!formEnabled" />
                    </div>

                    <!-- Producto Generico -->
                    <div class="form-group">
                        <label>Producto Generico <span class="required">*</span></label>
                        <div class="input-with-button">
                            <select v-model="form.productoGenerico" :disabled="!formEnabled">
                                <option value="">Seleccione un genérico</option>
                                <option v-for="(g, idx) in productoGenericos" :key="idx" :value="g">{{ g }}</option>
                            </select>
                            <!-- NOTA: el botón para abrir el modal está AVAILABLE siempre -->
                            <button class="btn-add-field" @click="openModal" type="button">
                                <i class="fa-solid fa-plus"></i>
                            </button>
                        </div>
                    </div>

                    <!-- Rotación -->
                    <div class="form-group">
                        <label>Rotación <span class="required">*</span></label>
                        <div class="radio-group">
                            <label class="radio-label">
                                <input type="radio" name="rotacion" value="A" v-model="form.rotacion"
                                    :disabled="!formEnabled" />
                                <span>A</span>
                            </label>
                            <label class="radio-label">
                                <input type="radio" name="rotacion" value="B" v-model="form.rotacion"
                                    :disabled="!formEnabled" />
                                <span>B</span>
                            </label>
                            <label class="radio-label">
                                <input type="radio" name="rotacion" value="C" v-model="form.rotacion"
                                    :disabled="!formEnabled" />
                                <span>C</span>
                            </label>
                        </div>
                    </div>
                </div>

                <!-- Columna derecha -->
                <div class="form-column">
                    <!-- ID -->
                    <div class="form-group">
                        <label>ID</label>
                        <input type="text" :value="form.id || editingId || '—'" disabled class="disabled-input" />
                    </div>

                    <!-- Código de Barra -->
                    <div class="form-group">
                        <label>Código de Barra</label>
                        <input type="text" value="9999999999999" disabled class="disabled-input" />
                    </div>

                    <!-- Imagen -->
                    <div class="form-group">
                        <label>Imagen</label>
                        <div class="image-upload" :class="{ disabled: !formEnabled }">
                            <i class="fa-solid fa-image"></i>
                            <span>Subir una imagen</span>
                        </div>
                    </div>

                    <!-- Marca -->
                    <div class="form-group">
                        <label>Marca <span class="required">*</span></label>
                        <div class="input-with-button">
                            <select v-model="form.marca" :disabled="!formEnabled">
                                <option value="">Seleccione una marca</option>
                                <option v-for="(m, idx) in marcas" :key="idx" :value="m">{{ m }}</option>
                            </select>
                            <!-- permitimos abrir modal de marca siempre -->
                            <button class="btn-add-field" @click="openMarcaModal" type="button">
                                <i class="fa-solid fa-plus"></i>
                            </button>
                        </div>
                    </div>

                    <!-- Presentación (DROPDOWN + BUTTON) -->
                    <div class="form-group">
                        <label>Presentación <span class="required">*</span></label>
                        <div class="input-with-button">
                            <select v-model="form.presentacion" :disabled="!formEnabled">
                                <option value="">Seleccione una presentación</option>
                                <option v-for="(p, idx) in presentaciones" :key="idx" :value="p">{{ p }}</option>
                            </select>
                            <!-- permitimos abrir modal de presentación siempre -->
                            <button class="btn-add-field" @click="openPresentacionModal" type="button">
                                <i class="fa-solid fa-plus"></i>
                            </button>
                        </div>
                    </div>

                    <!-- Proveedor -->
                    <div class="form-group">
                        <label>Proveedor <span class="required">*</span></label>
                        <div class="input-with-button">
                            <select v-model="form.proveedor" :disabled="!formEnabled">
                                <option value="">Seleccione un proveedor</option>
                                <option v-for="(p, idx) in proveedores" :key="idx" :value="p">{{ p }}</option>
                            </select>
                            <!-- permitimos abrir modal de proveedor siempre -->
                            <button class="btn-add-field" @click="openProveedorModal" type="button">
                                <i class="fa-solid fa-plus"></i>
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Botones del formulario -->
            <div class="form-actions">
                <button class="btn-cancel" type="button" @click="cancelForm">Cancelar</button>
                <button class="btn-submit" type="button" @click="submitForm" :disabled="!formEnabled">
                    Cargar
                </button>
            </div>
        </div>

        <!-- Modal Producto Genérico -->
        <div v-if="showModal" class="modal-overlay" @click="closeModal">
            <div class="modal-content" @click.stop>
                <div class="modal-header">
                    <div class="breadcrumb">
                        <span>Inicio</span>
                        <span class="separator">/</span>
                        <span>Producto</span>
                        <span class="separator">/</span>
                        <span class="active">Producto Genérico</span>
                    </div>
                    <button class="btn-close" @click="closeModal">
                        <i class="fa-solid fa-xmark"></i>
                    </button>
                </div>

                <h2 class="modal-title">Genérico</h2>

                <div class="modal-form">
                    <div class="modal-form-row">
                        <div class="form-group">
                            <label>Nombre <span class="required">*</span></label>
                            <input type="text" placeholder="Ingrese Nombre" v-model="modalForm.nombre" />
                        </div>
                        <div class="form-group">
                            <label>ID Producto Genérico <span class="required">*</span></label>
                            <input type="text" value="999999" disabled class="disabled-input" />
                        </div>
                    </div>

                    <!-- <div class="modal-form-row">
                        <div class="form-group">
                            <label>Nombre <span class="required">*</span></label>
                            <input type="text" placeholder="Ingrese Nombre" v-model="modalForm.nombre" />
                        </div>
                    </div> -->

                    <div class="modal-form-row">
                        <div class="form-group">
                            <label>Categoría <span class="required">*</span></label>
                            <div class="input-with-button">
                                <select v-model="modalForm.categoria">
                                    <option value="">Ingrese Categoría</option>
                                    <option value="alimentos">Alimentos</option>
                                    <option value="bebidas">Bebidas</option>
                                </select>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="modal-actions">
                    <button class="btn-cancel" @click="closeModal">Cancelar</button>
                    <button class="btn-submit" @click="saveModal">Cargar</button>
                </div>
            </div>
        </div>

        <!-- Modal Marca -->
        <div v-if="showMarcaModal" class="modal-overlay" @click="closeMarcaModal">
            <div class="modal-content modal-small" @click.stop>
                <div class="modal-header">
                    <div class="breadcrumb">
                        <span>Inicio</span>
                        <span class="separator">/</span>
                        <span>Producto</span>
                        <span class="separator">/</span>
                        <span class="active">Marca</span>
                    </div>
                    <button class="btn-close" @click="closeMarcaModal">
                        <i class="fa-solid fa-xmark"></i>
                    </button>
                </div>

                <h2 class="modal-title">Marca</h2>

                <div class="modal-form">
                    <div class="modal-form-row-single">
                        <div class="form-group">
                            <label>Nombre <span class="required">*</span></label>
                            <input type="text" placeholder="Ingrese Nombre" v-model="marcaForm.nombre" />
                        </div>

                        <div class="form-group">
                            <label>ID Marca <span class="required">*</span></label>
                            <input type="text" value="999999" disabled class="disabled-input" />
                        </div>
                    </div>

                </div>

                <div class="modal-actions">
                    <button class="btn-cancel" @click="closeMarcaModal">Cancelar</button>
                    <button class="btn-submit" @click="saveMarcaModal">Cargar</button>
                </div>
            </div>
        </div>

        <!-- Modal Presentación -->
        <div v-if="showPresentacionModal" class="modal-overlay" @click="closePresentacionModal">
            <div class="modal-content modal-small" @click.stop>
                <div class="modal-header">
                    <div class="breadcrumb">
                        <span>Inicio</span>
                        <span class="separator">/</span>
                        <span>Producto</span>
                        <span class="separator">/</span>
                        <span class="active">Presentación</span>
                    </div>
                    <button class="btn-close" @click="closePresentacionModal">
                        <i class="fa-solid fa-xmark"></i>
                    </button>
                </div>

                <h2 class="modal-title">Presentación</h2>

                <div class="modal-form">
                    <div class="modal-form-row-single">
                        <div class="form-group">
                            <label>Nombre <span class="required">*</span></label>
                            <input type="text" placeholder="Ingrese Nombre" v-model="presentacionForm.nombre" />
                        </div>

                        <div class="form-group">
                            <label>ID Presentación <span class="required">*</span></label>
                            <input type="text" value="999999" disabled class="disabled-input" />
                        </div>
                    </div>
                </div>

                <div class="modal-actions">
                    <button class="btn-cancel" @click="closePresentacionModal">Cancelar</button>
                    <button class="btn-submit" @click="savePresentacionModal">Cargar</button>
                </div>
            </div>
        </div>

        <!-- Modal Proveedor -->
        <div v-if="showProveedorModal" class="modal-overlay" @click="closeProveedorModal">
            <div class="modal-content modal-small" @click.stop>
                <div class="modal-header">
                    <div class="breadcrumb">
                        <span>Inicio</span>
                        <span class="separator">/</span>
                        <span>Producto</span>
                        <span class="separator">/</span>
                        <span class="active">Proveedor</span>
                    </div>
                    <button class="btn-close" @click="closeProveedorModal">
                        <i class="fa-solid fa-xmark"></i>
                    </button>
                </div>

                <h2 class="modal-title">Proveedor</h2>

                <div class="modal-form">
                    <div class="modal-form-row-single">
                        <div class="form-group">
                            <label>Nombre <span class="required">*</span></label>
                            <input type="text" placeholder="Ingrese Nombre" v-model="proveedorForm.nombre" />
                        </div>

                        <div class="form-group">
                            <label>ID Proveedor <span class="required">*</span></label>
                            <input type="text" value="999999" disabled class="disabled-input" />
                        </div>
                    </div>


                </div>

                <div class="modal-actions">
                    <button class="btn-cancel" @click="closeProveedorModal">Cancelar</button>
                    <button class="btn-submit" @click="saveProveedorModal">Cargar</button>
                </div>
            </div>
        </div>
    </div>
</template>



<script setup>
import { reactive, ref, onMounted } from 'vue';

/* ---------------------------
   Formulario principal
---------------------------- */
const form = reactive({
    id: '',
    nombreProducto: '',
    precioUnitario: '',
    puntoCritico: '',
    existencias: '',
    productoGenerico: '',
    rotacion: 'A',
    marca: '',
    presentacion: '',
    cantidad: '',
    unidadMedida: '',
    proveedor: ''
});

const productos = ref([]);
const marcas = ref(['Playadito', 'Taragüí', 'Rosamonte']);
const presentaciones = ref(['Bolsa 500g', 'Bolsa 1kg', 'Caja 12 unidades']);
const proveedores = ref(['Proveedor 1', 'Proveedor 2', 'Proveedor 3']);

/* LISTA de genéricos (ahora reactiva) */
const productoGenericos = ref(['Yerba', 'Café', 'Té']);

/* control y estado */
const editingId = ref(null);
const formEnabled = ref(false);

/* Modales */
const showModal = ref(false);
const showMarcaModal = ref(false);
const showPresentacionModal = ref(false);
const showProveedorModal = ref(false);

const modalForm = reactive({ descripcion: '', nombre: '', categoria: '' });
const marcaForm = reactive({ descripcion: '', nombre: '' });
const presentacionForm = reactive({ descripcion: '', nombre: '' });
const proveedorForm = reactive({ descripcion: '', nombre: '' });

/* ---------------------------
   onMounted: leer localStorage
---------------------------- */
onMounted(() => {
    const productosData = localStorage.getItem('productos');
    if (productosData) productos.value = JSON.parse(productosData);

    const marcasData = localStorage.getItem('marcas');
    if (marcasData) marcas.value = JSON.parse(marcasData);

    const presentacionesData = localStorage.getItem('presentaciones');
    if (presentacionesData) presentaciones.value = JSON.parse(presentacionesData);

    const proveedoresData = localStorage.getItem('proveedores');
    if (proveedoresData) proveedores.value = JSON.parse(proveedoresData);

    const genericosData = localStorage.getItem('productoGenericos');
    if (genericosData) productoGenericos.value = JSON.parse(genericosData);
});

/* ---------------------------
   Guardar en localStorage
---------------------------- */
const saveToLocalStorage = () => {
    localStorage.setItem('productos', JSON.stringify(productos.value));
    localStorage.setItem('marcas', JSON.stringify(marcas.value));
    localStorage.setItem('presentaciones', JSON.stringify(presentaciones.value));
    localStorage.setItem('proveedores', JSON.stringify(proveedores.value));
    localStorage.setItem('productoGenericos', JSON.stringify(productoGenericos.value));
};

/* ---------------------------
   Funciones CRUD
---------------------------- */
const addItem = () => {
    formEnabled.value = true;
    editingId.value = null;
    Object.keys(form).forEach(k => form[k] = '');
    form.rotacion = 'A';
    form.id = Math.floor(100000 + Math.random() * 900000); // 👈 generate random ID here
};

const submitForm = () => {
    if (!formEnabled.value) return;

    if (!form.nombreProducto || !form.precioUnitario) {
        alert('Complete los campos obligatorios.');
        return;
    }

    if (editingId.value) {
        const index = productos.value.findIndex(p => p.id === editingId.value);
        if (index !== -1) {
            productos.value[index] = { ...form, id: editingId.value };
            alert('Producto modificado correctamente');
        }
    } else {
        const randomId = Math.floor(100000 + Math.random() * 900000);
        form.id = randomId;
        const newProduct = { ...form };
        productos.value.push(newProduct);
        alert('Producto guardado correctamente');
    }

    saveToLocalStorage();
    formEnabled.value = false;
    editingId.value = null;
    Object.keys(form).forEach(k => form[k] = '');
    form.rotacion = 'A';
};

const selectItem = (item) => {
    Object.assign(form, item);
    editingId.value = item.id;
    formEnabled.value = false;
};

const editItem = () => {
    if (!editingId.value) {
        alert('Seleccione un producto para modificar.');
        return;
    }
    formEnabled.value = true;
};

const deleteItem = () => {
    if (!editingId.value) {
        alert('Seleccione un producto para eliminar.');
        return;
    }
    if (confirm('¿Seguro que desea eliminar este producto?')) {
        productos.value = productos.value.filter(p => p.id !== editingId.value);
        saveToLocalStorage();
        alert('Producto eliminado.');
        editingId.value = null;
        Object.keys(form).forEach(k => form[k] = '');
        form.rotacion = 'A';
        formEnabled.value = false;
    }
};

const cancelForm = () => {
    formEnabled.value = false;
    editingId.value = null;
    Object.keys(form).forEach(k => form[k] = '');
    form.rotacion = 'A';
};

/* ---------------------------
   Modales - Producto Genérico (ahora agrega a la lista)
---------------------------- */
const openModal = () => {
    modalForm.descripcion = '';
    modalForm.nombre = '';
    modalForm.categoria = '';
    showModal.value = true;
};
const closeModal = () => showModal.value = false;

const saveModal = () => {
    const name = (modalForm.nombre || '').trim();
    if (!name) {
        alert('Por favor ingrese un nombre para el genérico.');
        return;
    }

    if (!productoGenericos.value.includes(name)) {
        productoGenericos.value.push(name);
        saveToLocalStorage();
    }

    // Seleccionar automáticamente el genérico recién creado en el formulario
    form.productoGenerico = name;

    // limpiar y cerrar
    modalForm.descripcion = '';
    modalForm.nombre = '';
    modalForm.categoria = '';
    closeModal();
};

/* ---------------------------
   Modales - Marca / Presentación / Proveedor
   (se mantienen igual, pero aseguro guardado en localStorage)
---------------------------- */
const openMarcaModal = () => {
    marcaForm.descripcion = '';
    marcaForm.nombre = '';
    showMarcaModal.value = true;
};
const closeMarcaModal = () => showMarcaModal.value = false;
const saveMarcaModal = () => {
    const name = (marcaForm.nombre || '').trim();
    if (!name) {
        alert('Por favor ingrese un nombre para la marca.');
        return;
    }
    if (!marcas.value.includes(name)) {
        marcas.value.push(name);
        saveToLocalStorage();
    }
    form.marca = name;
    marcaForm.descripcion = '';
    marcaForm.nombre = '';
    closeMarcaModal();
};

const openPresentacionModal = () => {
    presentacionForm.descripcion = '';
    presentacionForm.nombre = '';
    showPresentacionModal.value = true;
};
const closePresentacionModal = () => showPresentacionModal.value = false;
const savePresentacionModal = () => {
    const name = (presentacionForm.nombre || '').trim();
    if (!name) {
        alert('Por favor ingrese un nombre para la presentación.');
        return;
    }
    if (!presentaciones.value.includes(name)) {
        presentaciones.value.push(name);
        saveToLocalStorage();
    }
    form.presentacion = name;
    presentacionForm.descripcion = '';
    presentacionForm.nombre = '';
    closePresentacionModal();
};

const openProveedorModal = () => {
    proveedorForm.descripcion = '';
    proveedorForm.nombre = '';
    showProveedorModal.value = true;
};
const closeProveedorModal = () => showProveedorModal.value = false;
const saveProveedorModal = () => {
    const name = (proveedorForm.nombre || '').trim();
    if (!name) {
        alert('Por favor ingrese un nombre para el proveedor.');
        return;
    }
    if (!proveedores.value.includes(name)) {
        proveedores.value.push(name);
        saveToLocalStorage();
    }
    form.proveedor = name;
    proveedorForm.descripcion = '';
    proveedorForm.nombre = '';
    closeProveedorModal();
};
</script>



<style scoped>
* {
    box-sizing: border-box;
}

.main-container {
    background-color: #f5f5f5;
    min-height: 100vh;
    padding: 1rem;
}

.container {
    background-color: white;
    display: flex;
    padding: 1em;
    justify-content: space-between;
    gap: 1rem;
    margin-bottom: 1rem;
}

.crud-buttons {
    display: flex;
    gap: 0.75rem;
}

.btn-add {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.5rem 1rem;
    background-color: white;
    border: 2px solid;
    border-radius: 4px;
    font-weight: 500;
    cursor: pointer;
    transition: all 0.2s;
    font-size: 14px;
}

.btn-new {
    border-color: #16a34a;
    color: #16a34a;
}

.btn-new:hover {
    background-color: #f0fdf4;
}

.btn-edit {
    border-color: #f97316;
    color: #f97316;
}

.btn-edit:hover {
    background-color: #fff7ed;
}

.btn-delete {
    border-color: #dc2626;
    color: #dc2626;
}

.btn-delete:hover {
    background-color: #fef2f2;
}

.navigation-buttons {
    display: flex;
    gap: 0.5rem;
}

.nav-btn {
    padding: 0.5rem 0.75rem;
    background-color: #f97316;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.2s;
    font-weight: 600;
    font-size: 14px;
}

.nav-btn:hover {
    background-color: #ea580c;
}

/* Formulario */
.form-container {
    background-color: white;
    padding: 2rem;
    border-radius: 4px;
}

.form-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
    margin-bottom: 2rem;
}

.form-column {
    display: flex;
    flex-direction: column;
    gap: 1rem;
}

.form-group {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
}

.form-group label {
    font-size: 14px;
    font-weight: 600;
    color: #333;
}

.required {
    color: #dc2626;
}

.form-group input,
.form-group select {
    padding: 0.6rem;
    border: 1px solid #d1d5db;
    border-radius: 4px;
    font-size: 14px;
    outline: none;
    transition: border-color 0.2s;
}

.form-group input:focus,
.form-group select:focus {
    border-color: #3b82f6;
}

.form-group input::placeholder {
    color: #9ca3af;
}

.disabled-input {
    background-color: #e5e7eb;
    cursor: not-allowed;
}

.input-with-button {
    display: flex;
    gap: 0.5rem;
}

.input-with-button select {
    flex: 1;
}

.btn-add-field {
    padding: 0.6rem 0.8rem;
    background-color: #f97316;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.2s;
}

.btn-add-field:hover {
    background-color: #ea580c;
}

.image-upload {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 2rem;
    border: 2px dashed #d1d5db;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.2s;
    background-color: #f9fafb;
}

.image-upload:hover {
    border-color: #9ca3af;
    background-color: #f3f4f6;
}

.image-upload i {
    font-size: 2rem;
    color: #9ca3af;
    margin-bottom: 0.5rem;
}

.image-upload span {
    font-size: 14px;
    color: #6b7280;
}

.radio-group {
    display: flex;
    gap: 1.5rem;
}

.radio-label {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    cursor: pointer;
    font-size: 14px;
}

.radio-label input[type="radio"] {
    width: auto;
    cursor: pointer;
}

.form-actions {
    display: flex;
    justify-content: flex-start;
    gap: 1rem;
    margin-top: 1rem;
}

.btn-cancel {
    padding: 0.6rem 2rem;
    background-color: #dc2626;
    color: white;
    border: none;
    border-radius: 4px;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.2s;
}

.btn-cancel:hover {
    background-color: #b91c1c;
}

.btn-submit {
    padding: 0.6rem 2rem;
    background-color: #16a34a;
    color: white;
    border: none;
    border-radius: 4px;
    font-weight: 600;
    cursor: pointer;
    transition: background-color 0.2s;
}

.btn-submit:hover {
    background-color: #15803d;
}

@media (max-width: 768px) {
    .form-row {
        grid-template-columns: 1fr;
    }

    .container {
        flex-direction: column;
    }
}

/* Modal Styles */
.modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
}

.modal-content {
    background-color: white;
    border-radius: 8px;
    width: 90%;
    max-width: 600px;
    max-height: 90vh;
    overflow-y: auto;
    box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
}

.modal-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem 1.5rem;
    border-bottom: 1px solid #e5e7eb;
}

.breadcrumb {
    font-size: 12px;
    color: #6b7280;
}

.breadcrumb .separator {
    margin: 0 0.5rem;
}

.breadcrumb .active {
    color: #111827;
    font-weight: 600;
}

.btn-close {
    background: none;
    border: none;
    font-size: 1.5rem;
    cursor: pointer;
    color: #6b7280;
    padding: 0;
    width: 32px;
    height: 32px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
    transition: background-color 0.2s;
}

.btn-close:hover {
    background-color: #f3f4f6;
}

.modal-title {
    font-size: 1.5rem;
    font-weight: 600;
    padding: 1rem 1.5rem 0.5rem;
    margin: 0;
}

.modal-crud-buttons {
    display: flex;
    gap: 0.75rem;
    padding: 0 1.5rem 1rem;
}

.modal-form {
    padding: 0 1.5rem 1rem;
    display: flex;
    flex-direction: column;
    gap: 1rem;
}

.modal-form-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
}

.modal-form-row:last-of-type {
    grid-template-columns: 1fr;
}

.modal-actions {
    display: flex;
    justify-content: flex-start;
    gap: 1rem;
    padding: 1rem 1.5rem;
    border-top: 1px solid #e5e7eb;
}

.modal-small {
    max-width: 500px;
}

.modal-form-row-single {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
}

.modal-form-row-full {
    display: grid;
    grid-template-columns: 1fr;
}

.presentation-input {
    display: flex;
    gap: 0.5rem;
}

.quantity-input {
    width: 40%;
    min-width: 80px;
}

.unit-select {
    flex: 1;
}
</style>
