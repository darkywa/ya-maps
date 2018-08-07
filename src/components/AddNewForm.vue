<template lang="pug">
	
	a(@click.prevent="addNewForm" href="#0" id="addNewForm") +

</template>

<script>
	
	export default {
		methods: {
			// регистрируем событие на добавление новой формы
			addNewForm: function() {
				this.$emit('newform');
			}
		}
	}

</script>

<style lang="less">
	
	#addNewForm {
	    position: absolute;
	    z-index: 100;
	    top: 0;
	    left: -50px;
	    background: #5cb85c;
	    color: #fff;
	    text-decoration: none;
	    display: flex;
	    text-align: center;
	    border-radius: 50%;
	    font-size: 25px;
	    width: 37px;
	    height: 37px;
	    justify-content: center;
	    align-items: center;
	    transition: all .5s;
		
		&:hover {
	    	opacity: .8;
	    }
	}

	@media screen and (max-width: 960px) {
		#addNewForm {
		    position: fixed;
		    bottom: 50px;
		    right: 50px;
		    top: auto;
		    left: auto;
		    width: 50px;
		    height: 50px;
		    font-size: 30px;
		    box-shadow: 0px 2px 5px #bbb;
		}
	}

</style>