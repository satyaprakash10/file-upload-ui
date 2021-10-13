<template>
  <div class="mt-32 relative mb-10 h-full max-w-7xl mx-auto w-full px-8">
    <!-- file upload section-->
    <div>
      <div class="flex items-center justify-between">
        <h1 class="font-medium text-base sm:text-xl text-gray-900">
          Uploading
        </h1>

        <button
          @click="isUploading = !isUploading"
          class="font-normal hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-indigo-400"
        >
          Cancel Upload

          <ChevronUpArrowIcon
            class="mr-5 ml-4 text-gray-900 "
            style="40px"
            v-if="!isUploading"
          />
          <ChevronDownArrowIcon
            class="mr-5 ml-4 text-gray-900 "
            style="40px"
            v-else
          />
        </button>
      </div>

      <!-- border  -->
      <div
        class="border mt-4 mb-4 border-gray-200 max-w-7xl mx-auto w-full"
      ></div>

      <transition name="fade" mode="out-in">
        <div class="py-4 mt-4 relative" v-show="isUploading">
          <XIcon
            class="absolute top-8 w-4 h-4 right-4 cursor-pointer  hover:text-blue-500 z-10"
          />
          <file-upload class="" />
        </div>
      </transition>
    </div>

    <!-- Next up section  -->
    <div class="mt-6 sm:mt-12">
      <div class="flex items-center justify-between">
        <h1 class="font-medium text-base sm:text-xl text-gray-900">
          Next Up
        </h1>

        <button
          @click="isNextUp = !isNextUp"
          class="font-normal hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-indigo-400"
        >
          Cancel all

          <ChevronUpArrowIcon
            class="mr-5 ml-4 text-gray-900 "
            style="40px"
            v-if="!isNextUp"
          />
          <ChevronDownArrowIcon
            class="mr-5 ml-4 text-gray-900 "
            style="40px"
            v-else
          />
        </button>
      </div>

      <!-- border  -->
      <div
        class="border mt-4 mb-4 border-gray-200 max-w-7xl mx-auto w-full"
      ></div>

      <transition
        enter-active-class="transition ease-out duration-100"
        enter-from-class="transform opacity-0 scale-95"
        enter-to-class="transform opacity-100 scale-100"
        leave-active-class="transition ease-in duration-75"
        leave-from-class="transform opacity-100 scale-100"
        leave-to-class="transform opacity-0 scale-95"
      >
        <div class=" py-4" v-show="isNextUp">
          <div class="relative">
            <div class="absolute left-6 top-16">
              <div class="flex space-x-4 items-center">
                <FileIcon class="w-12 h-12" />
                <span class="mt-4 font-normal text-gray-600 text-base mb-4">
                  File Name
                  <br />
                  <span class="mt-4 text-gray-400 font-normal text-sm"
                    >12 Mb</span
                  >
                </span>
              </div>
            </div>
            <div>
              <XIcon
                class="absolute top-10 w-3 h-3 right-3 sm:w-4 sm:h-4 sm:top-10 cursor-pointer  hover:text-blue-500 z-10"
              />
            </div>
            <div
              class="absolute right-6 sm:right-10 z-10 top-20 sm:top-16 cursor-pointer"
            >
              <LoadingIcon class="sm:w-8 ml-2 sm:ml-1 sm:h-8 w-6 h-6" />
              <label
                class="text-xs sm:text-base font-normal text-gray-400 mt-2"
              >
                Waiting
              </label>
            </div>
          </div>
          <NextUpFileUpload class="mt-6" />
        </div>
      </transition>
    </div>

    <!-- Completed upload section  -->
    <div class="mt-6 sm:mt-12">
      <div class="flex items-center justify-between">
        <h1 class="font-medium text-base sm:text-xl text-gray-900">
          Completed
        </h1>

        <button
          @click="isCompleted = !isCompleted"
          class="font-normal cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-indigo-400"
        >
          <ChevronUpArrowIcon
            class="mr-5 ml-4 text-gray-900 "
            style="40px"
            v-if="!isCompleted"
          />
          <ChevronDownArrowIcon
            class="mr-5 ml-4 text-gray-900 "
            style="40px"
            v-else
          />
        </button>
      </div>

      <!-- border  -->
      <div
        class="border mt-4 mb-4 border-gray-200 max-w-7xl mx-auto w-full"
      ></div>

      <transition name="fade" mode="out-in">
        <div class=" py-4" v-show="isCompleted">
          <div class="relative">
            <div class="absolute left-6 top-16">
              <div class="flex space-x-4 items-center">
                <FileIcon class="w-12 h-12" />
                <span class="mt-4 font-normal text-gray-600 text-base mb-4">
                  File Name
                  <br />
                  <span class="mt-4 text-gray-400 font-normal text-sm"
                    >12 Mb</span
                  >
                </span>
              </div>
            </div>
            <div>
              <XIcon
                class="absolute top-10 w-3 h-3 right-3 sm:w-4 sm:h-4 sm:top-10 cursor-pointer  hover:text-blue-500 z-10"
              />
            </div>
            <div class="absolute right-6 z-10 top-20 sm:top-20 cursor-pointer">
              <CheckMarkIcon class="sm:w-6 ml-1 sm:ml-2 sm:h-6 w-6 h-6" />
              <label class="text-xs sm:text-base font-normal text-gray-400 mt-2"
                >Done</label
              >
            </div>
          </div>
          <FileUploadCompleted class="mt-6" />
        </div>

        <label
          v-if="!isNextUp"
          class="font-normal flex justify-start text-gray-400 z-10 text-base mt-8"
        >
          No incomplete upoads yet.
        </label>
      </transition>
    </div>

    <!-- Incomplete download section  -->
    <div class="mt-6 sm:mt-12">
      <div class="flex items-center justify-between">
        <h1 class="font-medium text-base sm:text-xl text-gray-900">
          Incomplete Uploads
        </h1>

        <button
          @click="isIncomplete = !isIncomplete"
          class="font-normal cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-indigo-400"
        >
          <ChevronUpArrowIcon
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
      <div
        class="border mt-4 mb-4 border-gray-200 max-w-7xl mx-auto w-full"
      ></div>

      <transition name="fade" mode="out-in">
        <div class=" py-4" v-show="isIncomplete">
          <div class="flex items-center justify-between">
            <label
              class="font-normal hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-indigo-400"
            >
              RETRY ALL
            </label>
            <label
              class="font-normal hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-indigo-400"
            >
              DISMISS All
            </label>
          </div>

          <div class="relative">
            <div class="absolute left-6 top-16">
              <div class="flex space-x-4 items-center">
                <FileIcon class="w-12 h-12" />
                <span class="mt-4 font-normal text-gray-600 text-base mb-4">
                  File Name
                  <br />
                  <span class="mt-4 text-gray-400 font-normal text-sm"
                    >12 Mb</span
                  >
                </span>
              </div>
            </div>
            <div>
              <XIcon
                class="absolute top-10 w-3 h-3 right-3 sm:w-4 sm:h-4 sm:top-10 cursor-pointer  hover:text-blue-500 z-10"
              />
            </div>
            <div class="absolute right-6 z-10 top-20 sm:top-20 cursor-pointer">
              <RetryIcon class="sm:w-6 ml-1 sm:ml-3 sm:h-6 w-6 h-6">
              </RetryIcon>
              <label
                class="text-xs sm:text-base font-normal text-gray-400 mt-2"
              >
                Failed
              </label>
            </div>
          </div>
          <incomplete-file-upload class="mt-6" />
        </div>

        <label
          v-if="!isIncomplete"
          class="font-normal flex justify-start text-gray-400 z-10 text-base mt-8"
        >
          No incomplete upoads yet.
        </label>
      </transition>
    </div>
  </div>
</template>

<script>
import ChevronUpArrowIcon from './icons/ChevronUpArrowIcon.vue'
import ChevronDownArrowIcon from './icons/ChevronDownArrowIcon.vue'
import FileUpload from './FileUpload.vue'
import FileUploadCompleted from './FileUploadCompleted.vue'
import LoadingIcon from './icons/LoadingIcon.vue'
import XIcon from './icons/XIcon.vue'
import FileIcon from './icons/FileIcon.vue'
import RetryIcon from './icons/RetryIcon.vue'
import CheckMarkIcon from './icons/CheckMarkIcon.vue'
import IncompleteFileUpload from './IncompleteFileUpload.vue'
import NextUpFileUpload from './NextUpFileUpload.vue'

export default {
  components: {
    FileUpload,
    ChevronUpArrowIcon,
    ChevronDownArrowIcon,
    FileUploadCompleted,
    XIcon,
    RetryIcon,
    FileIcon,
    IncompleteFileUpload,
    CheckMarkIcon,
    NextUpFileUpload,
    LoadingIcon,
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
