<template>
    <Page>
        <ActionBar>
            <Label text="Calculator"/>
        </ActionBar>

        <GridLayout columns="*,*,*,*" rows="30, 30, 30, 30, 30, 30">
            <label row="0" col="0" colSpan="4" class="display">{{current || '0'}}</label>
            <label @tap="clear" text="AC" row="1" col="0" class="btn"/>
            <label @tap="sign" text="+/-" row="1" col="1" class="btn"/>
            <label @tap="percent" text="%" row="1" col="2" class="btn"/>
            <label @tap="divide" text="/" row="1" col="3" class="btn operator"/>
            <label @tap="append(7)" text="7" row="2" col="0" class="btn"/>
            <label @tap="append(8)" text="8" row="2" col="1" class="btn"/>
            <label @tap="append(9)" text="9" row="2" col="2" class="btn"/>
            <label @tap="times" text="*" row="2" col="3" class="btn operator"/>
            <label @tap="append(4)" text="4" row="3" col="0" class="btn"/>
            <label @tap="append(5)" text="5" row="3" col="1" class="btn"/>
            <label @tap="append(6)" text="6" row="3" col="2" class="btn"/>
            <label @tap="plus" text="+" row="3" col="3" class="btn operator"/>
            <label @tap="append(1)" text="1" row="4" col="0" class="btn"/>
            <label @tap="append(2)" text="2" row="4" col="1" class="btn"/>
            <label @tap="append(3)" text="3" row="4" col="2" class="btn"/>
            <label @tap="minus" text="-" row="4" col="3" class="btn operator"/>
            <label @tap="append(0)" text="0" row="5" col="0" colSpan="2" class="btn zero"/>
            <label @tap="dot" text="." row="5" col="2" class="btn"/>
            <label @tap="equal" text="=" row="5" col="3" class="btn operator"/>
        </GridLayout>
    </Page>
</template>

<script>
  export default {
    data(){
        return {
            previous: null,
            current: "",
            operator: null,
            operatorTaped: false,
        }
    },
    methods: {
        clear(){
            this.current = '';
        },
        sign(){
            this.current = this.current.charAt(0) === '-' ?
            this.current.slice(1) : `-${this.current}`;
        },
        percent(){
            this.current = `${parseFloat(this.current) / 100}`;
        },
        append(number){
            if (this.operatorTaped){
                this.current="";
                this.operatorTaped = false;
            }
            this.current = `${this.current}${number}`;
        },
        dot(){
            if (this.current.indexOf('.') === -1) {
                this.append('.')
            }
        },
        setPrevious(){
            this.previous = this.current;
            this.operatorTaped = true;
        },
        divide(){
            this.operator = (a, b) => a / b;
            this.setPrevious();
        },
        times(){
            this.operator = (a, b) => a * b;
            this.setPrevious();
        },
        plus(){
            this.operator = (a, b) => a + b;
            this.setPrevious();
        },
        minus(){
            this.operator = (a, b) => a - b;
            this.setPrevious();
        },
        equal() {
            this.current = `${this.operator(
                parseFloat(this.previous),
                parseFloat(this.current)
            )}`;
        }
    },
  };
</script>

<style scoped lang="scss">
    @import '@nativescript/theme/scss/variables/blue';

    // Custom styles

    .display {
        text-align: right;
        font-size: 15px;
        background-color: #333;
        color: white;
    }

    .zero {
        grid-column: 1/3;
    }

    .btn {
        text-align: center;
        font-size: 15px;
        background-color: #eee;
        border-width: 1px;
        border-color: #333;
    }

    .operator {
        background-color: orange;
        color: white;
    }
</style>
