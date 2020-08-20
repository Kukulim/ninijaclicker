<template>
  <div class="container">
    <div class="row">

      <div class="col"></div>  

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
        hitpower:1,
        weapon:2,
      },
      stage:1,
      world:1,
    };
  },
    methods:{
    hit(){
      this.monster.health-=(this.hero.hitpower+this.hero.weapon);
      if(this.monster.health <= 0){
        this.stage++;
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
    },
  }
}
</script>

<style></style>
