<template>
    <div id="app" style="height: 100%">
        <div class="container-player">
            <video id="player" src="@/assets/clip.mp4" width="10px" height="10px" loop></video>
        </div>

        <div class="container w3-display-container font-comfortaa" style="height: 100%">
            <div class="w3-display-middle" style="width: 220px" @click="togglePlaying()">
                <div style="padding-left: 17px;">{{dayName}}, {{day}}.{{month}}.{{year}}</div>
                <div>
                    <i class="material-icons" :style="styleShowVideoPausedIcon">visibility_off</i>
                    <span style="font-size: 64px">{{ hour }}:{{ minute }}.</span>
                    <span style="font-size: 32px">{{ second }}</span>
                </div>
            </div>
            <div style="clear:both"></div>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'app',
        data() {
            return {
                year: "2018",
                month: "11",
                day: "20",
                dayName: "Sonntag",
                hour: "",
                minute: "",
                second: "",
                week: ["Sonntag", "Montag", "Dienstag", "Mittwoch", "Donnerstag", "Freitag", "Samstag"],
                statVideo: 'paused',
                fontSize: 16
            }
        },
        computed: {
            styleShowVideoPausedIcon() {
                const style = {};
                style.fontSize = this.fontSize + 'px';
                style.visibility = (this.statVideo === 'paused') ? 'visible' : 'hidden';
                return style;
            }
        },
        mounted() {
            this.updateTime();
            setInterval(this.updateTime, 1000);

            this.setFullScreenModeByAngle();
            if ("orientation" in screen) {
                const o = screen.orientation;
                o.addEventListener("change", this.setFullScreenModeByAngle, false);
            }
        },
        methods: {
            updateTime() {
                const ct = new Date();
                this.year = ct.getFullYear();
                this.month = ct.getMonth() + 1;
                this.day = ct.getDate();
                this.dayName = this.week[ct.getDay()];
                this.hour = ct.getHours();
                this.minute = ct.getMinutes().toString().padStart(2, "0");
                this.second = ct.getSeconds().toString().padStart(2, "0");
            },
            setFullScreenModeByAngle() {
                const DOM = document.querySelector("#app");
                const o = screen.orientation;
                if (o.angle !== 0 && o.angle !== 180) {
                    if (!document.fullscreenElement) {
                        DOM.webkitRequestFullscreen();
                        DOM.requestFullscreen();
                    }
                } else {
                    if (document.fullscreenElement) {
                        document.exitFullscreen();
                    }
                }
            },
            togglePlaying() {
                const DOM = document.querySelector("#player");
                (DOM.paused) ? DOM.play() : DOM.pause();
                this.statVideo = (DOM.paused) ? 'paused' : 'playing';
            }
        }
    }
</script>

<style scoped>
    @import url('https://fonts.googleapis.com/css?family=Comfortaa|Material+Icons&display=swap');

    .font-comfortaa {
        font-family: 'Comfortaa', cursive;
    }

    .container {
        height: 100%;
        background-color: #000;
        color: #3E606F;
    }

    .container-player {
        position: fixed;
        top: -200px;
    }

    .pause-icon {
        font-size: 16px;
        position: relative;
        bottom: -3px;
    }
</style>
