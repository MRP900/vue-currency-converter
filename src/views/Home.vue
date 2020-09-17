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
		<!-- <h3>{{ converted }}</h3> -->
		<table id="results" v-if="conversions.length > 0">
			<thead>
				<tr>
					<th>Amount / Converted From</th>
					<th>Amount / Converted to</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="conversion in conversions" v-bind:key="conversion.id">
					<td> {{ conversion.start }} / {{ conversion.from }} </td>
					<td> {{ conversion.end }} / {{ conversion.to }} </td>
				</tr>
			</tbody>
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
			let obj = {}
			obj = {
				id: this.conversions.length,
				start: money,
				end: this.converted,
				from: rateFrom,
				to: rateTo
			}
			
			this.conversions.push(obj);
			console.log(this.conversions);
			// // this.rate = rate;
			// if (rate != 'EUR') {
			// 	this.converted = this.converted / this.apiResult.rates[rate];
			// }
			
			console.log(this.converted);
		},
		clearOut() {
			this.converted = "";
			this.conversions = [];
			this.conversionId = 0;
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
			cleanData : [],
			conversions: [],
			// conversionId: 0
		}
	},
	// Get API Data
	created() {
		axios.get('http://data.fixer.io/api/latest?access_key='+apiKey).then(res => {
		
		this.apiResult = res.data;
		this.abb = Object.keys(this.apiResult.rates);
		this.rates = Object.values(this.apiResult.rates);
		
		for (let i = 0; i < this.abb.length; i++) {
			let obj = {}
			obj = {
				id: i,
				abb: this.abb[i],
				// rate: this.rates[i]
			}
			this.cleanData.push(obj);
		}
	});
      
  },
  
}

</script>

<style scoped>

#results {
	margin: 1em auto;
	width: 60%;
	
}
th, td {
	padding: .5em;
}
table {
	border: 1px solid black;
}
</style>