# WebP images with htaccess
This snippet detects if the browser [supports WebP](http://caniuse.com/#search=webp) images and then serves a .webp image instead of jpg/png if a .webp file is available at the same location as the supplied jpg/png/gif. Read more about the webp format and other ways to serve it here: [https://images.guide](https://images.guide/#how-do-i-serve-webp).

## Usage
Place the following in your .htaccess file and jpg/png/gif images will be replaced with WebP images if found in the same folder.
```apache
<IfModule mod_rewrite.c>
  RewriteEngine On

  # Check if browser supports WebP images
  RewriteCond %{HTTP_ACCEPT} image/webp

  # Check if WebP replacement image exists
  RewriteCond %{DOCUMENT_ROOT}/$1.webp -f

  # Serve WebP image instead
  RewriteRule (.+)\.(jpe?g|png|gif)$ $1.webp [T=image/webp,E=REQUEST_image]
</IfModule>

<IfModule mod_headers.c>
  # Vary: Accept for all the requests to jpeg, png and gif
  Header append Vary Accept env=REQUEST_image
</IfModule>

<IfModule mod_mime.c>
  AddType image/webp .webp
</IfModule>
```

## Preferred solution
Controlling your files using htaccess sure is fun, but a more responsible way is to use the `<picture>`-element instead of this solution. It has [great support](https://caniuse.com/webp) in all major browsers and has a built-in fallback for those without it.
```html
<picture>
  <source srcset="/path/to/image.webp" type="image/webp">
  <img src="/path/to/image.jpg" alt="">
</picture>
```
