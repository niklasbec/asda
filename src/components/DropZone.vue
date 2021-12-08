<template>
  <div
    @dragenter.prevent="toggle_active()"
    @dragleave.prevent="toggle_active()"
    @dragover.prevent
    @drop.prevent="drop"
    :class="{ 'bg-green-100 border-green-300 dark:bg-green-700': dragActive }"
    class="main-container"
  >
    <div class="inner-container">
      <span>
        <img class="main-icon" src="https://img.icons8.com/ultraviolet/40/000000/puzzle.png"/>
      </span>
      <p class="text-base md:text-xl font-semibold">Drag an element here</p>
      <!-- Dropped file info -->
      <div
        v-if="droppedFile !== null"
        class="
          flex flex-wrap
          items-center
          justify-center
          gap-2
          text-base
          font-semibold
          text-gray-600
          dark:text-gray-100
        "
      >
        <button
          class="inline-flex items-center justify-center"
        >
          <span>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              class="w-6 h-6 text-red-500"
              fill="none"
              viewBox="0 0 24 24"
              stroke="currentColor"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z"
              />
            </svg>
          </span>
        </button>
      </div>
    </div>
  </div>
</template>
<script setup>
import useDropZone from "./dropzone.js";
const { dragActive, droppedFile, toggle_active, drop } =
  useDropZone();
</script>

<style lang="less" scoped>
.main-container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  .inner-container {
    .main-icon {
      width: 50px;
      height: 50px;
    }
  }
}
</style>