```js
import axios from 'axios'

/*
- Windows package.json配置
{
  "scripts": {
    "serve": "vue-cli-service serve",
    "serve:test": "set NODE_ENV=test && vue-cli-service serve",
    "serve:production": "set NODE_ENV=production && vue-cli-service serve"
  }
}

- MacOS package.json配置
{
  "scripts": {
    "serve": "vue-cli-service serve",
    "serve:test": "NODE_ENV=test vue-cli-service serve",
    "serve:production": "NODE_ENV=production vue-cli-service serve"
  }
}
 */

const baseURL = (() => {
  let baseURL = ''

  switch (process.env.NODE_ENV) {
    case 'production': // 启动生产环境
      baseURL = 'http://api.domain.com'
      break
    case 'test': // 启动测试环境
      baseURL = 'http://test.domain.com'
      break
    default: // 其它环境（一般为开发环境）
      baseURL = 'http://vue-test.oovwall.com/api'
  }
  return baseURL
})()

/**
 * 请求拦截成功
 * @param config
 */
const requestFulfilled = (config) => {
  let token = localStorage.getItem('token')
  token && (config.headers.common['Authorization'] = token)
  return config
}

/**
 * 请求拦截失败
 * @param error
 * @returns {Promise<never>}
 */
const requestRejected = (error) => {
  return Promise.reject(error)
}

/**
 * 响应拦截成功
 * @param response
 * @returns {*}
 */
const responseFulfilled = (response) => {
  return response.data
}

/**
 * 响应拦截失败
 * @param error
 * @returns {Promise<never>}
 */
const responseRejected = (error) => {
  let { response } = error

  // 服务器有返回结果
  if (response) {
    // 这里是指服务器返回错误
    switch (response.status) {
      case 401:
        // 一般指当前请求需要用户验证（一般指用户未登录）
        break
      case 403:
        // 一般TOKEN过期，跳转到登录页
        break
      case 404:
        // 找不到请求地址
        break
    }
    return Promise.reject(response)
  } else {
    // 服务器没有返回结果
    if (!window.navigator.onLine) { // 断网
      console.log('ERR_INTERNET_DISCONNECTED')
      return
    }
    return Promise.reject(error)
  }
}

/**
 * 默认axios实例
 * @type {AxiosInstance}
 */
const axiosDefault = axios.create({
  baseURL,
  timeout: 30000
})
axiosDefault.interceptors.request.use(requestFulfilled, requestRejected)
axiosDefault.interceptors.response.use(responseFulfilled, responseRejected)

export {
  axiosDefault as axios
}
```
