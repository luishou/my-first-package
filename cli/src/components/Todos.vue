<template>
    <div id="my-todo">
        <ul class="list-group">
            <li v-for="(item, index) in todos" class="list-group-item"
                v-bind:class="{'finished' : item.finished}">

                <router-link :to="{ name: 'todo', params: { id: item.id }}">{{item.text}}</router-link>
                <button class="btn btn-warning btn-xs pull-right" v-on:click="delTodo(index, item.id)">delete</button>
                <button class="btn btn-success btn-xs pull-right"
                        v-bind:class="[item.finished ? 'btn-danger' : 'btn-success']"
                        v-on:click="toggleFinished(item)">
                    {{ item.finished ? 'cancel' : 'complete'}}
                </button>
            </li>
        </ul>
        <todo-form :todos="todos"></todo-form>
    </div>
</template>
<style>
    .finished {
        color: #5cb85c;
        text-decoration: line-through;
    }

    .margin-right-10 {
        margin-right: 10px;
    }
</style>

<script>
    import TodoForm from './TodoForm.vue'
    export default{
        name: 'my-todo',
        computed: {
            todos() {
                return this.$store.state.todos;
            }
        },
        methods: {
            delTodo: function (index, id) {
                this.axios.delete('http://homestead.app/api/todo/del/' + id).then(response => {
                    console.log(response.data);
                    this.todos.splice(index, 1);
                });
            },
            toggleFinished: function (item) {
                this.axios.patch('http://homestead.app/api/todo/update/' + item.id).then(response => {
                    console.log(response.data);
                    item.finished = !item.finished;
                });
            }
        },
        components: {
            TodoForm
        }

    }
</script>