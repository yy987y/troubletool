<template>
  <v-app>
    <v-app-bar color="primary" app>
      <v-spacer></v-spacer>
      <v-btn color="success">
        {{ text.start_text }}
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <v-row wrap fill-height>
          <v-col md4 offset-md4 v-if="currentStep == 0">
            <v-card>
              <v-card-title>{{ text.following_step }}</v-card-title>
              <v-card-text>
                <v-list>
                  <v-list-item v-for="item in testUnites" :key="item.id">
                    <v-list-item-content>
                      <v-list-item-title>{{ item.label }}</v-list-item-title>
                    </v-list-item-content>
                  </v-list-item>
                </v-list>
              </v-card-text>
            </v-card>
          </v-col>
          <v-col md4 offset-md4 v-else>
            <v-stepper>
              <v-stepper-header>
                <v-stepper-step :complete="e1 > 1" step="1">
                  Name of step 1
                </v-stepper-step>

                <v-divider></v-divider>

                <v-stepper-step :complete="e1 > 2" step="2">
                  Name of step 2
                </v-stepper-step>

                <v-divider></v-divider>

                <v-stepper-step step="3"> Name of step 3 </v-stepper-step>
              </v-stepper-header>

              <v-stepper-items>
                <v-stepper-content step="1">
                  <v-card
                    class="mb-12"
                    color="grey lighten-1"
                    height="200px"
                  ></v-card>

                  <v-btn color="primary" @click="e1 = 2"> Continue </v-btn>

                  <v-btn text> Cancel </v-btn>
                </v-stepper-content>

                <v-stepper-content step="2">
                  <v-card
                    class="mb-12"
                    color="grey lighten-1"
                    height="200px"
                  ></v-card>

                  <v-btn color="primary" @click="e1 = 3"> Continue </v-btn>

                  <v-btn text> Cancel </v-btn>
                </v-stepper-content>

                <v-stepper-content step="3">
                  <v-card
                    class="mb-12"
                    color="grey lighten-1"
                    height="200px"
                  ></v-card>

                  <v-btn color="primary" @click="e1 = 1"> Continue </v-btn>

                  <v-btn text> Cancel </v-btn>
                </v-stepper-content>
              </v-stepper-items>
            </v-stepper>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import * as i18n from "../utils/i18n";
export default {
  name: "HelloWorld",

  data() {
    return {
      currentStep: 1,
      testUnites: [
        {
          id: "0",
          label: "browser_compatibility",
          notError: true,
          extra: "",
        },
        {
          id: "1",
          label: "microphone",
          notError: true,
          extra: "",
        },
        {
          id: "2",
          label: "speaker",
          notError: true,
          extra: "",
        },
        {
          id: "3",
          label: "resolution",
          notError: true,
          extra: "",
        },
        {
          id: "4",
          label: "connection",
          notError: true,
          extra: "",
        },
      ],
    };
  },
  computed: {
    text() {
      const lang = "zh";
      const property = i18n[lang]["default"];
      const obj = {};
      for (let key of Object.keys(property)) {
        Object.assign(obj, {
          [`${key}`]: property[key],
        });
      }
      console.log("computed");
      return obj;
    },
  },
  mounted() {
    console.log(this.text);
    console.log("mounted");
  },
};
</script>
