# SQL vs. NoSQL: Relational vs. Non-Relational Databases

Lecture slides for CSCI 40500/77100 (Software Engineering), CUNY Hunter College, Fall 2026. Compares relational (SQL) and non-relational (NoSQL) databases: data models, scalability, cloud and big-data fit, complexity, Agile development, performance, and applications.

## Viewing

Open `sql_nosql.html` in any web browser. The deck uses [Slidy](https://www.w3.org/Talks/Tools/Slidy2/): advance with the arrow keys (or space), or by clicking; press `c` or the "Contents" button for the table of contents. The distributed `sql_nosql.html` is a single self-contained file and works fully offline.

## Building from Source

The slides are written in Pandoc Markdown (`sql_nosql.md`) and built with [Pandoc](https://pandoc.org) (version 2.11 or newer, which bundles the `slidy` writer, so no extra installs are needed):

```bash
make                  # build sql_nosql.html
make self-contained   # single-file HTML with resources inlined (for distribution)
```

See the `Makefile` for the exact Pandoc invocation and the other targets.

Source files:

- `sql_nosql.md`—the slide content (edit this)
- `header.html`—CSS and JavaScript injected into the document `<head>`
- `graphics/`—figures

The `make deploy` target is for the author's own web host and relies on an ssh alias (`compsci`) defined in `~/.ssh/config`; adopters can ignore it.

## Attribution

Based on the "[SQL vs NoSQL](https://s3.amazonaws.com/files.commons.gc.cuny.edu/wp-content/blogs.dir/2880/files/2021/05/SQL_NoSQL.pdf)" slides from Software Engineering (CSCI 40500/77100), Hunter College, Spring 2021. The relational-vs.-document example is adapted from [Couchbase](https://adtmag.com/articles/2016/06/22/couchbase-4-5.aspx) and redrawn as `graphics/relational-vs-document.svg`. The graph database slides are adapted from "[Graph Databases](https://www.ksi.mff.cuni.cz/~svoboda/courses/2015-1-NDBI040/lectures/Lecture-10-Graph.pdf)" by Irena Holubová (NDBI040, Charles University, 2015); their example graph, after Sadalage and Fowler's *NoSQL Distilled*, is redrawn as `graphics/social-graph.svg`.
