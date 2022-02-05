<template>
  <v-app>
    

    <v-main>
      <WayFinder/>
    </v-main>
  </v-app>
</template>

<script>
import WayFinder from './components/WayFinder';

export default {
  name: 'App',

  components: {
    WayFinder,
  },

  data: () => ({
    //
  }),
};
</script>
