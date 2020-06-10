<template>
    <div class="notepad">
        <h2>Notepad</h2>
        <textarea placeholder="Write your notes here..." @input="$emit('input', $event.target.value)" v-bind:value="value"/>
    </div>
</template>

<script>
export default {
    name: "ModuleNotepad",
    props: ["value"],
}
</script>

<style scoped>
.notepad {
    text-align: center;
    display: flex;
    height: 100%;
    flex-direction: column;
}

textarea {
    font-size: 20px;
    margin: auto;
    width: calc(100%-10px);
    margin: 5px;
    margin-top: 0;
    resize: none;
    height: 100%;
}

h2 {
    margin: 2px;
    height: 26px;
}
</style>