<template>
<div class="btn_container" > 
    <button class="btn">
      <slot>
      </slot>
    </button>

</div>

</template>

<script>
export default {
name: 'my-button'
}
</script>

<style>
.btn_container {
    display: flex;
    justify-content: center;
    
}

.btn {
    display: block;
    margin-top: 8px;
    margin: 3px;
    padding: 3px;
    color: rgb(129, 70, 184);
    border: solid;
    border-radius: 10px;
    width: 150px;
    height: 40px;
}
</style>