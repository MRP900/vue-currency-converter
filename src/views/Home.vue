<template>
	<div id="app">
		<Header />
		<Convert v-on:new-convert="convertUSD" v-on:clear-out="clearOut" v-bind:rates="this.cleanData" />
		<!-- <Result v-bind:message2="output" /> -->
		<!-- <div v-if="converted.length > 0">
			<h3>Dollar to Euro</h3>
			<h3>{{ apiResult.rates.USD }} : 1</h3>
			<h3>Dollars to {{ rate }} {{ converted }}</h3>
		</div> -->
		<h3>{{ converted }}</h3>
	</div>
</template>

<script>
import axios from 'axios';
import Convert from '../components/Convert';


export default {
	name: 'Home',
	components: {
		Convert
	},
	methods: {
		convertUSD(money, rate) {		
			this.converted = money / this.apiResult.rates.USD;
			this.rate = rate;
			if (this.rate != 'EUR') {
				this.converted *= this.apiResult.rates[this.rate];
			}
			// // this.rate = rate;
			// if (rate != 'EUR') {
			// 	this.converted = this.converted / this.apiResult.rates[rate];
			// }
			console.log(this.apiResult.rates.EUR);
			console.log(this.converted);
		},
		clearOut() {
			this.converted = "";
		}
		
	},
	data() {
		return {
			messages: {
				m1: 'Enter a Dollar amount'
			},
			apiResult: {

			},
			// euros: "",
			rate: "",
			converted : "",
			abb : [],
			rates : [],
			cleanData : []
		}
	},
	created() {
		axios.get('http://data.fixer.io/api/latest?access_key=893d6c36e2e81633bee78cd279acba84').then(res => {
		// console.log(res.data);
		// console.log(this.apiResult.rates.USD);
		// this.usdRate = this.apiResult.rates.USD;
		this.apiResult = res.data;
		this.abb = Object.keys(this.apiResult.rates);
		this.rates = Object.values(this.apiResult.rates);
		// // this.usdRate = this.apiResult.rates.USD;
		// this.keys = this.apiResult.rates;
		for (let i = 0; i < this.abb.length; i++) {
			let obj = {}
			obj = {
				id: i,
				abb: this.abb[i],
				rate: this.rates[i]
			}
			this.cleanData.push(obj);
		}
		// console.log(this.cleanData);
		// console.log(this.abb);
		// console.log(this.rates);
		// console.log(keys);
		// console.log(currencyRates);
	});
      
  },
  
}

</script>

<style scoped>
  
</style>