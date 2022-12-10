<script>
import axios from "axios";
export default {

    data: () => ({
        result: null,
        id: null,
        asignatur: {
            nombres: null,
            creditos: null,
            docente: null,
            prerrequisito: null,
            horasTA: null,
            horasTD: null
        },
        bool: false
    }),

    methods: {
        deleteAsignatura(asignaturaId) {
            return axios.delete(`http://localhost/asignaturasLaravel/public/api/asignatura/${asignaturaId}/destroy`, {
                headers: {
                    'Content-type': 'application/json'
                }
            }).then((result) => {
                this.getAll();
            })
        },

        editAsignatura(asignaturaId) {
            return axios.get(`http://localhost/asignaturasLaravel/public/api/asignatura/${asignaturaId}/edit`, {
                headers: {
                    'Content-type': 'application/json'
                }
            }).then((result) => {
                this.asignatur = result.data.data;
                this.bool = true;
                console.log(this.asignaturaer);
            })
        },

        asignaturaPost(data) {
            axios.post('http://localhost/asignaturasLaravel/public/api/asignatura/store', data, {
                headers: {
                    'Content-type': 'application/json'
                }
            }).then((result) => {
                this.asignatur = result.data.data;
                console.log(this.asignaturaer);
                this.bool = false;
                this.getAll();
            })
        },

        putAsignatura(asignaturaId, data) {
            return axios.put(`http://localhost/asignaturasLaravel/public/api/asignatura/${asignaturaId}/update`, data, {
                headers: {
                    'Content-type': 'application/json'
                }
            }).then((result) => {
                this.asignatur = result.data.data;
                this.bool = false;
                console.log(this.asignatur);
                this.getAll();
            })
        },

        getAll() {
            axios.get("http://localhost/asignaturasLaravel/public/api/asignatura/all").then((result) => {
                this.result = result.data.data;
            })
        }
    },

    created() {
        axios.get("http://localhost/asignaturasLaravel/public/api/asignatura/all").then((result) => {
            this.result = result.data.data;
            console.log(result.data.data[0].nombres);
        })
    }
};
</script>

<template >
    <div>
        <br>
        <div class="row">
                <div class="col" v-if="bool">
                    <label for="nameCategoriaId">ID de la asignatura: </label>
                    <label for="nameCategoriaId">{{ asignatur.id }} </label>
                </div>
                <br>
                <div class="col">
                    <label for="nameCategoriaId">Asignatura: </label>
                    <input type="text" id="nameCategoriaId" v-model="asignatur.nombres">
                </div>
                <br>
                <div class="col">
                    <label for="categoryId">Créditos: </label>
                    <input type="text" id="categoryId" v-model="asignatur.creditos">
                </div>
                <br>
                <div class="col">
                    <label for="descriptionCategoriaId"> Docente: </label>
                    <input type="text" id="descriptionCategoriaId" v-model="asignatur.docente">
                </div>
                <br>
                <div class="col">
                    <label for="descriptionCategoriaId">Prerrequisito: </label>
                    <input type="text" id="descriptionCategoriaId" v-model="asignatur.prerrequisito">
                </div>
                <br>
                <div class="col">
                    <label for="descriptionCategoriaId">Horas TA: </label>
                    <input type="text" id="descriptionCategoriaId" v-model="asignatur.horasTA">
                </div>
                <br>
                <div class="col">
                    <label for="descriptionCategoriaId">Horas TD: </label>
                    <input type="text" id="descriptionCategoriaId" v-model="asignatur.horasTD">
                </div>
                <br>
                <br>
                <div class="form-group">
                    <button class="btn btn-primary" type="submit" @click="asignaturaPost(asignatur)"
                        v-if="(bool == false)">Guardar</button>
                    <button class="btn btn-secondary" type="submit" @click="putAsignatura(asignatur.id, asignatur)"
                        v-if="bool">Actualizar</button>
                </div>
        </div>
        <br>
        <table class="table">
            <thead>
                <tr>
                    <th scope="col">id</th>
                    <th scope="col">Asignatura</th>
                    <th scope="col">Creditos</th>
                    <th scope="col">Docente</th>
                    <th scope="col">prerrequisitos</th>
                    <th scope="col">horas TA</th>
                    <th scope="col">horas TD</th>
                    <th scope="col">Acciones</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="asignatura in result" :key="asignatura.id">
                    <td>{{ asignatura.id }}</td>
                    <td>{{ asignatura.nombres }}</td>
                    <td>{{ asignatura.creditos }}</td>
                    <td>{{ asignatura.docente }}</td>
                    <td>{{ asignatura.prerrequisito }}</td>
                    <td>{{ asignatura.horasTA }}</td>
                    <td>{{ asignatura.horasTD }}</td>
                    <td>
                        <button class="btn btn-secondary" @click="editAsignatura(asignatura.id)">Editar</button>
                        <button class="btn btn-danger" @click="deleteAsignatura(asignatura.id)">Borrar</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<style>

</style>
