<script>
	// 项目的根组件
	export default {
		onLaunch() {
			if(process.env.NODE_ENV === 'development'){
				//做开发操作配置设置
			    console.log('开发环境')
			}else{
				// 做生产环境配置设置
			    console.log('生产环境')
			}
		}
	}
</script>

<style>
	/*每个页面公共css */
</style>
