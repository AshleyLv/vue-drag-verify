<template>
	<div class="drag_verify" :style="dragVerifyStyle" @mousemove="dragMoving" @mouseup="dragFinish" @touchmove="dragMoving" @touchend="dragFinish">
			
			<div class="dv_progress_bar" ref="progressBar" :style="progressBarStyle">
				
			</div>
			<div class="dv_text" :style="textStyle" ref="message">{{message}}</div>
			
			<div class="dv_handler dv_handler_bg" @mousedown="dragStart"  ref="handler" :style="handlerStyle" @touchstart="dragStart">
				<i :class="handlerIcon"></i>
			</div>

		</div>
</template>
<script>
	export default{
		name:'dragVerify',
	props:{
		width:{
			type:Number,
			default:200
		},
		height:{
			type:Number,
			default:60
		},
		text:{
			type:String,
			default:'swiping to the right side'
		},
		successText:{
			type:String,
			default:'success'
		},
		background:{
			type:String,
			default:'#ccc'
		},
		progressBarBg:{
			type:String,
			default:'#FFFF99'
		},
		completedBg:{
			type:String,
			default:'#66cc66'
		},
		circle:{
			type:Boolean,
			default: true
		},
		handlerIcon:{
			type:String

		},
		successIcon:{
			type:String
		},
		handlerBg:{
			type:String,
			default:'#fff'
		},
		textSize:{
			type:String,
			default:'20px'
		}

	},
	computed:{
		handlerStyle:function(){
			return {
				left: '0px',
				width : this.height-2+'px',
				height : this.height-2+'px',
				borderRadius: this.circle?'50%':0,
				background: this.handlerBg
			}
		},
		message : function(){
			return this.isPassing?this.successText:this.text
		},
		dragVerifyStyle:function(){
			return {
				width: this.width + 'px',
				height : this.height+'px',
				lineHeight: this.height+'px',
				background:this.background,
				borderRadius: this.circle?this.height/2+'px':0
			}
		},
		progressBarStyle: function(){
			return {
				background: this.progressBarBg,
				height : this.height+'px',
				borderRadius: this.circle?this.height/2+'px 0 0 ' + this.height/2+'px':0
			}
		},
		textStyle: function(){
			return {
				height : this.height+'px',
				width : this.width + 'px',
				fontSize:this.textSize
			}
		},
		handlerIconClass: function(){
			return this.isPassing?this.handlerIcon:this.successIcon
		}

	},
	data(){
		return{
			isMoving : false,
			x:0,
			isPassing : false
		}
	},
	mounted:function(){
		this.init()
	},
	methods:{
		init: function(){
			
		},
		dragStart: function(e){
			if(!this.isPassing) {
				this.isMoving = true;
				var handler = this.$refs.handler;
				this.x = (e.pageX||e.touches[0].pageX) - parseInt(handler.style.left.replace('px',''), 10);
			}
			
		},
		dragMoving: function(e){
            if(this.isMoving && !this.isPassing){
            	var _x = (e.pageX||e.touches[0].pageX) - this.x;
            	var handler = this.$refs.handler;
                if(_x > 0 && _x <= (this.width-this.height)){
                    handler.style.left = _x + 'px';
                    this.$refs.progressBar.style.width = (_x+this.height/2)+'px';
                }else if(_x > (this.width-this.height)){  
                	handler.style.left = (this.width - this.height)+ 'px';
                    this.$refs.progressBar.style.width = (this.width-this.height/2)+'px';
                     this.passVerify();
                }
            }

		},
		dragFinish: function(e){
			if(this.isMoving && !this.isPassing){
				var _x = (e.pageX || e.changedTouches[0].pageX)- this.x;
				if(_x <(this.width - this.height)){
					this.$refs.handler.style.left = '0';
					this.$refs.progressBar.style.width = '0';
				}
				this.isMoving = false;
			}
			
		},
		passVerify: function(){
           	this.isPassing = true;
           	this.isMoving = false;
           	var handler = this.$refs.handler;
           	handler.className += ' dv_handler_ok_bg';
           	handler.children[0].className = this.successIcon;
           	this.$refs.progressBar.style.background = this.completedBg;
           	this.$refs.message.style.color = '#fff';
           	this.$emit('passcallback');
		}
	}
	}
</script>
<style>
	.drag_verify{ 
        position: relative;
        background-color: #e8e8e8;
        text-align: center;

    }
    .drag_verify .dv_handler{
        position: absolute;
        top: 0px;
        left: 0px;
		border: 1px solid #ccc;
        cursor: move;
    }
    .drag_verify .dv_handler_bg{
       /* background: #fff url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA3hpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDIxIDc5LjE1NTc3MiwgMjAxNC8wMS8xMy0xOTo0NDowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDo0ZDhlNWY5My05NmI0LTRlNWQtOGFjYi03ZTY4OGYyMTU2ZTYiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6NTEyNTVEMURGMkVFMTFFNEI5NDBCMjQ2M0ExMDQ1OUYiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6NTEyNTVEMUNGMkVFMTFFNEI5NDBCMjQ2M0ExMDQ1OUYiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTQgKE1hY2ludG9zaCkiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDo2MTc5NzNmZS02OTQxLTQyOTYtYTIwNi02NDI2YTNkOWU5YmUiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6NGQ4ZTVmOTMtOTZiNC00ZTVkLThhY2ItN2U2ODhmMjE1NmU2Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+YiRG4AAAALFJREFUeNpi/P//PwMlgImBQkA9A+bOnfsIiBOxKcInh+yCaCDuByoswaIOpxwjciACFegBqZ1AvBSIS5OTk/8TkmNEjwWgQiUgtQuIjwAxUF3yX3xyGIEIFLwHpKyAWB+I1xGSwxULIGf9A7mQkBwTlhBXAFLHgPgqEAcTkmNCU6AL9d8WII4HOvk3ITkWJAXWUMlOoGQHmsE45ViQ2KuBuASoYC4Wf+OUYxz6mQkgwAAN9mIrUReCXgAAAABJRU5ErkJggg==") no-repeat center;*/
    }
    .drag_verify .dv_handler_ok_bg{
       /* background: #fff url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABAAAAAQCAYAAAAf8/9hAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAA3hpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNS1jMDIxIDc5LjE1NTc3MiwgMjAxNC8wMS8xMy0xOTo0NDowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wTU09Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9tbS8iIHhtbG5zOnN0UmVmPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvc1R5cGUvUmVzb3VyY2VSZWYjIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDo0ZDhlNWY5My05NmI0LTRlNWQtOGFjYi03ZTY4OGYyMTU2ZTYiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6NDlBRDI3NjVGMkQ2MTFFNEI5NDBCMjQ2M0ExMDQ1OUYiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6NDlBRDI3NjRGMkQ2MTFFNEI5NDBCMjQ2M0ExMDQ1OUYiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTQgKE1hY2ludG9zaCkiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDphNWEzMWNhMC1hYmViLTQxNWEtYTEwZS04Y2U5NzRlN2Q4YTEiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6NGQ4ZTVmOTMtOTZiNC00ZTVkLThhY2ItN2U2ODhmMjE1NmU2Ii8+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+k+sHwwAAASZJREFUeNpi/P//PwMyKD8uZw+kUoDYEYgloMIvgHg/EM/ptHx0EFk9I8wAoEZ+IDUPiIMY8IN1QJwENOgj3ACo5gNAbMBAHLgAxA4gQ5igAnNJ0MwAVTsX7IKyY7L2UNuJAf+AmAmJ78AEDTBiwGYg5gbifCSxFCZoaBMCy4A4GOjnH0D6DpK4IxNSVIHAfSDOAeLraJrjgJp/AwPbHMhejiQnwYRmUzNQ4VQgDQqXK0ia/0I17wJiPmQNTNBEAgMlQIWiQA2vgWw7QppBekGxsAjIiEUSBNnsBDWEAY9mEFgMMgBk00E0iZtA7AHEctDQ58MRuA6wlLgGFMoMpIG1QFeGwAIxGZo8GUhIysmwQGSAZgwHaEZhICIzOaBkJkqyM0CAAQDGx279Jf50AAAAAABJRU5ErkJggg==") no-repeat center;*/
    }
    .drag_verify .dv_handler i{
    	color: #666;
    	font-size: 1.5em;
    }
    .drag_verify .dv_progress_bar{
        position: absolute;
        height: 34px;
        width: 0px;
        transition: background 2s ease-in;
    }
    .drag_verify .dv_text{
        position: absolute;
        top: 0px;
        color:#444;
        -moz-user-select: none;
        -webkit-user-select: none;
        user-select: none;
        -o-user-select:none;
        -ms-user-select:none;
    }
</style>