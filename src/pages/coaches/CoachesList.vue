<template>
<div>
<base-dialog :show="!!error" title="An error occured!" @close="handleError">
<p>{{error}}</p>
</base-dialog>
<section>
    <coach-filter @change-filter="setFilters"></coach-filter>
</section>
<section>
    <base-card>
    <div class="controls">
    <base-button mode="outline" @click="loadCoaches(true)">Refresh</base-button>
    <base-button link to="/auth?redirect=register" v-if="!isLoggedIn">Login to Register as Teacher</base-button>
    <base-button v-if="isLoggedIn && !isCoach && !isLoading" link to="/register">Register as Teacher</base-button>
    </div>
    <div v-if="isLoading">
        <base-spinner></base-spinner>
    </div>
    <ul v-else-if="hasCoaches">
        <coach-item v-for="coach in filteredCoaches" :key="coach.id" :id="coach.id" :first-name="coach.firstName" :last-name="coach.lastName" :rate="coach.hourlyRate" :areas="coach.areas"></coach-item>
    </ul>
    <h3 v-else>No coaches found..!</h3>
    </base-card>
</section>
</div>
</template>


<script>
import CoachItem from '../../components/coaches/CoachItem.vue';
import CoachFilter from '../../components/coaches/CoachFilter.vue';
import BaseButton from '../../components/ui/BaseButton.vue';
export default{

components:{
    CoachItem,
    CoachFilter,
    BaseButton
},

data(){
        return{
        isLoading:false,
        error:null,
        activeFilters:{
            mathematics:true,
            literaly:true,
            economics:true
        }
    };
},
computed:{
    isLoggedIn(){
     return this.$store.getters.isAuthenticated;

    },
    isCoach(){
        return this.$store.getters['coaches/isCoach'];
    },
    
filteredCoaches(){
    const coaches= this.$store.getters['coaches/coaches'];
    return coaches.filter(coach => {
        if (this.activeFilters.mathematics && coach.areas.includes('mathematics')){
            return true;
        }
        if (this.activeFilters.literaly && coach.areas.includes('literaly')){
            return true;
        }
        if (this.activeFilters.economics && coach.areas.includes('economics')){
            return true;
        }

         if (this.activeFilters.informatics && coach.areas.includes('informatics')){
            return true;
        }
      
        return false;
    });
    
},

hasCoaches(){
    return !this.isLoading && this.$store.getters['coaches/hasCoaches'];
},
},

created(){
    this.loadCoaches();
},
methods:{
    setFilters(updatedFilters){
         this.activeFilters=updatedFilters;
    },
   async loadCoaches(refresh=false){
        this.isLoading=true;
        try{
        await this.$store.dispatch('coaches/loadCoaches',{forceRefresh:refresh});
        }catch (error){
            this.error=error.message || 'Something went wrong';
        }
        this.isLoading=false;
    },
    handleError(){
        this.error=null;
    }
}

};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

.controls {
  display: flex;
  justify-content: space-between;
}

</style>