<template>
	<div class="left-panel">
		<div class="left-panel__header">
			<h3>{{ title }}</h3>
			<p>
				{{ description }}
			</p>
		</div>
		<Navbar :items="navbarItems" @update-selected-item="updateNavbarItem" />
	</div>
</template>

<script>
import Navbar from "./Navbar.vue";

export default {
	name: "LeftPanel",

	components: {
		Navbar,
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

		navbarItems: {
			type: Array,
			default: () => [],
		},
	},

	methods: {
		updateNavbarItem(item) {
			this.$emit("update-selected-item", item);
		},
	},
};
</script>

<style lang="scss">
.left-panel {
	background-color: #fff;
	border-right: 1px solid #dcdde2;
	box-shadow: 0 2px 11px rgba(0, 0, 0, 0.05);

	transition: width 0.3s ease-in-out;

	&__header {
		padding: 30px;
	}

	h3 {
		font-size: 20px;
		font-weight: 600;
		line-height: 1.5;
		color: rgb(4, 1, 28);
		margin: 30px 0 10px 0;
	}

	p {
		font-size: 14px;
		line-height: 1.2;
		color: #666;
	}
}
</style>
