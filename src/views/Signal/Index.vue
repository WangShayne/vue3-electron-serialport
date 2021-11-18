<template>
    <n-layout-content content-style="padding: 24px;">
        <PortSet />
        <Control />
        <Meter />
    </n-layout-content>
</template>

<script>
import { defineComponent } from 'vue'
import PortSet from './PortSet.vue'
import Control from './Control.vue'
import Meter from './Meter/Index.vue'
// console.log(mMeter)

export default defineComponent({
    components: {
        PortSet,
        Control,
        Meter
    },
    setup() {
       
    },
})
</script>
