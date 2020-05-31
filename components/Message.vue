<template>
  <div>
    <div v-if="name === 'admin'" class="system">
      <p class="text-xs-center">{{text}}</p>
      <hr>
    </div>
    
    <div v-else class="wrap">
      <div class="message" :class="{owner}">
        <small>
          <strong>{{name}}</strong>
        </small>
        <p>{{text}}</p>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      name: String,
      text: String,
      owner: {
        type: Boolean,
        default: false
      }
    }
  }
</script>

<style lang="scss" scoped>
  .system {
    margin-bottom: .5rem;
    
    p {
      margin-bottom: 0;
    }
  }
  
  .wrap {
    display: flex;
    flex-direction: column;
    
    .message {
      padding: 1rem;
      width: 60%;
      margin: 0 1rem 1rem 0;
      box-shadow: 0 1px 0 0 rgba(50, 50, 50, .3);
      border-radius: 5px;
      background: #E8F5E9;
      color: #000;
      position: relative;
      
      p {
        margin-bottom: 0;
      }
    }
    .owner {
      background-color: #4CAF50;
      color: #fff;
      align-self: flex-end;
    }
  }
</style>
