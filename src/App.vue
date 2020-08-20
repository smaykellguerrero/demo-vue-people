<template>
  <div class="container mt-4">
    <div class="row">
      <div class="col col-md-4">
        <form>
          <div class="form-row">
            <div class="form-group col-md-12">
              <label for="name">Nombre</label>
              <input
                id="name"
                class="form-control"
                type="text"
                v-model="person.name"
                required
              />
            </div>
          </div>
          <div class="form-row">
            <div class="form-group col-md-6">
              <label for="gender">Genero</label>
              <select id="gender" class="form-control" v-model="person.gender">
                <option
                  v-for="gender in genders"
                  :key="gender.value"
                  :value="gender.value"
                  >{{ gender.text }}</option
                >
              </select>
            </div>
            <div class="form-group col-md-6">
              <label for="birth-date">Fec. Nac.</label>
              <input
                id="birth-date"
                class="form-control"
                type="date"
                v-model="person.birthDate"
                required
              />
            </div>
          </div>
          <div class="form-row">
            <div class="form-group col-md-6">
              <label for="weight">Peso</label>
              <input
                id="weight"
                class="form-control"
                type="text"
                v-model="person.weight"
                required
              />
            </div>
            <div class="form-group col-md-6">
              <label for="height">Altura</label>
              <input
                id="height"
                class="form-control"
                type="text"
                v-model="person.height"
                required
              />
            </div>
          </div>
          <div class="form-row">
            <div class="form-group col-md-6">
              <button
                class="btn btn-outline-primary btn-sm"
                v-if="person.id == null"
                v-on:click.prevent="createPerson"
              >
                Guardar
              </button>
              <button
                class="btn btn-outline-warning btn-sm"
                v-else
                v-on:click.prevent="updatePerson(person.id)"
              >
                Actualizar
              </button>
              <button
                class="btn btn-outline-secondary btn-sm"
                v-on:click.prevent="clearForm"
              >
                Cancelar
              </button>
            </div>
          </div>
        </form>
      </div>
      <div class="col">
        <div class="table-responsive">
          <table class="table table-hover">
            <thead>
              <tr>
                <th v-for="field in fields" :key="field">{{ field }}</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="person in people" :key="person.id">
                <th scope="row">{{ person.id }}</th>
                <td>{{ person.name }}</td>
                <td>{{ person.gender }}</td>
                <td>{{ person.birthDate }}</td>
                <td>{{ person.weight }}</td>
                <td>{{ person.height }}</td>
                <td>
                  <button
                    class="btn btn-outline-primary btn-sm"
                    v-on:click="finOne(person.id)"
                  >
                    Editar
                  </button>
                  <button
                    class="btn btn-outline-danger btn-sm"
                    v-on:click="deletePerson(person.id)"
                  >
                    Eliminar
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

const url = "http://localhost:5000";

export default {
  data() {
    return {
      fields: [
        "Id",
        "Nombre",
        "Genero",
        "Fec. Nac.",
        "Peso",
        "Altura",
        "Operación",
      ],
      people: [],
      genders: [
        { value: "M", text: "Masculino" },
        { value: "F", text: "Femenino" },
      ],
      person: {
        id: null,
        name: "",
        gender: "M",
        birthDate: "",
        weight: 0,
        height: 0,
      },
    };
  },
  created() {
    this.findAll();
  },
  methods: {
    clearForm() {
      this.person = {
        id: null,
        name: "",
        gender: "M",
        birthDate: "",
        weight: 0,
        height: 0,
      };
    },
    async findAll() {
      try {
        const res = await axios.get(`${url}/api/people/`);
        this.people = res.data;
      } catch (error) {
        alert(error);
      }
    },
    async finOne(id) {
      try {
        const res = await axios.get(`${url}/api/people/${id}`);
        this.person = res.data;
      } catch (error) {
        alert(error);
      }
    },
    async createPerson() {
      try {
        const res = await axios.post(`${url}/api/people/`, this.person);
        this.person = res.data;
        console.log(res);
        this.findAll();
      } catch (error) {
        alert(error);
      }
    },
    async updatePerson(id) {
      try {
        const res = await axios.put(`${url}/api/people/${id}`, this.person);
        this.person = res.data;
        this.findAll();
      } catch (error) {
        alert(error);
      }
    },
    async deletePerson(id) {
      try {
        const res = await axios.delete(`${url}/api/people/${id}`);
        if (res.status == 200) this.findAll();
      } catch (error) {
        alert(error);
      }
    },
  },
};
</script>
