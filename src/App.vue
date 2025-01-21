<script setup>
import PButton from './components/PButton.vue'
import PTextField from './components/PTextField.vue'
import './style.css'
</script>

<template>
  <div class="displayDiv">
    <h1>Poseidon Components</h1>
    <h2>PButton Component Styles</h2>
    <div>
      <h3>Login Button</h3>
      <PButton class="loginButton" msg="Login" />
    </div>

    <div>
      <h3>Gradient Button</h3>
      <PButton class="gradButton" msg="Book Your Flight Here Now!" />
    </div>

    <div>
      <h3>Gradient Button (Small)</h3>
      <PButton class="gradButtonSmall" msg="Talk with me!" />
    </div>

    <div>
      <h2>PTextField</h2>
      <h3>Text Field</h3>
      <PTextField class="textField" label="Enter Username" />
    </div>
  </div>


</template>

<style>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
