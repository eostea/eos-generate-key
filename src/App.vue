<template>
  <div id="app">
    <img src="https://eosfans-static.strahe.com/photo/2018/bc17c8bf-d168-4fdc-b99a-83bea533d752.png?x-oss-process=image/resize,w_1920" style="height:200px;">

    <div v-show="show">
      <p><span>私钥：</span>{{ privateKey }}</p>
      <p><span>公钥：</span>{{ publicKey }}</p>
    </div>

    <div style="margin-top: 40px;">
      <button class="primary" @click="getPrivateKey">一键生成私钥/公钥</button>
      <p>通过一键生成私钥和公钥，<span style="color:orange;">同时请注意保管好自己的私钥</span></p>
    </div>

    <div style="margin-top: 40px;">
      <input type="text" v-model="seed">
      <button class="primary" @click="getPrivateKeyBySeed">通过助记词，获取私钥</button>
      <p>助记词: 应该使用至少128个随机位来产生一个好的私钥，<span style="color:orange;">同时请注意保管好助记词</span></p>
    </div>

    <div style="margin-top: 20px;">
      <div>
        <input type="text" v-model="valiPublicKey">
        <button class="primary" @click="isValidPublic">验证公钥格式</button>
      </div>
      <p v-if="showPublicTips"><span>验证EOS公钥：</span>{{ valiPublicStatus  ? '正确' : '错误！'}}</p>
      <p>（验证EOS公钥格式）</p>
    </div>

    <div style="margin-top: 20px;">
      <div>
        <input type="text" v-model="valiPrivateKey">
        <button class="primary" @click="isValidPrivate">验证私钥格式</button>
      </div>
      <p v-if="showPrivateTips"><span>验证EOS私钥：</span>{{ valiPrivateStatus ? '正确' : '错误！'}}</p>
      <p>（验证EOS私钥格式）</p>
    </div>
  </div>
</template>

<script>
import ecc from 'eosjs-ecc'

export default {
  name: 'App',
  data () {
    return {
      show: false,
      publicKey: '',
      privateKey: '',
      seed: '',
      valiPublicKey: '',
      valiPrivateKey: '',
      valiPublicStatus: false,
      valiPrivateStatus: false,
      showPublicTips: false,
      showPrivateTips: false
    }
  },
  methods: {
    getPrivateKey () {
      this.show = true
      ecc.randomKey().then(privateKey => {
        this.privateKey = privateKey.toString()
        this.publicKey = ecc.privateToPublic(this.privateKey)
      })
    },
    getPrivateKeyBySeed () {
      this.show = true
      this.privateKey = ecc.seedPrivate(this.seed)
      this.publicKey = ecc.privateToPublic(this.privateKey)
    },
    isValidPublic () {
      this.showPublicTips = true
      this.valiPublicStatus = ecc.isValidPublic(this.valiPublicKey)
    },
    isValidPrivate () {
      this.showPrivateTips = true
      this.valiPrivateStatus = ecc.isValidPrivate(this.valiPrivateKey)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

input[type='text'] {
  -webkit-appearance: none;
  background-color: #fff;
  background-image: none;
  border-radius: 4px;
  border: 1px solid #dcdfe6;
  box-sizing: border-box;
  color: #606266;
  display: inline-block;
  font-size: inherit;
  height: 40px;
  line-height: 40px;
  outline: none;
  padding: 0 15px;
  transition: border-color .2s cubic-bezier(.645,.045,.355,1);
  width: 40%;
}
input[type='text']:focus {
    outline: none;
    border-color: #409eff;
}
.primary {
    color: #fff;
    background-color: #409eff;
    border-color: #409eff;
}
.primary:active {
  background: #3a8ee6;
  border-color: #3a8ee6;
  color: #fff;
}
.primary:hover {
    background: #66b1ff;
    border-color: #66b1ff;
    color: #fff;
}
button {
  display: inline-block;
  line-height: 1;
  white-space: nowrap;
  cursor: pointer;
  background: #fff;
  border: 1px solid #dcdfe6;
  border-color: #dcdfe6;
  color: #606266;
  -webkit-appearance: none;
  text-align: center;
  box-sizing: border-box;
  outline: none;
  margin: 0;
  transition: .1s;
  font-weight: 500;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  padding: 12px 20px;
  font-size: 14px;
  border-radius: 4px;
}

</style>
