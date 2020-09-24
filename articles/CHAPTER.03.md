##token失效后，如何跳转到登录页面

**首先，更正网上的错误论点："axios单独调用中不能使用router"，其实可以使用；**
#####使用方法：
1. 直接引用
2. import router from '@/router'
3. router.push('/login')

那为什么token失效之后，没有跳转到login页面呢？
>问题在于 router/index.js 中的拦截；


    router.beforeEach((to, from, next) => {

    // 登录界面登录成功之后，会把用户信息保存在会话
    // 存在时间为会话生命周期，页面关闭即失效。
    let userName = sessionStorage.getItem('user')
    if (to.path === '/login') {
        // 如果是访问登录界面，如果用户会话信息存在，代表已登录过，跳转到主页
        if (userName) {
            next({path: '/'})
        } else {
            next()
        }
    } else {
        // 每次都会执行到这里，userName 不为空或不等于undefined,都会跳转到else 中去添加路由，
        // 解决方案：在axois中，将 sessionStorage.getItem('user')移除；sessionStorage.removeItem('user')
        if (!userName) {
            // 如果访问非登录界面，且户会话信息不存在，代表未登录，则跳转到登录界面
            next({path: '/login'})
        } else {
            // 加载动态菜单和路由
            addDynamicMenuAndRoutes(userName, to, from)
            next()
        }
    }
    })

axios.js中的代码：
          
          instance.interceptors.response.use(
              response => {
                  return response.data
              },
              err => {
                  if (err && err.response) {
  
                      switch (err.response.status) {
                          case 400:
                              err.message = '请求错误'
                              break
                          case 401:
                              err.message = '未授权，请登录'
                              break
                          case 403:
                              err.message = '拒绝访问'
                              break
                          case 404:
                              err.message = `请求地址出错: ${err.response.config.url}`
                              break
                          case 405:
                              err.message = 'token过期，请求失败，请登录'
                             // debugger
                              // 重定向到登录页面
                              Cookies.set('token', '') // 放置token到Cookie
                              sessionStorage.removeItem('user')
                              router.push('/login')
                              break
                          case 408:
                              err.message = '请求超时'
                              break
                          case 500:
                              err.message = '服务器内部错误'
                              break
                          case 501:
                              err.message = '服务未实现'
                              break
                          case 502:
                              err.message = '网关错误'
                              break
                          case 503:
                              err.message = '服务不可用'
                              break
                          case 504:
                              err.message = '网关超时'
                              break
                          case 505:
                              err.message = 'HTTP版本不受支持'
                              break
                          default:
                      }
                  }
                 // console.error(err)
                  return Promise.reject(err) // 返回接口返回的错误信息
              }
          )   
          
  