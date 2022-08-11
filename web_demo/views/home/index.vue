<template>
    <el-row class="home" :gutter="20">
        <el-col :span="8" style="margin-top:20px">
            <el-card shadow="hover">
                <div class="user">
                    <img :src="userImg" alt="" />
                    <div class="userinfo">
                        <p class="name">Admin</p>
                        <p class="access">超级管理员</p>
                    </div>
                </div>
                <el-divider></el-divider>
                <div class="login-info">
                    <p>上次登录的时间：<span>2021-7-19</span></p>
                    <p>上次登录的地点：<span>北京</span></p>
                </div>
            </el-card>
        </el-col>
        <el-col style="width:600px">
            <el-card style="margin-top:20px;height:460px;">
                <el-table :data="tableData">
                    <el-table-column 
                    v-for="(val,key) in tableLabel" 
                    :key="key" 
                    :prop="key"
                    :label="val"></el-table-column>
                </el-table>
            </el-card>
        </el-col>
        <el-col class="three" style="margin-top:20px;" :span="16">
            <div class="num">
                <el-card
                style="width:300px"
                v-for="item in countData"
                :key="item.name"
                :body-style="{display:'flex',padding:0}">
                    <i class="icon" :class="`el-icon-${item.icon}`" size="medium " :style="{background:item.color}"></i>
                    <div class="detail">
                        <p class="num1">{{item.value}}</p>
                        <p class="txt">{{item.name}}</p>
                    </div>
                </el-card>
            </div>
            <el-card style="height:280px;margin-top:20px">
                <!-- <div style="height:280px;" ref="echarts"></div> -->
                <echart :chartData="echartData.order" style="height:280px;width:95%" />
            </el-card>
            <div class="graph">
                <el-card style="height:260px;width:550px">
                    <!-- <div  style="height:240px;" ref="userEcharts"></div> -->
                    <echart :chartData="echartData.user" style="height:240px;" />
                </el-card>
                <el-card style="height:260px;width:550px">
                    <!-- <div  style="height:240px;" ref="videoEcharts"></div> -->
                    <echart :chartData="echartData.video" :isAxisChart="false" style="height:240px;" />
                </el-card>
            </div>
        </el-col>
    </el-row>
</template>

<script>
import { getData } from '../../api/data.js'
import * as echarts from 'echarts'
import Echart from '../../src/components/ECharts'

export default {
    name:'home',
    components:{
        Echart
    },
    data(){
        return{
            userImg:require('../../src/assets/image/1.jpg'),
            tableData:[
                // {
                //     name:"oppo",
                //     todayBuy:"100",
                //     monthBuy:"200",
                //     totalBuy:"300",
                // },
                // {
                //     name:"vivo",
                //     todayBuy:"100",
                //     monthBuy:"200",
                //     totalBuy:"300",
                // },
                // {
                //     name:"苹果",
                //     todayBuy:"100",
                //     monthBuy:"200",
                //     totalBuy:"300",
                // },
                // {
                //     name:"小米",
                //     todayBuy:"100",
                //     monthBuy:"200",
                //     totalBuy:"300",
                // },
                // {
                //     name:"三星",
                //     todayBuy:"100",
                //     monthBuy:"200",
                //     totalBuy:"300",
                // },
                // {
                //     name:"魅族",
                //     todayBuy:"100",
                //     monthBuy:"200",
                //     totalBuy:"300",
                // },
            ],
            tableLabel:{
                name:"课程",
                todayBuy:"今日购买",
                monthBuy:"本月购买",
                totalBuy:"总购买",
            },
            countData:[
                {
                    name:"今日支付订单",
                    value:"￥1234",
                    icon:"success",
                    color:"#2ec7c9",
                },
                {
                    name:"今日收藏订单",
                    value:"￥210",
                    icon:"star-on",
                    color:"#ffb980",
                },
                {
                    name:"今日未支付订单",
                    value:"￥1234",
                    icon:"s-goods",
                    color:"#5ab1ef",
                },
                {
                    name:"本月支付订单",
                    value:"￥1234",
                    icon:"success",
                    color:"#2ec7c9",
                },
                {
                    name:"本月收藏订单",
                    value:"￥210",
                    icon:"star-on",
                    color:"#ffb980",
                },
                {
                    name:"本月未支付订单",
                    value:"￥1234",
                    icon:"s-goods",
                    color:"#5ab1ef",
                },
            ],
            echartData:{
                order:{
                    xData:[],
                    series:[]
                },
                user:{
                    xData:[],
                    series:[]
                },
                video:{
                    series:[]
                }
            }
        }
    },
    mounted(){
        getData().then(res => {
            const {code,data} = res.data
            if(code == 20000){
                this.tableData = data.tableData;
                const order = data.orderData
                const xData = order.date
                const keyArray = Object.keys(order.data[0])
                const series = []
                keyArray.forEach(key => {
                    series.push({
                        name: key,
                        data: order.data.map(item => item[key]),
                        type: 'line'
                    })
                })
                // const option = {
                //     xAxis:{
                //         data: xData
                //     },
                //     yAxis:{ },
                //     legend:{
                //         data:keyArray
                //     },
                //     series
                // }
                this.echartData.order.xData = xData
                this.echartData.order.series = series   
                // const E = echarts.init(this.$refs.echarts)
                // E.setOption(option)
                
                
                //柱状图
                // const userOption = {
                //     legend:{
                //         textStyle:{
                //             color:'#333',
                //         },
                //     },
                //     grid:{
                //         left:"20%",
                //     },
                //     tooltip:{
                //         trigger:'axis',
                //     },
                //     xAxis:{
                //         type:'category',
                //         data:data.userData.map(item => item.data),
                //         axisLine:{
                //             lineStyle:{
                //                 color:'#333',
                //             },
                //         },
                //         axislabel:{
                //             interval:0,
                //             color:'#333',
                //         }
                //     },
                //     yAxis:[
                //         {
                //             type:'value',
                //             axisLine:{
                //                 lineStyle:{
                //                     color:'#17b3a3',
                //                 }
                //             }
                //         }
                //     ],
                //     color:['#2ec7c0','#b6a2de'],
                //     series:[
                //         {
                //             name:'新增用户',
                //             data:data.userData.map(item => item.new),
                //             type:'bar',
                //         },
                //         {
                //             name:'活跃用户',
                //             data:data.userData.map(item => item.active),
                //             type:'bar',
                //         }
                //     ],
                // }
                this.echartData.user.xData = data.userData.map(item => item.data);
                this.echartData.user.series = [
                    {
                        name:'新增用户',
                        data:data.userData.map(item => item.new),
                        type:'bar',
                    },
                    {
                        name:'活跃用户',
                        data:data.userData.map(item => item.active),
                        type:'bar',
                    }
                ];
                // const U = echarts.init(this.$refs.userEcharts)
                // U.setOption(userOption)


                //饼图
                // const videoOption = {
                //     tooltip:{
                //         trigger:'item'
                //     },
                //     color:[
                //         '#0f78f4',
                //         '#dd536b',
                //         '#9462e5',
                //         '#a6a6a6',
                //         '#e1bb22',
                //         '#39c326',
                //         '#3ed1cf',
                //     ],
                //     series:[
                //         {
                //             data:data.videoData,
                //             type:'pie'
                //         }
                //     ],
                // }
                // const V = echarts.init(this.$refs.videoEcharts)
                // V.setOption(videoOption)
                this.echartData.video.series = [
                    {
                        data:data.videoData,
                        type:'pie'
                    }
                ]
            }
            console.log(res);
        })
    }
}
</script>

<style lang="less" scoped>
.width(@px){  
    width: @px;
}
.home{
    height: 900px;
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    .width(1800px);
}
.user{
    display: flex;
    img{
        width: 100px;
        height: 100px;
        border-radius: 50%;
    }
    .userinfo{
        margin-left: 30px;
        .name{
            font-size: 20px;
            font-weight: 600;
        }
    }
}
.three{
    height: 200px;
    .num{
        width: 100%;
        height: 100%;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        justify-content: space-between;
        align-content: space-between;
        .detail{
            width: 200px;
            height: 75px;
            p{
                margin: 0 0 0 10px;
                padding: 0;
                height: 37.5px;
                line-height: 37.5px;
            }
            .num1{
                font-weight: 600;
            }
        }
        .icon{
            width: 75px;
            height: 75px;
            font-size: 30px;
            text-align: center;
            line-height: 75px;
        }
    }
}
.graph{
    margin-top:20px;
    display:flex;
    justify-content: space-between;
    align-content: space-between;
}
</style>