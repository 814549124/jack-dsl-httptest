描述

# 词汇

action, 命名的action, 匿名的action, 嵌套的action

action path: action1.action1_1.action1_1_1

handleRule, parallel, sequence

variable scope, 作用域为 { }

> action, 一般只调用一次
>
> 数据类型: 字符串, 数组, 对象
>
> 判断表达式: 200 == k , 200 in k, k belongTo obj, \[100 200\] in k

默认全局变量: action

# 语法实例

```

/*
注释
*/

set port 8888 // 声明和赋值
set arr \[1 2\]
set obj {
    k1 1
    k2 2
}

actions {
    baseUrl http://localhost:${port} // 注释
}

actions {
    defaultHeader Authorize auth
    defaultParam _method PUT
    
    action login {
        post /login
        param user user
        param password password
        assert {
            jsonBody _ {
                set action.login.token code
            }
        }
        
    }
    
    set v v

    action method {
        action login
    
        set v v
    
        get /method
        header header1 header1
        header header2 header2
        query 'method name' one
        param token ${action.login.token}
        assert {
            code 200
            header h1 h1
            jsonBody _ {
                assert code == 200
                assert data in one
                assert data[0] == one
            }
            assert stringBody == "{\"code\":200,\"data\":\"one\"}"
            xmlBody {
                assertXpathContent root root
            }
            // ...
        }
        
        if(code == 200) {
            action { }
        }
        
        jsonBody _ {
            if(one in data) {
                action { }
            }
            if(one == data[0]) {
                action { }
            }
        }
        
        // jsonBody.data
    }
    
    action method2 {
        post /method
        type multipart / formUrlEncoded
        file photo path contentType
        param photoName photoName
    }
}

parallel { login method }
sequence { parallel { login method } }

```