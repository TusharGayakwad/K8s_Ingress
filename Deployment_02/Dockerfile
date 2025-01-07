FROM tomcat:latest
RUN mkdir -p /usr/local/tomcat/webapps/ROOT && \
    echo "Welcome to Tomcat!" > /usr/local/tomcat/webapps/ROOT/index.html
EXPOSE 8080
CMD ["catalina.sh", "run"]
