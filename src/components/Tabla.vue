<template>
  <div class="container">
    <table class="table table-striped">
      <thead>
        <tr>
          <th scope="col">Cedula</th>
          <th scope="col">Nombre</th>
          <th scope="col">Apellido</th>
          <th scope="col">Correo</th>
          <th scope="col">Detalles</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="persona in personas" :key="persona.id">
          <th scope="row">{{persona.cedula}}</th>
          <td>{{persona.nombre}}</td>
          <td>{{persona.apellido}}</td>
          <td>{{persona.correo}}</td>
          <td>
            <router-link :to="{name: 'Usuario', params: {id: persona.id}}">
              <button type="button" class="btn btn-primary text-white">Detalle</button>
            </router-link>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
export default {
  name: "Tabla",
  props:["personas"]
};
</script>
<style scoped></style>
