<template>
  <q-page class="q-pa-md">
    <q-card flat bordered class="my-card">
      <q-card-section>
        <div class="text-h6">Machine UUID</div>
      </q-card-section>

      <q-card-section>
        <q-btn @click="getUUID" color="primary" label="Get Device UUID" />
        <div v-if="machineId" class="q-mt-md">
          <q-chip label="UUID" icon="fingerprint" />
          <div class="text-body1 q-mt-sm">{{ machineId }}</div>
        </div>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script setup>
import { ref } from "vue";

// Reactive variable to hold machine UUID
const machineId = ref(null);

// Method to get UUID from Electron using node-machine-id
const getUUID = async () => {
  if (window.require) {
    const { machineId: getMachineId } = window.require("node-machine-id");

    try {
      // Fetch UUID
      machineId.value = await getMachineId();
    } catch (error) {
      console.error("Error fetching machine UUID:", error);
    }
  } else {
    console.warn("Electron not available.");
  }
};
</script>

<style scoped>
.my-card {
  max-width: 400px;
  margin: 0 auto;
}
</style>
