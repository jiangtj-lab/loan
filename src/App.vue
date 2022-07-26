<template>
  <el-config-provider namespace="ep">
    <BaseHeader />
    <div style="display: flex">
      <div>
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
