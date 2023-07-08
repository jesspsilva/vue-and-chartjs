<template>
	<nav class="navbar">
		<ul>
			<li
				v-for="item in items"
				:key="item.id"
				class="navbar__item"
				:class="{
					'navbar__item--active': selectedItemId === item.id,
				}"
				:aria-disabled="selectedItemId === item.id"
				@click="selectItem(item.id)"
			>
				{{ item.label }}
			</li>
		</ul>
	</nav>
</template>

<script>
export default {
	name: "Navbar",

	data() {
		return {
			selectedItemId: null,
		};
	},

	props: {
		items: {
			type: Array,
			required: true,
		},
	},

	created() {
		this.selectedItemId = this.items[0].id;
	},

	methods: {
		selectItem(id) {
			this.selectedItemId = id;

			this.$emit("update-selected-item", this.selectedItemId);
		},
	},
};
</script>

<style lang="scss">
.navbar {
	ul {
		padding: 0;
		margin: 0;
	}

	&__item {
		list-style: none;
		margin: 0;
		padding: 10px 0 10px 30px;

		border-bottom: 1px solid #f1ecec;

		font-size: 14px;
		line-height: 1.2;
		font-weight: 500;
		color: #9c9999;
		text-align: left;

		cursor: pointer;
	}

	&__item:not(.navbar__item--active):hover {
		text-decoration: underline;
	}

	&__item--active {
		color: #000;
		font-weight: 600;
	}
}
</style>
