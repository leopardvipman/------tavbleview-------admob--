1.使用的是sdwebimage.frame框架；
下面是原理和接口（不看也可以）｛
It is also possible to use the aync based image cache store independently. SDImageCache
maintains a memory cache and an optional disk cache. Disk cache write operations are performed
asynchronous so it doesn't add unnecessary latency to the UI.
　　也可以使用aync图像缓存存储独立基础。SDImageCache
　　维护一个内存缓存和一个可选的磁盘缓存。磁盘缓存执行写操作
　　异步的,不添加不必要的延迟到UI。
The SDImageCache class provides a singleton instance for convenience but you can create your own
instance if you want to create separated cache namespace.

To lookup the cache, you use the imageForKey: method. If the method returns nil, it means the cache
doesn't currently own the image. You are thus responsible for generating and caching it. The cache
key is an application unique identifier for the image to cache. It is generally the absolute URL of
the image.
　　来查找缓存,您使用imageForKey:方法。如果方法返回零,这意味着缓存
　　目前还没有自己的图像。你因此负责生成和缓存它。缓存
　　关键是一个应用程序的唯一标识图像缓存。它通常是绝对URL
　　图像。
```objective-c
SDImageCache *imageCache = [SDImageCache.alloc initWithNamespace:@"myNamespace"];
[imageCache queryDiskCacheForKey:myCacheKey done:^(UIImage *image)
{
    // image is not nil if image was found
}];
```

By default SDImageCache will lookup the disk cache if an image can't be found in the memory cache.
You can prevent this from happening by calling the alternative method `imageFromMemoryCacheForKey:`.

To store an image into the cache, you use the storeImage:forKey: method:
下面是存储办法
```objective-c
[[SDImageCache sharedImageCache] storeImage:myImage forKey:myCacheKey];
```

By default, the image will be stored in memory cache as well as on disk cache (asynchronously). If
you want only the memory cache, use the alternative method storeImage:forKey:toDisk: with a negative
third argument.
｝
2 使用办法首先把SDWebImage.framework;倒入项目
3 让后把imageio.frmework 倒入项目；
4 进行文件中的图片（option。png）的操作；
5 主要代码

- (void)configureView
{
    if (self.imageURL)
    {
        __block UIActivityIndicatorView *activityIndicator;
        [self.imageView setImageWithURL:self.imageURL placeholderImage:nil options:SDWebImageProgressiveDownload progress:^(NSUInteger receivedSize, long long expectedSize)
         {
             if (!activityIndicator)
             {
                 [self.imageView addSubview:activityIndicator = [UIActivityIndicatorView.alloc initWithActivityIndicatorStyle:UIActivityIndicatorViewStyleGray]];
                 activityIndicator.center = self.imageView.center;
                 [activityIndicator startAnimating];
             }
         }
                              completed:^(UIImage *image, NSError *error, SDImageCacheType cacheType)
         {
             [activityIndicator removeFromSuperview];
             activityIndicator = nil;
         }];
    }
}
6 我做的第一个页面是异步加在，第二个页面时缓存（模仿github做的）原网址https://github.com/rs/SDWebImage