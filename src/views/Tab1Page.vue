<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Random Cocktail</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content v-if="state.loading">
      <div class="loading-center">
        <ion-spinner color="primary"></ion-spinner>
      </div>
    </ion-content>
    <ion-content :fullscreen="true" v-else>
      <ion-refresher slot="fixed" @ionRefresh="doRefresh">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>
      <drink-card :drink="state.randomCocktail"></drink-card>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import {
  IonPage,
  IonHeader,
  IonToolbar,
  IonTitle,
  IonContent,
  IonRefresher,
  IonRefresherContent,
  IonSpinner,
} from '@ionic/vue'
import { reactive } from 'vue'
import axios from 'axios'
import DrinkCard from '@/components/DrinkCard.vue'

export default  defineComponent({
  name: 'Tab1Page',
  components: {
    IonHeader,
    IonToolbar,
    IonTitle,
    IonContent,
    IonPage,
    IonRefresher,
    IonRefresherContent,
    IonSpinner,
    DrinkCard,
  },
  setup() {
    const state = reactive({
      randomCocktail: {},
      loading: false,
      ingredientCount: 15,
    })

    const fetchRandomCocktail = async (dispLoaderPage: boolean) => {
      if (dispLoaderPage) {
        state.loading = true
      }

      const res = await axios.get('https://www.thecocktaildb.com/api/json/v1/1/random.php')

      if (res.data) {
        state.randomCocktail = res.data?.drinks[0]
      }

      state.loading = false
    }

    const doRefresh = (event: CustomEvent) => {
      fetchRandomCocktail(false)

      // eslint-disable-next-line @typescript-eslint/ban-ts-comment
      //@ts-ignore
      event.target?.complete()
    }

    fetchRandomCocktail(true)

    return {
      state,
      fetchRandomCocktail,
      doRefresh,
    }
  },
})
</script>

<style scoped>
.loading-center {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 90vh;
}

ion-spinner {
  transform: scale(1.5);
}
</style>