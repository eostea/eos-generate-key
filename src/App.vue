<template>
  <div id="app">
    <img src="https://eosfans-static.strahe.com/photo/2018/bc17c8bf-d168-4fdc-b99a-83bea533d752.png?x-oss-process=image/resize,w_1920" style="height:200px;">

    <div v-show="show">
      <h3>随机生成私钥</h3>
      <p><span>私钥：</span>{{ privateKey }}</p>
      <p><span>公钥：</span>{{ publicKey }}</p>
    </div>
    <div style="margin-top: 40px;">
      <button class="primary" @click="getPrivateKey">一键生成私钥/公钥（随机生成）</button>
      <p>通过一键生成私钥和公钥，<span style="color:orange;">同时请注意保管好自己的私钥</span></p>
    </div>
    <hr>
    <div v-show="showSeed">
      <h3>通过助记词生成私钥</h3>
      <p><span>私钥：</span>{{ privateKeyBySeed }}</p>
      <p><span>公钥：</span>{{ publicKeyBySeed }}</p>
    </div>
    <div style="margin-top: 40px;">
      <input type="text" v-model="seed">
      <button class="primary" @click="showDialog = true">通过助记词，获取私钥（生成固定私钥）</button>
      <p>可以输入一个固定的字符串，生成一个固定的私钥/公钥。</p>
      <p>助记词: 应该是使用至少128个随机位来产生一个好的私钥，<span style="color:orange;">同时请注意保管好助记词</span></p>
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

    <div class="dialog" v-if="showDialog">
      <div class="dialog-container">
        <div class="dialog-header">
          <h2>提示：</h2>
        </div>
        <div class="dialog-body">
          <p>确定使用当前的助记词生成固定的私钥吗？</p>
          <p>助记词：{{ this.seed }}</p>
        </div>
        <div class="dialog-footer">
          <button @click="showDialog = false">取消</button>
          <button class="primary" @click="generateKeyBySeed">确定</button>
        </div>
      </div>
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
      showSeed: false,
      publicKey: '',
      privateKey: '',
      publicKeyBySeed: '',
      privateKeyBySeed: '',
      seed: '',
      valiPublicKey: '',
      valiPrivateKey: '',
      valiPublicStatus: false,
      valiPrivateStatus: false,
      showPublicTips: false,
      showPrivateTips: false,

      // dialog
      showDialog: false
    }
  },
  methods: {
    generateKeyBySeed () {
      this.getPrivateKeyBySeed()
      this.showDialog = false
    },
    getPrivateKey () {
      ecc.randomKey().then(privateKey => {
        this.privateKey = privateKey.toString()
        this.publicKey = ecc.privateToPublic(this.privateKey)
        this.show = true
      })
    },
    getPrivateKeyBySeed () {
      this.privateKeyBySeed = ecc.seedPrivate(this.seed)
      this.publicKeyBySeed = ecc.privateToPublic(this.privateKeyBySeed)
      this.showSeed = true
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
  margin-top: 20px;
}
body {
  position: relative;
  width: 100%;
  height: 100%;
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
.dialog {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: left;
}
.dialog-container {
  background: #fff;
  padding: 30px 40px;
  border-radius: 2px;
}
.dialog-footer {
  text-align: right;
}

</style>
