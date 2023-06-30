<template>
  <div>
    <div class="ccc">
      <label for="startDate">วันที่เริ่มต้น:</label>
      <input type="date" id="startDate" v-model="startDate" @change="filterData" /> <br>
      <label for="endDate">วันที่สิ้นสุด:</label>
      <input type="date" id="endDate" v-model="endDate" @change="filterData" />
    </div>

    <canvas id="myChart"></canvas>
  </div>
</template>

<script>
import Chart from 'chart.js/auto';

export default {
  name: 'WelcomeItem',
  data() {
    return {
      startDate: '',
      endDate: '',
      chart: null,
      originalData: null,
      filteredData: null
    };
  },
  mounted() {
    const ctx = document.getElementById('myChart');
    const url = 'https://disease.sh/v3/covid-19/all';

    fetch(url)
      .then(response => response.json())
      .then(data => {
        this.originalData = data;
        this.filteredData = data;
        this.createChart(ctx, this.filteredData);
      })
      .catch(error => {
        console.log(error);
      });
  },
  methods: {
    createChart(ctx, data) {
      this.chart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: ['ประชากรทั้งหมด', 'ฟื้นตัวแล้ว', 'ผู้เสียชีวิต'],
          datasets: [{
            label: 'Covid-19 Stats',
            data: [data.population, data.recovered, data.deaths],
            backgroundColor: ['rgba(75, 192, 192, 0.2)', 'rgba(54, 162, 235, 0.2)', 'rgba(255, 99, 132, 0.2)'],
            borderColor: ['rgba(75, 192, 192, 1)', 'rgba(54, 162, 235, 1)', 'rgba(255, 99, 132, 1)'],
            borderWidth: 1
          }]
        },
        options: {
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
    },
    filterData() {
      const startDate = new Date(this.startDate);
      const endDate = new Date(this.endDate);

      this.filteredData = {
        population: this.originalData.population,
        recovered: this.originalData.recovered,
        deaths: this.originalData.deaths
      };

      this.chart.data.datasets[0].data = [this.filteredData.population, this.filteredData.recovered, this.filteredData.deaths];
      this.chart.update();
    }
  }
};
</script>

<style>
body {
  display: flex;
  justify-content: center;
}
</style>

<style>
body{
 display: flex;
 justify-content: center;

}
.ccc{
  margin-top: 25px;
  margin-bottom: 25px;
  text-align: center;
}

</style>