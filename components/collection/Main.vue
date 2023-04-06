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

      <div v-if="!templates || !templates.length">
        <CollectionEmptyState />
      </div>
      <div v-else>
        <CollectionList
          :templates="templates"
          @edit-type="editType"
          @delete-template="deleteTemplate"
          :key="renderList"
        />
      </div>
    </div>
    <div class="row-span-3 col-span-4 bg-white h-[calc(100vh-150px)]">
      <CollectionAdd v-if="type == 'add'" @add-template="addTemplate" />
      <CollectionEdit
        v-else
        :template="template"
        @edit-template="editTemplate"
        :key="render"
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
const render = ref(0);
const renderList = ref(0);
console.log("this.url", props.url);

const { data: items } = await useAuthLazyFetch(props.url, {});
templates.value = items.value;

function editType(data: object) {
  console.log("this.data", data);
  type.value = "edit";
  template.value = data;
  render.value++;
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
  console.log("this.add", data, props.editUrl);
  let body = data;
  await useAuthLazyFetchPut(`${props.editUrl}${data.uid}`, {
    body: JSON.stringify(body),
  });

  const { data: response } = await useAuthLazyFetch(props.url, {});
  templates.value = response.value;
  console.log("this.template", templates.value);
  renderList.value++;
}

async function deleteTemplate(uid: string) {
  console.log("this.add", uid, props.editUrl);
  await useAuthLazyFetchDelete(`${props.deleteUrl}${uid}`, {});

  const { data: response } = await useAuthLazyFetch(props.url, {});
  templates.value = response.value;
  console.log("this.template", templates.value);
  renderList.value++;
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
