<!--
  - Copyright 2019 Padduck, LLC
  -  Licensed under the Apache License, Version 2.0 (the "License");
  -  you may not use this file except in compliance with the License.
  -  You may obtain a copy of the License at
  -          http://www.apache.org/licenses/LICENSE-2.0
  -  Unless required by applicable law or agreed to in writing, software
  -  distributed under the License is distributed on an "AS IS" BASIS,
  -  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  -  See the License for the specific language governing permissions and
  -  limitations under the License.
  -->

<template>
  <div>
    <v-expansion-panels
      multiple
      class="mb-2"
    >
      <v-expansion-panel
        v-for="(env, i) in value"
        :key="i"
      >
        <v-expansion-panel-header v-text="env.type" />
        <v-expansion-panel-content>
          <ui-input
            v-if="env.type === 'docker'"
            v-model="env.image"
            :label="$t('templates.DockerImage')"
            hide-details
            class="mb-2"
          />
          <v-btn
            color="error"
            block
            @click="$delete(value, i)"
            v-text="$t('common.Delete')"
          />
        </v-expansion-panel-content>
      </v-expansion-panel>
    </v-expansion-panels>
    <v-btn
      v-if="!addingEnv"
      text
      block
      @click="addingEnv = true"
      v-text="$t('templates.AddEnvironment')"
    />
    <v-row v-else>
      <v-col
        cols="12"
        md="6"
      >
        <ui-select
          v-model="newEnv"
          :label="$t('templates.Environment')"
          :items="possibleEnvironments"
          dense
        />
      </v-col>
      <v-col
        cols="6"
        md="3"
      >
        <v-btn
          color="primary"
          block
          @click="addEnv()"
          v-text="$t('templates.AddEnvironment')"
        />
      </v-col>
      <v-col
        cols="6"
        md="3"
      >
        <v-btn
          color="error"
          block
          @click="newEnv = 'standard'; addingEnv = false"
          v-text="$t('common.Cancel')"
        />
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  props: {
    value: { type: Array, default: () => [] }
  },
  data () {
    return {
      addingEnv: false,
      newEnv: 'standard',
      possibleEnvironments: [
        'standard',
        'tty',
        'docker'
      ]
    }
  },
  methods: {
    addEnv () {
      this.value.push({ type: this.newEnv })
      this.newEnv = 'standard'
      this.addingEnv = false
    }
  }
}
</script>
