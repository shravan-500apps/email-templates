<template>
  <div
    class="grid grid-rows-3 grid-flow-col grid-cols-4 border w-[80vw] mx-auto my-5 rounded-lg pr-[4px]"
  >
    <div
      class="row-span-3 bg-gray-50 border-r p-5 rounded-l-lg h-[calc(100vh-150px)] overflow-auto"
    >
      <div class="pb-3 w-[100%]">
        <!-- Adding template to the list -->
        <button
          class="bg-white hover:bg-gray-50 hover:text-gray-800 border focus-visible:outline focus-visible:outline-2 focus-visible:outline-indigo-600 focus-visible:outline-offset-2 font-semibold inline-flex items-center justify-center px-3 py-3 rounded-md shadow-sm text-gray-600 text-sm w-[100%]"
          @click="type = 'add'"
        >
          <span>
            <IconCSS name="material-symbols:add" class="mr-2" size="20" />
          </span>
          Add Template
        </button>
      </div>
      <div>
        <CollectionList :templates="templates" @edit-type="editType" />
      </div>
      <div v-if="!templates || !templates.length">
        <CollectionEmptyState />
      </div>
    </div>
    <div class="row-span-3 col-span-4 bg-white h-[calc(100vh-150px)]">
      <CollectionAdd v-if="type == 'add'" @add-template="addTemplate" />
      <CollectionEdit
        v-else
        :template="template"
        @edit-template="editTemplate"
      />
    </div>
  </div>
</template>
<script setup lang="ts">
// Import vue
import { ref } from "vue";
const props = defineProps({
  // Get Template Mail Data
  url: { type: String, default: "" },
  addUrl: { type: String, default: "" },
  editUrl: { type: String, default: "" },
  deleteUrl: { type: String, default: "" },
});

const templates = ref<object[]>([]);
const template = ref({
  name: "Edit",
  subject: "Editsubject",
  body: "Editbody",
});
const type = ref("add");
console.log("this.url", props.url);
const { data: items } = await useAuthLazyFetch(props.url, {});
templates.value = items.value;

function editType(data: object) {
    console.log("this.data",data)
  type.value = "edit";
  template.value = data;
}

async function addTemplate(data: object) {
  console.log("this.add", data, props.addUrl);
  let body = {
    project_id: "1",
    name: data.name,
    subject: data.subject,
    body: data.body,
    is_active: "1",
    type: "HTML",
    share_type: "PRIVATE",
    category: "string",
  };
  const { data: items, pending } = await useAuthLazyFetchPost(props.addUrl, {
    body: JSON.stringify(body),
  });
  console.log("this.data", items, pending);
  templates.value.unshift(data);
}

async function editTemplate(data: object) {
  console.log("this.add", data, props.addUrl);
  let body = data;
  await useAuthLazyFetchPut(props.editUrl, {
    body: JSON.stringify(body),
  });

  templates.value = (await useAuthLazyFetch(`${props.editUrl}daab5e98-ef45-4e00-93b7-2aee057de10f`, {})).data;
}

// // Add Template to the template data
// const addTemplate = (data: any) => {
//   templates.value.push({
//     name: templateName.value,
//     text: data,
//     subject: templateSubject.value,
//   });
//   templateName.value = "";
//   templateBody.value = "";
//   templateSubject.value = "";
// };
</script>
