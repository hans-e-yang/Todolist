<script setup>
// imports
const settings = useSettings();
const firestore = useFirestore();

// Template
const backgroundColorPallete = ref(settings.getColorPallete("colorPallete"));
const textColorPallete = ref(settings.getColorPallete("textColorPallete"));


const backgroundColorSelection = ref("#000000");
const textColorSelection = ref("#000000");

// Save functionality
const isSaving = ref(false);
function saveSettings() {
  isSaving.value = true;
  firestore.saveSettings()
    .then(()=>{
      isSaving.value = false;
    })
}

function setDefaultSettings() {
    const res = confirm("Are you sure you want to set the color settings to defaults?")
    if (res) {
        settings.setDefault()
    }
}
</script>

<template>
    <NuxtLayout>
        <div class="flex flex-col gap-16 pt-20 sm:flex-row ">
            <!-- Background Color Column -->
            <div class="flex flex-col items-start gap-1">
                <label >
                    Pick a Color
                    <input type="color" class="border border-gray-800" v-model="backgroundColorSelection">
                </label>
                <button class="my-2 button-small" @click="settings.addColor(backgroundColorSelection, 'colorPallete')">Add Color</button>

                <h2 class="text-3xl font-bold">Colors</h2>
                <div v-for="backgroundColor, i in settings.getColorPallete('colorPallete')" class="flex items-center gap-2 p-1 border border-black">
                    <div :style="{ backgroundColor }" class="w-4 h-4 border border-black"></div>
                    {{ backgroundColor }}
                    <button v-if="(backgroundColorPallete.length > 1)" class="hover:rotate-12 focus:rotate-12" @click="settings.deleteColor(i, 'colorPallete')">
                        <svg xmlns="http://www.w3.org/2000/svg" width="1.5em" height="1.5em"
                            preserveAspectRatio="xMidYMid meet" viewBox="0 0 24 24">
                            <path fill="currentColor"
                                d="M6 19c0 1.1.9 2 2 2h8c1.1 0 2-.9 2-2V7H6v12zM19 4h-3.5l-1-1h-5l-1 1H5v2h14V4z" />
                        </svg>
                    </button>
                </div>
            </div>

            <!-- Text Color Column -->
            <div class="flex flex-col items-start gap-1">
                <label >
                    Pick a Color
                    <input type="color" class="border border-gray-800" v-model="textColorSelection">
                </label>
                <button class="my-2 button-small" @click="settings.addColor(textColorSelection, 'textColorPallete')">Add Text Color</button>

                <h2 class="text-3xl font-bold">Text Colors</h2>
                <div v-for="color in settings.getColorPallete('textColorPallete')" class="flex items-center gap-2 p-1 border border-black">
                    <div :style="{ backgroundColor: color }" class="w-4 h-4 border border-black"></div>
                    {{ color }}
                    <button v-if="(textColorPallete.length > 1)" class="hover:rotate-12 focus:rotate-12" @click="settings.deleteColor(i, 'textColorPallete')">
                        <svg xmlns="http://www.w3.org/2000/svg" width="1.5em" height="1.5em"
                            preserveAspectRatio="xMidYMid meet" viewBox="0 0 24 24">
                            <path fill="currentColor"
                                d="M6 19c0 1.1.9 2 2 2h8c1.1 0 2-.9 2-2V7H6v12zM19 4h-3.5l-1-1h-5l-1 1H5v2h14V4z" />
                        </svg>
                    </button>
                </div>
            </div>

            <!-- Results Column -->
            <div class="flex flex-col items-center">
                <h2 class="my-4 text-3xl font-bold">Result</h2>
                <div v-for="i in settings.longestLength" :style="{
                    backgroundColor: settings.getColor(i-1),
                    color: settings.getTextColor(i-1)
                }" class="p-1 px-2">
                    Test  
                </div>
            </div>

            <div class="flex flex-col gap-5 mb-12">
                <button class="h-10 button" @click="setDefaultSettings">Set to default</button>
                <button class="h-10 button" @click="saveSettings">Save Settings</button>
                <div v-if="isSaving" class="flex gap-2 ml-8">
                    <svg class="w-5 h-5 mr-3 -ml-1 animate-spin text-primary" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                        <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                        <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                    </svg>
                    Saving
                </div>
            </div>
        </div>

    </NuxtLayout>
</template>

<style>

</style>