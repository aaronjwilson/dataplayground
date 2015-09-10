# setting up the R Studio server
Setting up on an Unbuntu Linux Server.  Information will work well for an amazon ubuntu image.

This was tricky because the information is in differenct areas. Two measures need to be taken before installing R on a Linux server.  The first is to establish a source from where your packages will be updated and create a link to the source for updating within the sources list.  The second step is to create a keyserver.    

To establish where your package updates will come from one must add a location to the sources.list in the linux platform. this is done by going to the R cran mirrors site choosing a location and adding "/cran/bin/linux/ubuntu trusty/(or whatever your release version name is (precise, trusty, vivid).
CL script for updating sources
```R
$ echo "echo "deb http://cran.rstudio.com/bin/linux/ubuntu trusty/" >> /etc/apt/sources.list" | sudo bash
```

Create a keyserver for allowing access to ubuntu machine.  
```R
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9
```

#Finally:Installing R
```R
$ sudo apt-get install r-base-core r-base-dev
```
NOTE:this may take a bit of time. 


#Launch R and install packages
```R
install.packages(c('ggplot2','plyr', 'dplyr', 'stringr', 'reshape2', 'knitr', 'drc','Rlabkey'), dependencies=TRUE, repos='http://cran.rstudio.com/')
#should also include data.table with this bunch
```
Alternatively one can install packages from command line without launching R through CMD BATCH
Create the file ```installPackages.R``` add the above lines to the file, save and run the command
```$ R CMD BATCH installPackages.R```

If using Labkey interface it is best to install some curl and xml parsers with:
```
$ sudo apt-get install libcurl4-gnutls-dev libxml2-dev r-cran-xml
```
alway do release updates along the way to fill in the blanks.  

##RStudio Server
If you just want to use R from the terminal space then the above will suffice.  There is some X11 renderings for graphical output but I am still figuring that one out.  In the meantime I would highly recommend the genius IDE of R Studio (Server).  It is a very aesthetically pleasing interface for the R environment which comes with a server flavoring as well. And it is easy to setup.   

```
$ sudo apt-get install gdebi-core
$ sudo atp-get install libapparmor1 #NOTE: as of 2015 this step has been deprecated
$ cd /tmp
$ wget https://download2.rstudio.org/rstudio-server-0.99.484-amd64.deb
$ sudo gdebi rstudio-server-0.99.484-amd64.deb
```




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
