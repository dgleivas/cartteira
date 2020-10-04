<template>
  <div id="app">
    <base-spinner></base-spinner>
    <Mensagens></Mensagens>
    <router-view></router-view>
  </div>
</template>

<script>
import BaseSpinner from "./components/global/BaseSpinner";
import Mensagens from "./components/global/mensagens";

export default {
  components: {
    BaseSpinner,
    Mensagens
  },
  mounted() {
    if (this.$store.state.email == null) {
      this.$firebase.auth().signOut();
      this.$router.push({ name: "Login" });
      this.$root.$emit("Spinner::hide");
    } else {
      this.$firebase.auth().onAuthStateChanged(user => {
        window.uid = user ? user.uid : null;
        this.$router.push({ name: window.uid ? "Home" : "Login" });
        this.$root.$emit("Spinner::hide");
      });
    }
  }
};
</script>

<style scoped>
#app {
  height: 80vh;
}
</style>
