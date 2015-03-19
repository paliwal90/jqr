jqr
=======



[![Build Status](https://travis-ci.org/ropensci/jqr.png?branch=master)](https://travis-ci.org/ropensci/jqr)
[![Coverage Status](https://coveralls.io/repos/ropensci/jqr/badge.svg?branch=master)](https://coveralls.io/r/ropensci/jqr?branch=master)

R interface to jq http://stedolan.github.io/jq/

To install, after cloning the repo the first time, run (from the command line)

```
./bootstrap.R
```

which will download the 1.4 release of [jq](http://stedolan.github.io/jq/).  This does not need to be run on subsequent runs, and will stop being required once [this issue](https://github.com/ropensci/jqr/issues/1) is resolved.


```r
library("jqr")
```

## Interfaces

### low level

There's a low level interface in which you can execute `jq` code just as you would on the command line:


```r
str <- '[{
    "foo": 1,
    "bar": 2
  },
  {
    "foo": 3,
    "bar": 4
  },
  {
    "foo": 5,
    "bar": 6
}]'
```


```r
jq_(str, ".[]")
#> [1] "{\"foo\":1,\"bar\":2}" "{\"foo\":3,\"bar\":4}" "{\"foo\":5,\"bar\":6}"
```


```r
jq_(str, "[.[] | {name: .foo} | keys]")
#> [1] "[[\"name\"],[\"name\"],[\"name\"]]"
```

### high level 

The other is higher level, and uses a suite of functions to construct queries. Queries are constucted, then excuted with the function `jq()`. 

Examples:

Index


```r
x <- '[{"message": "hello", "name": "jenn"}, {"message": "world", "name": "beth"}]'
x %>% index() %>% jq
#> [1] "{\"message\":\"hello\",\"name\":\"jenn\"}"
#> [2] "{\"message\":\"world\",\"name\":\"beth\"}"
```

Sort


```r
'[8,3,null,6]' %>% sort %>% jq
#> [1] "[null,3,6,8]"
```

reverse order


```r
'[1,2,3,4]' %>%  reverse %>% jq
#> [1] "[4,3,2,1]"
```

## Meta

* Please [report any issues or bugs](https://github.com/ropensci/jqr/issues).
* License: MIT
* Get citation information for `jqr` in R doing `citation(package = 'jqr')`

[![rofooter](http://ropensci.org/public_images/github_footer.png)](http://ropensci.org)
