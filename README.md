




                                                          The csvserver assignment

The developer team of the csvserver was working hard to get it ready for production. The team decided to go for a trip before the launch, and has been missing since then. You have been given the responsibility to figure out how to get the csvserver running correctly with the help of the following document. You might need to understand why things are failing and try to fix them, and make it ready for a launch1.

Prerequisites

You don't need to know Docker or Prometheus beforehand to solve this assignment, take a look at the following docs and understand the basics about these tools.

1.Read Docker orientation and setup: https://docs.docker.com/get-started/

2.Read Docker build and run your image: https://docs.docker.com/get-started/part2/

3.Read Get started with Docker Compose: https://docs.docker.com/compose/gettingstarted/

4.Read Prometheus getting started: https://prometheus.io/docs/prometheus/latest/getting_started/

5.Read Prometheus installation with Docker: https://prometheus.io/docs/prometheus/latest/installation/

6.Install Docker and docker-compose on your machine and run following commands,

                   docker pull infracloudio/csvserver:latest

                   docker pull prom/prometheus:v2.22.0

ANSWER : By running these commands in termninal docker images will be downloaded


Clone this repository to your machine. (Don't fork it).
Use bash shell for all the operations, other shells like ksh, fish etc might cause unknown issues.
Create a new private repository on GitHub.
cd into the solution directory, and perform all the steps from that directory.
NOTE: If you have a Windows machine, you can try to do this assignment on WSL-2 or use https://labs.play-with-docker.com or install GNU/Linux (i.e. Ubuntu) in a virtual machine.

Please note

Any step from the assignment does not require you to modify the container image, or build your own container image at all.

Make sure all the files you create have the exact same names as given.

Don't commit all of your work as a single commit, commit it as you finish each part, so we can see the work as you built it up.

The solution should work on a different machine, which has docker and docker-compose, without any modifications.

Reading this document carefully is the key to solve this assignment.

If you need more time or are stuck at some point, don't hesitate to reach out to us.



                                                               Part I


1.Run the container image infracloudio/csvserver:latest in background and check if it's running.




2.If it's failing then try to find the reason, once you find the reason, move to the next step.




SOLUTION : It wont run becuase of the missing file in container






3.Write a bash script gencsv.sh to generate a file named inputFile whose content looks like:

 0, 234

 1, 98

 2, 34


These are comma separated values with index and a random number.





SOLUTION : By using for condition and Random command in shell script we can get the above content.









.





Running the script without any arguments, should generate the file inputFile with 10 such entries in current directory.




SOLUTION : At the end of the shell-script give this [ > inputFille ] so it will generate inputFile and while running the shell script the content in the shell will be in the inputFile.








You should be able to extend this script to generate any number of entries, for example 100000 entries.





Run the script to generate the inputFile. Make sure that the generated file is readable by other users.





4.Run the container again in the background with file generated in (3) available inside the container (remember the reason you found in (2)).



SOLUTION : 
Run this command
docker run -d -v `pwd`/inputFile:/csvserver/inputdata [ image id ]

Hint : With this command the missingfile will be attached to the container




5.Get shell access to the container and find the port on which the application is listening. Once done, stop / delete the running container.



SOLUTION : Once the container is running we can find the container port 

its listening on : 9300




6.Same as (4), run the container and make sure,



The application is accessible on the host at http://localhost:9393



Set the environment variable CSVSERVER_BORDER to have value Orange.


The application should be accessible at http://localhost:9393, it should have the 10 entries from inputFile and the welcome note should have an orange color border.



 SOLUTION :
 run this command 

 docker run -d -v `pwd`/inputFile:/csvserver/inputdata -p http://localhost:9393:9300 --env CSVSERVER_BORDER=Orange [image id ]

 Hint : With this command the application will be accesible on the host and the csvsever_border  will be changes to orange.


NOTE: If you are using play-with-docker.com then you will see the number 9393 highlighted at the top. You can access the application by clicking on that instead of using http://localhost:9393



NOTE: On play-with-docker.com, you can create files in the terminal and edit them with their online editor.



Save the solution


Create a file called README.md in the solution directory with all the commands you executed as part of this section (Part I).


Write the docker run command you executed for (6) in a file named part-1-cmd.


Run one of the following commands which will generate a file with name part-1-output.


                      curl -o ./part-1-output http://localhost:9393/raw
											   #if above command fails use below one
                      wget -O ./part-1-output http://localhost:9393/raw

SOLUTION : By running this command part-1-output file will be downloaded.


Run the following command which will generate a file with name part-1-logs.


                       docker logs [container_id] >& part-1-logs

SOLUTION : with this command part-1-logs will be saved



Make sure that the files gencsv.sh, inputFile, part-1-cmd, part-1-output, part-1-logs are present in the solution directory.


Commit and push the changes to your repository on GitHub.



NOTE: One should be able to follow the instructions from the solution/README.md file and get csvserver running on their machine.


                                                        PART - 2





Delete any containers running from the last part.





1.Create a docker-compose.yaml file for the setup from part I.




One should be able to run the application with docker-compose up.


SOLUTION : create a yml file.

Hint : stop/delete the running container and use this command

             docker-compose up [ yml file ]




Save the solution


Copy the docker-compose.yaml to the solution directory.


Commit and push the changes to your repository on GitHub.




                                                                PART -3


Delete any containers running from the last part.



1.Add Prometheus container (prom/prometheus:v2.22.0) to the docker-compose.yaml form part II.

HINT : Add prometheus container to the docker-compose.yml



2.Configure Prometheus to collect data from our application at <application>:<port>/metrics endpoint. (Where the <port> is the port from I.5)
	
HINT: Pull the config of prometheus image and makes changes in scrape and fill the ip adress of the application and save the changes in prometheus.yml
	
	
	
3.Make sure that Prometheus is accessible at http://localhost:9090 on the host.
	
HINT: run this command
	
	  docker run -d -v `pwd`/prometheus.yml:/etc/prometheus/prometheus.yml -p 9090:9090 [ image id ]
	
	
4.Type csvserver_records in the query box of Prometheus. Click on Execute and then switch to the Graph tab.
	
	
HINT : Make changes in the time graph update it to 5 mins.
	
	
The Prometheus instance should be accessible at http://localhost:9090, and it should show a straight line graph with value 10 (consider shrinking the time range to 5m).
