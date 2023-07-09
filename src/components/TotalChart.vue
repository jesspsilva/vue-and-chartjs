<template>
	<div class="total-chart">
		<div class="total-chart__header">
			<div class="total-chart__header-content">
				<h2>{{ title }}</h2>
				<p v-if="description">{{ description }}</p>
			</div>
		</div>
		<BarChart
			:data="barChartData"
			:options="barChartOptions"
			:height="chartHeight"
		/>
	</div>
</template>

<script>
import BarChart from "./BarChart.vue";

export default {
	name: "TotalChart",

	components: {
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
		};
	},
	computed: {
		barChartData() {
			return {
				labels: this.labels,
				datasets: [
					{
						data: this.data,
						backgroundColor: "#d1da7e",
						borderColor: "#d1da7e",
					},
				],
			};
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
