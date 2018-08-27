<template>
    <div>
        <number-input 
          :defaultNumber="pureNumber" 
          @update="pureNumber = $event"
          :isValidate="true"
          errorMsg='这是错误提示语'
          :int="true"
          >
          </number-input>  
    </div>
</template>
 
<script>
import numberInput from './numberInput'
    export default {
        data() {
            pureNumber: 0
        },
        components: {
            numberInput
        },
    }
</script>
<style>

</style>
