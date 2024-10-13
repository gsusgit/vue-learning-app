<template>
  <div>
    <base-card>
      <base-button @click="setSeletedTab('stored-resources')" :mode="storedResourcesButtonMode">Stored Resources</base-button>
      <base-button @click="setSeletedTab('add-resource')" :mode="addResourceButtonMode">Add Resource</base-button>
    </base-card>
    <keep-alive>
        <component :is="selectedTab"></component>
    </keep-alive>
  </div>
</template>

<script>
import StoredResources from './StoredResources.vue'
import AddResource from './AddResource.vue'

export default {
  components: {
    StoredResources,
    AddResource,
  },
  data() {
    return {
      selectedTab: 'stored-resources',
      storedResources: [
        {
          id: 'official-guide',
          title: 'Official Guide',
          description: 'The official Vue JS Guide',
          link: 'https://www.vuejs.org',
        },
        {
          id: 'google',
          title: 'Google',
          description: 'Learn to google...',
          link: 'https://www.google.com',
        },
      ]
    }
  },
  computed: {
    storedResourcesButtonMode() {
        return this.selectedTab === 'stored-resources' ? null : 'flat';
    },
    addResourceButtonMode() {
        return this.selectedTab === 'add-resource' ? null : 'flat';
    }
  },
  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResource,
      deleteResource: this.deleteResource
    }
  },
  methods: {
    setSeletedTab(tab) {
      this.selectedTab = tab;
    },
    addResource(title, description, link) {
        const resource = {
          id: new Date().toISOString(),
          title,
          description,
          link
        }
        this.storedResources.unshift(resource);
        this.setSeletedTab('stored-resources');
    },
    deleteResource(id) {
      const resourceToDelete = this.storedResources.findIndex(resource => resource.id === id);
      this.storedResources.splice(resourceToDelete, 1);
    }
  }
}
</script>
