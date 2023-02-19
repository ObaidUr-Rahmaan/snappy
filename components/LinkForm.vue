<script lang="ts" setup>
import { nanoid } from "nanoid";

const user = useSupabaseUser();

const isLoading = ref<boolean>(false);

const emits = defineEmits(["shorten"]);

const form = ref<{
  long_url: string;
  key: string;
}>({
  long_url: "",
  key: "",
});

const errors = ref<{
  long_url: string | null;
  key: string | null;
}>({
  long_url: null,
  key: null,
});

onMounted(() => {
  form.value.key = nanoid(6);
});

const handleShorten = async (e: Event) => {
  e.preventDefault();

  if (!user.value) {
    return;
  }

  if (!form.value.long_url) {
    errors.value.long_url = "Long URL is required";
    return;
  }

  if (!form.value.key || form.value.key.length < 6) {
    errors.value.key = "Key must be at least 6 characters";
    return;
  }

  try {
    isLoading.value = true;
    const { data, error } = await useLinks().createLink(
      form.value.long_url,
      form.value.key
    );
    if (!error) {
      emits("shorten", data);
      form.value.long_url = "";
      form.value.key = nanoid(6);
      errors.value.long_url = null;
      errors.value.key = null;
    }
  } catch (e) {
    console.log(e);
  } finally {
    isLoading.value = false;
  }
};
</script>

<template>
  <form
    class="flex flex-wrap justify-between gap-5 mt-10 card sm:flex-nowrap items-middle"
    @submit.prevent="handleShorten"
  >
    <div class="w-full form-group sm:w-6/12">
      <label for="long_url" class="block mb-1 text-xs text-white/30"
        >Long URL</label
      >
      <input
        v-model="form.long_url"
        for="long_url"
        type="text"
        placeholder="enter or paste long URL"
      />
      <span v-if="errors.long_url" class="py-1 text-xs text-red-500">
        {{ errors.long_url }}</span
      >
    </div>
    <div class="w-full form-group sm:w-4/12">
      <label for="key" class="block mb-1 text-xs text-white/30"
        >Short Key</label
      >
      <input
        v-model="form.key"
        for="key"
        type="text"
        placeholder="enter a key (optional)"
        required
        min="6"
      />
      <span v-if="errors.key" class="py-1 text-xs text-red-500">
        {{ errors.key }}</span
      >
    </div>
    <div class="flex self-end justify-end w-full sm:w-2/12">
      <button
        type="submit"
        class="flex items-center justify-center w-full gap-2 px-5 py-2 transition-all duration-200 rounded-lg bg-primary hover:shadow-xl"
      >
        <template v-if="isLoading">
          <svg
            class="w-6 h-6 text-white animate-spin"
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
          >
            <circle
              class="opacity-25"
              cx="12"
              cy="12"
              r="10"
              stroke="currentColor"
              stroke-width="4"
            ></circle>
            <path
              class="opacity-75"
              fill="currentColor"
              d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
            ></path>
          </svg>
        </template>
        <template v-else>
          Shorten
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="w-6 h-6"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M13.19 8.688a4.5 4.5 0 011.242 7.244l-4.5 4.5a4.5 4.5 0 01-6.364-6.364l1.757-1.757m13.35-.622l1.757-1.757a4.5 4.5 0 00-6.364-6.364l-4.5 4.5a4.5 4.5 0 001.242 7.244"
            />
          </svg>
        </template>
      </button>
    </div>
  </form>
</template>
