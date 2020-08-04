<template>
  <div>
      <b-icon id="backToTop" icon="arrow-up" @click="scrollUp()"></b-icon>
  </div>
</template>

<script>
window.onscroll = function() {
    if ((window.innerHeight + window.scrollY) >= 2000) {
        document.querySelector('#backToTop').style.display = "block";
    } else {
        document.querySelector('#backToTop').style.display = "none";
    }
};

export default {
    name: "BackToTop",
    methods: {
            scrollUp: () => {
            window.scroll({
                top: 0,
                behavior: 'smooth'
            });
        }
    }
}
</script>

<style scoped>
#backToTop {
    display: none;
    background: rgba(0,0,0,1);
    color: #fff;
    font-size: 50px;
    border-radius: 50%;

    position: fixed;
    right: 10px;
    bottom: 10px;
}

#backToTop:hover {
    cursor: pointer;
    background: rgba(0,0,0,.8);
}
</style>