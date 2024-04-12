<script setup lang="ts">
import { ref, onMounted, computed } from 'vue';
import { getPrimary, getSecondary } from '@/utils/UpdateColors';
import axios from 'axios';
/*Chart*/

const chartData = ref([1, 2, 3, 4, 5, 6, 7, 8, 9, 0]);

function rescaleValues(value: any) {
	if (value >= 1 && value <= 1.99) {
		return (Math.log2(value) * 10 - 10).toFixed(2);
	} else if (value >= 2 && value <= 10000) {
		return (Math.log2(value) * 3.32).toFixed(2);
	}
	return value; // Return the original value if it doesn't fit the specified ranges
}

const props = defineProps({
	total: {
		type: Number,
		required: true
	}
});

const fetchData = () => {
	axios
		.get('http://149.56.95.160:3000/get-graph-data', {
			params: { total: props.total }
		})
		.then((response) => {
			chartData.value = response.data.data.map((element: number) => element / 100).reverse();
		})
		.catch((error) => {
			console.error('Error fetching data:', error);
		});
};

const chartOptions = computed(() => {
	return {
		grid: {
			show: false,
			borderColor: 'transparent',
		},
		plotOptions: {
			bar: {
				horizontal: false,
				columnWidth: '80%',
				borderRadius: 0,
				colors: {
					ranges: [
						{
							from: 0,
							to: 2,
							color: '#f44336'
						},
						{
							from: 2,
							to: 10,
							color: '#76ff03'
						},
						{
							from: 10,
							to: 100,
							color: '#ffee33'
						},
						{
							from: 100,
							to: 100000,
							color: '#dd33fa'
						}
					]
				}
			}
		},
		// colors: [getPrimary.value,getSecondary.value],
		fill: { type: 'solid', opacity: 1 },
		chart: {
			type: 'bar',
			height: 320,
			// offsetX: -15,
			toolbar: { show: false },
			foreColor: '#adb0bb',
			fontFamily: 'Poppins',
			sparkline: { enabled: false }
		},
		dataLabels: { enabled: false },
		markers: { size: 0 },
		legend: { show: false },
		xaxis: {
			categories: chartData.value.map((_, index) => index + 1),
			labels: {
				style: { cssClass: 'grey--text lighten-2--text fill-color' }
			}
		},
	};
});
const chartOptionsVisual = computed(() => {
	return {
		chart: {
			type: 'bar',
			height: 320
		},
		plotOptions: {
			bar: {
				colors: {
					ranges: [
						{
							from: 0,
							to: 10,
							color: '#76ff03'
						},
						{
							from: 10,
							to: 100,
							color: '#ffee33'
						},
						{
							from: -10,
							to: -5,
							color: '#F15B46'
						}, {
							from: -5,
							to: 0,
							color: '#FEB019'
						}]
				},
				columnWidth: '80%',
			}
		},
		dataLabels: {
			enabled: false,
		},
		xaxis: {
			categories: chartData.value.map((_, index) => index + 1),
			labels: {
				style: { cssClass: 'grey--text lighten-2--text fill-color' }
			}
		},
	}
});
const Chart = computed(() => {
	return {
		series: [{ name: 'Bang', data: chartData.value }]
	};
});

const ChartVisual = computed(() => {
	return {
		series: [{ name: 'Bang', data: chartData.value.map(value => rescaleValues(value)) }]
	};
});

const elementVisible = ref(false);
onMounted(() => {
	fetchData();
	setTimeout(() => (elementVisible.value = true), 10);
	setInterval(fetchData, 4000);
});
</script>

<template>
	<!-- ------------------------------------ -->
	<!-- html -->
	<!-- ------------------------------------ -->
	<VCard elevation="10">
		<v-card-text>
			<div class="d-sm-flex align-center">
				<div>
					<h3 class="text-h5 title mb-1">Sales Overview</h3>
					<h5 class="text-subtitle-1">Ample Admin Vs Pixel Admin</h5>
				</div>
				<div class="ml-auto">
					<div class="d-flex align-center">
						<div class="d-flex align-center px-2">
							<span class="text-primary">
								<span class="text-overline">
									<i class="mdi mdi-brightness-1 mx-1"></i>
								</span>
								<span class="font-weight-regular">Ample</span>
							</span>
						</div>
						<div class="d-flex align-center px-2">
							<span class="text-secondary">
								<span class="text-overline">
									<i class="mdi mdi-brightness-1 mx-1"></i>
								</span>
								<span class="font-weight-regular">Pixel Admin</span>
							</span>
						</div>
					</div>
				</div>
			</div>
			<div v-show="elementVisible" class="mt-5">
				<apexchart type="bar" height="320" :options="chartOptions" :series="Chart.series"></apexchart>
				<apexchart type="bar" height="320" :options="chartOptionsVisual" :series="ChartVisual.series"></apexchart>
			</div>
		</v-card-text>
	</VCard>
</template>
