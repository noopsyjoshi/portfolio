<template>
  <div class="cursor" :class="`cursor--${cursorColor}`">
    <div ref="cursorInner" class="cursor cursor--inner" />
    <div ref="cursorOuter" class="cursor cursor--outer" />
    <div ref="cursorText" class="cursor cursor--text">&#43;</div>
  </div>
</template>

<style lang="scss">
@import '@/assets/scss/atomic/elements/_cursor.scss';
</style>

<script>
import { TweenMax } from 'gsap/all';

export default {
  props: {
    cursorColor: {
      type: String,
      default: () => '',
    },
  },

  created() {
    window.addEventListener('mousemove', this.moveCursor);
  },

  destroyed() {
    window.removeEventListener('mousemove', this.moveCursor);
  },

  mounted() {
    this.cursorInner = this.$refs.cursorInner;
    this.cursorOuter = this.$refs.cursorOuter;
    this.cursorText = this.$refs.cursorText;

    this.navigations = Array.from(document.querySelectorAll('.link'));
    this.projects = Array.from(document.querySelectorAll('.projects__project-image'));

    this.navigations.forEach((nav) => {
      nav.addEventListener('mouseover', this.hoverCursor);
    });

    this.navigations.forEach((nav) => {
      nav.addEventListener('mouseout', this.hoverCursorOut);
    });

    this.projects.forEach((project) => {
      project.addEventListener('mouseover', this.hoverCursorProject);
    });

    this.projects.forEach((project) => {
      project.addEventListener('mouseout', this.hoverCursorOutProject);
    });
  },

  methods: {
    moveCursor() {
      if (this.cursorInner && this.cursorOuter && this.cursorText) {
        TweenMax.to(this.cursorInner, 0.1, {
          x: event.clientX,
          y: event.clientY,
        });

        TweenMax.to(this.cursorOuter, 0.3, {
          x: event.clientX,
          y: event.clientY,
        });

        TweenMax.to(this.cursorText, 0.3, {
          x: event.clientX,
          y: event.clientY,
        });
      }
    },

    // Navigation
    hoverCursor() {
      TweenMax.to(this.cursorInner, 0.1, {
        opacity: 1,
        scale: 0,
      });
      
      TweenMax.to(this.cursorOuter, 0.3, {
        scale: 2,
        opacity: 1,
      });
    },

    hoverCursorOut() {
      TweenMax.to(this.cursorInner, 0.1, {
        opacity: 1,
        scale: 1,
      });

      TweenMax.to(this.cursorOuter, 0.3, {
        scale: 1,
        opacity: 0.4,
        backgroundColor: 'transparent',
      });
    },

    // Projects
    hoverCursorProject() {
      TweenMax.to(this.cursorInner, 0.1, {
        opacity: 1,
        scale: 0,
      });

      TweenMax.to(this.cursorText, 0.3, {
        scale: 3,
        opacity: 1,
      });
      
      TweenMax.to(this.cursorOuter, 0.3, {
        scale: 4,
        opacity: 1,
        borderColor: 'transparent',
      });
     
    },

    hoverCursorOutProject() {
      TweenMax.to(this.cursorInner, 0.1, {
        opacity: 1,
        scale: 1,
      });

      TweenMax.to(this.cursorOuter, 0.3, {
        scale: 1,
        opacity: 0.4,
        backgroundColor: 'transparent',
        borderColor: 'rgba(255, 255, 255, 1)',
      });

      TweenMax.to(this.cursorText, 0.3, {
        opacity: 0,
        scale: 0,
      });
    },
  },
};

</script>
