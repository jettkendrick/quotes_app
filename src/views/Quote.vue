<template>
	<div class="container">
		<div align="center">
			<input type="text" placeholder="search quotes" v-model="search" style="text-align: center; font-size: 15px;">
		</div>
		<table align="center">
			<caption>Quotes</caption>
			<caption style="font-size:15px;">click column headers to order</caption>
			<thead>
				<tr>
					<th @click="sortBy('source')">Source</th>
					<th @click="sortBy('context')">Context</th>
					<th @click="sortBy('quote')">quote</th>
					<th @click="sortBy('theme')">Theme</th>
				</tr>
			</thead>
			<tbody>
				<template v-for="quote in filteredQuotes.slice(pageNumber, limit)">
					<tr>
						<td>{{ quote.source }}</td>
						<td>{{ quote.context }}</td>
						<td>"{{ quote.quote}}"</td>
						<td>{{quote.theme}}</td>
					</tr>					
				</template>
			</tbody>
		</table>
		<p align="center">
		 <button @click="prevPage">previous</button>
		 <button @click="nextPage">Next</button>
		</p>
		<div align="center">
		 <button @click="catagorySort('all')">sort by all</button>
		 <button @click="catagorySort('games')">sort by games</button>
	   <button @click="catagorySort('movies')">sort by movies</button>
		 <button @click="catagorySort('Corpse'); imBatman('https://i1.theportalwiki.net/img/3/3d/GLaDOS_escape_00_part1_nag15-1.wav');">cake?</button>	   
		</div>
		<p align="center" style="font-weight: bold;">sort by batman </p>
		<img @click="catagorySort('Batman'); imBatman('http://www.moviesoundclips.net/movies1/batmanbegins/batman.mp3');" src="https://1000logos.net/wp-content/uploads/2016/10/Batman-logo.png" align="center">
	</div>
</template>

<style>
table {
  border-collapse: collapse;
  width: 50%;
}
caption {
	font-size: x-large;
	font-weight: bold;
	padding: 5px;
}
td {
  text-align: left;
  padding: 5px;
}
th {
	background-color: #A9A9A9;
	text-align: center;
	padding: 5px;
	cursor: s-resize;
}
th:hover {opacity: 0.7;}
table, th, td {
  border: 1px solid black;
}
input[type=text] {
	width: 25%;
	padding: 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
button {
	cursor: pointer;
	display: inline-block;
	background-color: #330000;
	font-size: 16px;
	color: white;
	opacity: 0.4;
}
button:hover {opacity: 0.9;}
img {
	text-align:center;
	display:block;
	margin: 10px auto;
	width:10%;
	height: 50px;
	cursor: pointer;
}
</style>

<script>
var axios = require("axios");
export default {
  data: function() {
    return {
      quotes: [],
      limit: 15,
      pageNumber: 0,
      search: "",
      current: 'all',
      theme: ''
    };
  },
  created: function() {
    axios.get("https://gist.githubusercontent.com/benchprep/dffc3bffa9704626aa8832a3b4de5b27/raw/quotes.json").then(response => {
      this.quotes = response.data;
    });
  },
  methods: {
    nextPage:function() {
    	if (this.quotes.length > this.limit) {
      	this.limit += 15;
      	this.pageNumber += 15;    	
    	}   
    }, 
    prevPage:function() {
    	if (this.pageNumber !== 0) {
    		this.pageNumber -= 15;
    		this.limit -= 15; 
    	}
    },
    sortBy(column) {
    	this.quotes.sort((a,b) => {
    		if (a[column] < b[column]) { 
    			return -1;
    		} else if (a[column] > b[column]) { 
    			return 1; } 
    			else {
    				return 0;
    		}		
    	});
    },
    catagorySort(catagory) {
    	this.current = catagory;
    },
    imBatman(sound) { 
    	 if (sound) {
    	var audio = new Audio(sound);
    	audio.play();
      }
    }
  },
  computed: {
  	filteredQuotes: function() {
  		var catagory = this.current;
  		var ok = [];


  		if (catagory === "all") {
  			 return this.quotes.filter((text) => {
  				return text.quote.match(this.search);
  			});
  		}  else {
  			  return this.quotes.filter((quote) => {  
  					 return quote.theme && quote.source === catagory || quote.theme === catagory;  
  				});

  			} 
  		}	
  	}
};
</script>
