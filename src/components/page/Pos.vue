<template>
  <div class="pos">
    <el-row>
      <el-col :span="8">
        <el-tabs type="border-card" v-model="activeName1">
          <el-tab-pane label="点餐" name="first">
            <el-table :data="tableData" border style="width: 100%">
              <el-table-column fixed prop="goodsName" label="商品"></el-table-column>
              <el-table-column fixed prop="count" label="数量"></el-table-column>
              <el-table-column fixed prop="price" label="金额"></el-table-column>
              <el-table-column fixed="right" label="操作">
                <template scope="scope">
                  <el-button type="text" size="small" @click="deleteSigleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
          </el-tab-pane>
          <el-tab-pane label="挂单" name="second">配置管理</el-tab-pane>
          <el-tab-pane label="外卖" name="third">角色管理</el-tab-pane>
        </el-tabs>
        <el-row style="margin-top: 20px;">
          <el-button type="primary" round>挂单</el-button>
          <el-button type="success" round @click="checkout">结账:（数量:{{totalCount}}+金额:{{totalPrice}}）</el-button>
          <el-button type="danger" round>删除</el-button>
        </el-row>
      </el-col>
      <el-col :span="16">
        <div>
          <el-menu
            :default-active="activeIndex"
            class="el-menu-demo"
            mode="horizontal"
            background-color="#545c64"
            text-color="#fff"
            active-text-color="#ffd04b">
            <el-menu-item index="1">常用商品</el-menu-item>
          </el-menu>
          <div class="often-goods-list">
            <ul v-for="goods in oftenGoods">
              <li @click="addOrderList(goods)">
                <span>{{goods.goodsId}}</span>
                <span>{{goods.goodsName}}</span>
                <span class="o-price">￥{{goods.price}}</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="bottom-goods">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div class="often-goods-list">
                <ul class='cookList' v-for="type0 in type0Goods">
                  <li @click="addOrderList(type0)">
                    <span class="foodImg"><img :src="type0.goodsImg" width="100%"></span>
                    <span class="foodName">{{type0.goodsName}}</span>
                    <span class="foodPrice">￥{{type0.price}}</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <div class="often-goods-list">
                <ul class='cookList' v-for="type0 in type1Goods">
                  <li @click="addOrderList(type0)">
                    <span class="foodImg"><img :src="type0.goodsImg" width="100%"></span>
                    <span class="foodName">{{type0.goodsName}}</span>
                    <span class="foodPrice">￥{{type0.price}}</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div class="often-goods-list">
                <ul class='cookList' v-for="type0 in type2Goods">
                  <li @click="addOrderList(type0)">
                    <span class="foodImg"><img :src="type0.goodsImg" width="100%"></span>
                    <span class="foodName">{{type0.goodsName}}</span>
                    <span class="foodPrice">￥{{type0.price}}</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <div class="often-goods-list">
                <ul class='cookList' v-for="type0 in type3Goods">
                  <li @click="addOrderList(type0)">
                    <span class="foodImg"><img :src="type0.goodsImg" width="100%"></span>
                    <span class="foodName">{{type0.goodsName}}</span>
                    <span class="foodPrice">￥{{type0.price}}</span>
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
  import axios from 'axios';

  export default {
    name: "pos",
    data() {
      return {
        tableData: [],
        totalCount: 0,
        totalPrice: 0,
        activeName1: "first",
        activeIndex: '1',
        activeIndex1: '1',
        oftenGoods: [],
        type0Goods: [],
        type1Goods: [],
        type2Goods: [],
        type3Goods: [],
      }
    },
    created() {
      axios.get('http://jspang.com/DemoApi/oftenGoods.php')
        .then(res => {
          // console.log(res);
          this.oftenGoods = res.data;
        })
        .catch(err => {
          console.log(err);
        });
      axios.get('http://jspang.com/DemoApi/typeGoods.php')
        .then(res => {
          // console.log(res);
          this.type0Goods = res.data[0];
          this.type1Goods = res.data[1];
          this.type2Goods = res.data[2];
          this.type3Goods = res.data[3];
        })
    },
    methods: {
      /**
       * 左边点击添加到右边的结算栏目里面
       */
      addOrderList(goods) {
        let isHave = false;
        let len = this.tableData.length;
        /**
         * 判断这个商品是不是存在于列表中
         * isHave
         */
        for (let i = 0; i < len; i++) {
          if (this.tableData[i].goodsId === goods.goodsId) {
            isHave = true;
          } else {
            // console.log('error');
          }
        }
        if (isHave) {
          //此商品存在
          let arr = this.tableData.filter(value => value.goodsId === goods.goodsId);
          arr[0].count++;
          // console.log('此商品存在', arr);
        } else {
          //此商品不存在
          let newArr = {
            goodsId: goods.goodsId,
            goodsName: goods.goodsName,
            price: goods.price,
            count: 1
          }
          this.tableData.push(newArr);
        }
        //计算总金额
        this.getAllMoney();
      },
      deleteSigleGoods(goods) {
        console.log(goods);
        this.tableData = this.tableData.filter(o => o.goodsId !== goods.goodsId);
        this.getAllMoney();
      },
      //获取总金额
      getAllMoney() {
        this.totalCount = 0;
        this.totalPrice = 0;
        if (this.tableData) {
          this.tableData.forEach(element => {
            this.totalCount += element.count;
            this.totalPrice += (element.count * element.price)
          })
        }
      },
      //结算
      checkout() {
        if (this.totalCount !== 0) {
          this.tableData = [];
          this.totalCount = 0;
          this.totalPrice = 0;
          this.$message({
            message: '结账成功！',
            type: 'success'
          });
          this.$notify({
            title: '结账成功',
            message: '这是一条成功的提示消息',
            type: 'success'
          });
        } else {
          this.$message.error('Oops!先添加点商品吧，小主...');
        }
      }

    },


  }
</script>

<style scoped>
  .pos {
    height: 100%;
  }

  .often-goods-list ul li {
    list-style: none;
    float: left;
    border: 1px solid #E5E9F2;
    padding: 10px;
    margin: 5px;
    background-color: #fff;
    border-radius: 6px;
  }

  .o-price {
    color: #58B7FF;
  }

  .bottom-goods {
    overflow: hidden;
    display: inline-block;
    width: 100%;
    margin-left: 20px;
  }

  .cookList li {
    list-style: none;
    width: 23%;
    border: 1px solid #E5E9F2;
    height: auto;
    overflow: hidden;
    background-color: #fff;
    padding: 2px;
    float: left;
    margin: 2px;

  }

  .cookList li span {

    display: block;
    float: left;
  }

  .foodImg {
    width: 40%;
  }

  .foodName {
    font-size: 16px;
    padding-left: 10px;
    color: brown;

  }

  .foodPrice {
    font-size: 16px;
    padding-left: 10px;
    padding-top: 10px;
  }

</style>
