<script setup>
import { reactive, ref, toRaw } from "vue";
import { gsap } from "gsap";
import {
  IconSharpArrowBack,
  IconOutlineClose,
  IconOutlineCheck,
  IconBaselineBugReport as iconbaselinebugreport,
  IconBaselineLightbulb as iconbaselinelightbulb,
  IconBaselineMoreHoriz as iconbaselinemorehoriz,
} from "@iconify-prerendered/vue-ic";

const feedbackTypes = reactive([
  {
    id: 1,
    label: "Issue",
    header: "Report an issue",
    icon: iconbaselinebugreport,
    placeholder: "I noticed that...",
    color: "text-[#E84545]",
  },
  {
    id: 2,
    label: "Idea",
    header: "Share an idea",
    icon: iconbaselinelightbulb,
    placeholder: "I would love...",
    color: "text-[#FFF600]",
  },
  {
    id: 3,
    label: "Other",
    icon: iconbaselinemorehoriz,
    header: "Tell us something",
    placeholder: "I think...",
    color: "text-gray-400",
  },
]);
const props = defineProps(["isModalOpen"]);
const emit = defineEmits(["closeModal"]);
const reported = ref("");
const pickedOption = reactive({});

const report = ref("");
const activeModal = ref("options");
const toReport = (id) => {
  pickedOption.value = toRaw(feedbackTypes.find((x) => x.id === id));
  reported.value = pickedOption.value.label;
  activeModal.value = "reporting";
};

const sendReport = () => {
  activeModal.value = "send";
  report.value = "";
};

const sendAnotherReport = () => {
  activeModal.value = "options";
};

const back = () => {
  activeModal.value = "options";
  report.value = "";
};
const closeModal = () => {
  emit("closeModal");
  activeModal.value = "options";
  report.value = "";
};

// gsap animation
const enter = () => {
  gsap.to(".modalOverall", {
    top: 50,
    opacity: 1,
    duration: 0.2,
  });
};
</script>
<template>
  <Transition appear @enter="enter">
    <div
      v-if="isModalOpen"
      class="modalOverall absolute top-5 opacity-0 mt-20px px-3 py-4 w-320px bg-dark-200 rounded-md"
    >
      <div v-show="activeModal == 'options'">
        <h2 class="text-2xl font-bold text-center">Whats on your mind?</h2>
        <!-- Picking option -->
        <div class="flex items-center justify-between">
          <div
            v-for="item in feedbackTypes"
            :key="item.id"
            class="flex flex-col items-center justify-center hover:bg-dark-300 cursor-pointer transition-all ease-linear duration-150 px-2 py-3 rounded-md"
            @click="toReport(item.id)"
          >
            <component
              :is="item.icon"
              style="font-size: 72px"
              :class="item.color"
            ></component>
            <p>{{ item.label }}</p>
          </div>
        </div>
      </div>
      <!-- Reporting -->
      <div v-if="activeModal == 'reporting'" class="reporting">
        <div class="flex items-center justify-between mb-2">
          <div class="cursor-pointer grid items-center">
            <IconSharpArrowBack class="text-2xl" @click="back" />
          </div>
          <p>{{ pickedOption.value.header }}</p>
          <div class="cursor-pointer grid items-center">
            <IconOutlineClose @click="closeModal" class="text-2xl" />
          </div>
        </div>
        <div class="py-2">
          <textarea
            v-model="report"
            style="resize: none"
            class="w-full rounded p-2"
            :placeholder="pickedOption.value.placeholder"
          ></textarea>
          <button
            type="button"
            class="mt-2"
            :class="{
              'bg-dark-300 pointer-events-none text-dark-50': !report,
            }"
            :disabled="!report"
            @click="sendReport"
          >
            Send Feedback
          </button>
        </div>
      </div>
      <!-- Gratitude -->
      <div v-if="activeModal === 'send'">
        <div class="flex items-center justify-between">
          <div></div>
          <div class="cursor-pointer grid items-center">
            <IconOutlineClose @click="closeModal" class="text-2xl" />
          </div>
        </div>
        <div>
          <IconOutlineCheck class="text-4xl text-lime-400" />
          <p class="mt-3">Thanks! We received your feedback.</p>
          <button class="mt-3" type="button" @click="sendAnotherReport">
            Send Another
          </button>
        </div>
      </div>
    </div>
  </Transition>
</template>
