<template>
  <div class="container">
    <div class="row mt-5">
      <div class="col">
        <img src="../images/gold.png" alt="cap" class="goldimg" />
        <div>
          <div class="goldvalue mt-2">: {{ hero.gold }}</div>
        </div>
        <div class="mt-5">
          <img src="../images/armor/cap.png" alt="cap" class="capimg" />
          <img src="../images/armor/katana.png" alt="cap" class="katanaimg" />
          <img
            src="../images/armor/shuriken.png"
            alt="cap"
            class="shurikenimg"
          />
          <img src="../images/armor/sai.png" alt="cap" class="saiimg" />
          <img src="../images/armor/knife.png" alt="cap" class="knifeimg" />

          <img src="../images/hero.png" class="heroimg" />
          <p>hero lvl: {{ hero.lvl }}</p>
          <div class="proggresbar">
            <b-progress
              :value="hero.exp"
              :max="hero.maxlvlvalue"
              variant="success"
              class="mb-3"
            ></b-progress>
          </div>
          <p>skills:</p>
          <div class="row">
            <div class="col">
              <div @click="usefury()" :class="activefury()">
                <img
                  src="../images/skills/a.png"
                  alt=""
                  class="btn btn-info skillsbutton"
                />
              </div>
            </div>
            <div class="col">
              <div @click="usepowerhit()" :class="activepowerhit()">
                <img
                  src="../images/skills/b.png"
                  alt=""
                  class="btn btn-danger skillsbutton"
                />
              </div>
            </div>
            <div class="col">
              <div @click="useshurikenwind()" :class="activeshurikenwind()">
                <img
                  src="../images/skills/c.png"
                  alt=""
                  class="btn btn-success skillsbutton"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="col">
        <div>
          <p>Stage: {{ stage }} World: {{ world }}</p>
        </div>
        <div class="monsterhealthproggresbar mt-5">
          <b-progress
            :value="monster.health"
            :max="monster.currentmonsterhealth"
            variant="danger"
            class="mb-3"
          ></b-progress>
          <div v-show="elementVisible" class="hideElement hitpozition">
            -{{ monster.health - monster.health }}
          </div>
        </div>
        <img
          :src="require(`@/images/${monster.type[0]}.png`)"
          alt=""
          @click="hit()"
          class="monsterimg"
          v-bind:class="getClass()"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Ninija",
  data() {
    return {
      monster: {
        basehealth: 30,
        health: 30,
        currentmonsterhealth: 30,
        bosshealth: 30,
        bosspower: 2,
        worldbosspower: 2,
        type: ["A", "B", "C", "D", "E", "F", "G"],
      },
      hero: {
        lvl: 1,
        exp: 0,
        maxlvlvalue: 100,
        hitpower: 1,
        weapon: 2,
        gold: 0,
      },
      stage: 1,
      world: 1,
      elementVisible: false,
      skills: {
        fury: { dps: 2, isactive: false },
        powerhit: { dps: 50, isactive: false },
      },
    };
  },
  methods: {
    hit() {
      this.elementVisible = true;
      setTimeout(() => (this.elementVisible = false), 100);
      this.monster.health -=
        this.hero.hitpower + this.hero.weapon + 2 * this.hero.lvl;
      if (this.skills.fury.isactive == true) {
        this.monster.health -=
          (this.hero.hitpower + this.hero.weapon + 2 * this.hero.lvl) *
          this.skills.fury.dps;
      }
      if (this.skills.powerhit.isactive == true) {
        this.monster.health -= this.skills.powerhit.dps;
        this.skills.powerhit.isactive = false;
      }
      if (this.monster.health <= 0) {
        this.stage++;
        this.shuffleMosnsters();
        this.hero.gold = parseInt(1.1 * this.stage * this.world);
        this.hero.exp += 10;
        this.monster.health = this.monster.basehealth + this.stage * this.world;
        this.monster.currentmonsterhealth = this.monster.health;
        if (this.stage == 25 || this.stage == 50 || this.stage == 75) {
          this.monster.health =
            this.monster.bosshealth * this.monster.bosspower;
          this.monster.currentmonsterhealth = this.monster.health;
          this.monster.bosspower++;
        }
        if (this.stage == 100) {
          this.monster.health =
            this.monster.bosshealth *
            this.monster.bosspower *
            this.monster.worldbosspower;
          this.monster.currentmonsterhealth = this.monster.health;
          this.monster.worldbosspower++;
        }
      }
      if (this.stage == 101) {
        this.stage = 1;
        this.world++;
      }
      if (this.hero.exp >= this.hero.maxlvlvalue) {
        this.hero.exp = 0;
        this.hero.lvl++;
        this.hero.maxlvlvalue = parseInt(this.hero.maxlvlvalue * 1.1);
      }
    },
    usefury() {
      this.skills.fury.isactive = true;
      setTimeout(() => (this.skills.fury.isactive = false), 30000);
    },
    usepowerhit() {
      this.skills.powerhit.isactive = true;
    },
    useshurikenwind() {
      var myTimer = setInterval(() => this.hit(), 500);
      setTimeout(() => clearInterval(myTimer), 30000);
    },
    shuffleMosnsters() {
      for (let i = this.monster.type.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [this.monster.type[i], this.monster.type[j]] = [
          this.monster.type[j],
          this.monster.type[i],
        ];
      }
    },
    getClass() {
      if (this.stage == 25 || this.stage == 50 || this.stage == 75)
        return "miniboss";
      if (this.stage == 100) return "bigboss";
    },
    activefury() {
      if (this.hero.lvl >= 10) return "";
      else return "disableddiv";
    },
    activepowerhit() {
      if (this.hero.lvl >= 20) return "";
      else return "disableddiv";
    },
    activeshurikenwind() {
      if (this.hero.lvl >= 30) return "";
      else return "disableddiv";
    },
  },
};
</script>

<style></style>
