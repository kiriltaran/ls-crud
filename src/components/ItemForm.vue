<template>
  <div class="item-form">
    <v-form ref="form">
      <v-text-field
        v-model="form.name"
        :counter="10"
        :rules="formValidationRules.name"
        label="Name"
        required
      />
      <v-textarea
        v-model="form.description"
        :rules="formValidationRules.description"
        :counter="300"
        label="Description"
      />
      <div class="mt-5">
        <v-btn
          color="blue"
          class="mr-2 white--text"
          @click="onSaveClick"
        >
          Save
        </v-btn>
        <v-btn
          color="red"
          class="white--text"
          @click="onCancelClick"
        >
          Cancel
        </v-btn>
      </div>
    </v-form>
  </div>
</template>

<script>
export default {
  name: 'ItemForm',
  props: {
    initialValue: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {
      form: {
        name: '',
        description: '',
      },
      formValidationRules: {
        name: [
          v => !!v || 'Name is required',
          v => v.length <= 10 || 'Name must be no more than 10 characters',
        ],
        description: [
          v => !!v || 'Description is required',
          v => v.length <= 300 || 'Description must be no more than 300 characters',
        ],
      },
    };
  },
  watch: {
    initialValue() {
      this.defineInitialValue();
    },
  },
  methods: {
    onSaveClick() {
      if (this.$refs.form.validate()) {
        this.$emit('save', this.form);
      }
    },
    onCancelClick() {
      this.$emit('cancel');
    },
    defineInitialValue() {
      if (this.initialValue) {
        this.form = { ...this.initialValue };
      }
    },
  },
};
</script>
