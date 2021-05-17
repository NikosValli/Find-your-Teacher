<template>
<div>
<section>
<base-card>
<h4 style="font-style:oblique;">Name</h4>
<h2>{{fullName}}</h2>
<hr style="margin:20px 0px;">
<h4 style="font-style:oblique;">Fee</h4>
<h3>${{rate}} / hour</h3>
</base-card>
<section>
<base-card>
<h4 style="font-style:oblique;">Expertised in :</h4>
<base-badge v-for="area in areas" :key="area" :type="area" :title="area" ></base-badge>
<hr style="margin:20px 0px;">
<h4 style="font-style:oblique;">Details about the teacher</h4>
<p>{{description}}</p>
</base-card>
</section>
</section>
<section>
<base-card>
<header>
<h2>Interested? Reach Out now!</h2>
<base-button v-if="$route.name != 'contact'"  link :to="contactLink" v-on:click="isContactButtonClicked">Contact</base-button>
</header>
<router-view></router-view>
</base-card>
</section>

</div>
</template>

<script>
export default {
props:['id'],
data(){
    return {
        selectedCoach:null,
        isContactClicked:true
    };
},

computed:{
    fullName(){
return this.selectedCoach.firstName + ' ' +this.selectedCoach.lastName;
    },

    areas(){
      return this.selectedCoach.areas;
    },
    rate(){
        return this.selectedCoach.hourlyRate;
    },
    description(){
        return this.selectedCoach.description;
    },
    contactLink(){
        
        return this.$route.path +'/contact';
        
       
      
    }
},

methods:{

isContactButtonClicked(){
    this.isContactClicked=false;
},

},


created(){
    this.selectedCoach=this.$store.getters['coaches/coaches'].find(coach=>coach.id===this.id);
}



};

</script>