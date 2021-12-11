<template>
  <div class="newObj_wrapper">
        <div >
            <input type="text" placeholder="Название блюда" v-model="this.newDish.name">
        </div>
        <div>
            <!-- <h4> Введите способ приготовления:</h4> -->
            <textarea class="textarea"
             type="text"
             placeholder="Способ приготовления" 
             v-model="this.newDish.comment">
             </textarea>
        </div>
       
        <div class="newObj_wrapper__field"
        v-for="item in this.newDish.listOfIngredients"
         :key="item.id">
            <div> 
                <input type="text" name="" 
                id="" v-model="item.name" 
                placeholder="Введите ингредиент">
            </div>
            <div class="newObj_wrapper__container" > 
                <input type="text" name="" 
                id="" v-model="item.quantity" 
                placeholder="Введите количество">
            </div>
            
        
        </div >
        <div class="newObj_btn">
                <my-button  @click="addItem"
                style="width:40px" 
                title="Добавить ингредиент"> + </my-button>      

                <my-button 
                @click="deleteItem()"
                style="width:40px"> - </my-button>
        </div>
        <hr align="center" width="90%" size="3" color="#ff9900" />
        <div class="btn_container">
            
            
            <my-button @click="formArrayTransfer" > Подтвердить </my-button>
        </div> 
    
  </div>
</template>

<script>
import myButton from '@/UI/myButton.vue'
export default {
    components: { myButton },
    data() {
        return {
            newDish: {
                name: '', comment: '', id: '1', listOfIngredients: [{name: '', quantity :'', id: 1}]
                }
        
        }
    },
    methods: {
        addItem() {
            this.newDish.listOfIngredients.push({name: '', quantity: '', id: Date.now()});
        },
        deleteItem() {
            this.newDish.listOfIngredients.pop()
            // console.log(this.newDish.listOfIngredients)
        },
        formArrayTransfer() {
            // console.log(option);
        //   par.id = Date.now()
            this.$emit('newDishArray', this.newDish)

        }
    },
}
</script>

<style>
input, textarea {
    margin: 5px;
}

.newObj_wrapper {
    display:flex;
    justify-content: center;
    flex-direction: column ;
    align-items: center;
}

.newObj_wrapper__container {
    display: flex;
    position: relative;
    border: solid 2 px black;
    flex-direction: column;
    align-items: center;

}



.newObj_wrapper__field {
    display: flex;
    flex-direction: row;
    border: solid 2px;
    margin: 2px 0;

}

@media (max-width: 520px) {
    .newObj_wrapper__field {
        display: block;
    }
   }

.newObj_btn {
    padding-top: 3px;
    display: flex;
    flex-direction: row;
}

.textarea {
    min-width: 200px;
    width: 250px;
    height: 300px;
}
</style>