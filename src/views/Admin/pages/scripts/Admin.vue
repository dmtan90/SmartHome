<template lang="pug">
.script-admin-component
	.scripts-list
		el-table.scripts-table(v-if="scripts.length!==0" :data="scripts" :row-class-name="disabledRowClass" stripe :height="scripts.length<5?420:470")
			el-table-column(prop="showName" label="电器" width="78")
			el-table-column(prop="showStatus" label="状态" width="60")
			el-table-column(prop="showBrightness" label="亮度" width="70")
			el-table-column(prop="color" label="颜色" width="70")
			el-table-column(prop="showCodition" label="触发条件")
			el-table-column(prop="showDuration" label="触发时间" width="115")
			el-table-column(fixed="right" label="操作" width="75")
				template(slot-scope="scope")
					el-button.script-disabled-button(@click.native.prevent="disableScript(scope.$index)" type="primary" size="mini")
						| {{scripts[scope.$index].disabled?'启用':'禁用'}}
					el-button.script-delete-button(@click.native.prevent="deleteScript(scope.$index)" type="danger" size="mini") 删除
		.when-script-is-null(v-else :span="24" type="flex" align="middle" justify="center")
			el-row
				| 目前暂无指令哦
			.script-add-button(:span="24" type="flex" align="middle" justify="center")
				el-button(type="success" @click.native.prevent="$router.push({name:'ScriptsAdd'})" round) 增加指令
	.script-admin-tip(v-show="scripts.length<5" :span="24" type="flex" align="middle" justify="center")
			textra.tip(:data="words" :timer="1.1" :sequence="true" :infinite="true")
</template>

<script>
import {Component,Vue} from 'vue-property-decorator'
import {mapState,mapActions} from 'vuex'
import notice from '@/utils/ui/notice'

@Component({
	computed:{
		...mapState('scripts',['scripts'])
	},
	methods:{
		...mapActions('scripts',['disableScriptByIndex','deleteScriptByIndex','addScript'])
	}
})
export default class ScriptsAdmin extends Vue{
	words = ['设置触发条件，将自动执行您的指令操作。让您的生活更舒适。','比如：下班之后时间开灯、阴天自动开灯、根据睡觉时间，自动关灯等。']
	disabledRowClass({row,rowIndex}){
		if(this.scripts[rowIndex].disabled){
			return 'disabled-row'
		}
	}
	disableScript(index){
		this.disableScriptByIndex(index).then(e=>{
			if(e.success){
				notice.success(e.message,'成功')
			}else{
				notice.error(e.message,'失败')
			}
		})
	}
	deleteScript(index){
		this.deleteScriptByIndex(index).then(e=>{
			if(e.success){
				notice.success(e.message,'成功')
			}else{
				notice.error(e.message,'失败')
			}
		})
	}

}
</script>

<style lang="stylus">
.script-admin-component
	font-beautify()
.script-admin-tip
	position fixed
	font-size .8em
	bottom 60px
	z-index 333
.disabled-row
	opacity .8
	color transparent
	text-shadow 0 0 1px #666
	cursor not-allowed
.when-script-is-null
	margin 150px auto 50px
.scripts-list
	padding-top 15px
.script-add-button
	margin-top 40px
.script-delete-button
	transform translateX(-10px) translateY(2px) !important
	border-radius 0px !important
.script-disabled-button
	transform translateY(-2px) !important
	border-radius 0px !important
.scripts-table
	box-shadow 0 2px 10px #ccc
	border-radius 5px
	background-color transparent !important
</style>
