<template>
  <div class="hello">
    <div class="header">高一物理</div>

    <div class="btn_bg">
      <div class="btn" @click="click1('add')">添加准考证号位数</div>
      <div class="btn" @click="click1('reduce')">减少准考证号位数</div>
      <div class="btn" @click="click2('add')">添加客观题数量</div>
      <div class="btn" @click="click2('reduce')">减少客观题数量</div>
      <div class="btn" @click="click3('add')">添加解答题数量</div>
      <div class="btn" @click="click3('reduce')">减少解答题数量</div>
    </div>

    <div class="div1">
      <div class="div1_head">准考证号码（至少6位，至多12位）</div>
      <div class="div1_container">
        <div class="div1_col" v-for="(item, col) in div1Col" :key="col">
          <div class="div1_numSel">{{div1Num[col]}}</div>
          <div class="div1_num_bg" v-for="(item, row) in div1Row" :key="row">
            <div class="div1_num" :class="{'select':div1Num[col]>=0&&div1Num[col]==row}" @click="clickNum1(row, col)">{{row}}</div>
          </div>
        </div>
      </div>
    </div>

    <div class="div2">
      <div class="div2_head">客观题（1-9为单选，10以后为多选，至少12题，至多20题）</div>
      <div class="div2_container">
        <div class="div2_num" v-for="(item, $index) in div2Num" :key="$index">
          <div style="width: 18px;">{{item}}</div>
          <div class="div2_sel" :class="{'select':div2Arr[$index]==item2 || (div2Arr[$index]&&div2Arr[$index][$index2]==item2) }" v-for="(item2, $index2) in div2Sel" :key="$index2" @click="clickNum2(item, $index2)">{{item2}}</div>
        </div>
      </div>
      <div class="div2_head">客观题答案</div>
      <div class="div2_answer">
        <div class="answer" v-for="(item, $index) in div2Arr" :key="$index"><span style="color: red;">{{$index+1}}</span>{{item | answerFilter}}</div>
      </div>
    </div>

    <div class="div3">
      <div class="div3_head">解答题（至少1题，至多5题）</div>
      <div class="div3_content" v-for="(item, $index) in div3Num" :key="$index">第{{item}}题</div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      div1Row: 10,/*行数*/
      div1Col: 8,/*列数*/
      div1Num: [],/*准考证号*/

      div2Num: 16,/*客观题数量*/
      div2Sel: ['A', 'B', 'C', 'D'],/*可选项*/
      div2Arr: [],/*客观题答案*/

      div3Num: 1,/*解答题数量*/
    }
  },
  filters: {
    answerFilter(val) {
      let result;
      if(typeof(val) == 'object') {
        result = val.join('');
      }else {
        result = val;
      }
      return result;
    }
  },
  watch: {
    div1Col: {
      handler: function(val) {
        this.div1Num.length = val;
      },
      immediate: true
    },
    div2Num: {
      handler: function(val) {
        this.div2Arr.length = val;
      },
      immediate: true
    }
  },
  methods: {
    click1(val) {
      if(val == 'add' && this.div1Col < 12) {
        this.div1Col++;
      }else if(val == 'reduce' && this.div1Col > 6) {
        this.div1Col--;
      }
    },
    click2(val) {
      if(val == 'add' && this.div2Num < 20) {
        this.div2Num++;
      }else if(val == 'reduce' && this.div2Num > 12) {
        this.div2Num--;
      }
    },
    click3(val) {
      if(val == 'add' && this.div3Num < 5) {
        this.div3Num++;
      }else if(val == 'reduce' && this.div3Num > 1) {
        this.div3Num--;
      }
    },
    // 选择准备证号
    clickNum1(val, col) {
      this.$set(this.div1Num, col, val);
    },
    // 选择客观题答案
    clickNum2(val1, val2) {/*题数，选择的答案*/
      if(val1 >= 1 && val1 <= 9) {
        this.$set(this.div2Arr, val1-1, this.div2Sel[val2]);
      }else if(val1 > 9) {
        let arr = [];
        arr.length = 4;
        if(!this.div2Arr[val1-1]) {
          arr[val2] = this.div2Sel[val2];
        }else {
          arr = this.div2Arr[val1-1];
          if(arr[val2] == this.div2Sel[val2]) {
            arr[val2] = null;
          }else {
            arr[val2] = this.div2Sel[val2];
          }
        }
        this.$set(this.div2Arr, val1-1, arr);
      }
    }
  }
}
</script>

<style scoped>
  @import '../css/helloWord.css';
</style>