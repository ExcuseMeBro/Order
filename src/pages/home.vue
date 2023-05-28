<script setup>
import { f7 } from 'framework7-vue';
import MazAvatar from 'maz-ui/components/MazAvatar';
import { onMounted, ref, watch } from 'vue';
const isLoginSuccessful = ref(false)
// watch(() => localStorage.getItem("loggedIn"), () => {
//   console.log(localStorage.getItem("loggedIn"));
// })
onMounted(() => {
  if (localStorage.getItem("loggedIn")) {
    isLoginSuccessful.value = true
  }
})
</script>
<template>
  <f7-page name="home">
    <!-- Top Navbar -->
    <f7-navbar large :sliding="false">
      <f7-nav-left>
        <f7-link v-if="isLoginSuccessful" panel-open="left">
          <MazAvatar size="10px" src="https://placekitten.com/640/600" />
        </f7-link>
        <f7-button v-else login-screen-open="#my-login-screen" icon-ios="f7:person_circle" icon-md="material:account_circle"></f7-button>
      </f7-nav-left>
      <f7-nav-title sliding>Order</f7-nav-title>
      <f7-nav-right>
        <f7-link icon-ios="f7:bell" icon-md="material:notifications" href="/notifications/"></f7-link>
      </f7-nav-right>
      <f7-nav-title-large>Order</f7-nav-title-large>
    </f7-navbar>

    <!-- Page content-->
    <f7-block>
      <p>This is an example of tabs-layout application. The main point of such tabbed layout is that each tab contains
        independent view with its own routing and navigation.</p>
    </f7-block>
    <f7-block-title>Navigation</f7-block-title>
    <f7-list strong inset dividersIos>
      <f7-list-item link="/about/" title="About"></f7-list-item>
      <f7-list-item link="/notifications/" title="Notifications"></f7-list-item>
      <f7-list-item link="/form/" title="Form"></f7-list-item>
    </f7-list>

    <f7-block-title>Modals</f7-block-title>
    <f7-block class="grid grid-cols-2 grid-gap">
      <f7-button fill popup-open="#my-popup">Popup</f7-button>
      <f7-button fill login-screen-open="#my-login-screen">Login Screen</f7-button>
    </f7-block>

    <f7-block-title>Panels</f7-block-title>
    <f7-block class="grid grid-cols-2 grid-gap">
      <f7-button fill panel-open="right">Right Panel</f7-button>
    </f7-block>

    <f7-list strong inset dividersIos>
      <f7-list-item title="Dynamic (Component) Route"
        link="/dynamic-route/blog/45/post/125/?foo=bar#about"></f7-list-item>
      <f7-list-item title="Default Route (404)" link="/load-something-that-doesnt-exist/"></f7-list-item>
      <f7-list-item title="Request Data & Load" link="/request-and-load/user/123456/"></f7-list-item>
    </f7-list>
  </f7-page>
</template>