<template>
  <el-config-provider namespace="ep">
    <BaseHeader />
    <div style="display: flex">
      <div style="width: 100%">
        <el-row justify="center">
          <el-link href="https://github.com/jiangtj-lab/loan" target="_blank">GitHub项目地址</el-link>
        </el-row>
        <LoanCalc />
      </div>
    </div>
  </el-config-provider>
</template>

<style>
#app {
  text-align: center;
  color: var(--ep-text-color-primary);
}

.element-plus-logo {
  width: 50%;
}
</style>
