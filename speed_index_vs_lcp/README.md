# Outliers where Largest Contentful Paint differs from Speed Index

If you look at the [scatter plots](https://datastudio.google.com/c/u/0/reporting/1sCHMXg3hgza3vqNVhDFAftYRgI94s2da/page/QDj4),
you'll see some dots that are outliers, where Largest Contentful Paint doesn't really match Speed Index.

I picked a few of these outliers in the mobile chart, and looked into them using the following method:
* Loaded page in Chrome devtools performance panel to find the timing and element that was the largest contentful paint
* Saved the timeline and ran it through [speedline](https://github.com/paulirish/speedline) to get the speed index.

## Outliers where Largest Contenful Paint is much later than Speed Index

HttpArchive on mobile uses a slow network connection, and the mobile outliers I saw where largest contentful paint was much later
than speed index all had a slow-loading hero image as the largest contentful paint.

### https://files.google.com

On this page, the largest contentful paint is a big hero image which takes a long time to load under slow network conditions.
Many visual updates occur during this time, and since Speed Index is an average it occurs earlier in the load.

1664ms: First paint | 3002ms: Speed Index | 5533ms: Hero image still loading | 8686ms: Hero image loaded, LCP
------------------- | ------------------- | -------------------------------- | ------------------------------
![](googlefiles/1664.png) | ![](googlefiles/3002.png) | ![](googlefiles/5533.png) | ![](googlefiles/8686.png)

### https://news.google.com

On this page, the hero image is the largest contentful paint, and it takes a long time to fully download.
It's a little difficult to see in the filmstrip that it's not loaded until the last screenshot.
Speed index occurs sooner because it's averaging when things are displayed.

7372ms | 7438ms | 7589ms | 7622ms: Speed Index | 7856ms: Hero image finishes loading
------ | ------ | ------ | ------------------- | -----------------------------------
![](googlenews/7372.png) | ![](googlenews/7438.png) | ![](googlenews/7589.png) | ![](googlenews/7622.png) | ![](googlenews/7856.png)


## Outliers where Largest Contenful Paint is much earlier than Speed Index

### https://ca.sports.yahoo.com

On this site, the hero image is replaced with an ad, so the largest contentful paint is its accompanying headline.
Most of the content is available at the time of the largest contentful paint, but speed index tracks the video ad starting to load.

**Speed Index occurs at 6241ms in this example, which is not visible in the filmstrip**

1734ms | 1935ms: LCP | 2968ms: hero image removed | 5152ms: ad starts loading | 6688ms: ad loading
------ | ----------- | -------------------------- | ------------------------- | ------------------
![](yahoosports/1734.png) | ![](yahoosports/1935.png) | ![](yahoosports/2968.png) | ![](yahoosports/5152.png) | ![](yahoosports/6688.png)

### http://wap.baidu.com/

The largest contentful paint on this page is a text paragraph. The full text content of the page paints quickly, but
smaller images load in slowly over the next few seconds, causing speed index to be later.

**Speed Index occurs at 2714ms in this example, which is not visible in the filmstrip**

1838ms | 1871ms | 2088ms: LCP | 3256ms | 3556ms
------ | ------ | ----------- | ------ | ------
![](baiduwap/1838.png) | ![](baiduwap/1871.png) | ![](baiduwap/2088.png) | ![](baiduwap/3256.png) | ![](baiduwap/3556.png)
