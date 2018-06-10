how to mock remote http server

# 数据类型

string

# 语法

```

declare vname value

rule { } // 匿名
rule rule {
    header content-type application/json
    responseHeader content-type application/json
    path /get {
        method get post
        header content-type application/json
        when {
            method post
            header header header
        }
        then {
            text 'hello world'
            text ${request.param.vname}
            file filepath
            json {}
            json []
        }
    }
    
    path /get2 { }
}

```