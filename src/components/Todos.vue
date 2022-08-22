<template>
    <section class="todoapp">
        <header class="header">
            <h1>Todos</h1>
            <input type="text" class="new-todo" placeholder="ajouter une todo" v-model="newTodo" @keyup.enter="addTodo">
        </header>
        <div class="main">
            <input type="checkbox" class="toggle" v-model="allDone">
            <div class="todo-list">
                <li class="todo" v-for=" todo in filtredTodo"
                    :class="{ completed: todo.completed, editing: todo === editing }">
                    <div class="view">
                        <input type="checkbox" v-model="todo.completed" class="toggle">
                        <label @dblclick="editTodo(todo)">{{ todo.name }}</label>
                        <button class="destroy" @click.prevent="deleteTodo(todo)"></button>
                    </div>
                    <input type="text" class="edit" v-model="todo.name" @keyup.enter="doneEdit"
                        v-focus="todo === editing" @blur="doneEdit">
                </li>

            </div>
        </div>
        <footer class="footer" v-show="todos.length > 0">
            <span class="todo-count"><strong>{{ remaining }}</strong> tache<span>{{ plurielle }}</span> à faire</span>
            <ul class="filters">
                <li><a href="#" :class="{ selected: filter === 'all' }" @click.prevent="filter = 'all'">Toutes</a></li>
                <li><a href="#" :class="{ selected: filter === 'todo' }" @click.prevent="filter = 'todo'">à faire</a>
                </li>
                <li><a href="#" :class="{ selected: filter === 'done' }" @click.prevent="filter = 'done'"> faite</a>
                </li>
            </ul>

        </footer>

    </section>

</template>

<script>
import Vue from 'vue';



export default {
    data() {
        return {
            todos: [{
                name: "todo",
                completed: false
            }],
            newTodo: '',
            filter: 'all',
            editing: null

        }

    },
    methods: {
        addTodo() {
            this.todos.push({ name: this.newTodo, completed: false });
            this.newTodo = ''
        },
        deleteTodo(todo) {
            this.todos = this.todos.filter(t => t != todo)
        },
        editTodo(todo) {
            this.editing = todo
        },
        doneEdit() {
            this.editing = null
        }

    },
    computed: {
        remaining() {
            return this.todos.filter(todo => !todo.completed).length
        },
        plurielle() {
            let terminaisons = "s"
            if (this.remaining > 1) {
                return terminaisons
            }

        },
        filtredTodo() {
            if (this.filter === "done") {
                return this.todos.filter(todo => todo.completed)
            }
            else if (this.filter === "todo") {
                return this.todos.filter(todo => !todo.completed)
            }
            else {
                return this.todos
            }
        },
        allDone: {
            get() {
                return this.remaining === 0

            },
            set(value) {

                this.todos.map(todo => todo.completed = value)


            }
        }
    },
    directives: {
        focus(el, value) {
            if (value) {
                Vue.nextTick(() => {
                    el.focus();
                })

            }
        }
    }

}
</script>
<style src="./todo.css">
</style>