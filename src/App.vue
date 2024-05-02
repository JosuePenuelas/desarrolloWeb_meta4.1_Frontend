<template>
  <v-card>
    <v-layout>
      <v-navigation-drawer expand-on-hover rail color="indigo">
        <v-list>
          <v-list-item prepend-icon="mdi-vuetify" title="Servicio Web REST" subtitle="Sistema De Control De Activos"></v-list-item>
        </v-list>

        <v-divider></v-divider>

        <v-list density="compact" nav>
          <v-list-item prepend-icon="mdi-home" link to="/" title="Inicio"></v-list-item>
          <v-list-item prepend-icon="mdi-alpha-a-box" link to="/activos" title="Activos"></v-list-item>
          <v-list-item prepend-icon="mdi-tag-multiple" link to="/tags" title="Tags"></v-list-item>
          <v-list-item prepend-icon="mdi-audio-input-xlr" link to="/activoTags" title="ActivoTags"></v-list-item>
          <v-list-item prepend-icon="mdi-account" link to="/responsables" title="Responsables"></v-list-item>
          <v-list-item prepend-icon="mdi-sitemap" link to="/ubicaciones" title="Ubicaciones"></v-list-item>
        </v-list>
      </v-navigation-drawer>

      <v-main style="height: auto; width: auto;">
        <RouterView />
      </v-main>
    </v-layout>
  </v-card>
</template>

<script setup>
//
</script>