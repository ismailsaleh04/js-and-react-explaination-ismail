# Keys

## Definition

A **key** is a special prop that gives each item in a list a **stable, unique identity** among its siblings.


RULES:

- Keys must be **UNIQUE AMONT SIBLINGS** (NOT GLOBALLLY).
- Keys must be **STABLE**. Meaning the same item should always get the same key.
- Use an ID from your data. Avoid `Math.random()`; avoid array **index** when the list can be reordered, filtered, or have items inserted.
- key prop is not passed to components as a standard prop because it is a special prop consumed internally by Re