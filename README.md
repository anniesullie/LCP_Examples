# LCP Examples
There have been questions around the rationale and data behind some of the decisions made in the Largest Contentful Paint metric. Here are some examples and data to help answer the questions.

* **Why are body background images invalidated as candidates for LCP?** [Examples of pages with/without body background invalidated](body_background/README.md), showing problems with not invalidating them.
* **How does LCP correlate with other performance metrics in the lab?** [Correlation study of over 4 million sites on HttpArchive](https://datastudio.google.com/reporting/1sCHMXg3hgza3vqNVhDFAftYRgI94s2da/page/i0H5). Largest contentful paint [correlates well with speed index](https://datastudio.google.com/reporting/1sCHMXg3hgza3vqNVhDFAftYRgI94s2da/page/QDj4) and [not very well with first contentful paint](https://datastudio.google.com/reporting/1sCHMXg3hgza3vqNVhDFAftYRgI94s2da/page/aWq4).
