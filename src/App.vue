<template>
    <div class="main_div">
        <img class="mb-10 border border-white" src="./components/images/logo-9fb5691f.jpg" alt="" srcset="">
        <form @submit.prevent="add">
            <div class="main_grid">
                <div class="col-span-8">
                    <div class="flex flex-col">
                        <span class="main_label">TODO</span>
                        <input autocomplete="off" v-model="new_item" id="todo_input" placeholder="Enter your todo here" type="text"
                            class="main_input text-black">
                    </div>
                </div>
                <div class="col-span-4 flex items-end">
                    <todo_button />
                </div>
            </div>
        </form>
        <div class="main_display">
            <div v-if="items.length == 0" class="text-center">
                <span>Nothing to display.</span>
            </div>
            <div v-else v-for="(item, index) in items" :key="item.id" id="todo-display" class="px-2 mb-1">
                <div class="flex justify-between items-center mb-2">
                    <label class="font-bold text-base">TODO #{{ item.id }}</label>
                    <span class="remove" :key="item.id" @click="deleteEvent(item.id)">×</span>
                </div>
                <div class="flex justify-between flex-col mb-1">
                    <span role="button" @click="editTodo(index)" v-if="editingIndex !== index">{{ item.name }}</span>
                    <span v-else>
                        <input placeholder="Enter your todo here" ref="inputField" class="main_input w-full " type="text"
                            v-model="item.name" :style="inputStyles(index)"
                            @keyup.enter="onKey(index, item.name)" autocomplete="off">
                        <small class="italic text-xs">Press enter to update</small>
                    </span>
                    <small class="text-end font-semibold text-sm">{{ item.date }}</small>
                </div>
                <hr class="line">
            </div>
        </div>
    </div>
</template>

<script>
import todo_button from './components/TodoButton.vue'

export default {
    components: {
        todo_button,
    },

    data() {
        return {
            count: 0,
            new_item: '',
            editingIndex: -1,
            items: [],
            edit: false,
        }
    },
    methods: {
        // add todo
        add() {
            if (this.new_item) {
                // get current date and time
                const dateTime = new Date().toLocaleString();
                // add to the array
                this.items.push(
                    {
                        name: this.new_item,
                        id: this.items.length ? this.items[this.items.length - 1].id + 1 : 1,
                        date: dateTime
                    }
                );
                this.new_item = '';
                return true;
            }
            // validation for no input
            alert('Please enter a todo!')
        },
        // remove todo from the list
        deleteEvent(id) {
            this.items = this.items.filter(todo => todo.id !== id);
        },
        // update todo name
        editTodo(index) {
            this.editingIndex = index;
            this.$nextTick(() => this.$refs.inputField[index].focus());
        },
        // when pressing enter 
        onKey(index, name) {
            if (name == '') {
                // validation for no input
                alert('Please enter a todo!')
            } else {
                this.editingIndex = -1;
            }
        },
        // change css style
        inputStyles(index) {
            return {
                backgroundColor: this.editingIndex === index ? 'white' : 'transparent',
                color: this.editingIndex === index ? 'black' : 'white'
            };
        }
    },

}
</script>
