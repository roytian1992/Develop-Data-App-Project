# ui.R
library(shiny)
shinyUI(fluidPage(
  titlePanel("Crime Distribution of Baltimore"),
  sidebarLayout(
    sidebarPanel(
      selectInput("selectYear", "Select Year:", choices = c(2012:2017)),
      selectInput("selectType", "Crime Distribution According to:", 
                choices = c('District', 'Weapon', 'Weekday', 'Month', 'Type')),
      submitButton('Confirm')
    ),
    mainPanel(
      plotOutput('plot1'),
      plotOutput('plot2')
      )
    )
  )
)



library(shiny)
library(ggplot2)
library(lubridate)
library(ggmap)

# server.R
shinyServer(function(input, output) {
output$plot1 <- renderPlot({
  YrInput <-input$selectYear
  TypeInput <- input$selectType
  
  df <- read.csv('CrimeData.csv')
  df$CrimeDate <- ymd(df$CrimeDate)
  df.selected <- df[(year(df$CrimeDate) == YrInput),]
  
  df.selected$Weekday <- weekdays(df.selected$CrimeDate)
  df.selected$Month <- month(df.selected$CrimeDate)
  df.selected$Type <- df.selected$Description
  x <- factor(unlist(df.selected[TypeInput]))
  qplot(x, geom = 'bar', fill = x, xlab = TypeInput)
  })

output$plot2 <- renderPlot({
  YrInput <-input$selectYear
  TypeInput <- input$selectType
  
  df <- read.csv('CrimeData.csv')
  df$CrimeDate <- ymd(df$CrimeDate)
  df.selected <- df[(year(df$CrimeDate) == YrInput),]
  
  df.selected$Weekday <- weekdays(df.selected$CrimeDate)
  df.selected$Month <- month(df.selected$CrimeDate)
  df.selected$Type <- df.selected$Description
  
  df.selected$cols <- factor(unlist(df.selected[TypeInput]))

  baltimore <- get_map('Baltimore', zoom = 12)
  ggmap(baltimore) + 
    geom_point(data = df.selected, 
               aes(x = Longitude, y = Latitude, color = cols), 
               alpha = 0.5, size = 0.5) +
    theme_void()
  
})

})
