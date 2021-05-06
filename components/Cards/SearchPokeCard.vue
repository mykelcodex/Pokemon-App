<template>
	<div>	
		<div>
			<template v-if="loading">
				<div class="mx-auto py-12 px-4 max-w-7xl sm:px-6 lg:px-8 lg:py-24">
					<div class="space-y-12 sm:grid sm:grid-cols-2 sm:gap-x-6 sm:gap-y-12 sm:space-y-0 lg:grid-cols-4 lg:gap-x-8">
						<content-placeholders
								v-for="p in 30"
								:key="p"
								rounded
								class="article-card-block"
							>
							<content-placeholders-img />
							<content-placeholders-text :lines="3" />
						</content-placeholders>
					</div>
				</div>  
			</template>
			<template v-else>
				<div class="mx-auto py-12 px-4 max-w-7xl sm:px-6 lg:px-8 lg:pt-24">
					<div class="space-y-12">
						<ul  class="space-y-12 sm:grid sm:grid-cols-2 sm:gap-x-6 sm:gap-y-12 sm:space-y-0 lg:grid-cols-4 lg:gap-x-8">
							<li>
								<div class="space-y-4">
									<div class="aspect-w-3 aspect-h-2 bg-white overflow-hidden shadow-lg rounded-lg">
										<img class="object-cover p-3" :src="getImage(pokeData.id)" :alt="pokeData.name">
									</div>

									<div class="space-y-2 bg-white shadow-md rounded-lg p-4" >
										<div class="text-lg leading-6 font-medium space-y-1">
											<h3 class="capitalize">{{ pokeData.name }}</h3>
										</div>
										<nuxt-link :to="`/${ pokeData.name }`"
										 class="flex justify-center bg-indigo-500 hover:bg-indigo-700 duration-300 ease-in-out transition 
										 w-full text-white text-sm text-center rounded-lg py-1">
											View Character
										</nuxt-link>
									</div>
								</div>
							</li>

						</ul>
					</div>
				</div>
			</template>
    </div>
	</div>
</template>

<script>
	export default {
		props:{
			pokeData:{
				type:Object
			},
			loading:{
				type:Boolean,
				default:true
			},
		},
		methods:{
			getImage(id){
				let finalURL = `https://pokeres.bastionbot.org/images/pokemon/${ id }.png`
        return finalURL;
      },
		}
	}
</script>

<style lang="scss" scoped>

</style>