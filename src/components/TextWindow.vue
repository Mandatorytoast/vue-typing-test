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
            this.typedWords = [];
            this.toTypeArr = []
        });
        Event.$on("keypress", data => {
            //data[0] == The complete input of the user 
            //data[1] == the key pressed by the user
            this.splitTyped(data[0])
                if ( data[0] === this.text.substring(0, data[0].length)){
                    this.isMistake = false;
                    this.emitCorrect();
                } else {
                    if (data[0].charAt(data[0].length - 1) !== this.text.charAt(data[0].length - 1)){
                        this.emitError();
                    }
                    this.isMistake = true;
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
    border-width: 6px;
    border-style: solid;
    border-color: #E2E2E2;
    border-radius: 20px;
    padding: 30px;
    color: #121212;
    background-color: #E2E2E2;
    font-size: 1.1em;
}
.error {
    border-color:  #d7005f;
}
.typed {
    color: #959595;
}
.current {
    color: #121212;
    background-color: #65ccb8;
}
</style>
