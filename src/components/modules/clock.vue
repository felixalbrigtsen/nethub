<template>
    <div class="clock">
        <h1>{{timeString}}</h1>
        <h2>{{dateString}}</h2>
    </div>
</template>

<script>
export default {
    name: "ModuleClock",
    data() {
        return {
            timeString: "",
            dateString: "",
            intervalTimer: null
        }
    },
    methods: {
        zeroPad(num) {
            return ("00"+num.toString()).slice(-2)
        },
        updateTime() {
            let cd = new Date()
            this.timeString = this.zeroPad(cd.getHours())+":"+this.zeroPad(cd.getMinutes())+":"+this.zeroPad(cd.getSeconds())
            this.dateString = ["Mandag","Tirsdag","Onsdag","Torsdag","Fredag","Lørdag","Søndag"][cd.getDay()]+" "+this.zeroPad(cd.getDate())+"/"+this.zeroPad(cd.getMonth()+1)
        }
        
    },
    mounted: function() {
        this.updateTime()
        this.intervalTimer = setInterval(this.updateTime, 1000)
    }
}

</script>

<style>
.clock {
    width: 100%;
    height: 100%;
    text-align: center;
    display: flex;
    flex-direction: column;
    justify-content: center;
    font-family: "Courier New", Courier, monospace;
}
.clock > h1 {
    font-size: 38px;
}
</style>