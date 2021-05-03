FROM rocker/shiny:3.6.3

ARG JOBE_IP

COPY shiny-server.sh /usr/bin/shiny-server.sh

COPY Primer-izboljsav /srv/shiny-server/Primer-izboljsav
COPY Preprost-primer /srv/shiny-server/Preprost-primer

COPY libraries /home/libraries

RUN apt-get update && apt-get install -y \
    libv8-dev \
    build-essential \
    libcurl4-gnutls-dev \
    libxml2-dev \
    libssl-dev 
# libapparmor-dev \
# apparmor-utils

# 'RAppArmor'
# Download and install shiny server and R packages
RUN R -e "install.packages(c('devtools', 'unix'), dependencies = TRUE)" && \
    R -e "devtools::install_github('tadson10/learnr', dependencies = TRUE)" && \
    # cp -Rf /usr/local/lib/R/site-library/RAppArmor/profiles/debian/* /etc/apparmor.d/ && \
    # rm /etc/apparmor.d/rapparmor.d/r-user && \
    chmod +x /usr/bin/shiny-server.sh && \
    # chmod 755 -R /var/lib/shiny-server && \
    chown shiny:shiny -R /srv/shiny-server/ && \
    chmod 755 -R /srv/shiny-server/ && \
    sed -i "s/#JOBE_SERVER_IP#/$JOBE_IP/g" /srv/shiny-server/Primer-izboljsav/PrimerIzboljsav.Rmd

EXPOSE 3838

# Copy r-user RAppArmor profile
# COPY r-user /etc/apparmor.d/rapparmor.d/r-user


CMD ["/usr/bin/shiny-server.sh"]