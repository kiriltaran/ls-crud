<template>
  <div class="item-create">
    <div class="span headline mb-5">
      Create
    </div>
    <v-divider />
    <item-form
      @save="onFormSave"
      @cancel="onFormCancel"
    />
  </div>
</template>

<script>
import uuidv4 from 'uuid/v4';

import ItemForm from '@/components/ItemForm.vue';


export default {
  name: 'ItemCreate',
  components: {
    ItemForm,
  },
  methods: {
    onFormSave(form) {
      try {
        const item = {
          ...form,
          id: uuidv4(),
        };

        const items = JSON.parse(localStorage.getItem('items')) || [];
        items.push(item);
        const stringifiedItems = JSON.stringify(items);

        localStorage.setItem('items', stringifiedItems);
        this.$router.push({ name: 'home' });
      } catch (error) {
        console.log(error);
      }
    },
    onFormCancel() {
      this.$router.push({ name: 'home' });
    },
  },
};
</script>
