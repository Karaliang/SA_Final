<!-- Developed by Taipei Urban Intelligence Center 2023 -->

<script setup>
import { ref, computed } from 'vue';
import { useMapStore } from '../../store/mapStore';

const props = defineProps(['chart_config', 'activeChart', 'series', 'map_config']);
const mapStore = useMapStore();

const chartOptions = ref({
	chart: {
          type: 'bar',
          height: 350
        },
		plotOptions: {
          bar: {
            horizontal: false,
            columnWidth: '55%',
            endingShape: 'rounded'
          },
        },
		dataLabels: {
          enabled: false
        },
		stroke: {
          show: true,
          width: 2,
          colors: ['transparent']
        },
		xaxis: {
          categories: ['Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct'],
        },
		yaxis: {
          title: {
            text: '$ (thousands)'
          }
        },
		fill: {
          opacity: 1
        },
		tooltip: {
          y: {
            formatter: function (val) {
              return "$ " + val + " thousands"
            }
          }
        }


});



</script>

<template>
	<div v-if="activeChart === 'BasicChart'">
		<apexchart type="bar" height="350" :options="chartOptions" :series="series"
			@dataPointSelection="handleDataSelection"></apexchart>
	</div>
</template>