<template>
    <Page class="page">
        <ActionBar class="action-bar">
            <!--
            Use the NavigationButton as a side-drawer button in Android
            because ActionItems are shown on the right side of the ActionBar
            -->
            <NavigationButton ios:visibility="collapsed" icon="res://menu" @tap="onDrawerButtonTap"/>
            <!--
            Use the ActionItem for IOS with position set to left. Using the
            NavigationButton as a side-drawer button in iOS is not possible,
            because its function is to always navigate back in the application.
            -->
            <ActionItem icon="res://menu"
                        android:visibility="collapsed"
                        @tap="onDrawerButtonTap"
                        ios.position="left"/>
            <Label class="action-bar-title" text="Covid-19Tracker"/>
        </ActionBar>
      <ScrollView>
        <GridLayout class="page__content text-center">
           <StackLayout orientation="vertical" style="margin-top: 25px; padding: 40px ">
            <Label class="h2" text="Population Totale | Maine-et-Loire"/>
            <Label class="h3" :text="this.nb_population_maine_et_loire"/>
            <Label class="h3" text="Pourcentage de personnes ayant reçu une première dose de vaccin:"/>
            <Label class="h3" :text="this.people_vaccine_1"/>
            <Label class="h3" text="Pourcentage de personnes ayant reçu 2 doses de vaccin :"/>
            <Label class="h3" :text="this.people_vaccine_2"/>
            <Image class="img" src="res://shield" stretch="aspectFit" />
            <Label class="p" text="Découvrez en direct l'avancement de la campagne vaccinale au Maine-et-Loire! Ensemble, luttons contre le Covid-19 ! "/>
            <Label class="p" text="Les données fournies par cette application proviennent du site www.opendatasoft.com"/>
          </StackLayout> 
        </GridLayout>
      </ScrollView>
    </Page>
</template>

<script>
  import * as utils from "~/shared/utils";
  import SelectedPageService from "../shared/selected-page-service";
  import axios from "axios";
  export default {
    mounted() {
      SelectedPageService.getInstance().updateSelectedPage("Home");
      this.api_get_data();
    },
    data() {
      return {
        errorMessage: '',
        records: {},
        nb_population_maine_et_loire: 815325,
        nb_dose_cum_1: 0,
        people_vaccine_1: 0, 
        nb_dose_cum_2: 0,
        people_vaccine_2: 0, 
      }
    },
    computed: {
      message() {
        return "<!-- Page content goes here -->";
      }
    },
    methods: {
      onDrawerButtonTap() {
        utils.showDrawer();
      },
      api_get_data(){
        axios.get('https://public.opendatasoft.com/api/records/1.0/search/?dataset=covid-19-france-vaccinations-age-sexe-dep&q=&rows=10&sort=date&facet=date&facet=variable&facet=variable_label&facet=dep_name&facet=reg_code&facet=reg_name&facet=dep_area_code&refine.dep_name=Maine-et-Loire&refine.variable_label=Tous+%C3%A2ges')
          .then(response => {
              this.records = response.data.records[0].fields;
              console.log(this.records);
              this.nb_dose_cum_1 = this.records.n_cum_dose1;
              this.people_vaccine_1 = this.nb_dose_cum_1 / this.nb_population_maine_et_loire * 100;
              this.people_vaccine_1 = parseFloat(this.people_vaccine_1).toFixed(2) + '%';

              this.nb_dose_cum_2 = this.records.n_cum_dose2;
              this.people_vaccine_2 = this.nb_dose_cum_2 / this.nb_population_maine_et_loire * 100;
              this.people_vaccine_2 = parseFloat(this.people_vaccine_2).toFixed(2) + '%';
              console.log(this.people_vaccine_2);


          }).catch(error => {
            this.errorMessage = error.message;
            console.log(this.errorMessage);
          });
      }
    }
  };
</script>

<style scoped lang="scss">
    // Start custom common variables
    @import '~@nativescript/theme/scss/variables/blue';
    // End custom common variables

    // Custom styles
    .h3 {  

  overflow: hidden;  
  text-overflow: ellipsis;  
}
    .h1, .h2, .h3, .p {
  font-size: 15px;
  white-space: normal
  ;
    }
   .p{
     margin-top: 20px;
     margin-bottom: 20px;
   }
    .GridLayout {
  white-space: normal
    }
    .img{
      margin-top: 30px;
    }
</style>
