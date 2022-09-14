
## 令牌无感知刷新

1. 将 Token 和 Token 的过期时间返回给前端，前端对每次请求进行拦截，如快过期，则生成一个新 Token 返回给前端(**前端判断, 前端请求拦截**)
2. 后端判断请求中的 Token，如快过期，则在 Response 里面返回一个 Token Header 给前端，前端根据是否有这个 Token Header，更新缓存中的 Token(**后端判断,前端响应拦截更新 Token**)
3. 后端返回一个 Access Token 和一个 Refresh Token(过期时间更长)，如果 Access Token 过期，则携带 Refresh Token 发送请求给后端的刷新 Access Token 接口，若 Refresh Token 也过期，则重新登录(**2次请求,前端响应拦截发送刷新 Token 请求**)