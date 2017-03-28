# LuaPromise
```txt
Promise, 把回调写法转换为顺序写法的一种方式,具体可参看js里的promise
这里只是用lua的coroutine写的一个简单的基本的实现
e.g.
    function httpGetPromise(url)
        return LuaPromise.new(
            function (promise)
                http.get(
                    url,
                    function (event, response)
                        promise:resolve(response)
                    end,
                    function (event)
                        promise:reject(event.request:getErrorMessage())
                    end
                )
            end
        )
    end
    httpGetPromise("www.google.com")
        :andThen(function ( promise, data )
            print("do something to change data")
            data = "hello world..."
            return true, data
        end)
        :andThen(function ( promise, data )
            print("do something~~~", data)
        end)
        :catch(function ( msg )
            print("catched:", msg)
        end)
        :finally(function (  )
            print("finally: Over~~~~~~") 
        end)
        :done()
```

