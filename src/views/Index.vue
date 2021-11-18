<template>
    <n-layout has-sider  position="absolute">
        <n-layout-sider content-style="padding: 24px;" width="220px" bordered>
            <n-anchor>
                <n-anchor-link title="单项校表" href="#Props" />
            </n-anchor>
        </n-layout-sider>
        <n-layout>
            <!-- <n-layout-header>颐和园路</n-layout-header> -->
            <n-layout-content content-style="padding: 24px;">
               <Signal />
            </n-layout-content>
            <!-- <n-layout-footer>成府路</n-layout-footer> -->
        </n-layout>
    </n-layout>
</template>

<script>
console.log("vite")
import { defineComponent } from 'vue'
import Signal from './Signal/Index.vue'

// const SerialPort = require('serialport')
// console.log(SerialPort)
import SerialPort from 'serialport'
console.log(SerialPort)
// const port = new serialport('COM4', {
//   baudRate: 9600
// })

// Switches the port into "flowing mode"
// port.on('data', function (data) {
//     console.log('Data:', data)
//   })
export default defineComponent({
    components:{
        Signal
    }
})

</script>
