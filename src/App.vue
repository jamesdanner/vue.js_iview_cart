<template>
  <div id="app">
    <div class="container">
      <h1>丰橙商城购物车</h1>
      <div class="table">
        <Table :columns="columns" :data="booksData">
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
          <Button size="large" @click="submit" type="success">付款</Button>
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
      booksData: [
        {
          id: '0',
          name: "JavaScript权威指南（第6版）",
          pic: 'http://img3m0.ddimg.cn/13/17/22722790-1_b_6.jpg',
          price: 111.20,
          total: 0,
          count: 1,
        },
        {
          id: '1',
          name: "JavaScript忍者秘籍 第2版",
          pic: 'http://img3m0.ddimg.cn/6/7/25234710-1_b_3.jpg',
          price: 95.00,
          total: 0,
          count: 0,
        },
        {
          id: '2',
          name: "JavaScript ES6 函数式编程入门经典",
          pic: 'http://img3m2.ddimg.cn/62/16/25202492-1_b_3.jpg',
          price: 43.80,
          total: 0,
          count: 0,
        },
        {
          id: '3',
          name: "高性能JavaScript",
          pic: 'http://img3m5.ddimg.cn/16/29/23762095-1_b_204.jpg',
          price: 62.40,
          total: 0,
          count: 0,
        },
        {
          id: '4',
          name: "深入浅出Node.js",
          pic: 'http://img3m1.ddimg.cn/69/1/23371791-1_b_3.jpg',
          price: 54.50,
          total: 0,
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
            this.booksData.splice(index, 1);
            this.$Message.info('删除成功！');
          },
          onCancel: () => {
            // this.$Message.info('Clicked cancel');
          }
      });
    },
    submit() {
      const req = this.booksData.map(item => ({id: item.id, count: item.count}));
      console.log(req)
      this.$Message.info(JSON.stringify(req));
    }
  },
  computed: {
    total() {
      const price = this.booksData.reduce((a, b) => {
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
}
.footer {
  text-align: right;
}
.footer h2 {
  padding: 10px;
}
.container .goods-total {
  color: red;
  font-size: 14px;
}
</style>
