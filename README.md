library(shiny)  

# 定义UI  
ui <- fluidPage(  
  titlePanel("动态演示F分布"),  
  
  # 第一个滑块输入，用于调整第一个自由度df1  
  sliderInput("df1", "第一个自由度 (df1):", min = 1, max = 100, value = 10, step = 1),  
  
  # 第二个滑块输入，用于调整第二个自由度df2  
  sliderInput("df2", "第二个自由度 (df2):", min = 1, max = 100, value = 20, step = 1),  
  
  # 图形输出  
  plotOutput("fDistPlot")  
)  

# 定义服务器逻辑  
server <- function(input, output) {  
  # 反应式表达式，根据输入的自由度生成F分布数据  
  fDistData <- reactive({  
    x <- seq(0, 10, length.out = 1000)  
    data.frame(x = x, y = df(x, df1 = input$df1, df2 = input$df2))  
  })  
  
  # 绘制F分布图  
  output$fDistPlot <- renderPlot({  
    plot(fDistData(), type = 'l', col = 'blue', lwd = 2,  
         main = paste("F分布 (df1 =", input$df1, ", df2 =", input$df2, ")"),  
         xlab = "x", ylab = "密度")  
    abline(v = 1, lty = 2, col = 'gray')  # 通常在F分布中，我们不特别关注x=1这条线，但这里作为示例添加  
  })  
}  
shinyApp(ui = ui, server = server)
