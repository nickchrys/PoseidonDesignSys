<script setup>
import PButton from './components/PButton.vue'
import PTextField from './components/PTextField.vue'
import PEvent from './components/PEvent.vue'
import PFlight from './components/PFlight.vue'
import PProfilePic from './components/PProfilePic.vue'
import PFinanceBlock from './components/PFinanceBlock.vue'
import PDropDown from './components/PDropDown.vue'
import './style.css'
</script>

<template>
  <div class="display-div">
    <h1>Poseidon Components</h1>
    <h2>PButton Component Styles</h2>
    <div>
      <h3>Login Button</h3>
      <PButton design="login" label="Login" />
    </div>

    <div>
      <h3>Gradient Button</h3>
      <PButton design="gradient" label="Book Your Flight Here Now!" />
    </div>

    <div>
      <h3>Gradient Button (Small)</h3>
      <PButton design="gradient-small" label="Talk with me!" />
    </div>

    <div>
      <h2>PTextField</h2>
      <h3>Text Field</h3>
      <PTextField design="p-textfield" label="Enter Username" />
    </div>

    <div>
      <h2>PEventBlock</h2>
      <div class="p-event__container">
        <PEvent design="block" organization="Organization" event="Event 1" :startDate="new Date('2025-11-8')"
          :endDate="new Date('2025-11-12')"
          bgImage='https://images.unsplash.com/photo-1556743595-0c9952ae2645?q=80&w=2007&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D' />
      </div>
      <h2>event-header</h2>
      <PEvent design="header" organization="Organization" event="Event 1" :startDate="new Date('2025-11-8')"
        :endDate="new Date('2025-11-12')"
        bgImage='https://images.unsplash.com/photo-1556743595-0c9952ae2645?q=80&w=2007&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D' />
    </div>

    <div>
      <h2>PFlight</h2>
      <div class="p-event__container">
        <PFlight design="p-flight" :flightDate="new Date('2024-11-7')" flightDepTime="12:30" flightArrTime="14:45"
          flightRoute="EWR-ROC" seatNumber="3B" flightType="Nonstop" flightGate="41C" flightClass="Economy" :price=156
          airline="United" />
      </div>
    </div>

    <div>
      <h2>PDropDown</h2>
      <div class="p-dropdown__container">
        <PDropDown design="p-dropdown" dropDownLabel="Airlines" :options="['United', 'Delta', 'American', 'Spirit']" />

        <PDropDown design="p-dropdown" dropDownLabel="Price" :options="['Lowest', 'Highest']" />
      </div>
    </div>

    <div>
      <h2>PProfilePic</h2>
      <div>
        <PProfilePic design="small"
          profileImage='https://media.licdn.com/dms/image/v2/D5603AQH6HnVE-AYPnw/profile-displayphoto-shrink_200_200/profile-displayphoto-shrink_200_200/0/1669255177815?e=1743638400&v=beta&t=6G9Svq9PfuMXtPFzi6KCU2Ksitp_FFvS9z9jrm1o1PY' />
        <PProfilePic design="big"
          profileImage='https://media.licdn.com/dms/image/v2/D5603AQH6HnVE-AYPnw/profile-displayphoto-shrink_200_200/profile-displayphoto-shrink_200_200/0/1669255177815?e=1743638400&v=beta&t=6G9Svq9PfuMXtPFzi6KCU2Ksitp_FFvS9z9jrm1o1PY' />
      </div>
    </div>

    <div>
      <h2>PFinanceBlock</h2>
      <PFinanceBlock design="p-finance" name="Timothy Wagner" jobTitle="Finance Manager" email="TWagner49@gmail.com"
        phoneNum="246-123-5124"
        profileImage="https://media.licdn.com/dms/image/v2/D5603AQH6HnVE-AYPnw/profile-displayphoto-shrink_200_200/profile-displayphoto-shrink_200_200/0/1669255177815?e=1743638400&v=beta&t=6G9Svq9PfuMXtPFzi6KCU2Ksitp_FFvS9z9jrm1o1PY" />
    </div>



  </div>


</template>
