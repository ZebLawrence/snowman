<template>
    <nav class="navbar navbar-light bg-light">
        <div class="form-inline">
            <button
                class="btn btn-success"
                @click="isEditingPhrase = true"
                v-if="!isEditingPhrase"
            >
                Update Phrase
            </button>
            <template v-if="isEditingPhrase">
                <input
                    class="form-control mx-sm-2"
                    type="password"
                    aria-label="Search"
                    autocomplete="off"
                    v-model="phrase"
                />
                <button
                    class="btn btn-primary mr-sm-2"
                    @click="handleSave"
                >
                    Save
                </button>
                <button
                    class="btn btn-success mr-sm-2"
                    @click="generateRandom"
                >
                    Randomize
                </button>
                <button
                    class="btn btn-danger"
                    @click="handleCancel"
                >
                    Cancel
                </button>
            </template>
        </div>
        <div class="form-inline">
            <label class="mr-sm-2" for="sound-check">Sound</label>
            <input
                @change="setSound"
                :checked="sound"
                class="form-control mr-sm-2"
                id="sound-check"
                type="checkbox" />
            <select
                class="custom-select custom-select-sm mr-sm-2"
                v-model="difficulty"
                @change="selectDifficulty"
            >
                <option value="easy">Easy</option>
                <option value="medium">Medium</option>
                <option value="hard">Hard</option>
            </select>
            <div class="input-group">
                <input 
                    type="color"
                    class="mr-sm-2"
                    v-model="color"
                    @input="selectColor"
                    @change="changeColor"
                    aria-label="Color"
                    autocomplete="off"
                />
            </div>
            <button
                class="btn btn-warning"
                @click="$emit('reset')"
            >
                Reset
            </button>
        </div>
    </nav>
</template>
<script>
import { rword } from 'rword'
export default {
    components: {
    },
    data () {
        return {
            isEditingPhrase: false,
            phrase: null
        }
    },
    methods: {
        selectColor (e) {
            e.target.style.color = e.target.value;
        },
        selectDifficulty (e) {
            this.$emit('difficultyUpdate', e.target.value)
        },
        changeColor (e) {
            this.$emit('colorUpdate', e.target.value)
        },
        setSound () {
            this.$emit('soundUpdate')
        },
        generateRandom () {
            this.phrase = rword.generate()
            this.handleSave()
        },
        handleCancel () {
            this.phrase = ''
            this.isEditingPhrase = false
        },
        handleSave () {
            this.$emit('phraseUpdate', this.phrase)
            this.phrase = ''
            this.isEditingPhrase = false
        }
    },
    props: {
        sound: {
            type: Boolean,
            required: true
        },
        color: {
            type: String,
            required: true
        },
        difficulty: {
            type: String,
            required: true
        }
    }
}
</script>
<style>
#app .navbar {
    z-index: 1;
}
</style>