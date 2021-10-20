<template>
  <q-item clickable>
    <q-item-section avatar>
      <q-checkbox @update:model-value="$emit('change:complete')" :model-value="complete"/>
    </q-item-section>
    <q-item-section>
      <q-item-label>
        {{title}}
      </q-item-label>
      <q-popup-edit @update:model-value="(val) => $emit('change:title', val)" :model-value="title" auto-save v-slot="scope">
        <q-input v-model="scope.value" dense autofocus counter @keyup.enter="scope.set" />
      </q-popup-edit>
    </q-item-section>
    <q-item-section side>
      <q-item-label>
        <template v-if="complete">
          <q-btn
            flat
            size="sm"
            round
            color="positive"
            icon="done"
          />
        </template>
        <template v-else>
          <q-btn
            flat
            size="sm"
            round
            color="negative"
            icon="delete"
            @click="$emit('delete', id)"
          />
        </template>
      </q-item-label>
    </q-item-section>
  </q-item>
</template>

<script lang="ts">
import { Vue, prop, Options } from 'vue-class-component';

class Props {
  id!: number;
  title = prop<string>({ default: '' });
  complete = prop<boolean>({ default: false });
}

@Options({})
export default class TodoCard extends Vue.with(Props) {
  edit = false
  onEdit () {
    this.edit = !this.edit
  }
}
</script>
