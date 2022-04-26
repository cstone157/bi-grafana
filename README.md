# grafana-docker

A docker grafana dashboard for prototype purpose (the default configuration is already connected to the postgres-docker, so that there is a preloaded set of data).

# NOTE: 
With Grafana the time field has to be in UTC format, to do this for example use -> TO_DATE('2021-01-01', 'YYYY-MM-DD') at time zone 'utc' as "time"	to convert the string '2021-01-01' into a UTC formatted date string.

# For Example:
SELECT
  TO_DATE(public.t_oil.year || '-01-01', 'YYYY-MM-DD') at time zone 'utc' as "time",
  production
FROM public.t_oil
WHERE
  country = 'USA'


The docker-compose will start 3 containers grafana(http://localhost:3000/login), pg_container(a postgres server), and pg_admin(a pgAdmin server, a browser based database admin tool)(http://localhost:3031)
Grafana username/password - shoc/JustKeepSwimming
pgAdmin username/password - shoc@shoc.us/JustKeepSwimming

To add the pg_container in pgAdmin, first login and then add a new server with the host: postgres, username: shoc, password: JustKeepSwimming
If unmodifed the pg_container will come loaded with the parametric dataset (incomplete), t_oil in the default (for the demo of a simple dashboard), and data_gov (several large data sets)
The parametric data is divided into the siso and meped schemas, and the data_gov are in the data_gov schema.  You will have to use manual edit sql to be able to display data.

For instance:
SELECT
  TO_DATE(data_gov.meteorite_landings.year || '-01-01', 'YYYY-MM-DD') at time zone 'utc' as "time",
  count(data_gov.meteorite_landings.year) as count
FROM data_gov.meteorite_landings
GROUP BY data_gov.meteorite_landings.year



To start to project
$ sudo docker-compose up -d
To watch the logs of a container as be loaded (incase there are dashboards or databases missing)
$ sudo docker-compose up -d && sudo docker logs <cotainer_name> -f

To stop the project
$ sudo docker stop grafana pg_admin pg_container 

Cleanup the containers/images
$ sudo docker rm grafana pg_admin pg_container
$ sudo docker image rm work_grafana_bi_project_postgres

Cleanup the database (if you leave files in place they will persist).  DON'T COMMIT THE DATA FOLDER.
$ sudo rm -rf postgres/data && mkdir postgres/data

