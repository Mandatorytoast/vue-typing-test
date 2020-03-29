<template>
    <div :class="[this.isMistake ? 'error' : ' ',  'text-window']" >
        <Phrase class="typed">{{ showTypedText }} </Phrase>
        <Phrase class="current">{{ showCurrentWord }}</Phrase>
        <Phrase> {{ hideTypedText }}</Phrase> 
    </div> 
</template>

<script>
import Phrase from "./Phrase.vue";
export default {
    props: {
        text: {required: true},
        textArr: {requred: true},
    },

    components: { 
        Phrase
    },

    methods: {
        calculateScore() {
            Event.$emit("calculated-score", {errors: this.errors,
                length: this.text.length});
        },
        emitError() {
            Event.$emit("error-made");
        },
        emitCorrect() {
            Event.$emit("correct");
        },
        splitTyped(text) {
            this.typedWords = this.textArr.slice(0, text.split(" ").length);
            this.toTypeArr = this.textArr.slice(this.typedWords.length, this.textArr.length);
        }
    },
    data() {
        return {
            isMistake: false,
            errors: 0,
            position: 0,
            currentWordIndex: 0,
            typedWords: [],
            toTypeArr: [],
        }
    },
    beforeUpdate() {
        if (this.toTypeArr.length === 0) {
            this.toTypeArr = this.textArr;
        }
    },
    mounted() {
        this.toTypeArr = this.textArr;
    },
    created() { 
        Event.$on("init", () => {
            this.isMistake = false;
            this.errors = false;
            this.position = 0;
            this.currentWordIndex = 0;
            this.typedWords = [];
            this.toTypeArr = []
        });
        Event.$on("keypress", data => {
            //data[0] == The complete input of the user 
            //data[1] == the key pressed by the user
            this.splitTyped(data[0])
            if ( data[1] !== "Backspace" ) {
                if (data[1] === " ") {
                    if (this.text.charAt(this.position) === " "){ 
                        this.currentWordIndex += 1; 
                    }
                }
                this.position += 1;
                if (data[0].length === this.text.length) {
                    return;
                }
                else if ( data[0] === this.text.substring(0, data[0].length)){
                    this.isMistake = false;
                    this.emitCorrect();
                } else {
                    if (data[0].charAt(data[0].length - 1) !== this.text.charAt(data[0].length - 1)){
                        this.emitError();
                    }
                    this.isMistake = true;
                }
            } else {
                if (this.position > 0)
                    this.position -= 1;
                if (this.text.charAt(this.position) === " ") {
                    this.currentWordIndex -= 1;
                }
            }
        });
    },  
    computed: {
        hideTypedText() {
            return this.toTypeArr.join(" ");
        },  
        showTypedText() {
            return this.typedWords.slice(0, this.typedWords.length - 1).join(" ");
        },
        showCurrentWord() {
            return this.textArr[this.typedWords.length - 1];
        }
    },
}
</script>

<style scoped>
.text-window {
    border-width: 5px;
    border-style: solid;
    border-color: #212121;
    border-radius: 20px;
    padding: 30px;
    color: #AAAAAA;
    background-color: #121212;
    font-size: 1.1em;
}
.error {
    border-color:  #d7005f;
}
.typed {
    color: #424242;
}
.current {
    color: #121212;
    background-color: #EEEEEE;
}
</style>
