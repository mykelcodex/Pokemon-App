<template>
	<div>
		<div class="mx-auto px-4 max-w-7xl sm:px-6 lg:px-8 py-6" v-if="next || previous">
      <nav class="bg-white px-4 py-3 flex items-center justify-between sm:px-6 rounded-lg shadow-md overflow-hidden" aria-label="Pagination">
        <div class="hidden sm:block">
          <p class="text-sm text-gray-700">
            Total Count
            <span class="font-bold">{{ count }}</span>
          </p>
        </div>
        <div class="flex-1 flex justify-between sm:justify-end">
          <button v-if="previous" @click.prevent="paginate(previous)" class="relative inline-flex items-center px-4 py-2 border border-gray-300 text-sm font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50">
            Previous
          </button>
          <button v-if="next" @click.prevent="paginate(next)" class="ml-3 relative inline-flex items-center px-4 py-2 border border-gray-300 text-sm font-medium rounded-md text-gray-700 bg-white hover:bg-gray-50">
            Next
          </button>
        </div>
      </nav>
    </div>
	</div>
</template>

<script>
	export default {
		props:{
			next:{
				type:String,
			},
			previous:{
				type:String
			},
			count:{
				type: Number
			}
		},
		methods:{
			getParameterByName(name, url) {
				name = name.replace(/[\[\]]/g, '\\$&');
				var regex = new RegExp('[?&]' + name + '(=([^&#]*)|&|#|$)'),
						results = regex.exec(url);
				if (!results) return null;
				if (!results[2]) return '';
				return decodeURIComponent(results[2].replace(/\+/g, ' '));
			},

			paginate(url){
				let offset = this.getParameterByName('offset', url)
				let limit = this.getParameterByName('limit', url)
				this.$emit('pushPaginate', { offset:offset, limit:limit })
			}
			
		}
	}
</script>

<style lang="scss" scoped>

</style>