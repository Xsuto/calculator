<template>
    <div class="frame">
        <button class="circle red" @click="exitApp"></button>
        <button class="circle yellow" @click="hide"></button>
        <button class="circle green" @click="resize"></button>
    </div>
</template>
<script>

export default {
    setup(){
    let key = 0

    window.addEventListener('keydown', (e) => {
      if(key === 18 && e.keyCode === 81){
        exitApp()
      }else{
          key = e.keyCode
      }
    })
    const hide = () => {
        try{
        const {remote} = window.require('electron')
        const win = remote.getCurrentWindow()
        win.minimize()
        }catch(err){
        console.log(err);
    }
        
    }
    const resize = () => {
        try{
        const {remote} = window.require('electron')
        const win = remote.getCurrentWindow();
        win.setSize(400,600)
                }catch(err){
        console.log(err);
    }
        
    }

    const exitApp = () =>  {
                try{
        const {remote} = window.require('electron')
        const win = remote.getCurrentWindow();
        win.close();
                        }catch(err){
        console.log(err);
    }
    };
    return {
        hide,resize,exitApp
    }
    }
}

</script>
<style lang="scss" scoped>
.frame{
    
  display:flex;
  justify-content: flex-start;
  align-items: center;
  background-color: rgba(0,0,0,0.3);
  -webkit-user-select: none;
  -webkit-app-region: drag;
  min-height: 5vh;


  button {
  -webkit-app-region: no-drag;
  width: 12px;
  height: 12px;
  border-radius: 50%;
  outline: none;
  border: none;
  margin-top: .5rem ;
  margin-left: 0.5rem;
  font-size: 12px;
  &:hover{
      filter: brightness(0.7);
  }

}
}
  .green {
      background-color: rgb(39,201,63);
  }
  .yellow {
      background-color: rgb(255,190,47);
  }
  .red {
      background-color: rgb(255,95,86);
  }
</style>