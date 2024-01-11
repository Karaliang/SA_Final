<!-- Developed by Taipei Urban Intelligence Center 2023 -->

<script setup>
import { ref, computed } from 'vue';
import { useMapStore } from '../../store/mapStore';

const props = defineProps(['chart_config', 'activeChart', 'series', 'map_config']);
const mapStore = useMapStore();

const chartOptions = ref({
	chart: {
          type: 'bar',
          height: 350,
		  toolbar: {
					show: false,
				},
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
		grid: {
             show: false,
            },
		stroke: {
          show: true,
          width: 2,
          colors: ['transparent']
        },
		tooltip: {
		custom: function ({ series, seriesIndex, dataPointIndex, w }) {
			// The class "chart-tooltip" could be edited in /assets/styles/chartStyles.css
			return '<div class="chart-tooltip">' +
				'<h6>' +  `  ${w.globals.seriesNames[seriesIndex]}` + '</h6>' +
				'<span>' + series[seriesIndex][dataPointIndex] + ` ${props.chart_config.unit}` + '</span>' +
				'</div>';
			},
		},

		xaxis: {
          categories: ['一', '二', '三', '四', '五', '六', '日'],
        },
		yaxis: {
          title: {
            text: '發生件數'
          },
        },
		fill: {
          opacity: 1
        },



});



</script>

<template>
	<div v-if="activeChart === 'BasicChart'">
		<apexchart
			width="100%"
			height="270px"
			type="bar"
			:options="chartOptions"
			:series="series"
			@dataPointSelection="handleDataSelection"
		></apexchart>
	</div>
</template>