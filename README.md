# eve-ui

⚠️ Please be advised that the original author of [eve-ui](https://github.com/quiescens/eve-ui) has marked the project as inactive, this folk is to keep the project alive and provide support.

----


A mostly standalone EVE Online fit display script for inclusion in arbitrary websites.
(Mostly standalone, requires jQuery and uses CCP's APIs but no other servers.)
Basic usage example:
```
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js" defer></script>
<script src="eve-ui.min.js" defer></script>
```
For [Next.js and other react framework, check here](#work-with-nextjs)

If you are using a strict CSP you may want to set `eveui_style=""` to prevent an error about inline styles, if you still want to use the default style you will need to link the CSS manually.

checkout https://redcokedevelopment.github.io/eve-ui-updated/examples.html to see what it looks like by default, view source to see how it works.

With the removal of the in game browser, the old shortcut of using IGB links to display ship fittings is no longer available. 
This script can be loaded in an arbitrary HTML document to automatically generate viewable, copy-pasteable fit windows for any element that it detects as a fit.

* Does not require any particular server side infrastructure on your part.
* Has no specific third party reliance aside from CCP.
* Requires jQuery to be loaded before this script.
* Pulls item info from CCP's own API which should (theoretically) always be up to date.

At present, the script should work with almost any element that has a data-dna / data-itemid / data-charid attribute containing the appropriate values (dna string or itemid), or links with a href starting with "fitting:" / "item:" / "char:", such as:
* `<img src="blah" data-dna="670::" />`
* `<div data-dna=":670::">Pod</div>`
* `<a href="fitting:670::">Pod</a>`
* `<a href="item:27740">Vindicator</a>`
* `<a href="char:90788766">CCP Testguy1</a>`

The data- format is more flexible, but the href format can be useful for supporting forums or CMS's where users might only be allowed to post URLs, or where you already have links on a site and only need to change the href.

Getting the DNA string for a fit is left as an exercise for the reader, see:
* Pyfa
* https://www.fuzzwork.co.uk/ships/dnagen.php
* Copying ingame text from a notepad or chat also works if you know how

# Build Compressed JS file

Check `Makefile`, use `make all` to generate all

# Work with Next.js

Download the `eve-ui.min.js` and drop it into `public/static` folder,
then add the following code into `pages/_app.tsx`:

```ts
export default function App({ Component, pageProps }: AppProps) {
  return (
    <div>
      <script
        src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"
        async
      ></script>
      <Script
        onReady={() => {
          // @ts-ignore - if you want to change config:
          eveui_allow_edit = true;
        }}
        type="text/javascript"
        src="./static/eve-ui.min.js"
        defer
      ></Script>
      <Component {...pageProps} />
    </div>
  );
}
```
