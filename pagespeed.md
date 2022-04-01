# Page Speed Tests

Page Speed tests are focused on analyzing the loading speed of a web application as experienced by an end user.  Where performance load tests focus on how well a system performs for all users, page speed tests focus on how fast the system appears to an individual user.  A Page Speed Test is especially important for Static UI Applications where the site is fully cached on a CDN as HTTP load testing a CDN is a fairly redundant task.

> Page Speed also plays an important role in [SEO](https://moz.com/beginners-guide-to-seo)

## Engineering Excellence

- Performing page speed tests while conducting load tests is a fantastic way to micro-analyze the end user’s experienced performance while the system is under production-like load.
- [Real User Monitoring (RUM)](https://stackify.com/what-is-real-user-monitoring/) provides similar insights in production with real end users using tools such as [New Relic Browser](https://newrelic.com/products/browser-monitoring).
- Lighthouse includes javascript profiling when executed within Chrome

## Reference Tools

### [Lighthouse](https://developers.google.com/web/tools/lighthouse#devtools)

Maintained by Google and commonly referred to as the “gold standard”.  Does a great job of summarizing the information into a single score.  If lighthouse’s performance test results are “green”, the website’s performance is likely sufficient (although room for improvement may exist). Focuses more on summarized information and recommendations.

- Easiest way to use: Multiple ways to execute lighthouse, including from within Chrome to test in-network applications.  See [docs](https://developers.google.com/web/tools/lighthouse) for more information.

### [Webpagetest](https://www.webpagetest.org)

The original tool and still works well.  Created by a google chrome developer. Focuses more on providing an abundance of data for the developer/engineer to diagnose how to increase improvement.  Allows running multi-page flows and tests from multiple locations around the world.

- Easiest way to use: Public hosted webpagetest.org can only target publicly available websites.

### Real User Monitoring (RUM)

Most browser-based performance monitoring, such as a New Relic javascript snippet, will track the page speed for live users. This is very beneficial to ensure the application is performing well for end users as well as validation to application support of an end users performance experience in the application.
