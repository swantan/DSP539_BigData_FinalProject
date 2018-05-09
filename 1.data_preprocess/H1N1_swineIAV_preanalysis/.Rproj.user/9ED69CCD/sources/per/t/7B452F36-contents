library(tidyverse)

#load data header
flu_data <- read.csv("dataset/header_H1N1_swineIAV.csv", header = FALSE, stringsAsFactors = FALSE, sep = "|")
colnames(flu_data) <- c("ID", "Organism", "Strain Name", "Segment", "Subtype", "Host")

#check duplicates
n_occur <- data.frame(table(flu_data$`Strain Name`))
duplicate_record <- n_occur[n_occur$Freq > 1,]

#to output a list of unique protein segments
flu_proteinsegment <- unique(flu_data$Segment)
flu_proteinsegment <- flu_proteinsegment[sort.list(flu_proteinsegment)]
write.table(flu_proteinsegment, "proteinsegment.txt", sep = ",", quote = FALSE,
          col.names = FALSE, row.names = FALSE)
