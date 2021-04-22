# Versioning Library for Go
![Build Status](https://github.com/6543/go-version/workflows/Release/badge.svg)
[![License: MPL](https://img.shields.io/badge/License-MPL2-red.svg)](https://opensource.org/licenses/MPL-2.0)
[![GoDoc](https://godoc.org/github.com/6543/go-version?status.svg)](https://godoc.org/github.com/6543/go-version)
[![Go Report Card](https://goreportcard.com/badge/github.com/6543/go-version)](https://goreportcard.com/report/github.com/6543/go-version)

go-version is a library for parsing versions and version constraints,
and verifying versions against a set of constraints. go-version
can sort a collection of versions properly, handles prerelease/beta
versions, can increment versions, etc.

Versions used with go-version must follow [SemVer](http://semver.org/).

# Installation and Usage

Package documentation can be found on
[GoDoc](http://godoc.org/github.com/6543/go-version).

Installation can be done with a normal `go get`:

```
$ go get github.com/6543/go-version
```

# Version Parsing and Comparison

```go
v1, err := version.NewVersion("1.2")
v2, err := version.NewVersion("1.5+metadata")

// Comparison example. There is also GreaterThan, Equal, and just
// a simple Compare that returns an int allowing easy >=, <=, etc.
if v1.LessThan(v2) {
    fmt.Printf("%s is less than %s", v1, v2)
}
```

# Version Constraints

```go
v1, err := version.NewVersion("1.2")

// Constraints example.
constraints, err := version.NewConstraint(">= 1.0, < 1.4")
if constraints.Check(v1) {
	fmt.Printf("%s satisfies constraints %s", v1, constraints)
}

### Tilde Range Comparisons (Patch)

The tilde (`~`) comparison operator is for patch level ranges when a minor
version is specified and major level changes when the minor number is missing.
For example,

* `~1.2.3` is equivalent to `>= 1.2.3, < 1.3.0`
* `~1` is equivalent to `>= 1, < 2`
* `~2.3` is equivalent to `>= 2.3, < 2.4`
* `~1.2.x` is equivalent to `>= 1.2.0, < 1.3.0`
* `~1.x` is equivalent to `>= 1, < 2`

### Caret Range Comparisons (Major)

The caret (`^`) comparison operator is for major level changes once a stable
(1.0.0) release has occurred. Prior to a 1.0.0 release the minor versions acts
as the API stability level. This is useful when comparisons of API versions as a
major change is API breaking. For example,

* `^1.2.3` is equivalent to `>= 1.2.3, < 2.0.0`
* `^1.2.x` is equivalent to `>= 1.2.0, < 2.0.0`
* `^2.3` is equivalent to `>= 2.3, < 3`
* `^2.x` is equivalent to `>= 2.0.0, < 3`
* `^0.2.3` is equivalent to `>=0.2.3 <0.3.0`
* `^0.2` is equivalent to `>=0.2.0 <0.3.0`
* `^0.0.3` is equivalent to `>=0.0.3 <0.0.4`
* `^0.0` is equivalent to `>=0.0.0 <0.1.0`
* `^0` is equivalent to `>=0.0.0 <1.0.0`
```

# Version Sorting

```go
versionsRaw := []string{"1.1", "0.7.1", "1.4-beta", "1.4", "2"}
versions := make([]*version.Version, len(versionsRaw))
for i, raw := range versionsRaw {
    v, _ := version.NewVersion(raw)
    versions[i] = v
}

// After this, the versions are properly sorted
sort.Sort(version.Collection(versions))
```

# Issues and Contributing

If you find an issue with this library, please report an issue. If you'd
like, we welcome any contributions. Fork this library and submit a pull
request.
