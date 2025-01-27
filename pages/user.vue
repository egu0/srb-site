<template>
  <div>
    <h2>User</h2>
    <ul>
      <li>
        <NuxtLink to="/user">/user</NuxtLink>
      </li>
      <li>
        <NuxtLink to="/user/user1">/user/1</NuxtLink>
      </li>
      <li>
        <NuxtLink to="/user/user2">/user/2</NuxtLink>
      </li>
    </ul>
    <!-- 出口 -->
    <h2>嵌套路由出口</h2>
    <div style="border: 1px solid pink">
      <NuxtChild />
    </div>
  </div>
</template>

<script>
export default {
  layout: 'layout1',
}
</script>
