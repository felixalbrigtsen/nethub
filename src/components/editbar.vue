<template>
  <div class="EditBar">
      <button @click="removeTile(tileKey)">Remove Tile</button>
  </div>
</template>

<script>
export default {
    name: "EditBar",
    props: ["removeTile","tileKey"]
}
</script>

<style scoped>
.EditBar {
    height: 20px;
    width: 100%;
    background-color: gray;

    border-top-right-radius: 10px;
    border-top-left-radius: 10px;
   
    text-align: center; 
    transition: 1s visibility, 0s display 1s;
    
    visibility: visible;
    opacity: 1;
    transition: 1s opacity, 1s visibility, display 1s 1s;
}

.EditBar > button {
    background-color: red;
    color: white;
}

.hidden {
  visibility: collapse;
  opacity: 0;
  transition: 1s opacity, 1s visibility 1s;
}

</style>