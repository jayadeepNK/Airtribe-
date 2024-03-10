<!-- Typescript -->
<script>
    import {Container, Draggable} from 'vue3-smooth-dnd';
    export default {
        components: { Container, Draggable },

        props: {
            title: String,
        },
        data() {
            return {
                tasks: [], // Initialize an empty array for tasks
                isOpen: false,
                newTitle: '',
                newDesc: '',
            };
        },
        methods: {
            addTask() {
                if (this.newTitle.trim() && this.newDesc.trim()) {
                    const newTask = { // Create a new object for each task
                        title: this.newTitle,
                        desc: this.newDesc,
                    };
                    this.tasks.push(newTask);
                    this.newTitle = '';
                    this.newDesc = '';
                }
            },
            onDrop(dropResult){
                this.tasks = this.applyDrag(this.tasks, dropResult);
            },
            applyDrag(arr, dragResult){
                const { removedIndex, addedIndex, payload } = dragResult;

                if (removedIndex === null && addedIndex === null) return arr;

                const result = [...arr];
                let itemToAdd = payload;
                
                if (removedIndex !== null) {
                    itemToAdd = result.splice(removedIndex, 1)[0];
                }
                if (addedIndex !== null) {
                    result.splice(addedIndex, 0, itemToAdd);
                }
                return result;
            }
        }
    }
</script>

<!-- HTML & CSS -->
<template>
    <!-- <div class="border shadow-md rounded-md p-4 dark:shadow-gray-500 flex justify-center tasks-center h-[32rem]">{{ title }}</div> -->
    <div class="status border shadow-md rounded-md px-4 py-4 h-[34rem] flex flex-col gap-2">
        <!-- header -->
        <div class="header flex tasks-center justify-between text-xl">
            <!-- Status Name -->
            <p class="font-medium">{{ title }}</p>
            
            <!-- task count -->
            <p class="py-1 px-2.5 bg-gray-100 rounded-md dark:bg-gray-900">{{ tasks.length }}</p>

            <!-- buttons -->
            <div class="btns flex gap-4">
                <UButton @click="isOpen = true">New</UButton>
                <UModal v-model="isOpen">
                    <div class="p-14 flex flex-col gap-8 text-lg m-4">
                        <p class="text-lg text-center">Enter the title & description</p>
                        <input v-model="newTitle" type="text" placeholder="Title..." class="border rounded-md px-2 py-1 mr-2">
                        <textarea v-model="newDesc" type="text" placeholder="Description..." class="border rounded-md px-2 py-1 mr-2" rows="4"></textarea>

                        <div class="btns flex justify-between">
                            <button @click="isOpen = false; newTitle='';newDesc='' " class="text-lg px-2 py-1 rounded-md bg-blue-500 text-white hover:bg-blue-700">Discard</button>
                            <button @click="addTask();isOpen = false" class="text-lg px-2 py-1 rounded-md bg-blue-500 text-white hover:bg-blue-700">Submit</button>
                        </div>
                    </div>
                </UModal>
                <UButton>Delete</UButton>
            </div>
        </div>

        <!-- grid of tasks -->
        <div class="grid grid-cols-1 gap-2 p-4 overflow-auto">
            <!-- <template v-for="task in tasks" :key="task">
                <Task :name="task.title" :desc="task.desc" />
            </template> -->
            <Container @drop="onDrop">            
                <Draggable v-for="task in tasks">
                    <Task :name="task.title" :desc="task.desc" />
                </Draggable>
            </Container>
        </div>
    </div>
</template>