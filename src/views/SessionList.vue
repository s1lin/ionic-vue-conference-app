<template>
  <div class="ion-page">

    <ion-header>
      <ion-card color="light" style="border-radius: 10px; margin: 10px">
        <ion-row>
          <ion-col size="3">
            <div class="img-wrapper">
              <img :src="user.profilePic" :alt="user.name">
            </div>
          </ion-col>
          <ion-col size="9">
            <h1 style="margin-top: 10px; margin-bottom: 5px">{{ user.name }}</h1>
            <ion-icon style="margin-top:0.65%" name="pin" color="medium">
            </ion-icon>
            <ion-text color="medium" style="font-size: 16px">
              {{ user.location }}
            </ion-text>
          </ion-col>
        </ion-row>
      </ion-card>
    </ion-header>

    <ion-content>
      <ion-slides>
        <ion-slide>
          <ion-card>
            <ion-card-header class="ion-text-center" :color="'danger'">
              <h3>Status: <strong>Danger</strong></h3>
              <p>As for {{ new Date().toLocaleDateString() }},{{ new Date().toLocaleTimeString() }}</p>
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
        <ion-slide>
          <ion-card>
          <ion-card-header :color="'medium'">
            <h4>In resident room, and not in bed</h4>
            <ion-note slot="end" color="light">
              <ion-icon style="margin-top: 0.5%" name="bed" size="medium"></ion-icon>
            </ion-note>

            <ion-row>
              <ion-col size="12" size-lg>
                <ion-item lines="none" color="light" style="--border-radius : 10px;">
                  <ion-label> Breakfast: 8:00am</ion-label>
                  <ion-note color="success">
                    <ion-icon name="checkmark-circle" size="large" style="margin-top:0.5%"></ion-icon>
                  </ion-note>
                </ion-item>
              </ion-col>

              <ion-col size="12" size-lg>
                <ion-item lines="none" color="light" style="--border-radius : 10px;">
                  <ion-label> Lunch: scheduled @12pm</ion-label>
                  <ion-note color="warning">
                    <ion-icon name="help-circle" size="large"></ion-icon>
                  </ion-note>
                </ion-item>
              </ion-col>
              <ion-col size="12" size-lg>
                <ion-item lines="none" color="light" style="--border-radius : 10px;">
                  <ion-label> Dinner: scheduled @6pm</ion-label>
                  <ion-note color="warning">
                    <ion-icon name="help-circle" size="large"></ion-icon>
                  </ion-note>
                </ion-item>
              </ion-col>
            </ion-row>
          </ion-card-header>
          </ion-card>
        </ion-slide>
      </ion-slides>


      <ion-toolbar color="primary">
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
            <ion-item button @click="goToSessionDetail(session)">
              <ion-label>
                <h3>{{ session.name }}</h3>
                <p>{{ session.dateTimeStart | dateFormat("h:mm a") }} &mdash;
                  {{ session.dateTimeEnd | dateFormat("h:mm a") }}: {{ session.location }}</p>
              </ion-label>
            </ion-item>
            <ion-item-options>
              <ion-item-option
                color="favorite"
                @click="addFavorite($event, session)"
                v-if="segment === 'all'"
              >Favorite
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
  border-left: 2px solid var(--ion-color-primary);
  padding-left: 10px;
}

ion-item-sliding[track="angular"] ion-label {
  border-left: 2px solid var(--ion-color-angular);
  padding-left: 10px;
}

ion-item-sliding[track="communication"] ion-label {
  border-left: 2px solid var(--ion-color-communication);
  padding-left: 10px;
}

ion-item-sliding[track="tooling"] ion-label {
  border-left: 2px solid var(--ion-color-tooling);
  padding-left: 10px;
}

ion-item-sliding[track="services"] ion-label {
  border-left: 2px solid var(--ion-color-services);
  padding-left: 10px;
}

ion-item-sliding[track="design"] ion-label {
  border-left: 2px solid var(--ion-color-design);
  padding-left: 10px;
}

ion-item-sliding[track="workshop"] ion-label {
  border-left: 2px solid var(--ion-color-workshop);
  padding-left: 10px;
}

ion-item-sliding[track="food"] ion-label {
  border-left: 2px solid var(--ion-color-food);
  padding-left: 10px;
}

ion-item-sliding[track="documentation"] ion-label {
  border-left: 2px solid var(--ion-color-documentation);
  padding-left: 10px;
}

ion-item-sliding[track="navigation"] ion-label {
  border-left: 2px solid var(--ion-color-navigation);
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

  groupedByStartTime(sessions: Session[]) {
    return sessions
      .sort(
        (a, b) =>
          parseDate(a.dateTimeStart).valueOf() -
          parseDate(b.dateTimeStart).valueOf()
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
      return this.groupedByStartTime(this.$store.getters.allFiltered);
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

ion-card .img-wrapper {
  position: fixed;
  transform: translateX(-10%) translateY(5%);
}

ion-card .img-wrapper img {
  border-radius: 10px;
  width: 80px;
  height: 80px;
}

ion-slide {
  padding: 10px 0;
}

.card {
  background-color: #1fc8db;
  background-image: linear-gradient(141deg, #9fb8ad 0%, #1fc8db 51%, #2cb5e8 75%);
  border-radius: 10px;
  height: 170px;
  margin: 0 9px;
}

ion-row {
  margin: 10px;
}

.balance, .expDate, .card-number {
  text-align: left;
}

.balance, .card-number {
  font-size: 20px;
}

.card-number {
  font-size: 7vw;
}

.card-icon {
  text-align: right;
}

</style>
