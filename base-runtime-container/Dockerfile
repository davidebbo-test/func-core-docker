FROM core-and-node
WORKDIR /app
COPY FunctionsRuntime FunctionsRuntime
COPY init_container.sh /bin/
ENV AzureWebJobsScriptRoot=/home/site/wwwroot
RUN echo "root:Docker!" | chpasswd \
    && echo "cd /app" >> /etc/bash.bash.rc 
ENTRYPOINT ["/bin/init_container.sh"]
