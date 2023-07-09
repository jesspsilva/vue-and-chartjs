<template>
	<div class="total-chart">
		<div class="total-chart__header">
			<div class="total-chart__header-content">
				<h2>{{ title }}</h2>
				<p v-if="description">{{ description }}</p>
			</div>
			<ElSelect
				v-model="selectedChartType"
				size="large"
				class="total-chart__type-select"
				popper-class="total-chart__type-select-dropdown"
			>
				<ElOption
					v-for="(chart, index) in availableChartTypes"
					:key="`chart-type-${index}`"
					:label="chart.label"
					:value="chart.label"
				/>
			</ElSelect>
		</div>
		<BarChart
			v-if="selectedChartType === 'Bar'"
			:data="barChartData"
			:options="barChartOptions"
			:height="chartHeight"
		/>
		<PieChart
			v-if="selectedChartType === 'Pie'"
			:data="pieChartData"
			:options="pieChartOptions"
			:height="chartHeight"
		/>
		<DoughnutChart
			v-if="selectedChartType === 'Doughnut'"
			:data="pieChartData"
			:options="pieChartOptions"
			:height="chartHeight"
		/>
	</div>
</template>

<script>
import BarChart from "./BarChart.vue";
import PieChart from "./PieChart.vue";
import DoughnutChart from "./DoughnutChart.vue";
import { ElSelect } from "element-plus";

export default {
	name: "TotalChart",

	components: {
		BarChart,
		ElSelect,
		PieChart,
		DoughnutChart,
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
			barChartOptions: {
				responsive: true,
				lineTension: 1,
				indexAxis: "y",
				plugins: {
					legend: {
						display: false,
					},
					tooltip: {
						usePointStyle: true,
						borderWidth: 0,
						pointStyle: "rectRounded",
						caretSize: 0,
					},
					label: {
						display: false,
					},
				},
			},
			pieChartOptions: {
				responsive: true,
				maintainAspectRatio: false,
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
					label: "Pie",
				},
				{
					label: "Doughnut",
				},
			],
			selectedChartType: "Bar",
		};
	},
	computed: {
		barChartData() {
			return {
				labels: this.labels,
				datasets: [
					{
						data: this.data.values,
						backgroundColor: this.data.backgroundColor.bar,
					},
				],
			};
		},

		pieChartData() {
			return {
				labels: this.labels,
				datasets: [
					{
						data: this.data.values,
						backgroundColor: this.data.backgroundColor.pie,
					},
				],
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
.total-chart {
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
		align-items: self-start;

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
