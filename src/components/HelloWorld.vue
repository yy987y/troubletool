<template>
  <v-app>
    <v-app-bar color="primary" app>
      <v-spacer></v-spacer>
      <v-btn color="success" @click="startTest">
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
            <v-stepper v-model="currentStep">
              <v-stepper-header>
                <v-stepper-step :complete="currentStep > 0" step="1">
                  Name of step 1
                </v-stepper-step>

                <v-divider></v-divider>

                <v-stepper-step :complete="currentStep > 1" step="2">
                  Name of step 2
                </v-stepper-step>

                <v-divider></v-divider>

                <v-stepper-step step="3"> Name of step 3 </v-stepper-step>
              </v-stepper-header>

              <v-stepper-items>
                <!-- mic test -->
                <v-stepper-content step="1">
                  <v-row>
                    <v-col md6>
                      <v-card color="info" height="100%">
                        <v-card-title>
                          <div class="headline">
                            {{ text.microphone_check }}
                          </div>
                        </v-card-title>
                        <v-card-text>
                          {{ text.microphone_check_desc }}
                        </v-card-text>
                      </v-card>
                    </v-col>
                    <v-col md6>
                      <v-card color="info" height="100%">
                        <v-card-title>
                          {{ text.microphone_volume_check_desc }}
                        </v-card-title>
                        <v-card-text>
                          <v-progress-linear
                            :value="inputVolume"
                          ></v-progress-linear>
                        </v-card-text>
                      </v-card>
                    </v-col>
                  </v-row>
                </v-stepper-content>

                <!-- speaker test -->
                <v-stepper-content step="2">
                  <v-row>
                    <v-col md6>
                      <v-card color="info" height="100%">
                        <v-card-title>
                          <div class="headline">
                            {{ text.speacker_check }}
                          </div>
                        </v-card-title>
                        <v-card-text>
                          {{ text.speaker_check_desc }}
                        </v-card-text>
                        <v-btn @click="currentStep = 3"> Continue </v-btn>

                        <v-btn text color="error"> Cancel </v-btn>
                      </v-card>
                    </v-col>
                    <v-col md6>
                      <v-card color="info" height="100%">
                        <v-card-title>
                          <div class="headline">{{ text.sample_music }}</div>
                        </v-card-title>
                        <v-card-text>
                          <audio id="sampleMusic" controls="controls">
                            <source
                              src="../assets/music.mp3"
                              type="audio/mp3"
                            />
                            {{ text.sample_music_desc }}
                          </audio>
                        </v-card-text>
                      </v-card>
                    </v-col>
                  </v-row>
                </v-stepper-content>

                <!-- resolution test -->
                <v-stepper-content step="3">
                  <v-row>
                    <v-col md6>
                      <v-card color="info" height="100%">
                        <v-card-title>
                          <div class="headline">
                            {{ text.resolution_check }}
                          </div>
                        </v-card-title>
                        <v-card-text>
                          {{ text.resolution_check_desc }}
                          <v-btn @click="resolutionTest">test</v-btn>
                        </v-card-text>
                      </v-card>
                    </v-col>
                    <v-col md6>
                      <v-card height="100%">
                        <v-card-title>
                           {{text.resolution_list}} </v-card-title>
                        <v-card-text>
                          <v-list>
                            <v-list-item
                              v-for="(profile,index) in profiles"
                              :key="index"
                            >
                              <v-list-item-content>
                                <v-list-item-title>{{
                                  profile.resolution
                                }}</v-list-item-title>
                                <v-list-tile-action icon>
                                  <v-icon v-if="profile.status==='resolve'" color="success">done</v-icon>
                                  <v-icon v-else-if="profile.status==='reject'" color="error">close</v-icon>
                                  <v-icon v-else>more_horiz</v-icon>
                                </v-list-tile-action>
                              </v-list-item-content>
                            </v-list-item>
                          </v-list>
                        </v-card-text>
                      </v-card>
                    </v-col>
                  </v-row>
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
import * as webRTC from "../utils/NIM_Web_NERTC_v4.4.1";
import { profileArray } from "../utils/resolutionList";
export default {
  name: "HelloWorld",

  data() {
    return {
      client: {},
      localStream: {},
      currentStep: 0,
      inputVolume: 0,
      uid: "",
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
      profiles: profileArray.map((item) => {
        item.status = "pending";
        return item;
      }),
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
      return obj;
    },
  },
  mounted() {
    this.client = webRTC.createClient({
      appkey: "4727023efa991d31d61b3b32e819bd5b",
      debug: true,
    });
  },
  methods: {
    startTest() {
      this.currentStep++;
      this.audioTest();
    },
    audioTest() {
      this.uid = Math.floor(Math.random() * 10000);
      this.localStream = webRTC.createStream({
        uid: this.uid,
        audio: true,
        video: true,
        screen: false,
      });

      this.localStream.init().then(() => {
        this.localStream
          .play(null, {
            audio: true,
            video: false,
            screen: false,
          })
          .then(() => {
            console.warn("播放成功");
          })
          .catch((err) => {
            console.warn("播放失败: ", err);
          });
        let _this = this;
        let micTestTimer = setInterval(() => {
          _this.inputVolume = parseInt(this.localStream.getAudioLevel() * 100);
        }, 100);
        setTimeout(() => {
          clearInterval(micTestTimer);
          console.log(this.localStream.getAudioTrack());
          this.localStream.destroy();
          // .then(() => {
          //   console.warn("关闭 mic sucess");
          // })
          // .catch((err) => {
          //   console.warn("关闭 mic 失败: ", err);
          // });

          this.currentStep = 2;
        }, 3000);
      });
    },
    speakerTest() {
      console.log("speakerTest");
    },
    resolutionTest() {
      let localStream = webRTC.createStream({
        uid: Math.floor(Math.random() * 10000),
        audio: false,
        video: true
      });
      for (let profile of this.profiles) {
        localStream.setVideoProfile({
          profile: webRTC[`${profile.resolution}`],
        });
        localStream.init().then(() => {
          console.log(`${profile.resolution} init success`);
          profile.status="resolve";
          localStream.destroy();
        }).catch(()=>{
          console.log(`${profile.resolution} init failed`);
          profile.status="reject";
          localStream.destroy();
        });
      }
    },
  },
};
</script>
