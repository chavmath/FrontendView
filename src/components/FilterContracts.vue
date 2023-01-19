<template>
    <div>
        <table class="table is-striped is-bordered mt-2 is-fullwidth">
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
                    <td>{{ item.fech_con }}</td>
                </tr>
            </tbody>
        </table>
        <div class="field">
            <label class="label">Desde</label>
            <div class="control">
                <input class="input" type="date" placeholder="Desde" />
            </div>
        </div>

        <div class="field">
            <label class="label">Hasta</label>
            <div class="control">
                <input class="input" type="date" placeholder="Hasta" />
            </div>
        </div>

        <div class="control">
            <button class="button is-success" @click="filtrar()">Buscar</button>
        </div>
        <div  v-if="data.length > 0">
            holaa
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
            contracts: []
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
                let data = [0, 1];
                await axios.post("http://localhost:5000/filter/" + data, {
                    product_name: this.productName,
                    product_price: this.productPrice,
                });
                this.productName = "";
                this.productPrice = "";
                this.$router.push("/");
            } catch (err) {
                console.log(err);
            }
        },
    },
};
</script>
  
<style>

</style>