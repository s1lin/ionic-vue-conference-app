<template>
  <div class="ion-page">
    <ion-content>
      <ion-refresher slot="fixed" @ionRefresh="doRefresh($event)">
        <ion-refresher-content
          pullingIcon="arrow-down"
          pulling-text="Pull to refresh"
          refreshingSpinner="bubbles"
          refreshing-text="Refreshing...">
        </ion-refresher-content>
      </ion-refresher>
      <!--        style="box-shadow: 0 4px 16px #00000059"-->
      <ion-card color="light" style="overflow: visible; padding-bottom: 10px">
        <div class="img-wrapper">
          <img :src="user.profilePic" :alt="user.name">
          <!--            <ion-button color="light">-->
          <!--              <ion-icon slot="icon-only" name="camera-outline" color="medium">-->
          <!--              </ion-icon>-->
          <!--            </ion-button>-->
        </div>
        <ion-card-content class="ion-text-center" style="margin-top: 40px; padding-top: 50px">
          <h1 style="margin-top: 10px; margin-bottom: 5px; font-weight: 530; color: black">{{ user.name }}</h1>
          <ion-text color="medium">
            <div style="margin-top: 0.65%; display: flex;" class="ion-justify-content-center">
              <ion-icon name="pin" color="medium">
              </ion-icon>
              <ion-text color="medium" style="font-size: 13px">
                {{ user.location }}
              </ion-text>
            </div>
          </ion-text>
        </ion-card-content>
      </ion-card>
      <ion-slides pager="true">
        <ion-slide v-show='status === 0' style="padding-top: 0">
        <ion-card>
          <ion-card-header class="ion-text-center" :color="'success'">
            <h3>Overall: <strong>Normal</strong></h3>
            <!--              <p>As for {{ new Date().toLocaleDateString() }},{{ new Date().toLocaleTimeString() }}</p>-->
            <p>As for 3/16/2021, 9:20:12 AM</p>

            <ion-item lines="none" color="light" style="--border-radius : 10px; margin-bottom: 2%">
              <ion-label text-wrap>In resident room, and in not bed
              </ion-label>
              <ion-note color="danger">
                <ion-icon name="bed" size="large" style="margin-top:30%"></ion-icon>
              </ion-note>
            </ion-item>

            <ion-item style="--detail-icon-color: black; --detail-icon-font-size:30px; --border-radius : 10px;"
                      detail="true" lines="none" button color="warning"
                      @click="goToSpeakerDetail(keeper)">
              <ion-row>
                <ion-col size="9" style="margin-top: -3%">
                  <h4 style="color: darkblue" text-wrap>
                    Today's Caregiver: <strong>{{ keeper.name }}</strong>
                  </h4>
                </ion-col>
                <ion-col size="3">
                  <ion-avatar>
                    <img v-bind:src="keeper.profilePic" alt="Speaker profile pic">
                  </ion-avatar>
                </ion-col>
              </ion-row>
            </ion-item>

          </ion-card-header>
        </ion-card>
      </ion-slide>
        <ion-slide v-show='status === 1' style="padding-top: 0">
          <ion-card>
            <ion-card-header class="ion-text-center" :color="'danger'">
              <h3>Overall: <strong>Danger</strong></h3>
              <p>As for 3/16/2021, 9:20:13 AM</p>

              <ion-item lines="none" color="light" style="--border-radius : 10px; margin-bottom: 2%">
                <ion-label text-wrap>In resident room, and not in bed
                </ion-label>
                <ion-note color="danger">
                  <ion-icon name="bed" size="large" style="margin-top:30%"></ion-icon>
                </ion-note>
              </ion-item>

              <ion-item style="--detail-icon-color: black; --detail-icon-font-size:30px; --border-radius : 10px;"
                        detail="true" lines="none" button color="warning"
                        @click="goToSpeakerDetail(keeper)">
                <ion-row>
                  <ion-col size="12">
                    <h5 style="color: darkblue" text-wrap>
                      <strong> Fall Down Detected!</strong>
                      <br> Immediate Attention Needed!
                      <br> Click to Contact Care Facility!
                    </h5>
                  </ion-col>
                  <ion-col size="9" style="margin-top: -3%">
                    <h4 style="color: darkblue" text-wrap>
                      Today's Caregiver: <strong>{{ keeper.name }}</strong>
                    </h4>
                  </ion-col>
                  <ion-col size="3">
                    <ion-avatar>
                      <img v-bind:src="keeper.profilePic" alt="Speaker profile pic">
                    </ion-avatar>
                  </ion-col>
                </ion-row>
              </ion-item>

            </ion-card-header>
          </ion-card>
        </ion-slide>
        <ion-slide v-show='status === 2' style="padding-top: 0">
          <ion-card>
            <ion-card-header class="ion-text-center" :color="'success'">
              <h3>Overall: <strong>Normal</strong></h3>
              <!--              <p>As for {{ new Date().toLocaleDateString() }},{{ new Date().toLocaleTimeString() }}</p>-->
              <p>As for 3/16/2021, 08:00:00 AM</p>

              <ion-item lines="none" color="light" style="--border-radius : 10px; margin-bottom: 2%">
                <ion-label text-wrap>In resident room, and in not bed
                </ion-label>
                <ion-note color="danger">
                  <ion-icon name="bed" size="large" style="margin-top:30%"></ion-icon>
                </ion-note>
              </ion-item>

              <ion-item style="--detail-icon-color: black; --detail-icon-font-size:30px; --border-radius : 10px;"
                        detail="true" lines="none" button color="warning"
                        @click="goToSpeakerDetail(keeper)">
                <ion-row>
                  <ion-col size="9" style="margin-top: -3%">
                    <h4 style="color: darkblue" text-wrap>
                      Today's Caregiver: <strong>{{ keeper.name }}</strong>
                    </h4>
                  </ion-col>
                  <ion-col size="3">
                    <ion-avatar>
                      <img v-bind:src="keeper.profilePic" alt="Speaker profile pic">
                    </ion-avatar>
                  </ion-col>
                </ion-row>
              </ion-item>

            </ion-card-header>
          </ion-card>
        </ion-slide>
        <ion-slide v-show='status === 3' style="padding-top: 0">
          <ion-card>
            <ion-card-header class="ion-text-center" :color="'success'">
              <h3>Overall: <strong>Normal</strong></h3>
              <!--              <p>As for {{ new Date().toLocaleDateString() }},{{ new Date().toLocaleTimeString() }}</p>-->
              <p>As for 3/16/2021, 08:00:12AM</p>

              <ion-item lines="none" color="light" style="--border-radius : 10px; margin-bottom: 2%">
                <ion-label text-wrap>In resident room, and in not bed
                </ion-label>
                <ion-note color="danger">
                  <ion-icon name="bed" size="large" style="margin-top:30%"></ion-icon>
                </ion-note>
              </ion-item>

              <ion-item style="--detail-icon-color: black; --detail-icon-font-size:30px; --border-radius : 10px;"
                        detail="true" lines="none" button color="warning"
                        @click="goToSpeakerDetail(keeper)">
                <ion-row>
                  <ion-col size="9" style="margin-top: -3%">
                    <h4 style="color: darkblue" text-wrap>
                      Today's Caregiver: <strong>{{ keeper.name }}</strong>
                    </h4>
                  </ion-col>
                  <ion-col size="3">
                    <ion-avatar>
                      <img v-bind:src="keeper.profilePic" alt="Speaker profile pic">
                    </ion-avatar>
                  </ion-col>
                </ion-row>
              </ion-item>

            </ion-card-header>
          </ion-card>
        </ion-slide>
        <ion-slide v-show='status === 4' style="padding-top: 0">
          <ion-card>
            <ion-card-header class="ion-text-center" :color="'success'">
              <h3>Overall: <strong>Normal</strong></h3>
              <!--              <p>As for {{ new Date().toLocaleDateString() }},{{ new Date().toLocaleTimeString() }}</p>-->
              <p>As for 3/16/2021, 07:29:10 AM</p>

              <ion-item lines="none" color="light" style="--border-radius : 10px; margin-bottom: 2%">
                <ion-label text-wrap>In resident room, and in bed
                </ion-label>
                <ion-note color="success">
                  <ion-icon name="bed" size="large" style="margin-top:30%"></ion-icon>
                </ion-note>
              </ion-item>

              <ion-item style="--detail-icon-color: black; --detail-icon-font-size:30px; --border-radius : 10px;"
                        detail="true" lines="none" button color="warning"
                        @click="goToSpeakerDetail(keeper)">
                <ion-row>
                  <ion-col size="9" style="margin-top: -3%">
                    <h4 style="color: darkblue" text-wrap>
                      Today's Caregiver: <strong>{{ keeper.name }}</strong>
                    </h4>
                  </ion-col>
                  <ion-col size="3">
                    <ion-avatar>
                      <img v-bind:src="keeper.profilePic" alt="Speaker profile pic">
                    </ion-avatar>
                  </ion-col>
                </ion-row>
              </ion-item>

            </ion-card-header>
          </ion-card>
        </ion-slide>
        <ion-slide v-show='status === 5' style="padding-top: 0">
        <ion-card>
          <ion-card-header class="ion-text-center" :color="'success'">
            <h3>Overall: <strong>Normal</strong></h3>
            <!--              <p>As for {{ new Date().toLocaleDateString() }},{{ new Date().toLocaleTimeString() }}</p>-->
            <p>As for 3/16/2021, 07:30:00 AM</p>

            <ion-item lines="none" color="light" style="--border-radius : 10px; margin-bottom: 2%">
              <ion-label text-wrap>In resident room, and in not bed
              </ion-label>
              <ion-note color="danger">
                <ion-icon name="bed" size="large" style="margin-top:30%"></ion-icon>
              </ion-note>
            </ion-item>

            <ion-item style="--detail-icon-color: black; --detail-icon-font-size:30px; --border-radius : 10px;"
                      detail="true" lines="none" button color="warning"
                      @click="goToSpeakerDetail(keeper)">
              <ion-row>
                <ion-col size="9" style="margin-top: -3%">
                  <h4 style="color: darkblue" text-wrap>
                    Today's Caregiver: <strong>{{ keeper.name }}</strong>
                  </h4>
                </ion-col>
                <ion-col size="3">
                  <ion-avatar>
                    <img v-bind:src="keeper.profilePic" alt="Speaker profile pic">
                  </ion-avatar>
                </ion-col>
              </ion-row>
            </ion-item>

          </ion-card-header>
        </ion-card>
        </ion-slide>
        <ion-slide v-show='status === 6' style="padding-top: 0">
          <ion-card>
            <ion-card-header class="ion-text-center" :color="'success'">
              <h3>Overall: <strong>Normal</strong></h3>
              <!--              <p>As for {{ new Date().toLocaleDateString() }},{{ new Date().toLocaleTimeString() }}</p>-->
              <p>As for 3/15/2021, 11:20:12 PM</p>

              <ion-item lines="none" color="light" style="--border-radius : 10px; margin-bottom: 2%">
                <ion-label text-wrap>In resident room, and in bed
                </ion-label>
                <ion-note color="success">
                  <ion-icon name="bed" size="large" style="margin-top:30%"></ion-icon>
                </ion-note>
              </ion-item>

              <ion-item style="--detail-icon-color: black; --detail-icon-font-size:30px; --border-radius : 10px;"
                        detail="true" lines="none" button color="warning"
                        @click="goToSpeakerDetail(keeper2)">
                <ion-row>
                  <ion-col size="9" style="margin-top: -3%">
                    <h4 style="color: darkblue" text-wrap>
                      Today's Caregiver: <strong>{{ keeper2.name }}</strong>
                    </h4>
                  </ion-col>
                  <ion-col size="3">
                    <ion-avatar>
                      <img v-bind:src="keeper2.profilePic" alt="Speaker profile pic">
                    </ion-avatar>
                  </ion-col>
                </ion-row>
              </ion-item>

            </ion-card-header>
          </ion-card>
        </ion-slide>
        <ion-slide v-show='status === 7' style="padding-top: 0">
          <ion-card>
            <ion-card-header class="ion-text-center" :color="'success'">
              <h3>Overall: <strong>Normal</strong></h3>
              <!--              <p>As for {{ new Date().toLocaleDateString() }},{{ new Date().toLocaleTimeString() }}</p>-->
              <p>As for 3/15/2021, 11:20:12 PM</p>

              <ion-item lines="none" color="light" style="--border-radius : 10px; margin-bottom: 2%">
                <ion-label text-wrap>In resident room, and not in bed
                </ion-label>
                <ion-note color="danger">
                  <ion-icon name="bed" size="large" style="margin-top:30%"></ion-icon>
                </ion-note>
              </ion-item>

              <ion-item style="--detail-icon-color: black; --detail-icon-font-size:30px; --border-radius : 10px;"
                        detail="true" lines="none" button color="warning"
                        @click="goToSpeakerDetail(keeper2)">
                <ion-row>
                  <ion-col size="9" style="margin-top: -3%">
                    <h4 style="color: darkblue" text-wrap>
                      Today's Caregiver: <strong>{{ keeper2.name }}</strong>
                    </h4>
                  </ion-col>
                  <ion-col size="3">
                    <ion-avatar>
                      <img v-bind:src="keeper2.profilePic" alt="Speaker profile pic">
                    </ion-avatar>
                  </ion-col>
                </ion-row>
              </ion-item>

            </ion-card-header>
          </ion-card>
        </ion-slide>
        <ion-slide style="padding-top: 0">
          <ion-card>
            <ion-card-header :color="'primary'">
              <h3>Food Card: <strong>Normal</strong></h3>
              <p v-show="status === 2">As for 3/16/2021, 07:59:59 AM</p>
              <p v-show="status === 3">As for 3/16/2021, 08:00:00 AM</p>
              <p v-show="status <= 1 || status > 3">As for {{ new Date().toLocaleDateString() }},{{ new Date().toLocaleTimeString() }}</p>
              <ion-row>
                <ion-col size="12" size-lg>
                  <ion-item lines="none" color="light" style="--border-radius : 10px;">
                    <ion-label v-show="status <= 1 || status === 3 || status > 5"> Breakfast: 8:00am</ion-label>
                    <ion-label v-show="status === 2 || (status > 3 && status <= 5)"> Breakfast: scheduled@ 8am</ion-label>
                    <ion-note color="success" v-show="status <= 1 || status === 3 || status > 5">
                      <ion-icon name="checkmark-circle" size="large" style="margin-top:30%"></ion-icon>
                    </ion-note>
                    <ion-note color="warning" v-show="status === 2 || (status > 3 && status <= 5)">
                      <ion-icon name="help-circle" size="large" style="margin-top:30%"></ion-icon>
                    </ion-note>
                  </ion-item>
                </ion-col>

                <ion-col size="12" size-lg>
                  <ion-item lines="none" color="light" style="--border-radius : 10px;">
                    <ion-label v-show="status <= 5"> Lunch: scheduled @12pm</ion-label>
                    <ion-label v-show="status > 5"> Lunch: 12:00pm</ion-label>
                    <ion-note color="success" v-show="status > 5">
                      <ion-icon name="checkmark-circle" size="large" style="margin-top:30%"></ion-icon>
                    </ion-note>
                    <ion-note color="warning" v-show="status <= 5">
                      <ion-icon name="help-circle" size="large" style="margin-top:30%"></ion-icon>
                    </ion-note>
                  </ion-item>
                </ion-col>
                <ion-col size="12" size-lg>
                  <ion-item lines="none" color="light" style="--border-radius : 10px;">
                    <ion-label v-show="status <= 5"> Dinner: scheduled @6pm</ion-label>
                    <ion-label v-show="status > 5"> Dinner: 6:00pm</ion-label>
                    <ion-note color="success" v-show="status > 5">
                      <ion-icon name="checkmark-circle" size="large" style="margin-top:30%"></ion-icon>
                    </ion-note>
                    <ion-note color="warning" v-show="status <= 5">
                      <ion-icon name="help-circle" size="large" style="margin-top:30%"></ion-icon>
                    </ion-note>
                  </ion-item>
                </ion-col>
              </ion-row>
            </ion-card-header>
          </ion-card>
        </ion-slide>
      </ion-slides>
      <ion-toolbar color="light">
        <ion-searchbar v-model="queryText" @ionChange="updateSearchTerm" placeholder="Search"></ion-searchbar>
      </ion-toolbar>
      <ion-list v-show="allGrouped.length > 0">
        <ion-item-group v-for="group in allGrouped" :key="group.id">
          <ion-item-divider sticky>
            <ion-label>{{ group.startTime | dateFormat("h:mm a") }}</ion-label>
          </ion-item-divider>

          <ion-item-sliding
            v-for="session in group.sessions"
            :key="session.id"
            :track="session.tracks[0] | lowercase"
          >
            <ion-item button
                      @click="goToSessionDetail(session)"
                      :class="{'ion-color':session.tracks[0]==='design', 'ion-color-danger':session.tracks[0]==='design'}">
              <ion-label>
                <h3>{{ session.name }}</h3>
                <p>{{ session.dateTimeStart | dateFormat("h:mm a") }} &mdash;
                  {{ endTime(session.dateTimeEnd) }}: {{ session.location }}</p>
              </ion-label>
            </ion-item>
            <ion-item-options>
              <ion-item-option
                color="favorite"
                @click="addFavorite($event, session)"
                v-if="segment === 'all'"
              >Save
              </ion-item-option>
              <ion-item-option
                color="danger"
                @click="removeFavorite($event, session, 'Remove Favorite')"
                v-if="segment === 'favorites'"
              >Remove
              </ion-item-option>
            </ion-item-options>
          </ion-item-sliding>
        </ion-item-group>
      </ion-list>
      <ion-list-header v-show="allGrouped.length === 0">No Sessions Found</ion-list-header>

    </ion-content>

  </div>
</template>

<style scoped>
ion-item-sliding[track="ionic"] ion-label {
  border-left: 5px solid var(--ion-color-primary);
  padding-left: 10px;
}

ion-item-sliding[track="angular"] ion-label {
  border-left: 5px solid var(--ion-color-angular);
  padding-left: 10px;
}

ion-item-sliding[track="communication"] ion-label {
  border-left: 5px solid var(--ion-color-communication);
  padding-left: 10px;
}

ion-item-sliding[track="tooling"] ion-label {
  border-left: 5px solid var(--ion-color-tooling);
  padding-left: 10px;
}

ion-item-sliding[track="services"] ion-label {
  border-left: 5px solid var(--ion-color-services);
  padding-left: 10px;
}

ion-item-sliding[track="design"] ion-label {
  border-left: 5px solid var(--ion-color-design);
  padding-left: 10px;
}

ion-item-sliding[track="workshop"] ion-label {
  border-left: 5px solid var(--ion-color-workshop);
  padding-left: 10px;
}

ion-item-sliding[track="food"] ion-label {
  border-left: 5px solid var(--ion-color-food);
  padding-left: 10px;
}

ion-item-sliding[track="documentation"] ion-label {
  border-left: 5px solid var(--ion-color-documentation);
  padding-left: 10px;
}

ion-item-sliding[track="navigation"] ion-label {
  border-left: 5px solid var(--ion-color-navigation);
  padding-left: 10px;
}
</style>

<script lang="ts">
import {Component, Prop, Vue} from "vue-property-decorator";
import {Session, SessionGroup} from "../store/modules/sessions";
import {format, parse, parse as parseDate} from "date-fns";
import SessionListFilter from "./SessionListFilter.vue";


@Component
export default class SessionList extends Vue {
  $refs!: {
    fab: HTMLIonFabElement;
  };
  segment = "all";
  status = 0;
  counter = 0;

  doRefresh(event: CustomEvent) {
    console.log('Begin async operation', event);
    event.detail.complete();
    this.counter++;
    this.status = this.counter % 8;
  }

  groupedByStartTime(sessions: Session[]) {
    return sessions
      .sort(
        (a, b) =>
          parseDate(b.dateTimeEnd).valueOf() -
          parseDate(a.dateTimeEnd).valueOf()
      )
      .reduce(
        (groups, session) => {
          let starterHour = parseDate(session.dateTimeStart);
          starterHour.setMinutes(0);
          starterHour.setSeconds(0);
          const starterHourStr = starterHour.toJSON();

          const foundGroup = groups.find(
            group => group.startTime === starterHourStr
          );
          if (foundGroup) {
            foundGroup.sessions.push(session);
          } else {
            groups.push({
              startTime: starterHourStr,
              sessions: [session]
            });
          }
          return groups;
        },
        [] as SessionGroup[]
      );
  }

  get allGrouped() {
    if (this.segment === "all") {

      var temp = this.groupedByStartTime(this.$store.getters.allFiltered);
      console.log(temp)
      if(this.status === 0) //normal 1: fall down
        temp = temp.slice(1);
      else if (this.status === 2) //: before breakfast
        temp = temp.slice(3);
      else if (this.status === 3) //: post breakfast
        temp = temp.slice(2);
      else if (this.status === 4) //: before getup
        temp = temp.slice(4);
      else if (this.status === 5) { //: post getup
        temp = temp.slice(3);
        temp[0].sessions = temp[0].sessions.slice(1);
      } else if (this.status === 6) { //: post lie down
        temp = temp.slice(5);
      } else if (this.status === 7) { //: post getup
        temp = temp.slice(4);
        temp[0].sessions = temp[0].sessions.slice(1);
      }
      return temp;
    } else {
      return this.groupedByStartTime(this.$store.getters.favoritesFiltered);
    }
  }

  get queryText() {
    return this.$store.state.sessions.searchText;
  }

  async addFavorite(event: MouseEvent, session: Session) {
    if (
      this.$store.state.sessions.favoriteSessions.indexOf(session.id) !== -1
    ) {
      // woops, they already favorited it! What shall we do!?
      // prompt them to remove it
      this.removeFavorite(event, session, "Favorite already added");
    } else {
      // remember this session as a user favorite
      this.$store.dispatch("addFavorite", session.id);

      // create an alert instance
      const alert = await this.$ionic.alertController.create({
        header: "Favorite Added",
        buttons: [
          {
            text: "OK",
            handler: () => {
              // close the sliding item
              const slidingItem = (event.target as HTMLElement).closest(
                "ion-item-sliding"
              );
              (<any>slidingItem).close();
            }
          }
        ]
      });
      // now present the alert on top of all other content
      await alert.present();
    }
  }

  async removeFavorite(event: MouseEvent, session: Session, title: string) {
    const alert = await this.$ionic.alertController.create({
      header: title,
      message: "Would you like to remove this session from your favorites?",
      buttons: [
        {
          text: "Cancel",
          handler: () => {
            // they clicked the cancel button, do not remove the session
            // close the sliding item and hide the option buttons
            const slidingItem = (event.target as HTMLElement).closest(
              "ion-item-sliding"
            );
            (<any>slidingItem).close();
          }
        },
        {
          text: "Remove",
          handler: () => {
            // they want to remove this session from their favorites
            this.$store.dispatch("removeFavorite", session.id);

            // close the sliding item and hide the option buttons
            const slidingItem = (event.target as HTMLElement).closest(
              "ion-item-sliding"
            );
            (<any>slidingItem).close();
          }
        }
      ]
    });
    // now present the alert on top of all other content
    await alert.present();
  }

  goToSessionDetail(session: Session) {
    this.$router.push({
      name: "session-detail",
      params: {sessionId: session.id.toString()}
    });
  }

  async presentFilter() {
    const modal = await this.$ionic.modalController.create({
      component: SessionListFilter,
      componentProps: {
        excludedTracks: this.$store.state.sessions.trackFilters,
        allTracks: this.$store.getters.allTracks
      }
    });
    await modal.present();

    const {data} = await modal.onWillDismiss();
    if (data) {
      this.$store.dispatch("updateTrackFilters", data);
    }
  }

  updateSegment(e: CustomEvent) {
    this.segment = e.detail.value;
  }

  updateSearchTerm(e: CustomEvent) {
    this.$store.dispatch("setSearchText", e.detail.value);
  }

  async openSocial(network: string) {
    const loading = await this.$ionic.loadingController.create({
      message: `Posting to ${network}`,
      duration: Math.random() * 1000 + 500
    });
    await loading.present();
    await loading.onWillDismiss();
    this.$refs.fab.close();
  }

  get user() {
    return this.$store.state.speakers.speakers.concat().sort()[0];
  }

  sessionsBySpeaker(speakerId: number) {
    return this.$store.state.sessions.sessions.filter(
      (s: Session) => s.speakerIds.indexOf(speakerId) !== -1
    );
  }

  get speaker() {
    console.log(this.$store.state.speakers.speakers.concat().sort());
    return this.$store.state.speakers.speakers.concat().sort()[0]
  }

  get keeper() {
    console.log(this.$store.state.speakers.speakers.concat().sort());
    return this.$store.state.speakers.speakers.concat().sort()[2]
  }

  get keeper2() {
    console.log(this.$store.state.speakers.speakers.concat().sort());
    return this.$store.state.speakers.speakers.concat().sort()[3]
  }

  dateFormat(dateString: string, formatString: string) {
    return format(parse(dateString), formatString);
  }

  endTime(dateTimeEnd: string) {
    if (dateTimeEnd != "")
      return this.dateFormat(dateTimeEnd, "h:mm a")
    else
      return ""
  }

}
</script>
<style>

/*ion-card .img-wrapper {*/
/*  position: fixed;*/
/*  !*transform: translateX(-10%) translateY(5%);*!*/
/*}*/

/*ion-card .img-wrapper img {*/
/*  border-radius: 10px;*/
/*  width: 80px;*/
/*  height: 80px;*/
/*  margin-top: 10%;*/
/*}*/

/*ion-slide {*/
/*  padding: 10px 0;*/
/*}*/

/*ion-card {*/
/*  box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.2);*/
/*  overflow: visible;*/
/*  margin: 16px 0 24px;*/
/*  position: relative;*/
/*  padding-top: 60px;*/
/*}*/

ion-card .img-wrapper {
  position: absolute;
  left: 50%;
  top: -30px;
  transform: translateX(-50%);
}

ion-card .img-wrapper img {
  border-radius: 10px;
  width: 80px;
  height: 80px;
}

ion-card .img-wrapper ion-button {
  --border-width: 1px;
  --border-color: var(--ion-color-light-shade);
  --border-style: solid;
  --padding-start: 0;
  --padding-end: 0;
  --padding-bottom: 0;
  --padding-top: 0;

  --border-radius: 50%;
  height: 26px;
  width: 26px;

  position: absolute;
  right: -10px;
  bottom: -10px;
}

ion-card .img-wrapper ion-button ion-icon {
  width: 14px;
  height: 14px;
}

.swiper-container-horizontal > .swiper-pagination-bullets, .swiper-pagination-custom, .swiper-pagination-fraction {
  bottom: 0;
}

</style>
