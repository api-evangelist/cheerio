---
title: "Cheerio 1.0 Released, Batteries Included 🔋"
url: "https://cheerio.js.org/blog/cheerio-1.0/"
date: "Wed, 07 Aug 2024 00:00:00 GMT"
author: ""
feed_url: "https://cheerio.js.org/blog/rss"
---
<p>Cheerio 1.0 is out! After 12 release candidates and just a short seven years
after the initial 1.0 release candidate, it is finally time to call Cheerio 1.0
complete. The theme for this release is &quot;batteries included&quot;, with common use
cases now supported out of the box.</p>
<p>So grab a pair of double-As, and read below for what's new, what's changed, and
how to upgrade!</p>
<!--truncate--><h2>New Website and Documentation</h2>
<p>Since the last release, we've published a new website and documentation for
Cheerio. The new site features detailed guides and API documentation to get the
most from Cheerio. Check it out at <a href="https://cheerio.js.org/">cheerio.js.org</a>.</p>
<h2>A new way to load documents</h2>
<p>Loading documents into Cheerio has been revamped. Cheerio now supports multiple
loading methods, each tailored to different use cases:</p>
<ul>
<li><code>load</code>: The classic method for parsing HTML or XML strings.</li>
<li><code>loadBuffer</code>: Works with binary data, automatically detecting the document
encoding.</li>
<li><code>stringStream</code> and <code>decodeStream</code>: Parse HTML directly from streams.</li>
<li><code>fromURL</code>: Fetch and parse HTML from a URL in one go.</li>
</ul>
<p>Dive deeper into these methods in the <a href="/docs/basics/loading">Loading Documents</a>
tutorial.</p>
<h2>Simplified Data Extraction</h2>
<p>The new <code>extract</code> method allows you to extract data from an HTML document and
store it in an object. To fetch the latest release of Cheerio from GitHub and
extract the release date and the release notes from the release page is now as
simple as:</p>
<pre><code class="language-ts">import * as cheerio from 'cheerio';

const $ = await cheerio.fromURL(
  'https://github.com/cheeriojs/cheerio/releases',
);
const data = $.extract({
  releases: [
    {
      // First, we select individual release sections.
      selector: 'section',
      // Then, we extract the release date, name, and notes from each section.
      value: {
        // Selectors are executed within the context of the selected element.
        name: 'h2',
        date: {
          selector: 'relative-time',
          // The actual release date is stored in the `datetime` attribute.
          value: 'datetime',
        },
        notes: {
          selector: '.markdown-body',
          // We are looking for the HTML content of the element.
          value: 'innerHTML',
        },
      },
    },
  ],
});
</code></pre>
<p>Read more about all of the available options in the
<a href="/docs/advanced/extract">Extracting Data</a> guide.</p>
<h2>Breaking Changes and Upgrade Guide</h2>
<p>Cheerio 1.0 introduces several breaking changes, most notably:</p>
<ul>
<li><p>The minimum NodeJS version is now 18.17 or higher.</p>
</li>
<li><p>Import paths were simplified. For example, use <code>cheerio/slim</code> instead of
<code>cheerio/lib/slim</code>.</p>
</li>
<li><p>The deprecated default Cheerio instance and static methods were removed.</p>
<p>Before, it was possible to write code like this:</p>
<pre><code class="language-ts">import cheerio, { html } from 'cheerio';

html(cheerio('&lt;test&gt;&lt;/test&gt;')); // ~ '&lt;test&gt;&lt;/test&gt;' -- NO LONGER WORKS
</code></pre>
<p>Make sure to always load documents first:</p>
<pre><code class="language-ts">import * as cheerio from 'cheerio';

cheerio.load('&lt;test&gt;&lt;/test&gt;').html();
</code></pre>
</li>
<li><p>htmlparser2 options now reside exclusively under the <code>xml</code> key:</p>
<pre><code class="language-ts">const $ = cheerio.load('&lt;html&gt;', {
  xml: {
    withStartIndices: true,
  },
});
</code></pre>
</li>
<li><p>Node types previously re-exported by Cheerio must now be imported directly
from <a href="https://github.com/fb55/domhandler"><code>domhandler</code></a>.</p>
</li>
</ul>
<p>For a comprehensive list of changes, please consult
<a href="https://github.com/cheeriojs/cheerio/releases">the changelog</a>.</p>
<h2>Upgrading to Cheerio 1.0</h2>
<p>To upgrade to Cheerio 1.0, just run:</p>
<pre><code class="language-bash">npm install cheerio@latest
</code></pre>
<h2>Get Involved</h2>
<p>Explore the new features and let us know what you think! Encounter an issue?
Report it on our
<a href="https://github.com/cheeriojs/cheerio/issues">GitHub issue tracker</a>. Have an
idea for an improvement? Pull requests welcome :)</p>
<h2>Thank You</h2>
<p>Thanks to <a href="https://github.com/jugglinmike">@jugglinmike</a> for kick-starting
Cheerio 1.0, and to all the contributors who have helped shape this release. We
couldn't have done it without you.</p>
<p>Thanks to our
<a href="https://github.com/cheeriojs/cheerio?sponsor">sponsors and backers</a> for
supporting Cheerio's development. If you use Cheerio at work, consider asking
your company to support us!</p>
<p>And finally, thank you for using Cheerio 🙇🙇‍♀️</p>
