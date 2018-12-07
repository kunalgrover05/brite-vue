<template>
<div>
  <div v-for="item in insuranceOptions" :key="item.id" v-on:click="openForm(item)" style="font-size: 30px; padding: 10px; cursor: pointer; text-decoration: underline;">
    {{ item.name }}
  </div>

  <div style="margin-top: 100px; padding: 10px; cursor: pointer; text-decoration: underline">
    <router-link  to="/createdForms">View Created form data</router-link>

    
    </div>

</div>
</template>

<script>
import axios from "axios";
import router from 'vue-router';

export default {
    name: 'LandingPage',
    data() {
        return {
            insuranceOptions: [
                ]
        }
    },
    mounted() {
            axios.get("https://2lmqgnfs2l.execute-api.us-east-1.amazonaws.com/dev/insuranceFields/").then(result => {
                this.insuranceOptions = result.data;
                console.log(result);
            }, error => {
                console.error(error);
            });
        },
        methods: {
            openForm: function(event) {
                console.log("Clicked");
                console.log(event);
                this.$router.push({name: "CreateForm", params: {id: event.id}});
            }
        }


}
</script>
