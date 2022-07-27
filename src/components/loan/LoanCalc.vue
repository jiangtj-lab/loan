<template>
  <el-row class="row-bg" justify="center">
    <div>
      <el-descriptions class="margin-top" :column="3" size="default" border>
        <el-descriptions-item :span="2">
          <template #label>总贷款金额</template>
          {{ all }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>贷款时限</template>
          {{ time }}年
        </el-descriptions-item>

        <el-descriptions-item :span="3" />
        <el-descriptions-item :span="2">
          <template #label>按年:年利率</template>
          {{ (yLi * 100).toFixed(3) }}%
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>总利息</template>
          {{ (yFA * time - all).toFixed(2) }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>每期</template>
          {{ yFA.toFixed(2) }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>期数</template>
          {{ time }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>还款总额</template>
          {{ (yFA * time).toFixed(2) }}
        </el-descriptions-item>

        <el-descriptions-item :span="3" />
        <el-descriptions-item :span="2">
          <template #label>按月:月利率</template>
          {{ (mLi * 100).toFixed(3) }}%
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>总利息</template>
          {{ (mFA * time * 12 - all).toFixed(2) }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>每期</template>
          {{ mFA.toFixed(2) }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>期数</template>
          {{ time * 12 }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>还款总额</template>
          {{ (mFA * time * 12).toFixed(2) }}
        </el-descriptions-item>

        <el-descriptions-item :span="3" />
        <el-descriptions-item :span="2">
          <template #label>按日:日利率</template>
          {{ (dLi * 100).toFixed(3) }}%
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>总利息</template>
          {{ (dFA * time * 365 - all).toFixed(2) }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>每期</template>
          {{ dFA.toFixed(2) }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>期数</template>
          {{ time * 365 }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>还款总额</template>
          {{ (dFA * time * 365).toFixed(2) }}
        </el-descriptions-item>
      </el-descriptions>
    </div>
  </el-row>
  <el-row class="row-bg" justify="center" style="margin-top: 20px;">
    点击以下按钮调整贷款数据
  </el-row>
  <el-row class="row-bg" justify="center" style="margin-top: 10px;">
    <el-button type="primary" plain @click="allModal.v = true">贷款金额</el-button>
    <el-button type="primary" plain @click="timeModal.v = true">期数</el-button>
    <el-button type="primary" plain @click="liModal.v = true">贷款利率</el-button>
    <el-button type="primary" plain @click="feeModal.v = true">等额本息</el-button>
  </el-row>

  <el-dialog v-model="allModal.v" title="贷款金额">
    <el-form :inline="true">
      <el-form-item>
        <el-input v-model="allModal.all" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="allModal.onSubmit">确定</el-button>
      </el-form-item>
    </el-form>
    <el-row>
      该操作将重新计算每期金额
    </el-row>
  </el-dialog>

  <el-dialog v-model="timeModal.v" title="期数">
    <el-form :inline="true">
      <el-form-item>
        <el-select v-model="timeModal.time">
          <el-option label="1年" :value="1" />
          <el-option label="2年" :value="2" />
          <el-option label="3年" :value="3" />
          <el-option label="4年" :value="4" />
          <el-option label="5年" :value="5" />
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="timeModal.onSubmit">确定</el-button>
      </el-form-item>
    </el-form>
    <el-row>
      该操作将重新计算每期金额
    </el-row>
  </el-dialog>

  <el-dialog v-model="liModal.v" title="利率">
    <el-form :inline="true">
      <el-form-item label="类型">
        <el-radio-group v-model="liModal.type" @change="liModal.onChange">
          <el-radio-button :label="1">年</el-radio-button>
          <el-radio-button :label="2">月</el-radio-button>
          <el-radio-button :label="3">日</el-radio-button>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="利率">
        <el-input v-model="liModal.li" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="liModal.onSubmit">确定</el-button>
      </el-form-item>
    </el-form>
    <el-row>
      该操作将重新计算每期金额
    </el-row>
  </el-dialog>

  <el-dialog v-model="feeModal.v" title="每期金额">
    <el-form :inline="true">
      <el-form-item label="类型">
        <el-radio-group v-model="feeModal.type">
          <el-radio-button :label="1">年</el-radio-button>
          <el-radio-button :label="2">月</el-radio-button>
          <el-radio-button :label="3">日</el-radio-button>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="每期金额">
        <el-input v-model="feeModal.fee" />
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="feeModal.onSubmit">确定</el-button>
      </el-form-item>
    </el-form>
    <el-row>
      该操作将重新计算利率
    </el-row>
  </el-dialog>
</template>

<script setup lang="ts">
import { computed, Ref, ref, reactive } from "vue";
import { ElMessage } from 'element-plus'
import {
  Iphone,
  Location,
  OfficeBuilding,
  Tickets,
  User,
} from "@element-plus/icons-vue";

const all = ref(0);
const time = ref(0); // 年
const yLi = ref(0);
const yFA = ref(0);
const mLi = ref(0);
const mFA = ref(0);
const dLi = ref(0);
const dFA = ref(0);

// 默认值
all.value = 200000
yLi.value = 0.072;
time.value = 3

mLi.value = Math.pow(1 + yLi.value, 1 / 12) - 1;
dLi.value = Math.pow(1 + yLi.value, 1 / 365) - 1;

function calcAllFee() {
  yFA.value = calcAmount(yLi.value, time.value)
  mFA.value = calcAmount(mLi.value, time.value * 12)
  dFA.value = calcAmount(dLi.value, time.value * 365)
}
calcAllFee()
// 每月还款额=贷款本金×[月利率×(1+月利率)^还款月数]÷{[(1+月利率)^还款月数]-1}
function calcAmount(li: number, t: number) {
  return all.value * li * Math.pow(1 + li, t) / (Math.pow(1 + li, t) - 1)
}

const allModal = reactive({
  v: false,
  all: all.value,
  onSubmit: () => {
    all.value = Number.parseFloat(allModal.all + '')
    calcAllFee()
    allModal.v = false
  }
});
const timeModal = reactive({
  v: false,
  time: time.value,
  onSubmit: () => {
    time.value = timeModal.time
    calcAllFee()
    timeModal.v = false
  }
});
const liModal = reactive({
  v: false,
  type: 1,
  li: yLi.value,
  onChange: () => {
    if (liModal.type === 1) liModal.li = yLi.value
    if (liModal.type === 2) liModal.li = mLi.value
    if (liModal.type === 3) liModal.li = dLi.value
  },
  onSubmit: () => {
    if (liModal.type === 1) {
      yLi.value = Number.parseFloat(liModal.li + '')
      mLi.value = Math.pow(1 + yLi.value, 1 / 12) - 1;
      dLi.value = Math.pow(1 + yLi.value, 1 / 365) - 1;
    }
    if (liModal.type === 2) {
      mLi.value = Number.parseFloat(liModal.li + '')
      yLi.value = Math.pow(1 + mLi.value, 12) - 1;
      dLi.value = Math.pow(1 + yLi.value, 1 / 365) - 1;
    }
    if (liModal.type === 3) {
      dLi.value = Number.parseFloat(liModal.li + '')
      yLi.value = Math.pow(1 + dLi.value, 365) - 1;
      mLi.value = Math.pow(1 + yLi.value, 1 / 12) - 1;
    }
    calcAllFee()
    liModal.v = false
  }
});
const feeModal = reactive({
  v: false,
  type: 2,
  fee: 0,
  onSubmit: () => {
    // 12.345[6]%
    let a = [0, 0, 0, 0, 0, 0]
    let i = 0
    let t = 0
    if (feeModal.type === 1) t = time.value
    if (feeModal.type === 2) t = time.value * 12
    if (feeModal.type === 3) t = time.value * 365
    calcLi(Number.parseFloat(feeModal.fee + ''), t, a, i)
    let li = a[0] * 0.1 + a[1] * 0.01 + a[2] * 0.001 + a[3] * 0.0001 + a[4] * 0.00001 + a[5] * 0.000001
    if (!li) {
      ElMessage.error('Oops, 金额太少.')
      return
    }
    if (feeModal.type === 1) {
      yLi.value = li
      mLi.value = Math.pow(1 + yLi.value, 1 / 12) - 1;
      dLi.value = Math.pow(1 + yLi.value, 1 / 365) - 1;
      yFA.value = Number.parseFloat(feeModal.fee + '')
      mFA.value = calcAmount(mLi.value, time.value * 12)
      dFA.value = calcAmount(dLi.value, time.value * 365)
    }
    if (feeModal.type === 2) {
      mLi.value = li
      yLi.value = Math.pow(1 + mLi.value, 12) - 1;
      dLi.value = Math.pow(1 + yLi.value, 1 / 365) - 1;
      yFA.value = calcAmount(yLi.value, time.value)
      mFA.value = Number.parseFloat(feeModal.fee + '')
      dFA.value = calcAmount(dLi.value, time.value * 365)
    }
    if (feeModal.type === 3) {
      dLi.value = li
      yLi.value = Math.pow(1 + dLi.value, 365) - 1;
      mLi.value = Math.pow(1 + yLi.value, 1 / 12) - 1;
      yFA.value = calcAmount(yLi.value, time.value)
      mFA.value = calcAmount(mLi.value, time.value * 12)
      dFA.value = Number.parseFloat(feeModal.fee + '')
    }
    feeModal.v = false
  }
});

function calcLi(fee: number, t: number, a: Array<number>, i: number) {
  let li = a[0] * 0.1 + a[1] * 0.01 + a[2] * 0.001 + a[3] * 0.0001 + a[4] * 0.00001 + a[5] * 0.000001 + Math.pow(0.1, i + 1)
  let cFee = calcAmount(li, t)
  if (cFee === fee) {
    a[i] = a[i] + 1
    return;
  }
  if (cFee > fee) {
    if (i >= 5) {
      return;
    }
    i++
    calcLi(fee, t, a, i);
  }
  if (cFee < fee) {
    if (a[i] >= 9) {
      if (i >= 5) {
        return;
      }
      i++
    } else {
      a[i] = a[i] + 1
    }
    calcLi(fee, t, a, i);
  }
}

</script>

<style scoped>
.el-descriptions {
  margin-top: 20px;
}

.cell-item {
  display: flex;
  align-items: center;
}

.margin-top {
  margin-top: 20px;
}
</style>
