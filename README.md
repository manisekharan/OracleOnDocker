# OracleOnDocker

1. Install Docker Desktop
2. Install Oracle XE on docker

Step 1: Download the 18c XE image

docker pull gvenzl/oracle-xe

Step 2: Check the image exist in your Docker env

docker images

Step 3: Run the image

docker run -d -p 1521:1521 -e ORACLE_PASSWORD=SysPassword1 -v oracle-volume:/opt/oracle/oradata gvenzl/oracle-xe

This command remaps the 1521 port to local 1521, changed/set the password and gives volume details to all any changes to the database and image to be persisted i.e. when you restart the image your previous work will be there

Step 4: Rename image [you can skip this step if you want. I just wanted a different name]

docker ps
docker rename <image_name> 18XE

Step 5: Connect to the Database as DBA/Admin schema using SQL Developer

Create a DB Connection
Auth Type : Default
Username : system
Password : SysPassword1
Connection Type : Basic
Host Name : localhost
Port : 1521
Service Name : XEPDB1

Step 6: Create a new Database
create user demo identified by demo quota unlimited on users;
grant connect, resource to demo;
create table test (col1 NUMBER, col2 VARCHAR2(10));
insert into test values (1, 'Brendan');
select * from test;
commit;

Step 7: Test the Docker image persists the data

Stop the docker image

docker stop 18XE
Check it is no-longer running

docker ps
Nothing will be displayed

Step 8: Start the 18XE Docker image and Check data was persisted

docker start 18XE
docker ps
You should see the docker image is running
