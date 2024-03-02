<template>
    <div>
        <h1>Questões sobre tecnologia em Inglês</h1>
        <section class="score">
              Jogador <span>{{this.winCount}}</span> x <span>{{this.loseCount}}</span> Computador
        </section>
    </div>

</template>
<script>
export default{
    props: ['winCount', 'loseCount']
}
</script>
<style lang="scss">
.score{
    border-bottom: 1px solid #2c3e50;
    padding: 16px 32px;

    span{
      border: 1px solid rgb(0,0,0);
      padding: 8px;
    }
  }
</style>