<template>
  <div>
    <div class="ccc">
      <label for="startDate">วันที่เริ่มต้น:</label>
      <input type="date" id="startDate" v-model="startDate" @change="filterData" />
      <label for="endDate">วันที่สิ้นสุด:</label>
      <input type="date" id="endDate" v-model="endDate" @change="filterData" />
    </div>

    <table class="bbb">
      <thead>
        <tr>
          <th>ประเทศ</th>
          <th>ทวีป</th>
          <th>ประชากร</th>
          <th>ฟื้นตัวแล้ว</th>
          <th>ผู้เสียชีวิต</th>
          <th>ผู้ติดเชื้อวันนี้</th>
          <th>ผู้ตายวันนี้</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="country in filteredCountries" :key="country.country">
          <td style="text-align: center;">{{ country.country }}</td>
          <td style="text-align: center;">{{ country.continent }}</td>
          <td style="text-align: center;">{{ country.population }}</td>
          <td style="text-align: center;">{{ country.recovered }}</td>
          <td style="text-align: center;">{{ country.deaths }}</td>
          <td style="text-align: center;">{{ country.todayCases }}</td>
          <td style="text-align: center;">{{ country.todayDeaths }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue';
import axios from 'axios';

const countriesData = ref([]);
const startDate = ref('');
const endDate = ref('');

onMounted(async () => {
  try {
    const response = await axios.get('https://disease.sh/v3/covid-19/countries');
    countriesData.value = response.data;
  } catch (error) {
    console.log(error);
  }
});

const filteredCountries = computed(() => {
  if (startDate.value && endDate.value) {
    const start = new Date(startDate.value);
    const end = new Date(endDate.value);
    return countriesData.value.filter(country => {
      const date = new Date(country.updated);
      return date >= start && date <= end;
    });
  }
  return countriesData.value;
});

function filterData() {
  if (startDate.value && endDate.value) {
    const start = new Date(startDate.value);
    const end = new Date(endDate.value);
    filteredCountries.value = countriesData.value.filter(country => {
      const date = new Date(country.updated);
      return date >= start && date <= end;
    });
  } else {
    filteredCountries.value = countriesData.value;
  }
}
</script>

<style>
body{
 display: flex;
 justify-content: center;

}
#content{
  margin-left: 50px;
  margin-top: 100px;
  width: 100%;
  height: auto;
  padding: 0 130px 0 150px;
  justify-content: center;
  display: flex;
 
}
.ccc{
  margin-top: 25px;
  margin-bottom: 25px;
  text-align: center;
}

</style>