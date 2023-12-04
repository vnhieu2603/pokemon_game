<template>
    <h1>
        Interact Component here ...
    </h1>
    <card-flip 
        v-for="(card, index) in cardsContext" 
        :key="index" 
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index: index, value: card }"
        @onFliped="checkRule($event)"
    />
    <h1>{{ count }}</h1>
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
        }
    },
    methods: {
        checkRule(card) {
            if(this.rules.length == 2) return false;
            this.rules.push(card);
            if(this.rules.length == 2 && this.rules[0].value == this.rules[1].value) {
                console.log('right . . .');
                this.$refs[`card-${this.rules[0].index}`][0].onDisabledFlip();
                this.$refs[`card-${this.rules[1].index}`][0].onDisabledFlip();

                this.rules.pop()
                this.rules.pop()
                console.log(this.rules);
                this.count++;
            }
            if(this.rules.length == 2 && this.rules[0].value != this.rules[1].value) {
                console.log('wrong . . .');
                console.log(this.$refs[`card-${this.rules[0].index}`])
                setTimeout(() => {
                    this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
                    this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();

                    this.rules.pop()
                    this.rules.pop()
                }, 800)
                

                this.count++;
            } else return false;
        }
    },
}
</script>