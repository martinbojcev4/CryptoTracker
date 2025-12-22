<script setup>

import { useCoinsStore } from "../store/coinsStore";
import {ref,computed} from 'vue'

const coinStore = useCoinsStore();
const sortedBy = ref(null)

const sortedCoins = computed(()=>{
    if(!sortedBy.value){
        return coinStore.coinDataTop50;
    }
    if(sortedBy.value==='change'){
        return [...coinStore.coinDataTop50].sort((a,b)=>b.changePercent24Hr-a.changePercent24Hr)
    }else if(sortedBy.value==='volume'){
        return [...coinStore.coinDataTop50].sort((a,b)=>b.volumeUsd24Hr-a.volumeUsd24Hr)
    }


})

</script>
<template>
    <div class="bg-[#1B2028] w-full rounded-[10px] p-[20px] mt-[20px] mb-[15px] mx-auto max-w-7xl">
        <h1 class="text-white font-bold text-3xl">Live Market</h1>
        
        <div>
            <button @click="sortedBy=null" class="px-4 py-2 rounded bg-gray-700 text-white hover:bg-gray-600">
                Return to normal
            </button>
            <button @click="sortedBy = 'change'" class="px-4 py-2 rounded bg-gray-700 text-white hover:bg-gray-600">
                Sort by Change %
            </button>
            <button @click="sortedBy = 'volume' " class="px-4 py-2 rounded bg-gray-700 text-white hover:bg-gray-600">
                Sort by Volume 24h
            </button>

        </div>
        
        <div class="grid grid-cols-5 gap-4 mt-[20px] text-gray-400">
            <p>Coin</p>
            <p>Change</p>
            <p>Market Cap</p>
            <p>24h Volume</p>
            <p>Price</p>
        </div>
        <div class="max-h-[400px] overflow-y-scroll">
          <div v-for="coin in sortedCoins" :key="coin.id" class="grid grid-cols-5 gap-4 mt-[20px] text-gray-300">
            <p>{{ coin.name }}</p>
            <p  :class="(coin.changePercent24Hr) < 0 ? 'font-bold text-red-500' : 'font-bold text-[#1ECB4F]'">{{ Number(coin.changePercent24Hr).toFixed(2)}}%</p>
            <p>${{ Number(coin.marketCapUsd).toFixed(0) }}M</p>
            <p>${{ Number(coin.volumeUsd24Hr).toFixed(0)}}M</p>
            <p>${{ Number(coin.priceUsd).toFixed(4) }}</p>
        </div>
        </div>

        <div>

        </div>
    </div>
</template>

<style scoped>
button {
    margin: 10px;
}
</style>
