<template>
    <CollectionMain v-bind="{url,addUrl,editUrl,deleteUrl}"/>
   </template>
   <script setup lang="ts">
   // Declaring variables
   const url="https://v1-orm-lib.mars.hipso.cc/websites/?offset=0&limit=100&sort_column=id&sort_direction=desc"
   const addUrl="https://v1-orm-lib.mars.hipso.cc/websites/"
   const editUrl="https://v1-orm-lib.mars.hipso.cc/websites/123"
   const deleteUrl="https://v1-orm-lib.mars.hipso.cc/websites/123"
   </script>
   