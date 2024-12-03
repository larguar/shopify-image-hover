# Shopify Image Hover

The Shopify Combine theme[^1] product card settings allow you to "Slide through all product images", creating a slideshow effect when a user wants to see additional product photos without clicking through to the product page. For my own shop, each product has 2 images: one photo of the product, and one photo of the product on its packaging. Visually, I like the look of the first image fading into the 2nd on hover, so I wrote this code to make that happen for this specific theme. Additionally, I've added in the hover effect to show a 2nd image in the mega menu collections list.

![HTML Badge](https://img.shields.io/badge/-HTML-323795) ![Shopify Liquid Badge](https://img.shields.io/badge/-Shopify%20Liquid-750460)


## Table of Contents 
* [Code](#code)    
* [Questions](#questions) 
* [Donate](#donate)
* [Notes](#notes)


## Code

:file_folder: **snippets/site-nav-mega.liquid**

Search for `assign image = item.featured_image` and add below:
```
assign image2 = item.metafields.custom.makers.value.collage1
```

Search for `assign image = item.featured_media` and add below:
```
assign image2 = item.media[1]
```

Search for `class="card__image product-item__image` and add around `<div>`:
```
<div class="product-image-container">

...

</div>
```

Copy full `class="card__image product-item__image` `<div>` and paste below the first[^2]. Update `if image` and `image: image` to say image2:
```
if image2 == blank
```
```
render 'lazy-image', image: image2, alt: item.title...
```

Search for `<a href="{{ item.url }}"` and remove `title="{{ item.title | escape }}"`:
```
<a href="{{ item.url }}" class="product-item">
```

:file_folder: **snippets/product-item.liquid**

Search for `{%- render 'lazy-image', image: product.featured_media` and duplicate below with if statement around it. Replace image: `product.featured_media` with `image: product.media[1]`:
```
{% if product.media[1] != blank %}
	...
{% endif %}
```
```
{%- render 'lazy-image', image: product.media[1], alt: product.title...
```

Search for first `<a href="{{ product_url }}"` and add directly under `class="product-item__image` line:
```
{% if product.media[1] == blank %} solo{% endif %}
{% if product.type == 'Bundle' and product.metafields.custom.bundle_color != blank %} bg-{{ product.metafields.custom.bundle_color }}{% endif %}
```


## Questions
If you have any questions, feel free to find me at:
* Email: laurenguardala@gmail.com
* Website: [larguar.com](https://larguar.com)
* Github: [@larguar](https://github.com/larguar)


## Donate
Appreciate this code? Say thanks with a coffee:

[![ko-fi](https://www.ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/W7W21YVJJ)


## Notes
[^1]: This code is specific to the [Combine](https://themes.shopify.com/themes/combine/styles/objects) theme in Shopify, but should work for other themes with some adjustments. File names will likely be different across themes.
[^2]: Both `<div>`s should be nested within the `<div class="product-image-container">` you added the step prior.
