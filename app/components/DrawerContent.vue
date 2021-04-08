<template lang="html">
    <GridLayout rows="auto, *" class="nt-drawer__content">
        <StackLayout row="0" class="nt-drawer__header">
        
            <Label class="nt-drawer__header-brand" text=""/>
            <Label class="nt-drawer__header-footnote" text="Covid19Tracker"/>
        </StackLayout>

        <ScrollView row="1" class="nt-drawer__body">
            <StackLayout>
                <GridLayout columns="auto, *"
                            :class="'nt-drawer__list-item' + (selectedPage === 'Home' ? ' -selected': '')"
                            @tap="onNavigationItemTap(Home)">
     
                    <Label col="1" text="Accueil" class="p-r-10"/>
                </GridLayout>


                <GridLayout columns="auto, *"
                            :class="'nt-drawer__list-item' + (selectedPage === 'Vaccinations' ? ' -selected': '')"
                            @tap="onNavigationItemTap(Vaccinations)">
              
                    <Label col="1" text="Où se faire vacciner?" class="p-r-10"/>
                </GridLayout>

                <GridLayout columns="auto, *"
                            :class="'nt-drawer__list-item' + (selectedPage === 'Informations' ? ' -selected': '')"
                            @tap="onNavigationItemTap(Informations)">
                
                    <Label col="1" text="Les gestes barrières anti-Covid" class="p-r-10"/>
                   
                </GridLayout>

                <StackLayout class="hr"/>

                
                
            </StackLayout>
        </ScrollView>
    </GridLayout>
</template>

<script>
  import Home from "./Home";
  import Vaccinations from "./Vaccinations";
  import Informations from "./Informations";
  import * as utils from "~/shared/utils";
  import SelectedPageService from "~/shared/selected-page-service";
import VaccinationsVue from './Vaccinations.vue';

  export default {
    mounted() {
      SelectedPageService.getInstance().selectedPage$
        .subscribe((selectedPage) => this.selectedPage = selectedPage);
    },
    data() {
      return {
        Home: Home,
        Vaccinations: Vaccinations,
        Informations: Informations,
        selectedPage: ""
      };
    },
    components: {
      Home,
      Vaccinations,
      Informations,
    },
    methods: {
      onNavigationItemTap(component) {
        this.$navigateTo(component, {
          clearHistory: true
        });
        utils.closeDrawer();
      }
    }
  };
</script>

<style scoped lang="scss">
    // Start custom common variables
    @import '~@nativescript/theme/scss/variables/blue';
    // End custom common variables

    // Custom styles
</style>
