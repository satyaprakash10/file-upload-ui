<template>
  <div class="mt-32 relative mb-10 h-full max-w-7xl mx-auto w-full px-8">
    <div class="flex items-center justify-between mt-6">
      <h1 class="font-medium text-base sm:text-xl text-gray-900">
        Uploading
      </h1>

      <a
        @click="vremoved"
        class="font-normal focus:outline-none hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-blue-400"
      >
        Cancel Upload

        <ChevronUpArrowIcon
          @click="isUploading = !isUploading"
          class="mr-5 ml-4 text-gray-900 "
          style="40px"
          v-if="!isUploading"
        />
        <ChevronDownArrowIcon
          class="mr-5 ml-4 text-gray-900 "
          style="40px"
          v-else
        />
      </a>
    </div>

    <!-- border  -->
    <div class="border mt-3 mb-6 border-gray-200 max-w-7xl mx-auto"></div>

    <!-- Vue Dropzone file upload -->
    <div class="" v-show="isUploading">
      <vue-dropzone
        id="dropzone"
        class="bg-gray-100  hover:bg-gray-200 hover:border-gray-200 cursor-pointer hover:text-gray-600 px-4 py-5 rounded-md shadow-sm hover:shadow-sm transition-all duration-300"
        ref="myVueDropzone"
        :useCustomSlot="true"
        :options="dropzoneOptions"
        @vdropzone-error="errorFiles"
        @vdropzone-success="successFiles"
        @vdropzone-files-added="filesAddedHandler"
        @vdropzone-total-upload-progress="progressHandler"
      >
        <div class="dropzone-custom-content">
          <h3 class="dropzone-custom-title">
            Drag and drop to upload File content!
          </h3>
          <div class="subtitle">
            ...or click to select a file from your computer
          </div>
        </div>
      </vue-dropzone>
    </div>

    <!-- Next up file uploading section -->
    <div class="mt-10">
      <div class="flex items-center justify-between">
        <h1 class="font-medium text-base sm:text-xl text-gray-900">
          Next Up
        </h1>

        <a
          @click="vremoved"
          class="font-normal focus:outline-none hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-blue-400"
        >
          Cancel all

          <ChevronUpArrowIcon
            @click="isNextUp = !isNextUp"
            class="mr-5 ml-4 text-gray-900 "
            style="40px"
            v-if="!isNextUp"
          />
          <ChevronDownArrowIcon
            class="mr-5 ml-4 text-gray-900 "
            style="40px"
            v-else
          />
        </a>
      </div>

      <!-- border  -->
      <div class="border mt-3 mb-6 border-gray-200 max-w-7xl mx-auto"></div>

      <div v-show="isNextUp">
        <div
          v-for="(file, index) in pendingFileList"
          :key="index"
          class="bg-gray-100 hover:bg-gray-200 hover:border-gray-400 rounded-md shadow-sm px-6 py-6 h-32 mb-6"
        >
          <img :src="pendingFileList.file" />
          <div class="relative">
            <XIcon
              v-if="pendingFileList.length"
              class="-top-18 absolute cursor-pointer disabled h-4 hover:text-blue-500 right-0 w-8 z-10"
            />
          </div>
        </div>
      </div>
    </div>

    <!-- Completed uploads Section -->
    <div class="mt-10">
      <div class="flex items-center justify-between">
        <h1 class="font-medium text-base sm:text-xl text-gray-900">
          Completed
        </h1>

        <a
          @click="vremoved"
          class="font-normal focus:outline-none hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-blue-400"
        >
          <ChevronUpArrowIcon
            @click="isCompleted = !isCompleted"
            class="mr-5 ml-4 text-gray-900 "
            style="40px"
            v-if="!isCompleted"
          />
          <ChevronDownArrowIcon
            class="mr-5 ml-4 text-gray-900 "
            style="40px"
            v-else
          />
        </a>
      </div>

      <!-- border  -->
      <div class="border mt-3 mb-6 border-gray-200 max-w-7xl mx-auto"></div>

      <div
        v-show="isCompleted"
        v-for="(i, index) in successFilesList"
        :key="index"
        class="bg-gray-100 hover:bg-gray-200 hover:border-gray-400 rounded-md shadow-sm px-6 py-6 h-32 mb-6"
      >
        {{ successFilesList }}
        <img :src="dataURL.data" />
        <div v-show="isCompleted" class="relative">
          <XIcon
            v-if="successFilesList.length"
            class="-top-20 absolute cursor-pointer disabled h-4 hover:text-blue-500 right-0 w-8 z-10"
          />
        </div>
      </div>

      <div
        class="font-normal text-gray-400 text-sm mt-5"
        v-if="!successFilesList"
      >
        No file uploads completed yet.
      </div>
    </div>

    <!-- Incomplete Uploading Section -->
    <div class="mt-10">
      <div class="flex items-center justify-between">
        <h1 class="font-medium text-base sm:text-xl text-gray-900">
          Incomplete Uploads
        </h1>

        <button
          class="font-normal focus:outline-none hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-blue-400"
        >
          <ChevronUpArrowIcon
            @click="isIncomplete = !isIncomplete"
            class="mr-5 ml-4 text-gray-900 "
            style="40px"
            v-if="!isIncomplete"
          />
          <ChevronDownArrowIcon
            class="mr-5 ml-4 text-gray-900 "
            style="40px"
            v-else
          />
        </button>
      </div>

      <!-- border  -->
      <div class="border mt-3 mb-6 border-gray-200 max-w-7xl mx-auto"></div>

      <div
        v-show="isIncomplete"
        v-for="(i, index) in rejectedFilesList"
        :key="index"
        class="bg-gray-100 hover:bg-gray-200 hover:border-gray-400 rounded-md shadow-sm px-6 py-6 h-32 mb-6"
      >
        {{ rejectedFilesList }}
        <img :src="rejectedFilesList[i]" />
        <div v-show="isIncomplete" class="relative">
          <XIcon
            class="-top-20 absolute cursor-pointer disabled h-4 hover:text-blue-500 right-0 w-8 z-10"
          />
        </div>
      </div>

      <div class="font-normal text-gray-400 text-sm mt-5" v-if="!errorFiles">
        No incomplete upoads yet.
      </div>
    </div>
  </div>
</template>

<script>
import vue2Dropzone from 'vue2-dropzone'
import 'vue2-dropzone/dist/vue2Dropzone.min.css'
import ChevronDownArrowIcon from './icons/ChevronDownArrowIcon.vue'
import ChevronUpArrowIcon from './icons/ChevronUpArrowIcon.vue'
import XIcon from './icons/XIcon.vue'
export default {
  name: 'App',
  components: {
    vueDropzone: vue2Dropzone,
    ChevronDownArrowIcon,
    ChevronUpArrowIcon,
    XIcon,
  },
  data: function() {
    return {
      dropzoneOptions: {
        url: 'https://httpbin.org/post',
        thumbnailWidth: 150,
        // maxFilesize: 0.5,
        headers: { 'My-Awesome-Header': 'header value' },
      },
      isUploading: true,
      isNextUp: true,
      isCompleted: true,
      isIncomplete: true,
      fileId: 1,
      success: false,
      error: false,
      // removedFile: false,
      pendingFileList: [],
      successFilesList: [],
      rejectedFilesList: [],
    }
  },

  watch: {
    removedFile() {
      let that = this
      setTimeout(function() {
        that.removedFile = false
      }, 2000)
    },
  },

  methods: {
    // Success Completed uploads file
    async successFiles(file) {
      this.successFilesList = [
        ...this.successFilesList,
        { id: file?.lastModified, file },
      ]
      this.pendingFileList = this.pendingFileList?.filter(
        (f) => f.id != file?.lastModified
      )
      this.$refs?.myVueDropzone?.removeFile(file)
    },

    // Incomplete files
    errorFiles(file) {
      this.rejectedFilesList = [
        ...this.rejectedFilesList,
        { id: file?.lastModified, file },
      ]
    },

    // Add push files uploading
    filesAddedHandler(files) {
      for (let i = 0; i < files?.length; i++) {
        this.pendingFileList.push({
          id: files[i]?.lastModified,
          file: files[i],
        })
      }
    },

    // Remove file
    vremoved(file, xhr, error) {
      console.log('File', file)
      console.log('Xhr', xhr)
      console.log('Error', error)

      this.removedFile = true

      // alert('Your file removed successfully.')
    },

    // Remove Pending files
    removePendings() {
      for (let i = 0; i < this.pendingFileList?.length; i++) {
        // remove from drag and drop zone
        this.$refs?.myVueDropzone?.removeFile(this.pendingFileList[i])
        // remove from pending
        console.log(this.pendingFileList[i])
        this.pendingFileList = this.pendingFileList?.filter(
          (f) => f?.id == this.pendingFileList[i]?.lastModified
        )
      }
    },

    // Progress bar status
    progressHandler(totaluploadprogress, totalBytes, totalBytesSent) {
      console.log('totaluploadprogress ', totaluploadprogress)
      console.log('totalBytes ', totalBytes)
      console.log('totalBytesSent ', totalBytesSent)
    },
  },
}
</script>

<style lang="css">
.vue-dropzone {
  border: 2px solid rgb(212, 212, 212);
  font-family: Arial, sans-serif;
  letter-spacing: 0.2px;
  color: #777;
  background: rgba(246, 247, 252, 1);
  transition: 0.2s linear;
}
.vue-dropzone::hover() {
  border: 2px solid rgb(175, 175, 175);
  font-family: Arial, sans-serif;
  background: rgb(153, 152, 152);
  letter-spacing: 0.2px;
  color: #777;
  transition: 0.2s linear;
}
.dropzone {
  min-height: 150px;
  background: #f6f7fc;
  /* padding: 20px 20px; */
}
.vue-dropzone {
  border: white;
  font-family: Arial, sans-serif;
  letter-spacing: 0.2px;
  color: #777;
  transition: 0.1s linear;
}

.rounded-md {
  border-radius: 0.5rem;
}

.vue-dropzone:hover {
  background-color: #e2e7ea94;
  border: 2px solid #dadada;
}
.vue-dropzone > .dz-preview .dz-remove {
  z-index: 30;
  margin-left: 12px;
  padding: 10px;
  position: relative;
  /* border: 2px #d3d3d3 solid; */
  text-decoration: none;
  text-transform: uppercase;
  font-size: 0.8rem;
  font-weight: 800;
  letter-spacing: 1.1px;
  opacity: 0;
}

.dropzone .dz-preview .dz-remove {
  font-size: 14px;
  text-align: center;
  display: block;
  cursor: pointer;
  border: none;
  margin-top: 22px;
}
.vue-dropzone > .dz-preview .dz-remove {
  position: relative;
  z-index: 30;
  color: black;
  margin-left: 1px;
  padding: 10px;
  border: 2px black solid;
  text-decoration: none;
  text-transform: uppercase;
  font-size: 0.8rem;
  font-weight: 800;
  letter-spacing: 1.1px;
  opacity: 0;
  margin-top: 26px;
}

.vue-dropzone > .dz-preview .dz-image {
  border-radius: 0;
  width: 42%;
  height: 100%;
}

.dropzone .dz-preview.dz-image-preview {
  background: transparent;
}
.dropzone .dz-preview .dz-details {
  z-index: 20;
  position: absolute;
  left: 78px;
  display: block;
  color: black;
  opacity: 0;
  font-size: 13px;
  min-width: 100%;
  max-width: 100%;
  padding: 2em 1em;
  text-align: center;
  color: rgba(0, 0, 0, 0.9);
  line-height: 150%;
}
.vue-dropzone > .dz-preview .dz-details {
  bottom: 0;
  top: 0;
  color: black;
  background-color: transparent;
  transition: opacity 0.2s linear;
  /* text-align: left; */
}
.dropzone .dz-preview .dz-progress {
  opacity: 1;
  z-index: 1000;
  pointer-events: none;
  position: absolute;
  height: 40px;
  left: 34%;
  top: 24%;
  margin-top: -8px;
  width: 40px;
  margin-left: -40px;
  background: transparent;
  -webkit-transform: scale(1);
  border-radius: 50px;
  border: 3px solid lightgray;
  overflow: hidden;
}
.dropzone .dz-preview {
  position: relative;
  display: inline-block;
  vertical-align: top;
  margin: 10px 34px -3px;
  min-height: 100px;
}
</style>
