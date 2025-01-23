<script setup>
import PButton from './components/PButton.vue'
import PTextField from './components/PTextField.vue'
import PEventBlock from './components/PEventBlock.vue'
import PFlight from './components/PFlight.vue'
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
      <div class="eventContainer">
        <PEventBlock class="eventBlock" organization="Organization" event="Event 1" :startDate="new Date('2025-11-8')"
          :endDate="new Date('2025-11-12')"
          bgImage='https://images.unsplash.com/photo-1556743595-0c9952ae2645?q=80&w=2007&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D' />
      </div>
    </div>

    <div>
      <h2>PFlight</h2>
      <div class="eventContainer">
        <PFlight class="flight-block" :flightDate="new Date('2024-11-7')" flightDepTime="12:30" flightArrTime="14:45" flightRoute="EWR-ROC" seatNumber="3B" flightType="Nonstop" flightGate="41C" flightClass="Economy" price="156"
        airline="United"/>
      </div>
    </div>

  </div>


</template>
