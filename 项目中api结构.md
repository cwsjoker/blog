# API写法

项目中如果不对统一的api接口进行调用处理的话，直接将路径写在业务里面可能会出现后期难以维护的问题，
还有对ajax请求的一些统一的操作也可以提取到拦截器进行处理，集中对api进行管理。


## Fetch.js
Fetch主要发起ajax请求，这里写了比较通用的两种get和post，如有需要可自行添加。
数据的处理统一在dealRespones进行处理，在这里可以去过滤code，当然也可以在axios库的过滤器处理
``` js
import axios from './axios.js';
import { addSearch } from '../utils/operLocation.js';
export default class Fetch {
    #  get req
    static get(url, data) {
        const uri = url + addSearch(data);
        return Fetch.dealRespones(axios.get(uri, URLCONFIG));
    }
    #  post req
    static post(url, data) {
        return Fetch.dealRespones(axios.post(url, data, URLCONFIG));
    }
    # deal res
    static dealRespones(promise) {
        return promise.then(res => {
            if (res.data.code === '0') {
                # success data
                return res;
            } else {
                # deal res.code not 0
                throw new Error(res.data.message || res.data.msg)
            }
        })
    }
}
```

## Sign.js
这里主要是对项目的各个模块的ajax请求分类
``` js
import Fetch from '../fetch.js';
import serverApi from '../serverApi.js';
export default class Sign {
    # 登录
    static login(data) {
        return Fetch.post(serverApi.POST.user.login, data);
    }
}
```

## serverApi
这个文件主要是把项目中的所有api路径集合为一个对象，后续如有改动只需要改动这个文件
``` js
export default {
    GET: {},
    POST: {
        user: {
            login: 'api/user/login',
            register: 'api/user/register'
        }
    },
    PATH: {},
    DELETE: {}
}
```

## USE
业务中直接使用，引入定义好的请求模块，组装好请求数据直接使用，返回的data为在Fetch.js中的dealRespones中处理过的数据
``` js
import Sign from '../api/sign.js';
let params = {
    name: 'xxx',
    pwd: '123'
}
Sign.login(params).then(data => {
    # deal data
})
```
