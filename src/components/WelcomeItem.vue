<template>
  <div>
    <canvas id="myChart"></canvas>
  </div>
</template>

<script>
import Chart from 'chart.js/auto';

export default {
  name: 'WelcomeItem',
  mounted() {
    const ctx = document.getElementById('myChart');
    const url = 'https://disease.sh/v3/covid-19/all';

    fetch(url)
      .then(response => response.json())
      .then(data => {
        const myChart = new Chart(ctx, {
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
      })
      .catch(error => {
        console.log(error);
      });
  }
};
</script>


<style>
body{
 display: flex;
 justify-content: center;

}


</style>