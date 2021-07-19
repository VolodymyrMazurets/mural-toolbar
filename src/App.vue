<template>
  <div class="App">
    <div class="App__area" ref="container" @click="handleContainerClick">Draggable zone. Click on toolbar to turn on draggable mode. To dissable it click on empty space in this container</div>
    <div class="App__toolbar" @click="handleToolbarClick">
      <div
        v-for="item in [1, 2, 3, 4]"
        :key="item"
        class="App__circle"
        :ref="`item${item}`"
      >
        {{ item }}
      </div>
    </div>
  </div>
</template>

<script>
import { gsap } from "gsap";
import { Draggable } from "gsap/Draggable";

gsap.registerPlugin(Draggable);

export default {
  name: "App",
  data() {
    return {
      draggable: null,
    };
  },
  components: {},
  mounted() {},
  methods: {
    handleToolbarClick() {
      const { container, item1, item2, item3, item4 } = this.$refs;
      if (!this.draggable) {
        gsap.to([item1[0], item2[0], item3[0], item4[0]], {
          y: "-=300",
          backgroundColor: "red",
          duration: 0.5,
          ease: "power2.inOut",
          stagger: 0.1,
          onComplete: () => {
            this.draggable = Draggable.create([item1, item2, item3, item4], {
              bounds: container,
              throwProps: true,
              onClick: function() {
                console.log("clicked");
              },
              onDragEnd: function() {
                console.log("drag ended");
              },
            });
          },
        });
      }
    },
    handleContainerClick() {
      const { item1, item2, item3, item4 } = this.$refs;
      // eslint-disable-next-line no-extra-boolean-cast
      if (!!this.draggable) {
        this.draggable.forEach((element) => {
          element.kill();
        });
        this.draggable = null;
        gsap.to([item1[0], item2[0], item3[0], item4[0]], {
          backgroundColor: "rgb(189, 255, 183)",
          x: 0,
          y: 0,
          duration: 0.5,
          ease: "power2.inOut",
          stagger: 0.1,
        });
      }
    },
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.App {
  min-height: 100vh;
  background-color: rgb(253, 236, 236);
  padding: 10px;
  &__area {
    width: 80%;
    height: 80vh;
    margin: auto;
    background-color: rgb(218, 218, 218);
    border-radius: 4px;
  }
  &__toolbar {
    border-radius: 8px;
    background-color: rgb(192, 192, 192);
    padding: 10px;
    position: fixed;
    bottom: 10px;
    left: 50%;
    transform: translateX(-50%);
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 20px;
  }
  &__circle {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background-color: rgb(189, 255, 183);
    display: flex;
    justify-content: center;
    align-items: center;
  }
}
</style>
