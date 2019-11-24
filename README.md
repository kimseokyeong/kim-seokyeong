# kim-seokyeong
# 카카오 11/8 ~ 8/27 일별시세변동

install.packages("rvest")
library(rvest)
library(dplyr)
library(ggplot2)


url = "https://finance.naver.com/item/sise_day.nhn?code=035720&page="
page = 2:6

pages = paste0(url , page , sep = '')

extra = function(url) {
  
html = read_html(url)
link = html %>% html_nodes("table") %>% html_nodes("td") %>% html_text()  
text = gsub("(\n)" , "" , link)
text = gsub("(\t)" , "" , text)
text = text[-c(1)]
text = text[-c(74:87)]
text = text[-c(36:38)]
df = as.data.frame(matrix(text , nrow=10 , ncol=7 , byrow = TRUE))
colnames(df) = c("날짜","종가","전일비","시가","고가","저가","거래량")

nod <- html %>% html_nodes("a.hover-link")
dataIdx <- gsub("<a.*dataIdx=|&.*", "", nod)
dataIdx <- dataIdx[c(TRUE, FALSE)]
df = cbind(df)


}

result <- lapply(pages, extra)
final = do.call(rbind, result)


ggplot(data = final , aes(x=날짜 , y=종가)) + geom_point() 
