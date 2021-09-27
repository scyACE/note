### fetch

在了解fetch之前先回顾一下ajax的写法，并和fetch写法做一下对比

~~~javascript
const xhr = new XMLHttpRequest();

xhr.open('get', `https://api.github.com/search/users?q=${value}`);

xhr.send();

xhr.onreadystatechange = function () {
      if (xhr.readyState == 4) {
            if (xhr.status >= 200 && xhr.status < 300) {
                const response = JSON.parse(xhr.response);//返回的报文
            }
       }
}
~~~

~~~javascript
try{
       const response = await fetch(`https://api.github.com/search/users?q=${value}`);
       const result = await response.json();
       console.log(result);
}catch (e) {
            
}
~~~

fetch（关注分离的设计思想）采用了promise结构，避免了回调地狱的问题

老版本ie不支持fetch 不是使用的xhr发送的请求

~~~javascript
fetch(`https://api.github.com/search/users?q=${value}`).then(res=>{
            console.log('服务器连接成功',res);//这里并没有直接返回数据
            return res.json();//这里返回的是一个promise对象
        }).then(result=>{
            console.log('获取数据成功',result);//这里的result是响应的报文
        }).catch(reason => {
            console.error(reason); //这里返回的是报错信息
        });
~~~

