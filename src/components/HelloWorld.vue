<template>
<div id="app">
          <h1 is="sui-header">Bitcoin Price Index</h1>
  <sui-card>
      <sui-card-content>
        <sui-card-header>
            <section v-if="errored">
              <p>We're sorry, we're not able to retrieve this information at the moment, please try back later</p>
            </section>

            <section v-else>
              <div v-if="loading">Loading...</div>

              <div v-else v-for="currency in info" class="currency" v-bind:key="currency">
                {{ currency.description }}:
                <span class="lighten">
                  <span v-html="currency.symbol"></span>{{ currency.rate_float | currencydecimal }}
                </span>
              </div>
            </section>
          </sui-card-header>
      </sui-card-content>
  </sui-card>

</div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',
   data () {
    return {
      info: null,
      loading: true,
      errored: false
    }
  },
  filters: {
    currencydecimal (value) {
      return value.toFixed(2)
    }
  },
  mounted () {
    axios
      .get('https://api.coindesk.com/v1/bpi/currentprice.json')
      .then(response => {
        this.info = response.data.bpi
      })
      .catch(error => {
        console.log(error)
        this.errored = true
      })
      .finally(() => this.loading = false)
  }
}
</script>

<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.ui.card{
  margin: auto;
}
</style>
