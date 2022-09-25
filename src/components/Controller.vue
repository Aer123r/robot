<template>
  <div id="controllerCenter">
    <input v-model="publish.payload">
    <img id="status" src="wifi.slash@2x.png" alt="状态" ref="status">
    <div id="reset">复位</div>
    <div @click="createConnection" id="conn">
      连接
    </div>
    <div @click="doPublish" id="pub">
      发布
    </div>

  </div>

</template>
<script>



import mqtt from "mqtt";

export default {
  // eslint-disable-next-line no-undef,vue/multi-word-component-names
  name:"Controller",
  data() {
    return {
      connection: {
        host: '8.130.24.253',
        port: 8083,
        endpoint: '/mqtt',
        clean: true, // 保留会话
        connectTimeout: 40000, // 超时时间
        reconnectPeriod: 40000, // 重连时间间隔
        // 认证信息
        clientId: 'Robot-studio',
        username: 'admin',
        password: '112233wU',
      },
      subscription: {
        topic: 'esp8266',
        qos: 0,
      },
      publish: {
        topic: 'esp8266',
        qos: 0,
        payload: '发送内容',
      },
      receiveNews: '',
      qosList: [
        { label: 0, value: 0 },
        { label: 1, value: 1 },
        { label: 2, value: 2 },
      ],
      client: {
        connected: false,
      },
      subscribeSuccess: false,
    }
  },
  props: {
    msg: String
  },
  mounted() {
    // 创建连接


  },
  methods: {
    createConnection() {
      // 连接字符串, 通过协议指定使用的连接方式
      // ws 未加密 WebSocket 连接
      // wss 加密 WebSocket 连接
      // mqtt 未加密 TCP 连接
      // mqtts 加密 TCP 连接
      // wxs 微信小程序连接
      // alis 支付宝小程序连接
      const {host, port, endpoint, ...options} = this.connection  //
      const connectUrl = `ws://${host}:${port}${endpoint}`
      try {
        this.client = mqtt.connect(connectUrl, options)
      } catch (error) {
        console.log('mqtt.connect error', error)
      }
      this.client.on('connect', () => {
        console.log('Connection succeeded!')
        alert('连接成功')
        this.$refs.status.src="wifi.square.fill@2x.png"
      })
      this.client.on('error', error => {
        console.log('Connection failed', error)
      })
      this.client.on('message', (topic, message) => {
        this.receiveNews = this.receiveNews.concat(message)
        console.log(`Received message ${message} from topic ${topic}`)
      })

    },
    // 订阅主题
    doSubscribe() {
      const { topic, qos } = this.subscription
      this.client.subscribe(topic, { qos }, (error, res) => {
        if (error) {
          console.log('Subscribe to topics error', error)
          return
        }
        this.subscribeSuccess = true
        console.log('Subscribe to topics res', res)
      })
    },
    // 取消订阅
    doUnSubscribe() {
      const { topic } = this.subscription
      this.client.unsubscribe(topic, error => {
        if (error) {
          console.log('Unsubscribe error', error)
        }
      })
    },
    // 发送消息
    doPublish() {
      const { topic, qos, payload } = this.publish
      let data = `msg:${payload}&angle:${this.$store.angle}`
      this.client.publish(topic, data, qos, error => {
        if (error) {
          console.log('Publish error', error)
        }
      })
    },
    // 断开连接
    destroyConnection() {
      if (this.client.connected) {
        try {
          this.client.end()
          this.client = {
            connected: false,
          }
          console.log('Successfully disconnected!')
        } catch (error) {
          console.log('Disconnect failed', error.toString())
        }
      }
    },
  },

}


</script>

<style scoped>
  #controllerCenter {
    position: absolute;
    right: 0px;
    width: 200px;
    height: 200px;
    border: 3px solid pink;
    border-radius: 20px;
    color: #A79E7D;
  }
  #controllerCenter *{
    position: absolute;
    top: 20px;
    right: 30px;
    padding: 4px;
    border: 5px solid orange;
    border-radius: 15px;
  }
  #controllerCenter *:hover{
    background-color: rgba(0,0,0,.3);
    color: white;
    cursor: grab;
  }
  #controllerCenter #status {
    right: 126px;
    width: 28px;
    border: 1px solid transparent;
  }
  #controllerCenter #reset {
    right: 120px;
    top: 70px;
  }
  #pub{
    top: 70px;
    cursor: grab;
  }
  #controllerCenter input {
    top: 120px;
    background-color: bisque;
  }
  #controllerCenter input:focus {
    color: #A79E7D;
    background-color: bisque;
  }
</style>