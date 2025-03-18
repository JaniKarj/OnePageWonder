# Use an official Nginx image as a parent image
FROM nginx:latest

# Remove default Nginx configuration
RUN rm /etc/nginx/conf.d/default.conf

# Copy your Nginx configuration file
COPY nginx.conf /etc/nginx/conf.d/

# Copy static files from Django app
#COPY --from=django-web /app/staticfiles /var/www/static/

# Expose port 80
EXPOSE 80