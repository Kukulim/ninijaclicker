<template>
  <div class="container">
    <div>
      <div><p>Stage: {{stage}}</p><p>World: {{world}}</p></div>
      <div>{{monster.health}}</div>
      <img src="../assets/logo.png" alt="" @click="hit()">
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
        this.monster.health = this.monster.basehealth + this.stage;
      }
      if(this.stage==10){
        this.monster.health=this.monster.bosshealth*this.monster.bosspower;
        this.monster.bosspower++;
        this.stage=1;
      }
    },
  }
}
</script>

<style></style>
