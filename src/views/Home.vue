<template>
  <div class="home">
    <h1>Control Forestal Api Test</h1>
    <form @submit.prevent="handleFormSubmit">
      <div class="form-group">
        <label for="limit">limit:</label>
        <input v-model="filters.limit" type="number">
      </div>
      <div class="form-group">
        <label for="offset">offset:</label>
        <input v-model="filters.offset" type="number">
      </div>
      <div class="form-group">
        <label for="search">search:</label>
        <input v-model="filters.search" type="text">
      </div>
      <button type="submit">Fetch Api Data</button>
    </form>
    <h1>Total Trozos: {{ trozos.length }}</h1>
    <table>
      <thead>
        <tr>
          <th>Codigo</th>
          <th>Diametro</th>
          <th>Largo</th>
          <th>m3</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="trozo in trozos" :key="trozo.id">
          <td>{{ trozo.codigo_completo }}</td>
          <td>{{ trozo.diametro }}</td>
          <td>{{ trozo.number_largo }}</td>
          <td>{{ trozo.total }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'Home',
  data() {
    return {
      filters: {
        limit: 10,
        offset: 0,
        search: '',
        status: '',
        codigoOrigen: '',
        largo: '',
        diametro: '',
        order: {
          by: 'codigo',
          dir: 'asc'
        }
      },
      apiResponse: {}
    };
  },
  computed: {
    trozos() {
      return this.apiResponse.hasOwnProperty('data') ? this.apiResponse.data : [];
    }
  },
  methods: {
    handleFormSubmit() {
      let trozosApiEndpoint = '/api/trozos';
      let queryString = `?limit=${this.filters.limit}&offset=${this.filters.offset}&search=${this.filters.search}&status=${this.filters.status}&codigoOrigen=${this.filters.codigoOrigen}&largo=${this.filters.largo}&diametro=${this.filters.diametro}&order-by=${this.filters.order.by}&order-dir=${this.filters.order.dir}&token=${process.env.VUE_APP_API_TOKEN}`;
      fetch(process.env.VUE_APP_API_URL + trozosApiEndpoint + queryString)
        .then(response => response.json())
        .then(data => {
          this.apiResponse = data;
        });
    }
  }
};
</script>
<style>
  form {
    display: flex;
  }
  .form-group {
    flex: 1;
  }
  .form-group label {
    margin-right: 5px;
  }
  table {
    width: 100%;
  }
</style>
