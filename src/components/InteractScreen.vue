<template>
    <div class="screen">
        <button @click="turnAll">View all</button>
        <div class="flex-container" :style="{ 'max-width':maxWidth + 'px'}">
            <card-flip 
                v-for="(card, index) in cardsContext" 
                :key="index" 
                :ref="`card-${index}`"
                :imgBackFaceUrl="`images/${card}.png`"
                :card="{ index: index, value: card }"
                :rules="rules"
                @onFliped="checkRule($event)"
            />
        </div>
    </div>
</template>

<script>
import CardFlip from "./Card.vue";

export default {
    props: {
        cardsContext: {
            type: Array,
            default: function() {
                return [];
            }
        }
    },
    components: {
        CardFlip,
    },
    data() {
        return {
            rules: [],
            count: 0,
            maxWidth: Math.sqrt(this.cardsContext.length) * 100 + 150,
            countSuggest: 0,
        }
    },
    methods: {
        checkRule(card) {
            if(this.rules.length == 2) return false;
            this.rules.push(card);
            let i = 0;
            for(i ; i<this.rules.length;i++) {
                this.$refs[`card-${this.rules[i].index}`][0].onDisabledFlip();
            }

            if(this.rules.length == 2 && this.rules[0].value == this.rules[1].value) {
                console.log('right . . .');
                this.$refs[`card-${this.rules[0].index}`][0].onDisabledFlip();
                this.$refs[`card-${this.rules[1].index}`][0].onDisabledFlip();

                this.rules = [];
                this.count++;
                if(this.count == this.cardsContext.length / 2) {
                    setTimeout(()=>{
                        this.$emit("onFinish");
                    }, 920);
                }
            }
            if(this.rules.length == 2 && this.rules[0].value != this.rules[1].value) {
                console.log('wrong . . .');
                setTimeout(() => {
                    this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
                    this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();

                    this.rules = [];
                }, 800)
                this.$refs[`card-${this.rules[0].index}`][0].onEnabledFlip();
                this.$refs[`card-${this.rules[1].index}`][0].onEnabledFlip();
            } else return false;
        },

        turnAll() {
            if(this.countSuggest>1) {
                return alert("Nạp VIP để dùng nhiều trợ giúp hơn")
            }
            let i = 0;
            for(i ; i<this.cardsContext.length;i++) {
                console.log(this.$refs[`card-${i}`]);
                this.$refs[`card-${i}`][0].flipFrontEnabledCard();
            }
            let j = 0;
            setTimeout(()=>{

                for(j ; j<this.cardsContext.length;j++) {
                    this.$refs[`card-${j}`][0].flipBackEnabledCard() 
                }
            },4000);
            this.countSuggest++;
        }
    },
}
</script>

<style scoped>
.screen {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    flex-direction: column;
}
.flex-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    padding: 2%;
}
.screen button {
  font: var(--font);
  background: transparent;
  box-shadow: none;
  margin: 1rem;
  padding: 1rem 1.25rem;
  border-radius: 0.5rem;
  font-size: 1.25rem;
  cursor: pointer;
}

.screen button:hover {
  background-color: gray;
  color: var(--dark);
}
</style>