<template>
    <div class="container">
        <div class="row">
            <div class="col">
                <!-- <table class="table is-striped is-bordered mt-2 is-fullwidth">
                    <thead>
                        <tr>
                            <th>ID</th>
                            <th>Contrato</th>
                            <th>Monto</th>
                            <th>Fecha</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="item in items" :key="item.id_con">
                            <td>{{ item.id_con }}</td>
                            <td>{{ item.nombre_con }}</td>
                            <td>{{ item.monto_con }}</td>
                            <td>{{ item.fecha_con }}</td>
                        </tr>
                    </tbody>
                </table> -->
            </div>
            <div class="col">
                <div class="field">
                    <label class="label">Desde</label>
                    <div class="control">
                        <input class="input" type="date" placeholder="Desde" v-model="desde" />
                    </div>
                </div>

                <div class="field">
                    <label class="label">Hasta</label>
                    <div class="control">
                        <input class="input" type="date" placeholder="Hasta" v-model="hasta" />
                    </div>
                </div>

                <div class="control">
                    <button class="button is-success" @click="filtrar()">Buscar</button>
                </div>
                <div v-if="contracts.length > 0">
                    <table class="table is-striped is-bordered mt-2 is-fullwidth">
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Contrato</th>
                                <th>Total</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="item in contracts" :key="item.id_con">
                                <td>{{ item.id_con }}</td>
                                <td>{{ item.nombre_con }}</td>
                                <td>{{ item.monto_con }}</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script>
// import axios
import axios from "axios";

export default {
    name: "FilterContracts",
    data() {
        return {
            data: [0],
            items: [],
            contracts: [],
            desde: "",
            hasta: "",

        };
    },
    created() {
        this.getContracts();
    },
    methods: {
        async getContracts() {
            try {
                const response = await axios.get("http://localhost:5000/contracts");
                this.items = response.data;
            } catch (err) {
                console.log(err);
            }
        },
        // Create New product
        async filtrar() {
            try {
                let data = [];
                data.push(this.desde);
                data.push(this.hasta);
                const response2 = await axios.post("http://localhost:5000/filter/" + data);
                // this.desde = "";
                // this.hasta = "";
                let cont = response2.data;
                let conArray = [];
                let aux = [];
                //console.log(typeof cont);
                for (let i = 0; i < cont.length; i++) {
                    conArray = [];
                    let montoTotal = 0;
                    for (let j = 0; j < cont.length; j++) {
                        if (cont[i].id_cli == cont[j].id_cli) {
                            montoTotal += cont[j].monto_con;
                        }
                    }
                    conArray.push(cont[i].id_cli, montoTotal);
                    console.log(conArray);
                    aux.push(conArray);
                }
                console.log(aux);
                this.contracts = response2.data;
            } catch (err) {
                console.log(err);
            }
        },
    },
};
</script>
  
<style>

</style>