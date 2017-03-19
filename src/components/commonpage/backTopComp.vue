<template>
	  <div class="tip-menu">
	  	<div class="tip-nav" @click="openTip" v-bind:class="{ active: isActive }">
	  		<img src="../../assets/tip.svg"/>
	  	</div>
	  	<transition name="list-backTop">
	  		<span v-if="show" class="tips-backTop" @click="backTop">
		    	<img src="../../assets/backTop.svg" alt="">
	  		</span>
		  </transition>
		  <transition name="list-refresh">
				<span v-if="show" class="tips-refresh" @click="refresh">
		    	<img src="../../assets/refresh.svg" alt="">
	  		</span>
	  	</transition>
	  </div>	
</template>

<script>
  export default{
    data(){
      return {
      	show: false,
      	isActive: false,
      }
    },
    components: {

    },
		props:[
			'limit',
			'articleList'
		],
    methods: {
    	openTip() {
    		this.show = !this.show;
    		this.isActive = !this.isActive;
    	},
			backTop(){
				this.openTip();
        this.timer = setInterval(() => {
          let osTop = this.$parent.$refs.articleRef.scrollTop;
          let speed = Math.floor(-osTop / 6);
          this.$parent.$refs.articleRef.scrollTop = osTop + speed;
          if( osTop == 0){
            clearInterval(this.timer);
          };  
        }, 25)
      },
    	refresh() {
				this.backTop();
				let url = !this.$route.query.tab?'/topics?page=1&limit='+ this.limit :'/topics?page=1&limit='+ this.limit +'&tab='+ this.$route.query.tab
        this.$http.get(url)
	      .then((res)=>{
          this.$set(this.$parent.$data, 'articleList', res.data.data);
        })       
	      .catch((error)=>{
          console.log(error);
        });
			}
    }
  }
</script>

<style lang="less" scoped>
	.tip-menu {
		position: fixed;
		bottom: 40px;
		right: 10px;
		width: 40px;
		height: 40px;
		text-align: center;
		line-height: 40px;
		font-size: 40px;
		border-radius: 50%;
		border: 1px solid #80bd01;
		background: rgba(255,255,255,0.6);
		color: #80bd01;
		img {
			width: 100%;
		}
		p {
			display: block;
			width: 100%;
			height: 100%;
		}
		.tips-backTop, .tips-refresh {
			position: absolute;
			width: 40px;
			height: 40px;
			border-radius: 50%;
			background: rgba(255,255,255,0.6);
		}
		.tip-nav {
			height: 40px;
			transition: all .5s;
			transform-origin: center center; 	
		}
		.active {
			transform: rotateZ(45deg);
			background: none;
		}
		.tips-backTop {
			left: 0;
			top: -45px;
			
		}
		.tips-refresh {
			left: -45px;
			top: -25px;
		}
	}
	.list-backTop-enter-active {
	  transition: all .2s ease;
	}
	.list-backTop-leave-active {
	  transition: all 1s ease;
	}
	.list-backTop-enter, .list-backTop-leave-active {
	  transform: translateY(10px);
	  opacity: 0;
	}
	.list-refresh-enter-active {
	  transition: all 1s ease;
	}
	.list-refresh-leave-active {
	  transition: all .2s ease;
	}
	.list-refresh-enter, .list-refresh-leave-active {
	  transform: translateY(10px) translateX(10px);
	  opacity: 0;
	}
</style>
