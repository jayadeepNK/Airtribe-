<!-- JS -->
<script>
    export default {
        props: {
            name: String,
            desc: String
        }
    }
</script>

<!-- HTML & CSS -->

<template>
    <div class="card flex flex-col gap-4 p-4 border shadow-md h-min-content my-2">
        <p class="text-xl font-medium">{{ name }}</p>
        <p class="text-justify">{{ desc }}</p>
    </div>
</template>