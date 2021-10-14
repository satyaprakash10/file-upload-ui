<template>
  <div class="">
    <!--file Upload start stop button  -->
    <div class="upload-button cursor-pointer flex space-x-6">
      <div
        class="bg-blue-500 rounded-md shadow-lg hover:bg-opacity-75 transition-all duration-300  cursor-pointer px-4 py-2 text-white flex items-center"
      >
        <file-upload
          class="cursor-pointer"
          :post-action="postAction"
          :put-action="putAction"
          :extensions="extensions"
          :accept="accept"
          :multiple="multiple"
          :directory="directory"
          :create-directory="createDirectory"
          :size="size || 0"
          :thread="thread < 1 ? 1 : thread > 5 ? 5 : thread"
          :headers="headers"
          :data="data"
          :drop="drop"
          :drop-directory="dropDirectory"
          :add-index="addIndex"
          v-model="files"
          @input-filter="inputFilter"
          @input-file="inputFile"
          ref="upload"
        >
          Select
          <i class="fa ml-2 fa-plus"></i>
        </file-upload>
      </div>
      <button
        type="button"
        class="cursor-pointer  hover:bg-opacity-75 transition-all duration-300  bg-green-500 rounded-md shadow-lg px-4 py-2 text-white"
        v-if="!$refs.upload || !$refs.upload.active"
        @click.prevent="$refs.upload.active = true"
      >
        <i class="fa fa-arrow-up" aria-hidden="true"></i>
        Start Upload
      </button>
      <button
        type="button"
        class="btn btn-danger"
        v-else
        @click.prevent="$refs.upload.active = false"
      >
        <i class="fa fa-stop" aria-hidden="true"></i>
        Stop Upload
      </button>
    </div>
    <!-- Initial file Uploading section -->
    <div class="upload" v-show="!isOption && isUploading">
      <div>
        <table class="w-full mt-4 px-6 rounded-md">
          <tbody class="">
            <tr v-if="!files.length">
              <td colspan="9">
                <div
                  class="bg-gray-100 hover:border cursor-pointer hover:shadow-sm  transition-all duration-300 hover:scale-x-2 transform hover:border-gray-600 hover:bg-gray-200 py-4 px-6 rounded-md shadow-sm text-center"
                >
                  <h4>Drag & Drop files anywhere to upload<br />or</h4>
                  <label
                    :for="name"
                    class="transition duration-500 ease-in-out bg-blue-400 cursor-pointer hover:bg-blue-500 px-4 hover:text-white py-3 rounded-md shadow-sm hover:bg-opacity-35 transform hover:-translate-y-1 hover:scale-90"
                  >
                    Choose Files
                  </label>
                </div>
              </td>
            </tr>

            <!-- file Uploading section  -->
            <div
              class="flex items-center justify-between mt-6"
              v-if="files.length"
            >
              <h1 class="font-medium text-base sm:text-xl text-gray-900">
                Uploading
              </h1>

              <a
                @click.prevent="
                  files.active
                    ? $refs.upload.update(files, { error: 'cancel' })
                    : false
                "
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
            <div
              class="border mt-2 mb-4 border-gray-200 max-w-7xl mx-auto"
              v-if="files.length"
            ></div>

            <tr v-for="file in files" :key="file.id" class>
              <div
                class="bg-gray-100 flex borde-gray-200 px-6 py-4 justify-between shadow-sm w-full rounded-md"
              >
                <div class="flex items-center space-x-6">
                  <td class="">
                    <img
                      class="td-image-thumb"
                      v-if="file.thumb"
                      :src="file.thumb"
                    />
                    <span v-else>No Image</span>
                  </td>

                  <!-- File name and size  -->
                  <td>
                    <div class="text-gray-600 font-normal text-base tuncate">
                      {{ file.name }}
                    </div>
                    <div class="text-gray-400 font-normal text-sm mt-2">
                      {{ file.size }} MB
                    </div>
                  </td>
                </div>

                <div>
                  <!-- X-Icon to CANCEL FILE UPLOADING -->
                  <div class="mt-4 ">
                    <a
                      class="relative"
                      :class="{ 'dropdown-item': true, disabled: !file.active }"
                      href="#"
                      @click.prevent="$refs.upload.remove(file)"
                    >
                      <XIcon
                        class="-top-6 absolute cursor-pointer disabled h-4 hover:text-blue-500 right-0 w-8 z-10"
                      />
                    </a>
                  </div>

                  <!-- file progress bar status  -->
                  <div
                    class="progress"
                    v-if="file.active || file.progress !== '0.00'"
                  >
                    <div
                      :class="{
                        'progress-bar': true,
                        'progress-bar-striped': true,
                        'bg-danger': file.error,
                        'progress-bar-animated': file.active,
                      }"
                      role="progressbar"
                      :style="{ width: file.progress + '%' }"
                    >
                      {{ file.progress }}%
                    </div>
                  </div>

                  <div class="mt-2">
                    <a
                      class="dropdown-item"
                      href="#"
                      v-if="
                        file.error &&
                          file.error !== 'compressing' &&
                          file.error !== 'image parsing' &&
                          $refs.upload.features.html5
                      "
                      @click.prevent="
                        $refs.upload.update(file, {
                          active: true,
                          error: '',
                          progress: '0.00',
                        })
                      "
                    >
                      Retry upload
                    </a>
                    <a
                      :class="{
                        'dropdown-item': true,
                        disabled:
                          file.success ||
                          file.error === 'compressing' ||
                          file.error === 'image parsing',
                      }"
                      href="#"
                      v-else
                      @click.prevent="
                        file.success ||
                        file.error === 'compressing' ||
                        file.error === 'image parsing'
                          ? false
                          : $refs.upload.update(file, { active: true })
                      "
                    >
                      Upload
                    </a>
                  </div>
                </div>
              </div>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Next Up Upload section  -->
    <div class="flex items-center justify-between mt-16">
      <h1 class="font-medium text-base sm:text-xl text-gray-900">
        Next Up
      </h1>

      <button
        @click="isNextUp = !isNextUp"
        class="font-normal focus:outline-none hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-blue-400"
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

    <!-- border -->
    <div class="border mt-2 mb-4 border-gray-200 max-w-7xl mx-auto"></div>

    <!-- Waiting upload component  -->
    <div class="upload" v-show="!isOption && isNextUp">
      <div>
        <table class="w-full mt-4 px-6 rounded-md">
          <tbody class="">
            <!-- <tr v-if="!files.length">
              <td colspan="9">
                <div
                  class="bg-gray-100 hover:border cursor-pointer hover:shadow-sm  transition-all duration-300 hover:scale-x-2 transform hover:border-gray-600 hover:bg-gray-200 py-4 px-6 rounded-md shadow-sm text-center"
                >
                  <h4>Drag & Drop files anywhere to upload<br />or</h4>
                  <label
                    :for="name"
                    class="transition duration-500 ease-in-out bg-blue-400 cursor-pointer hover:bg-blue-500 px-4 hover:text-white py-3 rounded-md shadow-sm hover:bg-opacity-35 transform hover:-translate-y-1 hover:scale-90"
                  >
                    Choose Files
                  </label>
                </div>
              </td>
            </tr> -->

            <tr v-for="file in files" :key="file.id">
              <div
                class="bg-gray-100 flex borde-gray-200 px-6 py-4 justify-between shadow-sm w-full rounded-md"
              >
                <div class="flex items-center space-x-6">
                  <td class="">
                    <img
                      class="td-image-thumb"
                      v-if="file.thumb"
                      :src="file.thumb"
                    />
                    <span v-else>No Image</span>
                  </td>

                  <!-- File name and size  -->
                  <td>
                    <div class="text-gray-600 font-normal text-base tuncate">
                      {{ file.name }}
                    </div>
                    <div class="text-gray-400 font-normal text-sm mt-2">
                      {{ file.size }} MB
                    </div>
                  </td>
                </div>

                <div>
                  <!-- X-Icon to CANCEL FILE UPLOADING -->
                  <div class="mt-4 ">
                    <a
                      class="relative"
                      :class="{ 'dropdown-item': true, disabled: !file.active }"
                      href="#"
                      @click.prevent="$refs.upload.remove(file)"
                    >
                      <XIcon
                        class="-top-6 absolute cursor-pointer disabled h-4 hover:text-blue-500 right-0 w-8 z-10"
                      />
                    </a>
                  </div>

                  <!-- file progress bar status  -->
                  <div
                    class="progress"
                    v-if="file.active || file.progress !== '0.00'"
                  >
                    <div
                      :class="{
                        'progress-bar': true,
                        'progress-bar-striped': true,
                        'bg-danger': file.error,
                        'progress-bar-animated': file.active,
                      }"
                      role="progressbar"
                      :style="{ width: file.progress + '%' }"
                    >
                      {{ file.progress }}%
                    </div>
                  </div>
                  <td v-else-if="file.active">waiting</td>

                  <a
                    :class="{
                      'dropdown-item': true,
                      disabled:
                        file.success ||
                        file.error === 'compressing' ||
                        file.error === 'image parsing',
                    }"
                    href="#"
                    v-else
                    @click.prevent="
                      file.success ||
                      file.error === 'compressing' ||
                      file.error === 'image parsing'
                        ? false
                        : $refs.upload.update(file, { active: true })
                    "
                  >
                    Upload
                  </a>
                  <a
                    class="dropdown-item"
                    href="#"
                    v-if="file.active"
                    @click.prevent="
                      $refs.upload.update(file, { active: false })
                    "
                    >Abort</a
                  >
                  <a
                    class="dropdown-item"
                    href="#"
                    v-else-if="
                      file.error &&
                        file.error !== 'compressing' &&
                        file.error !== 'image parsing' &&
                        $refs.upload.features.html5
                    "
                    @click.prevent="
                      $refs.upload.update(file, {
                        active: true,
                        error: '',
                        progress: '0.00',
                      })
                    "
                    >Retry upload</a
                  >
                </div>
              </div>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <!-- Completed Upload section  -->
    <div class="flex items-center justify-between mt-16">
      <h1 class="font-medium text-base sm:text-xl text-gray-900">
        Completed
      </h1>

      <button
        @click="isUploadComplete = !isUploadComplete"
        class="font-normal focus:outline-none hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-blue-400"
      >
        <ChevronUpArrowIcon
          class="mr-5 ml-4 text-gray-900 "
          style="40px"
          v-if="!isUploadComplete"
        />
        <ChevronDownArrowIcon
          class="mr-5 ml-4 text-gray-900 "
          style="40px"
          v-else
        />
      </button>
    </div>

    <!-- border  -->
    <div class="border mt-2 mb-4 border-gray-200 max-w-7xl mx-auto"></div>

    <!-- Completed upload component  -->
    <div class="upload" v-show="!isOption && isUploadComplete">
      <div>
        <table class="w-full mt-4 px-6 rounded-md">
          <tbody class="">
            <!-- <tr v-if="!files.length">
              <td colspan="9">
                <div
                  class="bg-gray-100 hover:border cursor-pointer hover:shadow-sm  transition-all duration-300 hover:scale-x-2 transform hover:border-gray-600 hover:bg-gray-200 py-4 px-6 rounded-md shadow-sm text-center"
                >
                  <h4>Drag & Drop files anywhere to upload<br />or</h4>
                  <label
                    :for="name"
                    class="transition duration-500 ease-in-out bg-blue-400 cursor-pointer hover:bg-blue-500 px-4 hover:text-white py-3 rounded-md shadow-sm hover:bg-opacity-35 transform hover:-translate-y-1 hover:scale-90"
                  >
                    Choose Files
                  </label>
                </div>
              </td>
            </tr> -->

            <tr v-for="file in files" :key="file.id">
              <div
                class="bg-gray-100 flex borde-gray-200 px-6 py-4 justify-between shadow-sm w-full rounded-md"
              >
                <div class="flex items-center space-x-6">
                  <td class="">
                    <img
                      class="td-image-thumb"
                      v-if="file.thumb"
                      :src="file.thumb"
                    />
                    <span v-else>No Image</span>
                  </td>

                  <!-- File name and size  -->
                  <td>
                    <div class="text-gray-600 font-normal text-base tuncate">
                      {{ file.name }}
                    </div>
                    <div class="text-gray-400 font-normal text-sm mt-2">
                      {{ file.size }} MB
                    </div>
                  </td>
                </div>

                <div>
                  <!-- X-Icon to CANCEL FILE UPLOADING -->
                  <div class="mt-4 ">
                    <a
                      class="relative"
                      :class="{ 'dropdown-item': true, disabled: !file.active }"
                      href="#"
                      @click.prevent="$refs.upload.remove(file)"
                    >
                      <XIcon
                        class="-top-6 absolute cursor-pointer disabled h-4 hover:text-blue-500 right-0 w-8 z-10"
                      />
                    </a>
                  </div>

                  <!-- file progress bar status  -->
                  <div
                    class="progress"
                    v-if="file.active || file.progress !== '0.00'"
                  >
                    <div
                      :class="{
                        'progress-bar': true,
                        'progress-bar-striped': true,
                        'bg-danger': file.error,
                        'progress-bar-animated': file.active,
                      }"
                      role="progressbar"
                      :style="{ width: file.progress + '%' }"
                    >
                      {{ file.progress }}%
                    </div>
                  </div>
                  <td v-else-if="file.success">success</td>

                  <a
                    :class="{
                      'dropdown-item': true,
                      disabled:
                        file.success ||
                        file.error === 'compressing' ||
                        file.error === 'image parsing',
                    }"
                    href="#"
                    v-else
                    @click.prevent="
                      file.success ||
                      file.error === 'compressing' ||
                      file.error === 'image parsing'
                        ? false
                        : $refs.upload.update(file, { active: true })
                    "
                  >
                    Upload
                  </a>
                  <a
                    class="dropdown-item"
                    href="#"
                    v-if="file.active"
                    @click.prevent="
                      $refs.upload.update(file, { active: false })
                    "
                    >Abort</a
                  >
                  <a
                    class="dropdown-item"
                    href="#"
                    v-else-if="
                      file.error &&
                        file.error !== 'compressing' &&
                        file.error !== 'image parsing' &&
                        $refs.upload.features.html5
                    "
                    @click.prevent="
                      $refs.upload.update(file, {
                        active: true,
                        error: '',
                        progress: '0.00',
                      })
                    "
                    >Retry upload</a
                  >
                </div>
              </div>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <div class="mt-4 font-normal text-gray-400 text-left text-sm" v-if="!files">
      No file uploads completed yet.
    </div>

    <!-- InComplete Upload section  -->
    <div class="flex items-center justify-between mt-16">
      <h1 class="font-medium text-base sm:text-xl text-gray-900">
        Incomplete Uploads
      </h1>

      <button
        @click="isUploadIncomplete = !isUploadIncomplete"
        class="font-normal focus:outline-none hover:underline cursor-pointer hover:text-blue-500 hover:font-medium uppercase flex items-center text-base sm:text-base text-blue-400"
      >
        <ChevronUpArrowIcon
          class="mr-5 ml-4 text-gray-900 "
          style="40px"
          v-if="!isUploadIncomplete"
        />
        <ChevronDownArrowIcon
          class="mr-5 ml-4 text-gray-900 "
          style="40px"
          v-else
        />
      </button>
    </div>

    <!-- border  -->
    <div class="border mt-2 mb-4 border-gray-200 max-w-7xl mx-auto"></div>
    <!-- Incomplete upload component  -->
    <div class="upload" v-show="!isOption && isUploadIncomplete">
      <div>
        <table class="w-full mt-4 px-6 rounded-md">
          <tbody class="">
            <!-- <tr v-if="!files.length">
              <td colspan="9">
                <div
                  class="bg-gray-100 hover:border cursor-pointer hover:shadow-sm  transition-all duration-300 hover:scale-x-2 transform hover:border-gray-600 hover:bg-gray-200 py-4 px-6 rounded-md shadow-sm text-center"
                >
                  <h4>Drag & Drop files anywhere to upload<br />or</h4>
                  <label
                    :for="name"
                    class="transition duration-500 ease-in-out bg-blue-400 cursor-pointer hover:bg-blue-500 px-4 hover:text-white py-3 rounded-md shadow-sm hover:bg-opacity-35 transform hover:-translate-y-1 hover:scale-90"
                  >
                    Choose Files
                  </label>
                </div>
              </td>
            </tr> -->

            <tr v-for="file in files" :key="file.id">
              <div
                class="bg-gray-100 flex borde-gray-200 px-6 py-4 justify-between shadow-sm w-full rounded-md"
              >
                <div class="flex items-center space-x-6">
                  <td class="">
                    <img
                      class="td-image-thumb"
                      v-if="file.thumb"
                      :src="file.thumb"
                    />
                    <span v-else>No Image</span>
                  </td>

                  <!-- File name and size  -->
                  <td>
                    <div class="text-gray-600 font-normal text-base tuncate">
                      {{ file.name }}
                    </div>
                    <div class="text-gray-400 font-normal text-sm mt-2">
                      {{ file.size }} MB
                    </div>
                  </td>
                </div>

                <div>
                  <!-- X-Icon to CANCEL FILE UPLOADING -->
                  <div class="mt-4 ">
                    <a
                      class="relative"
                      :class="{ 'dropdown-item': true, disabled: !file.active }"
                      href="#"
                      @click.prevent="$refs.upload.remove(file)"
                    >
                      <XIcon
                        class="-top-6 absolute cursor-pointer disabled h-4 hover:text-blue-500 right-0 w-8 z-10"
                      />
                    </a>
                  </div>

                  <!-- file progress bar status  -->
                  <div
                    class="progress"
                    v-if="file.active || file.progress !== '0.00'"
                  >
                    <div
                      :class="{
                        'progress-bar': true,
                        'progress-bar-striped': true,
                        'bg-danger': file.error,
                        'progress-bar-animated': file.active,
                      }"
                      role="progressbar"
                      :style="{ width: file.progress + '%' }"
                    >
                      {{ file.progress }}%
                    </div>
                  </div>
                  <td v-else-if="file.error">error</td>

                  <a
                    :class="{
                      'dropdown-item': true,
                      disabled:
                        file.success ||
                        file.error === 'compressing' ||
                        file.error === 'image parsing',
                    }"
                    href="#"
                    v-else
                    @click.prevent="
                      file.success ||
                      file.error === 'compressing' ||
                      file.error === 'image parsing'
                        ? false
                        : $refs.upload.update(file, { active: true })
                    "
                  >
                    Upload
                  </a>
                  <a
                    class="dropdown-item"
                    href="#"
                    v-if="file.active"
                    @click.prevent="
                      $refs.upload.update(file, { active: false })
                    "
                    >Abort</a
                  >
                  <a
                    class="dropdown-item"
                    href="#"
                    v-else-if="
                      file.error &&
                        file.error !== 'compressing' &&
                        file.error !== 'image parsing' &&
                        $refs.upload.features.html5
                    "
                    @click.prevent="
                      $refs.upload.update(file, {
                        active: true,
                        error: '',
                        progress: '0.00',
                      })
                    "
                    >Retry upload</a
                  >
                </div>
              </div>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <div class="mt-4 font-normal text-gray-400 text-left text-sm" v-if="!files">
      No incomplete upoads yet.
    </div>
  </div>
</template>

<script>
import XIcon from './icons/XIcon.vue'
import Cropper from 'cropperjs'
import ImageCompressor from '@xkeshi/image-compressor'
import FileUpload from 'vue-upload-component'
import ChevronUpArrowIcon from './icons/ChevronUpArrowIcon.vue'
import ChevronDownArrowIcon from './icons/ChevronDownArrowIcon.vue'
export default {
  components: {
    FileUpload,
    XIcon,
    ChevronUpArrowIcon,
    ChevronDownArrowIcon,
  },

  data() {
    return {
      files: [],
      isUploading: true,
      isNextUp: false,
      isUploadComplete: false,
      isUploadIncomplete: false,
      accept: 'image/png,image/gif,image/jpeg,image/webp',
      // extensions: 'gif,jpg,jpeg,png,webp',
      extensions: ['gif', 'jpg', 'jpeg', 'png', 'webp'],
      // extensions: /\.(gif|jpe?g|png|webp)$/i,
      minSize: 1024,
      size: 1024 * 1024 * 10,
      multiple: true,
      directory: false,
      drop: true,
      dropDirectory: true,
      createDirectory: false,
      addIndex: false,
      thread: 3,
      name: 'file',
      postAction: '/upload/post',
      putAction: '/upload/put',
      headers: {
        'X-Csrf-Token': 'xxxx',
      },
      data: {
        _csrf_token: 'xxxxxx',
      },

      autoCompress: 1024 * 1024,
      uploadAuto: false,
      isOption: false,

      addData: {
        show: false,
        name: '',
        type: '',
        content: '',
      },

      editFile: {
        show: false,
        name: '',
      },
    }
  },

  watch: {
    'editFile.show'(newValue, oldValue) {
      //error
      if (!newValue && oldValue) {
        this.$refs.upload.update(this.editFile.id, {
          error: this.editFile.error || '',
        })
      }

      if (newValue) {
        this.$nextTick(() => {
          if (!this.$refs.editImage) {
            return
          }
          let cropper = new Cropper(this.$refs.editImage, {
            autoCrop: false,
          })
          this.editFile = {
            ...this.editFile,
            cropper,
          }
        })
      }
    },

    'addData.show'(show) {
      if (show) {
        this.addData.name = ''
        this.addData.type = ''
        this.addData.content = ''
      }
    },
  },

  methods: {
    inputFilter(newFile, oldFile, prevent) {
      if (newFile && !oldFile) {
        // Before adding a file

        // Filter system files or hide files
        if (/(\/|^)(Thumbs\.db|desktop\.ini|\..+)$/.test(newFile.name)) {
          return prevent()
        }

        // Filter php html js file
        if (/\.(php5?|html?|jsx?)$/i.test(newFile.name)) {
          return prevent()
        }

        // Automatic compression
        if (
          newFile.file &&
          newFile.error === '' &&
          newFile.type.substr(0, 6) === 'image/' &&
          this.autoCompress > 0 &&
          this.autoCompress < newFile.size
        ) {
          newFile.error = 'compressing'
          const imageCompressor = new ImageCompressor(null, {
            convertSize: 1024 * 1024,
            maxWidth: 512,
            maxHeight: 512,
          })
          imageCompressor
            .compress(newFile.file)
            .then((file) => {
              this.$refs.upload.update(newFile, {
                error: '',
                file,
                size: file.size,
                type: file.type,
              })
            })
            .catch((err) => {
              this.$refs.upload.update(newFile, {
                error: err.message || 'compress',
              })
            })
        }
      }

      if (
        newFile &&
        newFile.error === '' &&
        newFile.file &&
        (!oldFile || newFile.file !== oldFile.file)
      ) {
        // Create a blob field
        newFile.blob = ''
        let URL = window.URL || window.webkitURL
        if (URL) {
          newFile.blob = URL.createObjectURL(newFile.file)
        }

        // Thumbnails
        newFile.thumb = ''
        if (newFile.blob && newFile.type.substr(0, 6) === 'image/') {
          newFile.thumb = newFile.blob
        }
      }

      // image size
      if (
        newFile &&
        newFile.error === '' &&
        newFile.type.substr(0, 6) === 'image/' &&
        newFile.blob &&
        (!oldFile || newFile.blob !== oldFile.blob)
      ) {
        newFile.error = 'image parsing'
        let img = new Image()
        img.onload = () => {
          this.$refs.upload.update(newFile, {
            error: '',
            height: img.height,
            width: img.width,
          })
        }
        img.οnerrοr = () => {
          this.$refs.upload.update(newFile, { error: 'parsing image size' })
        }
        img.src = newFile.blob
      }
    },

    // add, update, remove File Event
    inputFile(newFile, oldFile) {
      if (newFile && oldFile) {
        // update

        if (newFile.active && !oldFile.active) {
          // beforeSend

          // min size
          if (
            newFile.size >= 0 &&
            this.minSize > 0 &&
            newFile.size < this.minSize
          ) {
            this.$refs.upload.update(newFile, { error: 'size' })
          }
        }

        if (newFile.progress !== oldFile.progress) {
          // progress
        }

        if (newFile.error && !oldFile.error) {
          // error
        }

        if (newFile.success && !oldFile.success) {
          // success
        }
      }

      if (!newFile && oldFile) {
        // remove
        if (oldFile.success && oldFile.response.id) {
          // $.ajax({
          //   type: 'DELETE',
          //   url: '/upload/delete?id=' + oldFile.response.id,
          // })
        }
      }

      // Automatically activate upload
      if (
        Boolean(newFile) !== Boolean(oldFile) ||
        oldFile.error !== newFile.error
      ) {
        if (this.uploadAuto && !this.$refs.upload.active) {
          this.$refs.upload.active = true
        }
      }
    },

    alert(message) {
      alert(message)
    },
  },
}
</script>

<style>
.example-full .btn-group .dropdown-menu {
  display: block;
  visibility: hidden;
  transition: all 0.2s;
}
.example-full .btn-group:hover > .dropdown-menu {
  visibility: visible;
  cursor: pointer;
}

.example-full label.dropdown-item {
  margin-bottom: 0;
}

.example-full .btn-group .dropdown-toggle {
  margin-right: 0.6rem;
}

.td-image-thumb {
  max-width: 4em;
  max-height: 4em;
}

.example-full .filename {
  margin-bottom: 0.3rem;
}

.example-full .btn-is-option {
  margin-top: 0.25rem;
}
.example-full .upload-button {
  padding: 0.5rem 0;
  border-top: 1px solid #e9ecef;
  border-bottom: 1px solid #e9ecef;
}

.example-full .edit-image img {
  max-width: 100%;
}

.example-full .edit-image-tool {
  margin-top: 0.6rem;
}

.example-full .edit-image-tool .btn-group {
  margin-right: 0.6rem;
}

.example-full .footer-status {
  padding-top: 0.4rem;
}

.example-full .drop-active {
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  position: fixed;
  z-index: 9999;
  opacity: 0.6;
  text-align: center;
  background: #000;
}

.example-full .drop-active h3 {
  margin: -0.5em 0 0;
  position: absolute;
  top: 50%;
  left: 0;
  right: 0;
  -webkit-transform: translateY(-50%);
  -ms-transform: translateY(-50%);
  transform: translateY(-50%);
  font-size: 40px;
  color: #fff;
  padding: 0;
}
</style>
