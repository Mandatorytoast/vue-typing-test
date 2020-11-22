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
        //__init defined in App.vue__
        //initialize everything to baes value and stop the countdown timer
        Event.$on("init", () => {
            this.phrase = "";
            this.firstPress = true;
            this.active = true;
            clearTimeout(this.gameTimer);
        });
    },

    methods: {
        // Everytime a key is pressed this will send a keypress event out
        handleKeyPress() {
            //if key is not shift or alt
            if (  event.key !== "Shift"
                && event.key !== "Alt") {
                //if this is the first key press 
                if (this.firstPress === true){
                    this.firstPress = false;
                    // Emit start-timer to let other components know that the countdown timer has started
                    Event.$emit("start-timer");
                    //start the 60 second timer
                    this.gameTimer = setTimeout(() => {
                        let uncorrected = 0;
                        alert("Times up");
                        this.active = false;
                        //for every typed character if the character is incorrect move uncorrected counter up
                        for (var i = 0; i < this.phrase.length; i++){
                            if (this.phrase.charAt(i) !== this.text.charAt(i)){
                                uncorrected += 1;
                            }
                        }
                        //emit time-out event with the user input and all the uncorrected erros
                        Event.$emit("time-out", {phrase: this.phrase, uncorrectedErrors: uncorrected});
                    }, 60000);
                }
                    //emit a keypress event with the full phrase that the user has input so far
                    Event.$emit("keypress", this.phrase);
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
       background-color: #E2E2E2;
       border-color: #121212;
       border-radius: 10px;
       padding: 5px;
   } 
.typing-input:focus{
        outline: none;
}
</style>
