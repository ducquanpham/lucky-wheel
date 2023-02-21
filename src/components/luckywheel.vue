<template>
  <div class="container">
    <div class="container-login" v-if="showLogin">
      <div class="login-item">
        <form action="" class="form form-login">
          <div class="form-field">
            <label for="login-username"><img class="icon-phone" src="../assets/img/phone.svg" alt=""></label>
            <input id="login-number" type="number" v-model="sdt" @input="onInput" class="form-input" placeholder="Số Điện Thoại" required>
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
        <img src="../assets/img/logo-banner.png" class="" style="display: block; margin: 0 auto;" alt="">
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
      <div class="loading-animation__wrapper">
        <div class="loading-animation__content">
          <div class="loading-animation__list-wrapper">
            <div class="loading-animation__tick"></div>
            <ul class="loading-animation__list">
              <li>Chúc Mừng Nguyễn Văn A Trúng thưởng 1 phần quà to lớn Vàng</li>
              <li>Chúc Mừng Nguyễn Văn Q Trúng thưởng 1 phần quà to lớn Xanh</li>
              <li>Chúc Mừng Nguyễn Văn E Trúng thưởng 1 phần quà to lớn Đỏ</li>
              <li>Chúc Mừng Nguyễn Văn R Trúng thưởng 1 phần quà to lớn Tím</li>
              <li>Chúc Mừng Nguyễn Văn Y Trúng thưởng 1 phần quà to lớn Trắng</li>
              <li>Chúc Mừng Nguyễn Văn T Trúng thưởng 1 phần quà to lớn Đen</li>
            </ul>
          </div>
        </div>
      </div>
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
        <button @click="closeModal" class="form-profile-btn">Đóng</button>
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
    toastImage() {
      return this.prize && this.prize.isPrize === 1
        ? require("../assets/img/ip2.png")
        : require("../assets/img/sorry.png");
    },
    toastIcon() {
      return this.prize && this.prize.isPrize === 1
        ? require("../assets/img/congratulation.png")
        : require("../assets/img/sorry.png");
    }
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
    onInput (e) {
      this.text = e.target.value;
    },
    initPrizeList() {
      this.prizeList = this.formatPrizeList(prizeList)
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
    }
  }
};
</script>
<style scoped>
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
  font-size: 10px;
  margin-top: 4px;
}

.lucky-title {
  width: 50%;
  display: block;
  margin: 0 auto;
  background-size: 100%;
  background-color: #5e2e86;
  padding: 1rem 0;
  width: 100%;
  margin-bottom: 4rem;
}

/* @media screen and ( max-width:768px){
  .lucky-title{
    width: 20%;
  }
} */
.wheel-main {
  margin: 0 auto;
  position: relative;
  width: 300px;
  height: 300px;
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
  width: 40px;
  border-radius: 5px;
  position: absolute;
  top: 15px;
  right: 15px;
}

.flag_tpbank {
  position: absolute;
  top: 15px;
  left: 10px;
  width: 35px;
  height: 35px;
}

.wheel-pointer {
  position: absolute;
  top: 50%;
  left: 50%;
  z-index: 2;
  width: 85px;
  height: 85px;
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
  height: 150px;
  top: 0;
  left: 50%;
  margin-left: -47.5px;
  transform-origin: 50% 100%;
}

.prize-pic img {
  /* width: 4.0625rem; */
  height: 2.5rem;
  margin-top: 1.5625rem;
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
  min-height: 25.5rem;
  background: rgb(243, 109, 86);
  padding-bottom: 1.6875rem;
}

.main-bg {
  width: 100%;
  height: 6.5625rem;
  position: absolute;
  top: -3.4375rem;
  left: 0;
  background: url("../assets/img/luck_bg.png") no-repeat center top;
  background-size: 100%;
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
  z-index: 10000;
  width: 100%;
  height: 100%;
}

.toast {
  position: fixed;
  top: 50%;
  left: 50%;
  z-index: 20000;
  transform: translate(-50%, -50%);
  width: 15.4375rem;
  background: #fff;
  border-radius: 0.3125rem;
  padding: 0.3125rem;
  background-color: rgb(252, 244, 224);
}

.toast-container {
  position: relative;
  width: 100%;
  height: 100%;
  border: 1px dotted #fccc6e;
}

.toast-picture {
  position: absolute;
  top: -6.5rem;
  left: -1.5rem;
  width: 18.75rem;
  height: 8.5625rem;
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

.loading-animation__list-wrapper {
  position: relative;
  height: 8rem;
  overflow: hidden;
}

.loading-animation__list-wrapper .loading-animation__tick {
  position: absolute;
  left: 0;
  top: 2.3rem;
  width: 1rem;
  height: 1rem;
}

.loading-animation__list-wrapper .loading-animation__tick:before {
  display: block;
  position: absolute;
  left: 1px;
  top: 5px;
  content: '';
  width: 9px;
  height: 4px;
  transform: rotate(45deg);
  transform-origin: left;
  background-color: #00d158;
  animation: tick-1 1.6s ease-out infinite;
}

.loading-animation__list-wrapper .loading-animation__tick:after {
  display: block;
  position: absolute;
  left: 5px;
  top: 11px;
  content: '';
  width: 15px;
  height: 4px;
  transform: rotate(-45deg);
  transform-origin: left;
  background-color: #00d158;
  animation: tick-2 1.6s ease-in infinite;
}

.icon-phone,
.icon-card {
  width: 15px;
}

.loading-animation__list-wrapper:after {
  display: block;
  position: absolute;
  content: '';
  width: 100%;
  left: 0;
  bottom: 0;
  height: 6.75rem;
}

.loading-animation__list {
  position: relative;
  padding-left: 2rem;
  max-width: calc(100vw - 2rem);
  animation: list 8s 0.8s linear infinite;
}

.loading-animation__list li {
  font-weight: bold;
  font-size: 0.8rem;
  line-height: 1.25rem;
  margin-bottom: 0.5rem;
  width: 100%;
  animation: fade-out 8s 0.8s ease-out infinite;
}

.loading-animation__list li:nth-child(2) {
  animation-delay: 2.4s;
}

.loading-animation__list li:nth-child(3) {
  animation-delay: 4s;
}

.loading-animation__list li:nth-child(4) {
  animation-delay: 5.6s;
}

.loading-animation__list li:nth-child(5) {
  animation-delay: 7.2s;
}

.loading-animation__list li:nth-child(6) {
  animation-delay: 8.8s;
}

.loading-animation__list li:nth-child(7) {
  animation-delay: 10.4s;
}

.loading-animation__list li:nth-child(8) {
  animation-delay: 12s;
}

.loading-animation__list li:nth-child(9) {
  animation-delay: 13.6s;
}

.loading-animation__list li:nth-child(10) {
  animation-delay: 15.2s;
}

@keyframes list {
  0% {
    transform: translateY(0);
  }

  7%,
  20% {
    transform: translateY(-2.75rem);
  }

  27%,
  40% {
    transform: translateY(-5.5rem);
  }

  47%,
  60% {
    transform: translateY(-8.25rem);
  }

  67%,
  80% {
    transform: translateY(-11rem);
  }

  87%,
  100% {
    transform: translateY(-13.75rem);
  }
}

@keyframes fade-out {
  0% {
    opacity: 1;
  }

  10%,
  70% {
    opacity: 0;
  }

  80%,
  100% {
    opacity: 1;
  }
}

@keyframes tick-1 {
  0% {
    width: 0;
    opacity: 1;
  }

  16% {
    width: 9px;
    opacity: 1;
  }

  50% {
    width: 9px;
    opacity: 1;
  }

  66%,
  100% {
    width: 9px;
    opacity: 0;
  }
}

@keyframes tick-2 {

  0%,
  16% {
    width: 0;
    opacity: 1;
  }

  33% {
    width: 15px;
    opacity: 1;
  }

  50% {
    width: 15px;
    opacity: 1;
  }

  66%,
  100% {
    width: 15px;
    opacity: 0;
  }
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
  position: fixed;
  top: 0;
  z-index: 999999999999999999999999999;
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
  padding: 20px;
  border-radius: 5px;
  width: 330px;
  z-index: 9999999999999999999999999999999;
  position: absolute;
  top: 45%;
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
}

.form-profile-desc {
  margin-top: 25px;
  color: red;
  font-size: 13.5px;
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
  background: #222222;
  background: #16222A;
  background: -webkit-linear-gradient(to top, #16222A, #3A6073);
  background: linear-gradient(to top, #16222A, #3A6073);
  z-index: 99;
  position: absolute;
  width: 100%;
  left: 0;
  /* display: none; */
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
  width: 95%;
  display: flex;
  flex-direction: column;
  top: 30%;
  left: 50%;
  transform: translateX(-50%);
  position: fixed;

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
  color: #3A3F44;
}

#login-number,
#login-code {
  width: 100%;
  border-radius: 0 5px 5px 0;
}

#login-code {
  padding-left: 15px;
}

.form-login label {
  background-color: #222222;
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
  background-color: #00B9BC;
  color: #eee;
  font-weight: bold;
  text-transform: uppercase;
  width: 100%;
  height: 50px;
  border-radius: 5px !important;
}

.form-login .login-btn:focus,
.form-login .login-btn:hover {
  background-color: #197071;
}

.form-field {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  margin-bottom: 2rem;
}
.textUser{
  color: white;
  display: block;
  position: absolute;
  right: 5px;
  top: 70px;
  background: linear-gradient(to bottom, rgba(255,255,255,0.15) 0%, rgba(0,0,0,0.15) 100%), radial-gradient(at top center, rgba(255,255,255,0.40) 0%, rgba(0,0,0,0.40) 120%) #989898;
 background-blend-mode: multiply,multiply;
 border-radius: 5px;
  padding: 4px 8px;
}
</style>

