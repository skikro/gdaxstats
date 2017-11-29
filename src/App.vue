<template>
    <div id="app">
        <h1>GDAX Stats</h1>
        <button type="button" @click.prevent="fetchData">Update!</button>
        <table>
            <tr>
                <th>Currency</th>
                <th>Current Price</th>
            </tr>
            <tr>
                <td>ETH</td>
                <td>{{ethPrice | dollar}}</td>
            </tr>
            <tr>
                <td>BTC</td>
                <td>{{btcPrice | dollar}}</td>
            </tr>
        </table>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data () {
            return {
                ethPrice: 1000000,
                btcPrice: 200000
            }
        },
        filters: {
            dollar: function(val) {
                return "$" + parseFloat(Math.round(val * 100) / 100).toFixed(2);
            }
        },
        methods: {
            fetchData(){
                this.$http.get('https://api.gdax.com/products/ETH-USD/ticker')
                    .then(response => {
                        return response.json();
                    })
                    .then(data => {
                        this.ethPrice = data.price;
                    });
                this.$http.get('https://api.gdax.com/products/BTC-USD/ticker')
                    .then(response => {
                        return response.json();
                    })
                    .then(data => {
                        this.btcPrice = data.price;
                    });
            }
        }
    }
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
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
</style>
