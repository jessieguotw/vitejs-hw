<script setup>
import { ref, onMounted } from 'vue';

const linkTitle = ref('');
const linkUrl = ref('');
const links = ref([]);

//1. 新增收藏:輸入「網址」與「標題」，並且按下新增按鈕
const addLink = () => {
  if (linkTitle.value != '' && linkUrl.value != '') {
    const newlink = {
      id: crypto.randomUUID(),
      title: linkTitle.value,
      url: linkUrl.value,
    };

    links.value.unshift(newlink);
    localStorage.setItem('my-links', JSON.stringify(links.value));
    linkTitle.value = '';
    linkUrl.value = '';
  }
};

//3. 刪除單一收藏 filter-> id != 我點下去那一筆的id全都保留
const removeLink = (id) => {
  links.value = links.value.filter((link) => link.id !== id);
  localStorage.setItem('my-links', JSON.stringify(links.value));
};

//4. 清空全部
const clearUrl = () => {
  links.value = [];
  localStorage.removeItem('my-links');
};

//5. onMounted 執行存本地雲端
onMounted(() => {
  const storage = localStorage.getItem('my-links');
  if (storage != null) {
    const result = JSON.parse(storage);
    links.value = result;
  }
});

//
</script>

<template>
  <div class="bg-slate-200 min-h-screen py-10">
    <div class="max-w-3xl mx-auto p-6 mt-10 bg-white shadow-xl rounded-lg">
      <div>
        <div>
          <h1 class="text-3xl font-bold text-slate-700 mb-2">
            📂 Bookmark Manager
          </h1>
        </div>
        <p class="text-base font-bold text-slate-500 py-2">
          Enter The Website Info To Save
        </p>
        <div class="flex items-enter justify-between gap-2 mb-4">
          <input
            v-model="linkTitle"
            placeholder="Enter title"
            type="text"
            class="input flex-1 px-4 py-2 border border-slate-300 rounded focus:outline-none focus:ring-1 focus:ring-slate-400 text-slate-600 bg-white"
          />

          <input
            v-model="linkUrl"
            placeholder="Enter URL"
            type="text"
            class="input flex-1 px-4 py-2 border border-slate-300 rounded focus:outline-none focus:ring-1 focus:ring-slate-400 text-slate-600 bg-white"
          />

          <button
            @click="addLink"
            class="btn w-[100px] px-4 py-2 bg-slate-600 text-white rounded hover:bg-slate-700 transition shadow-lg border-0 cursor-pointer"
          >
            Add
          </button>
        </div>
        <hr />
      </div>

      <div>
        <!--- 2. 顯示清單，以表格呈現 -->
        <div
          class="url-table overflow-x-auto flex items-center justify-between py-2"
        >
          <h2 class="text-base font-bold text-slate-500">Bookmark List</h2>
          <button
            @click="clearUrl"
            class="w-[100px] text-s px-4 py-2 bg-red-100 text-red-600 rounded hover:bg-red-200 transition border-0 cursor-pointer"
          >
            Clear All
          </button>
        </div>
        <div class="rounded-md overflow-hidden border border-slate-300">
          <table class="min-w-full">
            <thead class="bg-slate-200">
              <tr>
                <th class="text-left text-slate-700 px-4 py-2 text-sm">
                  Title
                </th>
                <th class="text-left text-slate-700 px-4 py-2 text-sm">URL</th>
                <th
                  class="text-slate-700 px-4 py-2 w-[100px] text-center whitespace-nowrap text-sm"
                >
                  Delete
                </th>
              </tr>
            </thead>

            <tbody>
              <tr
                v-for="link in links"
                :key="link.id"
                class="border-t border-slate-200 hover:bg-slate-50 transition"
              >
                <td class="px-4 py-2 text-slate-600 font-medium">
                  <a :href="link.url" target="_blank" class="text-sm">
                    {{ link.title }}</a
                  >
                </td>
                <td class="px-4 py-2 text-sm text-slate-600 break-all">
                  <a :href="link.url" target="_blank" class="text-sm">
                    🔗 {{ link.url }}
                  </a>
                </td>
                <td class="px-4 py-2 w-[100px] text-center whitespace-nowrap">
                  <button
                    @click="removeLink(link.id)"
                    class="p-2 rounded hover-red-100 transition bg-red-100 hover:bg-red-200 cursor-pointer"
                  >
                    <svg
                      xmlns="http://www.w3.org/2000/svg"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke-Width="1.5"
                      stroke="currentColor"
                      c
                      class="w-5 h-5 text-red-500 hover:text-red-700 transition"
                    >
                      <path
                        strokeLinecap="round"
                        strokeLinejoin="round"
                        d="m14.74 9-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 0 1-2.244 2.077H8.084a2.25 2.25 0 0 1-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 0 0-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 0 1 3.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 0 0-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 0 0-7.5 0"
                      />
                    </svg>
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>
