<template>
  <div>
    <cnavbar titleText="History" />
    <div class="container-bars">
      <asideLeft />
      <contenHistory />
    </div>
  </div>
</template>

<script>
import cnavbar from '../components/navbarHistory'
import asideLeft from '../components/sideleft'
import contenHistory from '../components/contentHistory'
export default {
  name: 'History',
  components: {
    cnavbar,
    asideLeft,
    contenHistory
  }
}
</script>
