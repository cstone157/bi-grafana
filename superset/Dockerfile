FROM apache/superset

#RUN superset fab create-admin --username shoc --firstname SHOC --lastname H2O --email shoc@shoc.us --password JustKeepSwimming

COPY --chown=superset superset_config.py /app/
ENV SUPERSET_CONFIG_PATH /app/superset_config.py

#RUN superset db upgrade
#RUN superset init

#RUN superset import-datasources -p /opt/superset/databases.yaml
#RUN superset import-dashboards -p dashboard_t_oil.zip -u shoc