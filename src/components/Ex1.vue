<script>
    //import component
    import SoundIcon from './subcomponents/SoundIcon.vue';
    export default { 
        components: {
            SoundIcon
        },
        data() {
            return {
                soundLevel: 0
            }
        } 
    }
</script>

<template>

   Sound level
    <input type="number" min="0" max="3" v-model="soundLevel"> 
    <!-- TODO: Add Code Here to make use of the component -->
    <sound-icon :level="soundLevel"></sound-icon>
    

</template>

