<template>
  <div class="container">
    <!--HERO-->

    <div class="row mt-5">
      <div class="col">
        <img src="../images/gold.png" alt="cap" class="goldimg" />
        <div>
          <div class="goldvalue mt-2">: {{ hero.gold }}</div>
        </div>
        <div class="mt-5">
          <img
            src="../images/armor/cap.png"
            alt="cap"
            class="capimg"
            @click="buycap()"
            v-bind:class="[
              { opaticyimgbuyed: equipment.cap.status },
              'opaticyimg', getrarity(this.equipment.cap)
            ]"
          />
          <div class="capequipnemtvalue">
            <span>cost: {{ equipment.cap.cost }}</span
            ><br />
            <span>lvl: {{ equipment.cap.lvl }}</span
            ><br />
            <span>dps: {{ equipment.cap.dps }}</span>
          </div>
          <img
            src="../images/armor/katana.png"
            alt="cap"
            class="katanaimg opaticyimg"
            @click="buykatana()"
            v-bind:class="[
              { opaticyimgbuyed: equipment.katana.status },
              'opaticyimg', getrarity(this.equipment.katana)
            ]"
          />
          <div class="katanaequipnemtvalue">
            <span>cost: {{ equipment.katana.cost }}</span
            ><br />
            <span>lvl: {{ equipment.katana.lvl }}</span
            ><br />
            <span>dps: {{ equipment.katana.dps }}</span>
          </div>
          <img
            src="../images/armor/shuriken.png"
            alt="cap"
            class="shurikenimg opaticyimg"
            @click="buyshuriken()"
            v-bind:class="[
              { opaticyimgbuyed: equipment.shuriken.status },
              'opaticyimg', getrarity(this.equipment.shuriken)
            ]"
          />
          <div class="shurikenequipnemtvalue">
            <span>cost: {{ equipment.shuriken.cost }}</span
            ><br />
            <span>lvl: {{ equipment.shuriken.lvl }}</span
            ><br />
            <span>dps: {{ equipment.shuriken.dps }}</span>
          </div>
          <img
            src="../images/armor/sai.png"
            alt="cap"
            class="saiimg opaticyimg"
            @click="buysai()"
            v-bind:class="[
              { opaticyimgbuyed: equipment.sai.status },
              'opaticyimg', getrarity(this.equipment.sai)
            ]"
          />
          <div class="saiequipnemtvalue">
            <span>cost: {{ equipment.sai.cost }}</span
            ><br />
            <span>lvl: {{ equipment.sai.lvl }}</span
            ><br />
            <span>dps: {{ equipment.sai.dps }}</span>
          </div>
          <img
            src="../images/armor/knife.png"
            alt="cap"
            class="knifeimg opaticyimg"
            @click="buyknife()"
            v-bind:class="[
              { opaticyimgbuyed: equipment.knife.status },
              'opaticyimg',getrarity(this.equipment.knife)
            ]"
          />
          <div class="knifeequipnemtvalue">
            <span>cost: {{ equipment.knife.cost }}</span
            ><br />
            <span>lvl: {{ equipment.knife.lvl }}</span
            ><br />
            <span>dps: {{ equipment.knife.dps }}</span>
          </div>

          <img src="../images/hero.png" class="heroimg" />

          <div v-show="skills.fury.isactive">
                <img
                  src="../images/skills/a.png"
                  alt=""
                  class="btn btn-info btnfurymin disableddiv"
                />
              </div>
          <div v-show="skills.powerhit.isactive">
                <img
                  src="../images/skills/b.png"
                  alt=""
                  class="btn btn-danger btnpowermin disableddiv"
                />
              </div>
          <div v-show="skills.shurikenwind.isactive">
                <img
                  src="../images/skills/c.png"
                  alt=""
                  class="btn btn-success btnwindmin disableddiv"
                />
          </div>

          <p>LVL: {{ hero.lvl }}</p>
          <div class="proggresbar">
            <b-progress
              :value="hero.exp"
              :max="hero.maxlvlvalue"
              variant="success"
              class="mb-3"
            ></b-progress>
          </div>

          <!--SKILLS-->

          <p>SKILLS:</p>
          <div class="row">

            <!-- FURY -->

            <div class="col">
              <div @click="usefury()" :class="activefury()">
                <img
                  src="../images/skills/a.png"
                  alt=""
                  class="btn btn-info skillsbutton"
                />
              </div>
              <div class="dropdown">
                <p>FURY</p>
                <div class="dropdown-content">
                  <span>5 lvl to unlock.                    
                  </span><br>
                  <span> multiple dps for 30s. </span>
                </div>
              </div>
              <circular-count-down-timer
                :initial-value=1
                :steps="300"
                :size="75"
                :second-label="''"
                :seconds-stroke-color="'#01525f'"
                :paused=skills.fury.cdtimer
                @update="updatedfury"
                ref="furytimer"
                v-show="skills.fury.cdtimer==false"
                class="furytimer"
              ></circular-count-down-timer>
            </div>

            <!-- POWERHIT -->

            <div class="col">
              <div @click="usepowerhit()" :class="activepowerhit()">
                <img
                  src="../images/skills/b.png"
                  alt=""
                  class="btn btn-danger skillsbutton"
                />
              </div>
              <div class="dropdown">
                <p>POWER HIT</p>
                <div class="dropdown-content">
                  <span>10 lvl to unlock.                    
                  </span><br>
                  <span>another hit + 50 dps</span>
                </div>
              </div>
              <circular-count-down-timer
                :initial-value=1
                :steps="30"
                :size="75"
                :second-label="''"
                :seconds-stroke-color="'#ad1725'"
                :paused=skills.powerhit.cdtimer
                @update="updatedpowerhit"
                ref="powerhittimer"
                v-show="skills.powerhit.cdtimer==false"
                class="powerhittimer"
              ></circular-count-down-timer>
            </div>

            <!-- SHURIKEN WIND -->

            <div class="col">
              <div @click="useshurikenwind()" :class="activeshurikenwind()">
                <img
                  src="../images/skills/c.png"
                  alt=""
                  class="btn btn-success skillsbutton"
                />
              </div>
              <div class="dropdown">
                <p>SHURIKEN WIND</p>
                <div class="dropdown-content">
                  <span>20 lvl to unlock.                    
                  </span><br>
                  <span>auto attacks for 20s.</span>
                </div>
              </div>
                            <circular-count-down-timer
                :initial-value=1
                :steps="180"
                :size="75"
                :second-label="''"
                :seconds-stroke-color="'#0d6922'"
                :paused=skills.shurikenwind.cdtimer
                @update="updatedshurikenwind"
                ref="shurikenwind"
                v-show="skills.shurikenwind.cdtimer==false"
                class="shurikenwindtimer"
              ></circular-count-down-timer>
            </div>
          </div>
        </div>
      </div>

      <!--Monster Side-->

      <div class="col">
        <div class="worldstats">
          <p class="worldstats">STAGE: {{ stage }} WORLD: {{ world }}</p>
        </div>
        <div class="heroimg">


        <div class="monsterhealthproggresbar mt-5">
          <b-progress
            :value="monster.health"
            :max="monster.currentmonsterhealth"
            variant="danger"
            class="mb-3"
          ></b-progress>
          <div v-show="elementVisible" class="hideElement hitpozition">
            -{{ hero.hitpower }}
          </div>
        </div>
        <img
          :src="require(`@/images/zombies/${monster.type[0]}.png`)"
          alt=""
          @click="hit()"
          class="monsterimg"
          v-bind:class="getClass()"
        />
        
        </div>
        <div v-if="getClass()=='miniboss'" class="bosswarningtext text-danger">BOSS STAGE !!!</div>
        <div v-if="getClass()=='bigboss'" class="bosswarningtext text-danger">WORLD BOSS STAGE !!!</div>
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
        basehealth: 180,
        health: 180,
        currentmonsterhealth: 180,
        type: ["A", "B", "C", "D", "E", "F", "G"],
      },
      hero: {
        lvl: 1,
        exp: 0,
        maxlvlvalue: 100,
        gold: 0,
        hitpower: 0,
      },
      skills: {
        fury: { dps: 2, isactive: false, cooldown:false, cdtimer:true},
        powerhit: { dps: 150, isactive: false, cooldown:false, cdtimer:true },
        shurikenwind:{isactive: false, cooldown:false, cdtimer:true}
      },
      equipment: {
        cap: { lvl: 0, dps: 1, cost: 5, status: false },
        katana: { lvl: 0, dps: 2, cost: 10, status: false },
        shuriken: { lvl: 0, dps: 3, cost: 20, status: false },
        sai: { lvl: 0, dps: 4, cost: 30, status: false },
        knife: { lvl: 0, dps: 5, cost: 50, status: false },
      },
      stage: 1,
      world: 1,
      elementVisible: false,
    };
  },
  methods: {
    hit() {
      this.elementVisible = true;
      setTimeout(() => (this.elementVisible = false), 100);

      var random = Math.floor(Math.random() * (6));;

      this.hero.hitpower =
        ((this.hero.lvl + this.hero.lvl) * 3) +
        this.equipment.cap.dps +
        this.equipment.katana.dps +
        this.equipment.shuriken.dps +
        this.equipment.sai.dps +
        this.equipment.knife.dps-4+
        random;

      this.monster.health -= this.hero.hitpower;
      if (this.skills.fury.isactive == true) {
        this.hero.hitpower *= this.skills.fury.dps;
        this.monster.health -= this.hero.hitpower;
      }
      if (this.skills.powerhit.isactive == true) {
        this.hero.hitpower += this.skills.powerhit.dps * this.hero.lvl;
        this.monster.health -= this.hero.hitpower;
        this.skills.powerhit.isactive = false;
      }
      if (this.monster.health <= 0) {
        this.stage++;
        this.shuffleMosnsters();
        this.hero.gold += parseInt(1.1 * this.stage * this.world * this.world)-3+random;
        if(this.hero.gold<=0)this.hero.gold+=1;
        this.hero.exp += 10;

        this.monster.basehealth += this.stage * this.world * 2;
        this.monster.health = this.monster.basehealth;

        if (this.stage == 25 || this.stage == 50 || this.stage == 75) {
          this.monster.health *=3;
        }
        if (this.stage == 100) {
          this.monster.health *=6;
        }
        this.monster.currentmonsterhealth = this.monster.health
      }
      if (this.stage == 101) {
        this.monster.basehealth = this.monster.basehealth/2;
        this.monster.health = this.monster.basehealth;
        this.monster.currentmonsterhealth = this.monster.health
        this.hero.gold += parseInt(5 * this.stage * this.world);
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
      if(this.skills.fury.cooldown==false)
      {
        this.skills.fury.isactive = true;
        this.skills.fury.cooldown = true;
        this.$refs.furytimer.updateTime(299);
        setTimeout(() => (this.skills.fury.isactive = false), 30000);
        setTimeout(() => (this.skills.fury.cooldown = false), 300000);
      }
    },    
    updatedfury: function() {
      if (this.$refs.furytimer.value == 1) {
        this.skills.fury.cdtimer = true;
      } else {
        this.skills.fury.cdtimer = false;
      }
    },
    usepowerhit() {
      if(this.skills.powerhit.cooldown==false)
      {
        this.skills.powerhit.cooldown = true;
        this.skills.powerhit.isactive = true;
        this.$refs.powerhittimer.updateTime(29);
        setTimeout(() => (this.skills.powerhit.cooldown = false),30000);
      }
    },
    updatedpowerhit(){
      if (this.$refs.powerhittimer.value == 1) {
        this.skills.powerhit.cdtimer = true;
      } else {
        this.skills.powerhit.cdtimer = false;
      }
    },
    useshurikenwind() {
      if(this.skills.shurikenwind.cooldown==false)
      {
        this.skills.shurikenwind.isactive=true;
        this.skills.shurikenwind.cooldown=true;
        this.$refs.shurikenwind.updateTime(179);
        var myTimer = setInterval(() => this.hit(), 500);
        setTimeout(() => clearInterval(myTimer), 20000);
        setTimeout(() => this.skills.shurikenwind.isactive=false, 20000);
        setTimeout(() => (this.skills.shurikenwind.cooldown = false),180000);
      }
    },
    updatedshurikenwind(){
      if (this.$refs.shurikenwind.value == 1) {
        this.skills.shurikenwind.cdtimer = true;
      } else {
        this.skills.shurikenwind.cdtimer = false;
      }
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
      if (this.hero.lvl >= 5) return "";
      else return "disableddiv";
    },
    activepowerhit() {
      if (this.hero.lvl >= 10) return "";
      else return "disableddiv";
    },
    activeshurikenwind() {
      if (this.hero.lvl >= 20) return "";
      else return "disableddiv";
    },
    buycap() {
      if (this.hero.gold >= this.equipment.cap.cost) {
        this.hero.gold -= this.equipment.cap.cost;
        this.equipment.cap.dps += 2 +this.equipment.cap.lvl++;
        this.equipment.cap.cost += this.equipment.cap.dps;
        this.equipment.cap.status = true;
      }
    },
    buykatana() {
      if (this.hero.gold >= this.equipment.katana.cost) {
        this.hero.gold -= this.equipment.katana.cost;
        this.equipment.katana.dps += 4 + this.equipment.katana.lvl++;
        this.equipment.katana.cost += this.equipment.katana.dps;
        this.equipment.katana.status = true;
      }
    },
    buyshuriken() {
      if (this.hero.gold >= this.equipment.shuriken.cost) {
        this.hero.gold -= this.equipment.shuriken.cost;
        this.equipment.shuriken.dps += 6 + this.equipment.shuriken.lvl++;
        this.equipment.shuriken.cost += this.equipment.shuriken.dps;
        this.equipment.shuriken.status = true;
      }
    },
    buysai() {
      if (this.hero.gold >= this.equipment.sai.cost) {
        this.hero.gold -= this.equipment.sai.cost;
        this.equipment.sai.dps += 8 + this.equipment.sai.lvl++;
        this.equipment.sai.cost += this.equipment.sai.dps;
        this.equipment.sai.status = true;
      }
    },
    buyknife() {
      if (this.hero.gold >= this.equipment.knife.cost) {
        this.hero.gold -= this.equipment.knife.cost;
        this.equipment.knife.dps += 12 + this.equipment.knife.lvl++;;
        this.equipment.knife.cost += this.equipment.knife.dps;
        this.equipment.knife.status = true;
      }
    },
    getrarity(item){
      if(item.lvl>=0&&item.lvl<10) return "junkitem";
      if(item.lvl>=10&&item.lvl<20) return "magicitem";
      if(item.lvl>=20&&item.lvl<30) return "rareitem";
      if(item.lvl>=30&&item.lvl<60) return "uniqueitem";
      if(item.lvl>=60) return "legenditem";
    }
  },
};
</script>

<style></style>
