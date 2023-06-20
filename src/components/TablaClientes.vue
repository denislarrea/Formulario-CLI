<template>
    <div class="tabla-container">
       <h1>Información - Registro de Clientes Sucursal 1</h1>
      <table>
        <thead>
          <tr>
            <th>Nombre</th>
            <th>Apellido</th>
            <th>Email</th>
            <th>Edad</th>
            <th>Tipo de Documento</th>
            <th>Número de Documento</th>
            <th>Número de Telefono</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(user, index) in users" :key="index">
            <td>{{ user.name }}</td>
            <td>{{ user.apellido }}</td>
            <td>{{ user.email }}</td>
            <td>{{ user.edad }}</td>
            <td>{{user.documentType}}</td>
            <td>{{user.documentNumber}}</td>
            <td>{{getFullPhoneNumber(user.telefono)}}</td>


          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
export default {
  name: "InfoTabla",
  props: {
    users: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    getFullPhoneNumber(telefono) {
      if (telefono && telefono.length === 2) {
        const [caracteristica, numero] = telefono;
        return `${caracteristica}-${numero}`;
      }
      return "";
    },
  },
};
  </script>
  
  <style scoped>
  .tabla-container {
    border: 2px solid black;
    margin-top: 20px;
  }
  
  table {
    width: 100%;
    border-collapse: collapse;
  }
  
  th,
  td {
    padding: 8px;
    text-align: center;
    border-bottom: 1px solid #57b3f4;
  }
  
  th {
    background-color: #7eb7f4;
  }
  </style>  