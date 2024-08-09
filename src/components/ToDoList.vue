<template>
    <div>
        <input v-model="newTodo" @keyup.enter="addTodo" placeholder="Add a new task" />
        <button @click="addTodo">Add</button>

        <div class="filters">
            <button @click="filter = 'all'" :class="{ active: filter === 'all' }">All</button>
            <button @click="filter = 'completed'" :class="{ active: filter === 'completed' }">Completed</button>
            <button @click="filter = 'active'" :class="{ active: filter === 'active' }">Active</button>
        </div>

        <ul>
            <li v-for="(todo, index) in filteredTodos" :key="index" :class="{ completed: todo.completed }">
                <input type="checkbox" v-model="todo.completed" />
                <span v-if="!todo.editing" @dblclick="editTodo(todo)">{{ todo.text }}</span>
                <input v-else v-model="todo.text" @blur="finishEdit(todo)" @keyup.enter="finishEdit(todo)" />
                <button @click="removeTodo(index)">Remove</button>
            </li>
        </ul>
    </div>
</template>

<script>
export default {
    data() {
        return {
            newTodo: '',
            todos: JSON.parse(localStorage.getItem('todos') || '[]'),
            filter: 'all'
        };
    },
    computed: {
        filteredTodos() {
            if (this.filter === 'completed') {
                return this.todos.filter(todo => todo.completed);
            } else if (this.filter === 'active') {
                return this.todos.filter(todo => !todo.completed);
            }
            return this.todos;
        }
    },
    methods: {
        addTodo() {
            if (this.newTodo.trim() !== '') {
                this.todos.push({ text: this.newTodo.trim(), completed: false, editing: false });
                this.newTodo = '';
                this.saveTodos();
            }
        },
        removeTodo(index) {
            this.todos.splice(index, 1);
            this.saveTodos();
        },
        editTodo(todo) {
            todo.editing = true;
        },
        finishEdit(todo) {
            if (todo.text.trim() === '') {
                this.removeTodo(this.todos.indexOf(todo));
            } else {
                todo.editing = false;
                this.saveTodos();
            }
        },
        saveTodos() {
            localStorage.setItem('todos', JSON.stringify(this.todos));
        }
    },
    watch: {
        todos: {
            handler() {
                this.saveTodos();
            },
            deep: true
        }
    }
};
</script>

<style scoped>
input {
    padding: 10px;
    margin-right: 20px;
}

input[type="text"] {
    width: calc(100% - 80px);
    padding: 10px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
}

button {
    padding: 10px 15px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 16px;
    background-color: #42b983;
    color: white;
}

button:hover {
    background-color: #3a9f71;
}

.filters {
    margin-top: 20px;
    margin-bottom: 20px;
}

.filters button {
    background-color: #f0f0f0;
    color: #333;
    margin-right: 5px;
}

.filters button.active {
    background-color: #42b983;
    color: white;
}

ul {
    list-style-type: none;
    padding: 0;
    margin: 0;
}

li {
    padding: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid #eee;
}

li:last-child {
    border-bottom: none;
}

.completed span {
    text-decoration: line-through;
    color: gray;
}

input[type="checkbox"] {
    margin-right: 10px;
}

li input[type="text"] {
    flex-grow: 1;
    padding: 5px;
    border: 1px solid #ccc;
    border-radius: 5px;
    font-size: 16px;
}

li button {
    background-color: #e74c3c;
}

li button:hover {
    background-color: #c0392b;
}
</style>