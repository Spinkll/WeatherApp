<template>
  <q-layout view="lHh Lpr lFf">
    <q-header>
      <q-toolbar class="text-secondary bg-primary">
        <q-toolbar-title class="text-bold custom-toolbar-title">
          є-Погода
        </q-toolbar-title>
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts"></script>

<style lang="scss" scoped>
.custom-toolbar-title{
  padding-left: 62px;
  font-size: 21px;
}
</style>
