<template>
  <div class="home">
    <v-card>
      <v-data-table
        :headers="tableHeaders"
        :items="tableItems"
        :items-per-page="5"
      >
        <template v-slot:item.actions="{ item }">
          <v-icon
            color="blue"
            class="mr-2"
            @click="onEditItemClick(item)"
          >
            mdi-square-edit-outline
          </v-icon>
          <v-icon
            color="red"
            @click="onDeleteItemClick(item)"
          >
            mdi-close-box-multiple-outline
          </v-icon>
        </template>
      </v-data-table>
    </v-card>
    <v-btn
      color="blue"
      class="mt-5 white--text"
      @click="onCreateItemClick"
    >
      Create
    </v-btn>
    <v-dialog
      v-model="isDeleteDialogVisible"
      width="500"
    >
      <v-card>
        <v-card-title class="justify-center">
          Are you sure you want to delete this item?
        </v-card-title>
        <v-divider />
        <v-card-actions class="justify-center">
          <v-btn
            color="red"
            text
            @click="onAcceptDeleteClick"
          >
            Delete
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>

export default {
  name: 'Home',
  data() {
    return {
      tableHeaders: [
        { text: 'Id', value: 'id' },
        { text: 'Name', value: 'name' },
        { text: 'Description', value: 'description' },
        { text: 'Actions', value: 'actions' },
      ],
      tableItems: [],
      isDeleteDialogVisible: false,
    };
  },
  mounted() {
    this.fetchItems();
  },
  methods: {
    onCreateItemClick() {
      this.$router.push({ name: 'create' });
    },
    onEditItemClick({ id }) {
      this.$router.push({
        name: 'edit',
        params: {
          id,
        },
      });
    },
    onDeleteItemClick({ id }) {
      this.isDeleteDialogVisible = true;

      // use cause of issue with reactivity in nested slots(actions -> dialog)
      this.deletedItemId = id;
    },
    fetchItems() {
      try {
        this.tableItems = [...JSON.parse(localStorage.getItem('items')) || []];
      } catch (error) {
        console.log(error);
      }
    },
    onAcceptDeleteClick() {
      const deletedIndex = this.tableItems.findIndex(i => i.id === this.deletedItemId);

      this.tableItems.splice(deletedIndex, 1);

      const parsedItems = JSON.stringify(this.tableItems);
      localStorage.setItem('items', parsedItems);
      this.isDeleteDialogVisible = false;
    },
  },
};
</script>
