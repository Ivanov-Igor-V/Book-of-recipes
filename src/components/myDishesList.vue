<template>
  <div class="dishes_main">
    <div class="container">
      <h2>Испробованные рецепты</h2>
      <v-element
        v-for="dish in triedDishes"
        class="list"
        :key="dish.id"
        :data="dish"
        @transferDishForDelete="removePost"
      >
      </v-element>
    </div>

    <div class="container">
      <h2>Запланированные рецепты</h2>
      <v-element
        v-for="dish in unTriedDishes"
        class="list"
        :key="dish.id"
        @transferDishForDelete="removePostFromUntried"
        :data="dish"
      >
        <div class="button_wrapper">
          <br />
          <my-button @click="gradeDialogOpen(dish)"> Приготовлено! </my-button>
        </div>
      </v-element>
    </div>

    <div>
      <my-button @click="addDish()">Добавить блюдо</my-button>
    </div>
    <my-dialog class="container" v-model:show="dialogOfAdd">
      <h3>Добавить блюдо:</h3>
      <new-object class="container" @newDishArray="newDish" />
    </my-dialog>
    <div>
      <my-dialog class="container" v-model:show="dialogOfTranser">
        <div>
          <div>
            <h3>Поставьте оценку блюду</h3>
            <select
              name="grade"
              id="gradeSelect"
              required
              v-model="this.gradeEnter"
            >
              <option value="1">1</option>
              <option value="2">2</option>
              <option value="3">3</option>
              <option value="4">4</option>
              <option value="5">5</option>
              <option value="6">6</option>
              <option value="7">7</option>
              <option value="8">8</option>
              <option value="9">9</option>
              <option value="10" selected>10</option>
            </select>
            <my-button @click="gradeAffixing(this.dishKeeper)">
              Подтвердить
            </my-button>
          </div>
        </div>
      </my-dialog>
    </div>
  </div>
</template>

<script>
import myButton from "@/UI/myButton.vue";
import myDialog from "@/UI/myDialog.vue";
import NewObject from "@/components/NewObject.vue";
import vElement from "@/components/vElement.vue";
export default {
  name: "my-dishes-list",
  components: { myButton, myDialog, NewObject, vElement },
  data() {
    return {
      triedDishes: [
        {
          name: "Цезарь",
          grade: 9,
          id: 1,
          comment: `Промыть, просушить и нарвать на небольшие кусочки листья салата, 
        отложить в холодильник.В горячую сковородку положить 1 столовую ложку сливочного масла. После того, как оно 
        полностью расплавится и начнет шипеть, кинуть нарезанный на пластины зубчик чеснока.
        Куриную грудку нарезать на кусочки приблизительно 1×3 см. Положить в сковороду к чесноку и маслу. 
        Обжаривать на сильном огне приблизительно 10 минут до румяной корочки. Снять с огня.
        В ту же сковородку добавить еще одну столовую ложку сливочного масла и второй зубчик чеснока. В это время нарезать
        на небольшие кубики хлеб. Положить в сковороду и обжаривать до румяной корочки. Желательно непрерывно помешивать,
        чтобы не подгорело.Сковорода с керамическим покрытием. Достать листья салата, туда же положить обжаренную куриную грудку,
        помидоры, нарезанные тонкой соломкой. Заправить соусом «Цезарь». Перемешать. Сверху положить получившиеся сухарики и натереть сыр.`,
          listOfIngredients: [
            { name: "Зеленый салат", quantity: "1 пучок" },
            { name: "Помидоры", quantity: "1шт." },
            { name: "Куриное филе", quantity: "300г" },
            { name: "Соус «Цезарь»", quantity: "по вкусу" },
            { name: "Сливочное масло", quantity: "2 ст. ложки" },
            { name: "Чеснок", quantity: "2 зубчика" },
            { name: "Сыр пармезан", quantity: "по вкусу" },
          ],
        },

        {
          name: "Греческий салат",
          grade: 8,
          id: 2,
          comment: `В небольшой банке смешайте оливковое масло, лимонный сок, измельченный чеснок, соль, перец и орегано. Закройте крышкой
        и хорошо встряхните, чтобы все перемешалось. В большую миску выложите нарезанные небольшими дольками помидоры, нарезанный полукруглыми 
        кусочками огурец, нарезанный тонкими кольцами красный лук, нарезанный тонкими полосками зеленый перец, нарезанную кубиками фету и маслины.
        Перед подачей полейте заправкой, аккуратно перемешайте и посыпьте сверху свежемолотым черным перцем.`,
          listOfIngredients: [
            { name: "Оливковое масло", quantity: "3 ст. ложки" },
            { name: "Лимонный сок", quantity: "1,5 ст. ложки" },
            { name: "Чеснок", quantity: "2 зубчика" },
            { name: "Помидоры", quantity: "3шт." },
            { name: "Красный лук", quantity: "0,25 ч.л." },
            { name: "Огурцы", quantity: "0.5 шт." },
            { name: "Сыр фета", quantity: "120г" },
            { name: "Маслины без косточек", quantity: "16шт." },
          ],
        },
      ],
      unTriedDishes: [
        {
          name: "Сельдь под шубой",
          grade: "",
          id: 1,
          comment: `Обернуть картошку, свеклу и морковь фольгой, выложить на противень и отправить в духовку, разогретую до 200 градусов. 
        Запекать до мягкости, картошке и моркови понадобится около 40 минут, а свекле час.В кипящую воду положить яйца и варить их 11 минут.
         Затем охладить, очистить и натереть на крупной терке. Картошку и морковь очистить и нарезать мелким кубиком, таким же кубиком нарезать
          филе сельди, лук и яблоки нарезать чуть мельче. Свеклу натереть на крупной терке.Выложить салат на блюдо слоями, промазывая каждый майонезом. 
          Вот рекомендуемый порядок продуктов: картошка, селедка, лук, яблоки, яйца, морковь, свекла. Завершающий слой свеклы также смазать майонезом. 
          Готовый салат отправить в холодильник на пару-тройку часов, чтобы дать ему пропитаться. Перед подачей украсить салат зеленью.`,
          listOfIngredients: [
            { name: "Филе сельди", quantity: "300 г" },
            { name: "Картофель", quantity: "300 г" },
            { name: "Морковь", quantity: "300 г" },
            { name: "Свекла", quantity: "500 г" },
            { name: "Куриное яйцо", quantity: "6шт." },
            { name: "Красный лук", quantity: "100г" },
            { name: "Майонез", quantity: "250г" },
          ],
        },
      ],

      newArray: {
        name: "",
        comment: "",
      },
      dialogOfAdd: false,
      dialogOfTranser: false,
      dialogOfProps: false,
      gradeEnter: 10,
      dishKeeper: null,
      optionsOfSort: [
        { name: "Название", value: "name" },
        { name: "Оценка", value: "grade" },
      ],
      nameOfQuery: "ВЫБОР",
      valueOfQuery: "",
    };
  },
  methods: {
    addDish() {
      this.dialogOfAdd = !this.dialogOfAdd;
    },

    addDish2(newArray) {
      this.unTriedDishes.push(newArray);
    },
    removePost(data) {
      this.triedDishes = this.triedDishes.filter((m) => m.id !== data.id);
    },
    removePostFromUntried(data) {
      console.log(data);
      this.unTriedDishes = this.unTriedDishes.filter((m) => m.id !== data.id);
    },

    dishTransfer(dishKeeper) {
      this.unTriedDishes = this.unTriedDishes.filter(
        (m) => m.id !== dishKeeper.id
      );
      this.triedDishes.push(dishKeeper);
      this.dialogOfTranser = !this.dialogOfTranser;
    },
    gradeAffixing(dishKeeper) {
      dishKeeper.grade = +this.gradeEnter;
      this.dishTransfer(dishKeeper);
    },
    gradeDialogOpen(dish) {
      this.dialogOfTranser = !this.dialogOfTranser;
      dish.id = Date.now();
      this.dishKeeper = dish;
    },
    addItem() {
      this.newArray.id = Date.now();
      this.unTriedDishes.push(this.newArray);
      this.newArray = {
        name: "",
        comment: "",
      };
      this.dialogOfAdd = !this.dialogOfAdd;
    },
    choosedOption(item) {
      this.nameOfQuery = item.name;
      this.valueOfQuery = item.value;
    },
    newDish(par) {
      this.unTriedDishes.push(par);
      this.dialogOfAdd = !this.dialogOfAdd;
    },
    logo(dish) {
      console.log(dish);
    },
  },
};
</script>

<style scoped>
.dishes_main {
  width: 100%;
  display: flex;
  flex-direction: column;
  padding-bottom: 20px;
}

.list {
  display: block;
  border: solid 2px;
  padding: 6px;
  border-radius: 20px;
  margin: 10px;
  max-width: 600px;
  min-width: 250px;
  position: relative;
}

.addDishPopup {
  display: flex;
  flex-direction: row;
}

.container {
  display: flex;
  justify-content: center;
  flex-direction: column;
  align-items: center;
}

.ingr {
  width: 40%;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  align-self: center;
}

.ingr_wrapper {
  display: flex;
  flex-direction: column;
}

.button_wrapper {
  display: block;
  flex-direction: column;
}

.h3 {
  margin: 4px;
}

.mdwb {
  position: relative;
}
</style>