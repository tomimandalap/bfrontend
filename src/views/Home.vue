<template>
  <div>
    <cnavbar titleText="Food Items" />
    <contenCustom />
  </div>
</template>

<script>
import cnavbar from '../components/navbar'
import contenCustom from '../components/sideright'
export default {
  name: 'Home',
  components: {
    cnavbar,
    contenCustom
    // asideRight
  }
}
</script>
