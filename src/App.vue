<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import Vue from 'vue';
import VueRouter from 'vue-router';
import CreateForm from './components/CreateForm';
import LandingPage from './components/LandingPage';
import CreatedForms from './components/CreatedForms';
import lang from 'element-ui/lib/locale/lang/en'
import locale from 'element-ui/lib/locale'

const router = new VueRouter({
    routes: [
        {
            path: "/",
            name: 'LandingPage',
            component: LandingPage
        },
        {
            path: "/createdForms",
            name: 'CreatedForms',
            component: CreatedForms
        },
        {
            path: '/form/:id',
            name: 'CreateForm',
            component: CreateForm
        }
    ]
})

Vue.use(VueRouter);
Vue.use(ElementUI);

// configure language
locale.use(lang)

export default {
  name: 'app',
  components: {
    CreateForm,
    LandingPage
  },
  router: router
}

</script>

<style>
</style>
