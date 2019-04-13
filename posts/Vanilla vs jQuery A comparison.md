This isn't meant to be another *You don't need jQuery post*. I simply want to showcase some of the differences and similarities between the vanilla DOM API and the jQuery library. Which one you want to use is ultimately up to you.

## Selecting

```js
const elem = document.querySelector(".elem")
const elemAll = document.querySelectorAll(".elem")
const $elem = $(".elem")

const inside = elem.querySelectorAll(".elem_inside")
const $inside = $elem.find(".elem_inside")

// most traversing methods are quite similar:
// children/children(), nextElementSibling/next(), ...
elem.children
$elem.children()
```

jQuery selections are always collections. Most of jQuery's methods implicitly loop over all elements. Getters usually return the value of the first element in the collection.

The vanilla DOM API now includes most of jQuery's traversal methods under similar names, often as properties, like `elem.children` instead of `$elem.children()`. Some of jQuery's useful methods like `siblings()` are still not included, though.

## Modifying existing elements

```js
elem.innerHTML = "lorem <b>ipsum</b>"
$elem.html("lorem <b>ipsum</b>")

elem.classList.toggle("-modifier")
$elem.toggleClass("-modifier")

elem.style.backgroundColor = "black"
elem.style.color = "white"
$elem.css({
  backgroundColor: "black",
  color: "white"
})

elem.getAttribute("title")
$elem.attr("title")

elem.dataset.customData = "only strings"
$elem.data("customData", { complexDataPossible: true })

// jQuery returns the element, allowing one to chain methods
$elem.addClass("-new").removeClass("-old")
```

jQuery's methods often accept objects as arguments and return the element. To get the computed CSS in vanilla, you need to use `window.getComputedStyle()`. jQuery's `css()` implicitly reads computed CSS.

## Modifying the document tree

```js
// remove(), append(), prepend(), before(), after() and replaceWith()
// are all available in vanilla and jquery
elem.remove()
$elem.remove()

// appendTo(), prependTo(), insertBefore() and insertAfter()
// are available only in jquery
$elem.appendTo($(".otherElem"))

const newElem = document.createElement("div")
newElem.textContent = "foobar"
newElem.classList.add("new")
const $newElem = $('<div class="new">foobar</div>')

const clone = elem.cloneNode(true)
const $clone = $elem.clone(true) // also clones event handlers
```

Creating elements is more straightforward in jQuery and clones inherit the original element's event handlers.

## Events

```js
elem.addEventListener("click", callback)
elemAll.forEach(x => x.addEventListener("click", callback))
$elem.on("click hover", callback)
$elem.click(callback)

elem.dispatchEvent(new Event("click"))
$elem.trigger("click")
```

In jQuery, you can easily bind the same callback to multiple events in one statement. There is also no need to manually loop over the elements, jQuery does this implicitly. There are also shorthands like `click()` for convenience.

## Dimensions and scrolling

```js
elem.offsetWidth
$elem.outerWidth()

elem.scrollHeight
$elem[0].scrollHeight // use vanilla prop

elem.scrollLeft
$elem.scrollLeft()

window.scrollY
window.scrollTo(0, 100)
$(window).scrollTop(100)
```

jQuery's `scrollTop()` works for both elements and the window. In vanilla, the window has its own set of properties and methods for scrolling.

## Form elements

```js
formInputElem.value
$formInputElem.val()

// multi select
Array.from(formSelectElem.options)
  .filter(x => x.selected)
  .map(x => x.value)
$formSelectElem.val()

formCheckElem.checked
$formCheckElem.is(":checked")
```

Once again, there are a lot of similarities. jQuery makes your life a bit easier though, as seen in the multi select example. There are also additional helpers like `serialize()`.

## Animations

```js
elem.classList.toggle('-hidden') // transition prop on css class
$elem.fadeToggle()

$elem.animate({ left: "+=50" }, 300, function() {
  console.log("done")
})
```

With vanilla, it's common to just toggle classes and define the transitions and animations in CSS. jQuery offers various built in animations and the powerful function `animate()`. 

## AJAX

```js
fetch(endpoint)
  .then(resp => resp.json())
  .then(data => console.log(data))
  .catch(error => console.error(error))
  .finally(() => console.log("always"))

async function getAndLogData() {
  try {
    const resp = await fetch(endpoint)
    const data = await resp.json()
    console.log(data)
  } catch (error) {
    console.error(error)
  } finally {
    console.log("always")
  }
}
getAndLogData()

$.get(endpoint)
  .done(data => console.log(data))
  .fail(() => console.error("error"))
  .always(() => console.log("always"))

```

Vanilla allows the use of *async await*, but JSON needs to be parsed manually.

## Closing remarks

jQuery used to be indispensable for frontend development. Not only did it offer a plethora of functionality, it also took care of browser inconsistencies. Today's vanilla DOM API has come a long way and we aren't as dependent on jQuery as we used to be. But even now, jQuery makes one's life a lot easier and there's nothing wrong with using it – at least in my opinion.

