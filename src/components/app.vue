<script setup>
import { f7 } from 'framework7-vue';
import MazInput from 'maz-ui/components/MazInput';
import { reactive, ref, watch } from 'vue';
import routes from '../js/routes.js';
import store from '../js/store';

// Framework7 Parameters
const f7params = {
  name: 'Order', // App name
  theme: 'auto', // Automatic theme detection
  colors: {
    primary: '#4365DE',
  },

  // App store
  store: store,
  // App routes
  routes: routes,
};

// Login screen data
const form = reactive({
  email: '',
  password: ''
})

const isEmailEmpty = ref(false)
const isPasswordEmpty = ref(false)

const session = ref({
  "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2ODUyNjM1ODMsImlhdCI6MTY4NTI2MTc4MywiaWQiOiI3MmE5MTFjOC1hZDI0LTRlMmQtODkzMC05YzNiYTUxNzQxZGYiLCJjcmVhdGVkQXQiOiIyMDIyLTExLTA3VDA2OjQzOjAxLjA4OSIsImZpcnN0bmFtZSI6IkFkbWluIiwibGFzdG5hbWUiOiJTdXBlciBNYW5hZ2VyIiwicGhvbmUiOiIrOTk4OTAxMjM0NTY3Iiwicm9sZSI6InN1cGVyX21hbmFnZXIifQ.FArKnQzts6Hq4mZCtzyvaCinylbwf9Y0ZzF5WRFQFq8",
  "refreshToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJleHAiOjE2ODUyNzYxODMsImlhdCI6MTY4NTI2MTc4M30.1Q3EdyeyCJ92irALvxBauObPeyoRS40iGxwTeIc2qmM"
})

const alertLoginData = () => {
  if (!form.email) {
    isEmailEmpty.value = true
  } else if (!form.password) {
    isPasswordEmpty.value = true
  } else {
    // f7.dialog.alert('Email: ' + form.email + '<br>Password: ' + form.password, () => {
    //   f7.loginScreen.close();
    // });
    f7.preloader.show()
    setTimeout(() => {
      localStorage.setItem('session', JSON.stringify(session.value))
      localStorage.setItem('loggedIn', true)
      f7.preloader.hide()
      f7.loginScreen.close();
      window.location.reload()
    }, 2000)
  }
}

const logout = () => {
  f7.preloader.show()
  setTimeout(() => {
    localStorage.clear()
    f7.preloader.hide()
    window.location.reload()
  }, 1000)
}

watch(() => form.email, (val) => {
  if (val) {
    isEmailEmpty.value = false
  }
})

watch(() => form.password, (val) => {
  if (val) {
    isPasswordEmpty.value = false
  }
})

</script>
<template>
  <f7-app v-bind="f7params">

    <!-- Left panel with cover effect-->
    <f7-panel left cover light>
      <f7-view>
        <f7-page>
          <f7-navbar title="Left Panel"></f7-navbar>
          <f7-block>
            Left panel content goes here
            <f7-link icon-ios="f7:square_arrow_left" icon-md="material:logout" tab-link="#view-home" text="logout"
              @click="logout"></f7-link>
          </f7-block>
        </f7-page>
      </f7-view>
    </f7-panel>


    <!-- Right panel with reveal effect-->
    <f7-panel right reveal light>
      <f7-view>
        <f7-page>
          <f7-navbar title="Right Panel"></f7-navbar>
          <f7-block>Right panel content goes here</f7-block>
        </f7-page>
      </f7-view>
    </f7-panel>


    <!-- Views/Tabs container -->
    <f7-views tabs class="safe-areas">
      <!-- Tabbar for switching views-tabs -->
      <f7-toolbar tabbar icons bottom>
        <f7-link tab-link="#view-home" tab-link-active icon-ios="f7:house_fill" icon-md="material:home"
          text="Home"></f7-link>
        <f7-link tab-link="#view-catalog" icon-ios="f7:square_list_fill" icon-md="material:view_list"
          text="Catalog"></f7-link>
        <f7-link tab-link="#view-settings" icon-ios="f7:gear" icon-md="material:settings" text="Settings"></f7-link>
      </f7-toolbar>

      <!-- Your main view/tab, should have "view-main" class. It also has "tab-active" class -->
      <f7-view id="view-home" main tab tab-active url="/"></f7-view>

      <!-- Catalog View -->
      <f7-view id="view-catalog" name="catalog" tab url="/catalog/"></f7-view>

      <!-- Settings View -->
      <f7-view id="view-settings" name="settings" tab url="/settings/"></f7-view>

    </f7-views>


    <!-- Popup -->
    <f7-popup id="my-popup">
      <f7-view>
        <f7-page>
          <f7-navbar title="Popup">
            <f7-nav-right>
              <f7-link popup-close>Close</f7-link>
            </f7-nav-right>
          </f7-navbar>
          <f7-block>
            <p>Popup content goes here.</p>
          </f7-block>
        </f7-page>
      </f7-view>
    </f7-popup>

    <f7-login-screen id="my-login-screen">
      <f7-view>
        <f7-page login-screen>
          <h1 class="text-center">Login</h1>
          <div class="p-5 space-y-1">
            <MazInput v-model="form.email" :hint="isEmailEmpty ? 'Please enter email!' : 'Email'" :error="isEmailEmpty" />
            <MazInput v-model="form.password" type="password" class="mt-10"
              :hint="isPasswordEmpty ? 'Please enter password!' : 'Password'" :error="isPasswordEmpty" />
          </div>
          <f7-list>
            <f7-list-button title="Sign In" @click="alertLoginData"></f7-list-button>
          </f7-list>
        </f7-page>
      </f7-view>
    </f7-login-screen>
  </f7-app>
</template>
