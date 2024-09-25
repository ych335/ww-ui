<template>
    <div ref="navbar" class="navbar pxtoview-ignore" :class="{ fix: fixed }">
      <slot></slot>
      <div v-for="index in 7" :key="index" class="nav" :class="['nav' + index, navbarArr[index - 1].spClass]"></div>
      <div v-show="route.name == 'pkteam'" class="btn-back" @click="handleBack()"></div>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted } from "vue";
  import { useRoute, useRouter } from "vue-router";
  
  const route = useRoute();
  const router = useRouter();
  const navbarArr = [
    {
      name: "首页",
      spClass: "inarr",
    },
    {
      name: "赛事亮点",
      spClass: "inarr",
    },
    {
      name: "赛事规则",
      spClass: "inarr",
    },
    {
      name: "赛事进程",
      spClass: "inarr",
    },
    {
      name: "赛事奖励",
      spClass: "inarr",
    },
    {
      name: "赛事资讯",
      spClass: "inarr",
    },
    {
      name: "无差别主播团",
      spClass: "",
    },
    {
      name: "总决赛直播",
      spClass: "inarr",
    },
  ];
  const fixed = ref(false);
  const navbar = ref(null);
  
  function setNavbar() {
    const $win = $(window);
  
    function getTopArr() {
      const $title = $(".cont > .title");
      const arr = [];
      for (let i = 0; i < $title.length; i += 1) {
        const { top } = $title.eq(i).offset();
        arr.push(top);
      }
      arr.unshift(0);
      return arr;
    }
  
    let topArr = getTopArr();
    let idx = 0;
  
    $(".navbar .nav.inarr").click(function () {
      const index = $(this).index(".inarr");
      if (route.name === "pkteam") {
        router.push("/index");
  
        setTimeout(() => {
          topArr = getTopArr();
          $(this).addClass("active").siblings().removeClass("active");
  
          window.scrollTo({
            top: topArr[index] - 120,
            behavior: "smooth",
          });
        }, 300);
      } else {
        topArr = getTopArr();
        $(this).addClass("active").siblings().removeClass("active");
  
        window.scrollTo({
          top: topArr[index] - 120,
          behavior: "smooth",
        });
      }
    });
  
    $(".navbar .nav")
      .eq(6)
      .on("click", function () {
        $(this).addClass("active").siblings().removeClass("active");
        if (route.name === "pkteam") {
          window.scrollTo({
            top: 0,
            behavior: "smooth",
          });
        } else {
          router.push("/pkteam");
  
          setTimeout(() => {
            window.scrollTo({
              top: 0,
              behavior: "smooth",
            });
          }, 100);
        }
      });
  
    $win
      .on("scroll", function () {
        if (route.name === "pkteam") {
          $(".navbar .nav")
            .eq(6)
            .addClass("active")
            .siblings()
            .removeClass("active");
  
          return;
        }
  
        topArr = getTopArr();
        const top = $(window).scrollTop();
  
        topArr.forEach(function (tittop, i) {
          if (tittop < top + 180) {
            idx = i;
          }
        });
        $(".navbar .nav.inarr")
          .eq(idx)
          .addClass("active")
          .siblings()
          .removeClass("active");
      })
      .trigger("scroll");
  }
  
  function handleBack() {
    router.push("/index");
    setTimeout(() => {
      $(".navbar .nav.inarr")
        .eq(0)
        .addClass("active")
        .siblings()
        .removeClass("active");
  
      window.scrollTo({
        top: 0,
        behavior: "smooth",
      });
    }, 100);
  }
  
  router.beforeEach((to, from) => {
    if (to.name === "pkteam") {
      $(".navbar .nav").eq(6).addClass("active").siblings().removeClass("active");
    }
  });
  
  function setPos() {
    if ($(".navbar").hasClass('fix')) {
      if ($(window).width() < 1880) {
        $(".navbar.fix").css('left', -window.scrollX + 'px')
        return
      }
    } 
      
    $(".navbar").css('left', '')
  }
  
  onMounted(() => {
    setNavbar();
  
    window.addEventListener("resize", () => {
      setPos()
    });
    window.addEventListener("scroll", () => {
      setPos()
    });
  
    const topbar = document.querySelector(".topbar");
    document.addEventListener("scroll", () => {
      if (topbar.getBoundingClientRect().bottom < 0) {
        fixed.value = true;
      } else {
        fixed.value = false;
      }
    });
  });
  </script>
  
<style lang="less" scoped>
@import url('./index.less');
</style>
  