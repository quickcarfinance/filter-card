<template>
  <card class="overflow-hidden flex flex-row">
    <button
      v-for="option in filter.options"
      :key="option.value"
      class="py-4 px-8 focus:outline-none flex-1"
      :class="[
        isActive(option)
          ? 'border-b-2 border-primary-500 text-primary-500 bold'
          : 'text-grey font-semibold border-40',
      ]"
      @click="handleChange(option)"
    >
      {{ option.label }}
    </button>
  </card>
</template>

<script>
import InteractsWithQueryString from "@/mixins/InteractsWithQueryString";
import Filterable from "@/mixins/Filterable";

export default {
  mixins: [InteractsWithQueryString, Filterable],
  props: {
    card: {
      required: true,
    },
    resourceName: {
      type: String,
      required: true,
    },
  },

  data: () => ({
    filterKey: null,
  }),

  created() {
    this.filterKey = this.card.filter;
  },

  mounted() {
    this.$el.classList.remove("min-h-40");
  },

  methods: {
    handleChange(option) {
      this.$store.commit(`${this.resourceName}/updateFilterState`, {
        filterClass: this.filterKey,
        value: option.value,
      });

      this.$emit("change");
    },

    isActive(option) {
      return String(this.value) == String(option.value);
    },
  },

  computed: {
    filter() {
      return this.$store.getters[`${this.resourceName}/getFilter`](
        this.filterKey
      );
    },

    value() {
      return this.filter.currentValue;
    },

    /**
     * Get the name of the page query string variable.
     */
    pageParameter() {
      return this.resourceName + "_page";
    },
  },
};
</script>
