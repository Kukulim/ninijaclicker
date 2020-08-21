<template>
  <div class="container">
    <div class="row mt-5">
      <div class="col">
        <div>
          <p>Gold: {{ hero.gold }}</p>
        </div>
        <img src="../images/hero.png" class="heroimg">
        <p>hero lvl: {{ hero.lvl }}</p>
        <div class="proggresbar">
        <b-progress
          :value="hero.exp"
          :max="hero.maxlvlvalue"
          variant='success'
          class="mb-3"
        ></b-progress>
        </div>
        <p>skills:</p>
      </div>
      <div></div>
      <div class="col">
        <div>
          <p>Stage: {{ stage }} World: {{ world }}</p>
        </div>
        <div class="monsterhealthproggresbar mt-5"> 
                <b-progress
          :value="monster.health"
          :max="monster.currentmonsterhealth"
          variant='danger'
          class="mb-3"
        ></b-progress>
        <div v-show="elementVisible" class="hideElement">-{{monster.health-monster.health}}</div>
        </div>
        <img src="../assets/logo.png" alt="" @click="hit()" class="monsterimg" />
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
        currentmonsterhealth:30,
        bosshealth: 30,
        bosspower: 2,
        worldbosspower: 2,
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
      elementVisible:false,
      skills:{
        powerhit:{dps:100, isactive:false},
      }
    };
  },
  methods: {
    hit() {
      this.elementVisible=true;
      setTimeout(() => this.elementVisible = false, 100)
      this.monster.health -=
        this.hero.hitpower + this.hero.weapon + 2 * this.hero.lvl;
        if(this.powerhit){
          this.monster.health -=(this.hero.hitpower + this.hero.weapon + 2 * this.hero.lvl)*this.powerhit.dps;
        }
      if (this.monster.health <= 0) {
        this.stage++;
        this.hero.gold=parseInt(1.1*this.stage*this.world);
        this.hero.exp += 10;
        this.monster.health = this.monster.basehealth + this.stage * this.world;
        this.monster.currentmonsterhealth=this.monster.health
        if (this.stage == 25 || this.stage == 50 || this.stage == 75) {
          this.monster.health =
            this.monster.bosshealth * this.monster.bosspower;
            this.monster.currentmonsterhealth=this.monster.health
          this.monster.bosspower++;
        }
        if (this.stage == 100) {
          this.monster.health =
            this.monster.bosshealth *
            this.monster.bosspower *
            this.monster.worldbosspower;
            this.monster.currentmonsterhealth=this.monster.health
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
  },
};
</script>

<style></style>
