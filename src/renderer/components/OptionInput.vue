<template>
  <div class="flex flex-col md:flex-row py-2">
    <div class="w-64">
      <label class="text-sm text-gray-900 dark:text-white" :for="field.name">{{ cleanName }}</label>
    </div>
    <div class="flex flex-col mt-2" v-if="field.accept_value == true">
      <input type="text" class="input-text" spellcheck="false" :id="field.name" :value="field.value" @input="$emit('input', $event.target.value)" />
      <label class="text-xs text-gray-600 mt-1 dark:text-white" :for="field.name">{{ field.description }}</label>
    </div>
    <div class="flex flex-row items-center mt-2" v-else>
      <input type="checkbox" class="input-checkbox" :id="field.name" :checked="field.value" @change="$emit('input', $event.target.checked)" />
      <label class="ml-2 text-sm text-gray-600 dark:text-white" :for="field.name">{{ field.description }}</label>
    </div>
  </div>
</template>

<script>
export default {
  name: "OptionInput",
  props: ["field"],
  computed: {
    cleanName() {
      return this.field.name.substring(2);
    }
  }
};
</script>

<style></style>
