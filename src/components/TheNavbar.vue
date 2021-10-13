<template>
  <nav id="nav">
    <div
      :class="[
        {
          'shadow-lg scroll-navabar bg-white h-10 sm:shadow-lg bg-transparent z-1': isActive,
        },
        'fixed top-0 transparent-navabar   shadow-lg sm:shadow-lg  left-0 z-40 bg-white py-8 top-0 h-24 w-full',
      ]"
    >
      <div
        class="flex justify-center sm:justify-between px-8  max-w-7xl mx-auto items-center"
      >
        <div
          class="text-center sm:text-auto font-medium flex justify-center text-lg sm:text-2xl "
        >
          Manage Files
        </div>

        <PlusIcon
          class="text-indigo-500 w-10 h-10 cursor-pointer text-right absolute right-0 w-12 h-10 sm:h-12 sm:relative "
        />
      </div>
    </div>
  </nav>
</template>

<script>
import PlusIcon from './icons/PlusIcon'
export default {
  components: {
    PlusIcon,
  },
  data() {
    return {
      isOpen: false,
      isActive: false,
    }
  },
  mounted() {
    window.document.onscroll = () => {
      const navabar = document.getElementById('nav')
      if (window.scrollY > navabar.offsetTop) {
        this.isActive = true
      } else {
        this.isActive = false
      }
    }
  },
}
</script>

<style lang="css">
.transparent-navabar {
  padding: 1.25rem 0.25rem;
  transition: all 500ms ease-in-out;
}
.scroll-navabar {
  padding: 1.25rem 0.25rem;
  transition: all 500ms ease-in-out;
}
</style>
