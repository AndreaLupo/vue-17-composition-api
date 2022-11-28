<template>
  <section class="container">
    <user-data :first-name="firstName" :last-name="lastName" :age="user.age"></user-data>
    <button @click="setAge">Change age</button>

    <div>
      <input type="text" placeholder="First Name" v-model="firstName"/>
      <input type="text" placeholder="Last Name" v-model="lastName"/>
      <h1>{{ fullName }}</h1>
    </div>
  </section>
</template>

<script>
import { ref, reactive, isReactive, isRef
        , computed, watch, toRefs } from 'vue';

import UserData from './components/UserData.vue';

export default {
  components: {
    UserData
  },
  setup() {
    // creates a reactive string - saved in an object
    // const uName = ref('Maximilian');
    // const uAge = ref(31);
    const uAge = ref(31);
    const firstName = ref('');
    const lastName = ref('');
    const user = reactive({
      name: 'Maximilian',
      age: 31
    });

    console.log('Age:', uAge); // ref (non proxy)
    console.log('User', user); // proxy

    console.log(uAge.value); // simple variable!
    console.log(user.name); // simple variable!

    console.log(isRef(uAge), isReactive(user));
    console.log(isRef(uAge.value), isReactive(user.name));

    setTimeout(function() {
      // uName.value = 'Max';
      // uAge.value++;
      /* user only on object with ref 
      user.value.name = 'Max';
      user.value.age++; 
      */

      // valid only with reactive
      user.name = 'Max';
      user.age++; 
    }, 2000);

    toRefs(user);

    function setNewAge() {
      user.age = 40;
    }

    function setFirstName(event) {
      firstName.value = event.target.value;
    }

    function setLastName(event) {
      lastName.value = event.target.value;
    }

    const fullName = computed(function() {
      return firstName.value + ' ' + lastName.value;
    });

    watch(uAge, function(newValue, oldValue) {
      console.log('New value', newValue);
      console.log('Old value', oldValue);
    });

    watch([uAge, user], function(newValues, oldValues) {
      console.log('Old age:', oldValues[0]);
      console.log('New age:', newValues[0]);
      
      console.log('Old user', oldValues[1]);
      console.log('New user', newValues[1]);
    });

    // data exposed to the template
    // BAD WAY to expose an object!
    /* return {
      userName: user.value.name,
      age: user.value.age
    }; */

    return {
      user: user,
      setAge: setNewAge,
      setFirstName,
      setLastName,
      fullName,
      firstName,
      lastName
    }
  }
  /* data() {
    return {
      userName: 'Maximilian',
    };
  }, 
  methods: {
    setNewAge() {
      this.age = 40;
    }
  }
  */
};
</script>

<style>
* {
  box-sizing: border-box;
}

html {
  font-family: sans-serif;
}

body {
  margin: 0;
}

.container {
  margin: 3rem auto;
  max-width: 30rem;
  border-radius: 12px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 1rem;
  text-align: center;
}
</style>