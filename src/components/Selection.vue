<template>
    <button
        class="btn btn-link text-uppercase btn-selection"
        :class="isBadGuess && isSelected ? 'bad' : ''"
        @click="select(letter)"
        @select="select(letter)"
        :disabled="isSelected"
        :style="`color: ${color}`"
    >
        {{letter}}
        <span class="popper">
            {{nato}}
        </span>
    </button>
</template>
<script>

import converter, { NATO_PHONETIC_ALPHABET  } from 'phonetic-alphabet-converter'
export default {
    methods: {
        select (letter) {
            this.$emit('select', letter)
        }
    },
    computed: {
        nato () {
            const natoLetter = converter(this.letter, { ...NATO_PHONETIC_ALPHABET, m: 'Mancy', z: 'Zeb-ra'});
            console.log('The nato letter', natoLetter)
            return (natoLetter.length ? natoLetter[0] : '');
        }
    },
    props: {
        color: {
            type: String,
            required: true
        },
        letter: {
            type: String,
            required: true
        },
        isSelected: {
            type: Boolean,
            default: false
        },
        isBadGuess: {
            type: Boolean,
            default: false
        }
    }
}
</script>
<style lang="scss">
.btn-selection{
    position: relative;
    font-size: 3rem !important;
    transition-property: top;
    transition-duration: 1s;
    transition-timing-function: ease-in-out;
    top: 0px;

    &:disabled{
        position: relative;
        top: -50px;
        transition-property: top;
        transition-duration: 1s;
        transition-timing-function: ease-in-out;

        &.bad{
            position: relative;
            top: 50px;

            &:before{
                top: 50%;
                left: 0;
                content:  '';
                height: 4px;
                width: 100%;
                background: red;
                position: absolute;
                transform: rotate(45deg) translateY(-50%);

            }
        }
    }
    &:hover{
        .popper{
            filter: blur(0px);
            background-color: #000000;
            z-index: 5;
        }
    }
    .popper{
        filter: blur(10px);
        transition: filter 0.5s;
        transition: background-color 0.5s;
        z-index: 1;
        left: 50%;
        top: 0%;
        -webkit-transform: translateX(-50%) translateY(-50%);
        -moz-transform: translateX(-50%) translateY(-50%);
        transform: translateX(-50%) translateY(-50%);
        font-size: 14px;
        position: absolute;
        padding: 10px;
        border-radius: 5px;
        color: #ffffff;
        background-color: #FFFFFF;
        white-space: nowrap;
    }
}
</style>