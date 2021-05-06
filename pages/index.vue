<template>
  <div class="bg-gray-50 min-h-screen">
    <div class="max-w-lg mx-auto pt-20">
      <div class="relative flex">
        <div class="absolute inset-y-0 left-0 pl-3 flex items-center pointer-events-none">
          <svg class="h-6 w-6 text-gray-400" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
          </svg>
        </div>
        <input v-on:keyup.enter="check" type="text" v-model="name" id="name" class="rounded-md shadow-sm focus:ring-indigo-500 text-xl focus:border-indigo-500 block w-full pl-11 border-gray-300 rounded-md" placeholder="Search for pokemon">
        <button @click.prevent="reloadPokemon()" class="bg-indigo-500 focus:outline-none ml-4 text-white px-4 rounded-md hover:bg-indigo-700 transition duration-300 ease-in-out">
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 4v5h.582m15.356 2A8.001 8.001 0 004.582 9m0 0H9m11 11v-5h-.581m0 0a8.003 8.003 0 01-15.357-2m15.357 2H15" />
          </svg>
        </button>
      </div>
      <p class="text-xs mt-1">Hint: Press <b>Enter</b> to Search</p>
    </div>
    <div ref="pokemon" v-if="message == null && searchResult == null">
      <PokeCard
        :pokeData="pokeData"
        :loading="loading"
      />
    </div>
    <div ref="pokemon" v-if="searchResult">
      <SearchPokeCard
        :pokeData="searchResult"
        :loading="loading"
      />
    </div>
    <div v-if="message" class="mx-auto py-12 px-4 max-w-7xl sm:px-6 lg:px-8 lg:py-24">
      <div class="flex items-center bg-white w-full shadow px-4 py-3 rounded-md">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-indigo-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
        </svg>
        <p class="ml-2 text-gray-600">{{ message }}</p>
      </div>
    </div>
    <div v-if="message == null && searchResult == null">
      <Pagination 
        :next="next" 
        :previous="previous"
        v-on:pushPaginate="receivePaginate($event)"
        :count="count"
      />
    </div>
  </div>
</template>

<script>
import PokeCard from '@/components/Cards/PokeCard'
import SearchPokeCard from '@/components/Cards/SearchPokeCard'
import Pagination from '@/components/Partials/Pagination'

export default {
    components:{ PokeCard, Pagination,SearchPokeCard },
    data() {
      return {
        loading:true,
        next: '',
        previous:'',
        pokeData: [],
        name:'',
        offset:'',
        limit:'',
        count:null,
        searchResult:null,
        message:null
      }
    },
    methods:{

      check(){
        if(this.name.length == 0){
          return
        }
        this.searchPokemon()
      },

      async fetch() {
        this.loading = true
        let pokemons = await fetch(
          `https://pokeapi.co/api/v2/pokemon?offset=${this.offset}&limit=${this.lim1t}`
        ).then(res => res.json())
        this.pokeData = pokemons.results
        this.next = pokemons.next
        this.count = pokemons.count
        this.previous = pokemons.previous || ''
        this.loading = false

      },

      async searchPokemon() {
        this.loading = true
        this.message = null
        this.searchResult = null
        try{
          let pokeName = this.name.toLowerCase()
          let pokemon = await fetch(
            `https://pokeapi.co/api/v2/pokemon/${ pokeName }`
          ).then(res => res.json())
          this.searchResult = pokemon
          this.loading = false
        }catch(e){
          this.message = "Character not found, try another keyword."
          this.loading = false
        }

      },
      reloadPokemon(){
        this.message = null
        this.searchResult = null
        this.fetch()
      },
      receivePaginate(data){
        this.offset = data.offset
        this.lim1t = data.limit
        this.fetch()
        this.$refs.pokemon.scrollIntoView({behavior: "smooth"});
      },

    },
    mounted(){
      this.fetch()
    }
  }
</script>

<style lang="scss" scoped>

</style>