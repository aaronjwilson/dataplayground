# setting up the R Studio server
Setting up on an Unbuntu Linux Server.  Information will work well for an amazon ubuntu image.

This was tricky because the information is in differenct areas. Two measures need to be taken before installing R on a Linux server.  The first is to establish a source from where your packages will be updated and create a link to the source for updating within the sources list.  The second step is to create a keyserver.    

To establish where your package updates will come from one must add a location to the sources.list in the linux platform. this is done by going to the R cran mirrors site choosing a location and adding "/cran/bin/linux/ubuntu trusty/(or whatever your release version name is (precise, trusty, etc).
CL script for updating sources
```R
echo "echo "deb http://cran.rstudio.com/bin/linux/ubuntu trusty/" >> /etc/apt/sources.list" | sudo bash
```

Create a keyserver for allowing access to ubuntu machine.  
```R
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9
```

#Finally:Installing R
this may take a bit of time. 
```R
sudo apt-get install r-base-core r-base-dev
```

#Launch R and install packages
```R
install.packages(c('ggplot2','plyr', 'dplyr', 'stringr', 'reshape2', 'knitr', 'drc','Rlabkey','faraway'), dependencies=TRUE)
```
Alternatively one can install packages from command line without launching R through CMD BATCH
Create the file ```installPackages.R```.
add lines to file 
```R
install.packages(c('ggplot2','plyr', 'dplyr', 'stringr', 'reshape2', 'knitr', 'drc','Rlabkey','faraway'), dependencies=TRUE)
```
for installation run the command ```R CMD BATCH installPackages.R```

#Side projects for myself learning linux shell
bash script
```R
#!/bin/bash
#mirror

echo "deb http://cran.rstudio.com/bin/linux/ubuntu trusty/"  >> /etc/apt/sources.list
```

side project echo into a file on a server by ssh.
use variable:
X_X="echo something >> file"
ssh user@server $X_X
