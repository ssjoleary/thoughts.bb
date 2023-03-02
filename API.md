# thoughts.api

## `clean`

```clojure

(clean opts)
```

Removes cache and output directories
<br><sub>[source](https://github.com/ssjoleary/thoughts.bb/blob/main/src/thoughts/api.clj#L422-L428)</sub>

## `new`

```clojure

(new opts)
```

Creates new `file` in posts dir.
<br><sub>[source](https://github.com/ssjoleary/thoughts.bb/blob/main/src/thoughts/api.clj#L403-L420)</sub>

## `refresh-templates`

```clojure

(refresh-templates opts)
```

Updates to latest default templates
<br><sub>[source](https://github.com/ssjoleary/thoughts.bb/blob/main/src/thoughts/api.clj#L430-L433)</sub>

## `render`

```clojure

(render opts)
```

Renders posts declared in `posts.edn` to `out-dir`.
<br><sub>[source](https://github.com/ssjoleary/thoughts.bb/blob/main/src/thoughts/api.clj#L361-L391)</sub>

## `serve`

```clojure

(serve opts)
(serve opts block?)
```

Runs file-server on `port`.
<br><sub>[source](https://github.com/ssjoleary/thoughts.bb/blob/main/src/thoughts/api.clj#L435-L450)</sub>

## `thoughts`

```clojure

(thoughts opts)
```

Alias for `render`
<br><sub>[source](https://github.com/ssjoleary/thoughts.bb/blob/main/src/thoughts/api.clj#L393-L396)</sub>

## `watch`

```clojure

(watch opts)
```

Watches posts, templates, and assets for changes. Runs file server using
`serve`.
<br><sub>[source](https://github.com/ssjoleary/thoughts.bb/blob/main/src/thoughts/api.clj#L454-L520)</sub>

# thoughts.cli

## `-main`

```clojure

(-main & args)
```

<sub>[source](https://github.com/ssjoleary/thoughts.bb/blob/main/src/thoughts/cli.clj#L141-L142)</sub>

## `dispatch`

```clojure

(dispatch)
(dispatch default-opts & args)
```

<sub>[source](https://github.com/ssjoleary/thoughts.bb/blob/main/src/thoughts/cli.clj#L125-L131)</sub>

## `run`

```clojure

(run default-opts)
```

Meant to be called using `clj -M:thoughts`; see Quickstart > Clojure in README
<br><sub>[source](https://github.com/ssjoleary/thoughts.bb/blob/main/src/thoughts/cli.clj#L133-L139)</sub>
