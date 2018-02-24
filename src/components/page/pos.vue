<template>
  <div class="pos">
   <el-row>
   <el-col :span='7' class="pos-order" id="order-list">
    <el-tabs>
      <el-tab-pane label="点餐">
         <el-table :data="tableData" border style="width:100%">
           <el-table-column prop="goodsName" label="商品名称"></el-table-column>
           <el-table-column prop="count" label="数量" width="90"></el-table-column>
           <el-table-column prop="price" label="金额" width="90"></el-table-column>
           <el-table-column  label="操作" width="100" fixed="right">
             <template slot-scope="scope"><!--用于将元素或者组件表示为作用域插槽。在最新版本以代替scope-->
               <el-button type="text" size="small" @click="delSingelgoods(scope.row)">删除</el-button>
               <el-button type="text" size="small" @click="addOrderlist(scope.row)">增加</el-button>
             </template>
           </el-table-column>
         </el-table>
         <div class="totalprice">
           <small>数量:</small>{{totalCount}} &nbsp;&nbsp;&nbsp;&nbsp; <small>金额:</small>{{totalMoney}}元
         </div>
        <div class="an">
          <el-button type="warning" plain @click="guadan">挂单</el-button>
          <el-button type="danger" plain @click="delallgoods">删除</el-button>
          <el-button type="success" plain @click="checkout">结账</el-button>
        </div>

      </el-tab-pane>
      <el-tab-pane label="挂单">
         <el-table :data="tableData1" border style="width:100%">
           <el-table-column prop="goodsName" label="商品名称"></el-table-column>
           <el-table-column prop="count" label="数量" width="90"></el-table-column>
           <el-table-column prop="price" label="金额" width="90"></el-table-column>
           <el-table-column  label="操作" width="100" fixed="right">
             <template slot-scope="scope"><!--用于将元素或者组件表示为作用域插槽。在最新版本以代替scope-->
               <el-button type="text" size="small" @click="delSingelgoods1(scope.row)">删除</el-button>
               <el-button type="text" size="small" @click="addOrderlist1(scope.row)">增加</el-button>
             </template>
           </el-table-column>
         </el-table>
         <div class="totalprice">
           <small>数量:</small>{{totalCount1}} &nbsp;&nbsp;&nbsp;&nbsp; <small>金额:</small>{{totalMoney1}}元
         </div>
        <div class="an">
          <el-button type="warning" plain>挂单</el-button>
          <el-button type="danger" plain @click="delallgoods1">删除</el-button>
          <el-button type="success" plain @click="checkout1">结账</el-button>
        </div>
      </el-tab-pane>
      <el-tab-pane label="外卖">
        外卖
      </el-tab-pane>
    </el-tabs>

   </el-col>
 
   <el-col :span='17'>
     <div class="goods">
       <div class="biaoti">常用商品</div>
      <div class="goods-list">
        <ul class="ausu">
          <li v-for="goods in oftengoods" @click="addOrderlist(goods)">
            <span>{{goods.goodsName}}</span>
            <span class="price">￥{{goods.price}}元</span>
          </li>
        </ul>
      </div>
     </div>



     <div class="goods-type">
       <el-tabs>
         <el-tab-pane label="汉堡">
           <div>
             <ul class="cooklist">
               <li v-for="goods in type0goods" @click="addOrderlist(goods)">
                 <span class="foodImg"><img :src="goods.goodsImg" width="100"></span>
                 <span class="foodname">{{goods.goodsName}}</span>
                 <span class="foodprice">￥{{goods.price}}元</span>
               </li>
             </ul>
           </div>
         </el-tab-pane>
         <el-tab-pane label="小食">
           <ul class="cooklist">
               <li v-for="goods in type1goods" @click="addOrderlist(goods)">
                 <span class="foodImg"><img :src="goods.goodsImg" width="100"></span>
                 <span class="foodname">{{goods.goodsName}}</span>
                 <span class="foodprice">￥{{goods.price}}元</span>
               </li>
             </ul>
         </el-tab-pane>
         <el-tab-pane label="饮料">
           <ul class="cooklist">
               <li v-for="goods in type2goods" @click="addOrderlist(goods)">
                 <span class="foodImg"><img :src="goods.goodsImg" width="100"></span>
                 <span class="foodname">{{goods.goodsName}}</span>
                 <span class="foodprice">￥{{goods.price}}元</span>
               </li>
             </ul>
         </el-tab-pane>
         <el-tab-pane label="套餐">
           <ul class="cooklist">
               <li v-for="goods in type3goods" @click="addOrderlist(goods)">
                 <span class="foodImg"><img :src="goods.goodsImg" width="100"></span>
                 <span class="foodname">{{goods.goodsName}}</span>
                 <span class="foodprice">￥{{goods.price}}元</span>
               </li>
             </ul>
         </el-tab-pane>
       </el-tabs>
     </div>
   </el-col>
   </el-row>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "pos",
  data() {
    return {
      tableData: [],
      tableData1:[],
      oftengoods: [],
      type0goods: [],
      type1goods: [],
      type2goods: [],
      type3goods: [],
      totalMoney: 0,
      totalCount: 0,
      totalMoney1: 0,
      totalCount1: 0,
    };
  },
  created: function() {
    axios
      .get("http://jspang.com/DemoApi/oftenGoods.php")
      .then(reponse => {
        //console.log(reponse);
        this.oftengoods = reponse.data;
      })
      .catch(error => {
        //console.log(error);
        alert("请求数据错误");
      });

    axios
      .get("http://jspang.com/DemoApi/typeGoods.php")
      .then(reponse => {
        console.log(reponse);
        this.type0goods = reponse.data[0];
        this.type1goods = reponse.data[1];
        this.type2goods = reponse.data[2];
        this.type3goods = reponse.data[3];
      })
      .catch(error => {
        //console.log(error);
        alert("请求数据错误");
      });
  },
  //当所有虚拟DOM加载完成后，执行里面的条件
  mounted: function() {
    var oderHeight = document.body.clientHeight; //原生js获得网页可视内容区域的高度
    document.getElementById("order-list").style.height = oderHeight + "px";
  },
  methods: {
    addOrderlist(goods) {
      this.totalMoney = 0;
      this.totalCount = 0;

      let isHave = false; //let定义只能为块级的变量或者全局
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true;
        }
      }

      if (isHave) {
        let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
        arr[0].count++;
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        };
        this.tableData.push(newGoods);
      }
      this.getallMoney();
    },

    addOrderlist1(goods) {
      this.totalMoney1 = 0;
      this.totalCount1 = 0;

      let isHave = false; //let定义只能为块级的变量或者全局
      for (let i = 0; i < this.tableData1.length; i++) {
        if (this.tableData1[i].goodsId == goods.goodsId) {
          isHave = true;
        }
      }

      if (isHave) {
        let arr = this.tableData1.filter(o => o.goodsId == goods.goodsId);
        arr[0].count++;
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1
        };
        this.tableData1.push(newGoods);
      }
      this.getallMoney1();
    },

    //单个删除
    delSingelgoods(goods) {
      this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
      this.getallMoney();
    },
    delSingelgoods1(goods) {
      this.tableData1 = this.tableData1.filter(o => o.goodsId != goods.goodsId);
      this.getallMoney1();
    },
    delallgoods() {
      this.tableData = [];
      this.totalCount = 0;
      this.totalMoney = 0;
    },
    delallgoods1() {
      this.tableData1 = [];
      this.totalCount1 = 0;
      this.totalMoney1 = 0;
    },
    checkout() {
      if (this.totalCount != 0) {
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
        this.$message({
          message: "结账成功,感谢你又为店里出了一份力",
          type: "success"
        });
      } else {
        this.$message.error("不能空结,本王了解你急切的心情");
      }
    },
    checkout1() {
      if (this.totalCount1 != 0) {
        this.tableData1 = [];
        this.totalCount1 = 0;
        this.totalMoney1 = 0;
        this.$message({
          message: "结账成功,感谢你又为店里出了一份力",
          type: "success"
        });
      } else {
        this.$message.error("不能空结,本王了解你急切的心情");
      }
    },
    getallMoney() {
      this.totalCount = 0;
      this.totalMoney = 0;
      if (this.tableData) {
        this.tableData.forEach(element => {
          this.totalCount += element.count;
          this.totalMoney = this.totalMoney + element.price * element.count;
        });
      }
    },
    getallMoney1() {
      this.totalCount1 = 0;
      this.totalMoney1 = 0;
      if (this.tableData1) {
        this.tableData1.forEach(element => {
          this.totalCount1 += element.count;
          this.totalMoney1 = this.totalMoney1 + element.price * element.count;
        });
      }
    },
    guadan(){
      this.tableData1=this.tableData;
      this.totalCount1 = 0;
      this.totalMoney1 = 0;
      if (this.tableData1) {
        this.tableData1.forEach(element => {
          this.totalCount1 += element.count;
          this.totalMoney1 = this.totalMoney1 + element.price * element.count;
        });
      }
      this.tableData=[];
      this.totalCount = 0;
      this.totalMoney = 0;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pos-order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}
.an {
  margin-top: 1em;
}
.biaoti {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
  text-align: left;
}
.goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #d5e1f8;
  padding: 10px;
  margin: 10px;
  background-color: #fff;
}
.price {
  color: #58b7ff;
}
.goods-type {
  clear: both;
}
.cooklist li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auot;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
  cursor: pointer;
}
.ausu {
  cursor: pointer;
}
.cooklist li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
}
.foodname {
  font-size: 18px;
  padding-left: 10px;
  color: brown;
}
.foodprice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
.totalprice {
  background-color: #fff;
  padding: 10px;
  border-bottom: 1px solid #d3dce6;
}
</style>
