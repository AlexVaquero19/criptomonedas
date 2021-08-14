<template>

  <div class="container mt-5">
    <div class="row">
      <h1>Coin Market</h1>

      <input type="text" class="form-control rounded my-4" placeholder="Buscar..." @keyup="buscarMoneda()" v-model="textoBuscado" />

      <table class="table table-light">
        <thead>
          <tr>
            <th v-for="titulo in titulos" :key="titulo">
              {{titulo}}
            </th>
          </tr>
        </thead>

        <tbody class="align-middle">
          <tr v-for="(coin, index) in filterCoins" :key="coin.id">
            <td class="text-muted text-center align-middle">
              {{index += 1}}
            </td>
            <td>
              <img :src="coin.image">
              <span class="ms-2">{{coin.name}}</span>
              <span class="ms-2 simbolo text-uppercase text-muted">{{coin.symbol}}</span>
            </td>
            <td>
              {{coin.current_price}} €
            </td>
            <td :class="[
              coin.price_change_percentage_24h > 0 
                ? 'text-success' 
                : 'text-danger']
              ">
              {{coin.price_change_percentage_24h}} %
            </td>
            <td>
              {{coin.total_volume.toLocaleString()}} €
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>

</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      coins: [],
      filterCoins: [],
      titulos: ['#','Moneda','Precio','Precio Cambiado','24H Volumen'],
      textoBuscado: ''
    };
  },

  async mounted() {
    const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=eur&order=market_cap_desc&per_page=100&page=1&sparkline=false');
    const data = await res.json();
    
    this.coins = data;
    this.filterCoins = data;
  },

  methods: {
    buscarMoneda() {
      this.filterCoins = this.coins.filter((coin) => 
        coin.name.toLowerCase().includes(this.textoBuscado.toLowerCase()) || 
        coin.symbol.toLowerCase().includes(this.textoBuscado.toLowerCase()) 
      );
    }
  }
}

</script>

<style>
  img{
    width: 50px;
    height: auto;
  }
</style>
