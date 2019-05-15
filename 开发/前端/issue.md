\#1 axios发起一个post跨域请求 400错误   
axios请求头的 Content-Type 默认是 application/json  
postman默认的是 application/x-www-form-urlencoded  
参数类型需要填写对应的请求头，格式要转化对才能避免该问题。   
解决方法:
1. 使用new URLSearchParams()构造参数
``` JavaScript
let params = new URLSearchParams();
params.append('key1', 'value1');       //你要传给后台的参数值 key/value
params.append('key2', 'value2');
params.append('key3', 'value3');
this.$axios({
    method: 'post',
    url:url,
    data:params
}).then((res)=>{

});
```  
2. 使用qs  
npm install qs --save
import qs from 'qs'
Vue.prototype.$qs = qs  
``` JavaScript
let postData = this.$qs.stringify({
    key1:value1,
    key2:value2,
    key3:value3,
});
this.$axios({
    method: 'post',
    url:'url',
    data:postData
}).then((res)=>{

});
```

json key-value
