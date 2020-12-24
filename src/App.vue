<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h3>Flight Board</h3>
    <form id="search" class="m-3">
      <input name="query" v-model="searchQuery" placeholder="Search" class="p-1"/>
    </form>
    <data-grid
      :rows="gridData"
      :columns="gridColumns"
      :filter-key="searchQuery"
    >
    </data-grid>
  </div>
</template>

<script>
import DataGrid from './components/DataGrid'

export default {
  components:{
    'data-grid':DataGrid
  },
  data() {
    return {
      searchQuery: "",
      gridColumns: ["FlightId", "DepartureAirport","DestinationAirport","DepartureTime","ArrivalTime","FlightCode","Airline"],
      gridData: [
        { FlightId: "FL0114202020", DepartureAirport: "Istanbul Havalimanı", DestinationAirport:"Ankara Esenboğa", DepartureTime:"13:45",ArrivalTime:"14:50",FlightCode:"TK2020",Airline:"THY"},
        { FlightId: "FL0214202020", DepartureAirport: "Londra Heathrow", DestinationAirport:"Istanbul Havalimanı", DepartureTime:"08:10",ArrivalTime:"11:50",FlightCode:"TK1230",Airline:"THY"},
        { FlightId: "FL0314202020", DepartureAirport: "Sabiha Gökçen Havalimanı", DestinationAirport:"Ankara Esenboğa", DepartureTime:"20:30",ArrivalTime:"21:40",FlightCode:"PG1938",Airline:"Pegasus"},
        { FlightId: "FL0414202020", DepartureAirport: "Sabiha Gökçen Havalimanı", DestinationAirport:"Kayseri Erkilet Havalimanı", DepartureTime:"00:45",ArrivalTime:"02:00",FlightCode:"PG3838",Airline:"Pegasus"},
        { FlightId: "FL0514202020", DepartureAirport: "Londra Heathrow", DestinationAirport:"Ankara Esenboğa", DepartureTime:"10:20",ArrivalTime:"12:50",FlightCode:"TK2000",Airline:"THY"},
        { FlightId: "FL0614202020", DepartureAirport: "Ankara Esenboğa", DestinationAirport:"Berlin Tegel", DepartureTime:"10:20",ArrivalTime:"12:50",FlightCode:"TK1997",Airline:"THY"},
        { FlightId: "FL0714202020", DepartureAirport: "Istanbul Havalimanı", DestinationAirport:"Londra Heathrow", DepartureTime:"10:20",ArrivalTime:"12:50",FlightCode:"TK2005",Airline:"THY"},
        { FlightId: "FL0814202020", DepartureAirport: "Kayseri Erkilet Havalimanı", DestinationAirport:"Sabiha Gökçen Havalimanı", DepartureTime:"10:20",ArrivalTime:"12:50",FlightCode:"PG3000",Airline:"Pegasus"},
        { FlightId: "FL0914202020", DepartureAirport: "Kayseri Erkilet Havalimanı", DestinationAirport:"Istanbul Havalimanı", DepartureTime:"10:20",ArrivalTime:"12:50",FlightCode:"TK1221",Airline:"THY"},
        { FlightId: "FL1014202020", DepartureAirport: "Istanbul Havalimanı", DestinationAirport:"Berlin Tegel", DepartureTime:"10:20",ArrivalTime:"12:50",FlightCode:"TK2019",Airline:"THY"},
      ]
    }
  },
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  padding-bottom: 50px;
}

h3 {
  font-weight: normal;
  margin: 0;
}

img{
  width: 200px;
}

</style>
