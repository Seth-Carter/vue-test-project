<template>
  <base-card>
    <base-button
      name="stored-resources"
      :mode="storedResButtonMode"
      @click="setSelectedTab($event)"
      >Stored Resources</base-button
    >
    <base-button
      name="add-resource"
      :mode="addResButtonMode"
      @click="setSelectedTab($event)"
      >Add Resource</base-button
    >
  </base-card>
  <keep-alive>
    <component :is="selectedTab"></component>
  </keep-alive>
</template>

<script>
import StoredResources from './StoredResources.vue';
import AddResource from './AddResource.vue';

export default {
  components: {
    StoredResources,
    AddResource
  },
  data() {
    return {
      selectedTab: 'stored-resources',
      storedResources: [
        {
          id: 'official-guide',
          title: 'Official Guide',
          description: 'The official Vue.js documentation.',
          link: 'https://google.org'
        },
        {
          id: 'official-guide2',
          title: 'Official Guide 2',
          description: 'The official Vue.js documentation.',
          link: 'https://google.org'
        }
      ]
    };
  },
  provide() {
    return {
      resources: this.storedResources,
      addResource: this.addResource,
      deleteResource: this.deleteResource
    };
  },
  computed: {
    storedResButtonMode() {
      return this.selectedTab === 'stored-resources' ? null : 'flat';
    },
    addResButtonMode() {
      return this.selectedTab === 'add-resource' ? null : 'flat';
    }
  },
  methods: {
    setSelectedTab(event) {
      this.selectedTab = event.target.name;
    },
    addResource(title, description, url) {
      const newResource = {
        id: new Date().toISOString(),
        title: title,
        description: description,
        link: url
      };
      this.storedResources.unshift(newResource);
      this.selectedTab = 'stored-resources';
      console.log(this.storedResources);
    },
    deleteResource(id) {
      console.log(id);
      const resourceIndex = this.storedResources.findIndex(
        resource => resource.id === id
      );
      console.log(resourceIndex);
      this.storedResources.splice(resourceIndex, 1);
    }
  }
};
</script>
