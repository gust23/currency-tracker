<template>
  <table class="table">
    <thead>
      <tr>
        <th>Code</th>
        <th>Name</th>
        <th>Max</th>
        <th>Min</th>
        <th>Variation</th>
        <th></th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(quote, key) in quotes" :key="key">
        <td>{{ key }}</td>
        <td>{{ quote.name }}</td>
        <td>{{ quote.high }}</td>
        <td>{{ quote.low }}</td>
        <td>
          <span
            :class="{ 'label-error': quote.pctChange < 0, 'label-success': quote.pctChange > 0 }"
            class="label label-rounded text-small"
          >
            {{ quote.pctChange }} %
          </span>
        </td>
        <td>
          <a
            v-if="!listenQuotes.includes(key)"
            class="btn btn-primary btn-sm tooltip-right"
            data-tooltip="Follow"
            @click="$emit('listen', key)"
          >
            <i class="icon icon-plus"></i>
          </a>
          <a
            @click="$emit('unlisten', key)"
            v-else
            data-tooltip="Remove"
            class="btn btn-error btn-sm tooltip tooltip-right"
          >
            <i class="icon icon-minus"></i>
          </a>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: { quotes: { type: Object, required: true }, listenQuotes: { type: Array, required: true } },
  emits: ['listen', 'unlisten'],
};
</script>
