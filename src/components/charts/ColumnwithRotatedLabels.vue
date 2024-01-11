<!-- Developed by Taipei Urban Intelligence Center 2023 -->

<script setup>
import { ref } from 'vue';
import { useMapStore } from '../../store/mapStore';

const props = defineProps(['chart_config', 'activeChart', 'series', 'map_config']);
const mapStore = useMapStore();

const chartOptions = ref({
	annotations: {
        points: [{
        	seriesIndex: 0,
            label: {
                borderColor: '#775DD0',
                offsetY: 0,
                style: {
                	color: '#fff',
                	background: '#775DD0',
                },
                }
              }]
            },
			chart: {
              height: 350,
              type: 'bar',
			  toolbar: {
					show: false,
				},
            },
			dataLabels: {
              enabled: false
            },
			grid: {
					show: false,
					},
			legend: {
				show: props.chart_config.categories ? true : false,

			},
			plotOptions: {
              bar: {
                borderRadius: 10,
                columnWidth: '50%',
              }
            },
			stroke: {
              width: 2
            },
	tooltip: {
		// The class "chart-tooltip" could be edited in /assets/styles/chartStyles.css
		custom: function ({ series, seriesIndex, dataPointIndex, w }) {
			return '<div class="chart-tooltip">' +
				'<h6>' + w.globals.labels[dataPointIndex] + `${props.chart_config.categories ? '-' + w.globals.seriesNames[seriesIndex] : ''}` + '</h6>' +
				'<span>' + series[seriesIndex][dataPointIndex] + ` ${props.chart_config.unit}` + '</span>' +
				'</div>';
		},
	},
			xaxis: {
				labels: {
                	rotate: -45
              	},
              	categories: props.chart_config.categories ? props.chart_config.categories : [],   
			},
			yaxis: {
        		title: {
            		text: '發生次數',
        	},
    	},
			fill: {
              type: 'gradient',
              gradient: {
                shade: 'light',
                type: "horizontal",
                shadeIntensity: 0.25,
                gradientToColors: undefined,
                inverseColors: true,
                opacityFrom: 0.85,
                opacityTo: 0.85,
                stops: [50, 0, 100]
              },
		}
	});


const selectedIndex = ref(null);

function handleDataSelection(e, chartContext, config) {
	if (!props.chart_config.map_filter) {
		return;
	}
	const toFilter = config.dataPointIndex;
	if (toFilter !== selectedIndex.value) {
		mapStore.addLayerFilter(`${props.map_config[0].index}-${props.map_config[0].type}`, props.chart_config.map_filter[0], props.chart_config.map_filter[1][toFilter]);
		selectedIndex.value = toFilter;
	} else {
		mapStore.clearLayerFilter(`${props.map_config[0].index}-${props.map_config[0].type}`);
		selectedIndex.value = null;
	}
}
</script>

<template>
	<div v-if="activeChart === 'ColumnwithRotatedLabels'">
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