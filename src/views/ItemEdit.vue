<template>
  <div class="item-edit">
    <div class="span headline mb-5">
      Edit
    </div>
    <v-divider />
    <item-form
      :initial-value="initialValue"
      @save="onFormSave"
      @cancel="onFormCancel"
    />
  </div>
</template>

<script>
import ItemForm from '@/components/ItemForm.vue';

export default {
  name: 'ItemEdit',
  components: {
    ItemForm,
  },
  data() {
    return {
      initialValue: null,
    };
  },
  mounted() {
    this.fetchInitialValue();
  },
  methods: {
    onFormSave(form) {
      try {
        const items = JSON.parse(localStorage.getItem('items'));

        const editedIndex = items.findIndex(i => i.id === this.$route.params.id);
        items[editedIndex] = { ...items[editedIndex], ...form };

        const parsedItems = JSON.stringify(items);

        localStorage.setItem('items', parsedItems);
        this.$router.push({ name: 'home' });
      } catch (error) {
        console.log(error);
      }
    },
    onFormCancel() {
      this.$router.push({ name: 'home' });
    },
    fetchInitialValue() {
      try {
        const items = JSON.parse(localStorage.getItem('items'));

        this.initialValue = { ...items.find(i => i.id === this.$route.params.id) };
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>
