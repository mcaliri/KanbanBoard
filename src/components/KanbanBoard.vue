<template>
  <div class="kanban-board">
    <div v-for="column in columns" :key="column.id" class="kanban-column">
      <h3>{{ column.name }}</h3>
      <draggable
        v-model="column.cards"
        item-key="id"
        class="drag-area"
        :group="'cards'"
      >
        <template #item="{ element }">
          <div
            :key="element.id"
            :class="['card', `card-type-${element.type}`]"
            style="width: 18rem"
          >
            <!-- Card di tipo immagine -->
            <div v-if="element.type === 'withImage'">
              <img :src="element.imageUrl" class="card-img-top" alt="..." />
              <div class="card-body">
                <h5 class="card-title">{{ element.title }}</h5>
                <p class="card-text">{{ element.text }}</p>
                <a href="#" class="btn btn-primary">Go somewhere</a>
              </div>
            </div>
            <!-- Card di tipo semplice -->
            <div v-else-if="element.type === 'simple'">
              <div class="card-body">
                <h5 class="card-title">{{ element.title }}</h5>
                <h6 class="card-subtitle mb-2 text-body-secondary">
                  {{ element.subtitle }}
                </h6>
                <p class="card-text">{{ element.text }}</p>
                <a href="#" class="card-link">Card link</a>
                <a href="#" class="card-link">Another link</a>
              </div>
            </div>
          </div>
        </template>
      </draggable>
      <!-- Footer con bottone -->
      <div class="kanban-footer">
        <button @click="addCardToColumn(column)" class="btn btn-secondary">
          Add Card
        </button>
      </div>
    </div>
    <!-- Placeholder column for adding new columns -->
    <div class="kanban-column add-column">
      <button @click="addColumn" class="btn btn-secondary">
        Aggiungi colonna
      </button>
    </div>
  </div>
</template>

<script>
import { defineComponent, reactive } from "vue";
import draggable from "vuedraggable";

export default defineComponent({
  components: { draggable },
  setup() {
    const columns = reactive([
      {
        id: 1,
        name: "To Do",
        cards: [
          {
            id: "t1",
            title: "Task 1",
            description: "Description for Task 1",
            type: "simple",
          },
        ],
      },
      {
        id: 2,
        name: "In Progress",
        cards: [
          {
            id: "t2",
            title: "Task 2",
            description: "Description for Task 2",
            type: "withImage",
            imageUrl: "https://picsum.photos/200",
          },
        ],
      },
      {
        id: 3,
        name: "Done",
        cards: [
          {
            id: "t3",
            title: "Task 3",
            description: "Description for Task 3",
            type: "simple",
          },
        ],
      },
    ]);
    const addCardToColumn = (column) => {
      // Aggiunge una nuova card alla colonna
      const newCard = {
        id: generateUniqueId(),
        title: "New Card",
        description: "new description",
        type: "simple",
      };
      column.cards.push(newCard);
    };

    const generateUniqueId = () => {
      // Genera un id univoco
      return Date.now().toString();
    };

    const addColumn = () => {
      // Aggiunge una nuova colonna
      const newColumn = {
        id: generateUniqueId(),
        name: "New Column",
        cards: [],
      };
      columns.push(newColumn);
    };

    return { columns, addCardToColumn, addColumn };
  },
});
</script>

<style>
.drag-area {
  min-height: 100px;
}
.kanban-board {
  display: flex;
  background-color: #f4f7f6;
  padding: 20px;
  gap: 20px;
  justify-content: flex-start;
  overflow-x: auto; 
}

.kanban-column {
  flex: 0 0 350px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  padding: 20px; 
  margin: 10px; 
  min-height: 800px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.kanban-column h3 {
  font-size: 1.2em;
  font-weight: bold;
  margin-bottom: 20px;
}

.card {
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0,0,0,0.2);
  margin-bottom: 10px;
  padding: 15px;
  transition: box-shadow 0.3s ease;
}

.kanban-column .drag-area {
  padding-bottom: 20px;
}

.card:hover {
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
}

.kanban-column .kanban-footer {
  margin-top: auto;
}

.card-img-top {
  width: 100%;
  border-radius: 8px 8px 0 0;
  object-fit: cover;
}

.card-body {
  padding: 10px;
}

.card-title {
  font-size: 1em;
  font-weight: bold;
}

.card-text {
  font-size: 0.9em;
  margin-top: 10px;
}

.btn {
  margin-top: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

.btn:hover {
  background-color: #0056b3;
}

.add-column {
  width: 300px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.btn-secondary {
  background-color: #6c757d;
}
</style>
