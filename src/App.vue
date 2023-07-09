<template>
	<div class="dashboard">
		<LeftPanel
			title="Data Data Visualization"
			description="Et Lorem incididunt esse velit consequat commodo magna irure duis
					Lorem deserunt. Tempor veniam esse deserunt fugiat id officia irure
					dolor nulla id duis reprehenderit minim. Nostrud labore ullamco
					excepteur laboris cillum tempor aute voluptate adipisicing nulla.
					Dolor pariatur incididunt irure pariatur veniam ipsum proident.
					Exercitation labore"
			:navbar-items="navbarItems"
			@update-selected-item="updateItem"
			class="dashboard__left-panel"
		/>
		<div class="dashboard__content">
			<div class="dashboard__comparison_charts" v-if="selectedItem === 1">
				<div class="dashboard__charts">
					<TotalChart
						title="Pokemon types"
						description="Number of pokemons by type"
						:labels="pokemonTypesLabels"
						:data="pokemonTypesData"
					/>
					<TotalChart
						title="Pokemon abilities"
						description="Number of pokemons by ability"
						:labels="pokemonAbilitiesLabels"
						:data="pokemonAbilitiesData"
					/>
				</div>
				<div class="dashboard__charts">
					<ComparisonChart
						v-if="pokemonStatsData.length"
						title="Pokemon stats"
						description="Comparison of pokemons by stats"
						:labels="pokemonStatsLabels"
						:data="pokemonStatsData"
					/>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import LeftPanel from "./components/LeftPanel.vue";
import TotalChart from "./components/TotalChart.vue";
import ComparisonChart from "./components/ComparisonChart.vue";
import { chartsLabelsColors } from "./utils/charts-labels-colors.js";

export default {
	name: "App",

	components: {
		LeftPanel,
		TotalChart,
		ComparisonChart,
	},

	data() {
		return {
			navbarItems: [
				{
					id: 1,
					label: "Pokemon comparison",
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
			pokemonsAPIData: [],
			pokemonTypes: {},
			pokemonAbilities: {},
			pokemonStatsLabels: [],
			pokemonStatsData: [],
		};
	},

	async created() {
		await this.fetchPokemons();
	},

	computed: {
		pokemonTypesData: function () {
			return {
				values: Object.values(this.pokemonAbilities),
				backgroundColor: {
					bar: "#d1da7e",
					pie: this.pokemonTypesGraphColors,
				},
			};
		},

		pokemonTypesLabels: function () {
			return Object.keys(this.pokemonTypes).map(
				(type) => type[0].toUpperCase() + type.slice(1)
			);
		},

		pokemonAbilitiesData: function () {
			return {
				values: Object.values(this.pokemonAbilities),
				backgroundColor: {
					bar: "#a2cb96",
					pie: this.pokemonAbilitiesGraphColors,
				},
			};
		},

		pokemonTypesGraphColors: function () {
			return this.selectRandomColors(this.pokemonTypesLabels.length);
		},

		pokemonAbilitiesGraphColors: function () {
			return this.selectRandomColors(this.pokemonAbilitiesLabels.length);
		},

		pokemonAbilitiesLabels: function () {
			return Object.keys(this.pokemonAbilities).map(
				(ability) => ability[0].toUpperCase() + ability.slice(1)
			);
		},

		radarGraphBackgroundColor: function () {
			return this.selectRandomColors(this.pokemonsAPIData.results.length);
		},
	},

	methods: {
		updateItem(id) {
			this.selectedItem = id;
		},

		async fetchPokemons() {
			try {
				const response = await fetch(
					"https://pokeapi.co/api/v2/pokemon?limit=20&offset=0"
				);

				this.pokemonsAPIData = await response.json();

				const pokemonsPromises = this.pokemonsAPIData.results.map((pokemon) =>
					this.fetchSinglePokemonData(pokemon.url)
				);

				const pokemonsData = await Promise.all(pokemonsPromises);

				const treatedPokemonData = this.formatPokemonsData(pokemonsData);
				this.pokemonTypes = treatedPokemonData.types;
				this.pokemonAbilities = treatedPokemonData.abilities;
				this.pokemonStatsLabels = treatedPokemonData.statsLabels;
				this.pokemonStatsData = treatedPokemonData.stats.map((stat, index) => {
					return {
						label: stat.label,
						data: stat.data,
						backgroundColor: this.radarGraphBackgroundColor[index],
						borderColor: this.radarGraphBackgroundColor[index],
						pointBackgroundColor: this.radarGraphBackgroundColor[index],
						pointBorderColor: "#fff",
						pointHoverBackgroundColor: "#fff",
						pointHoverBorderColor: this.radarGraphBackgroundColor[index],
					};
				});
			} catch (error) {
				console.error("Error fetching Pokémon data:", error);
			}
		},

		async fetchSinglePokemonData(url) {
			try {
				const response = await fetch(url);
				return await response.json();
			} catch (error) {
				console.error("Error fetching Pokémon data:", error);
				throw error;
			}
		},

		formatPokemonsData(pokemonsAPIData) {
			const pokemonsTypes = {};
			const pokemonsAbilities = {};
			const pokemonsStatsLabels = [];
			const pokemonsStatsValues = [];

			pokemonsAPIData.forEach((pokemon) => {
				pokemon.types.forEach((type) => {
					if (pokemonsTypes[type.type.name]) {
						pokemonsTypes[type.type.name] += 1;
					} else {
						pokemonsTypes[type.type.name] = 1;
					}
				});

				pokemon.abilities.forEach((ability) => {
					if (pokemonsAbilities[ability.ability.name]) {
						pokemonsAbilities[ability.ability.name] += 1;
					} else {
						pokemonsAbilities[ability.ability.name] = 1;
					}
				});

				pokemon.stats.forEach((stat) => {
					const statName =
						stat.stat.name[0].toUpperCase() + stat.stat.name.slice(1);
					if (!pokemonsStatsLabels.includes(statName)) {
						pokemonsStatsLabels.push(statName);
					}
				});

				pokemonsStatsValues.push({
					label: pokemon.name[0].toUpperCase() + pokemon.name.slice(1),
					data: pokemon.stats.map((stat) => stat.base_stat),
				});
			});

			return {
				types: pokemonsTypes,
				abilities: pokemonsAbilities,
				statsLabels: pokemonsStatsLabels,
				stats: pokemonsStatsValues,
			};
		},

		selectRandomColors(count) {
			const colorsCopy = [...chartsLabelsColors];

			for (let i = colorsCopy.length - 1; i > 0; i--) {
				const j = Math.floor(Math.random() * (i + 1));
				[colorsCopy[i], colorsCopy[j]] = [colorsCopy[j], colorsCopy[i]];
			}

			return colorsCopy.slice(0, count);
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
	background-color: #f7f7f7f7;
}

h1,
h2,
h3,
h4,
p {
	padding: 0;
	margin: 0;
	text-align: left;
}

.dashboard {
	display: flex;
	height: 100vh;

	&__left-panel {
		flex: 0 0 350px;
	}

	&__content {
		width: 100%;
		margin: 30px;

		overflow: scroll;
	}

	&__charts {
		margin-top: 30px;
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
		gap: 30px;
	}
}
</style>
