<template>
    <table class="table table-condensed table-hover">
        <thead>
            <tr>
                <th>Symbol</th>
                <th>Name</th>
                <th>Price</th>
                <th>Volume</th>
                <th>24h
                    <i class="fa fa-caret-up" aria-hidden="true"></i>
                    <i class="fa fa-caret-down" aria-hidden="true"></i>
                </th>
                <th>Last Update</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="crypto in cryptos">
                <th>{{ crypto.symbol }}</th>
                <td>{{ crypto.name }}</td>
                <td>${{ formatPrice(crypto.price_usd) }}</td>
                <td>${{ formatPrice(crypto["24h_volume_usd"]) }}</td>
                <td v-bind:class="[crypto.percent_change_24h < 0 ?
                'text-danger' : '', 'text-success']">{{
                    crypto.percent_change_24h }}%</td>
                <td>{{ formatDate(crypto.last_updated) }}</td>
            </tr>
        </tbody>
    </table>
</template>

<script>
    import moment from 'moment';
    export default {
        data() {
            return {
                cryptos: []
            }
        },

        mounted () {
            this.getCryptos();
            console.log(this.cryptos);
        },
        methods: {
            getCryptos() {
                let url = 'https://api.coinmarketcap.com/v1/ticker/?limit=20';
                delete axios.defaults.headers.common["X-Requested-With"];
                delete axios.defaults.headers.common["X-CSRF-TOKEN"];
                axios.get(url).then((response) => {
                    console.log(response);
                    this.cryptos = response.data;
                }).catch( error => { console.log(error); });
            },
            formatPrice(value) {
                let val = (value/1).toFixed(0).replace(',', '.');
                return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
            },
            formatDate(update_time) {
                return moment.unix(update_time).format("MM-DD-YYYY | hh:mm:ss");
            },
            formatDate(update_time) {
                return moment.unix(update_time).format("MM-DD-YYYY | hh:mm:ss");
            }
        },
    }
</script>
