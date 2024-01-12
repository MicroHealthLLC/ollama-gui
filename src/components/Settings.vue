<script setup lang="ts">
import { IconLayoutSidebarRightCollapse } from '@tabler/icons-vue'
import {
  baseUrl,
  debugMode,
  gravatarEmail,
  isSettingsOpen,
  toggleSettingsPanel,
} from '../services/appConfig.ts'

import useFileList from './file-list'
import DropZone from './Inputs/DropZone.vue'
import  FilePreview  from  './Inputs/FilePreview.vue'

const { files, addFiles, removeFile } = useFileList()

function onInputChange(e) {
    addFiles(e.target.files)
    e.target.value = null
}

import  createUploader  from  './Inputs/file-uploader'
const { uploadFiles } = createUploader('')
</script>

<template>
  <aside>
    <div
      class="relative h-screen w-60 overflow-y-auto border-l border-zinc-300 bg-zinc-50 py-4 sm:w-64 dark:border-zinc-700 dark:bg-zinc-900"
    >
      <div class="mb-4 flex items-center gap-x-2 px-2 text-zinc-800 dark:text-zinc-200">
        <button
          @click="toggleSettingsPanel()"
          class="inline-flex rounded-lg p-1 hover:bg-zinc-200 hover:dark:bg-zinc-700"
        >
          <IconLayoutSidebarRightCollapse class="h-6 w-6" />

          <span class="sr-only">Close settings sidebar</span>
        </button>
        <h2 class="text-lg font-medium">Documents</h2>
      </div>


              <DropZone class="drop-area" @files-dropped="addFiles" #default="{ dropZoneActive }">
                <label for="file-input">
                  <span v-if="dropZoneActive">
                      <span>Drop Them Here</span>
                      <span class="smaller">to add them</span>
                  </span>
                  <span v-else>
                      <span>Drag Your Files Here</span>
                      <span class="smaller">
                          or <strong><em>click here</em></strong> to select files
                      </span>
                  </span>

                  <input type="file" id="file-input" multiple @change="onInputChange" />
                </label>
                <ul class="image-list" v-show="files.length">
                    <FilePreview  v-for="file  of  files" :key="file.id" :file="file"  tag="li" @remove="removeFile" />
                </ul>
                <button @click.prevent="uploadFiles(files)"  class="upload-button">Upload</button>
              </DropZone>
    </div>
  </aside>
</template>
