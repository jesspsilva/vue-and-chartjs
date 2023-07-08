<template>
	<div class="dashboard">
		<div class="dashboard__left-panel">
			<div class="dashboard__left-panel-header">
				<h3>Data Visualization</h3>
				<p>
					Et Lorem incididunt esse velit consequat commodo magna irure duis
					Lorem deserunt. Tempor veniam esse deserunt fugiat id officia irure
					dolor nulla id duis reprehenderit minim. Nostrud labore ullamco
					excepteur laboris cillum tempor aute voluptate adipisicing nulla.
					Dolor pariatur incididunt irure pariatur veniam ipsum proident.
					Exercitation labore
				</p>
			</div>
			<Navbar :items="navbarItems" @update-selected-item="updateItem" />
		</div>
		<div class="dashboard__content"></div>
	</div>
</template>

<script>
import Navbar from "./components/Navbar.vue";

export default {
	name: "App",

	data() {
		return {
			navbarItems: [
				{
					id: 1,
					label: "Item 1",
				},
				{
					id: 2,
					label: "Item 2",
				},
				{
					id: 3,
					label: "Item 3",
				},
			],
			selectedItem: 1,
		};
	},

	components: {
		Navbar,
	},

	methods: {
		updateItem(id) {
			this.selectedItem = id;
		},
	},
};
</script>

<style lang="scss">
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
}

body {
	margin: 0;
	padding: 0;
	text-align: left;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
}

.dashboard {
	display: flex;
	height: 100vh;

	&__left-panel {
		width: 350px;
		background-color: #fff;
		border-right: 1px solid #dcdde2;
		box-shadow: 0 2px 11px rgba(0, 0, 0, 0.05);

		transition: width 0.3s ease-in-out;

		&-header {
			padding: 30px;
		}

		h3 {
			font-size: 20px;
			font-weight: 600;
			line-height: 1.5;
			color: rgb(4, 1, 28);
			text-align: left;
		}

		p {
			font-size: 14px;
			line-height: 1.2;
			color: #666;
			text-align: left;
		}
	}
}
</style>
