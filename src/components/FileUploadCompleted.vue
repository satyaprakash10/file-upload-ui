<template>
  <div>
    <vue-dropzone
      @vdropzone-file-added="vfileAdded"
      @vdropzone-success="vsuccess"
      @vdropzone-error="verror"
      @vdropzone-removed-file="vremoved"
      @vdropzone-sending="vsending"
      @vdropzone-success-multiple="vsuccessMuliple"
      @vdropzone-sending-multiple="vsendingMuliple"
      @vdropzone-queue-complete="vqueueComplete"
      @vdropzone-total-upload-progress="vprogress"
      @vdropzone-mounted="vmounted"
      @vdropzone-drop="vddrop"
      @vdropzone-drag-start="vdstart"
      @vdropzone-drag-end="vdend"
      @vdropzone-drag-enter="vdenter"
      @vdropzone-drag-over="vdover"
      @vdropzone-drag-leave="vdleave"
      @vdropzone-duplicate-file="vdduplicate"
      class="bg-gray-100 cursor-pointer rounded-md shadow-sm"
      :options="dropzoneOptions"
      :duplicateCheck="true"
    >
    </vue-dropzone>
  </div>
</template>

<script>
import vueDropzone from 'vue2-dropzone'
import 'vue2-dropzone/dist/vue2Dropzone.min.css'
export default {
  name: 'FileUpload',

  components: {
    vueDropzone,
  },

  props: {
    autoprocessqueue: {
      type: Boolean,
      default: true,
    },
  },

  data: function() {
    return {
      dropzoneOptions: {
        url: 'https://httpbin.org/post',
        thumbnailWidth: 150,
        maxFilesize: 0.5,
        headers: { 'My-Awesome-Header': 'header value' },
        addRemoveLinks: true,
        fileAdded: false,
        filesAdded: false,
        success: false,
        error: false,
        removedFile: false,
        sending: false,
        successMultiple: false,
        sendingMultiple: false,
        queueComplete: false,
        uploadProgress: false,
        progress: false,
        myProgress: 0,
        isMounted: false,
        dDrop: false,
        dStarted: false,
        dEnded: false,
        dEntered: false,
        dOver: false,
        dLeave: false,
        dDuplicate: false,
      },
    }
  },

  methods: {
    vfileAdded() {
      this.fileAdded = true
    },
    vfilesAdded() {
      this.filesAdded = true
    },
    vsuccess() {
      this.success = true
    },
    verror() {
      this.error = true
    },
    vremoved() {
      this.removedFile = true
    },
    vsending() {
      this.sending = true
    },
    vsuccessMuliple() {
      this.successMultiple = true
    },
    vsendingMuliple() {
      this.sendingMultiple = true
    },
    vqueueComplete() {
      this.queueComplete = true
    },
    vprogress(totalProgress) {
      this.progress = true
      this.myProgress = Math.floor(totalProgress)
    },

    watch: {
      fileAdded() {
        let that = this
        setTimeout(function() {
          that.fileAdded = false
        }, 2000)
      },
      filesAdded() {
        let that = this
        setTimeout(function() {
          that.filesAdded = false
        }, 2000)
      },
      success() {
        let that = this
        setTimeout(function() {
          that.success = false
        }, 2000)
      },
      error() {
        let that = this
        setTimeout(function() {
          that.error = false
        }, 2000)
      },
      removedFile() {
        let that = this
        setTimeout(function() {
          that.removedFile = false
        }, 2000)
      },
      sending() {
        let that = this
        setTimeout(function() {
          that.sending = false
        }, 2000)
      },
      successMultiple() {
        let that = this
        setTimeout(function() {
          that.successMultiple = false
        }, 2000)
      },
      sendingMultiple() {
        let that = this
        setTimeout(function() {
          that.sendingMultiple = false
        }, 2000)
      },
      queueComplete() {
        let that = this
        setTimeout(function() {
          that.queueComplete = false
        }, 2000)
      },
      progress() {
        let that = this
        setTimeout(function() {
          that.progress = false
        }, 2000)
      },
      isMounted() {
        let that = this
        setTimeout(function() {
          that.isMounted = false
        }, 2000)
      },
      dDrop() {
        let that = this
        setTimeout(function() {
          that.dDrop = false
        }, 2000)
      },
      dStarted() {
        let that = this
        setTimeout(function() {
          that.dStarted = false
        }, 2000)
      },
      dEnded() {
        let that = this
        setTimeout(function() {
          that.dEnded = false
        }, 2000)
      },
      dEntered() {
        let that = this
        setTimeout(function() {
          that.dEntered = false
        }, 2000)
      },
      dOver() {
        let that = this
        setTimeout(function() {
          that.dOver = false
        }, 2000)
      },
      dLeave() {
        let that = this
        setTimeout(function() {
          that.dLeave = false
        }, 2000)
      },
      dDuplicate() {
        let that = this
        setTimeout(function() {
          that.dDuplicate = false
        }, 2000)
      },
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="css">
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
</style>
