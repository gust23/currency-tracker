<template>
  <div class="container grid-lg my-2 py-2">
    <div class="card mb-2" v-if="listenQuotes.length > 0">
      <div class="card-header">
        <div class="h4">Following</div>
      </div>
      <div class="card-body">
        <WatchlistQuotes :listenQuotes="listenQuotes" @unlisten="onUnlisten" />
      </div>
    </div>
    <div class="card">
      <div class="card-header">
        <div class="h4">All currencies</div>
      </div>
      <div class="card-body">
        <ListQuotes @unlisten="onUnlisten" @listen="onListen" :quotes="quotes" :listenQuotes="listenQuotes" />
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, toRefs, onMounted } from 'vue';
import api from './services/api';
import ListQuotes from './components/list-quotes.vue';
import WatchlistQuotes from './components/watchlist-quotes.vue';

export default {
  name: 'App',
  components: { ListQuotes, WatchlistQuotes },
  setup() {
    const data = reactive({
      quotes: {},
      listenQuotes: [],
    });

    onMounted(async () => {
      const res = await api.all();
      data.quotes = res.data;
      console.log(res);
    });

    const onListen = (code) => {
      data.listenQuotes.push(code);
    };

    const onUnlisten = (code) => {
      data.listenQuotes = data.listenQuotes.filter((key) => key != code);
    };

    return { ...toRefs(data), onListen, onUnlisten };
  },
};
</script>

<style></style>
