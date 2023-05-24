<template>
  <ColorScheme placeholder="..." tag="span">
    <v-app
        :theme="$colorMode.value"
    >
      <NavigationDrawer />
      <slot />
    </v-app>
  </ColorScheme>
</template>

<!-- <script setup>
import { useTheme } from 'vuetify'
useTheme().global.name.value = useColorMode().value
</script> -->