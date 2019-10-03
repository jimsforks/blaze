# blaze

Observe changes to the URI and react to them.

## Usage

Right now `observeRoute()` and `pushRoute()` the bread and butter of {blaze}.
Use `observeRoute()` to watch for changes to the uri. 

Because {blaze} has to make use of the uri hash hack, users may not know to
include the leading `/#/`. This is why using `pushRoute()` is strongly advised.
With `pushRoute()` one of observer can have the application "browse" to a new
uri and an `observeRoute()` observer can detect this update.

## Example

Please forgive the trivial content of the tabs. In my current work, this is how
I am using `{blaze}`, changing tabs of content based on route path.

``` R
library(shiny)

shinyApp(
  ui = list(
    tags$head(
      tags$script(src = "blaze/js/blaze.js")
    ),
    fluidPage(
      tabsetPanel(
        id = "pages",
        tabPanel(
          value = "home",
          title = "Home",
          tags$p("Home")
        ),
        tabPanel(
          value = "hello_world",
          title = "World",
          tags$p("Hello, world!")
        ),
        tabPanel(
          value = "goodnight_moon",
          title = "Moon",
          tags$p("Goodnight, moon!")
        )
      )
    )
  ),
  server = function(input, output, session) {
    observeRoute("/", {
      updateTabsetPanel(session, "pages", "home")
    })
    
    observeRoute("/hello/world", {
      updateTabsetPanel(session, "pages", "hello_world")
    })
    
    observeRoute("/goodnight/moon", {
      updateTabsetPanel(session, "pages", "goodnight_moon")
    })
  }
)
```

## Installation

I couldn't say if and when this package will be on CRAN. 

```R
# install.packages("remotes")
remotes::install_github("nteetor/blaze")
```

## Other work

* [`{shiny.router}`](https://github.com/Appsilon/shiny.router) seems
  to do something similar, I needed a simpler toolset
* `{shiny}` has its own tools for retrieving the uri hash and reacting to uri
  changes, you could get by without blaze
