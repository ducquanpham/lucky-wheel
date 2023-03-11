<template>
  <div class="container">
    <div class="container-login" v-if="showLogin">
      <div class="a">
        <div class="login-banner">
          <img src="../assets/img/tpb-desktop.png" alt="">
        </div>
        <div class="logo-login">
          <img src="../assets/img/logo-banner.png" alt="">
        </div>
      </div>
      <marquee class="text-title" style="position: fixed; top: 15%;">
        Chào Mừng Bạn Đến Với Chương Trình Vòng Quay May Mắn Của Ngân Hàng TPBank, Vui Lòng Đăng Nhập Để Tham Gia Chương
        Trình
      </marquee>
      <div class="login-item">
        <form action="" class="form form-login">
          <div class="form-field">
            <label for="login-username"><img class="icon-phone" src="../assets/img/phone.svg" alt=""></label>
            <input id="login-number" type="number" v-model="sdt" @input="onInput" class="form-input"
              placeholder="Số Điện Thoại" required>
          </div>

          <div class="form-field">
            <label for="login-password"><img class="icon-card" src="../assets/img/card.svg" alt=""></label>
            <input id="login-code" v-model="password" type="text" class="form-input" placeholder="Mã Xác Nhận" required>
          </div>

          <div class="form-field">
            <button type="submit" class="login-btn" @click="closeLogin">Đăng Nhập</button>
          </div>
        </form>
      </div>
    </div>
    <div class="lucky-wheel">
      <div class="lucky-title">
        <img src="../assets/img/logo-banner.png" class="logo-title" style="display: block; margin: 0 auto;" alt="">
        <img src="../assets/img/logo-tpbank.png" class="flag_tpbank" alt="">
        <img src="../assets/img/co_vietnam.png" class="flag_vn" alt="">
        <marquee behavior="scroll" direction="left" class="text-scroll">Chúc Mừng Bạn Tham Gia Chương Trình Vòng Quay May
          Mắn Của TP Bank, Mọi thắc mắc liên hệ CSKH:012345678 </marquee>
      </div>
      <div class="textUser">Tên Người Dùng:{{ sdt }}</div>
      <div class="wheel-main">
        <div class="wheel-pointer" @click="beginRotate()">
          <img src="../assets/img/logo.png" class="logo-tp" alt="">
        </div>
        <div class="wheel-bg" :style="rotateStyle">
          <div class="prize-list">
            <div class="prize-item" v-for="(item, index) in prizeList" :key="index" :style="item.style">
              <div class="prize-pic">
                <img :src="item.icon" />
              </div>
              <div class="prize-type">{{ item.name }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="main">
      <div class="main-bg"></div>
      <div class="bg-p"></div>
      <div class="content">
        <div class="count">Số Luợt Còn Lại là: {{ count }}</div>
      </div>
      <ul class="media-list">
        <li v-for="(item, idx) in dataNewMediaIndex" :key="idx" class="media-item"> {{ item.item }}
        <img :src="item.image" alt="" width="20" height="20" class="media-item-check" style="-webkit-user-select: none;margin: auto;cursor: zoom-in;transition: background-color 300ms;">
        </li>
      </ul>
    </div>
    <div class="toast" v-show="prize">
      <div class="toast-container">
        <img :src="toastIcon" class="toast-picture" />
        <div class="close" @click="closeToast()"></div>
        <div class="toast-title">{{ toastTitle }}</div>
        <div class="toast-desc">Xin vui lòng liên hệ với nhân viên shop qua sđt:<span
            style="color:red; text-decoration: underline;">012345678</span> để nhận quà nhé❤️❤️❤️</div>
        <div class="toast-btn">
          <div class="toast-cancel" @click="openModal">Xác Nhận</div>
        </div>
      </div>
    </div>
    <div class="modal" v-if="show">
      <div class="modal-content">
        <div>Vui Lòng Bạn Nhập Thông Tin Dưới Đây</div>
        <form action="" class="form-profile">
          <input type="text" placeholder="Họ và Tên" class="input input-name">
          <input type="text" placeholder="Địa Chỉ" class="input input-name">
          <input type="number" placeholder="Số Điện Thoại" class="input input-name">
          <select id="phone" class="select-phone">
            <option value="red">IHPONE 14 512GB MÀU ĐỎ</option>
            <option value="black">IHPONE 14 512GB MÀU ĐEN</option>
            <option value="white">IHPONE 14 512GB MÀU TRẮNG</option>
            <option value="purple">IHPONE 14 512GB MÀU TÍM</option>
          </select>
          <p class="form-profile-desc">Sau Khi Điền Đầy Đủ Thông Tin,Vui Lòng Chụp Ảnh Màn Hình Lại Và Gửi Cho Nhân
            Viên CSKH Thông Qua Số <span style="font-size: 16px;text-decoration: underline;color:black"> Zalo:
              012345678</span> Để Được Nhận Quà ❤️❤️❤️</p>
        </form>
        <a href="https://www.facebook.com/CuanKun1102" class="link-fb"><button @click="closeModal"
            class="form-profile-btn">Đóng</button></a>
      </div>
    </div>
    <div class="footer">
      <img src="../assets/img/logo-banner.png" alt="">
      <div>
        <a href="tel:+012345678" class="footer-photography"> CSKH Liên Hệ SĐT: 012345678</a>
      </div>
    </div>
    <div class="toast-mask" v-show="prize"></div>
  </div>
</template>
<script>
import { prizeList } from './config'
import { dataNewMediaIndex } from './config'

const CIRCLE_ANGLE = 360

const config = {
  duration: 4000,
  circle: 5,
  mode: 'ease-in-out'
}

export default {
  name: 'Modal',
  data() {
    return {
      count: 3,
      duration: 3000,
      prizeList: [],
      dataNewMediaIndex: [],
      rotateAngle: 0,
      index: 0,
      prize: null,
      show: false,
      showLogin: true,
      password: '',
      sdt: '',
    };
  },
  created() {
    this.angleList = []
    this.isRotating = false
    this.config = config

    this.initPrizeList();
    this.initDataNewMediaIndex();
  },
  computed: {
    rotateStyle() {
      return `
        -webkit-transition: transform ${this.config.duration}ms ${this.config.mode};
        transition: transform ${this.config.duration}ms ${this.config.mode};
        -webkit-transform: rotate(${this.rotateAngle}deg);
            transform: rotate(${this.rotateAngle}deg);`
    },
    toastTitle() {
      return this.prize && this.prize.isPrize === 1
        ? "Chúc Mừng Bạn Đã Trúng: " +
        this.prize.name
        : "Xin Lỗi Bạn";
    },
    toastIcon() {
      return this.prize && this.prize.isPrize === 1
        ? require("../assets/img/congratulation.png")
        : require("../assets/img/sorry.png");
    },

  },
  methods: {
    closeLogin() {
      if (this.password === '123456' && this.sdt.length == 10) {
        this.showLogin = false
      } if (this.password !== '123456') {
        alert("vui lòng nhập đúng mã xác nhận")
      };
      if (this.sdt.length !== 10) {
        alert("vui lòng nhập đủ 10 số")
      } return
    },
    onInput(e) {
      this.text = e.target.value;
    },
    initPrizeList() {
      this.prizeList = this.formatPrizeList(prizeList)
    },
    initDataNewMediaIndex() {
      this.dataNewMediaIndex = this.formatPrizeList(dataNewMediaIndex)
    },
    openModal() {
      this.show = true
    },
    closeModal() {
      this.show = false;
      this.prize = null;
    },
    formatPrizeList(list) {
      const angleList = []

      const l = list.length
      const average = CIRCLE_ANGLE / l

      const half = average / 2

      list.forEach((item, i) => {

        const angle = -((i * average) + half)
        item.style = `-webkit-transform: rotate(${angle}deg);
                      transform: rotate(${angle}deg);`

        angleList.push((i * average) + half)
      })

      this.angleList = angleList

      return list
    },
    beginRotate() {

      if (this.count === 0) return

      this.index = this.random(this.prizeList.length - 1);

      this.count--

      this.rotating()
    },
    random(max, min = 0) {
      return parseInt(Math.random() * (max - min + 1) + min)
    },
    rotating() {
      const { isRotating, angleList, config, rotateAngle, index } = this

      if (isRotating) return

      this.isRotating = true

      const angle =
        rotateAngle +
        config.circle * CIRCLE_ANGLE +
        angleList[index] -
        (rotateAngle % CIRCLE_ANGLE)


      this.rotateAngle = angle

      setTimeout(() => {

        this.rotateOver()
      }, config.duration + 1000)
    },
    rotateOver() {
      this.isRotating = false

      this.prize = prizeList[this.index]

      console.log(this.prize, this.index)
    },
    closeToast() {
      this.prize = null;
    },

  }
};
</script>
<style>
.container {
  width: 100%;
  overflow: hidden;
  position: relative;
}

.lucky-wheel {
  width: 100%;
  background: rgb(252, 207, 133) url("../assets/img/color_pillar.png") no-repeat center bottom;
  background-size: 100%;
}

.text-scroll {
  position: absolute;
  color: #fff;
  font-size: 20px;
  margin-top: 2px;
}

.lucky-title {
  width: 50%;
  display: block;
  margin: 0 auto;
  background-size: 100%;
  background-color: #5e2e86;
  width: 100%;
  margin-bottom: 4rem;
  padding: 2rem 0;
}

.logo-title {
  width: 200px;
}

.wheel-main {
  margin: 0 auto;
  position: relative;
  width: 500px;
  height: 500px;
}

.wheel-bg {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 1;
  width: 100%;
  height: 100%;
  background: url("../assets/img/draw_wheel.png") no-repeat center top;
  background-size: 100%;
  color: #fff;
}

.flag_vn {
  border-radius: 5px;
  position: absolute;
  top: 15px;
  right: 15px;
  width: 60px;
}

.flag_tpbank {
  position: absolute;
  top: 15px;
  left: 10px;
  width: 55px;
  height: 55px;
}

.wheel-pointer {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 100px;
  height: 100px;
  z-index: 2;
  background: url("../assets/img/draw_btn.png") no-repeat center top;
  background-size: 100%;
  transform: translate3d(-50%, -50%, 0);
}

.logo-tp {
  position: absolute;
  width: 60%;
  top: 59%;
  border-radius: 100%;
  left: 51%;
  -webkit-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
}

.wheel-bg div {
  text-align: center;
}

.prize-list {
  width: 100%;
  height: 100%;
  position: relative;
}

.prize-list .prize-item {
  position: absolute;

  width: 95px;
  height: 190px;
  top: 11%;
  left: 50%;
  margin-left: -47.5px;
  transform-origin: 50% 100%;
}

.prize-pic img {
  /* width: 4.0625rem; */
  height: 3.5rem;
  margin-top: 0;
}

.prize-count {
  font-size: 0.875rem;
}

.prize-type {
  font-size: 0.6rem;
}

.main {
  position: relative;
  width: 100%;
  min-height: 50rem;
  background: rgb(243, 109, 86);
  padding-bottom: 1.6875rem;
}

@media screen and (min-width: 280px) and (max-width:1023px) {
  .main {
    min-height: 22rem;
  }
}

.main-bg {
  width: 35%;
  height: 6.5625rem;
  position: absolute;
  top: -3.4375rem;
  background: url("../assets/img/luck_bg.png") no-repeat center top;
  background-size: 100%;
  left: 50%;
  transform: translateX(-50%);
}

.bg-p {
  width: 100%;
  height: 2.95rem;
  background: rgb(252, 207, 133);
}

.content {
  position: absolute;
  top: 1.175rem;
  left: 50%;
  transform: translateX(-50%);
  background: rgb(243, 109, 86);
  width: 100%;
  font-size: 1.125rem;
  color: #ffeb39;
}

.content div {
  text-align: center;
}

.tip {
  position: relative;
  margin: 2.5rem auto 0;
  width: 17.5rem;
  border: 1px solid #fbc27f;
}

.tip-title {
  position: absolute;
  top: -1rem;
  left: 50%;
  transform: translate(-50%, 0);
  font-size: 1rem;
  color: #fccc6e;
  background: rgb(243, 109, 86);
  padding: 0.3125rem 0.625rem;
}

.tip-content {
  padding: 1.5625rem 0.625rem;
  font-size: 0.875rem;
  color: #fff8c5;
  line-height: 1.5;
}

.toast-mask {
  position: fixed;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.6);
  z-index: 10;
  width: 100%;
  height: 100%;
}

.toast {
  position: fixed;
  top: 50%;
  left: 50%;
  z-index: 200;
  transform: translate(-50%, -50%);
  width: 32rem;
  background: #fff;
  border-radius: 0.3125rem;
  padding: 0.3125rem;
  background-color: rgb(252, 244, 224);
  height: 400px;
}

.toast-container {
  position: relative;
  width: 100%;
  height: 100%;
  border: 1px dotted #fccc6e;
}

.toast-picture {
  width: 18.75rem;
  height: 8.5625rem;
  position: absolute;
  top: -22%;
  left: 50%;
  transform: translateX(-50%);
}

.toast-pictrue-change {
  position: absolute;
  top: -1.5rem;
  left: -1.375rem;
  width: 17.5rem;
  height: 3.125rem;
}

.toast-title {
  padding: 2.8125rem 0 2rem 0;
  font-size: 18px;
  color: #fc7939;
  text-align: center;
}

.toast-desc {
  padding: 0 6px;
  font-size: 0.8rem;
  text-align: center;
  padding-bottom: 1rem;
}

.toast-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 0.9375rem;
  margin-top: 50px;
}

.toast-btn div {
  background-image: -moz-linear-gradient(-18deg,
      rgb(242, 148, 85) 0%,
      rgb(252, 124, 88) 51%,
      rgb(252, 124, 88) 99%);

  background-image: -ms-linear-gradient(-18deg,
      rgb(242, 148, 85) 0%,
      rgb(252, 124, 88) 51%,
      rgb(252, 124, 88) 99%);
  background-image: -webkit-linear-gradient(-18deg,
      rgb(242, 148, 85) 0%,
      rgb(252, 124, 88) 51%,
      rgb(252, 124, 88) 99%);
  box-shadow: 0px 4px 0px 0px rgba(174, 34, 5, 0.7);
  padding: 1px 10px;
  border-radius: 1.875rem;
  text-align: center;
  line-height: 1.875rem;
  color: #fff;
}

.a {
  position: fixed;
  top: 10px;
  left: 0;
  right: 0;
}

.form-profile-btn {
  box-shadow: 0px 4px 0px 0px rgba(174, 34, 5, 0.7);
  padding: 1px 30px;
  border-radius: 1.875rem;
  text-align: center;
  line-height: 1.875rem;
  color: #fff;
  background-image: linear-gradient(-18deg,
      rgb(242, 148, 85) 0%,
      rgb(252, 124, 88) 51%,
      rgb(252, 124, 88) 99%);
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 15px auto 0;
}

.close {
  position: absolute;
  top: -0.9375rem;
  right: -0.9375rem;
  width: 2rem;
  height: 2rem;
  background: url("../assets/img/close_store.png") no-repeat center top;
  background-size: 100%;
}

.loading-animation__wrapper {
  position: absolute;
  top: 100px;
  left: 0;
  display: flex;
  width: 100vw;
  align-items: center;
  justify-content: center;
  font-family: 'youi_helv', 'Helvetica Neue', Helvetica, Arial, sans-serif;
  color: #fff;
  font-size: 1rem;
}

.loading-animation__wrapper * {
  box-sizing: border-box;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  font-family: Helvetica Neue;
  font-weight: bold;
  letter-spacing: -0.03em;
  color: #fff;
}

h1.light,
h2.light,
h3.light,
h4.light,
h5.light,
h6.light {
  font-weight: 300;
}

h1 {
  font-size: 44px;
  line-height: 44px;
}

h4 {
  font-size: 24px;
  line-height: 28px;
}

.icon-phone,
.icon-card {
  width: 15px;
}


.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem 0.5rem;
  background-color: #5e2e86;
}

.footer-photography {
  font-size: 11px;
  color: #fff;
  text-decoration: underline;
}

.modal {
  position: absolute;
  top: 0;
  z-index: 9999 !important;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(8 8 8 / 60%);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px 15px;
  border-radius: 5px;
  width: 450px;
  z-index: 9999999999999999999999999999999;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.form-profile {
  margin-top: 20px;
}

.form-profile .input {
  width: 100%;
  height: 36px;
  margin-bottom: 20px;
  border-radius: 5px;
  padding-left: 12px;
  border: 1px solid #ccc;
}

.select-phone {
  width: 80%;
  padding: 5px 0 5px 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: white;
  color: #000;
  height: 40px;
}

.select-phone:focus {
  background-color: #333;
  color: white;
}

.form-profile-desc {
  margin-top: 25px;
  color: red;
  font-size: 13.5px;
  line-height: 20px;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.container-login {
  height: 100%;
  font-family: 'lato', sans-serif;
  color: #fff;
  background: #5e2e86;
  position: absolute;
  width: 100%;
  left: 0;
  display: none;
  z-index: 99;
}

.login-banner {
  right: 0;
  width: 30%;
  margin-left: auto;
}

@media screen and (min-width:200px) and (max-width:911px) {
  .login-banner {
    position: absolute;
    top: 0;
    right: -10%;
    width: 70%;
  }
}

.login-banner img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.logo-login {
  position: absolute;
  top: 10px;
  left: 10px;
}

.logo {
  font-family: "museo-slab";
  font-size: 20px;
  text-align: center;
  padding: 20px 20px 0;
  margin: 0;
}

.login-item {
  color: #ffff;
  padding: 25px 25px 0;
  margin: 20px auto 0;
  border-radius: 3px;
  background: rgba(58, 63, 68, 0.5);
  border-radius: 5px;
  box-shadow: 0 1.5px 0 0 rgba(0, 0, 0, 0.1);
  width: 350px;
  display: flex;
  flex-direction: column;
  top: 30%;
  left: 50%;
  transform: translateX(-50%);
  position: fixed;
  background-image: url(../assets/img/meomeo.png);
  background-repeat: repeat;
  background-position: top center;
  background-size: cover;
  z-index: 99;
}

.login-item ::placeholder {
  color: #fff;
  opacity: 1;
  /* Firefox */
}

.login-item :-ms-input-placeholder {
  /* Internet Explorer 10-11 */
  color: #fff;
}

.login-item ::-ms-input-placeholder {
  /* Microsoft Edge */
  color: #fff;
}

.container-login input {
  border: 0;
  color: inherit;
  font: inherit;
  margin: 0;
  outline: 0;
  padding: 0;
  -webkit-transition: background-color .3s;
  transition: background-color .3s;
}

.form input[type="password"],
.form input[type="text"],
.form input[type="submit"] {
  width: 100%;
}

.form-login label,
.form-login input[type="text"],
.form-login input[type="number"],
.form-login input[type="submit"] {
  border-radius: 0.25rem;
  padding: 1rem;
  color: #fff;
}

#login-number,
#login-code {
  width: 100%;
  border-radius: 0 5px 5px 0;
  background: rgb(0 0 0 / 60%);
}

#login-code {
  padding-left: 15px;
}

.form-login label {
  background-color: rgb(0 0 0 / 75%);
  border-bottom-right-radius: 0;
  border-top-right-radius: 0;
  padding-left: 1.25rem;
  padding-right: 1.25rem;
}

.form-login input[type="text"],
.form-login input[type="password"] {
  background-color: #ffffff;
  border-bottom-left-radius: 0;
  border-top-left-radius: 0;
}

.form-login input[type="text"]:focus,
.form-login input[type="text"]:hover,
.form-login input[type="number"]:focus,
.form-login input[type="number"]:hover {
  background-color: #eeeeee;
}

.form-login .login-btn {
  background-color: #222222e6;
  color: #eee;
  font-weight: bold;
  text-transform: uppercase;
  width: 100%;
  height: 50px;
  border-radius: 5px !important;
}

.form-login .login-btn:focus,
.form-login .login-btn:hover {
  background-color: #222;
}

.form-field {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  margin-bottom: 2rem;
}

.textUser {
  color: white;
  display: block;
  position: absolute;
  right: 5px;
  background: linear-gradient(to bottom, rgba(255, 255, 255, 0.15) 0%, rgba(0, 0, 0, 0.15) 100%), radial-gradient(at top center, rgba(255, 255, 255, 0.40) 0%, rgba(0, 0, 0, 0.40) 120%) #989898;
  background-blend-mode: multiply, multiply;
  border-radius: 5px;
  padding: 4px 8px;
  top: 120px;
}

.wheel-main {
  margin: 0 auto;
  position: relative;
  width: 500px;
  height: 500px;
}

/* media list */
.media-list {
  padding: 0 10px;
  height: 200px;
  overflow-y: scroll;
  margin-top: 30px;
}

.media-item {
  color: #fff;
  font-size: 14px;
  position: relative;
  margin-top: 15px;
  margin-left: 25px;
  animation: animate 15s linear infinite;
}

.media-item-check {
  position: absolute;
  content: '';
  top: 50%;
  left: -28px;
  transform: translateY(-50%);
}
@keyframes animate {
	0% { transform: translateY(0); }
	100% { transform: translateY(calc(-500px))}
}
.media-list::-webkit-scrollbar{
  display: none;
}
@media screen and (max-width:911px) {
  .modal-content {
    width: 330px;
  }

  .toast-btn {
    margin-top: 0;
  }

  .toast-picture {
    top: -6.5rem;
    transform: 0;
  }

  .toast {
    height: unset;
    width: 15.4375rem;
  }

  .main-bg {
    left: 0;
    width: 100%;
    transform: none;
  }

  .prize-pic img {
    height: 2.5rem;
    margin-top: 1.5625rem;
  }

  .prize-list .prize-item {
    height: 150px;
    top: 0;
  }

  .wheel-pointer {
    width: 85px;
    height: 85px;
  }

  .flag_vn {
    width: 40px;
  }

  .flag_tpbank {
    width: 35px;
    height: 35px;
  }

  .wheel-main {
    width: 300px;
    height: 300px;
  }

  .lucky-title {
    padding: 1rem 0;
  }

  .logo-title {
    width: 150px;
  }



  .text-scroll {
    font-size: 14px;
  }

  .textUser {
    top: 70px;
  }

}</style>

