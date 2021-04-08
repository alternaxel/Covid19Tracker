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
            <Label class="action-bar-title" text="Vaccinations"/>
        </ActionBar>
         <ScrollView>
        <GridLayout class="page__content text-center">
           <WebView src="https://www.maine-et-loire.fr/aides-et-services/sante/ou-se-faire-vacciner-gratuitement" />
           <StackLayout orientation="vertical" style="margin-top: 25px; padding: 40px ">
                <Label class="page__content-icon fas" text.decode="&#xf015;"/>
                <Label class="page__content-placeholder" :text="message"/>
               
 </StackLayout> 
        </GridLayout>
      </ScrollView>
    </Page>
</template>

<script>
  import * as utils from "~/shared/utils";
  import SelectedPageService from "../shared/selected-page-service";

  export default {
    mounted() {
      SelectedPageService.getInstance().updateSelectedPage("Vaccinations");
    },
    computed: {
      message() {
        return "<!-- Vaccinations sds-->";
      }
    },
    methods: {
      onDrawerButtonTap() {
        utils.showDrawer();
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
