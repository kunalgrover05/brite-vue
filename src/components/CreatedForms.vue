<template>
<div>
  <div v-for="item in completedForms" :key="item.id" style="margin-bottom: 30px; margin-bottom: 30px">
    <div style="height: 1px; width: 100%; background: #ccc"></div>      
     <span style="font-size: 19px; color: gray">Form Data for {{ item.insuranceName }}</span>
      <div v-for="dataField in item.dataFieldValues" :key="dataField.id">
          <strong>{{dataField.field}}</strong>: {{dataField.value}}
      </div>
    <div style="height: 1px; width: 100%; background: #ccc"></div>
  </div>
</div>
</template>

<script>
import axios from "axios";
import router from "vue-router";

export default {
  name: "CreatedForms",
  data() {
    return {
      completedForms: []
    };
  },
  mounted() {
    axios
      .get(
        "https://2lmqgnfs2l.execute-api.us-east-1.amazonaws.com/dev/userData/"
      )
      .then(
        result => {
          this.completedForms = result.data;
          console.log(result);
        },
        error => {
          console.error(error);
        }
      );
  }
};
</script>
