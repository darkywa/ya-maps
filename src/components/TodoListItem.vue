<template lang="pug">

    div.app-logs__response-item
        span.app-logs__item-date {{ todo.dayNow }}/{{ todo.monthNow }} в {{ todo.timeNow }}
        span(v-if="todo.status === 'success'") {{ todo.pointA }} - {{ todo.pointB }} = 
          span.app-logs__item-km {{ todo.result }} км
        span(v-else-if="todo.status === 'error'" class="text-danger") Возникла ошибка! Подробности в консоли.

</template>

<script>
export default {
    props: {
        todo: {
          type: Object,
          required: true
        }
    }
}
</script>

<style lang="less">
    .app-logs__response-item {
        font-style: italic;

        .app-logs__item-date {
            font-weight: bold;
            margin-right: 5px;
        }
    }
</style>