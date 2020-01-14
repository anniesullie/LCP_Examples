# Images and Elements Removed from the DOM

## Background
The current implementation of Largest Contentful Paint in Chrome
[ignores elements which haved been removed from the DOM](https://github.com/WICG/largest-contentful-paint#ignore-removed-elements).
The reason for this is that splash screens can display large elements while a
page is loading, and Largest Contentful Paint should occur after the load.
However, this behavior comes at a cost. Carousels are often implemented by
removing the previously displayed image from the DOM. So if the largest
element on a carousel page is an image that is removed as a carousel cycles,
the LCP is updated with each carousel cycle and ends up tracking time spent
on the page.

## Analysis overview
I wanted to understand the effects that ignoring elements removed from the DOM
has on LCP. So I did the following:

  * Implemented a second version of LCP in Chrome's test framework. The original
    version takes the last LCP event generated in a Chrome trace. The modified
    version takes the largest, which would differ when the largest has been
    removed.
  * Ran the two metrics on 10,000 popular web pages.

400 pages had different results; I reviewed 200 of those manually in a modified
version of Chrome with the code for ignoring removed elements commented out.
About half the sites showed real differences in LCP (the rest were artifacts
of a changing sort order in my test).

Overall I found 4 types of behaviors, shown in detail with filmstrips below:
  * 64 sites (80%): carousels in which LCP is reported as roughly equal to the time
    spent on page when removed images are ignored.
  * 8 sites (10%): sites with placeholder images for the header image, where LCP
    is reported as the placeholder if removed images are not ignored, and the
    actual header image if removed images are ignored.
  * 5 sites (6%): a large flash of unstyled content was counted as LCP when
    removed images are not ignored; the main content is LCP when they
    are ignored.
  * 3 sites (4%): a splash screen is the LCP if removed images are not ignored;
    the main content is LCP if they are.

## Proposed next steps

I want to be sure the breakdowns in my analysis are correct:

  * ~2% of sites affected by this change
  * ~80% of affected sites (carousels) improved by this change.

So I propose implementing both versions of the metric in Chrome side by side,
and getting production data about the how many sites are affected and in which
ways.

## Detailed examples

The full list of sites I analyzed is [here](https://docs.google.com/spreadsheets/d/1GL7w_sHUXuZGm8WTEMScRowIjgJw_ktKpSj2NZyHL_I/edit?usp=sharing).

### Carousels

Here is an example of a carousel from aliyun.com. I left the page open as the
carousel shifted through three rotations. The LCP is the background of the
carousel div.
  * In green you can see the first carousel is the LCP because it is not ignored after it is removed from the DOM.
  * In pink you can see the last carousel is the LCP if images are ignored after they are removed from the DOM;
    as the carousel rotates this continuously updates.

Carousel 1 | Carousel 2 | Carousel 3
-----------|------------|-----------
![](aliyun/filmstrip_1.png) | ![](aliyun/filmstrip_2.png) | ![](aliyun/filmstrip_3.png)

### Header placeholders

Here is an example from yourstory.com.
  * In green, a placeholder for the header image is the LCP because it is not ignored after it is removed from the DOM.
  * In pink, the actual header image is the LCP as the placeholder is ignored as a candidate for LCP.

Loading | Header placeholder | Still loading | Actual header
--------|--------------------|---------------|--------------
![](yourstory/filmstrip_1.png) | ![](yourstory/filmstrip_2.png) | ![](yourstory/filmstrip_3.png) | ![](yourstory/filmstrip_4.png)

### Flash of unstyled content

Here is an example from sibnet.ru.
  * There is a flash of unstyled content as the page loads; a photo
    (highlighted in green) flashes onto the screen and disappears. This is the
    LCP if images removed from the DOM are not ignored.
  * The flash of unstyled content is removed from the DOM, so if removed images
    are ignored as candidates for LCP, the main headline image is the LCP
    (highlighted in pink).

Loading | Flash of unstyled content | Still loading | Main header
--------|---------------------------|---------------|------------
![](sibnet/filmstrip_1.png) | ![](sibnet/filmstrip_2.png) | ![](sibnet/filmstrip_3.png) | ![](sibnet/filmstrip_4.png)

## Splash screens
This is the use case that ignoring removed elements was originally intended for.
Here is an example from ih5.cn:
  * In green, a full-screen splash image is the LCP because it is not ignored
    after being removed.
  * In pink, the splash image is ignored after being removed and the main
    content of the page is the LCP.

Loading | Splash screen | Main content
--------|---------------|-------------
![](ihf/filmstrip_1.png) | ![](ihf/filmstrip_2.png) | ![](ihf/filmstrip_3.png)