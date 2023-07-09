<template>
	<div class="comparison-chart">
		<div class="comparison-chart__header">
			<div class="comparison-chart__header-content">
				<h2>{{ title }}</h2>
				<p v-if="description">{{ description }}</p>
			</div>
			<ElSelect
				v-model="selectedChartType"
				size="large"
				class="comparison-chart__type-select"
				popper-class="comparison-chart__type-select-dropdown"
			>
				<ElOption
					v-for="(chart, index) in availableChartTypes"
					:key="`comparison-chart-type-${index}`"
					:label="chart.label"
					:value="chart.label"
				/>
			</ElSelect>
		</div>
		<RadarChart
			v-if="selectedChartType === 'Radar'"
			:data="chartData"
			:options="radarChartOptions"
			:height="chartHeight"
		/>
		<BarChart
			v-if="selectedChartType === 'Bar'"
			:data="chartData"
			:options="barChartOptions"
			:height="chartHeight"
		/>
	</div>
</template>

<script>
import BarChart from "./BarChart.vue";
import RadarChart from "./RadarChart.vue";

export default {
	name: "ComparisonChart",

	components: {
		RadarChart,
		BarChart,
	},

	props: {
		title: {
			type: String,
			default: "",
		},

		description: {
			type: String,
			default: "",
		},

		labels: {
			type: Array,
			required: true,
		},

		data: {
			type: Array,
			required: true,
		},

		chartHeight: {
			type: Number,
			default: 200,
		},
	},

	data() {
		return {
			radarChartOptions: {
				responsive: true,
				plugins: {
					legend: {
						position: "bottom",
						align: "start",
					},
					tooltip: {
						usePointStyle: true,
						borderWidth: 0,
						pointStyle: "rectRounded",
						caretSize: 0,
					},
				},
			},
			barChartOptions: {
				responsive: true,
				lineTension: 1,
				indexAxis: "y",
				plugins: {
					legend: {
						position: "bottom",
						align: "start",
						labels: {
							usePointStyle: true,
							borderWidth: 0,
							pointStyle: "rectRounded",
						},
					},
					tooltip: {
						usePointStyle: true,
						borderWidth: 0,
						pointStyle: "rectRounded",
						caretSize: 0,
					},
				},
			},
			chartTypes: [
				{
					label: "Bar",
				},
				{
					label: "Radar",
				},
			],
			selectedChartType: "Radar",
		};
	},
	computed: {
		chartData() {
			return {
				labels: this.labels,
				datasets: this.data,
			};
		},

		availableChartTypes() {
			return this.chartTypes.filter(
				(type) => type.label !== this.selectedChartType
			);
		},
	},
};
</script>

<style lang="scss" scoped>
.comparison-chart {
	background: #fff;
	border: 1px solid #fff;
	border-radius: 4px;
	box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.1);

	padding: 20px;

	&__type-select {
		max-width: 120px;
	}

	&__type-select-dropdown .el-select-dropdown__item {
		font-family: Avenir, Helvetica, Arial, sans-serif;
	}

	&__header {
		display: flex;
		justify-content: space-between;
		align-items: center;

		margin-bottom: 20px;

		&-content {
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: flex-start;
		}

		h2 {
			padding: 0;
			margin: 0;
			color: #2f2e2e;
		}

		p {
			font-size: 14px;
			line-height: 1.43;
			color: #666;
			letter-spacing: -0.3px;
			padding: 0;
			margin: 0;
		}
	}
}
</style>
