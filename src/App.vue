<template>
    <div id="app">
        <div class="wrapper">
            <div class="main-panel">
                <div class="content">
                    <div class="row top-nav">
                        <div class="col-lg-8">
                            <div class="row">
                                <div class="col-sm-7">
                                    <div class="input-group">
                                            <input type="text" id="address-input" class="form-control search-city-input" placeholder="Wpisz nazwę miejscowości..."> 
                                    </div>
                                </div>
                                <div class="col-sm-5">
                                    <button id = "app" v-on:click="getLocation" class="btn btn-primary">Dane dla Twojej lokacji</button>
                                </div>
                            </div>
                        </div>
                        <div class="col-lg-4">
                            <div class="row">
                                <div class="col-sm-6 text-right">
                                    <button v-on:click="addFavourite" class="btn btn-primary">Dodaj do ulubionych</button>
                                </div>
                                <div class="col-sm-6 text-right">
                                    <select v-model="selected" class="form-control" v-on:change="runFavourite">
                                        <option disabled value="">Wybierz...</option>
                                        <option v-for="(favourite, index) in favourites" :key="favourite.name"  v-bind:value="index">
                                        {{ favourite.name }}
                                        </option>
                                    </select>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-lg-2">
                            <div class="card card-chart">
                                <div class="card-header">
                                    <h4 class="card-category">Pogoda</h4>
                                    <h1 class="card-title">{{ city }}, {{ country }}</h1>
                                </div>
                                <div class="card-body">
                                    <div class="water-details">
                                        <div class="row">
                                            <div class="col-sm-12">
                                                <span class="temperature">{{currentTemp}}°C</span>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="card-footer card-info">
                                    <h4 class="card-title m_title" >{{ description }}</h4>
                                </div>
                            </div>
                        </div>
                        <div class="col-lg-5">
                            <div class="card card-chart">
                                <div class="card-header">
                                    <h5 class="card-category">Prognoza temperatury na najbliższe godziny</h5>
                                </div>
                                <div class="card-body">
                                        <linechart v-bind:dates="dates" v-bind:temperature_values="temperature_values"/>
                                </div>
                            </div>
                        </div>
                        <div class="col-lg-5">
                            <div class="card card-chart">
                                <div class="card-header">
                                    <h5 class="card-category">Prognoza opadów i prędkości wiatru</h5>
                                </div>
                                <div class="card-body">
                                    <div>
                                        <mixedchart v-bind:dates="dates" v-bind:rain_values="rain_values" v-bind:wind_values="wind_values"/>          
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-lg-2">
                            <div class="card card-chart">
                                <div class="card-header">
                                    <h4 class="card-category">Pomiar zanieczyszczenia*</h4>
                                    <h1 class="card-title"> {{ installation }}</h1>
                                </div>
                                <div class="card-body">
                                    <div class="water-details pollution">
                                        <div class="row">
                                            <div class="col-6" style="font-weight: 600">PM10</div>
                                            <div class="col-6" style="font-weight: 600" v-bind:style="{color: colorPM10}">{{currentPM10}}%</div>
                                        </div>
                                        <div class="row">
                                            <div class="col-6" style="font-weight: 600">PM2.5</div>
                                            <div class="col-6" style="font-weight: 600" v-bind:style="{color: colorPM25}">{{ currentPM25}}%</div>
                                        </div>
                                    </div>
                                </div>
                                <div class="card-footer card-info">
                                    <h4 class="card-title">*Według dobowej normy WHO</h4>
                                </div>
                            </div>
                        </div>
                        <div class="col-lg-5">
                            <div class="card card-chart">
                                <div class="card-header">
                                    <h5 class="card-category">Prognoza zanieczyszczenia</h5>
                                </div>
                                <div class="card-body">
                                    <barchart v-bind:dates="dates" v-bind:pollution_one="pollution_one" v-bind:pollution_two="pollution_two" />
                                </div>
                            </div>
                        </div>
                        <div class="col-lg-5">
                            <div class="card card-chart">
                                <div class="card-header ">
                                    <div class="row">
                                        <div class="col-sm-6 text-left">
                                            <h5 class="card-category">Warunki do uprawiania sportu</h5>
                                        </div>
                                        <div class="col-sm-6">
                                            <nav>
                                                <div class="nav nav-tabs border-0" id="nav-tab" role="tablist">
                                                    <a class="nav-item nav-link active link-style" id="nav-home-tab" v-on:click="setConditions('rower')" data-toggle="tab" href="#nav-home" role="tab" aria-controls="nav-home" aria-selected="true">Rower</a>
                                                    <a class="nav-item nav-link link-style" id="nav-profile-tab" v-on:click="setConditions('bieganie')" data-toggle="tab" href="#nav-profile" role="tab" aria-controls="nav-profile" aria-selected="false">Bieganie</a>
                                                    <a class="nav-item nav-link link-style" id="nav-contact-tab" v-on:click="setConditions('street workout')" data-toggle="tab" href="#nav-contact" role="tab" aria-controls="nav-contact" aria-selected="false">Street workout</a>
                                                </div>
                                            </nav>
                                        </div>
                                    </div>
                                </div>
                                <div class="card-body">
                                    <div class="tab-content chart-area" id="nav-tabContent">
                                        <div class="tab-pane fade show active" id="nav-home" role="tabpanel" aria-labelledby="nav-home-tab">
                                            <h2 v-bind:style="{color: condition_color}">{{ conditions }}</h2>
                                            <h3>{{ warns }}</h3>
                                        </div>
                                        <div class="tab-pane fade" id="nav-profile" role="tabpanel" aria-labelledby="nav-profile-tab">
                                            <h2 v-bind:style="{color: condition_color}">{{ conditions }}</h2>
                                            <h3>{{ warns }}</h3>
                                        </div>
                                        <div class="tab-pane fade" id="nav-contact" role="tabpanel" aria-labelledby="nav-contact-tab">
                                            <h2 v-bind:style="{color: condition_color}">{{ conditions }}</h2>
                                            <h3>{{ warns }}</h3>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script src="https://cdn.jsdelivr.net/npm/chart.js@2.9.3/dist/Chart.min.js"></script>
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/places.js@1.18.1"></script>
<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>

<script>
Vue.config.silent = true;

/* importing required charting library */

import { Line } from 'vue-chartjs'
import { Bar } from 'vue-chartjs'


/* CHART COMPONENTS */
/* Weather Forecast Chart */

let linechart = Vue.component("linechart", {
    extends: Line,
    props: ['dates', 'temperature_values'],
    mounted() {
        this.renderLineChart(this.dates, this.temperature_values, this.options);
    },
    methods: {
        renderLineChart: function(x, y, options) {
            this.renderChart({
                labels: x,
                datasets: [{
                    label: 'Temperatura',
                    data: y,
                    backgroundColor: 'rgb(64, 186, 130)',
                    borderColor: 'rgb(64, 186, 130)',
                    borderWidth: 3,
                    fill: 'true'
                }]
            }, 
            {
            scales: {
                yAxes: [{
                    ticks: {
                        beginAtZero: true,
                        callback: function(value, index, values) {
                            return  value + '°';
                        }
                    }
                }]
            },
            legend:{
                display:false
            },
            responsive:true,
            maintainAspectRatio: false
            });
        }
    },
    watch: {
        temperature_values: function() {
            this.renderLineChart(this.dates, this.temperature_values, this.options);
        }
    }
});

/* Pollution Forecast Chart */

let barchart = Vue.component("barchart", {
    extends: Bar,
    props: ['dates','pollution_one', 'pollution_two'],
    mounted() {
        this.renderBarChart(this.dates, this.pollution_one, this.pollution_two);
    },
    methods: {
        renderBarChart: function(x, y1, y2) {
            this.renderChart(
                {
                    labels: x,
                    datasets: [
                        {
                            label: 'PM10',
                            data: y1,
                            borderColor: 'rgb(39, 124, 212)',
                            borderWidth: 3,
                            fill: 'true', 
                        },
                        {
                            label: 'PM25',
                                data: y2,
                                borderColor: 'rgb(64, 186, 130)',
                                fill: 'true',
                                borderWidth: 3
                        }
                    ]
                },
                {
                    scales: {
                        yAxes: [{
                            ticks: {
                                beginAtZero: true,
                            }
                        }],
                    },
                        responsive:true,
                        maintainAspectRatio: false     
                }
            )
        }
    },
    watch: {
        pollution_one: function() {
            this.renderBarChart(this.dates, this.pollution_one, this.pollution_two);
        },
    },
});

let mixedchart = Vue.component("mixedchart", {
    extends: Bar,
    props: ['dates','rain_values', 'wind_values'],
    mounted() {
        this.renderMixedChart(this.dates, this.rain_values, this.wind_values);
    },
    methods: {
        renderMixedChart: function(x, y1, y2) {
            this.renderChart(
                {
                    labels: x,
                    datasets: [
                        {
                            label: 'Opady',
                            data: y1,
                            borderColor: 'rgb(39, 124, 212)',
                            fill: 'true',
                            borderWidth: 3,
                            order: 2
                        },
                        {
                            label: 'Wiatr',
                            data: y2,
                            type: 'line',
                            borderColor: 'rgb(64, 186, 130)',
                            borderWidth: 3,
                            fill:'true',
                            order:1
                        }
                        ]
                },
                {
                    scales: {
                        yAxes: [{
                            ticks: {
                                beginAtZero: true,
                            }
                        }]
                    },
                        responsive:true,
                        maintainAspectRatio: false
                }
            );
        }
    },
    watch: {
        wind_values: function() {
            this.renderMixedChart(this.dates, this.rain_values, this.wind_values);
        }
    }
});

export default {
  name: 'App',
  components: {
      barchart: barchart,
      mixedchart: mixedchart,
      linechart: linechart
  },
data() {
    return {
        city: 'Kutno',
        country: 'Polska',
        corrds: [1,2,3],
        currentTemp: 13,
        currentPM10: 14,
        currentPM25: 17,
        colorPM10: 'green',
        colorPM25: 'green',
        installation: 'Kutno, Mikołajska',
        description:'wysokie opady deszczu',
        dates: ['13:00','16:00','19:00', '22:00', '01:00', '04:00', '07:00', '10:00'],
        rain_values: [10,50,56,70,10,15,35,89],
        wind_values: [30,14,56,23,85,94,27,42],
        temperature_values: [20,14,56,23,85,94,27,42],
        pollution_one: [20,14,12,23,13,30,27,31],
        pollution_two:[11,13,17,11,10,19,25,15],
        favourites: [],
        loc_type: 'search',
        selected:'',
        warns:'Aktualnie występują zbyt duże opady deszczu. Postaraj się zrobić trening w domu. Jak śpiewała Budka Suflera: "...a po nocy przychodzi dzień, a po burzy spokój".',
        condition_color:'red',
        conditions:'NIESPRZYJAJĄCE'
    } 
},

    mounted: function() {
        let placesAutocomplete = places({
            appId: 'plZPOBCP5Q3T',
            apiKey: 'c1f1ec07ed62d4a064179ffe3bed383a',
            container: document.querySelector('#address-input'),
            countries: ['pl'],
            type: ['city', 'address']
            });
            placesAutocomplete.on('change', e => this.runProcess(e.suggestion));

            if (localStorage.hasOwnProperty('favCities')) {
                this.favourites = JSON.parse(localStorage.getItem('favCities'))  
                } else {
                    this.favourites =[]
                } 
    },
    watch:{
        currentPM10: function(val){
            if(val > 150){
                this.colorPM10 = 'red'
            } else if (val > 60){
                this.colorPM10 = 'yellow'
            } else if(val == 'b/d'){
                this.colorPM10 = 'white'
            } else {
                this.colorPM10 = 'green'
            }
            this.setConditions('bieganie')
        },
        currentPM25: function(val){
            if(val > 150){
                this.colorPM25 = 'red'
            } else if (val > 60){
                this.colorPM25 = 'yellow'
            } else if(val == 'b/d'){
                this.colorPM25 = 'white'
            } else {
                this.colorPM25 = 'green'
            }
        }
    },
    methods: {
        fetchAirlyData: function(type,url){
            var that = this;
            var airlyXHR = new XMLHttpRequest();
            airlyXHR.onreadystatechange = function() {
                if (airlyXHR.readyState == 4 && airlyXHR.status == 200) {
                    let json_data = JSON.parse(airlyXHR.responseText);
                    if (type=='measurements'){
                        that.getPollutionData(json_data)
                    } else {
                        that.getInstaData(json_data)
                    }
                } else {
                    that.currentPM10 = 'b/d'
                    that.currentPM25 = 'b/d'
                    that.pollution_one = [],
                    that.pollution_two = []
                }
            }

            airlyXHR.open("GET", url);
            airlyXHR.setRequestHeader('Accept', 'application/json');
            airlyXHR.setRequestHeader('apikey', 'pg5noj6zcIxBn4L9rqsjK9fng71mseoT');
            airlyXHR.send()

            /*
            jq4HQHD02pGRmpMi6WfAAjQU7ND7eNGC
            ALA45rSZm5sGrZ9I4ibJUq1U21O82AKR
            pg5noj6zcIxBn4L9rqsjK9fng71mseoT
            */
        },

        fetchWeatherData: function(url){
            var that = this;
            var weatherXHR = new XMLHttpRequest();
            weatherXHR.onreadystatechange = function() {
                if (weatherXHR.readyState == 4 && weatherXHR.status == 200) {
                    let json_data = JSON.parse(weatherXHR.responseText);
                    that.getWeatherData(json_data)
                }
            }
            weatherXHR.open('GET', url );
            weatherXHR.send();
        },

        getWeatherData(json_data){
            console.log(json_data)
            this.dates = []
            this.rain_values = []
            this.temperature_values = []
            this.wind_values=[]
            for (let i = 0; i < 8; i++){
                var row = json_data.list[i]
                var rain_data = 0
                if (typeof(row.rain) != "undefined"){
                    rain_data = row.rain["3h"]
                }
                this.dates.push(row.dt_txt.substring(10,16))
                this.rain_values.push(rain_data)
                this.temperature_values.push(Math.round(row.main.temp - 273))
                this.wind_values.push(row.wind.speed)
            }
            this.currentTemp = this.temperature_values[0]
            this.description = json_data.list[0].weather[0].description
            if (this.loc_type == 'geo'){
                this.city = json_data.city.name
                this.country = json_data.city.country
            }
            console.log(this.rain_values)
        },

        getInstaData :function(json_data){
            var address = json_data[0].address
            this.installation = address.city + ", " + address.street + " " + address.number
        },

        getPollutionData: function(json_data){
            this.pollution_one = []
            this.pollution_two = []
            for (let i =2; i <24;i+=3){
                let row = json_data.forecast[i]
                this.pollution_two.push(row.standards[0].percent)
                this.pollution_one.push(row.standards[1].percent)
            }
            this.currentPM10 = Math.round(this.pollution_one[0])
            this.currentPM25 = Math.round(this.pollution_two[0])
        },

        runProcess(loc_data, type='search'){
            this.coords = [loc_data.latlng.lat, loc_data.latlng.lng]
            if (type != 'geo'){
                this.city = loc_data.name
                this.country = loc_data.country
                this.loc_type='search'
            } else {
                this.loc_type = 'geo'
            }
            
            this.fetchWeatherData(`http://api.openweathermap.org/data/2.5/forecast?lat=${loc_data.latlng.lat}&lon=${loc_data.latlng.lng}&lang=pl&appid=96e1f29b74494a9d9469860a9ff96f14`);
            this.fetchAirlyData('measurements',`https://airapi.airly.eu/v2/measurements/point?lat=${loc_data.latlng.lat}&lng=${loc_data.latlng.lng}`);
            this.fetchAirlyData('installation',`https://airapi.airly.eu/v2/installations/nearest?lat=${loc_data.latlng.lat}&lng=${loc_data.latlng.lng}&maxDistanceKM=-1.0`)
            
        },

        /* function checking geolocation and runing getCoords */
        getLocation: function() {
            if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(this.getCoords);
            } else {
            console.log("Geolocation is not supported by this browser.");
            }
        },

        /* function retreiving coordinates for geolocation and running data load */
        getCoords : function(position){
            var data_arg = {"latlng": {"lat": position.coords.latitude,
                                        "lng": position.coords.longitude}
                                }
            this.coords = [position.coords.latitude, position.coords.longitude]
            this.runProcess(data_arg, 'geo')
        },
        
        addFavourite : function() {
            if (!this.favourites) {this.favourites=[]}
            let favouriteCities = []
            let addition = {
                'name':this.city, 
                'country':this.country,
                'latlng':{'lat': this.coords[0], 'lng': this.coords[1]}
            }
            for (let i=0; i<this.favourites.length; i++) {
                favouriteCities.push(this.favourites[i].name)
            }
            if (favouriteCities.includes(this.city)){} 
            else {
                this.favourites.push(addition)
            }
            localStorage.setItem('favCities', JSON.stringify(this.favourites))
            console.log('stor', localStorage.getItem('favCities'))
        },

        runFavourite(event) {
            this.runProcess(this.favourites[event.target.value])
        },

        setConditions(activity){
            console.log(this.temperature_values[0],this.wind_values[0],this.rain_values[0])
            if (this.currentPM10!="b/d"){
                var pol_avg = (this.currentPM10 + this.currentPM25) /2
            } else {
                var pol_avg = 0
            }
            var ideal_vals = [20, 20, 0] 
            var values = [this.temperature_values[0],this.wind_values[0],this.rain_values[0]]
            var tab = [Math.abs(ideal_vals[0] - values[0]), 
                        Math.abs(ideal_vals[1] - values[1])*0.2,
                        Math.abs(ideal_vals[2] - values[2])*3]
            var average = tab.reduce(function(a,b){return a+b})
            var check_vals = {
                'bieganie': [0, 30, 13, 25],
                'rower':[0, 8, 15, 27],
                'street workout': [0, 7, 16 , 28 ]
            }
            console.log('avg',average)
            if (average <= check_vals[activity][1] && pol_avg < 60){
                this.condition_color = 'dark green'
                this.conditions = 'BARDZO DOBRE'
            } else if(average <= check_vals[activity][2] && pol_avg < 100){
                this.condition_color = 'green'
                this.conditions ='DOBRE'
            } else if (average <= check_vals[activity][3] && pol_avg < 120){
                this.condition_color = 'yellow'
                this.conditions ='UMIARKOWANE'
            } else {
                this.condition_color = 'red'
                this.conditions ='NIESPRZYJAJĄCE'
            }
            this.setWarns(activity)
        },

        setWarns(activity){
            this.warns = ''
            if(this.temperature_values[0] < 5 || this.temperature_values[0] > 25 ){
                this.warns += 'Temperatura nie sprzyja aktywności fizycznej - zwróć uwagę na odpowiedni strój. ' 
            }
            if(this.currentPM10 > 100 || this.currentPM25 > 100){
                this.warns += 'Uważaj na mocno zanieczyszczne powietrze. Jeśli możesz - zostań w domu. '
            } else if(this.currentPM10 > 60 || this.currentPM25 > 60){
                this.warns += 'Powietrze jest zanieczyszczone - sugerowane jest założenie maski antysmogowej. '
            }
            if(this.rain_values[0] > 4){
                this.warns += 'Aktualnie występują zbyt duże opady deszczu. Postaraj się zrobić trening w domu. Jak śpiewała Budka Suflera: "...a po nocy przychodzi dzień, a po burzy spokój".'
            } else if(this.rain_values[0] > 0.5){
                this.warns += 'Występują umiarkowane opady deszczu - zaopatrz się w nieprzemakalną odzież i gorącą herbatę po powrocie.'
            }
            if (this.wind_values[0] > 60){
                this.warns += 'Występuje zbyt silny wiatr - nie wychodź z domu i zaczekaj na zmianę warunków. '
            }
            if (this.temperature_values[0] > 15 && this.temperature_values[0] < 25 && this.rain_values[0] == 0 && this.wind_values[0] < 40 && this.warns.length==0){
                this.warns += 'Świetna pogoda do uprawiania sportu - i nie tylko! Zabierz swoich bliskich na zewnątrz i do woli korzystajcie z walorów przyrody. Kto wie, kiedy przydarzy się kolejna okazja...'
            }
        }
    }
}
</script>