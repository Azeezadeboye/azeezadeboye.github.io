---
title: 'Tutorial: Rapid intro to R'
date: 2021-03-14
permalink: /posts/2021/03/blog-Tutorial-Rapid-intro-to-R/
tags:
  - cool posts
  - category1
  - category2
---

#Tutorial: intro to R
# Dr. Azeez Adeboye (PhD)
# Cinvestav-Irapuato
# When I installed the R program and first saw the R console, I asked myself: now what? How do I enter my data? How can I do a simple T-test or ANOVA with this program? Even though there are many R tutorials on the internet explaining the many functions and features of R, it was quite tedious for me to start using R in a meaningful way. I had to spend over 3 weeks reading instructions and documents to get to a point where I could use R in a productive way. I do use Excel for all my basic calculations and some analysis. I like Excel very much, but I am also aware of its limitations for more advanced statistical analysis, for example for doing PCAs, K-means clustering or general linear models.
# Therefore, I decided to write this tutorial as a short cut introduction to R with practical examples of statistical analysis. My intention is not to show all the features of R, but rather to list some commands and functions that I commonly use for my work. I do research within the field of biological sciences, particularly in plant biochemistry, metabolomics, maize breeding and microarrays. 
# The R console has a prompt in which you can type the commands directly. In the following I will put some lines in blue. This refers to command code for R. The # sign denotes comments that are not interpreted by the R console. I use them here to explain some features of R. Copy the lines containing blue text and paste them into the R console. Instead of doing copy-paste from Word to R, you can also open this document in Tinn and select the blue lines and then click on the button send line to R. By observing the output you will learn to use the R program. 
# Introduction to R (Basic usage)
# R uses objects and functions. The objects contain data, while the functions use the data inside the objects and perform calculations and give outputs. If you want to see all functions of R then type:
help()                      # will open a window with general help info (list all functions)
help(var)		# specific help on the function var (variance)
help(sd)                   # specific help on the function sd (standard deviation)
?sd                   	# alternative way to call for help on the function sd

#Most functions in R need an open and closed bracket for entering variables or attributes for the respective function. This is needed even though sometimes the brackets are left empty.
#Everything you type in R is case sensitive. As in unix command line, in R you can recall previously typed commands using the up and down arrow keys.

# Defining and editing objects manually
#To work with R you first need to define objects. An object can be a variable, a vector, a dataframe or a matrix, depending on the amount of data and its structure. Choose a name for an object that is not a name for a function. Some possible names for objects are: a, b, c, x, y, z, data1, data2, matrix1, model1, etc.
b = 9    #to create a variable named b with only 1 value. If you need entering more values then use the combine function as follows:
x = c(1,3,2,7,9)    #the c function creates objects with several values (vector) 
y = c(a=1,a=3,b=2,c=7,c=9,d=2,d=3)    #define y with variable names and values
z=matrix(c(10,20,30,40,50,60),ncol=2) #define matrix with 2 columns. Values are equally distributed into the two columns. The first column is filled first and downwards.
b; x; y; z  	#show the contents of objects b, x, y and z
 
# You can choose several symbols to define objects. You can use the symbol =, but also with the symbols of <-   or also -> depending on the side in which you put the variable. I prefer to use = because it is only one keyboard strike, but for some obscure reason most R tutorials use <- .
x = c(1,3,2,7,9)    	#create an object x with 5 components
x <- c(1,3,2,7,9)    	#alternative option to create the same object x
c(1,3,2,7,9) -> x    	# alternative option to create the same object x. Note that the orientation of the symbol is inverted if the variable is on the right side

# If you are not very familiar with command line style, or if you are more used to spreadsheet style, a better option to define a matrix object is with the edit(data.frame()) function

data1 = edit(data.frame())    # opens a window to define data1 manually

# 

#After you enter the data and close the window of the spreadsheet style editor, your input is saved into the object data1. By clicking on the column labels you can define if the values should be numerical or categorical. You can give names to your variables or leave the default names (var1, var2, var3, etc). If you change the names of the variables, use a short and easy to remember name, so that you can have easy access to it later (attach function). Do not use space or strange characters for the variable names. To see what it is stored in that matrix, simply type the name of the object. Here I used “data1" but you can define any other similar name. 
# The first burden for using R is to enter large amounts of data into objects. The manual entry of data as described previously is sometimes neither practical nor convenient. There are better options to enter data from other files into R. You can import data from the windows clipboard. This works fine for small and medium size datasets. For extremely large data sets (such as microarrays with over 50 thousand data lines) you can import data from external text files. 

# Uploading data from Clipboard
# Rather than typing the data manually with the edit data.frame function, it is possible to import data from Excel or Word via the windows clipboard. For doing this, open your excel file or word document and copy the data table excluding or including row names and column headings. Then go to the R console and type the command:

 data1=read.table(file="clipboard", sep="\t")   #for importing without row names and column headings. Column variables will be named V1, V2, V3, consecutively. If the first cell is empty then this allows automatic recognition of row names and headings. This command creates the object “data1" with the contents of the windows clipboard. It transfers the data from the clipboard memory to the R program memory. This works fine for Excel, Word, PowerPoint and many other windows programs. For me this is the most convenient way to import data into R, since I do not need to create text files nor create folders or define working directories. 

data1=read.table(file="clipboard", header=T, sep="\t")   #for forcing import with row names and column headings. This option works even if the first cell is not empty.

#Example of a table with first cell empty:
	GYF	AD	ASI
A	11.00	60.0	-2.0
A	8.60	58.0	1.0
B	7.46	60.0	0.0
B	10.99	63.0	1.0
	#Table with first cell filled.
Entry	GYF	AD	ASI
A	11.00	60.0	-2.0
A	8.60	58.0	1.0
B	7.46	60.0	0.0
B	10.99	63.0	1.0


#Important notice: The text for the heading names must not include spaces or other strange characters. Also, the data matrix you copy into the clipboard must not contain empty cells. If it does, an error message will appear in the R console: Error en scan(file, what, nmax, sep, dec, quote, skip, nlines, na.strings,  :  la linea 1 no tiene 7 elementos. To avoid such error messages, do not use names with spaces and fill all empty cells in Excel with NA.

#Example of a table with empty cells 	#Corrected table with NA filled cells
Entry	G   Y	AD	ASI
A	11.00	60.0	-2.0
A	8.60	58.0	1.0
B	7.46		0.0
B	10.99	63.0	1.0
	Entry	GY	AD	ASI
A	11.00	60.0	-2.0
A	8.60	58.0	1.0
B	7.46	NA	0.0
B	10.99	63.0	1.0


# If you have many empty cells in excel, then additional options can correct data recognition and allow the import of tables with empty cells and spaces. Most importantly is the option sep="\t" to avoid importing errors.

data1=read.table(file="clipboard", header=T, sep="\t")

data1=read.table(file="clipboard", header=T, sep="\t", na.strings="")   

#Short note on data import: Some tutorials recommend you to export data from Excel sheets into text files, and then import the data back to R. This is totally unnecessary because you can use the clipboard directly for data import. Nevertheless, sometimes you will have very large amounts of data that cannot fit into the clipboard. For example, when doing microarrays of ten thousand genes. For these cases, you must use the read.table function to import your text file as follows: 
 
# Uploading data from text files
#The R program needs to define a folder to upload and save files. To change the directory in which you want to work go to the menu ->File -> Change directory and select a folder or directory of your choice. You can also do it manually on the console:

setwd("C:/Axel StatisticsR")    	# sets the working directory manually
getwd()     				#displays the path for the working directory
dir()            				# lists the files in the current work directory

# for importing data from a tab delimited text file you have several options:

data1=read.table("file.txt", header=T, sep="\t", row.names = 1)

# The header option T or F lets you specify if your data has a header in the first row. The sep="\t" option lets you define the separation symbol between values, in most cases separated by tab. The sep="\t" option will not work for files with format csv (comma separated values), in those cases use the option sep=",". The row names option lets you specify names for the rows. 
# The text file must be located in the default directory of R. Use setwd to define the correct directory. A more convenient way to upload files is with the file.choose() command. With this function you do not need to remember or type the filename manually. You can also browse the folder with windows. For importing data from standard tab delimited text file use the command:

data1=read.table(file.choose(),header=T)  # select file with a popup window

# If your datafile has no variable names in first row then use option header=F Remember that everything you type in R is case sensitive. Remember that you can recall previous commands using the up and down arrow keys to correct any typing mistakes. 

# Analysis of objects
#If you have defined an object x, you can do simple things like:
x       	  		# shows data stored in the object x on screen 
sort(x)       	  	# list data increasing order 
length(x)        	# number of items in x

#With data objects stored in the workspace one can perform many individual functions like sum(x),  max(x), min(x), median(x), mean(x), but it is better to use the summary function that integrates several of them:

summary(data1)	# shows a summary of x (min, max, mean, median, quartiles)
sd(data1, na.rm = T)    #standard deviation of elements in data1. Option of NAs removed
sd(data1)/sqrt(length(data1))	#gives the standard error of data in data1
var(data1, na.rm = T)     #variance value, or covariance matrix of elements in data1.  Option of NAs removed

# Short note on function var(). If only given one dimensional vector object, the result is the total variance of the dataset. If the object is a more dimensional matrix, then var() gives a covariance matrix, equivalent to the function cov(). In a covariance matrix, elements in diagonal are variances within that variable, and the other elements are the covariances between different variables. Covariance is a measure of how much the variables are independent or correlated. A covariance matrix can be numerically scaled to a correlation matrix with the function cov2cor().

plot(data1)     #matrix correlation plots with all elements in data1  
plot(data1[2:5])     #matrix plots with selected columns 2-5 of data1  
cor(data1, use = "pairwise.complete.obs") 	#correlation coefficient matrix of all elements in data1
round(cor(data1, use = "pairwise.complete.obs"),2)  #correlation matrix rounded to 2 decimals
round(cor(data.frame(data1$var1, data1$var2, data1$var3)),2)  #correlation matrix rounded to 2 decimals with some selected variables only
round(cor(data1[1:4]),2)  #same as previous with columns 1 to 4 selected

# Defining new objects
# Functions typed alone show the results only on the screen. If you need to do further calculations with the results of a given function, you need to use object= to define a new object which will store the output in the workspace memory. For example:

matrix1=var(data1)  # stores the covariance matrix in new object called Matrix1
matrix2=cor(data1)  # store the correlation coefficient matrix of elements in data1
matrix1; matrix2 	#shows the contents of the two newly created objects. The semicolon allows separating two functions given in the same line

#All objects are stored in the RAM memory of R. When using R no file is written to the hard-disk unless you save it specifically. When you quit R program you can save the workspace so that all objects created during the current session are available next time you work with R.

ls()			#to list all objects stored in the current workspace
rm(x)		#to remove object x from workspace
matrix1            	# to list the content of the Matrix1 object in the console window
edit(data1)	# It lets you print editions on screen but it does not save changes in the object memory. To edit and save changes use the following command:
data1=edit(data1)	# to re-edit the contents of object data1 and saving it 
fix(data1)                 # a simpler command that does the same as the above


# Working with matrices
# Perform some matrix manipulations like:
data3=2*data1   # define a new matrix with scalar matrix multiplication by factor 2
data4=data1+data3 	# matrix addition
data5=solve(data1)	 # define a new matrix using the inverse matrix of data1
data2=t(data1)   # define a new matrix using the transposed matrix from data1

data6=data.frame(data1,data3)   # use data.frame for adding elements to objects

# Extracting the component of a matrix involves one or two indices [row, column]. 
data1[2:3]   #elements of data1 at the second and third columns
z=data1[2:3]   #define new object with selected columns from data1
data1[2,3]   #element of data1 at the second row, third column


data3=read.table(file="clipboard", header=F, sep="\t")  # import data without header
data3=data.matrix(data3)  # convert data.frame to a data matrix.

# Eigenvalues and eigenvectors of a matrix is handled by eigen() function: 
m2=matrix(c(10,20,30,40),ncol=2)
eigen(m2)

#Attach and detach
# The attach function is used to access the variables inside an object. Use detach to get rid of those variables.
attach(data1)   # allows direct access to the variables defined inside an object
hist(var1)   	# plots an histogram of variable var1 inside attached object data1
detach(data1)   	# detaches variables
hist(data1$var1)   	# plots an histogram of var1 from data1 without attaching

#Conditional functions
#conditional recoding of values within an object
data1= x = rnorm(1000)     #create a set of 1000 random values
x[x > 1.2] = NA         # to recode all values of x > 1.2 to NA

data1= data.frame(a = rnorm(1000), b= rnorm(1000) , d=rnorm(1000), entry=c(1:10))
data1$a[data1$b > 1] = 5         # to recode all values of b > 1 to 5 

data5$avRep=ave(data5$Yield,data5$Rep)  # create new column with averages among Reps
data5$avEntry=ave(data5$Yield,data5$Entry) # create new column with average among Entries
 
#Creating graphs
# There are many simple functions to plot graphs using the data in the objects.

plot(data1)   	#graphical output with plot arrays of elements in data1. This graphical feature is particularly impressive since it plots all variables of the matrix against all variables. With this feature of R you can do exploratory analysis and discover some hidden correlations. If you want to do this in Excel, you will need much more time. (later in the tutorial I will present a more informative version of multiple scatterplots)
 
#Later in the tutorial I will explain how to make special scatterplots.
boxplot(data1)   	#graphical output with boxplots of all elements in data1
attach(data1)	  #to make the variables defined in data1 accesible to the console
var1  # to show the contents of var1  (defined inside attached data1)
boxplot(var1, var2)  #produces boxplots of var1 and var2
boxplot(var1 ~ var2)  #produces a boxplot of var1 grouped by factor var2
plot(data.frame(var1, var2, var3))   #plot arrays of some selected elements only.
plot(data1[1:4])   #plot only selected columns 1 to 4 of data1
detach(data1) #to detach the variables defined in data1
data1$var1  # to show the contents of var1 from data1 without the attach function

 

# If you want to get the statistics involved in the boxplots, the following commands show them: 
b=boxplot(data1); b$stats # gives the value of the lower end of the whisker, the first quartile (25th percentile), second quartile (median=50th percentile), third quartile (75th percentile), and the upper end of the whisker.
# plot() is a general graphic command with numerous options. 
plot(x) 		# plots the data x
plot(var1, var2)  	# plots var1 in dependence of var2
abline(line(var1,var2))  		# add a regression line
plot(y,z, main="Enter Title Here")  # scatterplot with variables y and z
fit=lm(y~z)   #A fitted straight line is shown in the plot by executing two more commands
abline(fit) 

# Exporting graphs
#Right clicking anywhere inside the active graphics window shows a context sensitive menu, allowing saving the plot as metafile (EMF) or postscript format (PS). The options Copy as metafile or Copy as bitmap (BMP) puts the graphics in the clipboard. You can then paste it in some applications, e.g., MS Word. More graphical formats are available from the main menu. While the graphic window is active, click File| Save As from the menu and it lists six file formats (metafile, postscript, PDF, PNG, BMP, and JPG at three quality levels) in total so you have plenty of choices. 
#Which is the best choice for the graphic format? In general metafile format retains graphic quality even when it is resized in the application. On the other hand, JPG is a very popular choice because the file size is usually much smaller and internet compatible. Except for rare circumstances, I would not recommend bitmap file format because it is usually very large and shows very poor picture quality when resized. Postscript file format is useful when including the graphic file in another postscript file or when postscript printer is available. Picture quality does not deteriorate when resized. 
# Use metafile format for MS office applications (e.g. Word, Powerpoint). Within the graphic window, right click the mouse button and select copy as metafile. This will transfer the figure into the windows clipboard. Go to Powerpoint and paste as metafile. For publications save graphics in jpeg format.

# Exporting data
write.table(x, file = "dataOut.txt", append = FALSE, col.names = NA, sep = " ")
# with the above commands a textfile will be created in in the working directory. To change the working directory use the following
getwd()     				#displays the path for the working directory
dir()            				# lists the files in the current work directory
setwd("C:/Axel StatisticsR")    	# sets the working directory manually
# open the text files in excel using the text import wizard.

#Handling of R Packages
#There are dedicated webpages for specific statistical topics. For example, for the analysis of microarray data with R, consult: http://bioconductor.org/ 
#Download R packages from diverse websites and save them in a folder as zip files. Within the R program use the option install from local zip files. Install packages from R directly with these commands:
install.packages("corrgram")
install.packages("agricolae")
install.packages("gplots")
install.packages("UsingR")
install.packages("lattice")
install.packages(“maanova")
# Once you have downloaded the packages, activate them with the library function. Then you can use the extended functions in those packages.

library(agricolae)    	#load and activate the library
library(corrgram); library(gplots);library(UsingR);library(lattice);library(maanova)

# Example of group comparisons (agricolae)
data1=read.table(file="clipboard", header=T, sep="\t")  # import data
attach(data1)	  #to make the variables defined in data1 accessible to the console
library(agricolae)    	#load and activate library
model1=aov(Yield~Gen*Env)  #define a model for variance analysis
df=df.residual(model1)
MSerror=deviance(model1)/df
comparison = HSD.test(Yield,Gen,df,MSerror, group=TRUE)
bar.err(comparison,std=TRUE)  #for plotting a graph

#Make an additional LSD test with:
comparison = LSD.test(Yield,Gen,df,MSerror, p.adj="bonferroni", group=FALSE)
bar.err(comparison,std=TRUE)  #for plotting a graph
#For adjusting the graph you can modify some options like:
bar.err(comparison,std=TRUE,ylim=c(0,45),density=4,border="blue")

#AMMIS biplots:
model= AMMI(Env, Entry, Rep, Yield, graph="biplot",number=FALSE)
#For adjusting the graph you can modify some options like:
model= AMMI(Env, Entry, Rep, Yield, xlim=c(-4,4),ylim=c(-4,4), graph="biplot",number=FALSE)

# Example (Pairwise.T.test)
data1=read.table(file="clipboard", header=T, sep="\t")  # import data
attach(data1)	  #to make the variables defined in data1 accessible to the console
pairwise.t.test(Var1, Var2)  # make t.tests of Var1 grouped by category Var2

# if you want to export the data you need to use following commands:
x=pairwise.t.test(Var1, Var2)  # create an object x for exporting
write.table(x$p.value, file = "dataOutPairwise.txt", append = FALSE, col.names = NA, sep = " ")
# a textfile will be created in in the working directory. 
getwd()     				#displays the path for the working directory
dir()            				# lists the files in the current work directory
# open the text file in excel using the text import wizard.

# Another option to make multiple comparisons.
model1=aov(var1~var2)
TukeyHSD(model1, "var1")

#Example PCA analysis (bioconductor)
#first you need to define the source to download the package 
source("http://bioconductor.org/biocLite.R")
biocLite("pcaMethods")   #downloading needs only to be done the first time
library(pcaMethods) #load the library. Necessary to do it every time you run R

# For importing data you have 3 options 
## 1) load the matrix with data. Considering that row names are present
data=read.table(file.choose(), header=T, sep="\t", row.names=1)

## or 2) not considereing the ID of metabolites or genes
data=read.table(file.choose(), header=T, sep="\t")

## or 3) importing data from clipboard
data=read.table("clipboard", header=T, sep="\t")

# depends on the type of data, you can use:
## 1) variables in the rows and samples in the columns 
md = prep(t(data), scale = "none", center = TRUE)
## or 2) variables in the columns and samples in the rows 
md = prep((data), scale = "none", center = TRUE)

## then do the analysis. In this case for the first 5 principal components.
resPPCA = pca(md, method = "ppca", center = FALSE, nPcs = 5)
slplot(resPPCA) # to plot the results
resPPCA@scores # to see all the principal components (treatment) 
resPPCA@loadings  # to see all the loadings

#Example PCA analysis (native R)
data=read.table("clipboard", header=T, sep="\t") #importing data from clipboard

# depends on the dataframe you have:
## 1) variables in the rows and samples in the columns 
md = prep(t(data), scale = "none", center = TRUE)
## or 2) variables in the columns and samples in the rows 
md = prep((data), scale = "none", center = TRUE)
md=data.frame(md[,2],md[,3],md[,4],md[,5],md[,6],md[,7])

## Analysis for 2 principal components.
pc=princomp(md, scale=TRUE)
loadings(pc)
biplot(pc)
pc
summary(pc)

#Example AMMIS analysis (agricolae)
data5=read.table(file="clipboard", header=T, sep="\t")  # import data
# data must be in the following format:
Rep	Plot	Entry	Env	Yield
1	1	A	E1	36.3
2	4	A	E1	37.9
1	2	B	E1	49.9
2	5	B	E1	50.3

attach(data5)	  #to make the variables defined in data1 accessible to the console
library(agricolae)    	#load and activate library
model= AMMI(Env, Entry, Rep, Yield, graph="biplot",number=FALSE)
#For changing the range of the axes use xlim and ylim:
model= AMMI(Env, Entry, Rep, Yield, xlim=c(-3,3),ylim=c(-4,4), graph="biplot",number=FALSE)

#Example: Conditional Averaging

data1$var3=ave(data1$var1,data1$var2)  # create new column named var10 with averages of var1 grouped by var2
attach(data1); data1$var3=ave(var1,var2)  # same as above but using attach function
data5$avEntry=ave(data5$Yield,data5$Entry) # create new column with average of Yield grouped by entries

#Tests for significance
t.test(var1~var2)    	#T.test within data of variable1 grouped by categories specified in variable 2. Grouping factor of var2 must have exactly 2 levels.

# Note the important difference between the t.test(var1,var2) and t.test(var1~var2)  since in the first case you are comparing all values of var1 against all values of var2. In the second case, you make subgroups of the values contained in var1 depending in the grouping levels defined in var2. If you have more than 2 grouping levels in var2, then it is better to use the pairwise.t.test function.

pairwise.t.test(var1, var2)  # make t.tests of var1 grouped by categories var2. The result is a pairwise comparison matrix. Note the comma in (var1, var2)  

ave(var1, var2)  # make averages of var1 grouped by factor categories var2. The result is a vector of the same length as var1.

#To make an analysis of variance:
a=aov(var1~var2+var3+var4)  	#defines a new object a with the results of the analysis of variance
a                     	#lists the content of the previously created object a
summary(a)    	#lists the summary of the statistical results of object a
attributes(a)    	#lists all attributes of new object

#To make a linear model:
model1=lm(var1 ~var2+var3+var4)
summary(model1)    # show summary of the linear model
attributes(model1)    # shows all attributes of the new object

#Making special graphics

# Multiple plots per page 
#You can combine multiple plots into one overall graph, using either the 
par( ) or layout( ) function. With the par( ) function, the option mfrow=c(nrows, ncols) creates a matrix of nrows x ncols plots that are filled in by row. mfcol=c(nrows, ncols) fills in the matrix by columns.

par(mfrow=c(2,2))
plot(var1,var2, main="Title plot1")
plot(var1,var3, main="Title plot2")
hist(var1, main="Title Histogram")
boxplot(var1, main="Title Boxplot", xlab="label1", ylab="label2")

# The layout( ) function has the form layout(mat) where mat is a matrix object specifying the location of the N figures to plot.
# Example of one figure in row 1 and two figures in row 2

layout(matrix(c(1,1,2,3), 2, 2, byrow = TRUE))
hist(var1)
hist(var2)
hist(var3)

# Histograms
# How many bins should an histrogram contain? This depends on the number of observations n. Use the approximation of Sturges k= log2(n)+1
#This means that for 100 observations you should use around 7-8 bins
# Sturges, H. A. (1926). "The choice of a class interval". J. American Statistical Association: 65–66.  

# Example (gplots)
data1=read.table(file="clipboard", header=T, sep="\t")   # import data 
attach(data1)	  #to make the variables defined in data1 accessible to the console
library(gplots)    	#load extended plot library
plotmeans(var1~var2)    	#plot means with error bars
 

# Example (lattice)
x=read.table(file="clipboard", header=T, sep="\t")   # import data 
attach(x)	  		#make variables accessible
library(lattice)    		#load library
xyplot(var1~ var2)
 
dotplot(var1~ var2)
barchart(var1~ var2)
stripplot(var1~ var2)
bwplot(var1~ var2)
 

# Example (Simple)
install.packages("UsingR")
require(UsingR)
data1=read.table("clipboard", header=T, sep="\t")
attach(data1)
simple.violinplot(var1 ~ var2, col=8) 
 
par(new=TRUE) #for adding to an existing graph. Next function will add.
plot(var1 ~ var2, add = TRUE) # so that a new plot is not started

# Example (ellipse)
data1=read.table("clipboard", header=T, sep="\t")
library(ellipse)
plotcorr(cor(data1, use = "pairwise.complete.obs"))
 
library(corrgram)
corrgram(data1)


#Trellis graphs
#In order to produce Trellis plots you must load the “Lattice" library and start a “trellis aware" device. 
library(lattice)
trellis.device()

#Make conditional plots with the coplot function
coplot(var1 ~ var2 | var3)  # plot var1 against var2, subdivided in categories var3
 
# It is possible to use two conditional variables in the form of var3*var4
coplot(var1 ~ var2 | var3*var4)  # plot var1 against var2, subdivided in categories var3 and var4
 

#Options for the coplot function: 
coplot(formula, data, given.values, panel = points, rows, columns, show.given = TRUE, col = par("fg"), pch = par("pch"), xlab = paste("Given :", a.name), ylab = paste("Given :", b.name), number = 6, overlap = 0.5, ...) co.intervals(x, number = 6, overlap = 0.5)
# formula 

# Add a trendline
#Use the functions lm() and abline() to add a trendline to an existing plot. 
data1=read.table("clipboard", header=T, sep="\t")
attach(data1)
plot(var1, var2)
model1=lm(var1~var2)
abline(model1,lwd=2)         #The lwd specifies the line width
model1         			#To show the slope and intercept of the trendline

# To add text to a graphic
text (x, ...) text.default (x, y = NULL, labels = seq(along = x), adj = NULL, pos = NULL, offset = 0.5, vfont = NULL, cex = 1, col = NULL, font = NULL, xpd = NULL)

mtext(text, side = 3, line = 0, outer = FALSE, at = NULL, adj = NA, cex = NA, col = NA, font = NA, vfont = NULL)
title(main="Name")



# 3D Perspective Plot
0.841	0.746	0.678	0.628	0.587	0.554	0.526	0.502
0.909	0.789	0.710	0.652	0.606	0.570	0.540	0.514
0.141	0.141	0.140	0.140	0.139	0.139	0.138	0.138
-0.757	-0.687	-0.634	-0.592	-0.558	-0.530	-0.505	-0.484
-0.959	-0.819	-0.730	-0.667	-0.619	-0.580	-0.548	-0.521
-0.279	-0.276	-0.272	-0.269	-0.266	-0.263	-0.260	-0.257
0.657	0.611	0.573	0.543	0.516	0.494	0.474	0.456
0.989	0.836	0.742	0.676	0.625	0.585	0.553	0.525
0.412	0.401	0.390	0.380	0.371	0.363	0.355	0.347
-0.544	-0.518	-0.495	-0.475	-0.457	-0.441	-0.427	-0.414

data3=read.table(file="clipboard", header=F, sep="\t")  # import data without header. It is important that it is without header in order to convert it to a matrix.
data3=data.matrix(data3)  # convert table to a data matrix. Important step because otherwise data will be treated as a dataframe and not as a datamatrix.
x=c(1:10)  # define x variable. In this case 10 values
y=c(1:8)  # define y variable. In this case 8 values
par(mfrow=c(2,2))  # for plotting 4 graphs in one page
contour(x, y, data3, col="blue")   # plot contour
image(x, y, data3)  # plot image
persp(x, y, data3, col="blue", theta = -20, phi = 15, scale = TRUE, axes = TRUE)
 

# Special Scatterplots
## In order to do special scatterplots, you need first to define extra functions, and then call them from within the function pairs.
## Extra function to put histograms on the diagonal
panel.hist = function(x, ...)
{
    usr = par("usr"); on.exit(par(usr))
    par(usr = c(usr[1:2], 0, 1.5) )
    h = hist(x, plot = FALSE)
    breaks = h$breaks 
    nB = length(breaks)
    y = h$counts; y = y/max(y)
    rect(breaks[-nB], 0, breaks[-1], y, col="cyan", ...)
}
## Extra function to put correlations R on the upper panels
panel.cor = function(x, y, digits=2, prefix="R=", cex.cor)
{
    usr = par("usr"); on.exit(par(usr))
    par(usr = c(0, 1, 0, 1))
    r = abs(cor(x, y, use="pairwise.complete.obs"))
    txt = format(c(r, 0.123456789), digits=digits)[1]
    txt = paste(prefix, txt, sep="")
    if(missing(cex.cor)) cex.cor = 0.8/strwidth(txt)
    text(0.5, 0.5, txt, cex = cex.cor)
}

## Extra function to put R2 values instead of simple R values. It also puts p values.
panel.cor2 = function(x, y, digits=2, prefix="", cex.cor)
{
    usr = par("usr"); on.exit(par(usr))
    par(usr = c(0, 1, 0, 1))
    r = abs(cor(x, y, use="pairwise.complete.obs"))
    r2 = abs(cor(x, y, use="pairwise.complete.obs")*cor(x,y, use="pairwise.complete.obs"))
    txt = format(c(r2, 0.123456789), digits=digits)[1]
    txt = paste(prefix, txt, sep="")
    if(missing(cex.cor)) cex.cor = 0.8/strwidth(txt)
    text(0.5, 0.5, txt, cex = cex.cor)
    modelo=summary(lm(x~y))
    valorP=signif(modelo$coefficients[8], digits=digits)    
    text(0.7, 0.2, valorP)
}

## To put histograms in diagonal and smooth lines in lower panel:
pairs(USJudgeRatings[1:5], panel=panel.smooth, cex = 1.5, pch = 1, bg="light blue", diag.panel=panel.hist, cex.labels = 2, font.labels=2)

## To put correlations on the upper panels and smooth lines in lower panel:
pairs(USJudgeRatings[1:6], lower.panel=panel.smooth, upper.panel=panel.cor)

## To put correlations R2 on the upper panels and smooth lines in lower panel:
pairs(USJudgeRatings[1:6], lower.panel=panel.smooth, upper.panel=panel.cor2)

## To put correlations R on the upper panels, histograms in diagonal, and smooth lines in lower panel:
pairs(USJudgeRatings[1:5], lower.panel=panel.smooth, upper.panel=panel.cor, diag.panel=panel.hist)

## To put correlations R2 on the upper panels, histograms in diagonal, and smooth lines in lower panel:
pairs(USJudgeRatings[1:5], lower.panel=panel.smooth, upper.panel=panel.cor2, diag.panel=panel.hist)

# Exercises
# Example of Graph:
p <- c(0:1000)/1000
x <- 6*(p^2)*((1-p)^2)
plot(p,x,col="red",type="l")
title(main="Likelyhood function", ylab="Likelyhood: L(p)")
grid()
 
# Example of data import and analysis: To perform statistical tests, first copy the data into the clipboard and then use the following commands in R:
data1=read.table(file="clipboard", header=T, sep="\t")   
summary(data1)    	#lists a summary of the data
attach(data1)	  #to make the variables defined in data1 accessible to the console
t.test(var1,var2)    	#T.test between data contained in two variables. Use the names of the variables inside the object. In my case they were named as var var2. 

#Examples & Exercises
Make	Cylinder	Weight	Mileage	Type
Volkswagen	V4	2330	26	Small
Ford	V4	2345	33	Small
Eagle	V4	2560	33	Small
Acura	V6	3265	20	Medium
Buick	V6	3325	23	Large
Chrysler	V6	3450	22	Medium

#Copy the above table from word and then import data using the command read.table(file="clipboard" header=T) in the R console as following:

CarData=read.table(file="clipboard", header=T)

# Boxplots are very useful when comparing grouped data. For example, side-by-side boxplots of weights grouped by vehicle types are shown below: 

attach(CarData)
boxplot(Weight ~Type)   		# create boxplot of Weight grouped by Type
title("Weight by Vehicle Types")  	# put title to the graph

boxplot(Mileage ~Cylinder) 
title("Mileage by Cylinder")

plot(Mileage, Weight) 		# create a x,y scatterplot
title("Mileage by weight") 	# put title to the graph

#Example of AMMIS

Rep	Plot	Entry	Env	Yield
1	1	A	E1	36.3
2	4	A	E1	37.9
1	2	B	E1	49.9
2	5	B	E1	50.3
1	3	C	E1	80.1
2	6	C	E1	82.3
1	1	A	E2	44.4
2	4	A	E2	46.3
1	2	B	E2	22.3
2	5	B	E2	20.8
1	3	C	E2	85.4
2	6	C	E2	86.7
1	1	A	E3	82.4
2	4	A	E3	86.3
1	2	B	E3	92.3
2	5	B	E3	92.8
1	3	C	E3	90.3
2	6	C	E3	94.6

#copy the above data into clipboard and the type the following commands 
data5=read.table(file="clipboard", header=T, sep="\t")  # import data
attach(data5)	  #to make the variables defined in data1 accessible to the console
library(agricolae)    	#load and activate library
model= AMMI(Env, Entry, Rep, Yield, graph="biplot",number=FALSE)
 
#For changing the range of the axes use xlim and ylim:
model= AMMI(Env, Entry, Rep, Yield, xlim=c(-3,3),ylim=c(-4,4), graph="biplot",number=FALSE)

#Conditional averaging
# use the same datatable as above (data5)
data5=read.table(file="clipboard", header=T, sep="\t")  # import data

data5$avRep=ave(data5$Yield,data5$Rep)  # create new column with averages among Reps
data5$avEntry=ave(data5$Yield,data5$Entry) # create new column with average among Entries

#Example of grain yield trial data

REP	BLK	PLOT	ENTRY	RANGE	LONGROW	GYF	AD	ASI	PH	EH	EPO	EPP
1	1	1	6	1	1	11.00	60.0	-2.0	185.0	96.0	0.52	1.35
1	1	2	5	1	2	8.60	58.0	1.0	194.0	99.0	0.51	0.96
1	1	3	2	1	3	7.46	60.0	0.0	165.0	70.0	0.42	1.00
1	1	4	28	1	4	10.99	63.0	1.0	192.0	109.0	0.57	1.08
1	1	5	11	1	5	11.23	63.0	-2.0	224.0	120.0	0.54	1.04
1	2	6	12	1	6	11.32	64.0	1.0	205.0	109.0	0.53	1.00
1	2	7	1	1	7	8.20	60.0	0.0	210.0	109.0	0.52	0.91
1	2	8	19	1	8	10.01	61.0	3.0	215.0	113.0	0.53	0.96
1	2	9	27	1	9	12.46	63.0	1.0	217.0	109.0	0.50	1.00
1	2	10	26	1	10	13.19	64.0	0.0	215.0	113.0	0.53	1.24
1	3	11	25	1	11	10.23	64.0	-2.0	233.0	108.0	0.46	1.00
1	3	12	9	1	12	11.64	63.0	-3.0	180.0	85.0	0.47	1.13
1	3	13	4	1	13	7.08	60.0	1.0	205.0	82.0	0.40	1.40
1	3	14	18	1	14	5.32	66.0	0.0	208.0	92.0	0.44	1.07
1	3	15	13	1	15	8.96	65.0	0.0	213.0	103.0	0.48	1.00
1	4	16	3	1	16	9.61	60.0	1.0	215.0	113.0	0.53	1.04
1	4	17	14	1	17	11.20	60.0	1.0	203.0	92.0	0.45	1.00
1	4	18	30	1	18	11.98	65.0	0.0	209.0	102.0	0.49	0.91
1	4	19	20	1	19	5.98	64.0	1.0	203.0	109.0	0.54	0.85
1	4	20	7	1	20	6.53	64.0	1.0	174.0	90.0	0.52	0.94
1	5	21	23	2	20	7.55	64.0	1.0	195.0	88.0	0.45	1.12
1	5	22	10	2	19	11.30	64.0	1.0	213.0	106.0	0.50	0.96
1	5	23	17	2	18	9.00	63.0	1.0	218.0	106.0	0.49	1.00
1	5	24	8	2	17	8.41	66.0	0.0	204.0	93.0	0.46	0.96
1	5	25	24	2	16	10.06	66.0	0.0	230.0	115.0	0.50	1.00
1	6	26	29	2	15	11.52	65.0	1.0	210.0	118.0	0.56	1.09
1	6	27	21	2	14	6.82	61.0	1.0	208.0	104.0	0.50	0.76
1	6	28	16	2	13	12.93	60.0	1.0	203.0	105.0	0.52	0.96
1	6	29	15	2	12	7.36	61.0	1.0	212.0	113.0	0.53	0.87
1	6	30	22	2	11	9.66	60.0	1.0	208.0	107.0	0.51	0.92
2	1	31	28	2	10	12.25	60.0	1.0	239.0	130.0	0.54	1.16
2	1	32	25	2	9	9.88	61.0	1.0	220.0	121.0	0.55	0.96
2	1	33	15	2	8	7.91	62.0	4.0	230.0	123.0	0.53	0.92
2	1	34	14	2	7	10.53	57.0	0.0	211.0	108.0	0.51	1.05
2	1	35	1	2	6	6.80	57.0	-1.0	210.0	95.0	0.45	0.95
2	2	36	6	2	5	10.36	58.0	0.0	220.0	117.0	0.53	1.04
2	2	37	27	2	4	10.96	60.0	1.0	223.0	121.0	0.54	1.00
2	2	38	13	2	3	13.86	61.0	1.0	209.0	113.0	0.54	0.96
2	2	39	10	2	2	12.55	60.0	1.0	217.0	102.0	0.47	0.92
2	2	40	21	2	1	9.82	61.0	-2.0	207.0	92.0	0.44	1.05
2	3	41	18	3	1	12.29	61.0	1.0	217.0	102.0	0.47	1.17
2	3	42	22	3	2	9.26	62.0	0.0	218.0	124.0	0.57	1.06
2	3	43	11	3	3	13.22	60.0	-2.0	219.0	104.0	0.47	1.19
2	3	44	3	3	4	6.90	60.0	-1.0	219.0	108.0	0.49	0.83
2	3	45	8	3	5	9.48	60.0	1.0	225.0	123.0	0.55	1.00
2	4	46	12	3	6	12.37	60.0	0.0	200.0	103.0	0.52	1.04
2	4	47	20	3	7	8.72	59.0	-1.0	224.0	103.0	0.46	1.00
2	4	48	2	3	8	7.30	60.0	1.0	190.0	97.0	0.51	0.81
2	4	49	17	3	9	7.64	60.0	1.0	217.0	83.0	0.38	1.00
2	4	50	29	3	10	11.82	65.0	0.0	205.0	97.0	0.47	1.05
2	5	51	9	3	11	10.23	61.0	0.0	204.0	115.0	0.56	1.14
2	5	52	19	3	12	8.46	61.0	1.0	209.0	101.0	0.48	1.05
2	5	53	7	3	13	8.10	60.0	1.0	171.0	74.0	0.43	1.05
2	5	54	5	3	14	6.43	58.0	0.0	183.0	76.0	0.42	1.00
2	5	55	23	3	15	7.53	60.0	1.0	195.0	90.0	0.46	1.43
2	6	56	30	3	16	10.49	66.0	0.0	189.0	86.0	0.46	1.20
2	6	57	4	3	17	7.32	60.0	1.0	194.0	96.0	0.49	1.10
2	6	58	24	3	18	5.60	63.0	2.0	191.0	88.0	0.46	0.93
2	6	59	26	3	19	10.92	60.0	1.0	227.0	107.0	0.47	1.30
2	6	60	16	3	20	10.96	60.0	1.0	197.0	96.0	0.49	1.05


#Copy the above table and then type the following commands in the R Console:
TrialData=read.table(file="clipboard", header=T)

#Then do exploratory data analysis. 

pairs(TrialData[7:13], lower.panel=panel.smooth, upper.panel=panel.cor2, diag.panel=panel.hist) 
#this assumes that you have previously defined the functions panel.hist and panel.cor2 as presented earlier in this tutorial.
 
attach(TrialData) 		#make variables accessible 
boxplot(GYF ~ENTRY)  # create boxplot of GYF grouped by category ENTRY
boxplot(GYF ~ENTRY, notch=T)  # Same as above but with notch option set to TRUE. If the notches of two plots do not overlap this is ‘strong evidence’ that the two medians differ
boxplot(GYF ~REP)  # create boxplot of GYF grouped by category REP
a=aov(GYF ~ENTRY+BLK+REP+RANGE+LONGROW)  # define a model for the anova of GYF 
summary(a)   	# show summary of anova
pairwise.t.test(GYF, BLK)  # make t.tests of GYF grouped by category BLK

#The pairwise.t.test function is quite useful since it makes T-tests with all possible two groups and provides a matrix of P values. This allows identifying groups that are significantly different from each other. Unfortunately, it seems that R can only handle about less than 20 groups.

#Linear models let you find causal relationships of the dependent variables. A breeder, for example, would be interested in modeling grain yield in dependence of other trial parameters and secondary traits. Copy the table with grain yield data and then enter following command in R for modeling.
TrialData=read.table(file="clipboard", header=T, sep="\t")
attach(TrialData) 		#make variables accesible 
model1=lm(GYF ~REP+BLK+PLOT+ENTRY+AD+ASI+PH+EH+EPO+EPP)
summary(model1)    # show summary of the linear model

# the lm coefficients let you know how much is the dependence from the other variable, whereas the p values indicate if the relationships are significant or not.
#Construct a new variable called SELINDEX taking into account the coefficient of those variables which were significant in the linear model. In this case use GYF, EPP and ASI

TrialData$SELINDEX=GYF+EPP*3.5-ASI*0.4  #Define a new variable called SELINDEX based on a linear combination of other variables GYF, EPP and ASI. The coefficients given by the linear model were used as weighting factors for this selection index.
boxplot(GYF ~ENTRY)
# GYF
#From the above boxplot, decide which is the highest yielding entry? Is it entry 10,11,12, 13 or 26? Are the differences significant? We can plot the selection index to find an answer.
boxplot(SELINDEX ~ENTRY)
# SELINDEX
title(main="Selection Index")  # to put a title to the graph
#From the above boxplot it can be seen that entry 11 is significantly better than entry 10, 12, 13 and 26. This was not significant if only looking at GYF parameter (previous boxplot). This highlights the usefulness of including secondary traits into the analysis.

 

# Some interesting codes
#To add an extra column containing the rank to a ragged array indexed by more than one grouping factors:

rank.lists <-with( data1, tapply( yield, list( site=site, year=year ), rank ) )

#To add an additional column ``rank'' containing the rank of the ``yield'' of 
the different varieties in relation to the indices ``year'' and ``site'' to the data1 object.  I achieved to calculate the ranks but I do not manage to merge this result to the original dataframe.

model1 <- lme(math ~ grade, mathdata, random=~grade|studentid)

#Ejemplo en español
#Utilice help() para aprender mas de las funciones que va a utilizar para analizar sus datos. Para importar un archivo, lo mejor usar la función file.choose() para escoger cualquier archivo de texto en cualquier directorio.
data1 = read.table(file.choose(), header=T)
data1=read.table(file="clipboard", header=T, sep="\t")  #importar datos del clipboard
summary(data1) # Le da un resumen de las estadísticas básicas
attributes(data1) # Le da los "atributos"
attributes(data1)$names # Le da los nombres de las variables
summary(data1$long)  # tambien da el resumen de la variable "long" dentro del objecto data1
# para acceder una variable sin el commando attach ponga el nombre del objeto seguid del simbolo de pesos $
attach(data1) # Anexa los datos de manera que R sepa de la existencia de esas variables sin necesidad de referirnos al objeto.
plot(x,y) # graficar una variable contra otra


#Glosary of frequent R functions

? name #does the same as help(name)
a=aov(var1~var2)  	#defines a new object a with the results of the analysis of variance
attach(data1)	#to make the variables defined in data1 accessible to the console
attach(object) #makes the elements of object available to be called directly
attributes(data1)  #lists the atributes of data1: variable and row names
boxplot(data1)   #boxplot
coplot(var1 ~ var2 | var3)  
data1=edit(data.frame())    # opens a window to define data1 manually
fix(data1)    # allows to edit and fix an existing object

data1=read.table(file.choose(),header=T)  # select file with a popup window 
data1=read.table(file="clipboard")   #import data from clipboard. Works only if there are no empty cells
data1=read.table(file="clipboard", header=T, sep="\t")  #import data from clipboard. Works even if empty cells are present
data1=read.table(file="clipboard", header=T, sep="\t", na.strings="")  #import data from clipboard. Works even if empty cells are present. Fills empty cells with NA.
detach(data1) 	#to detach the variables defined in data1
help (name) #provides help about "name" 
hist(data1)    #simple histogram
ls() #shows the variables in the workspace 
names(table) #what are the variables inside the table
pairs(data1)  #splom plot
pairwise.t.test(var1, var2)  # make t.tests of var1 grouped by categories var2. The result is a pairwise comparison matrix.
plot(data1)    #simple scatter plots
rm(list = ls()) #removes all variables from the work space 
rm(variable)  #removes that variable or object 
round(cor(data1),2)	#correlation matrix rounded to 2 decimals
summary (lm(var1~0+var2*var3))  #summary of the results of a linear model with interactions
summary (lm(var1~var2))  	#summary of the results of a linear model
summary(a)  	#summary of the contents of object a
summary(aov(var1~var2))  	#summary of the analysis of variance
summary(data1)    	#lists a summary of the data
t.test(var1,var2)    	#T.test between two variables. Use the names of the variables inside the object. In my case they were named as var1, and var2. 
t.test(var1~var2)    	#T.test within data of variable1 grouped by categories specified in variable 2. Grouping factor of var2 must have exactly 2 levels.



