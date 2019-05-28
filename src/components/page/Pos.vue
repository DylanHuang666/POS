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
                  <el-button type="text" size="small">删除</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="total">
              <span>数量：{{totalCount}}</span>
              <span>总价：￥{{totalMoney}}</span>
            </div>
            <div class="div-btn">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger">删除</el-button>
              <el-button type="success">结账</el-button>
            </div>          
          </el-tab-pane>
          <el-tab-pane label="挂单">
            
          </el-tab-pane>
          <el-tab-pane label="外卖">
            
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
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      totalMoney:0,
      totalCount:0
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
      this.tableData.forEach((ele)=>{
        this.totalCount+=ele.count
        this.totalMoney=this.totalMoney+(ele.price*ele.count)
      })
    }
  },
  created:function(){
    axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
    .then((response)=>{
        this.oftenGoods=response.data
    })
    .catch((error)=>{
      
    })
    axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/typeGoods')
    .then((response)=>{
      console.log(response)
        this.type0Goods=response.data[0]
        this.type1Goods=response.data[1]
        this.type2Goods=response.data[2]
        this.type3Goods=response.data[3]
    })
    .catch((error)=>{
      
    })
  },
  mounted: function() {
    var orderHeight = document.body.clientHeight;
    document.getElementById("order").style.height = orderHeight + "px";
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
  width: 23%;
  border: 1px solid #e5e9f2;
  display: inline-block;
  margin: 0 10px 10px 10px;
  background-color: #fff;
  cursor: pointer;
}

img{
  width: 40%;
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
</style>