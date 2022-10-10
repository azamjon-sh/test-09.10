<template>
  <div class="bg-gray-100 p-6 min-h-screen">
    <div class="rounded-md p-4 bg-white">
      <form class="">
        <div class="rounded-md border-2 py-2 5 px-3">
          <label for="teg" class="text-xs block font-medium">Тикер</label>
          <input type="text" placeholder="Например DOGE" class="border-0 outline-0 text-base placeholder-gray-500"
                 id="teg">
        </div>
        <button class="rounded-md px-6 py-3.5 bg-purple-900 mt-3 text-white font-medium">Добавить</button>
      </form>
    </div>
    <div class="rounded-md border-2 py-2 5 px-4 mt-4 flex items-center w-96 bg-white">
      <label for="teg" class="text-xs font-medium mr-2.5 w-6 h-6 flex items-center">
        <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg"
             class="inline-block ">
          <path
              d="M17.5 17.5L12.5 12.5M14.1667 8.33333C14.1667 11.555 11.555 14.1667 8.33333 14.1667C5.11167 14.1667 2.5 11.555 2.5 8.33333C2.5 5.11167 5.11167 2.5 8.33333 2.5C11.555 2.5 14.1667 5.11167 14.1667 8.33333Z"
              stroke="#9CA3AF" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </label>
      <input type="text" placeholder="Например DOGE" class="border-0 outline-0 text-base placeholder-gray-500"
             id="teg">
    </div>

    <div class="grid gap-4 grid-cols-3 mt-4">
      <div class="rounded-md border-2 py-4 5 px-5 bg-white" v-if="myDataResult">
        <div class="text-sm block font-medium flex justify-between">
          <div class="text-sm font-medium">{{ myDataResult.FSYM }}</div>
          <svg width="20" height="20" viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path
                d="M15.8335 5.83333L15.1107 15.9521C15.0484 16.8243 14.3227 17.5 13.4483 17.5H6.55203C5.67763 17.5 4.9519 16.8243 4.8896 15.9521L4.16683 5.83333M8.3335 9.16667V14.1667M11.6668 9.16667V14.1667M12.5002 5.83333V3.33333C12.5002 2.8731 12.1271 2.5 11.6668 2.5H8.3335C7.87326 2.5 7.50016 2.8731 7.50016 3.33333V5.83333M3.3335 5.83333H16.6668"
                stroke="#9CA3AF" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </div>
        <div class="text-4xl">{{ myDataResult.P }}$</div>
      </div>
    </div>
  </div>

</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      key: '938cc61ebaa57b5c8df41da31178841c07b07d3210aa5235f87347ac1d416422',
      connection: null,
      myDataResult: null,
    }
  },
  methods: {
    getCrypto() {
      let self = this
      let apiKey = "938cc61ebaa57b5c8df41da31178841c07b07d3210aa5235f87347ac1d416422";
      let ccStreamer = new WebSocket('wss://streamer.cryptocompare.com/v2?api_key=' + apiKey);
      ccStreamer.onopen = function onStreamOpen() {
        let subRequest = {
          "action": "SubAdd",
          "subs": ["2~Coinbase~BTC~USD"]
        };
        ccStreamer.send(JSON.stringify(subRequest));
      }
      ccStreamer.onmessage = function onStreamMessage(event) {
        self.myDataResult = JSON.parse(event.data);
        console.log(self.myDataResult)
      }
      ccStreamer.onclose = function onStreamClose(event) {
        console.log('Closed')
      }
    }
  },
  mounted() {
    this.getCrypto()

  },
  components: {}
}
</script>

<style lang="scss">
</style>
