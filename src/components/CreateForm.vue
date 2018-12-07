<template>
<div>
<form
  id="createForm"
  @submit="checkForm"
>
<div v-for="error in errors" :key="error.id">
    {{ error }}
</div>

<div v-for="item in insuranceFields.fields" :key="item.id">
    <h3>{{ item.name }}</h3>
    <div v-if="item.type == 'enum'">
        <el-select v-model="formFields[item.id]">
            <el-option disabled value="">Please select one</el-option>
            <el-option v-for="selectOption in item.enum_choices" :key="selectOption.id" v-bind:value="selectOption.id"  v-bind:label="selectOption.name"></el-option>
        </el-select>
    </div>
    <div v-else-if="item.type == 'text'">
        <el-input v-model="formFields[item.id]">
        </el-input>
    </div>
    <div v-else-if="item.type == 'number'">
        <el-input v-model.number="formFields[item.id]" type="number" step="0.01">
        </el-input>
    </div>
    <div v-else-if="item.type == 'date'">
        <el-date-picker v-model="formFields[item.id]">
        </el-date-picker>
    </div>
  </div>
  <el-button type="primary" native-type="submit" :disabled="formSubmitted">Save</el-button>
  <div if="formSubmitted">
      Submitted Successfully!
  </div>

</form> 
</div>

</template>

<script>
import axios from "axios";
import Vue from "vue";
import Datetime from "vue-datetime";
import "vue-datetime/dist/vue-datetime.css";
import { Select, Button, DatePicker } from "element-ui";

Vue.use(Datetime);
Vue.component(Select.name, Select);
Vue.component(Button.name, Button);
Vue.component(Option.name, Option);
Vue.component(DatePicker.name, DatePicker);

export default {
  name: "CreateForm",
  data() {
    return {
      insuranceFields: {},
      formFields: {},
      errors: [],
      formSubmitted: false
    };
  },
  mounted() {
    axios
      .get(
        "https://2lmqgnfs2l.execute-api.us-east-1.amazonaws.com/dev/insuranceFields/" +
          this.$route.params.id.toString()
      )
      .then(
        result => {
          this.insuranceFields = result.data;
        },
        error => {
          console.error(error);
        }
      );
  },
  methods: {
    checkForm: function(e) {
      e.preventDefault();

      this.errors = [];

      const data = {
        insurance: this.$route.params.id,
        user: 1,
        dataFieldValues: []
      };

      // Only need Client side validation for all fields being set
      // Other validations about data input validations are being done
      // by the selectors we use.
      for (let i in this.insuranceFields.fields) {
        console.log(i);
        console.log(this.insuranceFields.fields);
        const field = this.insuranceFields.fields[i];
        const fieldValue = this.formFields[field.id];
        if (fieldValue == null || fieldValue == "") {
          this.errors.push("Field is null: " + field.name);
        }
        if (field.type == "date") {
          // Django supported Date format
          fieldValue = fieldValue.toISOString().slice(0, 10);
        }
        data["dataFieldValues"].push({
          fieldId: field.id,
          value: fieldValue
        });
      }
      if (this.errors.length == 0) {
        axios
          .post("https://2lmqgnfs2l.execute-api.us-east-1.amazonaws.com/dev/userData/", data)
          .then(res => {
            this.formSubmitted = true;
            console.log(res);
          })
          .catch(err => {
            console.log(err);
          });
      }
    }
  }
};
</script>
