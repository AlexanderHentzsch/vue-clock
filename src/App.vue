<template>
    <div id="app" class="container w3-display-container font-comfortaa" style="height: 100%">
        &nbsp;
        <div class="w3-display-middle" style="width: 210px">
            <div>{{dayName}}, {{day}}.{{month}}.{{year}}</div>
            <div>
                <span style="font-size: 64px">{{ hour }}:{{ minute }}.</span>
                <span style="font-size: 32px">{{ second }}</span>
            </div>
        </div>
        <div style="clear:both"></div>
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
                week: ["Sonntag", "Montag", "Dienstag", "Mittwoch", "Donnerstag", "Freitag", "Samstag"]
            }
        },
        mounted() {
            this.updateTime();
            setInterval(this.updateTime, 1000);

            this.setFullScreenModeByAngle();
            if ("orientation" in screen) {
                let o = screen.orientation;
                o.addEventListener("change", this.setFullScreenModeByAngle, false);
            }
        },
        methods: {
            updateTime() {
                let ct = new Date();
                this.year = ct.getFullYear();
                this.month = ct.getMonth() + 1;
                this.day = ct.getDate();
                this.dayName = this.week[ct.getDay()];
                this.hour = ct.getHours();
                this.minute = ct.getMinutes().toString().padStart(2, "0");
                this.second = ct.getSeconds().toString().padStart(2, "0");
            },
            setFullScreenModeByAngle() {
                let DOM = document.querySelector("#app");
                let o = screen.orientation;
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
            }
        }
    }
</script>

<style scoped>
    @import url('https://fonts.googleapis.com/css?family=Comfortaa&display=swap');

    .font-comfortaa {
        font-family: 'Comfortaa', cursive;
    }

    .container {
        height: 100%;
        background-color: #000;
        color: #3E606F;
    }
</style>
