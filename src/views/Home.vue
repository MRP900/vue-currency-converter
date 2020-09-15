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
		<table id="results">
				<tr>
					<th>
						Amount Converted
					</th>
					<th>
						Rate
					</th>
					<th>
						Currency Converted to
					</th>
				</tr>		
		</table>
	</div>
</template>

<script>
import axios from 'axios';
import Convert from '../components/Convert';
import * as data from '../api.json';
const apiKey = data.default.key;
export default {
	name: 'Home',
	components: {
		Convert
	},
	methods: {
		convertUSD(money, rateFrom, rateTo) {		
			this.converted = money / this.apiResult.rates[rateFrom];
			this.rate = rateFrom;
			if (this.rate != 'EUR') {
				this.converted *= this.apiResult.rates[rateTo];
				
			}
			this.converted = this.converted.toFixed(2);
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
			rateFrom: "",
			converted : "",
			abb : [],
			rates : [],
			cleanData : []
		}
	},
	// Get API Data
	created() {
		// console.log(data);
		// console.log(data.default.key);
		axios.get('http://data.fixer.io/api/latest?access_key='+apiKey).then(res => {
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
				// rate: this.rates[i]
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

#results {
	margin: 1em auto;
	
}
th td {
	padding: .5em;
}
table th td {
	border: 1px solid black;
}
</style>