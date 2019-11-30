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
  * Compare filmstrips of both versions.

Note that very few sites affected by invalidating body background images were
found. We did find additional examples in the tail which could be provided if
needed.

## https://www.americascardroom.eu
## https://dailyvoice.com/
## https://m.coolmathgames.com/

Background image for coolmathgames:
![coolmathgames background image](body_background/coolmathgames/bodybg.png)

Frame 1 | Frame 2 | Frame 3 | Frame 4
------- | ------- | ------- | -------
![](body_background/coolmathgames/filmstrip_1.jpg) | ![](body_background/coolmathgames/filmstrip_2.jpg) | ![](body_background/coolmathgames/filmstrip_3.jpg) | ![](body_background/coolmathgames/filmstrip_4.jpg) 