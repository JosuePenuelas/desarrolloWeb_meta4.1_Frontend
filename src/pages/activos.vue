<template>
    <v-card flat>
        <v-card-title>Activos</v-card-title>
        <template v-slot:text>
            <v-text-field v-model="search" label="Buscar" prepend-inner-icon="mdi-magnify" single-line
                variant="outlined" hide-details></v-text-field>
        </template>

        <v-container>
            <v-btn class="mb-2 mr-2" color="primary" @click="showNewDialog = true">
                New Item
            </v-btn>
            <v-btn class="mb-2 mr-2" color="secondary" @click="showUpdateDialog = true">
                Update Item
            </v-btn>
            <v-btn class="mb-2" color="red" @click="showDeleteDialog = true">
                Delete Item
            </v-btn>
        </v-container>

        <v-data-table :headers="headers" :items="datos" :search="search" :sort-by="[{ key: 'id', order: 'asc' }]"
            :sort-by1="[{ key: 'title', order: 'asc' }]" :sort-by2="[{ key: 'body', order: 'asc' }]"></v-data-table>

        <!-- Diálogo para introducir ID a eliminar -->
        <v-dialog v-model="showDeleteDialog" max-width="500px">
            <v-card>
                <v-card-title>Eliminar Elemento</v-card-title>
                <v-card-text>
                    <v-text-field v-model.number="idToDelete" label="ID a eliminar" type="number"></v-text-field>
                </v-card-text>
                <v-card-actions>
                    <v-btn color="red darken-1" text
                        @click="eliminar(idToDelete); showDeleteDialog = false">Eliminar</v-btn>
                    <v-btn text @click="showDeleteDialog = false">Cancelar</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>

        <!-- Diálogo para introducir nuevos datos -->
        <v-dialog v-model="showNewDialog" max-width="500px">
            <v-card>
                <v-card-title>Nuevo Elemento</v-card-title>
                <v-card-text>
                    <v-text-field v-model.number="newId" label="ID" type="number"></v-text-field>
                    <v-text-field v-model.number="newNumSerie" label="Número de Serie" type="number"></v-text-field>
                    <v-text-field v-model.number="newNumInv" label="Número de Inventario" type="number"></v-text-field>
                    <v-text-field v-model="newDesc" label="Descripción"></v-text-field>
                    <v-text-field v-model="newUbicacion" label="Ubicación"></v-text-field>
                    <v-text-field v-model="newResponsable" label="Responsable"></v-text-field>
                    <v-text-field v-model="newImagen" label="Imagen"></v-text-field>
                </v-card-text>
                <v-card-actions>
                    <v-btn color="primary" @click="crearNuevoItem">Crear</v-btn>
                    <v-btn text @click="showNewDialog = false">Cancelar</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>

        <!-- Diálogo para actualizar datos -->
        <v-dialog v-model="showUpdateDialog" max-width="500px">
            <v-card>
                <v-card-title>Actualizar Elemento</v-card-title>
                <v-card-text>
                    <v-text-field v-model.number="updateId" label="ID" type="number"
                        @input="cargarDatosUpdate"></v-text-field>
                    <v-text-field v-model.number="updateNumSerie" label="Número de Serie" type="number"></v-text-field>
                    <v-text-field v-model.number="updateNumInv" label="Número de Inventario"
                        type="number"></v-text-field>
                    <v-text-field v-model="updateDesc" label="Descripción"></v-text-field>
                    <v-text-field v-model="updateUbicacion" label="Ubicación"></v-text-field>
                    <v-text-field v-model="updateResponsable" label="Responsable"></v-text-field>
                    <v-text-field v-model="updateImagen" label="Imagen"></v-text-field>
                </v-card-text>
                <v-card-actions>
                    <v-btn color="primary" @click="actualizarItem">Actualizar</v-btn>
                    <v-btn text @click="showUpdateDialog = false">Cancelar</v-btn>
                </v-card-actions>
            </v-card>
        </v-dialog>

    </v-card>
</template>

<script setup>
import { ref } from 'vue';

const search = ref('');
const headers = ref([
    { align: 'start', key: 'id', sortable: false, title: 'ID' },
    { key: 'numSerie', title: 'Número de serie' },
    { key: 'numInventario', title: 'Número de inventario' },
    { key: 'descripcion', title: 'Descripción' },
    { key: 'ubicacion', title: 'Ubicación' },
    { key: 'responsable', title: 'Responsable' },
    { key: 'imagen', title: 'Imagen' },
]);

const datos = ref([]);

//para borrar
const showDeleteDialog = ref(false);
const idToDelete = ref('');

//para nuevo dato
const showNewDialog = ref(false);
const newId = ref('');
const newNumSerie = ref('');
const newNumInv = ref('');
const newDesc = ref('');
const newUbicacion = ref('');
const newResponsable = ref('');
const newImagen = ref('');

//para nuevo dato
const showUpdateDialog = ref(false);
const updateId = ref('');
const updateNumSerie = ref('');
const updateNumInv = ref('');
const updateDesc = ref('');
const updateUbicacion = ref('');
const updateResponsable = ref('');
const updateImagen = ref('');

async function obtenerDatos() {
    try {
        const response = await fetch("https://localhost:4000/activos");
        const data = await response.json();
        datos.value = data;
    } catch (error) {
        console.error('Error al obtener datos:', error);
    }
}

async function eliminar(id) {
    try {
        const response = await fetch(`https://localhost:4000/activos/${id}`, {
            method: 'DELETE',
        });
        obtenerDatos();

    } catch (error) {
        console.error('Error al crear el delete:', error);
    }
}

async function crearNuevoItem() {
    try {
        const response = await fetch('https://localhost:4000/activos', {
            method: 'POST',
            body: JSON.stringify({
                id: newId.value,
                numSerie: newNumSerie.value,
                numInventario: newNumInv.value,
                descripcion: newDesc.value,
                ubicacionId: newUbicacion.value,
                responsableId: newResponsable.value,
                imagen: newImagen.value,
            }),
            headers: {
                'Content-type': 'application/json; charset=UTF-8',
            },
        });
        showNewDialog.value = false;
        obtenerDatos();
    } catch (error) {
        console.error('Error al crear el nuevo post:', error);
    }
}

async function actualizarItem() {
    try {
        const response = await fetch(`https://localhost:4000/activos/${updateId.value}`, {
            method: 'PUT',
            body: JSON.stringify({
                id: updateId.value,
                numSerie: updateNumSerie.value,
                numInventario: updateNumInv.value,
                descripcion: updateDesc.value,
                ubicacionId: updateUbicacion.value,
                responsableId: updateResponsable.value,
                imagen: updateImagen.value,
            }),
            headers: {
                'Content-type': 'application/json; charset=UTF-8',
            },
        });
        showUpdateDialog.value = false;
        obtenerDatos();
    } catch (error) {
        console.error('Error al crear el nuevo post:', error);
    }
}

async function cargarDatosUpdate() {
    try {
        const response = await fetch(`https://localhost:4000/activos/${updateId.value}`);
        const data = await response.json();
        updateNumSerie.value = data.numSerie;
        updateNumInv.value = data.numInventario;
        updateDesc.value = data.descripcion;
        updateUbicacion.value = data.ubicacion.value;
        updateResponsable.value = data.responsable.value;
        updateImagen.value = data.imagen;
    } catch (error) {
        console.error('Error al cargar los datos para actualizar:', error);
    }
}

obtenerDatos();
</script>