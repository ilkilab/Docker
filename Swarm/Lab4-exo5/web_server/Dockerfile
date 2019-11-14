FROM nginx:latest

# Make sure we are fully up to date
RUN apt-get update -q && \		
# Update our repo infos
    apt-get dist-upgrade -y && \		
# Upgrade packages 
    apt-get install openssl && \		
# Install openssl
    apt-get clean && \			
# Remove cached package files
    apt-get autoclean			
# Remove any packages that are no longer needed

EXPOSE 80		# Expose the container’s port 

# Setup any variables we need
ENV SRV_PATH /srv/www/html

# Get a variable defined for our password
ARG PASSWORD=test

# Remove default configuration
RUN rm /etc/nginx/conf.d/default.conf

# Copy the script to the container
COPY start_nginx.sh /usr/local/bin

# Change ownership of copied files, setup authentication file, and change rights
RUN mkdir -p $SRV_PATH && \
    chown nginx:nginx $SRV_PATH && \
    printf "user:$(openssl passwd -crypt $PASSWORD)\n" >> $SRV_PATH/.htpasswd && \
    chmod +x /usr/local/bin/start_nginx.sh

# Add our own configuration in
COPY nginx_main_site.conf /etc/nginx/conf.d/

# Execute the script
CMD [“/usr/local/bin/start_nginx.sh”]
