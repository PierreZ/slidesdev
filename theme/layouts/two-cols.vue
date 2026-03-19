<template>
  <div class="slidev-layout two-cols-header">
    <div class="header">
      <slot name="title" />
    </div>
    <div class="columns">
      <div class="col-left">
        <slot />
      </div>
      <div class="col-right">
        <slot name="right" />
      </div>
    </div>
  </div>
</template>
