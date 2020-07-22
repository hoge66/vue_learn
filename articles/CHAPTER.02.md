## 使用postman调试带token验证的接口；
###本教程的目标：解决公司后台使用jwt验证后，程序员无法通过postman工具进行接口调试的问题。
 **首先，postman工具配置；**

关键点：在请求地址栏下边，从左往右数第二个选项卡：Authorization的配置 ； 
 __具体步骤：__ 
1. 左侧Type下拉选框中： 选择OAuth2.0； 
2. 右侧 Access Token： 框中输入token的值：eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiLotoXnuqfnrqHnkIblkZgiLCJleHAiOjE1OTU0Mjk4OTYsImNyZWF0ZWQiOjE1OTUzODY2OTYxOTQsImF1dGhvcml0aWVzIjpbeyJhdXRob3JpdHkiOiIxMjMifV19.7kv02mRDljSyMboySqGISZ4tuxxiQJR5NRNJsStc6u5GI3L8H9cirgg6lmnqbkftIgW2fQK8t0dcC3dKvvHoAw 
> 该token值是在前端正常登录的时候，通过debug获取到后台传回的token值，
暂时在调试时不用改变，可以一直使用它；
 
3. ** 配置Headers项；** 
添加一项，key：Content-Type，对应value：application/json； 

 
4. ** 配置Body项；** 
 直接在raw选型卡中添加需要传递的json串； 
代码示例（department 添加）：
```java
{
  "children": [
    null
  ],
  "code": "string",
  "companyId": "string",
  "createBy": "string",
  "createDate": "2020-07-22T06:21:07.503Z",
  "delFlag": "string",
  "id": "string",
  "level": 0,
  "name": "string",
  "note": "string",
  "parentName": "string",
  "sort": 0,
  "superior": "string",
  "updateBy": "string",
  "updateDate": "2020-07-22T06:21:07.503Z"
}
```
5. 在地址栏里添加完整的，以http开头的url地址，发送方式选择POST；
6. 如果返回结果提示登录，请将右侧的Cookies 选型打开，将里面的所有项删除，再次发送请求，即可请求成功；