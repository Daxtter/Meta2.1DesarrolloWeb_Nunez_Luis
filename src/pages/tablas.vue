<script setup>
import { ref, computed, watch } from 'vue';


const informacion = ref("");
async function obtenerInformacion()
    {


        let respuesta = await fetch ("https://jsonplaceholder.typicode.com/todos");
        let respuesta2 = await respuesta.json()
        informacion.value = respuesta2
    }
    obtenerInformacion();
const dialog = ref(false);
const dialogDelete = ref(false);
const headers = [
  {title: 'ID usuario', key: 'userId'},
  {title: 'ID ', key: 'id'},
  {title: 'Titulo ', key: 'title'},
  {title: 'Completado', key: 'completed'},
  {title: 'Actions', key: 'actions',sortable:false}
];


const desserts = ref([]);
const editedIndex = ref(-1);
const editedItem = ref({
  userId: 0,
  id: 0,
  title: '',
  completed: '',
 
});
const defaultItem = {
  userId: 0,
  id: 0,
  title: '',
  completed: '',
 
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


const deleteItemConfirm = () => {
  informacion.value.splice(editedIndex.value, 1);
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


const save = () => {
  if (editedIndex.value > -1) {
    Object.assign(informacion.value[editedIndex.value], editedItem.value);
  } else {
    informacion.value.push(editedItem.value);
  }
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
        <v-toolbar-title>My CRUD</v-toolbar-title>
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
                      v-model="editedItem.userId"
                      label="ID usuario"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="4"
                    sm="6"
                  >
                    <v-text-field
                      v-model="editedItem.id"
                      label="ID"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="4"
                    sm="6"
                  >
                    <v-text-field
                      v-model="editedItem.title"
                      label="Titulo"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    md="4"
                    sm="6"
                  >
                    <v-text-field
                      v-model="editedItem.completed"
                      label="Completado"
                    ></v-text-field>
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
            <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue-darken-1" variant="text" @click="closeDelete">Cancel</v-btn>
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
