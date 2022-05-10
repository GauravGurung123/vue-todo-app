<template>
  <!-- <q-page class="flex flex-center">
    <img
      alt="Quasar logo"
      src="~assets/quasar-logo-vertical.svg"
      style="width: 200px; height: 200px"
    >
  </q-page> -->
  <h1>hello world</h1>
  <div class="q-pa-md">
    <q-btn label="Open Menu" color="primary">
      <q-menu>
        <q-list>
          <q-item tag="label">
            <q-item-section avatar>
              <q-checkbox v-model="firstItemEnabled" />
            </q-item-section>
            <q-item-section>
              <q-item-label>First Item Enabled</q-item-label>
            </q-item-section>
          </q-item>
          <q-item
            v-for="n in 5"
            :key="n"
            v-close-popup="n > 1 || firstItemEnabled"
            :clickable="n > 1 || firstItemEnabled"
            @click="onClick(n)"
          >
            <q-item-section>Menu Item {{ n }}</q-item-section>
          </q-item>
        </q-list>
      </q-menu>
    </q-btn>
  </div>
</template>

<script>
import { defineComponent } from "vue";
import { useQuasar } from 'quasar'
import { ref } from 'vue'


export default defineComponent({
  name: "IndexPage",
  setup () {
    const $q = useQuasar()
    const firstItemEnabled = ref(true)

    return {
      firstItemEnabled,
      onClick (index) {
        if (index > 1 || firstItemEnabled.value) {
          $q.notify({
            message: `Clicked on menu item #${index} and closed QMenu`,
            color: 'primary'
          })
        }
      }
    }
  }
});
</script>
