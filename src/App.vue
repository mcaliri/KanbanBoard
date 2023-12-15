<template>
 <h1 class="text-center">Kanban</h1>
<KanbanBoard></KanbanBoard>
</template>

<script>

import KanbanBoard from './components/KanbanBoard.vue';

export default {
  name: 'App',
  components: {
    KanbanBoard
}
}
</script>

<style>

</style>
