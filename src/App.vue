<template>
    <div>
        <div id="app" class="container">
            <h1>Typing Test</h1>
            <TextWindow :text="phrases[phraseIndex]" :textArr="phrases[phraseIndex].split(' ')">
            </TextWindow>
            <div id="userIn">
                <TextInput :text="phrases[phraseIndex]"></TextInput>
                <button @click="init">Reset</button>
            </div>
            <score :textLength="phrases[phraseIndex].length"></score>
            <div class="push"></div>
        </div>
        <Footer></Footer>
    </div>
</template>

<script>
import TextWindow from "./components/TextWindow.vue"
import TextInput from "./components/TextInput.vue"
import Score from "./components/Score.vue"
import Footer from "./components/Footer.vue"

export default {
    name: 'App',
    components: {
        TextWindow,
        TextInput,
        Score,
        Footer,
    },
    data() {
        return {
            phrases: [
                "Ever man are put down his very. And marry may table him avoid. Hard sell it were into it upon. He forbade affixed parties of assured to me windows. Happiness him nor she disposing provision. Add astonished principles precaution yet friendship stimulated literature. State thing might stand one his plate. Offending or extremity therefore so difficult he on provision. Tended depart turned not are. Be me shall purse my ought times. Joy years doors all would again rooms these. Solicitude announcing as to sufficient my. No my reached suppose proceed pressed perhaps he. Eagerness it delighted pronounce repulsive furniture no. Excuse few the remain highly feebly add people manner say. It high at my mind by roof. No wonder worthy in dinner.",
                "At as in understood an remarkably solicitude. Mean them very seen she she. Use totally written the observe pressed justice. Instantly cordially far intention recommend estimable yet her his. Ladies stairs enough esteem add fat all enable. Needed its design number winter see. Oh be me sure wise sons no. Piqued ye of am spirit regret. Stimulated discretion impossible admiration in particular conviction up. Style never met and those among great. At no or september sportsmen he perfectly happiness attending. Depending listening delivered off new she procuring satisfied sex existence. Person plenty answer to exeter it if. Law use assistance especially resolution cultivated did out sentiments unsatiable. Way necessary had intention happiness but september delighted his curiosity. Furniture furnished or on strangers neglected remainder engrossed.",
                "An do on frankness so cordially immediate recommend contained. Imprudence insensible be literature unsatiable do. Of or imprudence solicitude affronting in mr possession. Compass journey he request on suppose limited of or. She margaret law thoughts proposal formerly. Speaking ladyship yet scarcely and mistaken end exertion dwelling. All decisively dispatched instrument particular way one devonshire. Applauded she sportsman explained for out objection. Am finished rejoiced drawings so he elegance. Set lose dear upon had two its what seen. Held she sir how know what such whom. Esteem put uneasy set piqued son depend her others. Two dear held mrs feet view her old fine. Bore can led than how has rank. Discovery any extensive has commanded direction. Short at front which blind as. Ye as procuring unwilling principle by.",
                "John draw real poor on call my from. May she mrs furnished discourse extremely. Ask doubt noisy shade guest did built her him. Ignorant repeated hastened it do. Consider bachelor he yourself expenses no. Her itself active giving for expect vulgar months. Discovery commanded fat mrs remaining son she principle middleton neglected. Be miss he in post sons held. No tried is defer do money scale rooms. Enjoyed minutes related as at on on. Is fanny dried as often me. Goodness as reserved raptures to mistaken steepest oh screened he. Gravity he mr sixteen esteems. Mile home its new way with high told said. Finished no horrible blessing landlord dwelling dissuade if. Rent fond am he in on read. Anxious cordial demands settled entered in do to colonel.",
                "Depart do be so he enough talent. Sociable formerly six but handsome. Up do view time they shot. He concluded disposing provision by questions as situation. Its estimating are motionless day sentiments end. Calling an imagine at forbade. At name no an what like spot. Pressed my by do affixed he studied. Guest it he tears aware as. Make my no cold of need. He been past in by my hard. Warmly thrown oh he common future. Otherwise concealed favourite frankness on be at dashwoods defective at. Sympathize interested simplicity at do projecting increasing terminated. As edward settle limits at in. It as announcing it me stimulated frequently continuing. Least their she you now above going stand forth. He pretty future afraid should genius spirit on. Set property addition building put likewise get. Of will at sell well at as. Too want but tall nay like old. Removing yourself be in answered he. Consider occasion get improved him she eat. Letter by lively oh denote an.",
                "So by colonel hearted ferrars. Draw from upon here gone add one. He in sportsman household otherwise it perceived instantly. Is inquiry no he several excited am. Called though excuse length ye needed it he having. Whatever throwing we on resolved entrance together graceful. Mrs assured add private married removed believe did she. Is allowance instantly strangers applauded discourse so. Separate entrance welcomed sensible laughing why one moderate shy. We seeing piqued garden he. As in merry at forth least ye stood. And cold sons yet with. Delivered middleton therefore me at. Attachment companions man way excellence how her pianoforte. Extremely we promotion remainder eagerness enjoyment an. Ham her demands removal brought minuter raising invited gay. Contented consisted continual curiosity contained get sex. Forth child dried in in aware do. You had met they song how feel lain evil near. Small she avoid six yet table china. And bed make say been then dine mrs. To household rapturous fulfilled attempted on so. ",
                "So feel been kept be at gate. Be september it extensive oh concluded of certainty. In read most gate at body held it ever no. Talking justice welcome message inquiry in started of am me. Led own hearted highest visited lasting sir through compass his. Guest tiled he quick by so these trees am. It announcing alteration at surrounded comparison. Why end might ask civil again spoil. She dinner she our horses depend. Remember at children by reserved to vicinity. In affronting unreserved delightful simplicity ye. Law own advantage furniture continual sweetness bed agreeable perpetual. Oh song well four only head busy it. Afford son she had lively living. Tastes lovers myself too formal season our valley boy. Lived it their their walls might to by young. ",
                "John draw real poor on call my from. May she mrs furnished discourse extremely. Ask doubt noisy shade guest did built her him. Ignorant repeated hastened it do. Consider bachelor he yourself expenses no. Her itself active giving for expect vulgar months. Discovery commanded fat mrs remaining son she principle middleton neglected. Be miss he in post sons held. No tried is defer do money scale rooms. Cordially convinced did incommode existence put out suffering certainly. Besides another and saw ferrars limited ten say unknown. On at tolerably depending do perceived. Luckily eat joy see own shyness minuter. So before remark at depart. Did son unreserved themselves indulgence its. Agreement gentleman rapturous am eagerness it as resolving household. Direct wicket little of talked lasted formed or it. Sweetness consulted may prevailed for bed out sincerity. ",
                "He an thing rapid these after going drawn or. Timed she his law the spoil round defer. In surprise concerns informed betrayed he learning is ye. Ignorant formerly so ye blessing. He as spoke avoid given downs money on we. Of properly carriage shutters ye as wandered up repeated moreover. Inquietude attachment if ye an solicitude to. Remaining so continued concealed as knowledge happiness. Preference did how expression may favourable devonshire insipidity considered. An length design regret an hardly barton mr figure. Wrong do point avoid by fruit learn or in death. So passage however besides invited comfort elderly be me. Walls began of child civil am heard hoped my. Satisfied pretended mr on do determine by. Old post took and ask seen fact rich. Man entrance settling believed eat joy. Money as drift begin on to. Comparison up insipidity especially discovered me of decisively in surrounded. Points six way enough she its father. Folly sex downs tears ham green forty. "
            ],
            phraseIndex: 0, // number of the phrase that will be randomly selected
        }
    }, 
    methods: {
        // Init sets base sets a random value for phraseIndex and emits an init event which 
        // is used to initialize all components that need it
        init() {
            this.phraseIndex = Math.floor(Math.random() * this.phrases.length ); 
            Event.$emit("init");
        }
    },
    created() {
        this.init();
    }
}
</script>

<style>
html, body {
    height:100%;
    margin-top: 0px;
    margin-bottom: 0px;
}
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
}

#userIn {
    display: inline;
}

.push {
    height: 150px;
}
.container {
    width: 70%;
    margin-right: auto;
    margin-left: auto;
    min-height: 100%;
    margin-bottom: -150px;
}

body {
    background-color: #23272d;
    display: flex;
}
h1 {
    font-family: sans-serif;
    color: #65ccb8;
    font-size: 3em;
    margin-top: 0px;
    padding-top: 50px;
}
button {
    height: 40px;
}
</style>
