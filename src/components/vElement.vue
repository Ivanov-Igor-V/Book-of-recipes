<template>
  <div>
    <div class="v-elem">
      <h2>{{ data.name }}</h2>
      <h3 v-if="data.grade">Оценка: {{ data.grade }}</h3>
      <my-dialog :show="divIsVisible" @update:show="hidePopup">
        <h3>
          {{ data.name }}
        </h3>
        <h4>Способ приготовления:</h4>
        <div>
          {{ data.comment }}
        </div>
        <h4>Ингредиенты:</h4>
        <div
          class="v-emem__ingr-box"
          v-for="item in data.listOfIngredients"
          :key="item.name"
        >
          <div>{{ item.name }}</div>
          <div>{{ item.quantity }}</div>
        </div>
      </my-dialog>
      <my-button @click="this.divIsVisible = !this.divIsVisible">
        Подробнее
      </my-button>
      <my-button @click="idOfRemoving(data)"> Удалить </my-button>
    </div>
    <slot> </slot>
  </div>
</template>

<script>
import myDialog from "@/UI/myDialog.vue";
import myButton from "@/UI/myButton.vue";
export default {
  components: {
    myDialog,
    myButton,
  },
  name: "v-element",
  props: {
    data: {
      type: Object,
    },
  },
  data() {
    return {
      divIsVisible: false,
    };
  },
  methods: {
    hidePopup() {
      this.divIsVisible = !this.divIsVisible;
    },
    idOfRemoving(data) {
      this.$emit("transferDishForDelete", data);
    },
  },
};
</script>

<style >
.v-elem {
  display: block;
}

.v-emem__ingr-box {
  display: flex;
  justify-content: space-between;
}
</style>