<template>
    <div class="fireworks-container" :sound="sound"/>
</template>

<script>
import { Fireworks } from 'fireworks-js'
export default {
    mounted: function () {
        const container = document.querySelector('.fireworks-container')
        this.options = {
            target: container,
            hue: 120,
            startDelay: 1,
            minDelay: 14,
            maxDelay: 26,
            speed: 10,
            acceleration: 1.1,
            friction: 1,
            gravity: 1,
            particles: 500,
            trace: 3,
            explosion: 5,
            boundaries: {
                top: 50,
                bottom: container.clientHeight,
                left: 50,
                right: container.clientWidth
            },
            sound: {
                enable: this.sound,
                list: [
                    'explosion0.mp3',
                    'explosion1.mp3',
                    'explosion2.mp3'
                ],
                min: 4,
                max: 8
            }
        }
        this.fireworks = new Fireworks(this.options)
        this.fireworks.start()
    },
    beforeUpdate: function () {
        const container = document.querySelector('.fireworks-container')
        container.innerHTML = ''
        this.fireworks.stop()
        this.options.sound.enable = this.sound
        this.fireworks = new Fireworks({
            ...this.options,
            ...{ sound: {
                    enable: this.sound,
                    ...this.options.sound
                }
            }
        })
        this.fireworks.start()
    },
    destroyed: function () {
        this.fireworks.stop()
    },
    props: {
        sound: {
            type: Boolean,
            required: true
        }
    }
}
</script>
<style>
.fireworks-container{
    position: fixed;
    height: 100%;
    width: 100%;
    top: 0px;
    left: 0px;
    pointer-events: none;
}
</style>
