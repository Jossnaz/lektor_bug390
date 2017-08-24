# lektor_bug390

steps to reproduce error:

On lektor dev master as of 2017-08-24

clone project

cd into project

lektor server

go to http://localhost:5000

you will see a white page, that is expected

click on the edit pencil

click on the sub page: test blog entry

you will see in the console now: `Uncaught TypeError: Cannot read property 'props' of undefined`

in the source files open models/blog-post.ini


remove the two fields:

```aidl
[fields.body]
label = Body
type = flow
flow_blocks = heading,text,image,raw

[fields.categories]
label = Categories
type = checkboxes
source = site.query('/blog-categories')
width = 1/2
```

try again, no error now and the other fields will show

---

bonus bug (none the less, serious too)

Go again to the root of the admin panel

click on about -> this is a standard page.
Page inherits from page-no-replace-children.ini

but: there is no title field. There should be though.

---

bonus bug 2 (I call it bug, but it isn't so critical)

go to

models/page-frontpage.ini

remove

```aidl

[pagination]
enabled = yes
per_page = 10
```

rerun `lektor server` will lead to 

`RuntimeError: Pagination is disabled`

not sure why it is so important to have pagination enabled on all models. I think eventually I am iterating somewhere over all e.g. I do

`site.get('/').pagination.items.filter(F._model == 'blog-post')` in the models. This is because if there is no children, I have another bug that I cannot reproduce with this project here. But it's enough for now I think.
