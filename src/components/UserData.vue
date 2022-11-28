<template>
    <h2>{{ uName }}</h2>
    <h3>{{ age }}</h3>
</template>

<script>
import { computed } from 'vue';

export default {
    props: ['firstName', 'lastName', 'age'],
    setup(props, context) {
        const uName = computed( function() {
            return props.firstName + ' ' + props.lastName;
        });

        console.log(context);
        // context.emit('save-data', 1);

        return {
            uName
        };
    }
    /* computed: {
        username() {
            return this.firstName + ' ' + this.lastName;
        }
    } */
};


</script>