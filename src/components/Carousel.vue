<template>
  <div class="page">
    <div class="carousel">
      <div class="controls">
        <svg
          v-on:click="toggleNext()"
          class="arrow next"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 36.75 69.88"
        >
          <defs>
            <svg:style>.cls-1{fill:#fff;}</svg:style>
          </defs>
          <g id="Layer_2" data-name="Layer 2">
            <g id="Layer_1-2" data-name="Layer 1">
              <polygon class="cls-1" points="1.17 0 0 1.16 35.58 36.27 36.75 35.12 1.17 0" />
              <polygon
                class="cls-1"
                points="1.52 69.88 0.36 68.74 35.59 33.97 36.75 35.12 1.52 69.88"
              />
            </g>
          </g>
        </svg>
        <svg
          v-on:click="togglePrev()"
          class="arrow prev"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 36.75 69.88"
        >
          <defs>
            <svg:style>.cls-1{fill:#fff;}</svg:style>
          </defs>
          <g id="Layer_2" data-name="Layer 2">
            <g id="Layer_1-2" data-name="Layer 1">
              <polygon class="cls-1" points="0 35.12 1.17 36.27 36.75 1.16 35.58 0 0 35.12" />
              <polygon class="cls-1" points="0 35.12 1.16 33.97 36.39 68.74 35.23 69.88 0 35.12" />
            </g>
          </g>
        </svg>
      </div>
      <div class="slider">
        <div class="section" v-for="item in data" :key="item.title">
          <img class="image mobile" v-bind:src="item.media.mobile" alt />
          <img class="image desktop" v-bind:src="item.media.desktop" alt />
          <div :class="`action-items ${item.ctaPosition}`">
            <h1 class="title">{{item.title}}</h1>
            <h2 class="sub-heading" v-if="item.heading">{{item.heading}}</h2>
            <div class="button-container">
              <a class="button" :href="`${item.cta[0].url}`">
                <span>{{item.cta[0].label}}</span>
              </a>
              <a v-if="item.cta[1]" :href="`${item.cta[1].url}`" class="button">
                <span>{{item.cta[1].label}}</span>
              </a>
            </div>
          </div>
        </div>
      </div>
      <div class="dot-container">
        <div class="dot" v-for="item in data" :key="item" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Carousel",
  props: {
    msg: String
  },
  data() {
    return {
      data: null,
      direction: 1
    };
  },
  created() {
    this.fetchData();
  },
  mounted() {
    this.cycle();
  },
  methods: {
    fetchData() {
      axios
        .get("https://frontend-assessment-service.vcomm.io")
        .then(response => (this.data = response.data.data));
    },
    toggleNext() {
      const carousel = document.querySelector(".carousel");
      const slider = document.querySelector(".slider");
      if (this.direction === -1) {
        this.direction = 1;
        slider.prepend(slider.lastElementChild);
      }
      carousel.style.justifyContent = "flex-start";
      slider.style.transform = "translate(-33%)";
      setTimeout(() => {
        slider.appendChild(slider.firstElementChild);
        slider.style.transition = "none";
        slider.style.transform = "translate(0)";
        setTimeout(() => {
          slider.style.transition = "all 0.5s";
        }, 500);
      }, 500);
    },
    togglePrev() {
      const carousel = document.querySelector(".carousel");
      const slider = document.querySelector(".slider");
      if (this.direction === 1) {
        this.direction = -1;
        slider.appendChild(slider.firstElementChild);
      }
      carousel.style.justifyContent = "flex-end";
      slider.style.transform = "translate(33%)";
      setTimeout(() => {
        slider.prepend(slider.lastElementChild);
        slider.style.transition = "none";
        slider.style.transform = "translate(0)";
        setTimeout(() => {
          slider.style.transition = "all 0.5s";
        }, 500);
      }, 500);
    },
    cycle() {
      clearInterval();
      const self = this;
      setInterval(function() {
        self.toggleNext();
      }, 5000);

      clearInterval();
    }
  }
};
</script>

<style scoped lang="scss">
$screen-lg: 990px;
$screen-md: 767px;

a {
  text-decoration: none;
  color: white;
}

.page {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  @media (min-width: $screen-md) {
    grid-template-columns: repeat(6, 1fr);
  }
  @media (min-width: $screen-lg) {
    grid-template-columns: repeat(12, 1fr);
  }

  .carousel {
    grid-column: 1/5;
    height: 100%;
    display: flex;
    justify-content: flex-start;
    overflow: hidden;
    position: relative;
    border-radius: 10px;
    box-shadow: 10px 5px 5px gray;
    @media (min-width: $screen-md) {
      grid-column: 1/7;
    }
    @media (min-width: $screen-lg) {
      grid-column: 2/12;
    }

    .slider {
      display: flex;
      height: 100%;
      width: 300%;
      flex-shrink: 0;
      transition: all 0.5s;

      .section {
        flex-basis: 33%;
        flex-shrink: 0;
        flex: 1;
        width: 33%;
        align-items: center;
        justify-content: center;
        position: relative;

        .action-items {
          position: absolute;
          z-index: 1;
          top: 45%;
          left: 10%;
          display: flex;
          flex-direction: column;
          color: white;
          width: 80%;
          align-items: center;
        }

        .right {
          @media (min-width: $screen-md) {
            left: 55%;
            width: 33%;
            align-items: flex-start;
            justify-content: flex-start;
          }
          .button-container {
            @media (min-width: $screen-md) {
              align-items: flex-start;
              justify-content: flex-start;
            }

            .button {
              @media (min-width: $screen-md) {
                margin: 0;
                width: 70%;
              }
            }
          }
        }

        .left {
          @media (min-width: $screen-md) {
            right: 55%;
            width: 33%;
            align-items: flex-start;
            justify-content: flex-start;
          }
          .button-container {
            @media (min-width: $screen-md) {
              align-items: flex-end;
              justify-content: flex-start;
            }

            .button {
              @media (min-width: $screen-md) {
                margin: 0;
                width: 70%;
              }
            }
          }
        }
      }
    }
  }
}

.image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.button-container {
  display: flex;
  width: 100%;
  flex-direction: column;
  @media (min-width: $screen-md) {
    flex-direction: row;
    justify-content: center;
  }

  .button {
    border: 2px solid white;
    width: 90%;
    padding: 10px;
    cursor: pointer;
    margin-bottom: 1rem;
    transition: 0.2s;
    text-transform: uppercase;
    @media (min-width: $screen-md) {
      width: 40%;
      margin: 0 1rem 0 1rem;
    }

    &:hover {
      transition: 0.2s;
      background-color: white;
      color: black;
    }
  }
}

.title {
  text-transform: uppercase;
  margin: 0 0 1rem 0;
}

.sub-heading {
  font-size: 0.875rem;
}

.controls {
  z-index: 1;

  .arrow {
    position: absolute;
    color: white;
    top: 30%;
    cursor: pointer;
    height: 3rem;

    @media (min-width: $screen-md) {
      top: calc(50% - 1.5rem);
    }
  }
  .prev {
    left: 10px;
  }
  .next {
    right: 10px;
  }
}

.mobile {
  @media (min-width: $screen-md) {
    display: none;
  }
}

.desktop {
  display: none;
  @media (min-width: $screen-md) {
    display: block;
  }
}

.dot-container {
  position: absolute;
  bottom: 10px;
  width: 80%;
  left: 10%;
  display: flex;
  align-items: center;
  justify-content: center;


  .dot {
    height: .5rem;
    width: .5rem;
    background-color: white;
    border-radius: 50%;
    z-index: 5;
    margin: 0 1rem 0 1rem;
  }
}
</style>
