<template>
  <div>
    <FixedLayout />
    <section class="main">
      <div class="container">
        <div class="title-inner">
          <h2 class="title">kimdabeen</h2>
        </div>
        <div class="round-box">
          <div class="eye-inner">
            <span class="eye"></span>
            <span class="eye"></span>
          </div>
          <svg class="mouth" xmlns="http://www.w3.org/2000/svg" width="59" height="15" viewBox="0 0 59 15" fill="currentcolor">
            <path d="M29.5 15.0001C45.1967 15.0001 54.236 7.93116 59 1.55185L56.9845 0.00012195C51.3043 5.86219 42.6315 10.1151 29.5 10.1151C16.3685 10.1151 7.69565 5.86219 2.01553 0.000118672L-9.25456e-08 1.55184C4.76397 7.93115 13.8033 15.0001 29.5 15.0001Z" fill="#currentcolor"></path>
          </svg>
          <!-- <span class="mouth"></span> -->
        </div>
      </div>
      <div class="container">
        <div class="title-inner">
          <h2 class="title">publisher &</h2>
        </div>
      </div>
      <div class="container">
        <router-link  v-if="!isLoggedIn" class="round-box btn" to="/signUp">Sign Up</router-link>
        <router-link v-else @click="logout" class="round-box btn" to="/edit">Edit Info</router-link>
        <div class="title-inner">
          <h2 class="title">front-end</h2>
        </div>
      </div>
      <div class="container">
        <div class="title-inner">
          <h2 class="title">developer</h2>
        </div>
        <router-link v-if="!isLoggedIn" class="round-box btn" to="/login">Login</router-link>
        <button v-else @click="logout" class="round-box btn">Logout</button>
      </div>
    </section>

    <section class="about">
      <p class="about__text">안녕하세요. 김다빈입니다.</p>
    </section>
  </div>
</template>

<script>
import FixedLayout from "@/components/FixedLayout";
import store from "../vuex";

export default {
  name: "Main",
  components: {
    FixedLayout
  },
  data() {
    return {
    };
  },
  computed: {
    isLoggedIn() {
      return store.state.user || false;
    }
  },
  created() {
    if (window.location.pathname === "/") {
      setTimeout(() => {
        this.loadPage();
      }, 100);
    };
  },
  mounted() {
    this.moveElement();
  },
  methods: {
    loadPage() {
      const titles = document.querySelectorAll("h2.title");
      const roundBtn = document.querySelectorAll(".round-box");

      for (let i = 0; i < titles.length; i++) {
        const item = titles.item(i);
        item.style.transform = "translate3d(0, 0, 0) rotateZ(0deg)";
      };

      setTimeout(() => {
        for (let i = 0; i < roundBtn.length; i++) {
          const item = roundBtn.item(i);
          item.style.opacity = "1";
          item.style.transform = "translateX(0)";
        }
      }, 1100);
    },

    moveElement() {
      const eyes = document.querySelectorAll(".eye, .mouth");
      // const roundBtn = document.querySelectorAll(".round-box");
      const limit = 15;

      const calcValue = (mouse, width) => {
        return (((mouse / width) * limit) - (limit / 2)).toFixed(1);
      };

      const xValue = e => calcValue(e.x, window.innerWidth);
      const yValue = e => calcValue(e.y, window.innerHeight);

      document.addEventListener("mousemove", (e) => {
        [...eyes].forEach((item) => {
          item.setAttribute("ry", "7.5");
          item.style.transform = `translateX(${xValue(e) * 0.8}px) translateY(${yValue(e) * 0.8}px)`;
        });
      }, false);
    },

    logout() {
      store.dispatch("logout").then(() => {
        localStorage.removeItem("userInfo");
        this.$router.push("/login");
      });
    }
  }
};
</script>

<style scoped>
section {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  border-bottom: 1px solid #d2ad75;
}
.main {
  padding-top: 6rem;
}
.main .container {
  display: flex;
}
.main .container .title-inner {
  overflow: hidden;
}
.main .container:first-of-type .title-inner {
  padding: 0 2rem;
}
.main .container .title-inner .title {
  font-size: 14rem;
  line-height: 1;
  letter-spacing: -0.2rem;
  font-family: 'Trirong-Regular', serif;
  text-transform: uppercase;
  text-align: center;
  transform: translate3d(0, 140%, 0) rotateZ(7deg);
  transition: transform 1s cubic-bezier(.05, .96, .8, .98);
}
.main .container:first-of-type .title-inner .title {
  font-size: 13.5rem;
  font-style: italic;
}
.main .container .round-box {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 15rem;
  height: 15rem;
  color: #d2ad75;
  border-radius: 50%;
  border: 1px solid #d2ad75;
  backdrop-filter: blur(4.2px);
  overflow: hidden;
  opacity: 0;
  z-index: 5;
  transition: transform .3s, color .2s;
}
.main .container:nth-of-type(1) .round-box {
  left: -4%;
  justify-content: space-evenly;
  transform: translateY(-400%);
}
.main .container:nth-of-type(3) .round-box {
  right: -4%;
  transform: translateX(-400%);
}
.main .container:nth-of-type(4) .round-box {
  left: -5%;
  transform: translateX(400%);
}
.main .container .round-box.btn {
  font-size: 1.8rem;
}
.main .container .round-box.btn:hover {
  color: #1d1a1b;
}
.main .container:not(:first-of-type) .round-box::before {
  content: "";
  position: absolute;
  left: 0;
  bottom: -103%;
  width: 15rem;
  height: 15rem;
  background-color: #d2ad75;
  transition: bottom .5s cubic-bezier(.05, .96, .8, .98);
  z-index: -1;
}
.main .container:not(:first-of-type) .round-box:hover::before {
  bottom: 0;
}
.main .container .round-box .eye-inner {
  margin-top: 1.5rem;
}
.main .container .round-box .eye-inner .eye {
  display: inline-block;
  width: 1.5rem;
  height: 1.5rem;
  margin: 0 1.5rem;
  border-radius: 50%;
  background-color: #d2ad75;
}
.main .container:first-of-type .round-box:hover .eye-inner .eye {
  animation: eyes 0.3s;
}
/* .main .container .round-box .mouth {
  display: block;
  width: 4rem;
  height: 0.5rem;
  background-color: #d2ad75;
} */

.about__text {
  font-size: 2rem;
}

/* animation */
@keyframes eyes {
  0%, 60% {
    height: 1.5rem;
  }
  30%, 100% {
    height: 0.2rem;
  }
}

:root {
  --content-size: fit-content;
}

/* 반응형 */
@media screen and (max-width: 1200px) {
  .main .container .title-inner .title {
    font-size: 12rem;
  }
  .main .container:first-of-type .title-inner .title {
    font-size: 11.5rem;
  }
  .main .container .round-box, .main .container:not(:first-of-type) .round-box::before {
    width: 13rem;
    height: 13rem;
  }
}

@media screen and (max-width: 1024px) {
  section {
    height: 90vh;
  }
  .main .container .title-inner .title {
    font-size: 9rem;
  }
  .main .container:first-of-type .title-inner .title {
    font-size: 8.5rem;
  }
  .main .container .round-box, .main .container:not(:first-of-type) .round-box::before {
    width: 10rem;
    height: 10rem;
  }
  .main .container .round-box .eye-inner .eye {
    width: 1.1rem;
    height: 1.1rem;
    margin: 0 1.1rem;
  }
  .main .container .round-box.btn {
    font-size: 1.4rem;
  }
}

@media screen and (max-width: 768px) {
  section {
    height: 80vh;
  }
  .main .container .title-inner .title, .main .container:first-of-type .title-inner .title {
    font-size: 6.8rem;
    line-height: 1.1;
  }
  .main .container .round-box {
    display: none;
  }
}

@media screen and (max-width: 540px) {
  .main .container .title-inner .title, .main .container:first-of-type .title-inner .title {
    font-size: 6rem;
  }
}
@media screen and (max-width: 480px) {
  section {
    height: 75vh;
  }
  .main .container .title-inner .title, .main .container:first-of-type .title-inner .title {
    font-size: 4.5rem;
  }
}
</style>
