<template>
  <div class="home">
    <CalendarComp msg="2020-02-01" lang="ru"/>
  </div>
</template>

<script>

import CalendarComp from '@/components/Calendar.vue'

export default {
  name: 'HomeView',
  components: {
    CalendarComp,
  }
}
</script>
