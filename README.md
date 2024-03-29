A DIY rendering engine for all your rendering needs based on NodeJS, Express, PhantomJS and SlimerJS amongst other tools.

For a fully functioning example please visit the project http://github.com/neilellis/rendercat-example or http://github.com/neilellis/rendercat-snapshot or use the demos online at http://rendercat-example-1.neilellis.cont.tutum.io/ or http://rendercat-snapshot-1.neilellis.cont.tutum.io/

To use rendercat create a new project with a Dockerfile like this:

```Dockerfile
FROM neilellis/rendercat
```
Make sure you have directories called `public` and `modules` these will be added automatically (you don't need to put instructions in your Dockerfile). The `public` directory contains static artifacts that will be served up by rendercat's Nginx instance. The `modules` directory should contain CommonJS style modules (see http://github.com/neilellis/rendercat-snapshot for an example of layout).

Rendercat makes available the functions in the modules using the following URL format
```
  /api/<api-version>/<module>/<function>
```
The current api version is 0.1. 

The function should take only a single argument, the `rc` (Rendercat) object which also supplies a function for rendering URLs:

```JavaScript
function render(rc) {
  var req= rc.req;
  var res= rc.res;
  
  ...
  
  rc.renderUsing(url, delay, lang, width, height, viewportWidth, viewportHeight, imageType, deviceType, function (result) {
                            //If result starts with 'FAIL' then rendering failed otherwise it is the
                            //rendered file location.
                           rc.res.redirect(result.replace("/app/public/", "/"));
                           rc.res.end();
                       });
}
```

and a function for running JavaScript within PhantomJS

```JavaScript
 rc.inBrowser(function (ph) {
        ph.createPage(function (page) {
            page.open(address, function (status) {
              ...
            });
        });
  });
```


To understand how to use `ph.createPage` please read the [documentation](https://github.com/sgentle/phantomjs-node) for the phantomjs-node project.

The recommended manner for adding NodeJS modules is by adding `npm install x.y.z` instructions in the Dockerfile.

[![Dependency Status](https://www.versioneye.com/user/projects/54ae283134ff3ed1c900002a/badge.svg?style=flat)](https://www.versioneye.com/user/projects/54ae283134ff3ed1c900002a)

Released under ASL 2.0 (c) 2014-2015 Neil Ellis
