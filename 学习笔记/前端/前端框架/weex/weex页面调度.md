文件开发，打包发到cnd，
1. 请求瞎子啊
2. push刷新

首先下载所有weex页面文件的名字，md5

pushvc
先找内存，没有找bundle，没有网络下载
下载完加入磁盘，内存缓存，渲染页面


内存
队列先进先出

lru
yyChache  setObject

磁盘

网络下载

解压
BZipCompression



页面分为不同类型

boot normal lazy


didFinishLaunchingWithOptions
调用syncronizeBootAndAllPage
1. 向串行队列中添加异步任务
2. 加锁，请求boot文件



可以清除缓存


pushVC的时候
```objc
        FARM_block_resource_path callback = ^(NSString *path, NSURL *resourceURL, BOOL isSuccess, NSString *errorMsg){
            [SVProgressHUD dismiss];
            if (isSuccess && path) {
                NSURL *pathURL = [NSURL fileURLWithPath:path];
                [self pushWXVC:pathURL params:params name:name animated:animated];
            } else {
                
#ifdef DEBUG
                if (resourceURL) {
                    [self pushWXVC:resourceURL params:params name:name animated:animated];
                    return;
                }
#endif
                
                [[AppManager sharedManager] showMessage:errorMsg alternative:@"获取资源文件失败请稍后再试"];
            }
        };
        [[FaunaAppResourceManager sharedManager] getWeexResourcePath:name version:version callback:callback];
```


页面调度器的主要功能是 native页面和weex页面的管理功能。根据传入页面id加载对应的页面。以及管理weex页面的开发和调试。

 

 

[流程图]
(https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=native%26weex%E9%A1%B5%E9%9D%A2%E8%B0%83%E5%BA%A6%E5%99%A8.html#R1VrbbuM2EP0avhSoobuoR1G224dtsehise2jYjGydmXTpeU47td3hqKsG90YieykgRFQw%2BFFZ%2BbMDGkTN9k8%2FyLT3fo3kfGSOFb2TNw5cRzqWvAfBadaYAeeXUtyWWRa1gq%2BFP9wLdQD80OR8X1PsRKirIpdX7gS2y1fVbVMj02lFMe%2B2qMo%2B6vu0pyPBF9WadlIZ34r%2F1Zk1VrL7SBqO37lRb7Wi1MnqDse0tWPXIrDVq%2B4FVte92zSZhq90%2F06zcSxI3IXxE2kEFXd2jwnvERgG8yacdWp2Shx2bralPBgQ1N1Ly8Mtq8ZDO8l%2BbYH6MX5LGc0I88ARv0oZLUWudim5aKVMoUMxyms%2FvKwqjz9ifKZb3mN4C8Q%2FGzNLMtpJJ%2B5LDa84lJP8Z1X1Ul7UHqoBIjalT8Jseu9Hm5wYPj2fRuExEGutJZ%2BxSqVOddaYWsq8H8uYDPyBDqSl2lVPPWnT7Uj5me9FlFoaFDNAOu1n9Ly0Bhs4ROWEGphIw4JZSMLgE%2FtsLl5zpGas8dSHFfrVFYzgGxTbNNKAHLssSjLRJTYhlHuUv2BfF9J8YN3eiz1d%2B5puIDmgAWyAsBrtJWnt%2BJ5IYGahdiqLonD2KNo1dXkduS6Wt74jfWSW2pon7is%2BHNHNDaJ7nUCTRwdjprHY4fYDbfWHU43YewtRoyCKUlyrbMbzLhcKjO%2BjgahZeCBdyceROGN4owX2t04A2HGjt4aZkbII%2B5TI%2B%2FcCflm8UEIigNCoRGQaEGihCwiEtuE%2BdiI5iRWOpFFKOgHJeyZZcUTNHNsqmGUxMsjB%2FLqfthHV2VobbkWm4cDvAc7rouKf9mlCp0jBDhTrBgFBgPIl2MF7ccK2%2FFHwYIaYgWdIFaE9hhujdBO8gYeyXPQUd1W073fpdszfJ7VwbXXY9J2gr8PWHCw2U8Dk7w41E%2Fwc%2F2A2fc9cbx2xReGgrjz2iOvABtXffMbuEdr7g3S3TkapmWRY35agXcg1Rn6TgF1YKw7NkWWqXhi8rxz%2FWZN43tuOPA9b%2Bx7tmdwPmcK5zOVGwGJLcIibACb48WY69t0w1U3PLvn%2BuT3Og6NI8PQiC8Quofw5dqka15dhUxgjqhvDce%2FsmwIpigbxtX6NDkv6GW8D5ftvHG2o%2FeqM2zrVqD7twX9rcWdAfQwuleJ4RnDjioRdB0B5Ub33EOXBNlhrCxiigFHay%2BnLi%2FeNRo51O6HIxrerTCh9FYVeNCPRzPLpe9z0A8DQ%2BS5YJHpSRAYSdDPqcr3Y0zIC0rYHD9IlITEiSJKQpilKjiUwuCQLDzCGGHjE%2BiH9vPhYd25Z9r1buToER04etAKPkzyjd4x%2BUb%2BrUJM1Mu%2BeJnoTg%2B9C%2BCrm6wJob%2FXPWOz%2BPUpGCXLbyrWjBKzIQv%2FL%2FOtNziMuYZbw5vl2%2BhGZJi2%2Bn%2Bdr1PDjdbdMi013WiNT7kU%2FzNohJhmY9WIYkI9TLBz%2FnDIWbrnX2WpxjB0dGwsSOyr7LxU92IhMgF6UWIR6qp5ocv%2F%2BscnslgqeiQXKlnm4uBaCVZucz%2FkdIptPFLHSK%2B6CGAxglmzUF%2FO%2BbgqqAG4TBGZ%2Bvh%2B%2BE6JLiziOdL8ujL5Q5PV7ZPVt68sGiZhq%2BHaDkFmCuQAbz6icRQcf5%2FSB%2FOab3FevJS6Ddru4I6UmtC2blWiuVPGxhuHOtPJOrhXqDOcrCdywoyvir36Zu%2FdXND2%2B4Q3Xc4ZPNCewAPPrv1fyPJtFuMPEeBpVab7fbG6lJT754JzsTpMyq%2FzwOZW95pk24HON0DXyK52VL3CZ1HABi%2FdqkbewCL17vWg1iijec4X4U0QigYT1a88mgiskp46ajtU2F%2Fer%2B0P1vF6P4uARj1j6zpnSE3eBI%2FtDzxq9fYnNO7iXw%3D%3D)

 

注意：

页面调度器只打开本地weex页面。
native页面的加载不在讨论范围，一般情况下，需要在调度器初始化时注册native的页面ID。





