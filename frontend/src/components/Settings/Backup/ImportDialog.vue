<template>
  <div class="text-center">
    <v-dialog
      v-model="dialog"
      width="500"
      :fullscreen="$vuetify.breakpoint.xsOnly"
    >
      <v-card>
        <v-toolbar dark color="primary" v-show="$vuetify.breakpoint.xsOnly">
          <v-btn icon dark @click="dialog = false">
            <v-icon>mdi-close</v-icon>
          </v-btn>
          <v-toolbar-title></v-toolbar-title>
          <v-spacer></v-spacer>
          <v-toolbar-items>
            <v-btn dark text @click="raiseEvent('import')">
              {{ $t("general.import") }}
            </v-btn>
          </v-toolbar-items>
        </v-toolbar>
        <v-card-title> {{ name }} </v-card-title>
        <v-card-subtitle class="mb-n3"> {{ date }} </v-card-subtitle>
        <v-divider></v-divider>

        <v-card-text>
          <v-row>
            <v-col>
              <v-checkbox
                class="mb-n4 mt-1"
                dense
                :label="$t('settings.backup.import-recipes')"
                v-model="importRecipes"
              ></v-checkbox>
              <v-checkbox
                class="my-n4"
                dense
                :label="$t('settings.backup.import-themes')"
                v-model="importThemes"
              ></v-checkbox>
              <v-checkbox
                class="my-n4"
                dense
                :label="$t('settings.backup.import-settings')"
                v-model="importSettings"
              ></v-checkbox>
            </v-col>
            <!-- <v-col>
              <v-tooltip top>
                <template v-slot:activator="{ on, attrs }">
                  <span v-on="on" v-bind="attrs">
                    <v-checkbox
                      class="mb-n4 mt-1"
                      dense
                      label="Force"
                      v-model="forceImport"
                    ></v-checkbox>
                  </span>
                </template>
                <span>Force update existing recipes</span>
              </v-tooltip>
              <v-tooltip top>
                <template v-slot:activator="{ on, attrs }">
                  <span v-on="on" v-bind="attrs">
                    <v-checkbox
                      class="mb-n4 mt-1"
                      dense
                      label="Rebase"
                      v-model="rebaseImport"
                    ></v-checkbox>
                  </span>
                </template>
                <span
                  >Removes all recipes, and then imports recipes from the
                  backup</span
                >
              </v-tooltip>
            </v-col> -->
          </v-row>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-btn color="accent" text :href="`/api/backups/${name}/download`">
            {{ $t("general.download") }}
          </v-btn>
          <v-spacer></v-spacer>
          <v-btn color="error" text @click="raiseEvent('delete')">
            {{ $t("general.delete") }}
          </v-btn>
          <v-btn
            color="success"
            outlined
            @click="raiseEvent('import')"
            v-show="$vuetify.breakpoint.smAndUp"
          >
            {{ $t("general.import") }}
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>


<script>
export default {
  props: {
    name: {
      default: "Backup Name",
    },
    date: {
      default: "Backup Date",
    },
  },
  data() {
    return {
      dialog: false,
      importRecipes: true,
      forceImport: false,
      rebaseImport: false,
      importThemes: false,
      importSettings: false,
    };
  },
  methods: {
    open() {
      this.dialog = true;
    },
    close() {
      this.dialog = false;
    },
    raiseEvent(event) {
      let eventData = {
        name: this.name,
        recipes: this.importRecipes,
        force: this.forceImport,
        rebase: this.rebaseImport,
        themes: this.importThemes,
        settings: this.importSettings,
      };
      this.close();
      this.$emit(event, eventData);
    },
  },
};
</script>

<style>
</style>