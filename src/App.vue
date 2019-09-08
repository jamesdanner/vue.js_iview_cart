<template>
  <div id="app">
    <div class="container">
      <h1>LOL英雄皮肤购物车</h1>
      <div class="table">
        <Table :columns="columns" :data="skinData">
          <template slot-scope="{row}" slot="pic">
            <img class="goods-img" :src="row.pic" alt="">
          </template>
          <template slot-scope="{row}" slot="price">
            <span>￥{{row.price}}</span>
          </template>
          <template slot-scope="{row}" slot="total">
            <span class="goods-total">￥{{addTotal(row.count, row.price)}}</span>
          </template>
          <template slot-scope="{row, index}" slot="action">
            <Button type="error" @click="remove(index)">删除</Button>
          </template>
        </Table>
      </div>
      <div class="footer">
        <h2 >总计：{{total}}</h2>
        <div>
          <Button size="large" @click="submit" type="success">微信支付</Button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import bignumber from 'bignumber.js';

export default {
  name: "app",
  data() {
    return {
      columns: [
        {
          title: "序号",
          key: "index",
          type: 'index',
          width: '80',
        },
        {
          title: "图片",
          key: "age",
          slot: 'pic',
          width: '120',
        },
        {
          title: "商品名",
          key: "name"
        },
        {
          title: "单价",
          key: "price",
          slot: 'price',
        },
        {
          title: "商品总价",
          slot: 'total',
        },
        {
          title: "商品数量",
          key: "count",
          // slot: 'count',
          render: (h, params) => {
              return h('div', [
                  h('InputNumber', {
                      props: {
                          // type: 'person',
                          value: params.row.count,
                          max: 100,
                          min: 0,
                      },
                      on: {
                        'on-change': (value) => {
                          this.booksData[params.index].count = value;
                        }
                      }
                  }),
              ]);
          }
        },
        {
          title: "操作",
          key: "action",
          slot: 'action',
          width: '120'
        }
      ],
      skinData: [
        {
          id: '0',
          name: "元素女皇 奇亚娜",
          pic: 'https://game.gtimg.cn/images/daoju/app/lol/medium/1-246-.jpg?_t=1561703982',
          price: '43.65',
          count: 0,
        },
        {
          id: '1',
          name: "德玛西亚警官 卢锡安",
          pic: 'https://game.gtimg.cn/images/daoju/app/lol/medium/2-236009-.jpg?_t=1563344600',
          price: '76.63',
          count: 0,
        },
        {
          id: '2',
          name: "电玩女神 凯特琳",
          pic: 'https://game.gtimg.cn/images/daoju/app/lol/medium/2-51019-.jpg?_t=1561704164',
          price: '43.65',
          count: 0,
        },
        {
          id: '3',
          name: "战斗学院 伊泽瑞尔",
          pic: 'https://game.gtimg.cn/images/daoju/app/lol/medium/2-81021-.jpg?_t=1557906564',
          price: '96.03',
          count: 0,
        },
        {
          id: '4',
          name: "源计划：末日 沃里克",
          pic: 'https://game.gtimg.cn/images/daoju/app/lol/medium/2-19016-.jpg?_t=1565065348',
          price: '76.63',
          count: 0,
        },
      ]
    };
  },
  methods: {
    addTotal(count, price) {
      return new bignumber(count).multipliedBy(price).toFixed(2);
    },
    remove(index) {
      this.$Modal.confirm({
          title: '提示',
          content: '<p>你确定要删除这组数据吗？</p>',
          onOk: () => {
            this.skinData.splice(index, 1);
            this.$Message.info('删除成功！');
          },
          onCancel: () => {
            // this.$Message.info('Clicked cancel');
          }
      });
    },
    submit() {
      const req = this.skinData.map(item => ({id: item.id, count: item.count}));
      console.log(req)
      this.$Message.info(JSON.stringify(req));
    }
  },
  computed: {
    total() {
      const price = this.skinData.reduce((a, b) => {
        return new bignumber(a).plus(new bignumber(b.count).multipliedBy(b.price));
      }, 0);
      return new bignumber(price).toFixed(2);
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  /* text-align: center; */
  color: #2c3e50;
  /* margin-top: 60px; */
}
.container {
  width: 1224px;
  margin: 0 auto;
}
.container h1 {
  margin-top: 20px;
}
.table {
  margin-top: 20px;
}
.container .goods-img {
  width: 100px;
  padding: 10px;
}
.footer {
  text-align: right;
  padding-bottom: 200px;
  
}
.footer h2 {
  padding: 10px;
}
.container .goods-total {
  color: red;
  font-size: 14px;
}
</style>
