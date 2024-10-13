<template>
  <div>
    <base-card>
      <form @submit.prevent="submitData">
        <div class="form-control">
          <label for="title">Title</label>
          <input type="text" name="title" id="title" ref="titleInput" />
        </div>
        <div class="form-control">
          <label for="description">Description</label>
          <textarea
            name="description"
            id="description"
            rows="3"
            ref="descriptionInput"
          />
        </div>
        <div class="form-control">
          <label for="link">Link</label>
          <input type="url" name="link" id="link" ref="linkInput" />
        </div>
        <div class="form-control">
          <base-button type="submit">Add Resource</base-button>
        </div>
      </form>
    </base-card>
    <base-dialog v-if="invalidInput" @close="closeDialog">
        <template #header>Error</template>
        <p>All fields are required, please check the form and submit it again.</p>
    </base-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      invalidInput: false,
    }
  },
  inject: ['addResource'],
  methods: {
    submitData() {
      const title = this.$refs.titleInput.value
      const description = this.$refs.descriptionInput.value
      const link = this.$refs.linkInput.value
      if (title.trim() === '' ||description.trim() === '' ||link.trim() === '') {
        this.invalidInput = true
        return
      }
      this.addResource(title, description, link)
      this.$refs.titleInput.value = ''
      this.$refs.descriptionInput.value = ''
      this.$refs.linkInput.value = ''
    },
    closeDialog() {
        this.invalidInput = false;
    }
  }
}
</script>

<style>
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
