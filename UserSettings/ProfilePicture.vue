<template>
  <div class="grid grid-cols-12 gap-6">
    <div class="col-span-12">
      <form @submit.prevent="saveChanges" class="intro-y box p-5" method="POST" enctype="multipart/form-data">
        <div class="flex items-end ">
          <div class="mt-4 input-form" :class="{ 'has-error': isInvalid('file') }">
            <!-- BEGIN: Dropzone -->
            <div class="dropzone rounded-lg border-slate-200 w-[300px] h-[300px]" id="my-profile-picture">
              <div class="fallback">
                <input name="file" type="file" />
              </div>
              <div class="dz-message mt-[70px!important]">
                <div class="text-lg font-medium">Drop files here or click to upload.</div>
                <div class="text-gray-600">
                  Files will not be automatically uploaded.
                </div>
              </div>
            </div>
            <!-- END: Dropzone -->
            <div v-if="isInvalid('file')" class="error-message mt-1">{{ errorMessage('file') }}</div>
          </div>

          <div class="flex items-end mt-5 ml-5">
            <button type="submit" :disabled="loading" class="btn btn-primary w-24"
              :class="{ 'pointer-events-none': loading }">
              <LoadingSpinner v-if="loading"></LoadingSpinner>
              <span v-else>Update</span>
            </button>
            <button type="button" @click="selectFile" :disabled="loading" class="btn btn-secondary ml-5"
              :class="{ 'pointer-events-none': loading }">
              <span>Selec file</span>
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import LoadingSpinner from "@/Components/LoadingSpinner.vue";
import _ from "lodash";
import serverValidationErrorsMixin from "../mixins/serverValidationErrorsMixin";
import Dropzone from "dropzone";

export default {
  name: "ChangePassword",
  mixins: [serverValidationErrorsMixin],
  components: {
    LoadingSpinner,
  },
  props: ['profile_picture'],
  emits: ['set-profile-picuture'],
  data() {
    return {
      loading: false,
      formData: {},
      dropzone: null,
      dropzoneOptions: {
        thumbnailWidth: 260,
        thumbnailHeight: 260,
        maxFiles: 1,
        maxFilesize: 20, // mb
        parallelUploads: 1, // 1 was set
        paramName: "file",
        addRemoveLinks: true,
        uploadMultiple: false,
        url: '/', // Just a placeholder, needed because DZ asks for it, but we process the upload manually.
        autoProcessQueue: false,
        createImageThumbnails: true,
        accept: function (file, done) {
          const allowedMimeTypes = [
            "image/jpeg",
            "image/png",
            "image/bmp",
            "image/gif",
            "image/jpg",
          ];

          if (!allowedMimeTypes.includes(file.type)) {
            done("Error! Files of this type are not accepted");
          } else {
            done();
          }
        }
      },

    }
  },

  mounted() {
    console.log('this.profile_picture', this.profile_picture)
    this.initDropzone()
  },

  unmounted() {
    this.dropzone.destroy();
  },

  methods: {
    initDropzone() {
      Dropzone.autoDiscover = false;

      this.dropzone = new Dropzone('#my-profile-picture', this.dropzoneOptions);
      if (this.profile_picture) {
        const mockFile = { name: "existing", size: 1244 };

        const thumbPath = "/" + this.profile_picture.replace('public', 'storage')
        this.dropzone.displayExistingFile(mockFile, thumbPath)
        this.dropzone.files.push(mockFile);
      }

      const vm = this;

      this.dropzone.on("error", (error) => {
        console.log(error);
      });

      this.dropzone.on("addedfile", file => {
        vm.formData.file = file;
        if (vm.dropzone.files.length > 1) {
          vm.dropzone.removeAllFiles(true);
          vm.dropzone.addFile(file);
        }
      });

      this.dropzone.on("removedfile", file => {
        vm.formData.file = null;
      });

      this.dropzone.on("maxfilesexceeded", function (file) {
        vm.dropzone.removeAllFiles(true);
        vm.dropzone.addFile(file);
      });
    },
    selectFile() {
      this.dropzone.hiddenFileInput.click()

    },
    saveChanges() {
      if (this.loading) return;

      if (this.formData.file) {
        this.loading = true;
        const data = new FormData();

        data.append('file', this.formData.file);

        setTimeout(() => {
          axios
            .post(`/user-settings/upload-profile-picture`, data)
            .then((response) => {
              this.formData.file = null
              this.errors = {};
              this.$emit('set-profile-picuture', response.data.file);

              $notify.success({
                title: 'Success',
                message: 'Submitted successfully',
                showClose: true,
              })
            })
            .catch((error) => {
              this.errors = error.response.data.errors;
            })
            .finally(() => {
              this.loading = false;
            })
        }, 500)
      }

    },

    showNotification(message = null, title = 'Success') {
      let messageContent = message ? message : 'Submitted successfully';
      $notify.success({
        title: title,
        message: messageContent,
        showClose: true,
      })
    },
  },
}
</script>

<style>
#my-profile-picture {
  padding: 0 !important;
}

#my-profile-picture .dz-progress {
  display: none;
}

#my-profile-picture .dz-remove {
  display: none;
}

#my-profile-picture .dz-details {
  display: none;
}

#my-profile-picture .dz-preview .dz-image {
  width: 260px !important;
  height: 260px !important;
}

#my-profile-picture .dz-preview .dz-image img {
  width: 100% !important;
  height: 100% !important;
}

#my-profile-picture .dz-preview:hover .dz-image img {
  transform: none;
  filter: none;
}
</style>