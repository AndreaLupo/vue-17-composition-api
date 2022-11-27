<template>
  <section class="container">
    <h2>{{ user.name }}</h2>
    <h3>{{ user.age }}</h3>

    <button @click="setAge">Change age</button>

    <div>
      <input type="text" placeholder="First Name" @input="setFirstName"/>
      <input type="text" placeholder="Last Name" @input="setLastName"/>
      <h1>{{ fullName }}</h1>
    </div>
  </section>
</template>

<script>
import { ref, reactive, isReactive, isRef
        , computed } from 'vue';

export default {
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
      fullName
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