Source: [HTML Coding Standards](https://make.wordpress.org/core/handbook/best-practices/coding-standards/html/)

General advices:
* when writing HTML, try to validate it with the [W3C Validator](http://validator.w3.org/);
* use 4-spaces tabs;
* all tags and attributes must be written in lowercase: `<h1></h1>` not `<H1></H1>`;
* for tags that are self-closing, the forward slash should have exactly one space preceding it: `<br />`, `<img src="" alt="" />`;
* use double-quotes for attributes: `<img src="x.jpg" />` not `<img src='y.jpg' />`;
* optional - write a HTML comment after a wrapper element to make it more clearly where it closes:
```html
<div class="wrapper">
    <div class="row">
        <div class="col">
            <input type="text" placeholder="text" />
        </div>
        <div class="col">
            <input type="text" placeholder="text" />
        </div>
        <div class="col">
            <input type="text" placeholder="text" />
        </div>
        <div class="col">
            <input type="text" placeholder="text" />
        </div>
    </div><!-- .row -->
</div><!-- .wrapper -->
```