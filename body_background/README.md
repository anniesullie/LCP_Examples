# Body background images

Background images of the document's &lt;body&gt; or its &lt;html&gt; are more
likely to serve as a background for the page, and as such are not counted as
candidates for largest contenful paint. Here are some examples of filmstrips
to demonstrate the difference in largest contentful paint when invalidating
or not invalidating these images as candidates.

Methodology:
  * Run a puppeteer script on the top 800 sites ranked by
    [Quantcast](https://www.quantcast.com/top-sites/) to find ones with body
    background images, excluding adult sites.
  * Compare the largest contentful paint in Chrome stable with a modified
    version of Chrome which does not invalidate background images.
  * Show filmstrips of both versions.

Note that very few sites with body background images were found. I only found three sites
in the sample with background images that affected the timing of largest contentful paint.
However I think these three examples show clearly the value of invalidating body background
images. I did find additional examples in the tail which could be provided if needed.

## https://www.americascardroom.eu

This background image is large, and loads after the main content is displayed.
Using it as a candidate for largest contenful paint leads to a timing that is
too late. In the filmstrip below, you can see that when invalidating the background
image, largest contentful paint occurs when the main content of the page is displayed.
But if the background image is not invalidated, largest contentful paint occurs when
the subtle stars of the flag background are displayed.<br>
<img src="americascardroom/acreu_bg_flag.png" width=200>

Loading start | Background color | LCP if invalidated | LCP if not invalidated
------------- | ---------------- | ------------------ | ----------------------
![](americascardroom/filmstrip_1.jpg) | ![](americascardroom/filmstrip_2.jpg) | ![](americascardroom/filmstrip_3.jpg) | ![](americascardroom/filmstrip_4.jpg) 

## https://dailyvoice.com/

This background image is large, and depending on network speed it  can load
before or after the main content is displayed. Using it as a candidate for
largest contentful paint creates a race condition depending on when it loads,
and it ignores the fact that the main content is visible whether or not the
main content has loaded.

<img src="dailyvoice/dv-corp-mobile-v1.jpeg" width=200>

### Filmstrip when background loads quickly:

Loading start |   | LCP if not invalidated | LCP if invalidated
------------- | - | ---------------------- | ------------------
![](dailyvoice/filmstrip_1.jpg) | ![](dailyvoice/filmstrip_2.jpg) | ![](dailyvoice/filmstrip_3.jpg) | ![](dailyvoice/filmstrip_4.jpg) 

### Filmstrip when background loads slowly:

Loading start |   | LCP if invalidated | LCP if not invalidated
------------- | - | ---------------------- | ------------------
![](dailyvoice/filmstrip_1.jpg) | ![](dailyvoice/filmstrip_2.jpg) | ![](dailyvoice/filmstrip_3b.jpg) | ![](dailyvoice/filmstrip_4b.jpg) 

## https://m.coolmathgames.com/

This background image loads before the main content of the site.
Using it as a candidate for largest contentful paint leads to a timing that
is too early. (Note: the background is cropped to make this document easier
to read; it's actually much longer.)<br>
<img src="coolmathgames/bodybg.png" width=200>

Loading start | Background image (LCP if not invalidated) |   | LCP if invalidated
------------- | ----------------------------------------- | - | -------
![](coolmathgames/filmstrip_1.jpg) | ![](coolmathgames/filmstrip_2.jpg) | ![](coolmathgames/filmstrip_3.jpg) | ![](coolmathgames/filmstrip_4.jpg) 
