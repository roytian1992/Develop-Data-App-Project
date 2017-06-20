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
