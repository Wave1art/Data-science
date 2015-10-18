# Data-science

Notes for configuring AWS for use with various data science tools on Ubuntu trusty installation.


## Rstudio Server
Follow the installation steps on the RStudio Server website. In particular note the section regarding public keys to access the repositories. This must be done before the install or upgrade commandes will work.

Expose the Rstudio server ports by modifying the AWS security group that the instance sits on.

Create at least one valid user for RStudio server using the normal approach for <a href=http://linux.die.net/man/8/useradd> linux</a>. Note that it is important for the user to be given a home directory or logging in to RStudio server will fail. 
