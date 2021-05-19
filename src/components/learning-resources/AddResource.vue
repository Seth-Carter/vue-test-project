<template>
  <base-dialog
    @close="confirmError"
    v-if="inputIsInvalid"
    title="Invalid Input"
  >
    <template #default>
      <p>At least one input value is invalid.</p>
      <p>Check all inputs and make sure no fields are empty.</p>
    </template>
    <template #actions>
      <base-button @click="confirmError">Confirm</base-button>
    </template>
  </base-dialog>
  <base-card>
    <form @submit.prevent="handleSubmit">
      <div class="form-control">
        <label for="title">Title</label>
        <input v-model="formValues.title" id="title" name="title" type="text" />
      </div>
      <div class="form-control">
        <label for="description">Description</label>
        <textarea
          v-model="formValues.description"
          id="description"
          name="description"
        ></textarea>
      </div>
      <div class="form-control">
        <label for="link">Link</label>
        <input v-model="formValues.link" id="link" name="link" type="url" />
      </div>
      <div>
        <base-button type="submit">Add Resource</base-button>
      </div>
    </form>
  </base-card>
</template>

<script>
export default {
  inject: ['addResource'],
  data() {
    return {
      inputIsInvalid: false,
      formValues: {
        title: '',
        description: '',
        link: ''
      }
    };
  },
  methods: {
    handleSubmit() {
      if (
        this.formValues.title.trim() === '' ||
        this.formValues.description.trim() === '' ||
        this.formValues.link.trim() === ''
      ) {
        this.inputIsInvalid = true;
        return;
      }
      this.addResource(
        this.formValues.title,
        this.formValues.description,
        this.formValues.link
      );
      Object.keys(this.formValues).forEach(key => {
        this.formValues[key] = '';
      });
    },
    confirmError() {
      this.inputIsInvalid = false;
    }
  }
};
</script>

<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #3a0061;
  background-color: #f7ebff;
}

.form-control {
  margin: 1rem 0;
}
</style>
