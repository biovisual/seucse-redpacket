<template>
  <div class="container">
    <div class="table">
      <div class="cell" v-for="(item, index) in list" :key="index">
        <img :src="item.a"/>
        <a class="nickname"><span>{{item.n}}</span></a>
        <a class="count" v-if="index == 0" style="color:#DC143C;">{{item.s}}</a>
        <a class="count" v-else-if="index == 1" style="color:#FF8C00;">{{item.s}}</a>
        <a class="count" v-else-if="index == 2" style="color:#FFD700;">{{item.s}}</a>
        <a class="count" v-else>{{item.s}}</a>
      </div>

      <img src="../assets/imgs/sponsor.png" class="sponsor"/>
    </div>

    <div class="packet-amount">
      <div v-for="(item, index) in amount" :key="index">
        <img src="../assets/imgs/redpacket.png"/>
        <a v-if="index == 0" style="color:#DC143C;">{{item}}</a>
        <a v-else-if="index == 1" style="color:#FF8C00;">{{item}}</a>
        <a v-else-if="index == 2" style="color:#FFD700;">{{item}}</a>
        <a v-else>{{item}}</a>
      </div>
    </div>

  </div>
</template>

<script>

export default {
  name: 'SecondScreen',
  data () {
    return {
      socket: null,
      list: [],
      amount: [66.66, 20.18, 20.18, 17.18, 17.18, 17.18, 17.18, 17.18, 9.71, 9.71],
      isRunning: false
    }
  },
  created () {
    this.socket = new WebSocket("wss://myseu.cn/redpack/second")
    this.socket.onmessage = this.cb
    this.checkSocket()
  }, 
  methods: {
    checkSocket () {
      let that = this
      setInterval(() => {
        if (that.socket.readyState == WebSocket.CLOSED 
          || that.socket.readyState == WebSocket.CLOSING) {
          that.socket = new WebSocket("wss://myseu.cn/redpack/second")
          that.socket.onmessage = that.cb
        }
      }, 200)
    },
    cb (event) {
      var temp = JSON.parse(event.data)
      this.isRunning = temp.r

      if (temp.r) {
        this.list = temp.l
      }
    }
  }
}
</script>

<style scoped>
a{
  color: rgba(0, 0, 0, 0.6);
  font-weight: bold;
}

.container{
  width: 800px;
  height: 1200px;
  background-image: url("../assets/imgs/second-bg.png");
  background-repeat: no-repeat;
  padding-top: 390px;
  padding-left: 150px;
  display: flex;
  flex-direction: row;
}

.table{
  width: 420px;
  height: 710px;
}

.cell{
  width: 420px;
  height: 60px;
  border-radius: 30px;
  background-color: #EAEAEA;
  display: flex;
  flex-direction: row;
  align-items: center;
  margin: 12px 0;
}

.cell img{
  width: 60px;
  height: 60px;
  border-radius: 30px;
}

.nickname{
  width: 250px;
  height: 60px;
  line-height: 60px;
  margin-left: 10px;
  font-size: 30px;
  overflow: hidden;
}

.count{
  font-size: 40px;
}

.packet-amount{
  height: 710px;
  width: 200px;
  margin-left: 20px;
}

.packet-amount div{
  height: 60px;
  width: 200px;
  display: flex;
  flex-direction: row;
  align-items: center;
  margin: 12px 0;
}

.packet-amount div img{
  height: 35px;
  width: 35px;
  margin-right: 5px;
}

.packet-amount div a{
  font-size: 30px;
}

.sponsor{
  width: 520px;
  height: 48px;
  position: absolute;
  top: 1130px;
  left: 160px;
}
</style>