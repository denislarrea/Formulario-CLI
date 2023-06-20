<template>
    <div class="formulario">
<vue-form
  ref="form"
  class="formulario-listas"
  :state="formstate"
  @submit.prevent="onSubmit"
>
<!--Nombre -->
        <validate class="formulario-item" tag="label">
          <span>Nombre: </span>
          <input
            type="text"
            v-model="model.name"
            required
            name="name"
          />
          <field-messages name="name" show="$touched">
            <div slot="required">El nombre es requerido</div>
          </field-messages>
        </validate>
<!-- Apellido -->
        <validate class="formulario-item" tag="label">
          <span>Apellido: </span>
          <input
            type="text"
            v-model="model.apellido"
            required
            name="apellido"
          />
          <field-messages name="apellido" show="$touched">
            <div slot="required">El apellido es requerido</div>
          </field-messages>
        </validate>

<!--edad-->

        <validate class="formulario-item" tag="label">
          <span>Edad: </span>
          <input
            type="number"
            v-model="model.edad"
            required
            name="edad"
          />
          <field-messages name="edad" show="$touched">
            <div slot="required">La edad es requerida</div>
          </field-messages>
        </validate>

<!--Email-->

        <validate class="formulario-item" tag="label">
          <span>Email: </span>
          <input
            type="email"
            v-model="model.email"
            required
            name="email"
          />
          <field-messages name="email" show="$touched">
            <div slot="required">El email es requerido</div>
            <div slot="email">El email no es válido</div>
          </field-messages>
        </validate>

<!-- Documento -->

    <validate class="formulario-item" tag="label">
      <span>Tipo de documento: </span>
      <div class="radio-buttons">
        <div class="radio-button">
          <input
            type="radio"
            id="dni"
            value="DNI"
            v-model="model.documentType"
            required
            name="documentType"
          />
          <label for="dni">DNI</label>
        </div>

        <div class="radio-button">
          <input
            type="radio"
            id="cuit"
            value="CUIT"
            v-model="model.documentType"
            required
            name="documentType"
          />
          <label for="cuit">CUIT</label>
        </div>
      </div>
      <field-messages name="documentType" show="$touched">
        <div slot="required">Debe seleccionar un tipo de documento</div>
      </field-messages>
    </validate>

    <validate class="formulario-item" tag="label">
  <span>Número de Documento: </span>
  <input
    type="text"
    v-model="model.documentNumber"
    :maxlength="getDocumentNumberMaxLength()"
    required
    name="documentNumber"
  />
  <field-messages name="documentNumber" show="$touched">
    <div slot="required">El número de Documento es requerido</div>
    <div slot="maxlength">El número de Documento debe tener {{ getDocumentNumberMaxLength() }} dígitos</div>
  </field-messages>
</validate>

<!-- Telefono -->

  <validate class="formulario-item" tag="label">
  <span>Teléfono: </span>
  <div class="telefono-inputs">
   <input
    type="text"
    v-model="model.telefono[0]"
    :maxlength="4"
    required
    name="caracteristica"
    placeholder="Número sin el *0*"
  />
  <field-messages name="caracteristica" show="$touched">
    <div slot="required">La característica es requerida</div>
  </field-messages>

  <input
    type="text"
    v-model="model.telefono[1]"
    :maxlength="7"
    required
    name="numero"
    placeholder="Número sin el *15*"
  />
  <field-messages name="numero" show="$touched">
    <div slot="required">El número de teléfono es requerido</div>
  </field-messages>
  </div>
</validate>
 
  <!-- Password y boton enviar-->
        <validate
          class="formulario-item"
          tag="label"
          :custom="{'check-password': checkPassword}"
        >
          <span>Contraseña: </span>
          <input
            type="password"
            v-model="model.password"
            name="password"
            required
          />
          <field-messages name="password" show="$touched">
            <div slot="required">La contraseña es requerida</div>
            <div slot="check-password">
              Debe contener al menos 8 caracteres, una mayúscula, una minúscula,
              un número y un carácter especial
            </div>
          </field-messages>
        </validate>
  
        <button type="submit">Enviar</button>
      </vue-form>
    </div>
  </template>
  
   <!-- Datos y Validaciones -->

  <script>
  export default {
    name: "FormularioVue",
    data() {
      return {
        formstate: {},
        model: {
          name: "",
          apellido: "",
          email: "",
          edad: "",
          password: "",
          documentType: "",
          documentNumber:"",
          telefono: [],
     
        },
      };
    },
  
    methods: {

    
onSubmit() {
  if (
    this.formstate.$valid &&
    this.model.documentType &&
    this.isValidDocumentNumber() &&
    this.isValidPhoneNumber()
  ) {
    this.$emit("emit-form", { ...this.model });
    this.clearForm();
    alert("¡Formulario enviado!");
  } else {
    alert("Por favor, completa todos los campos del formulario correctamente.");
  }
},

isValidDocumentNumber() {
  if (this.model.documentType === 'DNI') {
    return /^\d{8}$/.test(this.model.documentNumber);
  } else if (this.model.documentType === 'CUIT') {
    return /^\d{11}$/.test(this.model.documentNumber);
  }
  return false;
},
isValidPhoneNumber() {
  const [caracteristica, numero] = this.model.telefono;

  if (caracteristica.startsWith("0")) {
    return false; // La característica no puede comenzar con 0
  }

  if (numero.startsWith("15")) {
    return false; // El número no puede comenzar con 15
  }

  return /^[0-9]{7}$/.test(numero); // Validar que el número tenga 7 dígitos
},

getDocumentNumberMaxLength() {
    if (this.model.documentType === 'DNI') {
      return 8; // DNI tiene 8 dígitos
    } else if (this.model.documentType === 'CUIT') {
      return 11; // CUIT tiene 11 dígitos
    }
    return 0;
  },

      checkPassword(value) {
        return /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[$@$!%*?&])[A-Za-z\d$@$!%*?&]{8,15}/.test(
          value
        );
      },
      clearForm() {
  this.model = {
    name: "",
    apellido: "",
    email: "",
    edad: "",
    password: "",
    documentType: "",
    documentNumber: "",
    telefono: [],
  };
  this.$refs.form.state = {}; // Reiniciar el estado del formulario manualmente
},
    },
  };
  </script>


  <!-- Estilos -->
  
  <style>
  .formulario {
    padding: 10px;
    background-color: rgb(188, 248, 213);
  }
  
  .formulario-item {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    padding-bottom: 10px;
  }
  
  .formulario-item input {
    border: none;
    border-radius: 5px;
    padding: 4px;
  }
  
  button {
    border: none;
    background-color: rgb(16, 235, 173);
    padding: 10px;
    border-radius: 5px;
    color: #fff;
  }


    .radio-buttons {
    display: flex;
  }

  .radio-button {
    margin-right: 10px;
  }

  .telefono-inputs {
  display: flex;
}

.telefono-inputs input {
  margin-right: 5px;
}


  </style>
  