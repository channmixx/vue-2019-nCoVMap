<template>
  <div class="map">
    <div id="chart" style="width:900px;height:500px;"></div>
  </div>
</template>

<script>
	import echarts from 'echarts'
	import jsonp from 'jsonp'
	import 'echarts/map/js/china'
	
	let option={
		title:{
			text:'vue实现疫情地图数据展示',
			x:'center',
			textStyle:{
				color:'#9C0505'
			}
		},
		tooltip:{
			trigger:'item',
			//a为系列名称  b为区域名称   c为合并数值
			formatter:'地区：{b}<br/>确诊{c}'
		},
		series:[
			{
				type:'map',
				map:'china',
				data:[
//					{name:'北京',value:200},
//					{name:'湖北',value:20000},
				],
				label:{
					show:true,
					color:'red',
					fontSize:10
				},
				zoom:1.25,
				itemStyle:{
					borderColor:'blue',
				},
				emphasis:{ //鼠标移入时
					label:{
						color:'white',
						fontSize:10
						
					},
					itemStyle:{
						areaColor:'green',
					}
				}
			}
		],
		visualMap:{ //数据可视化
			type:'piecewise',
			pieces: [
			    {min: 10000}, // 不指定 max，表示 max 为无限大（Infinity）。
			    {min: 1000, max: 9999},
			    {min: 100, max: 999},
			    {min: 10, max: 99},
			    {min: 1, max: 9},
			    {value: 0}
			],
			inRange:{
	            color: ['#fff', '#ffaa85', '#660208'],
	           
			},
			itemWidth:10,
			itemHeight:10
		},
		toolbox:{
        	feature:{
        		saveAsImage:{},
        		restore:{},
        		dataView:{}
        	}
        }
			
		
	};
	export default{
		data(){
			return{
				myChart:''
			}
		},
		mounted(){  //生命周期
			this.getData();
			// 基于准备好的dom，初始化echarts实例
			this.myChart=echarts.init(document.getElementById('chart'));
    
	        
		},
		methods:{//实时数据获取
			getData(){
				jsonp('http://interface.sina.cn/news/wap/fymap2020_data.d.json?_=1580892522427',(err,data)=>{	
					var lists=data.data.list.map(item=>{
						return{name:item.name,value:item.value}
					});
					console.log(lists);
					option.series[0].data=lists;
					// 使用刚指定的配置项和数据显示图表。
	        		this.myChart.setOption(option);
				})
			}
			
		}
	}
</script>

<style>
</style>