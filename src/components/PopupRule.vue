<template>
  <div class="wp-popup" v-bind:class='getClassPopup'>
    <div class="rule">
      <h3>Luật chơi</h3>
      <p>
        Lorem ipsum dolor sit, amet consectetur adipisicing elit. Eveniet,
        provident.
      </p>
      <button v-on:click='closePopup' class="confirm">Đã hiểu</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "popup-rule",
  data() {
    return {
        
    }
  },
  methods:{
      closePopup(){
            this.$emit('handleClosePopup');
        }
  },
  props:{
      isOpenPopup:{type:Boolean,default:false}
  },
  computed:{
      getClassPopup(){
          return {
              'open-popup':this.isOpenPopup
          }
      }
  }
};
</script>

<style>
.wp-popup{
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    font-family: Arial, Helvetica, sans-serif;
    background-color: rgba(0,0,0,0.4);
    opacity: 0;
    visibility: hidden;
    transition: all .3s ease;
}
.wp-popup.open-popup{
    opacity: 1;
    visibility: visible;
}
.rule{
    position: absolute;
    background: #fff;
    width: 350px;
    padding: 20px;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%) scale(1.2);
    transition: all .3s ease;
}
.rule h3{
    margin-bottom: 10px;

}
.rule .confirm{
    border: 2px solid #333;
    background: #fff;
    padding: 8px 15px;
    margin-top: 20px;
    cursor: pointer;
    transition: all 0.2s;
}
.rule .confirm:hover{
    background: #333;
    color: #fff;
}
.open-popup .rule{
    transform: translate(-50%,-50%) scale(1.2);
}
</style>