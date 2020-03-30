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
        text: {required: true}, //This is the text to be displayed in the window
        textArr: {requred: true}, // This is the text split into an array
    },

    components: { 
        Phrase //Phrase is pretty much a pointless component as it is just a paragraph. Might delete later
    },

    methods: {
        //Emits an error when an error is made
        emitError() {
            Event.$emit("error-made");
        },

        //Emits correct when the correct key is typed
        emitCorrect() {
            Event.$emit("correct");
        },

        // This seperates the text that has already been typed and the text that
        // is still to be typed into seperate arrays for styling
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
        // If there is nothing in toTypeArr (initialized) then use the textArr prop
        // to populete it
        if (this.toTypeArr.length === 0) {
            this.toTypeArr = this.textArr;
        }
    },
    mounted() {
        this.toTypeArr = this.textArr; //on first load this populates toTypeArr
    },
    created() { 
        //__init defined in App.vue__
        //initializes everything to base values everytime Reset is pressed
        Event.$on("init", () => {
            this.isMistake = false;
            this.errors = false;
            this.typedWords = [];
            this.toTypeArr = []
        });

        //__keypress defined in TextInput.vue__
        Event.$on("keypress", data => {
            //data is the users typed text
            this.splitTyped(data) 

            //emit correct if the correct key was pressed
            if ( data === this.text.substring(0, data.length)){
                this.isMistake = false;
                this.emitCorrect();
            //emit error if the incorrect key was pressed and set isMistake to true
            } else {
                if (data.charAt(data.length - 1) !== this.text.charAt(data.length - 1)){
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
