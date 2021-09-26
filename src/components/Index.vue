<template>
  <v-app>
    <v-app-bar color="primary" app>
      <v-spacer></v-spacer>
      <v-btn color="success" v-if="testing == false" @click="startTest">
        {{ text.start_text }}
      </v-btn>
    </v-app-bar>
    <v-main>
      <v-container>
        <v-row wrap fill-height>
          <!-- start page -->
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
          <!-- result page -->
          <v-col md4 offset-md4 v-else-if="currentStep == 6">
            <v-card>
              <v-card-title>{{ text.test_report }}</v-card-title>
              <v-card-text>
                <v-list>
                  <v-list-item v-for="item in testUnites" :key="item.id">
                    <v-list-item-content>
                      <v-list-item-title>{{ item.label }}</v-list-item-title>
                      <v-icon v-if="item.notError" color="success">Good</v-icon>
                      <v-icon v-else color="error">Failed</v-icon>
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
                  兼容性测试
                </v-stepper-step>

                <v-divider></v-divider>
                <v-stepper-step :complete="currentStep > 1" step="2">
                  麦克风测试
                </v-stepper-step>

                <v-divider></v-divider>

                <v-stepper-step :complete="currentStep > 2" step="3">
                  扬声器测试
                </v-stepper-step>

                <v-divider></v-divider>

                <v-stepper-step :complete="currentStep > 3" step="4">
                  分辨率测试
                </v-stepper-step>
                <v-divider></v-divider>

                <v-stepper-step :complete="currentStep > 4" step="5">
                  连通性测试
                </v-stepper-step>
              </v-stepper-header>

              <v-stepper-items>
                <!-- system requirements test -->
                <v-stepper-content step="1">
                  <v-row>
                    <v-col md6>
                      <v-card color="rgba(0,157,231,0.2)" height="100%">
                        <v-card-title>
                          <div class="headline">
                            {{ text.browser_check }}
                          </div>
                        </v-card-title>
                        <v-card-text>
                          {{ text.support_desc }}
                        </v-card-text>
                      </v-card>
                    </v-col>
                    <v-col md6>
                      <v-card color="rgba(0,157,231,0.2)" height="100%">
                        <v-card-title>
                          {{ text.checking }}
                        </v-card-title>
                        <v-card-text>
                          <v-progress-linear
                            :indeterminate="true"
                          ></v-progress-linear>
                        </v-card-text>
                      </v-card>
                    </v-col>
                  </v-row>
                </v-stepper-content>

                <!-- mic test -->
                <v-stepper-content step="2">
                  <v-row>
                    <v-col md6>
                      <v-card color="rgba(0,157,231,0.2)" height="100%">
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
                      <v-card color="rgba(0,157,231,0.2)" height="100%">
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
                <v-stepper-content step="3">
                  <v-row>
                    <v-col md6>
                      <v-card color="rgba(0,157,231,0.2)" height="100%">
                        <v-card-title>
                          <div class="headline">
                            {{ text.speacker_check }}
                          </div>
                        </v-card-title>
                        <v-card-text>
                          {{ text.speaker_check_desc }}
                        </v-card-text>
                        <v-btn @click="speakerTest"> 听得见 </v-btn>

                        <v-btn text color="error"> 听不见 </v-btn>
                      </v-card>
                    </v-col>
                    <v-col md6>
                      <v-card color="rgba(0,157,231,0.2)" height="100%">
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
                <v-stepper-content step="4">
                  <v-row>
                    <v-col md6>
                      <v-card color="rgba(0,157,231,0.2)" height="100%">
                        <v-card-title>
                          <div class="headline">
                            {{ text.resolution_check }}
                          </div>
                        </v-card-title>
                        <v-card-text>
                          {{ text.resolution_check_desc }}
                        </v-card-text>
                      </v-card>
                    </v-col>
                    <v-col md6>
                      <v-card color="rgba(0,157,231,0.2)" height="100%">
                        <v-card-title>
                          {{ text.resolution_list }}
                        </v-card-title>
                        <v-card-text>
                          <v-list>
                            <v-list-item
                              v-for="(profile, index) in profiles"
                              :key="index"
                            >
                              <v-list-item-content>
                                <v-list-item-title>{{
                                  profile.resolution
                                }}</v-list-item-title>
                                <v-list-item-action icon>
                                  <v-icon
                                    v-if="profile.status === 'resolve'"
                                    color="success"
                                    >Good</v-icon
                                  >
                                  <v-icon
                                    v-else-if="profile.status === 'reject'"
                                    color="error"
                                    >Failed</v-icon
                                  >
                                  <v-icon v-else>pending</v-icon>
                                </v-list-item-action>
                              </v-list-item-content>
                            </v-list-item>
                          </v-list>
                        </v-card-text>
                      </v-card>
                    </v-col>
                  </v-row>
                </v-stepper-content>

                <!-- connection test -->
                <v-stepper-content step="5">
                  <v-row md12 v-if="subscribed == false">
                    <v-alert>
                      {{ text.network_check_desc }}
                    </v-alert>
                  </v-row>
                  <v-row v-else>
                    <v-col md6>
                      <v-card height="100%">
                        <v-card-text>
                          <ve-line
                            :data="bitrateData"
                            :settings="chartSettings"
                          ></ve-line>
                        </v-card-text>
                      </v-card>
                    </v-col>
                    <v-col md6>
                      <v-card height="100%">
                        <v-card-text>
                          <ve-line
                            :data="packetsData"
                            :settings="chartSettings"
                          ></ve-line>
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
  name: "Index",

  data() {
    return {
      client: {},
      localStream: {},
      currentStep: 0,
      inputVolume: 0,
      uid: "",
      meetSystemRequirements: false,
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
      chartSettings: {},
      bitrateData: {
        columns: ["index", "tVideoBitrate", "tAudioBitrate"],
        rows: [
          {
            index: 0,
            tVideoBitrate: 0,
            tAudioBitrate: 0,
          },
        ],
      },
      packetsData: {
        columns: ["index", "tVideoPacketLoss", "tAudioPacketLoss"],
        rows: [
          {
            index: 0,
            tVideoPacketLoss: 0,
            tAudioPacketLoss: 0,
          },
        ],
      },
      subscribed: false,
      testing: false,
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
      appkey: "45c6af3c98409b18a84451215d0bdd6e",
      debug: true,
    });
  },
  methods: {
    startTest() {
      this.currentStep++;
      this.testing = true;
      setTimeout(() => {
        this.systemRequirementsTest();
      }, 1500);
    },
    systemRequirementsTest() {
      this.meetSystemRequirements = webRTC.checkSystemRequirements();
      this.currentStep++;
      this.audioTest();
    },
    audioTest() {
      let localUid=Math.floor(Math.random() * 10000);
      this.client
          .join({
            channelName: "房间名称",
            uid: localUid,
            token: "",
          })
          .then(() => {
            console.info("加入房间成功...");
            //初始化本地流，并且发布
            let localStream = webRTC.createStream({
              uid: localUid,
              audio: true,
              video: false,
            });
            //启动媒体，打开实例对象中设置的媒体设备
            localStream
              .init()
              .then(() => {
                console.warn("音视频开启完成，可以播放了");
                localStream
                  .play(null, {
                    audio: true,
                    video: false,
                  })
                  .then(() => {
                    console.warn("播放成功");
                  })
                  .catch((err) => {
                    console.warn("播放失败: ", err);
                  });
                // 发布
                this.client
                  .publish(localStream)
                  .then(() => {
                    console.warn("本地 publish 成功");
                  })
                  .catch((err) => {
                    console.error("本地 publish 失败: ", err);
                  });
              })
              .catch((err) => {
                console.warn("音视频初始化失败: ", err);
                localStream = null;
              });

            let _this = this;
            let micTestTimer = setInterval(() => {
              _this.inputVolume = parseInt(localStream.getAudioLevel() * 100);
            }, 100);
            setTimeout(() => {
              clearInterval(micTestTimer);
              this.client.leave();
              this.currentStep = 3;
            }, 3000);
          });
    },
    speakerTest() {
      this.resolutionTest();
    },
    resolutionTest() {
      this.currentStep = 4;
      this.resolutionBuild().then(() => {
        setTimeout(() => {
          this.connectionTest();
          this.currentStep = 5;
        }, 3000);
      });
    },
    resolutionBuild() {
      return new Promise((resolve) => {
        let localStream = webRTC.createStream({
          uid: Math.floor(Math.random() * 10000),
          audio: false,
          video: true,
        });
        for (let profile of this.profiles) {
          localStream.setVideoProfile({
            profile: webRTC[`${profile.resolution}`],
          });
          localStream
            .init()
            .then(() => {
              console.log(`${profile.resolution} init success`);
              profile.status = "resolve";
              localStream.destroy();
            })
            .catch(() => {
              console.log(`${profile.resolution} init failed`);
              profile.status = "reject";
              localStream.destroy();
            });
        }
        resolve(true);
      });
    },
    connectionTest() {
      this.connectionBuild().then(([localUid, remoteClient]) => {
        this.renderChart(localUid, remoteClient);
      });
    },
    connectionBuild() {
      return new Promise((resolve) => {
        let localUid = Math.floor(Math.random() * 10000);
        this.client
          .join({
            channelName: "房间名称",
            uid: localUid,
            token: "",
          })
          .then(() => {
            console.info("加入房间成功...");
            //初始化本地流，并且发布
            let localStream = webRTC.createStream({
              uid: localUid,
              audio: true,
              video: true,
            });
            //启动媒体，打开实例对象中设置的媒体设备
            localStream
              .init()
              .then(() => {
                console.warn("音视频开启完成，可以播放了");
                localStream
                  .play(null, {
                    audio: true,
                    video: true,
                  })
                  .then(() => {
                    console.warn("播放成功");
                  })
                  .catch((err) => {
                    console.warn("播放失败: ", err);
                  });
                // 发布
                this.client
                  .publish(localStream)
                  .then(() => {
                    console.warn("本地 publish 成功");
                  })
                  .catch((err) => {
                    console.error("本地 publish 失败: ", err);
                  });
              })
              .catch((err) => {
                console.warn("音视频初始化失败: ", err);
                localStream = null;
              });
          });

        let remoteClient = webRTC.createClient({
          appkey: "45c6af3c98409b18a84451215d0bdd6e", //您的 appkey
          debug: false, //是否开启调试日志
        });
        let remoteUid = Math.floor(Math.random() * 10000);
        remoteClient
          .join({
            channelName: "房间名称",
            uid: remoteUid,
            token: "",
          })
          .then(() => {
            console.info("加入房间成功...");
          });

        remoteClient.on("stream-added", (evt) => {
          let remoteStream = evt.stream;
          console.warn("收到对方发布的订阅消息: ", remoteStream.getId());
          remoteStream.setSubscribeConfig({
            audio: true,
            video: true,
          });
          remoteClient
            .subscribe(remoteStream)
            .then(() => {
              console.warn("本地 subscribe 成功");
              this.subscribed = true;
            })
            .catch((err) => {
              console.warn("本地 subscribe 失败: ", err);
            });
        });

        resolve([localUid, remoteClient]);
      });
    },
    renderChart(localUid, remoteClient) {
      let _this = this;
      let indexCount = 1;

      let statsTimer = setInterval(async () => {
        const localVideoStats = await _this.client.getLocalVideoStats();
        const localAudioStats = await _this.client.getLocalAudioStats();
        const remoteVideoStatsMap = await remoteClient.getRemoteVideoStats();
        const remoteAudioStatsMap = await remoteClient.getRemoteAudioStats();
        _this.bitrateData.rows.push({
          index: indexCount,
          tVideoBitrate: localAudioStats[0].SendBitrate,
          tAudioBitrate: localVideoStats[0].SendBitrate,
        });
        _this.packetsData.rows.push({
          index: indexCount,
          tVideoPacketLoss: remoteVideoStatsMap[localUid].PacketLossRate,
          tAudioPacketLoss: remoteAudioStatsMap[localUid].PacketLossRate,
        });
        indexCount++;
      }, 1000);

      setTimeout(() => {
        clearInterval(statsTimer);
        this.client.leave();
        remoteClient.leave();
        this.currentStep = 6;
        this.restore();
      }, 10000);
    },
    restore() {
      setTimeout(() => {
        this.currentStep = 0;
        this.testing = false;
        this.bitrateData= {
          columns: ["index", "tVideoBitrate", "tAudioBitrate"],
          rows: [
              {
                index: 0,
                tVideoBitrate: 0,
                tAudioBitrate: 0,
              },
            ],
        };
        this.packetsData={
          columns: ["index", "tVideoPacketLoss", "tAudioPacketLoss"],
          rows: [
            {
              index: 0,
              tVideoPacketLoss: 0,
              tAudioPacketLoss: 0,
            },
          ],
        };
        this.inputVolume=0;
        this.subscribed=false;
        for (let profile of this.profiles) {
          profile.status='pending';
        }
      }, 3000);
    },
  },
};
</script>
