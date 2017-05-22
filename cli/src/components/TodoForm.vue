<template>
    <form v-on:submit.prevent="addTodo(newItem)">
        <div class="form-group">
            <input type="text" class="form-control" v-model="newItem.text" placeholder="add a new todo"
                   required>
        </div>
        <div class="form-group">
            <button class="btn btn-success" type="submit">Add Todo</button>
        </div>
    </form>
</template>
<script>
    export default{
        props: ['todos'],
        data() {
            return {
                newItem: {id: null, text: '', finished: false}
            }
        },
        methods: {
            addTodo: function (newItem) {
                this.axios.post('http://homestead.app/api/todo/create', {
                    text: this.newItem.text,
                }).then(response => {
                    console.log(response.data);
                    this.todos.push(response.data);
                    this.newItem = {id: null, title: '', finished: false};
                });
            }
        }
    }
</script>