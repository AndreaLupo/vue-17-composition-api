<template>
  <section class="container">
    <h2>{{ user.name }}</h2>
    <h3>{{ user.age }}</h3>

    <button @click="setAge">Change age</button>
  </section>
</template>

<script>
import { ref, reactive, isReactive, isRef } from 'vue';

export default {
  setup() {
    // creates a reactive string - saved in an object
    // const uName = ref('Maximilian');
    // const uAge = ref(31);
    const uAge = ref(31);
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


    // data exposed to the template
    // BAD WAY to expose an object!
    /* return {
      userName: user.value.name,
      age: user.value.age
    }; */

    return {
      user: user,
      setAge: setNewAge
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