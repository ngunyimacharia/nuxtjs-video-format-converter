<template>
  <div class="bg-white p-16">
    <form class="space-y-8 divide-y divide-gray-200" @submit.prevent="submit">
      <div class="space-y-8 divide-y divide-gray-200 sm:space-y-5">
        <div>
          <div>
            <h3 class="text-lg leading-6 font-medium text-gray-900">
              Video format converter
            </h3>
            <p class="mt-1 max-w-2xl text-sm text-gray-500">
              Convert videos from one format to another using this utility.
            </p>
          </div>

          <div class="mt-6 sm:mt-5 space-y-6 sm:space-y-5">
            <div
              class="
                sm:grid sm:grid-cols-3
                sm:gap-4
                sm:items-start
                sm:border-t sm:border-gray-200
                sm:pt-5
              "
            >
              <label
                for="video"
                class="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2"
              >
                Video
              </label>
              <div class="mt-1 sm:mt-0 sm:col-span-2">
                <input
                  type="file"
                  name="video"
                  id="video"
                  accept="video/*"
                  v-on:change="handleFile"
                  required
                  class="
                    flex-1
                    block
                    w-full
                    focus:ring-indigo-500
                    focus:border-indigo-500
                    min-w-0
                    rounded-none rounded-r-md
                    sm:text-sm
                    border-gray-300
                  "
                />
              </div>
            </div>

            <div
              class="
                sm:grid sm:grid-cols-3
                sm:gap-4
                sm:items-start
                sm:border-t sm:border-gray-200
                sm:pt-5
              "
            >
              <label
                for="format"
                class="block text-sm font-medium text-gray-700 sm:mt-px sm:pt-2"
              >
                Format
              </label>
              <div class="mt-1 sm:mt-0 sm:col-span-2">
                <select
                  id="format"
                  name="format"
                  v-model="format"
                  required
                  class="
                    max-w-lg
                    shadow-sm
                    block
                    w-full
                    focus:ring-indigo-500
                    focus:border-indigo-500
                    sm:text-sm
                    border border-gray-300
                    rounded-md
                  "
                >
                  <option
                    v-for="format in formats"
                    :key="format.value"
                    :value="format.value"
                  >
                    {{ format.text }}
                  </option>
                </select>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="pt-5">
        <p class="flex justify-end text-sm text-gray-600" v-if="uploading">
          Uploading...
        </p>
        <div v-else class="flex justify-end">
          <button
            type="reset"
            class="
              bg-white
              py-2
              px-4
              border border-gray-300
              rounded-md
              shadow-sm
              text-sm
              font-medium
              text-gray-700
              hover:bg-gray-50
              focus:outline-none
              focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500
            "
          >
            Reset
          </button>
          <button
            type="submit"
            class="
              ml-3
              inline-flex
              justify-center
              py-2
              px-4
              border border-transparent
              shadow-sm
              text-sm
              font-medium
              rounded-md
              text-white
              bg-indigo-600
              hover:bg-indigo-700
              focus:outline-none
              focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500
            "
          >
            Save
          </button>
        </div>
      </div>
    </form>
    <div v-if="convertedUrl" class="text-center mt-10">
      <h1 class="text-xl font-bold">Video convertion complete</h1>
      <p>
        Here is a preview of your converted video. Use the button below to
        download
      </p>
      <cld-video
        class="w-1/3 m-auto my-5"
        controls="true"
        :public-id="cloudinaryVideo.public_id"
        :format="format"
      >
      </cld-video>
      <a
        target="_blank"
        :href="convertedUrl"
        class="
          m-auto
          inline-flex
          items-center
          px-3
          py-2
          border border-transparent
          text-sm
          leading-4
          font-medium
          rounded-md
          text-indigo-700
          bg-indigo-100
          hover:bg-indigo-200
          focus:outline-none
          focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500
        "
      >
        Download
      </a>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      uploadVideo: null,
      cloudinaryVideo: null,
      uploading: false,
      format: null,
      convertedUrl: null,
      formats: [
        { text: "FLV (Flash Video)", value: "flv" },
        { text: "HLS adaptive streaming", value: "m3u8" },
        { text: "MPEG-2 Transport Stream (ts)", value: "ts" },
        { text: "MPEG-2 Transport Stream (m2ts)", value: "m2ts" },
        { text: "MPEG-2 Transport Stream (mts)", value: "mts" },
        { text: "MOV", value: "mov" },
        { text: "MKV (Matroska Multimedia Container)", value: "mkv" },
        { text: "MP4", value: "mp4" },
        { text: "MPEG-DASH adaptive streaming	", value: "mpd" },
        { text: "OGV (Ogg Video)", value: "ogv" },
        { text: "WebM", value: "webm" },
      ],
    };
  },
  methods: {
    async handleFile(e) {
      this.uploadVideo = e.target.files[0];
      console.log(this.uploadVideo);
    },
    async readData(f) {
      return new Promise((resolve) => {
        const reader = new FileReader();
        reader.onloadend = () => resolve(reader.result);
        reader.readAsDataURL(f);
      });
    },
    async submit() {
      this.uploading = true;
      const videoData = await this.readData(this.uploadVideo);
      this.cloudinaryVideo = await this.$cloudinary.upload(videoData, {
        upload_preset: "default-preset",
        folder: "nuxtjs-video-format-converter",
      });
      this.uploading = false;
      console.log(this.cloudinaryVideo);
      this.convertedUrl = this.$cloudinary.video.url(
        this.cloudinaryVideo.public_id,
        { format: this.format }
      );
      console.log(this.convertedUrl, this.format);
    },
  },
};
</script>
