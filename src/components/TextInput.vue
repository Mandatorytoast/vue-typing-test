<template>
   <input class="typing-input" type="text" v-model="phrase" @keyup="handleKeyPress" :disabled="!this.active"> 
</template>

<script>
export default {
    props: {
        text: {required: true},
    },
    data() {
        return {
            phrase: "",
            firstPress: true,
            active: true,
            gameTimer: null,
        }
    },

    created() {
        Event.$on("init", () => {
            this.phrase = "";
            this.firstPress = true;
            this.active = true;
            clearTimeout(this.gameTimer);
        });
    },

    methods: {
        handleKeyPress() {
            if (  event.key !== "Shift"
                && event.key !== "Alt") {
                if (this.firstPress === true){
                    this.firstPress = false;
                    Event.$emit("start-timer");
                    this.gameTimer = setTimeout(() => {
                        let uncorrected = 0;
                        alert("Times up");
                        this.active = false;
                        for (var i = 0; i < this.phrase.length; i++){
                            if (this.phrase.charAt(i) !== this.text.charAt(i)){
                                uncorrected += 1;
                            }
                        }
                        Event.$emit("time-out", {phrase: this.phrase, uncorrectedErrors: uncorrected});
                    }, 60000);
                }
                    Event.$emit("keypress", [this.phrase, event.key]);
            }
        },    
    }, 
}

</script>
<style scoped>
   .typing-input{
       width: 80%;
       height: 35px;
       margin-right: 20px;
       margin-top: 40px;
       display: inline;
       background-color: #CCCCCC;
       border-color: #121212;
       border-radius: 10px;
       padding: 5px;
   } 
</style>
