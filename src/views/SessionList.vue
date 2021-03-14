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
      <ion-slides pager="true">
        <ion-slide>
          <ion-grid class="card">
            <ion-row>
              <ion-col class="expDate">
                {{ user.name }}
              </ion-col>
              <ion-col class="card-icon">
                <ion-icon name="card"/>
              </ion-col>
            </ion-row>
            <ion-row>
              <ion-col class="card-number">
                {{ user.name }}
              </ion-col>
            </ion-row>
            <ion-row>
              <ion-col class="balance">
                R {{ user.name }}
              </ion-col>
            </ion-row>
          </ion-grid>
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

      <ion-fab slot="fixed" vertical="bottom" horizontal="end" ref="fab">
        <ion-fab-button ref="fabButton">
          <ion-icon name="share"></ion-icon>
        </ion-fab-button>
        <ion-fab-list ref="fabList" side="top">
          <ion-fab-button color="vimeo" @click="openSocial('Vimeo')">
            <ion-icon name="logo-vimeo"></ion-icon>
          </ion-fab-button>
          <ion-fab-button color="google" @click="openSocial('Google+')">
            <ion-icon name="logo-googleplus"></ion-icon>
          </ion-fab-button>
          <ion-fab-button color="twitter" @click="openSocial('Twitter')">
            <ion-icon name="logo-twitter"></ion-icon>
          </ion-fab-button>
          <ion-fab-button color="facebook" @click="openSocial('Facebook')">
            <ion-icon name="logo-facebook"></ion-icon>
          </ion-fab-button>
        </ion-fab-list>
      </ion-fab>
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
import {parse as parseDate} from "date-fns";
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
