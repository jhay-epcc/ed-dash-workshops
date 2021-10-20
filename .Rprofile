library("utils")
if (!"BiocManager" %in% rownames(installed.packages())) {
    install.packages("BiocManager")
}
download.file("https://raw.githubusercontent.com/carpentries-incubator/high-dimensional-stats-r/gh-pages/dependencies.csv", destfile="dependencies.csv")
table <- read.table('dependencies.csv')
BiocManager::install(table[[1]])

dir.create("data", showWarnings = FALSE)
data_files <- c(
    "cancer_expression.rds",
    "coefHorvath.rds",
    "methylation.rds",
    "scrnaseq.rds",
    "small_methylation.rds"
)
for (file in data_files) {
    if (!file.exists(file.path("data", file))) {
        download.file(
            url = file.path(
                "https://raw.githubusercontent.com/carpentries-incubator/high-dimensional-stats-r/gh-pages/data",
                file
            ),
            destfile = file.path("data", file)
        )
    }
}