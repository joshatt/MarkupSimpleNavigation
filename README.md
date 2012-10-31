
# MarkupSimpleNavigation 1.1.2

## Basic usage

Once installed you can load it on demand in your templates by using $modules->get("MarkupSimpleNavigation") method.

Arguments:
render($options, $page, $rootPage)
$options, is an array of options
$page, is a page object for the current page
$rootPage, is a page object for the root of the menu

Simplest is to call with no options. It will render an nested UL markup tree on all levels expanded from the
root page "/". Containing "parent" and "current" class attributes on anchors by default.

    $treeMenu = $modules->get("MarkupSimpleNavigation"); // load the module
    echo $treeMenu->render(); // render default menu

## Default options


```php
$options = array(
    'parent_class' => 'parent',
    'current_class' => 'current',
    'has_children_class' => 'has_children',
    'levels' => true,
    'levels_prefix' => 'level-',
    'max_levels' => null,
    'firstlast' => false,
    'collapsed' => false,
    'show_root' => false,
    'selector' => '',
    'outer_tpl' => '<ul>||</ul>',
    'inner_tpl' => '<ul>||</ul>',
    'list_tpl' => '<li%s>||</li>',
    'item_tpl' => '<a href="{url}">{title}</a>',
    'item_active_tpl' => '<a href="{url}">{title}</a>'
)
echo $treeMenu->render($options);
```
### Same with comments

```php
$options = array(

    'parent_class' => 'parent',
    // overwrite class name for current parent levels

    'current_class' => 'current',
    // overwrite current class

    'has_children_class' => 'has_children',
    // overwrite class name for entries with children

    'levels' => true,
    // wether to output "level-1, level-2, ..." as css class in links

    'levels_prefix' => 'level-',
    // prefix string that will be used for level class

    'max_levels' => null,
    // set the max level rendered

    'firstlast' => false,
    // puts last,first class to link items

    'collapsed' => false,
    // if you want to auto-collapse the tree you set this to true

    'show_root' => false,
    // set this to true if you want to rootPage to get prepended to the menu

    'selector' => '',
    // define custom PW selector, you may sanitize values from user input

    'outer_tpl' => '<ul>||</ul>',
    // template string for the outer most wrapper. || will contain entries

    'inner_tpl' => '<ul>||</ul>',
    // template string for inner wrappers. || will contain entries

    'list_tpl' => '<li%s>||</li>',
    // template string for the items. || will contain entries, %s will replaced with class="..." string

    'item_tpl' => '<a href="{url}">{title}</a>',
    // template string for the inner items. Use {anyfield} and {url}, i.e. {headline|title}, if field is of type image it will return url to image (first image if multiple)

    'item_active_tpl' => '<a href="{url}">{title}</a>'
    // template string for the active inner items.
)
echo $treeMenu->render($options);
```

## Overwrite current page or root page

If you want to overwrite starting point for the root page to be another page you could do it like this:

```php
$rootPage = $pages->get("/en/"); // or any other page that has subpages
echo $treeMenu->render(null, null, $rootPage); // null at first is to not have to specify options, just use default
```

Or to even overwrite the current active page

```php
$currentPage = $pages->get(1242);
$rootPage = $pages->get("/en/");
echo $treeMenu->render(null, $currentPage, $rootPage);
```

## Default markup output example

    <ul>
        <li><a href='/'>Home</a></li>
        <li class='parent has_children'><a href='/about/'>About22</a>
            <ul>
                <li><a href='/about/what/'>Child page example 1</a></li>
                <li><a href='/about/background/'>Child page example 2</a></li>
                <li class='parent has_children'><a href='/about/test/'>test</a>
                    <ul>
                        <li class='current'><a href='/about/test/test2/'>test2</a></li>
                    </ul>
                </li>
            </ul>
            </li>
        <li class='has_children'><a href='/templates/'>Templates</a>
            <ul>
                <li><a href='/templates/template2/'>template2</a></li>
            </ul>
        </li>
        <li><a href='/site-map/'>Site Map</a></li>
    </ul>


## Markup templates (new in 1.1.0)

If you want to define your own markup for the output you can use the options template to overwrite default.

Classes placeholder "%s"
is used in either list or inner list item to define where the classes will be appended (current, parent, etc)

Fields placeholder "{anyfield}"
Can be used on item tpl to output fields you want. You can also use {headline|title} so if headline is
empty it will chose title. If it's an image field, it will return the url to image. To output url you
just use {url}.


## Installation

1. Put the module folder named as MarkupSimpleNavigation into your site/modules/ folder
2. Go to modules install page and click "check for new modules"
3. Install the module. The module appears under "Markup" section.

(Techically you don't even need to install it, after the load call it will
be installed automaticly on first request. But it feels better). However it will not be "autoloaded" by
Processwire unless you load it in one of your php templates.
