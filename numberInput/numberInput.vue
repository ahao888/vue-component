<style scoped>
#numberInput .warning{
        border-color: #f56c6c !important;
}
#numberInput .success{
    border-color: #67c23a !important;
}
#numberInput .el-input__inner{
    -webkit-appearance: none;
    background-color: #fff;
    background-image: none;
    border-radius: 4px;
    border: 1px solid #dcdfe6;
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    color: #606266;
    display: inline-block;
    font-size: inherit;
    height: 40px;
    line-height: 40px;
    outline: 0;
    padding: 0 15px;
    -webkit-transition: border-color .2s cubic-bezier(.645,.045,.355,1);
    transition: border-color .2s cubic-bezier(.645,.045,.355,1);
    width: 100%;
}
.errorMsg{
    color: #f56c6c;
    font-size: 12px;
    line-height: 1;
    padding-top: 2px;
    padding-bottom: 15px;
}
</style>
<template>
    <div id="numberInput">
        <input
            class="el-input__inner"
            placeholder="请输入纯数字"
            @input="handleInput('inputVal', $event)"
            v-model="inputVal"
            @blur="handleBlur"
            :class="validate"
            >
        <div v-if="isError"
             class="errorMsg">
             {{ errorMsg }}
        </div>
    </div>
</template>
<script>
export default {
    data() {
        return {
            inputVal: this.defaultNumber,
            status: 1,
            isError: false,
        }
    },
    computed:{
        validate() {
            if (this.status === 1) {
                return null
            } else if (this.status === 2) {
                return 'warning'
            } else if (this.status === 3) {
                return 'success'
            }
        }
    },
    props:{
        defaultNumber:{
            default:'',
        },
        isValidate: {
            default: true,
        },
        errorMsg: {
            default: '请输入内容'
        },
        int: {
            default: true
        },
        // 支持正负 true：正 false:负 默认支持负数
        pm: {
          default: false,
          type: Boolean
        },
        // 支持小数位
        decimal: {
          default: 2,
          type: Number
        },
    },
    methods:{
        handleBlur() {
            if (this.isValidate) {
                if (this.inputVal == '') {
                    this.status = 2
                    this.isError = true
                } else {
                    this.status = 3
                    this.isError = false
                }
            }
        },
        handleInput(val, e) {
            if (this.int) {
              let value = e.target.value.replace(/\D+/, '')
              var match = e.target.value.match(/-/);
              if (match) {
                value = this.pm ? value:-value
              }
              this[val] = value
            } else {
              let reg = this.pm ? '\\d+\\.?\\d*' : '-?\\d+\\.?(\\d*)?'
              let matchArr = e.target.value.match(reg)
              if (!matchArr) {
                this[val] = ''
              }
              this[val] = Number(matchArr[0]).toFixed(this.decimal)
            }
            this.$emit('update',this[val])
        }
    }
}
</script>



