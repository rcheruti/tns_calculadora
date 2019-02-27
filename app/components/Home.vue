<template>
    <Page class="page">
        <!--
        <ActionBar class="action-bar">
            <Label class="action-bar-title" text="Home"></Label>
        </ActionBar>
        -->

        <FlexboxLayout flexDirection="column">
            <StackLayout flexGrow="1" class="resultadoBox">
                <TextField ref="textInput" hint="0.00" keyboardType="none" class="calc" :text="calcStr"/>
                <Label class="resultado" :text="resultado"/>
            </StackLayout>

            <!--  Teclado  -->
            <FlexboxLayout flexGrow="2.5" flexDirection="row" class="teclado">
                <FlexboxLayout v-for="(lista, listaKey) in teclado" :key="listaKey" flexGrow="1" flexDirection="column">
                    <Button v-for="(x, key) in lista" :key="key" flexGrow="1" 
                        :text="x.text" :class="'btn ' + x.class" @tap="btnClick( $event, x.symbol || x.text )"></Button>
                </FlexboxLayout>
            </FlexboxLayout>
        </FlexboxLayout>
    </Page>
</template>

<script>
import { Calculadora } from '../services/Calculadora'

export default {
    data() {
        return {
            calcStr:    '',
            teclado: [
                [
                    { text: '(', symbol: '', class: '' } ,
                    { text: '1', symbol: '', class: '' } ,
                    { text: '4', symbol: '', class: '' } ,
                    { text: '7', symbol: '', class: '' } ,
                    { text: '0', symbol: '', class: '' } ,
                ],
                [
                    { text: ')', symbol: '', class: '' } ,
                    { text: '2', symbol: '', class: '' } ,
                    { text: '5', symbol: '', class: '' } ,
                    { text: '8', symbol: '', class: '' } ,
                    { text: ',', symbol: '.', class: '' } ,
                ],
                [
                    { text: '',  symbol: '', class: '' } ,
                    { text: '3', symbol: '', class: '' } ,
                    { text: '6', symbol: '', class: '' } ,
                    { text: '9', symbol: '', class: '' } ,
                    { text: '=', symbol: '', class: '' } ,
                ],
                [
                    { text: 'C', symbol: '',    class: 'ultimo' } ,
                    { text: 'x', symbol: ' x ', class: 'ultimo' } ,
                    { text: '/', symbol: ' / ', class: 'ultimo' } ,
                    { text: '-', symbol: ' - ', class: 'ultimo' } ,
                    { text: '+', symbol: ' + ', class: 'ultimo' } ,
                ],
            ]
        }
    },
    mounted() {
        /*
        if ( this.$refs.textInput.nativeView.setInputType ) {
            this.$refs.textInput.nativeView.setInputType(0)
        }
        if ( this.$refs.textInput.nativeView.inputView ) {
            let view = UIView.alloc().initWithFrame(CGRectZero) // native ??
            this.$refs.textInput.nativeView.inputView = view
        }
        */
    },
    computed: {
        resultado() {
            try {
                return Calculadora.parse( this.calcStr.replace(/x/g,'*') )
            } catch(ex) {
                return ''
            }
        }
    },
    methods: {
        btnClick(event, expStr) {
            switch(expStr) {
                case 'C': this.limpar(); break;
                case '=': this.calcular(); break;
                default: this.calcStr += expStr;
            }
            console.log( event )
        },
        calcular() {

        },
        limpar() {
            this.calcStr = ''
        }
    }
};
</script>

<style scoped lang="scss">
@import '../app-variables';

.resultadoBox {
    padding: 30 20;

    .calc {
        font-size: 28;
        margin-bottom: 20;
    }
}

.teclado {
    
    .btn {
        border-left-width: 1;
        border-left-color: #efefef;
        background-color: #efefef;
        margin: 0 ;
        z-index: 0;
        font-size: 22;
        color: #000;
    }
    .ultimo {
        color: $accent-light;
        border-left-color: #ccc;
    }
}

</style>
