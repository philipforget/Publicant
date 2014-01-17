# Publicant

Publicant is a simple to use python library for generating ePub and mobi
documents from text and html content. Mobi files are generated using a binary
provided by Amazon.


## Examples
```python
from publicant import Publicant

# Simple, single chapter ePub and mobi
content = """
<h3>Publicant is neat!</h3>
<p>I should have started using this a long time ago, it's quite simple to use!</p>
"""

pub = Publicant(
    title = 'Praises For Publicant',
    chapters = [content,])

pub.write_epub()
# Specify a filepath
pub.write_mobi('simple.mobi')
```

## Requirements

* Python 2.7+
* [kindlegen](http://smile.amazon.com/gp/feature.html?docId=1000765211) for mobi generation

## Roadmap

Things to consider:

Allow for configuration of where the kindlegen binary is. I think it'd be best
to check if an environment variable is present, and default to a PATH lookup if
it's not set. Something like

```python
kindlegen_bin = os.environ.get('KINDLEGEN_BIN', 'kindlegen')
```

We also want to allow for configuration of all the possible flags for
kindlegen. I imagine something simple like so:

```python
kindlegen_defaults = {
    "compression": 1,
    "foo": "bar",
}

pub.kindlegen_options = {
    "compression": 0,    
}

# inside Publicant, wherever the mobi actually gets created
def generate_mobi(self, blah):
    options = kindlegen_defaults.update(self.kindlegen_options)
    self.call_the_damn_binary(options)
```

Creating an ePub and mobi is really just sticking the right things into the
right templates. Do we want to rely on a templating engine like jinja or just
roll with simple python string formatting with static loading of basic
templates? The templates we will need are:

For Kindle, multi-chapter:

* book.ncx
* book.opf
* content.html
* cover.html
* toc.html

single chapter:

* article.html
* article.ncx
* article.opf
* toc.html

For epub:

* content.html
* content.opf
* cover.html
* index.html
* toc.ncx
