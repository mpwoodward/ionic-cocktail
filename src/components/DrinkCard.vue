<template>
  <ion-card>
    <img :src="drink.strDrinkThumb">
    <ion-card-header>
      <ion-card-subtitle>
        {{ drink.strCategory }} | Served In 
        {{ drink.strGlass }}
      </ion-card-subtitle>
      <ion-card-title>
        {{ drink.strDrink }}
      </ion-card-title>
    </ion-card-header>
    <ion-card-content>
      <p>{{ drink.strInstructions }}</p>
      <ion-list-header>
        Ingredients
      </ion-list-header>
      <ion-list>
        <div v-for="i in state.ingredientCount" :key="i">
          <ion-item v-if="drink[`strIngredient${i}`]">
            <ion-label>
              <span v-if="drink[`strMeasure${i}`]">
                {{ drink[`strMeasure${i}`] }} -
              </span>
              {{ drink[`strIngredient${i}`] }}
            </ion-label>
          </ion-item>
        </div>
      </ion-list>
    </ion-card-content>
  </ion-card>
</template>

<script lang="ts">
import { defineComponent, reactive } from '@vue/runtime-core'
import {
  IonCard,
  IonCardHeader,
  IonCardSubtitle,
  IonCardTitle,
  IonCardContent,
  IonListHeader,
  IonList,
  IonItem,
  IonLabel,
} from '@ionic/vue'

export default defineComponent({
  name: 'DrinkCard',
  props: {
    drink: Object,
  },
  components: {
    IonCard,
    IonCardHeader,
    IonCardSubtitle,
    IonCardTitle,
    IonCardContent,
    IonListHeader,
    IonList,
    IonItem,
    IonLabel,
  },
  setup() {
    const state = reactive({
      ingredientCount: 15,
    })

    return {
      state
    }
  }
})
</script>
