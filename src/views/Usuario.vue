<template>
  <div>
    <Header />
    <v-data-table
      :headers="headers"
      :items="telefonos"
      sort-by="tipo"
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar flat color="white">
          <v-toolbar-title>Telefonos</v-toolbar-title>
          <v-divider class="mx-4" inset vertical></v-divider>
          <v-spacer></v-spacer>
          <v-dialog v-model="dialog" max-width="500px">
            <template v-slot:activator="{ on, attrs }">
              <v-btn color="primary" dark class="mb-2" v-bind="attrs" v-on="on"
                >Nuevo telefono</v-btn
              >
            </template>
            <v-card>
              <v-card-title>
                <span class="headline">{{ formTitle }}</span>
              </v-card-title>

              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.numero"
                        label="Numero"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.tipo"
                        label="Tipo"
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" sm="6" md="4">
                      <v-text-field
                        v-model="editedItem.operadora"
                        label="Operadora"
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </v-container>
              </v-card-text>

              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="close">Cancel</v-btn>
                <v-btn color="blue darken-1" text @click="save">Save</v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item.actions="{ item }">
        <v-icon small class="mr-2" @click="editItem(item)" color="blue">
          mdi-pencil
        </v-icon>
        <v-icon small @click="deleteItem(item)" color="red">
          mdi-delete
        </v-icon>
      </template>
      <template v-slot:no-data>
        <v-btn color="primary" to="/">regresar</v-btn>
      </template>
    </v-data-table>
  </div>
</template>

<script>
import Header from "@/components/Header.vue";

export default {
  name: "Usuario",
  components: {
    Header,
  },
  data: () => ({
    dialog: false,
    headers: [
      {
        text: "Numero",
        align: "start",
        sortable: false,
        value: "numero",
      },
      { text: "Tipo", value: "tipo" },
      { text: "Operadora", value: "operadora" },

      { text: "Actions", value: "actions", sortable: false },
    ],
    telefonos: [],
    editedIndex: -1,
    editedItem: {
      numero: "",
      tipo: "",
      operadora: "",
    },
    defaultItem: {
      numero: "",
      tipo: "",
      operadora: "",
    },
  }),

  computed: {
    formTitle() {
      return this.editedIndex === -1 ? "Nuevo telefono" : "Editar telefono";
    },
  },

  watch: {
    dialog(val) {
      val || this.close();
    },
  },

  created() {
    this.getTelefonos();
  },

  methods: {
    async getTelefonos() {
      try {
        let userId = this.$route.params.id;
        let telefonos = await this.axios.get("usuarios/telefonos/" + userId);
        this.telefonos = telefonos.data;
        //console.log(telefonos);
      } catch (error) {
        console.log(error);
      }
    },

    editItem(item) {
      this.editedIndex = this.telefonos.indexOf(item);
      this.editedItem = Object.assign({}, item);
      this.dialog = true;
    },

    async deleteItem(item) {
      const index = this.telefonos.indexOf(item);
      let res =
        confirm("Are you sure you want to delete this item?") &&
        this.telefonos.splice(index, 1);
      if (res) {
        try {
          await this.axios.delete(`telefonos/${item.id}/`);
        } catch (error) {
          console.log(error);
        }
      }
    },

    close() {
      this.dialog = false;
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem);
        this.editedIndex = -1;
      });
    },

    async save() {
      console.log("Metodo Guardar");
      let userId = parseInt(this.$route.params.id);
      if (this.editedIndex > -1) {
        Object.assign(this.telefonos[this.editedIndex], this.editedItem);
        try {
          let telefonoDB = await this.axios.put(
            `telefonos/${this.editedItem.id}/`,
            this.editedItem
          );
        } catch (error) {
          console.log(error);
        }
      } else {
        let newTelefono = {
          numero: this.editedItem.numero,
          tipo: this.editedItem.tipo,
          operadora: this.editedItem.operadora,
          persona_fk: userId,
        };
        try {
          let telefonoDB = await this.axios.post("telefonos/", newTelefono);
          this.telefonos.push(this.editedItem);
        } catch (error) {
          console.log(error);
        }
      }
      this.close();
    },
  },
};
</script>
