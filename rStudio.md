# setting up the R Studio server


CL script for updating sources
```R
echo "echo "deb http://cran.rstudio.com/bin/linux/ubuntu trusty/" >> /etc/apt/sources.list" | sudo bash
```


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
