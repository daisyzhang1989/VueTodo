<script>
export default {
    data() {
        return {
            text: ''
        }
    },
    methods: {
        addTodo() {
            this.$emit('add-todo', {
                text: this.text,
                complete: false,
                date: new Date().toDateString(),
                dateCompleted: null
            })
            this.text = ''
        }
    }
}
</script>

<template>
    <div>
        <div class="input-group mb-3">
            <input type="text" class="form-control" v-model="text" placeholder="Add a new to do" aria-label="Add a new to do" aria-describedby="button-addon2">
            <button @click="addTodo" class="btn btn-outline-secondary" type="button" id="button-addon2">Add</button>
        </div>
    </div>
</template>