- 👋 Hi, I’m @jay0428
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

# Load packages 

library(shiny)
library(ggplot2)

# Get the data

file <- "https://github.com/rstudio-education/shiny-course/raw/main/movies.RData"
destfile <- "movies.RData"

download.file(file, destfile)

# Load data 

load("movies.RData")

# Define UI 

ui <- fluidPage(
  
  sidebarLayout(
    
    # Inputs: Select variables to plot
    sidebarPanel(
      
      # Select variable for y-axis
      selectInput(
        inputId = "y",
        label = "Y-axis:",
        choices = c("imdb_rating", "imdb_num_votes", "critics_score", "audience_score", "runtime"),
        selected = "audience_score"
      ),
      # Select variable for x-axis
      selectInput(
        inputId = "x",
        label = "X-axis:",
        choices = c("imdb_rating", "imdb_num_votes", "critics_score", "audience_score", "runtime"),
        selected = "critics_score"
      )
    ),
    
    # Output: Show scatterplot
    mainPanel(
      plotOutput(outputId = "scatterplot")
    )
  )
)

# Define server 

server <- function(input, output, session) {
  output$scatterplot <- renderPlot({
    ggplot(data = movies, aes_string(x = input$x, y = input$y)) +
      geom_point()
  })
}

# Create a Shiny app object 

shinyApp(ui = ui, server = server)
