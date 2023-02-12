<script>
import { fighterProfiles } from "./data";
import Dialog from "./components/Dialog.vue";

export default {
  // components: [Dialog],
  data() {
    return {
      response: {
        factionName: "",
        fighterPoints: "",
        fighterName: "",
        fighterRole: "",
        selectedLeaders: [],
        selectedHeroes: [],
        selectedFighters: [],
        selectedThralls: [],
        selectedMonsters: [],
        selectedAllies: [],
      },
      fighterProfiles,
      selectedAlliance: "",
      // totalLeaderPoints: '',
      // totalHeroPoints: '',
      // totalFighterPoints: '',
      // totalThrallPoints: '',
      // totalAllyPoints: '',
      // totalMonsterPoints:'',
      factionButton: true,
      showFactionButton: false,
      showFaction: false,
      showWarbandsFighters: false,
      showLeaders: false,
      showLeaderList: false,
      showLeaderButton: true,
      showHeroes: false,
      showHeroList: false,
      showHeroButton: true,
      showFighters: false,
      showFighterList: false,
      showFighterButton: true,
      showThralls: false,
      showThrallList: false,
      showThrallButton: true,
      showAllies: false,
      showAllyList: false,
      showAllyButton: true,
      showMonsters: false,
      showMonsterList: false,
      showMonsterButton: true,
    };
  },
  computed: {
    factionRunemarks() {
      return [
        ...new Set(
          this.fighterProfiles
            .filter(
              (profile) =>
                profile.grandAlliance === this.selectedAlliance &&
                profile.factionRunemark !== "Chaos" &&
                profile.factionRunemark !== "Death" &&
                profile.factionRunemark !== "Destruction" &&
                profile.factionRunemark !== ""
            )
            .map((profile) => profile.factionRunemark)
        ),
      ];
    },
    factionHeroes() {
      return [
        ...new Set(
          this.fighterProfiles
            .filter(
              (profile) =>
                profile.factionRunemark === this.response.factionName &&
                profile.hero === "Y"
            )
            .map((profile) => {
              return {
                fighterName: profile.fighterName,
                fighterPoints: profile.fighterPoints,
              };
            })
        ),
      ];
    },
    factionFighters() {
      return [
        ...new Set(
          this.fighterProfiles
            .filter(
              (profile) =>
                profile.factionRunemark === this.response.factionName &&
                profile.hero !== "Y"
            )
            .map((profile) => {
              return {
                fighterName: profile.fighterName,
                fighterPoints: profile.fighterPoints,
              };
            })
        ),
      ];
    },
    factionThralls() {
      return [
        ...new Set(
          this.fighterProfiles
            .filter(
              (profile) =>
                profile.grandAlliance === this.selectedAlliance &&
                (profile.runemarkOne === "Thrall" ||
                  profile.runemarkTwo === "Thrall" ||
                  profile.runemarkThree === "Thrall")
            )
            .map((profile) => {
              return {
                fighterName: profile.fighterName,
                fighterPoints: profile.fighterPoints,
              };
            })
        ),
      ];
    },
    factionAllies() {
      return [
        ...new Set(
          this.fighterProfiles
            .filter(
              (profile) =>
                profile.grandAlliance === this.selectedAlliance &&
                profile.factionRunemark !== this.response.factionName &&
                profile.hero === "Y"
            )
            .map((profile) => {
              return {
                fighterName: profile.fighterName,
                fighterPoints: profile.fighterPoints,
              };
            })
        ),
      ];
    },
    factionMonsters() {
      return [
        ...new Set(
          this.fighterProfiles
            .filter(
              (profile) =>
                profile.grandAlliance === this.selectedAlliance &&
                (profile.runemarkOne === "Monster" ||
                  profile.runemarkTwo === "Monster" ||
                  profile.runemarkThree === "Monster")
            )
            .map((profile) => {
              return {
                fighterName: profile.fighterName,
                fighterPoints: profile.fighterPoints,
              };
            })
        ),
      ];
    },
    allFighters() {
      return [
        ...this.response.selectedLeaders,
        ...this.response.selectedHeroes,
        ...this.response.selectedFighters,
        ...this.response.selectedThralls,
        ...this.response.selectedMonsters,
      ];
    },
    totalPoints() {
      return this.allFighters?.reduce((acc, fighter) => acc + Number(fighter.fighterPoints), 0) || 0;
    },
  },
  // created() {
  //     this.totalFighterPoints = this.response.selectedLeaders.reduce((acc, fighterPoints) => acc + response.fighterPoints, 0);
  // },
  methods: {
    setAlliance(alliance) {
      this.selectedAlliance = alliance;
      this.showFactionButton = true;
    },
    setOrderAlliance(alliance) {
      this.selectedAlliance = alliance;
      this.showFactionButton = true;
      this.showThrallButton = false;
    },
    selectFaction(runemark) {
      this.response.factionName = runemark;
      this.showFaction = false;
      this.showWarbandsFighters = true;
    },
    chooseFaction() {
      this.factionButton = false;
      this.showFaction = true;
    },
    chooseLeader() {
      this.showLeaderList = true;
    },
    addLeaderProfile(fighterProfile) {
      this.response.selectedLeaders.push(fighterProfile);
      this.showLeaderList = false;
      this.showLeaderButton = false;
      console.log(this.response.selectedLeaders);
    },
    chooseHero() {
      this.showHeroList = true;
    },
    addHeroProfile(fighterProfile) {
      this.response.selectedHeroes.push(fighterProfile);
      this.showHeroList = false;
    },
    chooseFighter() {
      this.showFighterList = true;
    },
    addFighterProfile(fighterProfile) {
      this.response.selectedFighters.push(fighterProfile);
      this.showFighterList = false;
    },
    chooseThrall() {
      this.showThrallList = true;
    },
    addThrallProfile(fighterProfile) {
      this.response.selectedFighters.push(fighterProfile);
      this.showThrallList = false;
    },
    chooseAlly() {
      this.showAllyList = true;
    },
    addAllyProfile(fighterProfile) {
      this.response.selectedFighters.push(fighterProfile);
      this.showAllyList = false;
    },
    chooseMonster() {
      this.showMonsterList = true;
    },
    addMonsterProfile(fighterProfile) {
      this.response.selectedFighters.push(fighterProfile);
      this.showMonsterList = false;
    },
  },
  components: { Dialog },
};
</script>

<template>
  <div class="background"></div>
  <header></header>
  <main>
    <h1 class="warbands-title">WARBANDS</h1>
    <div class="warbands-workspace">
      <div class="warbands-name">
        <input
          class="warbands-name__input"
          type="text"
          value="Name Your Warband"
        />
      </div>
      <section class="warbands-alliance">
        <div class="button-container">
          <button
            class="button button-chaos"
            :aria-pressed="selectedAlliance === 'Grand Alliance Chaos'"
            @click="setAlliance('Grand Alliance Chaos')"
          >
            CHAOS
          </button>
          <button
            class="button button-death"
            :aria-pressed="selectedAlliance === 'Grand Alliance Death'"
            @click="setAlliance('Grand Alliance Death')"
          >
            DEATH
          </button>
          <button
            class="button button-destruction"
            :aria-pressed="selectedAlliance === 'Grand Alliance Destruction'"
            @click="setAlliance('Grand Alliance Destruction')"
          >
            DESTRUCTION
          </button>
          <button
            class="button button-order"
            :aria-pressed="selectedAlliance === 'Grand Alliance Order'"
            @click="setOrderAlliance('Grand Alliance Order')"
          >
            ORDER
          </button>
        </div>
      </section>
      <section v-if="showFactionButton" class="warbands-faction">
        <button
          v-if="factionButton"
          @click="chooseFaction"
          class="warbands-faction__button"
        >
          <img src="./assets/add icon.png" style="width: 1.5rem" /> Add Your
          Faction
        </button>
        <div class="warbands-faction__selected-faction">
          {{ response.factionName }}
        </div>

        <!-- FACTIONS DIALOG START-->
        <Dialog heading="Factions" :open="showFaction">
          <ul class="options-list">
            <li v-for="runemark in factionRunemarks" :key="runemark">
              <label>
                <input
                  type="radio"
                  :value="runemark"
                  name="runemark"
                  @click="selectFaction(runemark)"
                />
                {{ runemark }}
              </label>
            </li>
          </ul>
        </Dialog>
        <!-- DIALOG END-->
      </section>
      <section v-if="showWarbandsFighters" class="warbands-fighters">
        <!-- Leader and Heroes Section -->
        <h2>Leader and Heroes</h2>
        <!-- Leader Section -->
        <table
          v-for="leader in response.selectedLeaders"
          :key="leader.fighterName"
          id="leader-table"
          class="warbands-fighters__table"
        >
          <tr>
            <td class="warbands-fighters__table-points">
              {{ leader.fighterPoints }}
            </td>
            <td>
              <table style="margin: 0px">
                <tr>
                  <td class="warbands-fighters__table-name">
                    {{ leader.fighterName }}
                  </td>
                </tr>
                <tr>
                  <td class="warbands-fighters__table-role">
                    {{ leader.fighterRole }}
                    <button
                      class="button-battlegroups-shield"
                      style="margin-right: 2rem"
                    >
                      <span class="button-battlegroups-shield__content"></span>
                    </button>
                    <button class="button-battlegroups-hammer">
                      <span class="button-battlegroups-hammer__content"></span>
                    </button>
                    <button class="button-battlegroups-dagger">
                      <span class="button-battlegroups-dagger__content"></span>
                    </button>
                  </td>
                </tr>
              </table>
            </td>
            <td style="margin: 0px">
              <button class="button-functions-duplicate">
                <span class="button-functions-duplicate__content"></span>
              </button>
              <button class="button-functions-trash">
                <span class="button-functions-trash__content"></span>
              </button>
            </td>
          </tr>
        </table>
        <ul v-if="showLeaderButton">
          <li>
            <button class="warbands-fighters__button" @click="chooseLeader">
              <img src="./assets/add icon.png" style="width: 1rem" /> Click to
              Add Leader
            </button>
            <!-- DIALOG START-->
            <Dialog heading="Leaders" :open="showLeaderList">
              <ul class="options-list">
                <li
                  v-for="fighterProfile in factionHeroes"
                  :key="fighterProfile.fighterName"
                >
                  <input
                    type="radio"
                    :value="fighterProfile.fighterName"
                    name="fighterName"
                    @click="addLeaderProfile(fighterProfile)"
                  />
                  {{ fighterProfile.fighterName }}
                  {{ fighterProfile.fighterPoints }}
                </li>
              </ul>
            </Dialog>
            <!-- DIALOG END -->
          </li>
        </ul>
        <!-- Heroes Section-->
        <table
          v-for="hero in response.selectedHeroes"
          :key="hero.fighterName"
          id="hero-table"
          class="warbands-fighters__table"
        >
          <tr>
            <td class="warbands-fighters__table-points">
              {{ hero.fighterPoints }}
            </td>
            <td>
              <table style="margin: 0px">
                <tr>
                  <td class="warbands-fighters__table-name">
                    {{ hero.fighterName }}
                  </td>
                </tr>
                <tr>
                  <td class="warbands-fighters__table-role">
                    {{ hero.fighterRole }}
                    <button
                      class="button-battlegroups-shield"
                      style="margin-right: 2rem"
                    >
                      <span class="button-battlegroups-shield__content"></span>
                    </button>
                    <button class="button-battlegroups-hammer">
                      <span class="button-battlegroups-hammer__content"></span>
                    </button>
                    <button class="button-battlegroups-dagger">
                      <span class="button-battlegroups-dagger__content"></span>
                    </button>
                  </td>
                </tr>
              </table>
            </td>
            <td style="margin: 0px">
              <button class="button-functions-duplicate">
                <span class="button-functions-duplicate__content"></span>
              </button>
              <button class="button-functions-trash">
                <span class="button-functions-trash__content"></span>
              </button>
            </td>
          </tr>
        </table>
        <ul v-if="showHeroButton">
          <li>
            <button class="warbands-fighters__button" @click="chooseHero">
              <img src="./assets/add icon.png" style="width: 1rem" /> Click to
              Add Hero
            </button>
            <!--DIALOG START-->
            <div v-if="showHeroList" class="options-list">
              <ul>
                <li
                  v-for="fighterProfile in factionHeroes"
                  :key="fighterProfile.fighterName"
                >
                  <input
                    type="radio"
                    :value="fighterProfile.fighterName"
                    name="fighterName"
                    @click="addHeroProfile(fighterProfile)"
                  />
                  {{ fighterProfile.fighterName }}
                  {{ fighterProfile.fighterPoints }}
                </li>
              </ul>
            </div>
            <!--DIALOG END-->
          </li>
        </ul>
        <!-- FIGHTERS and THRALLS SECTION-->
        <section>
          <h2>Fighters and Thralls</h2>
          <!--FIGHTER SECTION-->
          <table
            v-for="fighter in response.selectedFighters"
            :key="fighter.fighterName"
            id="fighter-table"
            class="warbands-fighters__table"
          >
            <tr>
              <td class="warbands-fighters__table-points">
                {{ fighter.fighterPoints }}
              </td>
              <td>
                <table style="margin: 0px">
                  <tr>
                    <td class="warbands-fighters__table-name">
                      {{ fighter.fighterName }}
                    </td>
                  </tr>
                  <tr>
                    <td class="warbands-fighters__table-role">
                      {{ fighter.fighterRole }}
                      <button
                        class="button-battlegroups-shield"
                        style="margin-right: 2rem"
                      >
                        <span
                          class="button-battlegroups-shield__content"
                        ></span>
                      </button>
                      <button class="button-battlegroups-hammer">
                        <span
                          class="button-battlegroups-hammer__content"
                        ></span>
                      </button>
                      <button class="button-battlegroups-dagger">
                        <span
                          class="button-battlegroups-dagger__content"
                        ></span>
                      </button>
                    </td>
                  </tr>
                </table>
              </td>
              <td style="margin: 0px">
                <button class="button-functions-duplicate">
                  <span class="button-functions-duplicate__content"></span>
                </button>
                <button class="button-functions-trash">
                  <span class="button-functions-trash__content"></span>
                </button>
              </td>
            </tr>
          </table>
          <ul v-if="showFighterButton">
            <li>
              <button class="warbands-fighters__button" @click="chooseFighter">
                <img src="./assets/add icon.png" style="width: 1rem" /> Click to
                Add Fighter
              </button>
              <!--DIALOG START-->
              <div v-if="showFighterList" class="options-list">
                <ul>
                  <li
                    v-for="fighterProfile in factionFighters"
                    :key="fighterProfile.fighterName"
                  >
                    <input
                      type="radio"
                      :value="fighterProfile.fighterName"
                      name="fighterName"
                      @click="addFighterProfile(fighterProfile)"
                    />
                    {{ fighterProfile.fighterName }}
                    {{ fighterProfile.fighterPoints }}
                  </li>
                </ul>
              </div>
              <!-- DIALOG END-->
            </li>
          </ul>
          <!--THRALL SECTION-->
          <table
            v-for="thrall in response.selectedThralls"
            :key="thrall.fighterName"
            id="thrall-table"
            class="warbands-fighters__table"
          >
            <tr>
              <td class="warbands-fighters__table-points">
                {{ thrall.fighterPoints }}
              </td>
              <td>
                <table style="margin: 0px">
                  <tr>
                    <td class="warbands-fighters__table-name">
                      {{ thrall.fighterName }}
                    </td>
                  </tr>
                  <tr>
                    <td class="warbands-fighters__table-role">
                      {{ thrall.fighterRole }}
                      <button
                        class="button-battlegroups-shield"
                        style="margin-right: 2rem"
                      >
                        <span
                          class="button-battlegroups-shield__content"
                        ></span>
                      </button>
                      <button class="button-battlegroups-hammer">
                        <span
                          class="button-battlegroups-hammer__content"
                        ></span>
                      </button>
                      <button class="button-battlegroups-dagger">
                        <span
                          class="button-battlegroups-dagger__content"
                        ></span>
                      </button>
                    </td>
                  </tr>
                </table>
              </td>
              <td style="margin: 0px">
                <button class="button-functions-duplicate">
                  <span class="button-functions-duplicate__content"></span>
                </button>
                <button class="button-functions-trash">
                  <span class="button-functions-trash__content"></span>
                </button>
              </td>
            </tr>
          </table>
          <ul v-if="showThrallButton">
            <li>
              <button class="warbands-fighters__button" @click="chooseThrall">
                <img src="./assets/add icon.png" style="width: 1rem" /> Click to
                Add Thrall
              </button>
              <!-- DIALOG START-->
              <div v-if="showThrallList" class="options-list">
                <ul>
                  <li
                    v-for="fighterProfile in factionThralls"
                    :key="fighterProfile.fighterName"
                  >
                    <input
                      type="radio"
                      :value="fighterProfile.fighterName"
                      name="fighterName"
                      @click="addThrallProfile(fighterProfile)"
                    />
                    {{ fighterProfile.fighterName }}
                    {{ fighterProfile.fighterPoints }}
                  </li>
                </ul>
              </div>
              <!--DIALOG END-->
            </li>
          </ul>
        </section>
        <!--ALLIES AND MONSTERS-->
        <section>
          <h2>Allies and Monsters</h2>
          <!--ALLY SECTION-->
          <table
            v-for="ally in response.selectedAllies"
            :key="ally.fighterName"
            id="ally-table"
            class="warbands-fighters__table"
          >
            <tr>
              <td class="warbands-fighters__table-points">
                {{ ally.fighterPoints }}
              </td>
              <td>
                <table style="margin: 0px">
                  <tr>
                    <td class="warbands-fighters__table-name">
                      {{ ally.fighterName }}
                    </td>
                  </tr>
                  <tr>
                    <td class="warbands-fighters__table-role">
                      {{ ally.fighterRole }}
                      <button
                        class="button-battlegroups-shield"
                        style="margin-right: 2rem"
                      >
                        <span
                          class="button-battlegroups-shield__content"
                        ></span>
                      </button>
                      <button class="button-battlegroups-hammer">
                        <span
                          class="button-battlegroups-hammer__content"
                        ></span>
                      </button>
                      <button class="button-battlegroups-dagger">
                        <span
                          class="button-battlegroups-dagger__content"
                        ></span>
                      </button>
                    </td>
                  </tr>
                </table>
              </td>
              <td style="margin: 0px">
                <button class="button-functions-duplicate">
                  <span class="button-functions-duplicate__content"></span>
                </button>
                <button class="button-functions-trash">
                  <span class="button-functions-trash__content"></span>
                </button>
              </td>
            </tr>
          </table>
          <ul v-if="showAllyButton">
            <li>
              <button class="warbands-fighters__button" @click="chooseAlly">
                <img src="./assets/add icon.png" style="width: 1rem" /> Click to
                Add Ally
              </button>
              <!-- DIALOG START-->
              <div v-if="showAllyList" class="options-list">
                <ul>
                  <li
                    v-for="fighterProfile in factionAllies"
                    :key="fighterProfile.fighterName"
                  >
                    <input
                      type="radio"
                      :value="fighterProfile.fighterName"
                      name="fighterName"
                      @click="addAllyProfile(fighterProfile)"
                    />
                    {{ fighterProfile.fighterName }}
                    {{ fighterProfile.fighterPoints }}
                  </li>
                </ul>
              </div>
              <!-- DIALOG END-->
            </li>
          </ul>
          <!--MONSTER SECTION-->
          <table
            v-for="leader in response.selectedMonsters"
            :key="monster.fighterName"
            id="monster-table"
            class="warbands-fighters__table"
          >
            <tr>
              <td class="warbands-fighters__table-points">
                {{ monster.fighterPoints }}
              </td>
              <td>
                <table style="margin: 0px">
                  <tr>
                    <td class="warbands-fighters__table-name">
                      {{ monster.fighterName }}
                    </td>
                  </tr>
                  <tr>
                    <td class="warbands-fighters__table-role">
                      {{ monster.fighterRole }}
                      <button
                        class="button-battlegroups-shield"
                        style="margin-right: 2rem"
                      >
                        <span
                          class="button-battlegroups-shield__content"
                        ></span>
                      </button>
                      <button class="button-battlegroups-hammer">
                        <span
                          class="button-battlegroups-hammer__content"
                        ></span>
                      </button>
                      <button class="button-battlegroups-dagger">
                        <span
                          class="button-battlegroups-dagger__content"
                        ></span>
                      </button>
                    </td>
                  </tr>
                </table>
              </td>
              <td style="margin: 0px">
                <button class="button-functions-duplicate">
                  <span class="button-functions-duplicate__content"></span>
                </button>
                <button class="button-functions-trash">
                  <span class="button-functions-trash__content"></span>
                </button>
              </td>
            </tr>
          </table>
          <ul v-if="showMonsterButton">
            <li>
              <button class="warbands-fighters__button" @click="chooseMonster">
                <img src="./assets/add icon.png" style="width: 1rem" /> Click to
                Add Monster
              </button>
              <!--DIALOG START-->
              <div v-if="showMonsterList" class="options-list">
                <ul>
                  <li
                    v-for="fighterProfile in factionMonsters"
                    :key="fighterProfile.fighterName"
                  >
                    <input
                      type="radio"
                      :value="fighterProfile.fighterName"
                      name="fighterName"
                      @click="addMonsterProfile(fighterProfile)"
                    />
                    {{ fighterProfile.fighterName }}
                    {{ fighterProfile.fighterPoints }}
                  </li>
                </ul>
              </div>
              <!--DIALOG END-->
            </li>
          </ul>
        </section>
      </section>
      <table class="totals" style="margin-top: 3rem">
        <tr class="totals__stats">
          <td><b>TOTAL POINTS:</b> {{totalPoints}}/1000</td>
          <td><b>TOTAL FIGHTERS:</b> {{allFighters.length}}/13</td>
        </tr>
      </table>
      <table class="totals">
        <tr class="totals__rules-check">
          <td><b>LEADER:</b> {{response.selectedLeaders.length}}/1</td>
          <td><b>HEROES:</b> {{response.selectedHeroes.length}}/3</td>
          <td><b>ALLIES:</b> {{response.selectedAllies.length}}/3</td>
          <td><b>THRALLS:</b> {{response.selectedThralls.length}}/3</td>
          <td><b>MONSTER:</b> {{response.selectedMonsters.length}}/1</td>
        </tr>
      </table>
    </div>
  </main>
  <footer></footer>
</template>

<style>
label {
  cursor: pointer;
}

.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
