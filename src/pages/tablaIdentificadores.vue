<script setup>
import { ref, computed, watch } from 'vue';
import axios from 'axios';
const informacion = ref("");
import Llave from "../llave.js";
const token = Llave.llave
axios.defaults.headers.common = {'Authorization': `bearer ${token}`};

let url  = "https://localhost:4005/identificadores";
//console.log(informacion)
async function obtenerInformacion()
    {
        let respuesta = await axios.get(url);
        informacion.value = respuesta.data;
    }
    obtenerInformacion();
      console.log(informacion);
const dialog = ref(false);
const dialogDelete = ref(false);
const headers = [
  {title: 'ID', key: 'id'},
  {title: 'Nombre ', key: 'nombre'},
  {title: 'IP', key: 'ip'},
  {title: 'Creado A', key: 'createdAt'},
  {title: 'Actualizado A ', key: 'updatedAt'},
  {title: 'Actions', key: 'actions',sortable:false}
];

let id;
const desserts = ref([]);
const editedIndex = ref(-1);
const editedItem = ref({
  id: '',
  nombre: '',
  ip:''
  });
const defaultItem = {
  id: '',
  nombre: '',
  ip:''
};


const formTitle = computed(() => (editedIndex.value === -1 ? 'New Item' : 'Edit Item'));


watch(dialog, (val) => val || close());
watch(dialogDelete, (val) => val || closeDelete());






const editItem = (item) => {
  editedIndex.value = informacion.value.indexOf(item);
  editedItem.value = { ...item };
  dialog.value = true;
};


const deleteItem = (item) => {
  editedIndex.value = informacion.value.indexOf(item);
  editedItem.value = { ...item };
  dialogDelete.value = true;
};


const deleteItemConfirm = async () => {
  id = informacion.value[editedIndex.value].id;
  console.log(id);
  await axios.delete(url+id);
  
  obtenerInformacion();
  
  closeDelete();
};


const close = () => {
  dialog.value = false;
  editedItem.value = { ...defaultItem };
  editedIndex.value = -1;
};


const closeDelete = () => {
  dialogDelete.value = false;
  editedItem.value = { ...defaultItem };
  editedIndex.value = -1;
};


const save = async () => {
  
  
  if (editedIndex.value > -1) {
    id  = informacion.value[editedIndex.value].id;
    console.log("HIzo un put")  
    console.log(editedItem.value);  
    await axios.put(url+"/"+id,editedItem.value);
    console.log(id);
  
  } else {
    await axios.post(url,editedItem.value);
  }
  obtenerInformacion();
  close();
};






    console.log(informacion);
   


  </script>
<template>
  <v-data-table
    :headers="headers"
    :items="informacion"
   
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>Tabla Identificadores</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ props }">
            <v-btn
              class="mb-2"
              color="primary"
              dark
              v-bind="props"
            >
              New Item
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>


            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    md="4"
                    sm="6"
                  >
                    <v-text-field
                      v-model="editedItem.id"
                      label="id"
                      readonly  
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="4"
                    sm="6"
                  >
                    <v-text-field
                      v-model="editedItem.nombre"
                      label="nombre"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="4"
                    sm="6"
                  >
                  <v-text-field
                      v-model="editedItem.ip"
                      label="ip"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="4"
                    sm="6"
                  >
                  </v-col>
                 </v-row>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">¿Estas seguro que quieres eliminar?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue-darken-1" variant="text" @click="closeDelete">Cancelar</v-btn>
              <v-btn color="blue-darken-1" variant="text" @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon
        class="me-2"
        size="small"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        size="small"
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
</template>