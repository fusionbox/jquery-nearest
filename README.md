jQuery.nearest
==============

A combination of `jQuery.find` and `jQuery.closest`, to find the element that matches the selector that is *somewhere nearby* :-)

```html
<div class="container">
  <span class="label"><label>my label</label></span>
  <span class="input"><input type="text" data-original="test" /> <a href="" style="display: hidden;" class="revert">revert</a></span>
</div>
```

```javascript
$('.container :input').on('keydown change', function()
{
  if ( this.value != $(this).data('original'))
  {
    $(this).nearest('.revert').show();
    $(this).nearest('label').addClass('changed');
  }
  else
  {
    $(this).nearest('.revert').hide();
    $(this).nearest('label').removeClass('changed');
  }
});

$('.revert').click(function()
{
  $(this).nearest(':input').val($(this).nearest(':input').data('original')).trigger('change');
});
```

And now, you could more-or-less completely change the HTML around, and your stuff won't break.  Designers and developers can hug again!


```html
<div class="container">
  <span class="label"><label>my label</label></span>
  <span class="input"><input type="text" data-original="test" /></span>
  <span class="actions"><a href="" style="display: hidden;" class="revert">revert</a></span>
</div>
```

It works by calling `jQuery.find` at the current node, then its parent, and so on until it gets a match, or it's at the top of the DOM.

If you have a **ton** of HTML elements, or it's possible the thing you're looking for isn't gonna be there,
performance takes a hit looking (eventually) at the entire DOM for your item(s).  So you can pass a "container"
as a second arg, and `nearest` will search within there first:

```javascript
$(this).nearest('.revert', '.container');
```

When you use it this way, it *almost the same* as `$(starting).closest(container).find(search)`, with the addition that if the search *is*
the container, it will be returned.
