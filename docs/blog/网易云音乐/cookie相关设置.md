# cookie相关使用

## cookie

> Cookie 是直接存储在浏览器中的一小串数据,最大不能超过 4KB。每个域下最多允许有 20+ 个左右的 cookie（具体取决于浏览器）

### options

###### key=value

存储的键值

###### path

可访问cookie的路径

> 如果一个 cookie 带有 `path=/admin` 设置，那么该 cookie 在 `/admin` 和 `/admin/something` 下都是可见的，但是在 `/home` 或 `/adminpage` 下不可见。

###### damain

可访问cookie的域名

###### expires,max-age

生命周期/过期时间

> 默认情况下，如果一个 cookie 没有设置这两个参数中的任何一个，那么在关闭浏览器之后，它就会消失。此类 cookie 被称为 "session cookie”。

###### secure

cookie只能被https访问

###### httpOnly

禁止js访问cookie

## js-cookie

简单，轻量级处理cookies的api

```js
// 设置cookie
export function setCookies(string) {
  const cookies = string.split(';;')
  cookies.map(cookie => {
    document.cookie = cookie
    const cookieKeyValue = cookie.split(';')[0].split('=')
    localStorage.setItem(`cookie-${cookieKeyValue[0]}`, cookieKeyValue[1])
  })
}

// 获得cookie
export function getCookie(key) {
  return Cookies.get(key) || localStorage.getItem(`cookie-${key}`) || undefined
}

// 移除cookie
export function removeCookie(key) {
  return Cookies.remove(key) || localStorage.removeItem(`cookie-${key}`)
}
```

## 对登录权限的验证

存在cookie则说明登录状态，网易云的部分接口需要登录(存在cookie，发送请求携带 `withCredentials: true`)才能调用

```js
// 验证是否登录
export function isLoggedIn() {
  console.log(getCookie('MUSIC_U'))
  return getCookie('MUSIC_U') !== undefined
}

//退出登录
export function doLogout() {
  logout()
  removeCookie('MUSIC_U')
  removeCookie('__csrf')
  // 清空用户数据
  rootStore.userStore.updateUserData({ key: 'user', value: {} })
  console.log('退出登录')
  console.log(getCookie('MUSIC_U'))
}
```

