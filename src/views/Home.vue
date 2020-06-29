<template>
  <div class="home">
    <Header msg="Usuarios" />
    <Tabla :personas="personas" />
  </div>
</template>

<script>
// @ is an alias to /src
import Header from "@/components/Header.vue";
import Tabla from "@/components/Tabla.vue";

export default {
  name: "Home",
  components: {
    Header,
    Tabla,
  },
  data() {
    return {
      personas: null
    };
  },
  personaService: null,
  created() {
    this.getUsuarios();
    //this.personaService = new PersonaService();
  },
  methods: {
    async getUsuarios() {
      try {
        console.log(this.headers)
        const usuarios = await this.axios.get("usuarios");
        this.personas = usuarios.data;
        //console.log(this.personas);
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>
