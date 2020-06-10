<template>
    <div class="moduleadder">
        <h1>New Module</h1>
        <label>Type <select v-model="selType" selected="">
            <option value="">Select a module</option>
            <option v-for="type in allTypes" v-bind:key="type" v-bind:value="type" >{{type[0].toUpperCase()+type.slice(1)}}</option>
        </select></label>
        <label>Width <select @change="$emit('setWidth', $event.target.value)">
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
        </select></label>
        <label>Height <select @change="$emit('setHeight', $event.target.value)">
            <option value="1">1</option>
            <option value="2">2</option>
            <option value="3">3</option>
        </select></label>
        
        <button @click="$emit('setType', selType)">Add module</button>
    </div>
</template>

<script>
export default {
    name: "ModuleAdder",
    props: ["allTypes"],
    data() {
        return {
            selType: ""
        }
    }
}
</script>

<style scoped>
.moduleadder {
    display: flex;
    flex-direction: column;
    align-items: center;
}
label {
    padding: 10px;
}
button {
    margin-top: 10px;
    font-size: 2rem
}
</style>