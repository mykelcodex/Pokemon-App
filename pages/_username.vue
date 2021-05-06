<template>
	<div>
		<div class="bg-gray-50 min-h-screen">
    	<div class="mx-auto py-12 px-4 max-w-7xl sm:px-6 lg:px-8 lg:py-24">
				<nuxt-link to="/" class="bg-indigo-500 text-white py-2 px-3 rounded-md">
					Back
				</nuxt-link>
				<template v-if="loading && pokemon == null">
					<content-placeholders :rounded="true" class="h-64 mt-10">
						<content-placeholders-img />
					</content-placeholders>
				</template>
				<template v-else>
					<div class="mt-10">
						<div class="grid grid-cols-3 gap-8">
							<div class="col-span-3 md:col-span-2 bg-white shadow-md rounded-lg p-3">
								<img :src="getImage(pokemon.id)" class="mx-auto w-80 mt-20" alt="">
							</div>
							<div class="col-span-3 md:col-span-1">
								<div class="bg-white px-4 py-4 shadow-md rounded-lg">
									<h2 class="font-bold text-xl capitalize">{{ pokemon.name }}</h2>
									<div class="mt-5">
										<div class="flex items-center">
											<div>
												Abilities: 
											</div>
											<span v-for="(ability, index) in pokemon.abilities" :key="index" class="inline-flex items-center px-2.5 py-0.5 rounded-md ml-2 text-xs font-medium bg-green-100 text-green-800">
												{{ ability.ability.name }}
											</span>
										</div>
										<div class="flex items-center mt-4">
											<div>
												Height: 
											</div>
											<p>{{ pokemon.height }}</p>
										</div>
										<div class="flex items-center mt-4">
											<div>
												Weight: 
											</div>
											<p>{{ pokemon.weight }}</p>
										</div>
										<div class="mt-10">
											<p class="font-bold mb-4">Moves</p>
											<div class="h-64 overflow-y-auto">
												<ul>
													<li v-for="(move, index) in pokemon.moves" :key="index">{{ move.move.name }}</li>
												</ul>
											</div>
										</div>
									</div>
								</div>
							</div>
							
						</div>
					</div>
				</template>	
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		
		data(){
			return{
				pokemon:null,
				loading:true
			}
		},

		methods:{
			getImage(id){
				let image = `https://pokeres.bastionbot.org/images/pokemon/${ id }.png`
        return image;
      },
			async fetchPokemon() {
        const pokemon = await fetch(
          `https://pokeapi.co/api/v2/pokemon/${ this.$route.params.username }`
        ).then(res => res.json())
        this.pokemon = pokemon
        this.loading = false

      },
		},
		
		mounted(){
			this.fetchPokemon()
		}
		
	}
</script>

<style lang="scss" scoped>

</style>