<template>
  <div class="pos">
    <el-row>
      <el-col :span='7' class="pos-order" id="order">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data=tableData border style="width:100%">
              <el-table-column label="商品名称" prop="goodsName"></el-table-column>
              <el-table-column label="数量" prop="count" width="50"></el-table-column>
              <el-table-column label="单价" prop="price" width="70"></el-table-column>
              <el-table-column label="操作" fixed="right" width="100">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">添加</el-button>
                  <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="total">
              <span>总数量：{{totalCount}}</span>
              <span>总价：￥{{totalMoney}}</span>
            </div>
            <div class="div-btn">
              <!-- <el-button type="warning">挂单</el-button> -->
              <el-button type="danger" @click="delAllGoods">删除</el-button>
              <el-button type="success" @click="pay">结账</el-button>
            </div>          
          </el-tab-pane>
          <el-tab-pane label="订单">
            <el-table :data=orderForm border style="width:100%">
              <el-table-column label="订单号" prop="orderNumber" width="100"></el-table-column>
              <el-table-column label="取餐号" prop="goodNumber" width="100"></el-table-column>
              <el-table-column label="金额" prop="money"></el-table-column>            
            </el-table>
          </el-tab-pane>
          <el-tab-pane label="配送">
            <div class="input">
              <span>收件人姓名：</span><el-input v-model="input" placeholder="请输入收件人姓名"></el-input>
              <span>收件地址：</span><el-input v-model="adress" placeholder="请输入收件地址"></el-input>
              <div class="btn-ipt"><el-button type="primary" @click="submitMessage" round>确认</el-button></div>
              
            </div>
                             
          </el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span='17'>
        <div class="often-goods">
          <div class="title">热卖商品</div>
          <div class="often-goods-list">
            <ul>
              <li v-for="goods in oftenGoods" @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">￥{{goods.price}}</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                    <img :src="goods.goodsImg" alt="">
                    <div class="nameandprice">
                      <p class="foodName">{{goods.goodsName}}</p>
                      <span class="foodPrice">￥{{goods.price}}</span>
                    </div>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                    <img :src="goods.goodsImg" alt="">
                    <div class="nameandprice">
                      <p class="foodName">{{goods.goodsName}}</p>
                      <span class="foodPrice">￥{{goods.price}}</span>
                    </div>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                    <img :src="goods.goodsImg" alt="">
                    <div class="nameandprice">
                      <p class="foodName">{{goods.goodsName}}</p>
                      <span class="foodPrice">￥{{goods.price}}</span>
                    </div>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                    <img :src="goods.goodsImg" alt="">
                    <div class="nameandprice">
                      <p class="foodName">{{goods.goodsName}}</p>
                      <span class="foodPrice">￥{{goods.price}}</span>
                    </div>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "Pos",
  data() {
    return {
      tableData: [],
      oftenGoods:[],
      type0Goods: [{
              goodsId:1,
              goodsImg:"https://img.4008823823.com.cn/kfcios/Version/614_607625.jpg",
              goodsName:'香辣鸡腿堡',
              price:18
          }, {
              goodsId:2,
              goodsImg:"https://img.4008823823.com.cn/kfcios/Version/614_607300.jpg",
              goodsName:'田园鸡腿堡',
              price:15
          }, {
              goodsId:3,
              goodsImg:"https://img.4008823823.com.cn/kfcios/Version/604_585900.jpg",
              goodsName:'和风汉堡',
              price:15
          }],
      type1Goods: [{
              goodsId:4,
              goodsImg:"https://img.4008823823.com.cn/kfcios/Version/604_585895.jpg",
              goodsName:'大包薯条',
              price:18
          }, {
              goodsId:5,
              goodsImg:"https://img.4008823823.com.cn/kfcios/Version/604_585872.jpg",
              goodsName:'脆皮炸鸡腿',
              price:20
          }, {
              goodsId:6,
              goodsImg:"https://img.4008823823.com.cn/kfcios/Version/604_581380.jpg",
              goodsName:'魔法鸡块',
              price:20
          }],
      type2Goods: [{
              goodsId:7,
              goodsImg:"https://img.4008823823.com.cn/kfcios/Version/604_583035.jpg",
              goodsName:'可乐大杯',
              price:10
          }, {
              goodsId:8,
              goodsImg:"https://img.4008823823.com.cn/kfcios/Version/630_652494.jpg",
              goodsName:'雪顶咖啡',
              price:18
          }],
      type3Goods: [{
              goodsId:9,
              goodsImg:"https://img.4008823823.com.cn/kfcios/Version/630_658431.jpg",
              goodsName:'儿童欢乐套餐',
              price:25
          }, {
              goodsId:10,
              goodsImg:"https://img.4008823823.com.cn/kfcios/Version/604_585864.jpg",
              goodsName:'快乐全家桶',
              price:99
          }],
      totalMoney:0,
      totalCount:0,
      orderForm:[],
      orderandgoodnumber:0,
      input:'',
      adress:''
    }
  },
  methods:{
    addOrderList:function(goods){
      this.totalMoney=0
      this.totalCount=0
      var isHave=false
      for(var i=0;i<this.tableData.length;i++){
        if(this.tableData[i].goodsId==goods.goodsId){
          isHave=true
        }
      }
      if(isHave){
        var arr=this.tableData.filter((o)=>{
          return o.goodsId==goods.goodsId
        })
        arr[0].count+=1
      }else{
        var newGoods={
          goodsId:goods.goodsId,
          goodsName:goods.goodsName,
          count:1,
          price:goods.price
        }
        this.tableData.push(newGoods)
      }
      this.getAllMoneyAndCount()
    },
    delSingleGoods(goods){
      this.tableData=this.tableData.filter((val)=>{
        return val.goodsId!=goods.goodsId
      })
      this.getAllMoneyAndCount()
    },
    getAllMoneyAndCount(){
      this.totalMoney=0
      this.totalCount=0
      this.tableData.forEach((ele)=>{
        this.totalCount+=ele.count
        this.totalMoney=this.totalMoney+(ele.price*ele.count)
      })
    },
    delAllGoods(){
      this.tableData=[]
      this.totalCount=0
      this.totalMoney=0
    },
    pay(){
      if(this.totalCount!=0){
        this.order()
        this.delAllGoods()
        this.$message({
          message:'结账成功，请耐心等待！可在订单栏查看您的取餐号。',
          type:'success'
        })
      }else{
        this.$message.error('请先添加商品再结账哦！')
      }
    },
    order(){
      this.orderandgoodnumber++
      var obj={
        orderNumber:this.orderandgoodnumber,
        goodNumber:this.orderandgoodnumber,
        money:this.totalMoney
      }
      this.orderForm.push(obj)
    },
    submitMessage(){  
      if(this.input!=''&&this.adress!=''){
        this.$message.success('已提交成功，我们会很快送达给您！')
      }
      this.input=''
      this.adress=''     
    }
  },
  created:function(){
    axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
    .then((response)=>{
      this.oftenGoods=response.data
    })
    .catch((error)=>{
      alert('网络错误')
    })
  },
  mounted: function() {
    var orderHeight = document.body.clientHeight;
    document.getElementById("order").style.height = orderHeight + "px";
  }
};
</script>

<style>
.pos-order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
  height: 100%;
}
.div-btn {
  margin-top: 10px;
  text-align: center;
}
th,
td {
  text-align: center !important;
}
.el-tabs__nav {
  margin: 0 20px !important;
}
.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 9px;
  color: brown;
}
.often-goods-list{
  padding:0 0 20px 0;
}
.often-goods-list ul li {
  display: inline-block;
  border: 1px solid #e5e9f2;
  background-color: #fff;
  padding: 10px;
  margin: 20px 0px 0px 20px;
  cursor: pointer;
}
.o-price {
  color: #58b7ff;
}
.cookList li {
  width: 30%;
  border: 1px solid #e5e9f2;
  display: inline-block;
  margin: 0 10px 10px 10px;
  background-color: #fff;
  cursor: pointer;
}

img{
  width:50%;
}
.foodName {
  font-size: 18px;
  color: brown;
}
.foodPrice {
  display: inline-block;
  font-size: 16px;
  
}
.nameandprice{
  display: inline-block;
  vertical-align: top;
  margin-top: 10px;
}
.total{
  background-color: #fff;
  border-bottom: 1px solid #d3dce6;
  text-align: center;
  padding: 10px;
}
.total span{
  margin: 0 10px;
}
.input{
  padding: 0px 20px;
  
}
div.el-input{
  margin-bottom: 10px;
}
.input > span{
  margin-bottom: 8px;
  display: inline-block;
}
div.btn-ipt{
  text-align: center;
  padding: 8px;
}
</style>