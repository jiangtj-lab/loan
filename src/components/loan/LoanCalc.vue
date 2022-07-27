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

        <el-descriptions-item>
          <template #label>按年:年利率</template>
          {{ (yLi * 100).toFixed(3) }}%
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>等额本息</template>
          {{ yFA.toFixed(2) }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>总利息</template>
          {{ (yFA * time - all).toFixed(2) }}
        </el-descriptions-item>

        <el-descriptions-item>
          <template #label>按月:月利率</template>
          {{ (mLi * 100).toFixed(3) }}%
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>等额本息</template>
          {{ mFA.toFixed(2) }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>总利息</template>
          {{ (mFA * time * 12 - all).toFixed(2) }}
        </el-descriptions-item>

        <el-descriptions-item>
          <template #label>按日:日利率</template>
          {{ (dLi * 100).toFixed(3) }}%
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>等额本息</template>
          {{ dFA.toFixed(2) }}
        </el-descriptions-item>
        <el-descriptions-item>
          <template #label>总利息</template>
          {{ (dFA * time * 365 - all).toFixed(2) }}
        </el-descriptions-item>
      </el-descriptions>
    </div>
  </el-row>
</template>

<script setup lang="ts">
import { computed, Ref, ref } from "vue";
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

yFA.value = calcAmount(yLi.value, time.value)

mLi.value = Math.pow(1 + yLi.value, 1 / 12) - 1;
mFA.value = calcAmount(mLi.value, time.value * 12)

dLi.value = Math.pow(1 + yLi.value, 1 / 365) - 1;
dFA.value = calcAmount(dLi.value, time.value * 365)

// 每月还款额=贷款本金×[月利率×(1+月利率)^还款月数]÷{[(1+月利率)^还款月数]-1}
function calcAmount(li: number, t: number) {
  return all.value * li * Math.pow(1 + li, t) / (Math.pow(1 + li, t) - 1)
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
