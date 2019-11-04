<template>
    <div class="calculator">
        <Display :value="displayValue"/>
        <Button label="AC" triple @onClick="clearMemory" />
        <Button label="/" operation @onClick="setOperation"/>
        <Button label="7" @onClick="addDigit"/>
        <Button label="8" @onClick="addDigit"/>
        <Button label="9" @onClick="addDigit"/>
        <Button label="*" operation @onClick="setOperation"/>
        <Button label="4" @onClick="addDigit"/>
        <Button label="5" @onClick="addDigit"/>
        <Button label="6" @onClick="addDigit"/>
        <Button label="-" operation @onClick="setOperation"/>
        <Button label="1" @onClick="addDigit"/>
        <Button label="2" @onClick="addDigit"/>
        <Button label="3" @onClick="addDigit"/>
        <Button label="+" operation @onClick="setOperation"/>
        <Button label="0" double @onClick="addDigit"/>
        <Button label="." operation @onClick="addDigit"/>
        <Button label="=" operation @onClick="setOperation"/>
    </div>
</template>

<script>
import Display from '../components/Display'
import Button from '../components/Button'
export default {
    data() {
        return {
            displayValue: '0',
            clearDisplay: false,
            operation: null,
            values: [0, 0],
            current: 0,
            botaoAnteriorOperacaco: false
        }
    },
    components: { Display, Button },
    methods: {
        clearMemory() {
            // fazer o objeto voltar ao estado inicial
            Object.assign(this.$data, this.$options.data())
        },
        setOperation(operation) {
            if (this.current === 0 || this.botaoAnteriorOperacaco) {
                this.operation = operation
                this.current = 1
                this.clearDisplay = true
            } else {
                try {
                    this.values[0] = eval(
                        `${this.values[0]} ${this.operation} ${this.values[1]}`
                    )
                } catch (e) {
                    this.$emit('onError', e)
                }
                this.values[1] = 0
                this.displayValue = this.values[0]
                this.operation = operation
                this.current = operation === "=" ? 0 : 1
                this.clearDisplay = true
            }
            this.botaoAnteriorOperacaco = true
        },
        addDigit(n) {
            if (n === '.' && this.displayValue.includes(".")) {
                return
            }
            const clearDisplay = this.displayValue === "0" || this.clearDisplay
            const currentValue = clearDisplay ? "" : this.displayValue
            const displayValue = currentValue + n
            this.displayValue = displayValue
            this.clearDisplay = false
            this.values[this.current] = displayValue
            this.botaoAnteriorOperacaco = false
        }
    }
}
</script>

<style>
.calculator {
    height: 320px;
    width: 235px;
    border-radius: 5px;
    overflow: hidden;
    display: grid;
    grid-template-columns: repeat(4, 25%);
    grid-template-rows: 1fr 48px 48px 48px 48px 48px;
}
</style>