<template>
  <div class="flex flex-row flex-1">
    <div class="bg-gray-100 w-72 flex flex-col">
      <h1 class="uppercase font-bold text-gray-500 px-5 pt-5 pb-4">Artisan</h1>
      <div class="px-3 pb-3">
        <input type="text" v-model="searchKeyword" class="w-full h-7 border-input border-gray-300 border rounded placeholder-gray-800 px-2.5 text-sm hover:border-gray-400 focus:outline-none focus:border-gray-500" placeholder="Search" />
      </div>
      <div class="overflow-scroll overflow-x-hidden flex-1 flex flex-col pb-4">
        <div class="flex flex-col">
          <span v-for="command in searchResults" :key="command.name" @click="commandName = command.name" :class="{ 'bg-gray-300': commandName == command.name }" class="py-1.5 hover:bg-gray-200 text-xs font-mono px-6" :alt="command.description">{{ command.name }}</span>
        </div>
      </div>
    </div>
    <div class="bg-gray-100 flex flex-col flex-1">
      <div class="flex justify-between items-center">
        <h1 class="font-bold px-5 py-4 text-gray-500">{{ name }}</h1>
        <div class="mr-2">
          <a class="underline hover:text-blue mr-2 cursor-pointer" v-if="serve != null" @click="openServe">http://127.0.0.1:8000</a>
          <button class="bg-green hover:bg-green-100 w-29 py-2 text-white rounded text-xs mx-1 focus:outline-none focus:ring-2 focus:ring-green" @click="serveService">{{ serve == null ? "Serve" : "Serving..." }}</button>
          <button class="bg-blue hover:bg-blue-100 w-29 py-2 text-white rounded text-xs mx-1 focus:outline-none focus:ring-2" @click="openFolder">Open folder</button>
          <button class="bg-blue hover:bg-blue-100 w-29 py-2 text-white rounded text-xs mx-1 focus:outline-none focus:ring-2">Open In Editor</button>
        </div>
      </div>
      <command v-if="commandName != null" :name="commandName" :key="commandName" class="view"></command>
      <artisan-default v-else class="view"></artisan-default>
    </div>
  </div>
</template>

<script>
import Command from "@/components/Command.vue";
import ArtisanDefault from "@/components/ArtisanDefault.vue";
import { mapState, mapActions } from "vuex";
import { remote } from "electron";
const { showItemInFolder, openExternal } = remote.shell;

export default {
  name: "Artisan",
  components: { Command, ArtisanDefault },
  data() {
    return {
      searchKeyword: "",
      commandName: null
    };
  },
  computed: {
    ...mapState(["name", "serve"]),
    searchResults() {
      const remove = ["serve", "tinker"];
      return this.$store.state.project.commands.filter(command => command.name.includes(this.searchKeyword) && !remove.includes(command.name)).sort((a, b) => (a.name > b.name ? 1 : -1));
    }
  },
  methods: {
    openFolder() {
      showItemInFolder(this.$store.state.dir);
    },
    openServe() {
      openExternal("http://127.0.0.1:8000");
    },
    ...mapActions(["serveService"])
  }
};
</script>

<style scoped>
.view {
  @apply bg-white-100 flex-1 overflow-y-auto pl-7 pr-2 py-5 text-xl;
}
</style>