<template>
  <OnClickOutside @trigger="isExpanded = false">
    <aside>
      <div class="sm:hidden" :style="{ height: '57px' }"></div>
      <div
        class="fixed bottom-0 inset-x-0 border-t border-slate-700 sm:border-t-0 pwa:sm:border-t pwa:sm:border-t-slate-900 pwa:sm:shadow-[inset_0_1px_0_rgb(30_30_37)] sm:border-r content-start transition-[height] sm:transition-none sm:!h-screen sm:w-[240px] sm:static bg-slate-800 sm:overflow-auto"
        :style="{
          height: isExpanded ? `${expandableContentHeight + 57}px` : `57px`,
        }"
      >
        <div ref="expandableContent">
          <div class="grid gap-y-5 px-3 py-4">
            <div class="grid gap-y-2 justify-start">
              <label class="font-semibold text-xs">Theme</label>
              <div class="grid grid-flow-col items-center gap-x-2">
                <button
                  v-for="theme in themes"
                  @click="setTheme(theme)"
                  class="group rounded-full focus:outline-none"
                  :title="`Use ${theme.name} theme`"
                >
                  <div
                    class="w-6 h-6 rounded-full group-hover:opacity-100 transition group-hover:scale-105 group-active:scale-95 group-focus:shadow-[inset_0_0_0_1px_rgba(255,255,255,.21)] group-focus:ring-[3px] ring-blue-800"
                    :class="{
                      'opacity-50': store.currentTheme !== theme.key,
                    }"
                    :style="{ backgroundImage: theme.background }"
                  ></div>
                </button>
              </div>
            </div>

            <div class="grid gap-y-1">
              <label for="language" class="font-semibold text-xs">Language</label>
              <BaseSelect
                id="language"
                :model-value="store.language"
                @update:model-value="setLanguage"
                :options="AVAILABLE_LANGUAGES"
              />
            </div>

            <div class="grid gap-y-2">
              <div class="grid grid-flow-col gap-y-2 items-center grid-cols-[1fr_auto_auto] gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="showTwitterBadge"
                  >Twitter Badge</label
                >
                <BaseButton
                  class="text-blue-500 px-2.5 font-semibold text-xs bg-blue-600/30 hover:bg-blue-600/40 h-5 rounded"
                  @click="store.expandTwitterOptions = !store.expandTwitterOptions"
                >
                  <IconChevronDown
                    width="12"
                    class="transition-transform"
                    :class="{
                      'rotate-180': store.expandTwitterOptions,
                    }"
                  />
                </BaseButton>
                <BaseSwitch v-model="store.showTwitterBadge" id="showTwitterBadge" />
              </div>

              <div class="grid gap-y-1 gap-x-2 items-start grid-cols-[auto_1fr]" v-if="store.expandTwitterOptions">
                <div v-if="store.picture" class="row-start-1 row-end-3 relative group">
                  <BaseButton
                    @click="store.picture = ''"
                    class="h-5 w-5 absolute right-0 top-0 group-hover:opacity-100 opacity-0 bg-red-600/80 hover:bg-red-600 transition rounded-full justify-center text-white"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      xmlns:xlink="http://www.w3.org/1999/xlink"
                      aria-hidden="true"
                      role="img"
                      width="16"
                      height="16"
                      preserveAspectRatio="xMidYMid meet"
                      viewBox="0 0 20 20"
                    >
                      <g fill="currentColor">
                        <path
                          d="M7.172 14.243a1 1 0 1 1-1.415-1.415l7.071-7.07a1 1 0 0 1 1.415 1.414l-7.071 7.07Z"
                        ></path>
                        <path
                          d="M5.757 7.172a1 1 0 1 1 1.415-1.415l7.07 7.071a1 1 0 0 1-1.414 1.415l-7.07-7.071Z"
                        ></path>
                      </g>
                    </svg>
                  </BaseButton>
                  <img
                    :src="store.picture"
                    alt=""
                    class="w-14 h-14 border border-slate-700 bg-slate-700/30 rounded-full"
                  />
                </div>
                <label
                  v-else
                  class="w-14 h-14 border border-slate-700 bg-slate-700/30 hover:bg-slate-700/50 text-slate-600 hover:text-slate-400 transition-colors cursor-pointer rounded-full flex items-center justify-center row-start-1 row-end-3"
                >
                  <input type="file" class="sr-only" @change="handlePicture" />
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    xmlns:xlink="http://www.w3.org/1999/xlink"
                    aria-hidden="true"
                    role="img"
                    width="24"
                    height="24"
                    preserveAspectRatio="xMidYMid meet"
                    viewBox="0 0 256 256"
                  >
                    <path
                      fill="currentColor"
                      d="M172 56a4 4 0 0 1 4-4h20V32a4 4 0 0 1 8 0v20h20a4 4 0 0 1 0 8h-20v20a4 4 0 0 1-8 0V60h-20a4 4 0 0 1-4-4Zm54.8 56.2A104.1 104.1 0 0 1 228 128a99.6 99.6 0 0 1-32.7 73.9l-.8.8a99.9 99.9 0 0 1-132.9 0a3.6 3.6 0 0 1-.9-.8A100 100 0 0 1 128 28a104.1 104.1 0 0 1 15.8 1.2a4 4 0 0 1 3.3 4.6a4 4 0 0 1-4.6 3.3A100 100 0 0 0 128 36a92 92 0 0 0-65.2 156.9a75.8 75.8 0 0 1 44.5-34.1a44 44 0 1 1 41.4 0a75.8 75.8 0 0 1 44.5 34.1A92.1 92.1 0 0 0 220 128a99 99 0 0 0-1.1-14.5a4 4 0 0 1 3.3-4.6a4 4 0 0 1 4.6 3.3ZM128 156a36 36 0 1 0-36-36a36 36 0 0 0 36 36Zm0 64a91.3 91.3 0 0 0 59.1-21.6a68 68 0 0 0-118.2 0A91.3 91.3 0 0 0 128 220Z"
                    ></path>
                  </svg>
                </label>

                <div class="grid gap-y-1 col-start-2">
                  <BaseInput
                    class="placeholder:text-slate-600/75"
                    type="text"
                    id="name"
                    autocomplete="off"
                    spellcheck="false"
                    placeholder="Name"
                    v-model="store.name"
                  />
                </div>

                <div class="grid gap-y-1 col-start-2">
                  <BaseInput
                    class="placeholder:text-slate-600/75"
                    type="text"
                    id="username"
                    autocomplete="off"
                    spellcheck="false"
                    placeholder="Username"
                    v-model="store.username"
                  />
                </div>
              </div>

              <div class="grid grid-flow-col gap-y-2 items-center justify-between gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="showWindowControls"
                  >Window controls</label
                >
                <BaseSwitch v-model="store.showWindowControls" id="showWindowControls" />
              </div>

              <div class="grid grid-flow-col gap-y-2 items-center justify-between gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="showLineNumbers"
                  >Line numbers</label
                >
                <BaseSwitch v-model="store.showLineNumbers" id="showLineNumbers" />
              </div>

              <div class="grid grid-flow-col gap-y-2 items-center justify-between gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="showBackground">Background</label>
                <BaseSwitch v-model="store.showBackground" id="showBackground" />
              </div>

              <div class="grid grid-flow-col gap-y-2 items-center justify-between gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="reflection">Reflection</label>
                <BaseSwitch v-model="store.reflection" id="reflection" />
              </div>

              <div class="grid grid-flow-col gap-y-2 items-center justify-between gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="paddingX">Padding X</label>
                <div class="grid gap-x-2 grid-flow-col text-sm">
                  <input
                    id="paddingX"
                    class="accent-blue-700"
                    type="range"
                    min="32"
                    max="256"
                    step="8"
                    :value="store.paddingX"
                    @input="store.paddingX = parseInt(($event.target as HTMLInputElement).value)"
                  />
                </div>
              </div>

              <div class="grid grid-flow-col gap-y-2 items-center justify-between gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="paddingY">Padding Y</label>
                <div class="grid gap-x-2 grid-flow-col text-sm">
                  <input
                    id="paddingY"
                    class="accent-blue-700"
                    type="range"
                    min="32"
                    max="128"
                    step="8"
                    :value="store.paddingY"
                    @input="store.paddingY = parseInt(($event.target as HTMLInputElement).value)"
                  />
                </div>
              </div>

              <div class="grid grid-flow-col gap-y-2 items-center justify-between gap-x-2">
                <label class="font-semibold text-xs select-none cursor-pointer" for="diff">Diff</label>
                <BaseSwitch v-model="store.diff" id="diff" />
              </div>
            </div>
          </div>
        </div>

        <div
          class="grid grid-cols-[1fr_auto_1fr] sm:grid-cols-1 gap-2 fixed inset-x-0 bottom-0 py-2 px-3 bg-slate-800 sm:static sm:bg-transparent sm:px-3 sm:py-0"
        >
          <label class="font-semibold text-xs hidden sm:block">Export</label>
          <BaseButton
            class="px-4 w-full bg-blue-600/30 text-blue-500 hover:bg-blue-600/40 group"
            @click="handleCopyLink"
          >
            <IconClipboardLink width="16" class="group-hover:scale-110 transition-transform group-hover:rotate-6" />
            <span class="truncate">
              {{ exportState === ExportState.JustCopiedLink ? "Copied!" : "Copy Link to Clipboard" }}
            </span>
          </BaseButton>
          <BaseButton
            class="px-4 w-full hidden sm:flex bg-emerald-600/30 text-emerald-500 hover:bg-emerald-600/40 group"
            @click="handleCopy"
          >
            <IconClipboard width="16" class="group-hover:scale-110 transition-transform group-hover:rotate-6" />
            <span class="truncate">
              {{
                exportState === ExportState.PreparingToCopy
                  ? "..."
                  : exportState === ExportState.JustCopied
                  ? "Copied!"
                  : exportState === ExportState.CopyFailure
                  ? "Error! Try to download"
                  : "Copy Image to Clipboard"
              }}
            </span>
          </BaseButton>
          <BaseButton
            class="bg-slate-700 text-slate-500 hover:bg-slate-700/80 group sm:hidden w-10 justify-center"
            @click="isExpanded = !isExpanded"
            square="w-10"
          >
            <IconChevronDown
              width="16"
              class="transition"
              :class="{
                'rotate-180': !isExpanded,
              }"
            />
          </BaseButton>
          <BaseButton
            class="px-4 w-full bg-rose-500/30 text-rose-300 hover:bg-rose-500/40 group"
            @click="handleDownload"
          >
            <IconDownload width="16" class="group-hover:scale-110 transition-transform group-hover:rotate-6" />
            <span class="truncate">
              {{
                exportState === ExportState.PreparingToDownload
                  ? "..."
                  : exportState === ExportState.JustDownloaded
                  ? "Downloaded!"
                  : "Download PNG"
              }}
            </span>
          </BaseButton>
        </div>

        <div
          class="sm:grid grid-cols-[1fr_auto_1fr] sm:grid-cols-1 gap-2 bg-slate-800 hidden sm:static sm:bg-transparent sm:px-3 sm:py-0 mt-4"
        >
          <div class="grid grid-flow-col gap-y-2 items-center grid-cols-[1fr_auto] gap-x-2">
            <label
              class="font-semibold text-xs hidden sm:block"
              :class="{
                'opacity-0': !store.expandSupportSection,
              }"
              >Support</label
            >
            <BaseButton
              class="px-2.5 font-semibold text-xs hover:bg-blue-600/40 h-5 rounded"
              :class="{
                'bg-slate-700 ': !store.expandSupportSection,
                'bg-blue-600/30 text-blue-500': store.expandSupportSection,
              }"
              @click="store.expandSupportSection = !store.expandSupportSection"
            >
              <IconChevronDown
                width="12"
                class="transition-transform"
                :class="{
                  'rotate-180': store.expandSupportSection,
                }"
              />
            </BaseButton>
          </div>

          <div class="grid gap-y-2" v-if="store.expandSupportSection">
            <BaseButton
              is="a"
              class="px-4 w-full border border-[#1da1f2]/10 text-[#1da1f2]/90 hover:border-[#1da1f2]/40 group"
              href="https://twitter.com/intent/follow?screen_name=Idered"
              target="_blank"
            >
              <IconTwitter width="16" class="group-hover:scale-110 transition-transform group-hover:rotate-6" />
              <span class="truncate text-slate-600">Follow on Twitter</span>
            </BaseButton>
            <BaseButton
              is="a"
              class="px-4 w-full border border-yellow-500/10 text-yellow-300/80 hover:border-yellow-400/40 hover:text- group"
              href="https://www.buymeacoffee.com/idered"
              target="_blank"
            >
              <IconCoffee width="16" class="group-hover:scale-110 transition-transform group-hover:rotate-6" />
              <span class="truncate text-slate-600">Buy me a coffee</span>
            </BaseButton>
            <label
              class="font-semibold text-xs hidden sm:block mt-1"
              :class="{
                'opacity-0': !store.expandSupportSection,
              }"
              >Learn</label
            >
            <BaseButton
              is="a"
              class="px-4 w-full border border-slate-700 text-slate-600 hover:border-slate-600/40 group"
              href="https://github.com/Idered/chalk.ist"
              target="_blank"
            >
              <IconGithub width="16" class="group-hover:scale-110 transition-transform group-hover:rotate-6" />
              <span class="truncate">View on GitHub</span>
            </BaseButton>
            <BaseButton
              is="a"
              class="px-4 w-full border border-slate-700 text-slate-600 hover:border-slate-600/40 group"
              href="https://umami.kasper.io/share/WCDyKkOU/chalk.ist"
              target="_blank"
            >
              <IconAnalytics width="16" class="group-hover:scale-110 transition-transform group-hover:rotate-6" />
              <span class="truncate">View Analytics</span>
            </BaseButton>

            <div class="text-xs hidden sm:block mt-2">
              <span class="opacity-75">Created by</span>
              <a
                href="https://twitter.com/Idered"
                class="hover:text-white transition outline-none font-medium focus:text-white"
              >
                Kasper Mikiewicz
              </a>
            </div>
          </div>
        </div>
      </div>
    </aside>
  </OnClickOutside>
</template>

<script setup lang="ts">
import { nextTick, ref } from "vue";
import { OnClickOutside } from "@vueuse/components";
import { isExporting, store } from "~/composables/store";
import * as themes from "~/themes";
import BaseSwitch from "./BaseSwitch.vue";
import BaseSelect from "./BaseSelect.vue";
import { AVAILABLE_LANGUAGES } from "~/constants";
import BaseInput from "./BaseInput.vue";
import BaseButton from "./BaseButton.vue";
import IconDownload from "./IconDownload.vue";
import IconClipboard from "./IconClipboard.vue";
import IconChevronDown from "./IconChevronDown.vue";
import { useElementSize } from "@vueuse/core";
import { Theme } from "~/composables/theme-utils";
import { exportState, ExportState } from "~/composables/export-state";
import IconClipboardLink from "./IconClipboardLink.vue";
import { resizeImage, cropImage } from "~/composables/image";
import IconCoffee from "./IconCoffee.vue";
import IconTwitter from "./IconTwitter.vue";
import IconGithub from "./IconGithub.vue";
import IconAnalytics from "./IconAnalytics.vue";
import * as htmlToImage from "html-to-image";

const isExpanded = ref(false);
const timeout = ref();
const expandableContent = ref();
const { height: expandableContentHeight } = useElementSize(expandableContent);

// const fontEmbedCSS = ref("");

// onMounted(async () => {
//   const frame = document.querySelector<HTMLDivElement>("[data-editor-frame]");
//   if (!frame) return;
//   fontEmbedCSS.value = await htmlToImage.getFontEmbedCSS(frame);
// });

const downloadPng = (blob: Blob | null) => {
  if (!blob) return;
  const url = URL.createObjectURL(blob);
  const link = document.createElement("a");
  link.href = url;
  link.download = "screenshot.png";
  link.click();
  exportState.value = ExportState.JustDownloaded;
  clearTimeout(timeout.value);
  timeout.value = setTimeout(() => {
    exportState.value = ExportState.Idle;
  }, 1000);
};

const copyToClipboard = (blob: Blob | null) => {
  if (!blob) return;
  try {
    const item = new ClipboardItem({ "image/png": blob });
    navigator.clipboard.write([item]);
    exportState.value = ExportState.JustCopied;
    clearTimeout(timeout.value);
    timeout.value = setTimeout(() => {
      exportState.value = ExportState.Idle;
    }, 1000);
  } catch {
    exportState.value = ExportState.CopyFailure;
    clearTimeout(timeout.value);
    timeout.value = setTimeout(() => {
      exportState.value = ExportState.Idle;
    }, 1000);
  }
};

const handleCopy = async () => {
  const frame = document.querySelector<HTMLDivElement>("[data-editor-frame]");
  if (!frame) return;
  umami.trackEvent("Copy to Clipboard", "export");
  exportState.value = ExportState.PreparingToCopy;
  isExporting.value = true;
  await nextTick();
  htmlToImage
    .toBlob(frame, {
      // fontEmbedCSS: fontEmbedCSS.value,
    })
    .then(function (blob) {
      isExporting.value = false;
      copyToClipboard(blob);
    });
};

const handleCopyLink = async () => {
  const frame = document.querySelector<HTMLDivElement>("[data-editor-frame]");
  if (!frame) return;
  umami.trackEvent("Copy Link", "export");

  // copy location.href to clipboard
  const { content } = store.value;
  const str = window.btoa(
    JSON.stringify({
      c: encodeURIComponent(content),
      t: store.value.currentTheme,
      l: store.value.language,
      px: store.value.paddingX,
      py: store.value.paddingY,
      w: store.value.frameWidth,
      n: store.value.name,
      u: store.value.username,
      b: store.value.showTwitterBadge,
      r: store.value.reflection,
      ln: store.value.showLineNumbers,
      wc: store.value.showWindowControls,
    })
  );
  const url = `${window.location.origin}/share/${str}`;
  navigator.clipboard.writeText(url);

  exportState.value = ExportState.JustCopiedLink;
  clearTimeout(timeout.value);
  timeout.value = setTimeout(() => {
    exportState.value = ExportState.Idle;
  }, 1000);
};

const handleDownload = async () => {
  const frame = document.querySelector<HTMLDivElement>("[data-editor-frame]");
  if (!frame) return;
  umami.trackEvent("Download PNG", "export");
  exportState.value = ExportState.PreparingToDownload;
  isExporting.value = true;
  await nextTick();
  htmlToImage.toBlob(frame).then(function (blob) {
    isExporting.value = false;
    downloadPng(blob);
  });
};

function handlePicture(event: Event) {
  const target = event.target as HTMLInputElement;
  if (target.files) {
    const file = target.files[0];
    if (!file) return;
    const reader = new FileReader();
    reader.onload = async (e) => {
      const croppedImage = await cropImage(reader.result as string, 1);
      const resizedImage = await resizeImage(croppedImage, 56 * 2);
      store.value.picture = resizedImage.toDataURL(file.type);
    };
    reader.readAsDataURL(file);
  }
}

function setTheme(theme: Theme) {
  store.value.currentTheme = theme.key;
  umami.trackEvent(store.value.currentTheme, "theme");
}

function setLanguage(language: string) {
  store.value.language = language;
  umami.trackEvent(store.value.language, "language");
}
</script>
