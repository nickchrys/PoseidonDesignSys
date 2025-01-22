<script setup>
import PButton from './components/PButton.vue'
import PTextField from './components/PTextField.vue'
import PEventBlock from './components/PEventBlock.vue'
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

    <div>
      <h2>PEventBlock</h2>
      <h3>EventFull</h3>
      <div class="eventContainer">
        <PEventBlock class="eventBlock" organization="Organization" event="Event 1" :startDate="new Date(2025-10-5)"
          :endDate="new Date(2025-10-12)"
          bgImage="https://tripspi-prod.imgix.net/blogs/vG3JS5lXOE/d2aJg-luis-fernando-felipe-alves-9mckk9YMglY-unsplash.jpg?ixlib=js-2.3.2&w=1024&auto=compress&fit=crop&s=f13ee1017771d50b761d3a2170de100a" />
      </div>
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
