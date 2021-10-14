<template>
  <div class="mt-32 relative mb-10 h-full max-w-7xl mx-auto w-full px-8">
    <!-- file upload section-->
    <div>
      <transition name="fade" mode="out-in">
        <file-upload class="" />
      </transition>
    </div>
  </div>
</template>

<script>
// import FileUploadCompleted from './FileUploadCompleted.vue'
// import LoadingIcon from './icons/LoadingIcon.vue'
// import FileIcon from './icons/FileIcon.vue'
// import CheckMarkIcon from './icons/CheckMarkIcon.vue'
// import IncompleteFileUpload from './IncompleteFileUpload.vue'
// import NextUpFileUpload from './NextUpFileUpload.vue'
import FileUpload from './FileUpload.vue'
// import XIcon from './icons/XIcon.vue'

export default {
  components: {
    FileUpload,

    // XIcon,
    // FileUploadCompleted,
    // RetryIcon,
    // FileIcon,
    // IncompleteFileUpload,
    // CheckMarkIcon,
    // NextUpFileUpload,
    // LoadingIcon,
  },
  data() {
    return {
      isUploading: true,
      isNextUp: false,
      isCompleted: false,
      isIncomplete: false,
      isFileAdded: false,
    }
  },
}
</script>

<style lang="css">
/* @import './assets/vuedropzone.css'; */
</style>
