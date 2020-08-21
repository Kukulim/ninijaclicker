<template>
  <div class="container">
    <div class="row">

      <div class="col">
        <div><p>hero lvl: {{hero.lvl}} Gold: {{hero.gold}}</p></div>
        <b-progress :value="hero.exp" :max="hero.maxlvlvalue" class="mb-3"></b-progress>
</div>
<div>
        </div>
      <div class="col">
        <div><p>Stage: {{stage}} World: {{world}}</p></div>
        <div>health: {{monster.health}}</div>
        <img src="../assets/logo.png" alt="" @click="hit()">
      </div>

    </div>
    </div>
</template>

<script>
export default {
  name: 'Ninija',
  data() {
    return {
      monster:{
        basehealth:10,
        health:10,
        bosshealth:30,
        bosspower:2,
        worldbosspower:2
      },
      hero:{
        lvl:1,
        exp:0,
        maxlvlvalue:100,
        hitpower:1,
        weapon:2,
        gold:0,
      },
      stage:1,
      world:1,
    };
  },
    methods:{
    hit(){
      this.monster.health-=(this.hero.hitpower+this.hero.weapon+2*this.hero.lvl);
      if(this.monster.health <= 0){
        this.stage++;
        this.hero.gold++;
        this.hero.exp+=10;
        this.monster.health = this.monster.basehealth + this.stage*this.world;
        if(this.stage==25||this.stage==50||this.stage==75){
              this.monster.health=this.monster.bosshealth*this.monster.bosspower;
              this.monster.bosspower++;
              }
        if(this.stage==100){
            this.monster.health=this.monster.bosshealth*this.monster.bosspower*this.monster.worldbosspower;
            this.monster.worldbosspower++;
        }
      }
      if(this.stage==101){
        this.stage=1;
        this.world++;
      }
      if(this.hero.exp>=this.hero.maxlvlvalue){
        this.hero.exp = 0;
        this.hero.lvl++;
        this.hero.maxlvlvalue=parseInt(this.hero.maxlvlvalue*1.1)
      }
    },
  }
}
</script>

<style></style>
