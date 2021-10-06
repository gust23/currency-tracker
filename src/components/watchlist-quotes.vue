<template>
  <div>
    <listQuotes :quotes="quotes" :listenQuotes="listenQuotes" @unlisten="onUnlisten" />
    <div class="mt-2 text-right">
      <cite class="text-small">
        Updates in <b>{{ nextUpdateTime }} seconds</b>
      </cite>
    </div>
  </div>
</template>
<script>
import listQuotes from './list-quotes.vue';
import { watch, onMounted, reactive, ref, toRefs, onUnmounted } from 'vue';
import api from '../services/api';
export default {
  emits: ['unlisten'],
  setup(props, { emit }) {
    const interval = ref(null);
    const quotes = ref({});
    const nextUpdateTime = ref(30);

    const methods = reactive({
      onUnlisten(code) {
        emit('unlisten', code);
      },
      async refresh() {
        const { data } = await api.listen(props.listenQuotes);
        quotes.value = data;
      },
      restartInterval() {
        clearInterval(interval.value);
        nextUpdateTime.value = 30;
        interval.value = setInterval(() => {
          nextUpdateTime.value -= 1;
          if (nextUpdateTime.value === 0) {
            nextUpdateTime.value = 30;
            this.refresh();
          }
        }, 1000);
      },
    });

    onMounted(() => {
      methods.refresh();
      methods.restartInterval();
    });

    watch(props, () => {
      methods.refresh();
      methods.restartInterval();
    });

    onUnmounted(() => {
      clearInterval(interval.value);
    });

    return { ...toRefs(methods), quotes, nextUpdateTime };
  },
  components: { listQuotes },
  props: { listenQuotes: { type: Array, required: true } },
};
</script>
<style lang=""></style>
