<script setup>
import { f7 } from 'framework7-vue';
import { ref, watch } from 'vue'
const isIos = ref(localStorage.getItem('theme').includes('ios') || false)

watch(() => isIos.value, (val) => {
  if (val) {
    f7.dialog.confirm('Save changes?', '', () => {
      localStorage.setItem('theme', 'ios')
      window.location.reload()
    }, () => { window.location.reload() })
  } else {
    f7.dialog.confirm('Save changes?', '', () => {
      localStorage.setItem('theme', 'md')
      window.location.reload()
    }, () => { window.location.reload() })
  }
})

const isDark = ref(false)

watch(() => isDark.value, (val) => {
  f7.setDarkMode(val)
})

</script>
<template>
  <f7-page name="settings">
    <f7-navbar title="Settings"></f7-navbar>
    <f7-block-title>App Settings</f7-block-title>
    <f7-list strong-ios dividers-ios outline-ios>
      <f7-list-item :title="isDark ? 'Dark' : 'Light'">
        <template #after>
          <f7-toggle v-model:checked="isDark" />
        </template>
      </f7-list-item>
      <f7-list-item :title="isIos ? 'Ios Theme' : 'Material Theme'">
        <template #after>
          <f7-toggle v-model:checked="isIos" />
        </template>
      </f7-list-item>
    </f7-list>
  </f7-page>
</template>
