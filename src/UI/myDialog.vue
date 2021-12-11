<template>
  <div class="dialog" v-if="show" @click="hideDialog">
      <div class="dialog__content" @click.stop >
          <slot> </slot>
          <div
          @click="hideDialog"
          class="btn_close">
              <i>X</i>
          </div>
          
          
      </div>
  </div>
</template>

<script>
import myButton from '@/UI/myButton.vue'
export default {
    name: 'my-dialog',
    components: { myButton },
    props: {
        show: {
            type: Boolean
        },
    },
    data() {
        return {
            isVisible: false
        }
    },
    
    methods: {
        hideDialog() {
            this.$emit('update:show', false)
        },
    }
    }

</script>

<style scoped>
.dialog {
    top: 0;
    bottom: 0;
    right: 0;
    left: 0;
    background: rgba(0,0,0,0.5) ;
    position: fixed;
    display: flex;
    z-index: 3;
    overflow: auto;
}
::-webkit-scrollbar {
    width: 4px; /* ширина для вертикального скролла */
    height: 8px; /* высота для горизонтального скролла */
    background-color: rgba(265,265,265,0);
}

.dialog__content {
    margin: 20px;
    background: white;
    border-radius: 12px;
    min-width: 300px;
    padding: 30px;
    position: absolute;
    overflow: auto;
    align-items: center;
    display: block;
    }

  

input {
    margin: 20px;
}

.btn_close {
    width: 25px;
    height: 25px;
    background: white;
    position: absolute;
    top: 10px;
    right: 10px;
    cursor: pointer;
    border-radius: 5px;
}
</style>