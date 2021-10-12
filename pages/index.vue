<template>
  <div class="bg-white p-16">
    <form class="space-y-8 divide-y divide-gray-200">
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
                  rows="3"
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
        <div class="flex justify-end">
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
  </div>
</template>

<script>
export default {
  data() {
    return {
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
};
</script>
