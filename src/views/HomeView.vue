<template>
  <div>
    <Banner />
    <div class="main-container">
      <h1>Cadastro de DEV amigo</h1>
      <DevForm />
    </div>
  </div>
</template>

<script>
import Banner from '../components/Banner'
import DevForm from '../components/DevForm'

export default {
  name: 'Home',
  components: {
    Banner,
    DevForm
  }
}
</script>
