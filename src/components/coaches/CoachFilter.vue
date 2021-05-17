<template>
<base-card>
<h2>Find your teacher!</h2>
<span class="filter-option">
<input type="checkbox" id="mathematics" checked @change="setFilter"/>
<label for="mathematics">Mathematics</label>
</span>
<span class="filter-option">
<input type="checkbox" id="literaly" checked @change="setFilter" />
<label for="literaly">Literaly</label>
</span>
<span class="filter-option">
<input type="checkbox" id="economics" checked @change="setFilter" />
<label for="economics">Economics</label>
</span>
<span class="filter-option">
<input type="checkbox" id="informatics" checked @change="setFilter" />
<label for="informatics">Informatics</label>
</span>

</base-card>
</template>


<script>
export default{
    emits:['change-filter'],
data() {
return {
    filters:{
        mathematics:true,
        literaly:true,
        economics:true,
        informatics:true
    }
};
},
methods:{
    setFilter(event){
        const inputId=event.target.id;
        const isActive=event.target.checked;
        const updatedFilters={
            ...this.filters,
            [inputId]:isActive
        };
        this.filters=updatedFilters;
        this.$emit('change-filter',updatedFilters);

    }
}
}



</script>

<style scoped>
h2 {
  margin: 0.5rem 0;
}

.filter-option {
  margin-right: 1rem;
}

.filter-option label,
.filter-option input {
  vertical-align: middle;
}

.filter-option label {
  margin-left: 0.25rem;
}

.filter-option.active label {
  font-weight: bold;
}

</style>