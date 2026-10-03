<template>
  <div class="container">
    <h1>Cotizador de Criptomonedas</h1>

    <Grid>
      <formulario @info-monedas="obtener" />
      <Data :cripto="info.cripto" :moneda="info.moneda" :img="info.img" :precio="info.precio"/>
    </Grid>
  </div>
</template>

<script>
import Formulario from "./components/Formulario.vue";
import Data from "./components/Data.vue";
import Grid from "./components/Grid.vue";

export default { 
  components: { Formulario, Data, Grid }, 

  data: () => ({
    info:{
      cripto: "*",
      moneda: "*",
      img: "",
      precio: 0,
    }
  }),

  methods: {
  async obtener(cripto, moneda) {
    //"API key required, please refer to the documentation at https://developers.coindesk.com/" ==> es la nueva direccion sugerida por la vieja api presentada por el profesor en los videos

    //https://docs.coingecko.com/ ==> es la alternativa 
    //cripto = "bitcoin";
    //moneda = "usd";
    const url2 = `https://api.coingecko.com/api/v3/simple/price?ids=${encodeURI(cripto)}&vs_currencies=${encodeURI(moneda)}`;
    const url = `https://api.coingecko.com/api/v3/coins/markets?vs_currency=${encodeURI(moneda.toLowerCase())}&symbols=${encodeURI(cripto.toLowerCase())}`;
    //const res = await fetch(`https://min-api.cryptocompare.com/data/pricemultifull?fsyms=${encodeURI(cripto)}&tsysm=${encodeURI(moneda)}`);
    const res = await fetch(url);
    const data = await res.json()
    console.log(data)

    this.info.cripto = cripto;
    this.info.moneda = moneda;
    this.info.precio = data[0].current_price;
    this.info.img = data[0].image;

    /*//const data = await res.json();
    const {RAW} = await res.json();
    console.log(res);
    //const dataCripto = data.RAW[cripto];
    const dataCripto = RAW[cripto];
    //const dataMoneda = dataCripto[moneda];
    const data = dataCripto[moneda];
    console.log(data);

    this.info.cripto = cripto;
    this.info.moneda = moneda;
    this.info.img = data.IMGURL;
    this.info.precio = data.PRICE;*/
    
  }
  }

};
</script>
